---
title: "루비 코드를 더 명확하고 높은 품질로 만들기 위한 10가지 Rubocop 규칙"
description: ""
coverImage: "/assets/img/2024-08-21-10RubocopRulesforClearerandHigher-QualityRubyCode_0.png"
date: 2024-08-21 18:05
ogImage: 
  url: /assets/img/2024-08-21-10RubocopRulesforClearerandHigher-QualityRubyCode_0.png
tag: Tech
originalTitle: "10 Rubocop Rules for Clearer and Higher-Quality Ruby Code"
link: "https://medium.com/unagi/10-rubocop-rules-for-clearer-and-higher-quality-ruby-code-eec49d64079e"
isUpdated: false
---


<img src="/assets/img/2024-08-21-10RubocopRulesforClearerandHigher-QualityRubyCode_0.png" />

프로젝트에 RuboCop을 추가하기로 결정했군요. 젬을 추가하고 처음으로 "rubocop" 명령을 실행하면 다음과 같은 결과가 표시됩니다:

<img src="/assets/img/2024-08-21-10RubocopRulesforClearerandHigher-QualityRubyCode_1.png" />

이제 어떻게 해야 할까요?... 무엇이 offense인가요? 무엇이 offense가 아닌가요? 이 규칙들을 변경할 수 있을까요?

<div class="content-ad"></div>

다양한 규칙을 이해하려고 많은 시간을 보냈고 필요한 것을 찾는 것에 많은 노력을 기울였기 때문에, 동일한 처우를 피하도록 도와드릴 수 있어요. 본 글에서는 RuboCop의 주요 규칙과 프로젝트에 맞게 도구를 사용자화하는 방법에 대해 설명할 거에요.

# RuboCop에서 '위반(offense)'이란 무엇인가요?

RuboCop은 루비를 위한 정적 코드 분석기 및 포매터로, 미리 정의된 규칙 세트를 통해 코드를 확인하여 코딩 표준과 스타일 가이드라인을 강요합니다.

위반은 부적절한 들여쓰기나 공백, 코드 복잡성 또는 보안 취약점과 같이 중요한 문제까지 다양할 수 있어요. 각 위반은 문제에 대한 설명 메시지와 함께 제시되며, 종종 어떻게 수정할지에 대한 제안이 포함되어 있어요.

<div class="content-ad"></div>

각 조직마다 고유한 규약과 프로그래밍 선호도가 있기 때문에 RuboCop은 유연성을 제공하여 프로젝트의 특정 요구 사항에 따라 규칙과 위반의 심각성을 사용자 정의할 수 있습니다. 이를 위해 프로젝트의 루트에 rubocop.yml 파일을 생성하여 원하는 규칙을 어떻게 재정의할지 지정해야 합니다 (기본 규칙을 고려합니다).

예를 들어, 이것은 저희 API를 위해 작성한 구성 파일입니다.

# 10 가지 주요 RuboCop 규칙

우리의 코드에 적용할 수 있는 다양한 규칙이 있지만, 저의 프로젝트에서 가장 가치 있고 빈번하게 발생하는 10가지 규칙은 다음과 같습니다:

<div class="content-ad"></div>

- Layout/LineLength: 코드를 읽기 쉽게 유지하고 수평 스크롤을 피하기 위해 최대 라인 길이를 설정하는 것입니다.

```js
# 나쁜 예
puts "루비콥이 설정한 최대 길이를 초과하는 매우 긴 줄입니다. 읽기가 어렵습니다."

# 좋은 예
puts "이 줄은 최대 길이 내에 있어 더 읽기 쉽습니다."
```

![이미지](/assets/img/2024-08-21-10RubocopRulesforClearerandHigher-QualityRubyCode_2.png)

2. Style/StringLiterals: 문자열 리터럴에 대한 일관된 작은 따옴표 또는 큰 따옴표의 사용을 강제하여 코드 스타일을 유지합니다.

<div class="content-ad"></div>

```js
# 나쁨
str1 = "이중 인용부호를 사용한 문자열입니다."
str2 = '단일 인용부호를 사용한 문자열입니다.'

# 좋음
str1 = '단일 인용부호를 사용한 문자열입니다.'
str2 = '다른 단일 인용부호를 사용한 문자열입니다.'
```

![이미지](/assets/img/2024-08-21-10RubocopRulesforClearerandHigher-QualityRubyCode_3.png)

3. Layout/TrailingWhitespace: 코드를 깨끗하게 유지하고 불필요한 문자를 피하기 위해 줄 끝에 있는 여백을 강조합니다. 다음 예시에서 "end" 단어 뒤에 공백 2개가 있습니다.

