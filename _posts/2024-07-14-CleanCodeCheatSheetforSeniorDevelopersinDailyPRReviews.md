---
title: "시니어 개발자를 위한 클린 코드 치트 시트 일일 PR 리뷰 필수 가이드"
description: ""
coverImage: "/assets/img/2024-07-14-CleanCodeCheatSheetforSeniorDevelopersinDailyPRReviews_0.png"
date: 2024-07-14 00:51
ogImage: 
  url: /assets/img/2024-07-14-CleanCodeCheatSheetforSeniorDevelopersinDailyPRReviews_0.png
tag: Tech
originalTitle: "Clean Code Cheat Sheet for Senior Developers in Daily PR Reviews"
link: "https://medium.com/@azeynalli1990/clean-code-cheat-sheet-for-senior-developers-in-daily-pr-reviews-6b77ee413469"
isUpdated: true
---




![img](/assets/img/2024-07-14-CleanCodeCheatSheetforSeniorDevelopersinDailyPRReviews_0.png)

요즘에는 기업들이 소프트웨어 엔지니어들에게(주로 시니어 레벨에서…) 요구하는 가장 중요한 기술 중 하나가 반복적이고 점진적인 방식으로 품질 높은 코드 리뷰를 제공하는 것입니다. 이것은 각 PR 리뷰 시에 개발자들이 병합될 코드의 품질을 계속해서 개선하도록 요청받는 것을 의미합니다. 이 게시물에서는 개발자가 리팩토링이나 리뷰를 할 때 마음에 새겨야 할 기본 원칙들을 지적하려고 합니다. 이 포인트들을 코드 조각에 실제로 구현한 것을 보고 싶다면 저의 다른 블로그인 "자바 리팩터링 예제로 배우는 깔끔한 코드 최종 안내서"를 확인해보세요.

이러한 포인트들을 주제별로 하나씩 살펴보겠습니다:

1. 의미 있는 이름 부여

<div class="content-ad"></div>

Utilize descriptive names: Choose names for methods and variables that clearly convey their purpose even without looking at the code.

Class names should be nouns or noun phrases.

Method names should be verbs.

Select one word for each concept: opt for one of "get," "retrieve," or "fetch" and consistently use it for similar actions.

<div class="content-ad"></div>

안녕하세요! 암호화폐 전문가입니다. 

위의 내용을 한국어로 번역해 드리겠습니다.

AccountAdapter라는 용어는 프로그래머들에게는 어댑터 패턴을 의미합니다. 만약 관련된 컴퓨터 과학 용어가 없다면 문제 지향적인 이름을 사용합니다.

Pronounceable names 또는 searchable names을 사용해주세요. IDE에서 특정 구문을 검색하는 것이 더 쉬워질 거예요.

2.함수

함수는 작아야 합니다. 함수가 클수록 디버깅하기 어려워지니까요.

<div class="content-ad"></div>

블록과 들여쓰기는 깔끔하게 해야 합니다: 현재 IDE들은 기본적으로 이러한 형식을 지원하기 때문에 이 점은 실제적이지 않습니다.

하나의 일을 하나의 기능으로 수행하세요.

하나의 함수에 하나의 추상화 수준만 포함시키세요: 함수는 충분히 작아서 하나의 추상화 수준 내에서 구현할 수 있어야 합니다.

위에서 아래로 코드 읽기: 단계별 원칙을 적용해야 합니다. 중첩된 함수는 상위 함수 뒤에 와야 하며, 이렇게 하면 책을 읽는 듯한 좋은 느낌을 줄 수 있습니다.

<div class="content-ad"></div>

적은 입력값을 사용하세요: 0개가 최고이며 1-3은 괜찮지만 3개 이상의 입력값은 최악입니다. 이는 함수가 한 가지 이상의 작업을 수행하고 있을 가능성이 있음을 의미합니다.

부작용이 없어야 합니다: 함수는 단 하나의 작업만 수행하며 그것을 제대로 수행해야 하며 다른 상태에 부정적인 영향을 미치면 안 됩니다.

중복이 없어야 합니다: 자주 사용되는 코드 조각을 한 곳에 집중하세요.

3. 주석

<div class="content-ad"></div>

```python
# Define a function to calculate the square of a number
def calculate_square(number):
    return number ** 2
```

<div class="content-ad"></div>

- 중얼거림
- 중복 된 코멘트
- 오도하는 코멘트
- 의무적인 코멘트
- 일지 코멘트
- 소음 코멘트

너무 많은 정보를 주지 마세요: 코멘트는 간결하고 짧아야 합니다.

