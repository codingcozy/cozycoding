---
title: "Go 1.23 개인적으로 추천하는 탑 기능들"
description: ""
coverImage: "/assets/img/2024-08-26-Go123PersonalTopFeatures_0.png"
date: 2024-08-26 19:27
ogImage: 
  url: /assets/img/2024-08-26-Go123PersonalTopFeatures_0.png
tag: Tech
originalTitle: "Go 1.23 Personal Top Features"
link: "https://medium.com/@dmytro.misik/go-1-23-personal-top-features-9eac82c5466b"
isUpdated: false
---


## Go 1.23 최고 업그레이드 탐험

![이미지](/assets/img/2024-08-26-Go123PersonalTopFeatures_0.png)

매번 새로운 Go 릴리스가 나올 때마다, 최신 기능을 살펴보고 제가 가장 좋아하는 기능을 강조합니다. 아래에서 제 이전 리뷰 링크를 찾을 수 있습니다:

8월 13일, Go 팀은 Go 1.23을 출시하여 새로운 개선 사항 및 업데이트를 함께 가져왔습니다. 이 게시물에서는 다시 한 번 제가 좋아하는 최고의 기능을 리뷰하고 싶습니다.

<div class="content-ad"></div>

## Range-over-func

이 업데이트는 언어의 중요한 향상을 소개합니다. 이제 어떻게 컬렉션을 반복하는지를 개선했습니다. 간단한 작업을 생각해봅시다: slice를 반복하는 것이죠. 기존에는 다음과 같이 for 루프를 사용했습니다:

```go
package main

func main() {
 s := []int{1, 2, 3, 4, 5}
 for i, x := range s {
  println(i, x)
 }
}
```

이 코드는 다음 출력을 생성합니다:

<div class="content-ad"></div>

```js
0 1
1 2
2 3
3 4
4 5
```

뒤로 반복하려면 다음과 같이 작성해야 합니다:

```js
s := []int{1, 2, 3, 4, 5}
for i := len(s) - 1; i >= 0; i-- {
  println(s[i])
}
```

새로운 "range-over-func" 언어 기능으로 range 키워드와 함께 사용할 수 있는 사용자 정의 재사용 가능한 함수를 구현할 수 있습니다.

<div class="content-ad"></div>

다음과 같은 방법으로 뒤로 반복할 수 있습니다:

```js
package main

func main() {
 s := []int{1, 2, 3, 4, 5}
 for i, x := range Backwards(s) {
  println(i, x)
 }
}

func Backwards[E any](s []E) func(func(int, E) bool) {
 return func(yield func(int, E) bool) {
  for i := len(s) - 1; i >= 0; i-- {
   if !yield(i, s[i]) {
    break
   }
  }
 }
}
```

Backwards 함수를 재사용할 수 있으며, 모든 유형의 슬라이스에 이점을 제공합니다. "range-over-func"의 주요 이점 중 하나는 뒤로 반복하는 데 사용되는 함수가 게으르게 실행된다는 것입니다. 즉, 메모리에 별도로 뒤집힌 슬라이스를 유지할 필요가 없습니다. Backwards는 반복을 시작할 때 호출되어 반환된 함수의 첫 번째 실행을 트리거합니다. 내부 함수가 yield를 호출할 때마다 (이 함수는 반복자로 지칭됩니다), 루프 본문이 실행됩니다 (이 경우 println(i, x)). yield가 false를 반환하거나 모든 항목을 반복할 때까지 반복자 함수가 계속 호출됩니다.

그러나 목표가 단순히 뒤로 반복하고 결과 슬라이스를 출력하는 것이라면, 이 수준의 복잡성은 필요하지 않을 수 있습니다.

<div class="content-ad"></div>

테이블 태그를 마크다운 형식으로 변경해주세요.

<div class="content-ad"></div>

