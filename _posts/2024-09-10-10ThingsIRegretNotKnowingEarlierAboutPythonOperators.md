---
title: "파이썬 연산자에 대해 알았더라면 더 나았을 10가지"
description: ""
coverImage: "/assets/img/2024-09-10-10ThingsIRegretNotKnowingEarlierAboutPythonOperators_0.png"
date: 2024-09-10 19:31
ogImage: 
  url: /assets/img/2024-09-10-10ThingsIRegretNotKnowingEarlierAboutPythonOperators_0.png
tag: Tech
originalTitle: "10 Things I Regret Not Knowing Earlier About Python Operators"
link: "https://medium.com/gitconnected/10-things-i-regret-not-knowing-earlier-about-python-operators-80fd996aad33"
isUpdated: false
---


마크다운 형식으로 다음을 변경해주세요.


![Python Operators](/assets/img/2024-09-10-10ThingsIRegretNotKnowingEarlierAboutPythonOperators_0.png)

# 1) The walrus operator :=

The walrus operator does 2 things. In x := 5

- it assigns the variable x to the value 5
- the expression (x := 5) returns the value of x itself.


<div class="content-ad"></div>


<img src="/assets/img/2024-09-10-10ThingsIRegretNotKnowingEarlierAboutPythonOperators_1.png" />

이 연산자는 결국 한 줄의 코드를 절약하는 데 도움이 됩니다.

# 2) divmod()

파이썬 기초를 배울 때 우리는 아마 배웠을 겁니다.


<div class="content-ad"></div>

- // 연산자는 버림 나눗셈(또는 정수 나눗셈)을 수행합니다.
- % 연산자는 숫자를 다른 숫자로 나눌 때 나머지를 제공합니다.

![image1](/assets/img/2024-09-10-10ThingsIRegretNotKnowingEarlierAboutPythonOperators_2.png)

하지만 이 둘을 동시에 수행하는 내장 함수가 있다는 것을 알고 계셨나요? divmod() 함수를 사용하면 이를 수행할 수 있습니다. 이 함수는 (몫, 나머지) 튜플을 반환합니다.

![image2](/assets/img/2024-09-10-10ThingsIRegretNotKnowingEarlierAboutPythonOperators_3.png)

<div class="content-ad"></div>

# 3) %로 문자열 서식 지정하기

% 연산자는 템플릿 문자열을 서식 지정하는 데에도 사용할 수 있어요.

![이미지](/assets/img/2024-09-10-10ThingsIRegretNotKnowingEarlierAboutPythonOperators_4.png)

이것은 문자열을 서식 지정하는 약간 오래된 방법이에요. 보통 우리는 이제 string.format()을 대신 사용해요.

<div class="content-ad"></div>

하지만 이 형식으로 문자열을 포맷하는 것은 여전히 유효하며, 약간 오래된 Python 코드 베이스에서 발견될 수 있습니다.

# 4) @ for matrix multiplication

이것은 놀라울 수도 있지만, Python에는 행렬 곱셈 연산자가 있습니다. 이를 위해 @ 기호를 사용합니다 (데코레이터와 혼동하지 마세요)

평소처럼 int, float 등과 같은 데이터 유형은 행렬 곱셈을 수행할 수 없을 수 있지만, numpy 라이브러리의 데이터 유형은 가능합니다.

<div class="content-ad"></div>

\<img src="/assets/img/2024-09-10-10ThingsIRegretNotKnowingEarlierAboutPythonOperators_5.png" />

그리고 클래스에서 __matmul__ 매직 메소드를 정의하면, 사용자 정의 객체에 대해 @ 연산자를 사용할 때 동작을 사용자 정의할 수 있습니다.

\<img src="/assets/img/2024-09-10-10ThingsIRegretNotKnowingEarlierAboutPythonOperators_6.png" />

# 빠른 일시 정지

<div class="content-ad"></div>

제가 최근에 쓴 책인 "파이썬에 대해 몰랐던 101가지"를 소개해요.

여기에는 많은 사람들이 (아마도 대부분이) 알지 못하는 101가지 멋진 파이썬 트릭과 사실들을 모았어요. 파이썬 지식을 더 향상시키고 싶다면 꼭 확인해보세요!

