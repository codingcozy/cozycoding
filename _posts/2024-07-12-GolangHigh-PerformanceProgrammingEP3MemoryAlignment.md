---
title: "고성능 Golang 프로그래밍 EP3 메모리 정렬 이해하기"
description: ""
coverImage: "/assets/img/2024-07-12-GolangHigh-PerformanceProgrammingEP3MemoryAlignment_0.png"
date: 2024-07-12 21:58
ogImage: 
  url: /assets/img/2024-07-12-GolangHigh-PerformanceProgrammingEP3MemoryAlignment_0.png
tag: Tech
originalTitle: "Golang High-Performance Programming EP3: Memory Alignment"
link: "https://medium.com/gitconnected/golang-high-performance-programming-ep3-memory-alignment-f3a947e6d9b0"
---



<img src="/assets/img/2024-07-12-GolangHigh-PerformanceProgrammingEP3MemoryAlignment_0.png" />

이것은 Go에서 고성능 프로그래밍에 관한 세 번째 글입니다. 왜 메모리 정렬이 필요한지, Go 메모리 정렬 규칙 및 메모리 정렬 사용 사례에 대해 분석합니다. 마지막으로, 개발하는 동안 메모리 정렬 문제를 식별하는 데 도움이 되는 두 가지 도구를 공유합니다.

# 메모리 정렬이란?

프로그래머에게 메모리는 그냥 큰 배열일 수 있습니다. 우리는 두 바이트를 차지하는 int16이나 네 바이트를 차지하는 int32를 쓸 수 있습니다. 예를들어:


<div class="content-ad"></div>

```go
type T1 struct {  
    a int8  
    b int64  
    c int16  
}
```

Go에 익숙하지 않은 사람들은 이 구조가 다음과 같이 배치되어 있다고 생각할 수 있습니다. 11 바이트 공간을 차지합니다.

그림 1: 일부 사람들이 이해하는 메모리 레이아웃

![Memory layout as understood by some people](/assets/img/2024-07-12-GolangHigh-PerformanceProgrammingEP3MemoryAlignment_1.png)


<div class="content-ad"></div>

하나 뒤에 하나, 매우 조밀하고 완벽합니다. 그러나 실제로는 이렇게 되어 있지는 않습니다. T1 변수들의 주소를 출력하면 다음과 같이 보이며, 총 24바이트의 공간을 차지합니다.

그림 2: T1의 실제 메모리 레이아웃

![T1 memory layout](/assets/img/2024-07-12-GolangHigh-PerformanceProgrammingEP3MemoryAlignment_2.png)

표 1: T1 크기

<div class="content-ad"></div>

```go
func main() {
    t := T1{}
    fmt.Println(fmt.Sprintf("%d %d %d %d", unsafe.Sizeof(t.a), unsafe.Sizeof(t.b), unsafe.Sizeof(t.c), unsafe.Sizeof(t)))
    fmt.Println(fmt.Sprintf("%p %p %p", &t.a, &t.b, &t.c))
    fmt.Println(unsafe.Alignof(t))
}
// output
// 1 8 2 24
// 0x14000114018 0x14000114020 0x14000114028
// 8
```

CPU는 워드 크기에 따라 메모리에서 데이터를 가져옵니다. 예를 들어, 64비트 CPU는 워드 크기가 8바이트이므로 CPU는 8바이트 단위로 메모리에 액세스하며, 이를 메모리 액세스 지정도로 참조합니다.

이러한 현상으로 인해 여러 가지 심각한 문제가 발생할 수 있습니다:

- 추가 CPU 명령으로 인한 성능 저하.
- 원래 원자적인 변수 읽기를 위한 연산이 더 이상 원자적이지 않게 되는 문제.
- 다른 예기치 않은 상황들.

<div class="content-ad"></div>

따라서 컴파일러들은 일반적으로 메모리 정렬을 구현하여 플랫폼 호환성과 성능을 보장하기 위해 메모리 공간을 희생합니다:

- 플랫폼 호환성: 모든 하드웨어 플랫폼이 임의의 주소에서 임의의 데이터에 액세스할 수 있는 것은 아닙니다. 특정 하드웨어 플랫폼은 특정 주소에서 특정 유형의 데이터만 가져올 수 있도록 하며, 그렇지 않으면 예외가 발생할 수 있습니다.
- 성능: 정렬되지 않은 메모리에 액세스하는 것은 CPU가 두 개의 메모리 액세스를 수행하도록 하고 정렬 및 연산 처리에 추가 클록 사이클을 소비하게 합니다. 정렬된 메모리는 단일 작업에서 액세스할 수 있어 효율성이 향상되며, 전형적인 시간 대 공간의 트레이드오프 입니다.