![이미지](/assets/img/2024-08-21-10RubocopRulesforClearerandHigher-QualityRubyCode_4.png)


<div class="content-ad"></div>

4. 스타일/배열 리터럴의 TrailingCommaInArrayLiteral: 일관된 형식을 위해 배열 리터럴에 후행 쉼표를 요구하거나 금지합니다.

```js
# 나쁨 (후행 쉼표가 필요하지 않은 경우)
array = [
  1,
  2,
]

# 좋음
array = [
  1,
  2
]
```

![이미지](/assets/img/2024-08-21-10RubocopRulesforClearerandHigher-QualityRubyCode_5.png)

5. 레이아웃/해시 정렬: 해시 리터럴에서 키와 값의 일관된 정렬을 강제합니다.

<div class="content-ad"></div>

- EnforcedColonStyle: 해시 내 콜론을 어떻게 정렬할지 결정합니다. 테이블 스타일은 키와 값의 정렬을 왼쪽으로 맞춥니다.

```js
# EnforcedColonStyle: table

# 나쁨
hash = {
  key1: "value1",
  val: "value2",
}

# 좋음
hash = {
  key1: "value1",
  val:  "value2",
}
```

- EnforcedHashRocketStyle: 해시 로켓(=>)의 정렬을 지정합니다. 테이블 스타일은 해시 로켓을 수직으로 정렬합니다.

```js
# EnforcedHashRocketStyle: table

# 나쁨
hash = {
  key1 => "value1",
  val => "value2",
}

# 좋음
hash = {
  key1 => "value1",
  val  => "value2",
}
```

<div class="content-ad"></div>


![Image](/assets/img/2024-08-21-10RubocopRulesforClearerandHigher-QualityRubyCode_6.png)

6. Lint/RescueWithoutErrorClass: 예외를 처리할 때 오류 클래스를 지정하지 않으면 예기치 않은 오류를 잡지 못하도록 하기 위해 예외 처리를 권장합니다.

```js
# 나쁜 예
begin
 # 일부 코드
rescue
 # 오류 처리
end

# 좋은 예
begin
 # 일부 코드
rescue StandardError
 # 오류 처리
end
```

![Image](/assets/img/2024-08-21-10RubocopRulesforClearerandHigher-QualityRubyCode_7.png)


<div class="content-ad"></div>

7. 명명/메소드명: 메소드의 명명 규칙을 강제하여 일관성과 명확성을 보장합니다.

```js
# 나쁜 예 (이 규칙은 기본적으로 snake_case를 강제합니다)
def SomeMethodName
 # 코드 작성
end

# 좋은 예
def some_method_name
 # 코드 작성
end
```

![Rubocop 규칙](/assets/img/2024-08-21-10RubocopRulesforClearerandHigher-QualityRubyCode_8.png)

8. 린트/불필요한할당: 불필요한 변수 할당을 탐지하고 플래그를 설정하여 중복 코드를 정리하는 데 도움이 됩니다.

<div class="content-ad"></div>

```js
# 나쁨
unused_variable = 42
puts "Hello, world!"

# 좋음
puts "Hello, world!"
```

<img src="/assets/img/2024-08-21-10RubocopRulesforClearerandHigher-QualityRubyCode_9.png" />

9. Style/SymbolArray: 배열의 심볼에 일관된 구문을 강제하며, 간결성을 위해 새로운 %i 표기법을 선호합니다.

```js
# 나쁨 (구문 오래된 버전)
symbols = [:foo, :bar, :baz]

# 좋음 (새로운 구문)
symbols = %i[foo bar baz]
```

<div class="content-ad"></div>


![image](/assets/img/2024-08-21-10RubocopRulesforClearerandHigher-QualityRubyCode_10.png)

10. Naming/RescuedExceptionsVariableName: 예외를 처리하는 변수 이름이 일관적이고 의미 있는지 확인합니다.

```js
# 나쁨
rescue => ex
puts ex.message

# 좋음
rescue => e
puts e.message
```

![image](/assets/img/2024-08-21-10RubocopRulesforClearerandHigher-QualityRubyCode_11.png)


<div class="content-ad"></div>

기본 규칙과 여기서 설명한 규칙 이외에도, 여러분의 프로젝트의 요구 사항을 분석하고 공식 Rubocop 문서를 검토하여 여러분에게 적합한 다른 규칙을 찾아보는 걸 권유합니다.

그들을 이해하고 설정함으로써 여러분의 Ruby 코드가 깨끗하고 일관되며 유지 보수 가능하게 유지할 수 있습니다 ✨.

Unagi는 12년 이상 지난 후에도 선택한 #Ruby와 #JavaScript 스택에서 소프트웨어 개발 서비스를 제공합니다. 더 많은 정보를 확인해보세요.