재사용 가능한 정수형 변수를 위한 함수를 만들고 싶다면, 범위 루프에서 사용할 수 있는 Even 함수를 구현할 수 있습니다:

```js
func Even[E constraints.Integer](s []E) func(func(E) bool) {
 return func(f func(E) bool) {
  for _, x := range s {
   if x%2 == 0 {
    if !f(x) {
     break
    }
   }
  }
 }
}
```

이제 이렇게 필터링된 컬렉션을 순회할 수 있습니다:

```js
s := []int{1, 2, 3, 4, 5}
for x := range Even(s) {
  println(x)
}
```

<div class="content-ad"></div>

"range-over-func" 기능의 장점은 커뮤니티가 여러 도우미 함수를 만들 수 있도록 해주어 변환된, 필터링된 또는 기타 방식으로 처리된 컬렉션을 쉽게 반복할 수 있다는 것입니다.

다른 함수를 구현해 봅시다. – N개의 랜덤 숫자를 생성하는 함수를 만들겠습니다:

```js
func Random(min, max, count int) func(func(int) bool) {
 r := rand.New(rand.NewSource(uint64(time.Now().UnixNano())))

 return func(f func(int) bool) {
  for i := 0; i < count; i++ {
   num := r.Intn(max-min) + min
   if !f(num) {
    return
   }
  }
 }
}
```

이 함수는 지연 방식으로 [min, max) 범위 내에 count개의 랜덤 숫자를 생성합니다. 다음과 같이 사용할 수 있습니다:

<div class="content-ad"></div>

```js
for x := range Random(5, 15, 10) {
  println(x)
}
```

우리가 정의한 반복자들은 for-range 루프에서 즉시 사용하는 것뿐만 아니라 나중에도 사용할 수 있습니다. 예를 들어:

```js
package main

import (
 "fmt"
)

func main() {
 s := []int{1, 2, 3, 4, 5}
 even := Filter(s, func(v int) bool { return v%2 == 0 })
 PrintAll(even)
}

func PrintAll[V any](seq func(func(V) bool)) {
 for v := range seq {
  fmt.Println(v)
 }
}

func Filter[V any](s []V, f func(V) bool) func(func(V) bool) {
 return func(yield func(V) bool) {
  for _, v := range s {
   if f(v) {
    if !yield(v) {
     break
    }
   }
  }
 }
}
```

이 예시에서, 먼저 even이라는 반복자를 생성합니다. 이 반복자가 즉시 실행되지 않고, 이 시점에서 메모리에는 짝수 값이 저장되지 않는 것을 주목해주세요. 대신에, 반복자가 일반적인 PrintAll 함수로 전달되고, 그 반복자를 통해 값을 모두 반복하고 출력합니다.

<div class="content-ad"></div>

다양한 반복자를 조합할 수도 있어요:

```js
package main

import (
 "fmt"
)

func main() {
 s := []int{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}
 i := ToIter(s)
 even := Filter(i, func(v int) bool { return v%2 == 0 })
 big := Filter(even, func(v int) bool { return v > 5 })
 PrintAll(big)
}

func ToIter[V any](s []V) func(func(V) bool) {
 return func(yield func(V) bool) {
  for _, v := range s {
   if !yield(v) {
    break
   }
  }
 }
}

func Filter[V any](i func(func(V) bool), f func(V) bool) func(func(V) bool) {
 return func(yield func(V) bool) {
  for v := range i {
   if f(v) {
    if !yield(v) {
     break
    }
   }
  }
 }
}

func PrintAll[V any](seq func(func(V) bool)) {
 for v := range seq {
  fmt.Println(v)
 }
}
```

출력은 다음과 같을 거에요:

```js
6
8
10
```

<div class="content-ad"></div>