너무 명백한 코멘트를 피하세요: 함수가 높이를 계산하는 것이라면 추가 설명이 필요하지 않습니다.

IDE에 의해 대부분 자동으로 처리되는 요즘의 문제입니다.

<div class="content-ad"></div>

수직 형식: 한 클래스 크기 당 최대 200-300 줄의 코드.

신문 비유: 클래스는 신문 기사처럼 되어야 합니다.

수직 간격: 클래스 내 변수/메서드 사이의 수직 간격.

변수 선언: 생성자 앞의 클래스 변수, 사용 근처에 있는 지역 변수.

<div class="content-ad"></div>

## 5. 객체와 데이터 구조

의존 메서드는 구현과 가깝게 배치해야 합니다. 이렇게 하면 코드 라인 사이를 쉽게 이동할 수 있습니다.

수평 밀도를 유지하세요. 긴 줄로 인해 스크롤해야 하는 경우를 피하세요.

팀 규칙: 팀 내 일관성이 깨끗한 코드보다 중요합니다.

<div class="content-ad"></div>

**데이터/Object 안티-대칭**

- 절차적 코드(데이터 구조를 사용한 코드)는 기존 데이터 구조를 변경하지 않고도 새로운 함수를 추가하기 쉽게 만듭니다. 반면 객체 지향 코드는 기존 함수를 변경하지 않고도 새로운 클래스를 추가하기 쉽게 만듭니다.
- 절차적 코드는 모든 함수를 변경해야 하기 때문에 새로운 데이터 구조를 추가하기 어렵게 만듭니다. 객체 지향 코드는 모든 클래스를 변경해야 하기 때문에 새로운 함수를 추가하기 어렵게 만듭니다.

**데메테르의 법칙:** 모듈은 조작하는 객체의 내부를 몰라야 합니다.

**데이터 전송 객체(DTO):** public 변수와 함수가 없는 클래스는 데이터 전송을 쉽게 만들지만, 보안 문제가 발생할 수 있습니다.

<div class="content-ad"></div>

6. 에러 처리

예외 처리를 사용하세요. 널 값을 반환하거나 에러 플래그를 반환하는 대신, 예외를 throw 합니다.

예외에 컨텍스트 제공하기: 잘 구성된 예외 처리 전략을 갖추도록 노력하세요.

널을 반환하지 마세요. 널을 전달하지 마세요.

<div class="content-ad"></div>

**7. 유닛 테스트**

TDD의 법칙:

- 실패하는 유닛 테스트를 작성하기 전에는 프로덕션 코드를 작성할 수 없습니다.
- 실패하도록 충분한 양의 유닛 테스트를 작성할 수 있을 정도만큼만 유닛 테스트를 작성할 수 있습니다.
- 현재 실패하는 테스트를 통과할 정도만큼의 프로덕션 코드를 작성할 수 있을 정도만큼만 프로덕션 코드를 작성할 수 있습니다.

테스트를 청결하고 가독성 있게 유지하세요.

<div class="content-ad"></div>

테스트 당 한 가지 assert를 하세요.

테스트는 F.I.R.S.T. 원칙을 따라야 합니다:

- Fast (빠르게)
- Independent (독립적으로)
- Repeatable (반복 가능하게)
- Self-Validating (자가 검증 가능하게)
- Timely (시기 적절하게)

8. 클래스

<div class="content-ad"></div>

캡슐화: 객체 지향 프로그래밍 활용하기.

SRP: 각 클래스는 단일 책임을 가져야 합니다.

응집성: 함수가 다루는 변수가 많을수록 응집성이 높아집니다.

최소주의 접근 방식을 사용해보세요.

<div class="content-ad"></div>

![이미지](/assets/img/2024-07-14-CleanCodeCheatSheetforSeniorDevelopersinDailyPRReviews_1.png)

클린 코드

클린 코드를 읽으면 당신의 리팩토링 실력이 향상될 것입니다. 이 책이 제 일상적인 코드 리팩토링에 큰 영향을 미쳤습니다.

소프트웨어 엔지니어링 관련 더 많은 주제에 관심이 있다면 아래 목록을 살펴보세요.

<div class="content-ad"></div>

관련 글:

- 프론트엔드 개발을 위한 소프트웨어 아키텍처 패턴
- 일상적인 사용을 위한 소프트웨어 아키텍처 치트 시트
- Spring Boot 응용 프로그램에 구성 응집성 원칙 적용하는 방법
- Spring Boot 응용 프로그램에 SOLID 소프트웨어 디자인 원칙 적용하는 방법

P.S. 트위터나 링크드인에서 저와 소통하실 수 있습니다.