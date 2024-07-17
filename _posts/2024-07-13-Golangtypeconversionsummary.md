---
title: "Golang 타입 변환 요약 정리"
description: ""
coverImage: "/assets/img/2024-07-13-Golangtypeconversionsummary_0.png"
date: 2024-07-13 21:47
ogImage: 
  url: /assets/img/2024-07-13-Golangtypeconversionsummary_0.png
tag: Tech
originalTitle: "Golang type conversion summary"
link: "https://medium.com/gitconnected/golang-type-conversion-summary-dc9e36842d25"
---


고랭의 타입 변환에 대해 명확히 설명한 기사입니다.

![Golang type conversion summary](/assets/img/2024-07-13-Golangtypeconversionsummary_0.png)

고에는 4가지 타입 변환 유형이 있습니다:

- Assertion.
- Forced type conversion.
- Explicit type conversion.
- Implicit type conversion.

<div class="content-ad"></div>

일반적으로 말하는 유형 변환은 단언을 의미하며, 강제 유형 변환은 일상생활에서 사용되지 않습니다. 명시적 유형 변환은 기본 유형 변환이고, 암시 적으로 사용되지만 주의하지 않습니다.

단언, 강제 및 명시 유형의 세 가지 범주는 Go 구문 설명에서 모두 설명되고 암시적으로는 일상 사용 과정에서 요약됩니다.

단언 유형 변환: 변수가 특정 유형으로 변환될 수 있는지를 판단하여 단언합니다.

<div class="content-ad"></div>

참고: 형식 단언은 인터페이스에서만 발생할 수 있습니다.

먼저 간단한 단언 표현식을 살펴봅시다.

```js
var s = x.(T)
```

x가 nil이 아니고 x가 형식 T로 변환될 수 있다면, 단언이 성공하고 형식 T의 변수 s가 반환됩니다.

<div class="content-ad"></div>

T가 인터페이스 타입이 아닌 경우, x의 타입은 T여야 하며, T가 인터페이스인 경우 x는 T 인터페이스를 구현해야 합니다.

단언 타입이 참인 경우, 표현식의 반환 값은 T 타입의 x이며, 단언에 실패하면 패닉이 발생합니다.

단언에 실패하면 패닉이 발생합니다. Go는 다음과 같은 반환을 사용하는 다른 단언 구문을 제공합니다:

```js
s, ok := x.(T)
```

<div class="content-ad"></div>

이 방법은 첫 번째 방법과 거의 동일하지만 ok는 패닉 없이 어서션 성공 여부를 반환하며, ok는 성공 여부를 지시합니다.

타입 선언을 할 때는 변수의 기본 타입을 알아야 하지만 항상 그렇지는 않습니다.

그래서 타입 어서션 식은 사실 두 번째 선택값을 반환합니다.

두 번째 값으로 우리는 쉽게 어서션 구조가 성공적이었는지를 판별할 수 있습니다.

<div class="content-ad"></div>

```go
var foo interface{} = "123" 
fooStr, ok := foo.(string)
if ok {
    // ...
}
```

타입 스위치.

타입 스위치의 또 다른 단언 방법은 Go 구문에서 제공됩니다.

x가 type의 유형으로 단언되며, type의 구체적인 값은 스위치 케이스의 값입니다.

<div class="content-ad"></div>

만약 x가 성공적으로 case 타입으로 확인되면 해당 case를 실행할 수 있습니다. 이때 i := x.(type)에 의해 반환된 i는 해당 타입의 변수로, 바로 case 타입으로 사용할 수 있습니다.

타입 스위치 구문은 인터페이스의 타입을 확실히 알 수 없을 때 유용하게 사용할 수 있는 구조입니다.

구체적인 예시를 살펴보겠습니다:

강제 타입 변환.

<div class="content-ad"></div>

변수 유형 수정으로 강제 유형 변환

이 방법은 일반적이지 않습니다. 주로 안전하지 않은 패키지 및 인터페이스 유형 감지에 사용됩니다. Go 변수에 대한 지식이 필요합니다.

```js
var f float64
bits = *(*uint64)(unsafe.Pointer(&f))
type ptr unsafe.Pointer
bits = *(*uint64)(ptr(&f))
var p ptr = nil
```