여기에 어떤 일이 발생했습니다: PrintAll 함수는 큰 이터레이터를 호출하고, 이는 순서대로 짝수 이터레이터를 호출하며, 마지막으로 ToIter 이터레이터의 결과를 호출합니다. 반복자의 재귀적 특성 때문에 복잡해 보일 수 있지만, 여기서의 핵심 이점은 지연 실행입니다. 언제나 하나의 요소만을 메모리에 저장하고 있어 고도로 효율적인 메모리 사용을 가능하게 합니다.

이 기능이 많은 새 라이브러리의 생성을 촉발할 것으로 예상하며, 이는 Go에서 슬라이스와 작업하는 방법을 더욱 개선할 것입니다.

# 새로운 독특한 패키지

문서에 따르면:

<div class="content-ad"></div>

다음 코드를 고려해 봅시다:

```js
package main

type Person struct {
 Name string
 Age  int
 // 그리고 많은 다른 필드들
}

func main() {
 p1 := Person{"John", 30}
 p2 := Person{"John", 30}

 SomeMethod(p1, p2)
}

func SomeMethod(p1, p2 Person) {
 // 복잡한 비즈니스 로직 (p1과 p2를 사용하지 않고)
 // ...
 // 이제 p1과 p2를 비교하여 결정을 내려야 합니다
 if p1 == p2 {
  // 어떤 복잡한 결정
 } else {
  // 다른 어려운 결정
 }
}
```

이 예제에서 SomeMethod에 Person의 두 인스턴스를 전달하는 것은 메모리를 많이 사용할 수 있습니다. p1과 p2를 복사해야 하기 때문입니다. 이 두 Person 인스턴스를 비교하는 것만 필요하다면, unique 패키지를 사용하여 효율적으로 처리할 수 있습니다:

```js
package main

import "unique"

type Person struct {
 Name string
 Age  int
 // 그리고 많은 다른 필드들
}

func main() {
 p1 := Person{"John", 30}
 p2 := Person{"John", 30}

 h1 := unique.Make(p1)
 h2 := unique.Make(p2)

 SomeMethod(h1, h2)
}

func SomeMethod(p1, p2 unique.Handle[Person]) {
 // 복잡한 비즈니스 로직 (p1과 p2를 사용하지 않고)
 // ...
 // 이제 p1과 p2를 비교하여 결정을 내려야 합니다
 if p1 == p2 {
  // 어떤 복잡한 결정
 } else {
  // 다른 어려운 결정
 }
}
```

<div class="content-ad"></div>

이 수정된 버전에서는 전체 Person 구조체를 전달하는 대신 unique.Make 함수를 사용하여 생성 된 Handle 유형을 전달합니다. Handle[T] 유형은 내부적으로 유형 T의 원래 값에 대한 참조를 저장합니다. 이 접근 방식은 몇 가지 주요 이점을 제공합니다:

- 효율성: Handle이 가벼워서 대형 구조체를 복사하는 오버헤드를 줄여 비교를 더 효율적으로 만듭니다;
- 간소화된 포인터 관리: Handle이 포인터 작업을 단순화하는 것처럼 보일 수 있지만, 주요 목표는 비교 가능한 유형을 최적화하는 것입니다;
- 원래 값에 액세스: SomeMethod 내에서 원래 값에 여전히 액세스해야하는 경우 Value() 메서드를 호출하여 손쉽게 검색할 수 있습니다:

```js
fmt.Printf("%v\n", p1.Value())
```

전반적으로 unique 패키지는 복잡한 유형을 관리하고 비교하는 간소화된 방법을 제공하여 코드를보다 효율적으로 유지 관리 가능하게합니다. Handle 유형을 사용하면 성능 향상과 비교 처리를 처리하는 더 깔끔하고 추상적인 접근이 가능합니다.

<div class="content-ad"></div>

# slices/maps/iter 패키지 개선 사항

Go 1.23에서는 새로운 iter 패키지가 소개되어 두 가지 주요 유형을 제공합니다:

```js
type (
 Seq[V any]     func(yield func(V) bool)
 Seq2[K, V any] func(yield func(K, V) bool)
)
```