링크: [payhip.com/b/vywcf](https://payhip.com/b/vywcf)

# 5) 연산자와 매직 메서드

<div class="content-ad"></div>

파이썬의 내장 연산자를 사용할 때 실제로는 백엔드에서 해당하는 매직 메소드를 호출하고 있어요.

![매직 메소드](/assets/img/2024-09-10-10ThingsIRegretNotKnowingEarlierAboutPythonOperators_7.png)

- 덧셈 연산자에는 __add__
- 뺄셈 연산자에는 __sub__
- 곱셈 연산자에는 __mul__
- 나눗셈 연산자에는 __truediv__
- 정수 나눗셈 연산자에는 __floordiv__ 가 사용됩니다.

이와 같이 사용할 수 있는 연산자는 사용자 정의 클래스에서도 정의해 줌으로써 사용자 정의 객체에서 해당 연산자를 사용할 수 있어요.

<div class="content-ad"></div>


![Image description](/assets/img/2024-09-10-10ThingsIRegretNotKnowingEarlierAboutPythonOperators_8.png)

# 6) Compound assignment operators

Let’s say we have an existing integer variable x. And we wish to increment x by 1.

![Image description](/assets/img/2024-09-10-10ThingsIRegretNotKnowingEarlierAboutPythonOperators_9.png)


<div class="content-ad"></div>

이 작업에 대해 복합 대입 연산자를 사용할 수도 있습니다.

![이미지](/assets/img/2024-09-10-10ThingsIRegretNotKnowingEarlierAboutPythonOperators_10.png)

이는 다른 모든 숫자 연산자에도 적용됩니다.

- x += 1은 x = x + 1과 동일합니다.
- x -= 1은 x = x - 1과 동일합니다.
- x *= 1은 x = x * 1과 동일합니다.
- x /= 1은 x = x / 1과 동일합니다.
- x //= 1은 x = x // 1과 동일합니다.
- x %= 1은 x = x % 1과 동일합니다.
- x **= 1은 x = x ** 1과 동일합니다.

<div class="content-ad"></div>

그리고 매번, 각 compound assignment 연산자에는 자체의 마법 메서드가 있어서 백그라운드에서 작동합니다.

- x += 1는 __iadd__를 사용합니다.
- x -= 1는 __isub__를 사용합니다.
- x *= 1는 __imul__를 사용합니다.
- x /= 1는 __itruediv__를 사용합니다.
- x //= 1는 __ifloordiv__를 사용합니다.
- x %= 1는 __imod__를 사용합니다.
- x ** 1는 __ipow__를 사용합니다.

# 7) -`를 사용한 타입 힌팅

네, -` 연산자를 사용하는 방법이 있습니다. 이 연산자를 사용하여 함수의 반환 형식을 표시합니다.

<div class="content-ad"></div>


![Image](/assets/img/2024-09-10-10ThingsIRegretNotKnowingEarlierAboutPythonOperators_11.png)

- Notice the `-` float that we place in our function definition.

  This just means that this specific function is meant to return a float value.

![Image](/assets/img/2024-09-10-10ThingsIRegretNotKnowingEarlierAboutPythonOperators_12.png)


<div class="content-ad"></div>

^ 그리고 이러한 경우에 -` list[int]는 우리의 함수가 정수 목록을 반환하기 위해 설계되었음을 의미합니다.

참고 — 유형 힌트는 힌트를 제공하지만 강제하지는 않습니다. 즉, 이 함수가 대신 문자열을 반환하도록 하는 것도 합법적입니다 (하지만 이는 형편없는 실천입니다)

# 8) 삼항 연산자

간단한 if-else 블록이 등급에 어떤 값을 할당하는 경우를 생각해 봅시다.

<div class="content-ad"></div>

아래는 마크다운 형식으로 변경되었습니다.


![image](/assets/img/2024-09-10-10ThingsIRegretNotKnowingEarlierAboutPythonOperators_13.png)

삼항 연산자를 사용하면 if-else 블록을 한 줄의 코드로 대체할 수 있습니다.

![image](/assets/img/2024-09-10-10ThingsIRegretNotKnowingEarlierAboutPythonOperators_14.png)

하나의 삼항 연산자 표현식 안에 여러 개의 ifs와 elses를 갖을 수 있다는 점을 유의하세요.


<div class="content-ad"></div>


![Image](/assets/img/2024-09-10-10ThingsIRegretNotKnowingEarlierAboutPythonOperators_15.png)

# 9) 'None or value' 표현

만약 None 또는 'apple' 표현을 사용한다면, 이 표현은 'apple'로 기본값이 설정됩니다. 이는 첫 번째 값이 falsy 값이기 때문입니다:

![Image](/assets/img/2024-09-10-10ThingsIRegretNotKnowingEarlierAboutPythonOperators_16.png)


<div class="content-ad"></div>

그와 반대로, 'None'을 참값으로 대체하면 표현식이 더 이상 'apple' 값을 갖지 않습니다:

![image1](/assets/img/2024-09-10-10ThingsIRegretNotKnowingEarlierAboutPythonOperators_17.png)

이 (변수 또는 값) 표현식은 종종 변수가 'None'이 아닌지 확인하기 위해 사용됩니다. 간단한 예시로 설명해보겠습니다. 'None'을 반환할 수 있는 함수가 있는 상황을 가정해봅시다.

![image2](/assets/img/2024-09-10-10ThingsIRegretNotKnowingEarlierAboutPythonOperators_18.png)

<div class="content-ad"></div>

여기서는 (get_fruit() 또는 'apple') 표현을 사용합니다:

- get_fruit()가 None을 반환하면, 과일에 기본값 'apple'이 할당됩니다.
- get_fruit()가 오렌지나 배를 반환하면, 과일은 해당 값을 갖습니다.

# 10) 드 모르간 법칙

이 법칙은 괄호와 not 연산자를 다룰 때 중요합니다.

<div class="content-ad"></div>

테이블 태그를 마크다운 형식으로 변경해 주세요.

<div class="content-ad"></div>


![이미지](/assets/img/2024-09-10-10ThingsIRegretNotKnowingEarlierAboutPythonOperators_19.png)

이 조건 앞에 not을 추가하고, 뒤집어 봅시다. 조건 (HAS_PEANUTS 또는 HAS_GARLIC)가 False인 경우, 나는 그 요리를 먹을 수 있습니다.

![이미지](/assets/img/2024-09-10-10ThingsIRegretNotKnowingEarlierAboutPythonOperators_20.png)

다음으로, De Morgan의 법칙을 사용하여 이를 확장해 봅시다. 이런 경우 괄호를 확장할 때에는 and를 or로, or를 and로 바꿔야 한다는 것을 기억해 주세요.


<div class="content-ad"></div>

지금 코드가 무엇을 의미하는지 알겠어요. 만약 우리 요리가 1) 땅콩이 없고 2) 마늘이 없다면, 그 요리를 먹을 수 있어요. 논리적으로 말이 되는 것을 주목해 주세요.

![이미지](/assets/img/2024-09-10-10ThingsIRegretNotKnowingEarlierAboutPythonOperators_21.png)

# 여기에 더 멋진 것들이 있어요!

파이썬 연산자에 대해 이전에 알았다면 후회하지 않을 13가지 것들

<div class="content-ad"></div>

# 결론

이 내용이 명확하고 이해하기 쉬웠으면 좋겠습니다.

# 만약 제가 창작자로서 저를 지원하고 싶다면

- 내 Substack 뉴스레터 가입하기: https://zlliu.substack.com/ — 나는 주간 이메일을 통한 Python에 관한 내용을 전송합니다.
- 내 책을 구매하기: https://payhip.com/b/vywcf — Python에 관한 101가지 알지 못했던 것
- 이 이야기에 대해 50번 박수치기
- 당신의 생각을 나에게 알려주는 댓글 남기기
- 이야기 중 가장 좋아하는 부분을 강조하기

<div class="content-ad"></div>

감사합니다! 작은 행동이 큰 변화를 이룰 수 있고, 정말 감사드립니다!

YouTube: [zlliu246](https://www.youtube.com/@zlliu246)

LinkedIn: [zlliu](https://www.linkedin.com/in/zlliu/)