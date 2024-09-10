---
title: "ES12의 5가지 혁신적인 JavaScript 기능"
description: ""
coverImage: "/assets/img/2024-09-10-The5mosttransformativeJavaScriptfeaturesfromES12_0.png"
date: 2024-09-10 18:26
ogImage: 
  url: /assets/img/2024-09-10-The5mosttransformativeJavaScriptfeaturesfromES12_0.png
tag: Tech
originalTitle: "The 5 most transformative JavaScript features from ES12"
link: "https://medium.com/coding-beauty/best-es12-js-features-0d45be551fac"
isUpdated: false
---



![ES12](/assets/img/2024-09-10-The5mosttransformativeJavaScriptfeaturesfromES12_0.png)

ES12은 정말 놀라운 업그레이드였어요.

우리가 JavaScript를 작성하는 방식을 완전히 변화시킬 가치 있는 기능으로 가득 찼어요.

코드가 더 깔끔하고 짧아지며, 더 쉽게 작성할 수 있게 되었어요.


<div class="content-ad"></div>

한번 확인해 보고 놓친 부분을 확인해 봐요.

# 1. Promise.any()

ES12 이전에 이미 Promise.all() 및 Promise.allSettled()이 있는데, 이들은 Promise 그룹을 기다리기 위한 것입니다.

여러 개의 Promise가 있지만 첫 번째로 해결된 Promise에만 관심이 있는 경우가 종종 있습니다.

<div class="content-ad"></div>

아래에서 표 태그를 Markdown 형식으로 변경해주세요:


| Function | Description             |
|----------|-------------------------|
| any()    | Resolves with the value that first resolves or rejects with the array of rejection reasons |


<div class="content-ad"></div>

# 2. replaceAll()

네, 이미 문자열 내에서 하위 문자열을 빠르게 교체하기 위해 replace()를 사용했었습니다.

![image](/assets/img/2024-09-10-The5mosttransformativeJavaScriptfeaturesfromES12_2.png)

하지만 이것은 정규식을 사용하지 않는 한 하위 문자열의 첫 번째 발생에 대해서만 그렇게 했습니다.

<div class="content-ad"></div>

ES12가 출시되어서 이제 replaceAll() 메서드를 사용해서 문자열의 각각의 인스턴스를 모두 바꿀 수 있게 되었어요.

![이미지](/assets/img/2024-09-10-The5mosttransformativeJavaScriptfeaturesfromES12_3.png)

# 3. WeakRefs

ES12부터 JavaScript 변수는 강한 참조 또는 약한 참조가 될 수 있어요.

<div class="content-ad"></div>

"이것들은 무엇인가요?

강력한 참조(Strong Refs)는 우리 일상적인 변수들입니다. 그러나 약한 참조(Weak Refs)는 명시적으로 WeakRef()로 생성해야 합니다:

![image](/assets/img/2024-09-10-The5mosttransformativeJavaScriptfeaturesfromES12_4.png)

JS 변수들은 메모리에 있는 실제 객체를 가리키는 참조일 뿐입니다."

<div class="content-ad"></div>

그래서 같은 객체에 대한 여러 개의 참조를 가질 수 있는 것입니다.

![image](/assets/img/2024-09-10-The5mosttransformativeJavaScriptfeaturesfromES12_5.png)

하지만 강한 참조와 약한 참조의 차이는 무엇일까요?

프로그래밍에서는 필요하지 않은 객체가 메모리에서 제거되어 자원을 절약하는 쓰레기 수집 기능이 있습니다.

<div class="content-ad"></div>

JavaScript에서는 객체들이 자동으로 가비지 수집 대상으로 표시됩니다. 이는 그 객체를 가리키는 모든 강한 참조 변수가 사용 불가능해지면(스코프 밖으로 벗어남) 발생합니다:

object person과 me은 func()가 실행되면 파괴 대기열에 넣습니다.

![이미지](/assets/img/2024-09-10-The5mosttransformativeJavaScriptfeaturesfromES12_6.png)

하지만 여기서 무슨 일이 일어나는지 살펴보세요:

<div class="content-ad"></div>