이러한 유형은 반복자 유형을 정의하는 단축형입니다. `func(func(V) bool)`를 반복적으로 정의하는 대신에 `iter.Seq[V]`로 간소화할 수 있습니다. 마찬가지로 `func(func(K, V) bool)`도 `iter.Seq2[K, V]`로 줄일 수 있습니다.

<div class="content-ad"></div>

이터 패키지는 Pull과 Pull2라는 두 가지 유용한 메소드를 소개합니다. 이러한 메소드는 "push-style" 이터레이터 시퀀스를 "pull-style" 이터레이터로 변환합니다. 다음은 이를 사용하는 방법입니다.

```js
next, stop := iter.Pull(iter)
defer stop()

for {
  v, ok := next()
  if !ok {
    break
  }

  fmt.Println(v)
}
```

이 블로그에서 이전에 구현한 Backwards 이터레이터를 기억하나요? 새로운 slices 패키지에는 여러 유용한 이터레이터 중 하나인 Backward가 포함되어 있습니다. slices 패키지에서 제공하는 몇 가지 유용한 메소드를 살펴보겠습니다.

```js
package main

import (
 "fmt"
 "slices"
)

func main() {
 s := []int{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}

 i1 := slices.Values(s) // 슬라이스를 값의 이터레이터로 변환
 for v := range i1 {
  println(v) // 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 출력
 }

 i2 := slices.All(s) // 슬라이스를 인덱스 및 값의 이터레이터로 변환
 for i, v := range i2 {
  println(i, v) // 0 1, 1 2, 2 3, 3 4, 4 5, 5 6, 6 7, 7 8, 8 9, 9 10 출력
 }

 b := slices.Backward(s) // 슬라이스를 역순 인덱스 및 값의 이터레이터로 변환
 for i, v := range b {
  println(i, v) // 9 10, 8 9, 7 8, 6 7, 5 6, 4 5, 3 4, 2 3, 1 2, 0 1 출력
 }

 s = slices.Collect(i1) // 이터레이터에서 값들을 슬라이스로 수집
 fmt.Printf("%v\n", s)  // [1 2 3 4 5 6 7 8 9 10] 출력
}
```

<div class="content-ad"></div>

또한, maps 패키지에는 맵을 이터레이터로 변환하는 방법을 확장한 메서드가 포함되어 있습니다:

```js
package main

import "maps"

func main() {
 m := map[string]int{"a": 1, "b": 2, "c": 3}

 ik1 := maps.Keys(m) // 키에 대한 이터레이터
 for k := range ik1 {
  println(k) // a, b, c 출력
 }

 iv1 := maps.Values(m) // 값에 대한 이터레이터
 for v := range iv1 {
  println(v) // 1, 2, 3 출력
 }

 i2 := maps.All(m) // 키-값 쌍에 대한 이터레이터
 for k, v := range i2 {
  println(k, v) // a 1, b 2, c 3 출력
 }
}
```

# 결론

Go 1.23은 이터레이터 기능을 발전시키고 향상시키는 중요한 단계를 나타냅니다. 이전에 언급했듯이, Go 커뮤니티는 이 이터레이터 개선을 활용하는 많은 새로운 패키지를 선보일 것으로 예상됩니다. 나는 Go가 계속 발전하는 모습에 특히 기쁘게 생각하며, 이러한 향상 사항들은 개발자들이 더 많은 메모리를 절약하면서 코드를 작성할 수 있게 합니다.

<div class="content-ad"></div>

반복자 개선 외에도 Go 팀은 디버깅을 지원하고 소중한 지표 및 통계를 수집하는 데 도움이 되는 텔레메트리와 같은 새로운 도구를 소개했습니다. 모든 새로운 기능 및 개선 사항에 대한 포괄적인 개요를 보려면 이 링크를 참고하십시오:

# 자료들