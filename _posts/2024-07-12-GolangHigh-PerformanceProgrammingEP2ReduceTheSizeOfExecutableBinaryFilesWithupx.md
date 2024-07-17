---
title: "Golang 고성능 프로그래밍 EP2 UPX로 실행 파일 크기 줄이는 방법"
description: ""
coverImage: "/assets/img/2024-07-12-GolangHigh-PerformanceProgrammingEP2ReduceTheSizeOfExecutableBinaryFilesWithupx_0.png"
date: 2024-07-12 22:20
ogImage: 
  url: /assets/img/2024-07-12-GolangHigh-PerformanceProgrammingEP2ReduceTheSizeOfExecutableBinaryFilesWithupx_0.png
tag: Tech
originalTitle: "Golang High-Performance Programming EP2: Reduce The Size Of Executable Binary Files With upx"
link: "https://medium.com/faun/golang-high-performance-programming-ep2-reduces-compiled-volume-with-upx-62c0ef00d51b"
---


<img src="/assets/img/2024-07-12-GolangHigh-PerformanceProgrammingEP2ReduceTheSizeOfExecutableBinaryFilesWithupx_0.png" />

우리 모두가 알고 있는 것처럼 Golang은 컴파일 속도가 엄청나게 빠른 중요한 특징을 갖고 있어요. Go가 설계될 때 컴파일 속도는 주요 고려 사항이었답니다. 하지만 Go가 코드를 컴파일한 후 생성된 실행 가능한 이진 파일의 크기를 살펴 보신 적이 있나요? 간단한 HTTP 서버 예제를 살펴 보겠습니다:

```js
import (
    "fmt"
    "net/http"
)
func main() {
    // http 서버를 만들고 핸들러 hello를 만들어 hello world를 반환합니다
    http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
        fmt.Fprintf(w, "Hello, World\n")
    })
    // 포트 8080을 듣습니다
    err := http.ListenAndServe(":8080", nil)
    if err != nil {
        return
    }
}
```

컴파일된 크기는 6.5M입니다.

<div class="content-ad"></div>

```js
➜  binary-size git:(main) ✗ go build  -o server main.go                 
 ➜  binary-size git:(main) ✗ ls -lh server 
 -rwxr-xr-x  1 hxzhouh  staff   6.5M Jul  2 14:20 server
```

Go 언어 컴파일러는 바이너리 파일의 크기를 최적화합니다. 이에 관심이 있다면 다른 내 게시물도 확인해보세요:

이제 서버의 크기를 최적화해 보겠습니다.

# 디버그 정보 제거

<div class="content-ad"></div>

Go 컴파일러는 기본적으로 컴파일된 프로그램에 심볼 테이블과 디버그 정보를 포함시킵니다. 일반적으로 릴리스 버전을 위해 이 디버그 정보를 제거하여 이진 크기를 줄일 수 있습니다.

```js
➜  binary-size git:(main) ✗ go build -ldflags="-s -w" -o server main.go
➜  binary-size git:(main) ✗ ls -lh server                              
-rwxr-xr-x  1 hxzhouh  staff   4.5M Jul  2 14:30 server
```

- -s: 심볼 테이블과 디버그 정보를 생략합니다.
- -w: DWARFv3 디버그 정보를 생략합니다. 이 옵션을 사용하면 gdb를 사용하여 디버깅할 수 없습니다.

이로 인해 사이즈가 6.5M에서 4.5M로 줄어들어 전체적으로 30% 정도 감소했습니다. 이것은 좋은 첫 번째 단계입니다.

<div class="content-ad"></div>

# UPX 사용하기

Mac에서는 brew를 통해 UPX를 설치할 수 있어요.

```js
brew install upx
```

## UPX 만으로 압축하기

<div class="content-ad"></div>

UPX에는 1부터 13까지의 압축 비율이 있는 여러 매개변수가 있습니다. 1은 가장 낮은 압축 비율을 나타내고, 13은 가장 높은 압축을 나타냅니다. UPX만을 사용하여 이진 크기를 얼마나 줄일 수 있는지 알아봅시다.

```js
➜  binary-size git:(main) ✗ go build -o server main.go && upx --brute server && ls -lh server 
 -rwxr-xr-x  1 hxzhouh  staff   3.9M Jul  2 14:38 server
```

6.5M에서 3.9M으로 압축률은 약 60%입니다.

# UPX + 컴파일러 옵션

<div class="content-ad"></div>

UPX와 -ldflags="-s -w"를 함께 사용하도록 설정하였습니다:

```js
➜  binary-size git:(main) ✗ go build -ldflags="-s -w"  -o server main.go && upx --brute server && ls -lh server 
 -rwxr-xr-x  1 hxzhouh  staff   1.4M Jul  2 14:40 server
```

결과적으로 압축되고 UPX를 사용한 실행 파일의 크기는 6.5M에서 1.4M로 줄어 80%의 공간을 절약할 수 있었습니다. 대규모 애플리케이션의 경우 매우 상당한 양이죠.

# UPX 작동 방식

<div class="content-ad"></div>

압축된 프로그램은 원본처럼 작동하며 복원없이 정상적으로 실행될 수 있습니다. 이 압축 방법은 셸 압축이라고 불리며 두 부분으로 구성됩니다:

- 프로그램의 시작 부분이나 다른 적절한 위치에 복원 코드를 삽입합니다.
- 나머지 부분을 압축합니다.

실행될 때 또한 두 부분을 포함합니다:

- 처음에 삽입된 복원 코드가 먼저 실행되어 원래 프로그램을 메모리로 압축 해제합니다.
- 그런 다음 압축 해제된 프로그램이 실행됩니다. UPX는 프로그램이 실행될 때 추가 복원 단계가 도입되지만 이번 시간은 거의 무시할 만합니다.

<div class="content-ad"></div>

업무에 엄격한 요구사항이 없다면 UPX 압축을 사용하지 않는 선택지도 고려해 볼 수 있어요. 일반적으로 서버사이드 독립형 백그라운드 서비스는 압축된 크기가 필요하지 않아요. 하지만 라즈베리 파이와 같은 임베디드 장치의 경우, 한 번 시도해 볼 가치가 있어요.

# 결론

이 게시물에는 많은 흥미로운 답변이 포함되어 있어요:

- 예를 들어, C와 Go로 동일한 기능을 구현할 때 (작은 데모 실행), C로 생성된 실행 파일 크기는 Go로 생성된 것의 1/20입니다 (왜 그럴까요?).
- fmt 패키지를 포함하지 않고 println을 사용하면 실행 파일 크기를 더 줄일 수 있어요.

<div class="content-ad"></div>

<img src="/assets/img/2024-07-12-GolangHigh-PerformanceProgrammingEP2ReduceTheSizeOfExecutableBinaryFilesWithupx_1.png" />

## 👋 이 정보가 도움이 되셨다면, 아래 👏 버튼을 몇 번 클릭하여 저자를 지원해주세요 👇

## 🚀 FAUN 개발자 커뮤니티에 가입하고 매주 인박스에서 비슷한 이야기를 받아보세요