func()이 끝난 후에도 짝수 위치에 있는 사람이 범위를 벗어나더라도, 나라는 전역 강력한 참조가 남아 있었습니다.

![image](/assets/img/2024-09-10-The5mosttransformativeJavaScriptfeaturesfromES12_7.png)

하지만 나가 약한 참조였다면 어떨까요?

이제 func()이 실행된 후에는 사람이 객체에 대한 유일한 강력한 참조가 될 것입니다.

<div class="content-ad"></div>

그래서 해당 객체는 가비지 수집 대상으로 표시됩니다:

![image](/assets/img/2024-09-10-The5mosttransformativeJavaScriptfeaturesfromES12_8.png)

그렇다면 왜 약한 참조가 필요할까요?

그 중요한 사용 사례는 캐싱입니다.

<div class="content-ad"></div>

여기에서 어떤 일이 일어날까요: processData()가 실행되고 새로운 객체가 캐시에 저장됩니다.

데이터가 사용할 수 없게 되었지만, 객체는 캐시에서 강력한 참조를 가지고 있기 때문에 가비지 수집되지 않습니다.

하지만 processData()가 종료된 후에 객체를 해제하고 싶다면 어떻게 해야 할까요?

<div class="content-ad"></div>

저는 WeakRef를 Map 값으로 사용할 겁니다:

![링크](/assets/img/2024-09-10-The5mosttransformativeJavaScriptfeaturesfromES12_10.png)

## 4. 논리 할당 연산자

ES12의 귀여운 구문 설탕 기능입니다.

<div class="content-ad"></div>

아래와 같이 사용합니다:


![Image 1](/assets/img/2024-09-10-The5mosttransformativeJavaScriptfeaturesfromES12_11.png)


정확히 다음과 같이도 사용할 수 있습니다:


![Image 2](/assets/img/2024-09-10-The5mosttransformativeJavaScriptfeaturesfromES12_12.png)


<div class="content-ad"></div>

**??=**: 변수가 null 또는 정의되지 않은 경우에 빠르게 값을 할당합니다 ("nullish").

![ES12/13에서 가장 혁신적인 JavaScript 기능](/assets/img/2024-09-10-The5mosttransformativeJavaScriptfeaturesfromES12_13.png)

**||=**: ??=와 유사하지만 0, undefined, null, ``, NaN 또는 false와 같은 거짓 값을 위해 값을 할당합니다.

![ES12/14에서 가장 혁신적인 JavaScript 기능](/assets/img/2024-09-10-The5mosttransformativeJavaScriptfeaturesfromES12_14.png)

<div class="content-ad"></div>

표 태그를 Markdown 형식으로 변경해주세요.

![image](/assets/img/2024-09-10-The5mosttransformativeJavaScriptfeaturesfromES12_15.png)

# 5. Numeric separators

큰 숫자 리터럴을 더 읽기 쉽고 사용자 친화적으로 만들어 주는 작지만 영향력 있는 새로운 추가 기능입니다.

<div class="content-ad"></div>

<img src="/assets/img/2024-09-10-The5mosttransformativeJavaScriptfeaturesfromES12_16.png" />

컴파일러는 저 뭐그렇게 귀찮은 밑줄들을 완전히 무시해 — 모두 인간을 위한 것이야!

# 최종 결론

이것들이 ES12에 도입된 새로운 멋진 JavaScript 특징들이야.

<div class="content-ad"></div>

개발자로서 생산성을 높이고 보다 간결하고 표현력이 풍부하며 명확한 코드를 작성하는 데 도움이 되는 다음과 같은 요소들을 활용해보세요.

# 자바스크립트가 하는 모든 미친 짓

이미 알고 있다고 생각했던 모든 특징들을 알고나니.
아픈 버그를 피하고 가치 있는 시간을 절약할 수 있는 방법으로, 자바스크립트가 하는 모든 미친 짓은 자바스크립트의 세세한 함정과 잘 알려지지 않은 부분에 대한 매혹적인 가이드입니다.

오늘 이곳에서 무료로 받아보세요.

<div class="content-ad"></div>


![The 5 most transformative JavaScript features from ES12-17](/assets/img/2024-09-10-The5mosttransformativeJavaScriptfeaturesfromES12_17.png)