# Go에서의 메모리 정렬

Go 사양은 Go의 정렬 규칙을 명시합니다.

<div class="content-ad"></div>

```js
타입                              바이트 단위 크기
 
 byte, uint8, int8                    1
 uint16, int16                        2
 uint32, int32, float32               4
 uint64, int64, float64, complex64    8
 complex128                          16
```

- 임의의 타입 변수 x의 경우: unsafe.Alignof(x)는 최소 1입니다.
- 구조체 타입 변수 x의 경우: unsafe.Alignof(x)는 x의 각 필드 f에 대한 모든 값 unsafe.Alignof(x.f) 중 가장 큰 값이지만 최소 1입니다.
- 배열 타입 변수 x의 경우: unsafe.Alignof(x)는 배열 요소 타입의 정렬과 같습니다.

대부분의 경우, Go 컴파일러가 메모리를 자동으로 정렬해주기 때문에 걱정할 필요가 없습니다. 그러나 특정한 경우에 수동 정렬이 필요합니다.

예를 들어, x86 플랫폼에서 64비트 포인터 원자 연산의 경우 정렬이 필수입니다. 왜냐하면 32비트 플랫폼에서 64비트 원자 연산은 8바이트 정렬이 필요하므로 그렇지 않으면 프로그램이 패닉합니다. 아래 코드를 살펴보세요:

<div class="content-ad"></div>

```go
package main

import "sync/atomic"

type T3 struct {
    b int64
    c int32
    d int64
}

func main() {
    a := T3{}
    atomic.AddInt64(&a.d, 1)
}
```

AMD64 아키텍처에서 실행하면 오류가 발생하지 않지만 i386 아키텍처에서는 패닉이 발생할 것입니다.

Figure 3: T3 패닉

![이미지](/assets/img/2024-07-12-GolangHigh-PerformanceProgrammingEP3MemoryAlignment_3.png)


<div class="content-ad"></div>

이유는 32비트 플랫폼에서 T3가 4바이트로 정렬되고 64비트 플랫폼에서는 8바이트로 정렬되기 때문입니다. 64비트 플랫폼에서 그 메모리 레이아웃은 다음과 같습니다:

Figure 4: amd64에서 T3 메모리 레이아웃

![이미지](/assets/img/2024-07-12-GolangHigh-PerformanceProgrammingEP3MemoryAlignment_4.png)

하지만 i386 레이아웃에서는:

<div class="content-ad"></div>

Figure 5: i386에서의 T3 메모리 레이아웃

![T3 메모리 레이아웃](/assets/img/2024-07-12-GolangHigh-PerformanceProgrammingEP3MemoryAlignment_5.png)

이 문제는 atomic 패키지에 문서화되어 있습니다.

이를 해결하기 위해서는 T3를 "보기"에 8바이트로 정렬하도록 수동으로 패딩해야 합니다:

<div class="content-ad"></div>

```go
type T3 struct {
     b int64
     c int32
     _ int32
     d int64
 }
```

Go 소스 코드 및 오픈 소스 라이브러리에서 비슷한 작업을 볼 수 있습니다.
예를 들면:

- mgc
- groupcache

# 실용 공학

<div class="content-ad"></div>

## 필드 정렬

fieldalignment은 코드에서 잠재적인 메모리 정렬 최적화를 식별하고 자동으로 정렬해주는 공식 Go 도구입니다. 예를 들어, T1을 메모리에 맞게 자동으로 정렬합니다.

```js
➜  go_mem_alignment git:(main) ✗ fieldalignment -fix .          
 /Users/hxzhouh/workspace/github/blog-example/go/go_mem_alignment/main.go:8:8: struct of size 24 could be 16
 
 // 변경
 type T1 struct {  
     b int64  
     c int16  
     a int8  
 }
```

golangci-lint에서도 사용할 수 있습니다. fieldalignment은 go vet의 하위 기능으로, .golangci.yaml에서 다음과 같이 활성화됩니다:

<div class="content-ad"></div>