float64가 uint64 유형으로 변환되어 float의 주소는 값이지만 유형은 float64입니다. 그런 다음 uint64 유형 변수가 생성되며 주소 값은 float64의 주소 값과 같습니다. 두 변수의 값은 같지만 유형이 다르며 최종적으로 유형이 강제로 변환됩니다.

<div class="content-ad"></div>

위험한 강제 변환은 포인터의 기본 작업입니다. C 언어를 사용하는 개발자들은 이러한 포인터 유형 변환이 잘 알려져 있습니다. 메모리 정렬을 사용하면 변환이 신뢰할 수 있습니다.

예를 들어, int와 uint 사이에 부호 비트 차이가 있고, 위험한 변환 후 값이 다를 수 있지만, 메모리에 저장된 이진값은 정확히 동일합니다.

인터페이스 유형 감지.

```js
var _ Context = (*ContextBase)(nil)
```

<div class="content-ad"></div>

nil의 유형은 nil이며 주소 값은 0이며 강제 변환을 통해 *ContextBase로 변환됩니다. 반환된 변수의 유형은 *ContextBase이며 주소 값은 0입니다.

그런 다음 Context=xx가 할당됩니다. xx가 Context 인터페이스를 구현하면 괜찮습니다. 구현되지 않은 경우 컴파일 시간에 오류가 보고되며 구현에서 컴파일 중에 인터페이스가 구현되었는지 확인됩니다.

명시적 유형 변환.

명시적으로 변환 가능한 표현식 T(x), 여기서 T는 유형이고 x는 유형 T로 변환 가능한 표현식입니다. 예: uint(123).

<div class="content-ad"></div>

변수 x를 T 유형으로 변환할 수 있는 경우는 다음과 같습니다:

- x가 T 유형에 할당 가능한 경우.
- x와 T가 같은 기본 유형을 갖고 있는 struct 레이블의 유형을 무시할 때.
- x의 struct 플래그 유형과 T가 미정의 유형의 포인터 유형인 경우, 그리고 포인터 기본 유형이 동일한 경우.
- x와 T의 유형이 모두 정수 또는 부동 소수점 유형인 경우.
- x와 T의 유형이 모두 복소수 유형인 경우.
- x의 유형이 정수 또는 []byte 또는 []rune인 경우, T는 문자열 유형인 경우.
- x의 유형이 문자열이고 T의 유형이 []byte 또는 []rune인 경우.

예를 들어, 다음 코드는 변환 규칙을 사용하며 규칙 구현은 reflect.Value.Convert 메서드 논리를 참조할 수 있습니다.

```js
int64(123)
[]byte("hello")
type A int
A(0)
```

<div class="content-ad"></div>

암시적 유형 변환.

일상적으로 암시적 유형 변환이 느껴지지는 않지만, 연산 중에 유형 변환이 발생하며, 이 중 두 가지가 아래에 나열되어 있습니다.

# 1. 조합 간 재단언 유형.

```js
type Reader interface {
    Read(p []byte) (n int, err error)
}
type ReadCloser interface {
    Reader
    Close() error
}
var rc ReaderClose
r := rc
```

<div class="content-ad"></div>

The ReaderClose interface combines the Reader interface, but type conversion occurs when r=rc is assigned, and Go uses the system's built-in function to perform the type conversion.

If you've encountered variable assignments involving interface composition types, and then used pprof and bench testing to discover this detail, you may have wasted some performance on interface type transfers.

## 2. Assignment between the same type.

```go
type Handler func()
func NewHandler() Handler {
    return func() {}
}
```

<div class="content-ad"></div>

```js
type Handler은 Handler 유형을 정의하지만, Handler와 func()은 두 가지 실제 유형입니다. 이들 유형은 동일하지 않으며, 반사와 단언을 사용하여 두 유형이 다르게 될 것입니다.

검사 코드:

package main
import (
    "fmt"
    "reflect"
)
type Handler func()
func a() Handler {
    return func() {}
}
func main() {
    var i interface{} = main
    _, ok := i.(func())
    fmt.Println(ok)
    _, ok = i.(Handler)
    fmt.Println(ok)
    fmt.Println(reflect.TypeOf(main) == reflect.TypeOf((*Handler)(nil)).Elem())
}

결과는:
```

<div class="content-ad"></div>

```js
true
false
false
```

이런 이야기를 좋아하시면서 제게 지원하고 싶으시다면, 박수를 보내주세요.

여러분의 지원은 저에게 매우 중요합니다. 감사합니다.