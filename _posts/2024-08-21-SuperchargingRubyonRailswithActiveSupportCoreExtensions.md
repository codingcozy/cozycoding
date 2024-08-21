---
title: "Active Support 핵심 확장 기능으로 루비 온 레일즈 슈퍼차징하기"
description: ""
coverImage: "/assets/img/2024-08-21-SuperchargingRubyonRailswithActiveSupportCoreExtensions_0.png"
date: 2024-08-21 18:01
ogImage: 
  url: /assets/img/2024-08-21-SuperchargingRubyonRailswithActiveSupportCoreExtensions_0.png
tag: Tech
originalTitle: "Supercharging Ruby on Rails with Active Support Core Extensions"
link: "https://medium.com/unagi/supercharging-ruby-on-rails-with-active-support-core-extensions-30aff2300168"
isUpdated: false
---


<img src="/assets/img/2024-08-21-SuperchargingRubyonRailswithActiveSupportCoreExtensions_0.png" />

만약 당신이 루비 온 레일즈를 사용 중이라면, 레일즈가 이미 당신이 필요한 기능을 정확히 제공하는 메소드를 갖고 있다는 사실에 놀란 적이 있을 것입니다. 하지만 그 메소드들의 뒤에 무엇이 있는지 궁금해 본 적이 있나요?

오늘은 루비 온 레일즈의 필수 구성 요소인 Active Support Core Extensions에 대해 이야기해보려고 합니다. Active Support의 목적은 루비에 더 많은 슈퍼파워를 부여하는 확장 및 유틸리티를 제공하는 것입니다. Active Support는 루비의 기본 클래스와 모듈을 추가 메소드로 확장하여 레일즈 개발을 더 직관적이고 강력하게 만들어줍니다.

예를 들어, 아마도 .blank? 메소드를 사용해 본 적이 있을 것인데, 이 메소드는 루비의 핵심 부분이 아닙니다; 루비 온 레일즈에서 Active Support가 제공하는 것입니다. Active Support는 다양한 루비 객체에 blank? 메소드를 추가하여 객체가 "빈"인지 쉽게 확인할 수 있게 해줍니다. "빈"이라는 것은 해당 객체 유형에 따라 nil, empty 또는 공백 문자열인지를 의미합니다.

<div class="content-ad"></div>

```js
nil.blank? # 참
['A'].blank? # 거짓
false.blank? # 참
```

액티브 서포트 없이는 이러한 메서드가 표준 루비에서 사용할 수 없습니다.

# 확장 기능 로드

기본 크기를 가능한 작게 유지하기 위해 액티브 서포트는 기본적으로 최소한의 종속성을 로드합니다. 따라서 아래와 같이 간단히 불러오기만 하면, 액티브 서포트 프레임워크 자체에 필요한 확장만 로드됩니다.


<div class="content-ad"></div>

```js
require "active_support"
```

액티브 서포트는 작은 조각으로 나뉘어 있어 필요한 특정 확장 기능만 로드할 수 있습니다. 또한 한 번에 관련된 확장 기능을 로드하기 위한 편리한 진입점을 제공합니다. 또는 한꺼번에 모든 것을 로드할 수도 있죠.

예를 들어, 문자열을 시간 객체로 변환하는 확장 기능만 필요한 경우 (예를 들어 API와 작업할 때 매우 유용):

```js
require 'active_support/core_ext/string/conversions'

"2024-08-19".to_time # => 2024-08-19 0000 -0300
```

<div class="content-ad"></div>

모든 확장 기능을 한꺼번에로 로드하려면:

```js
require 'active_support/all'
```

# Active Support 확장 기능 소개

Active Support에는 많은 유용한 확장 기능이 포함되어 있습니다. 이미 사용하고 계신 몇 가지 기능을 소개해드리겠습니다. 그들이 어디서 왔는지도 모르고 사용하고 계시는 경우가 많을 것입니다.

<div class="content-ad"></div>

시간 계산

시간과 날짜와 관련된 작업을 하고 계시다면 (예를 들어 이벤트나 알림을 처리하는 앱이라든지), 아마도 다음과 같은 것들을 사용하고 계실 겁니다:

```js
5.days.ago
2.hours.from_now
```

문자열 변형

<div class="content-ad"></div>

다른 형식으로 문자열을 변환하는 것은 우리가 항상 하는 작업이며 Active Support를 사용하면 매우 쉽습니다:

```js
"active_support".camelize # => "ActiveSupport"
"Post".underscore # => "post"
```

해시 슬라이싱

대형 해시와 작업할 때 중요한 키만 선택하는 것은 아주 간단합니다.

<div class="content-ad"></div>

```js
my_hash = { a: 100, b: 200, c: 300 }
my_hash.slice(:a, :c) # => { a: 100, c: 300 }
```

객체 복제

Ruby 2.5부터 대부분의 객체는 dup 또는 clone을 통해 복제할 수 있습니다:

```js
"something".dup     # => "something"
"".dup              # => ""
10.dup              # => 1
10.method(:+).dup   # => TypeError (Method에 대한 할당자가 정의되지 않았습니다)
```

<div class="content-ad"></div>

활성 지원은 객체를 조회하는 데 사용할 수 있는 `duplicable?`을 제공합니다:

```js
"something".duplicable?     # => true
"".duplicable?              # => true
10.method(:+).duplicable?   # => false
```

더 많은 예제를 찾을 수 있는 활성 지원 코어 익스텐션 공식 가이드를 확인하는 것을 추천합니다.

Unagi Software은 루비 온 레일즈 및 자바스크립트에서 +11년 동안 소프트웨어 개발 서비스를 제공해왔습니다. 저희에 대해 더 알아보려면 저희 플랫폼에서 확인해주세요.