```yaml
# .golangci.yml
linters:
  disable-all: true
  enable:
    - govet
  fast: false

linters-settings:
  govet:
    # shadowed variables에 대한 보고
    check-shadowing: false
    fast: false
    enable:
      - fieldalignment
```

그러나 fieldalignment에는 짜증나는 단점이 있습니다. 구조체 멤버를 재배열할 때 모든 공백 라인과 주석을 제거합니다. 따라서 이 도구를 사용하기 전에 한 번 git commit하고, 도구를 사용한 후 git diff를 통해 변경 사항을 검토하고 필요한 후속 처리를 수행해야 합니다. 그래서 저는 대부분의 경우 structlayout을 선호하여 제작 환경에서 이 도구를 거의 사용하지 않습니다.

# structlayout

structlayout은 구조체의 레이아웃과 크기를 표시하며 데이터를 SVG 또는 json 형식으로 출력할 수 있습니다. 구조체가 복잡한 경우 이 도구를 사용하여 최적화할 수 있습니다.


<div class="content-ad"></div>

# structlayout을 사용하여 Go 구조체 레이아웃 시각화 및 최적화하기

structlayout은 구조체의 레이아웃과 크기를 SVG 또는 JSON 형식으로 출력하여 표시할 수 있습니다. 또한 이 도구를 사용하여 복잡한 구조체를 최적화할 수도 있습니다.

# 설치

```js
go install honnef.co/go/tools/cmd/structlayout@latest 
go install honnef.co/go/tools/cmd/structlayout-pretty@latest
go install honnef.co/go/tools/cmd/structlayout-optimize@latest
go install github.com/ajstarks/svgo/structlayout-svg@latest
```

<div class="content-ad"></div>

# structlayout을 사용하여 T1을 분석해보세요

```js
structlayout -json ./main.go T1 | structlayout-svg > T1.svg
```

그림 6: T1 구조 레이아웃

<img src="/assets/img/2024-07-12-GolangHigh-PerformanceProgrammingEP3MemoryAlignment_6.png" />

<div class="content-ad"></div>

두 개의 패딩 영역이 분명히 보입니다: 7 사이즈와 6 사이즈.
최적화된 T2

```js
type T2 struct {  
     a int8
     c int16
     b int64  
 }
```

표 7: T2 구조 레이아웃

![T2 Structure Layout](/assets/img/2024-07-12-GolangHigh-PerformanceProgrammingEP3MemoryAlignment_7.png)

<div class="content-ad"></div>

아직 두 개의 패딩 영역이 있지만 다섯 가지 크기만 존재합니다.

# 요약

메모리 정렬은 프로그램 성능과 호환성을 향상시키기 위해 설계된 프로그래밍의 중요한 기술입니다. 본 문서는 Go를 예시로 들어 메모리 정렬의 기본 개념과 필요성을 자세히 설명하며, 코드 예제를 통해 메모리 내 다양한 구조체의 실제 레이아웃을 시연합니다.

Go에서의 메모리 정렬 규칙은 주로 구조체 필드의 순서에 반영됩니다. 컴파일러는 자동으로 정렬을 통해 성능과 플랫폼 이식성을 보장하지만, 개발자는 때로는 성능 문제와 잠재적 오류를 피하기 위해 수동으로 구조체 필드를 조정해야 할 수 있습니다.

<div class="content-ad"></div>

빈 구조체는 메모리 정렬 최적화에 도움이 되는 도구입니다. 구체적인 작업에 대해서는 제 다른 기사를 참고하십시오:

개발자가 메모리 정렬 문제를 감지하고 최적화하는 데 도움을 주는 이 기사에서는 두 가지 실용적인 도구를 소개합니다:

- fieldalignment: 자동으로 구조체 메모리 정렬을 최적화할 수 있는 공식 Go 도구입니다.
- structlayout: 구조체의 메모리 레이아웃을 표시하여 개발자가 메모리 사용을 더 직관적으로 이해하고 최적화할 수 있도록 도와줍니다.

이러한 도구를 효과적으로 활용하면 개발자들은 메모리 낭비를 줄이고 개발 효율성을 높이며 프로그램의 성능과 안정성을 보장할 수 있습니다.

<div class="content-ad"></div>

# 참고 자료

- IBM DeveloperWorks: 데이터 정렬
- Go 규격: 크기 및 정렬 보증