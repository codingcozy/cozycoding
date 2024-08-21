---
title: "CodeIgniter 4 Query Builder의 union 및 unionAll 메서드 사용 방법"
description: ""
coverImage: "/assets/img/2024-07-13-CodeIgniter4QueryBuilderunionandunionAllmethods_0.png"
date: 2024-07-13 21:45
ogImage: 
  url: /assets/img/2024-07-13-CodeIgniter4QueryBuilderunionandunionAllmethods_0.png
tag: Tech
originalTitle: "CodeIgniter 4 Query Builder union() and unionAll() methods"
link: "https://medium.com/gitconnected/codeigniter-4-query-builder-union-and-unionall-methods-de00668e785c"
isUpdated: true
---




UNION 및 UNION ALL 세트 연산자는 1개 이상의 SELECT 쿼리에서 결합된 행을 반환합니다. CodeIgniter 4 Query Builder는 이제 $builder->union() 및 $builder->unionAll() 메서드를 사용하여 UNION 및 UNION ALL 쿼리를 지원합니다. 이 문서에서는 이러한 유형의 쿼리를 생성하는 방법을 배워보세요.

이 문서와 이어지는 많은 문서들은 새로운 개념을 배우면서 쓰고 공유하는 제가 쓰는 시리즈의 일부입니다. 제가 개발자로서 배운 것들을 공유하는 것을 의무로 생각합니다. 서로 배우고 성장하는 과정에서 서로를 지원해야 합니다. #buildinpublic #learninpublic #indiehackers #developer

## PHP 및 MySQL 개발자를 위한 뉴스레터

OpenLampTech 뉴스레터를 구독하면 "10 MySQL Tips For Everyone"이라는 eBook을 무료로 받아보실 수 있습니다.

<div class="content-ad"></div>

## 데이터 설정 및 정리

예제 쿼리에 사용할 간단한 두 개의 테이블, 고객 및 직원 테이블을 사용하고 있어요:

![customer table](/assets/img/2024-07-13-CodeIgniter4QueryBuilderunionandunionAllmethods_0.png)

![employee table](/assets/img/2024-07-13-CodeIgniter4QueryBuilderunionandunionAllmethods_1.png)

<div class="content-ad"></div>

저는 작업 중인 Model 클래스에 다음과 같은 2개의 보호된 속성을 가지고 있습니다:

```js
protected $db;
protected $builder;
```

실행된 모든 쿼리를 $db-`getLastQuery() 메서드를 사용하여 로깅할 것입니다. 과거에 $db-`getLastQuery()에 대해 자세히 작성한 적이 있으므로 이 자리에서 자세히 다루지는 않겠습니다.

## CodeIgniter 4 쿼리 빌더 $builder-`union() 메서드

<div class="content-ad"></div>

이 두 테이블을 사용한 전통적인 데이터베이스 연습 문제 중 하나는 "모든 직원 및 고객을 하나의 테이블에 표시해 보라"는 요구사항일 겁니다.

$builder-`union() 메소드를 사용하여 UNION을 통해 절대로 그것을 할 수 있어요.

![이미지](/assets/img/2024-07-13-CodeIgniter4QueryBuilderunionandunionAllmethods_2.png)

$builder-`union() 호출은 MySQL 코드에 표시된 대로 동등한 UNION 쿼리를 생성합니다:

<div class="content-ad"></div>

<img src="/assets/img/2024-07-13-CodeIgniter4QueryBuilderunionandunionAllmethods_3.png" />

제 관찰 결과를 공유해볼게요:
처음부터 SQL 문이 실행될 때 몇 가지 흥미로운 내용이 있어요:

- 두 SELECT 문 모두 파생 테이블을 쿼리하고 있습니다.
- 각 SELECT 문은 자동으로 별칭이 지정됩니다. `uwrp0`가 주 쿼리이고 UNION의 일부로 포함되는 추가 SELECT는 `uwrp+n` 형태로 지정됩니다.

아래 연관 배열 결과를 보면, 'John Doe' 레코드에 대한 요소가 1개뿐이지만 고객 및 직원 테이블에서 'John Doe' 행이 모두 존재함을 주목해주세요.

<div class="content-ad"></div>

`$builder->union()` 메서드는 중복 행이 없는 고유한 행만 반환합니다.

![이미지](/assets/img/2024-07-13-CodeIgniter4QueryBuilderunionandunionAllmethods_4.png)

귀하의 브랜드, 제품 또는 서비스가 필요로 하는 주목을 OpenLampTech 뉴스레터의 저렴한 분류 광고로 확보하세요. 지원해 주셔서 감사합니다!

## CodeIgniter 4 Query Builder `$builder->unionAll()` 메서드

<div class="content-ad"></div>

만약 중복을 포함한 모든 행이 필요한 경우 $builder->unionAll() 메서드를 사용하면 됩니다:

![이미지](/assets/img/2024-07-13-CodeIgniter4QueryBuilderunionandunionAllmethods_5.png)

다음의 MySQL 코드에서 $builder->unionAll() 메서드에서 UNION ALL 집합 연산자가 사용되는 것을 확인할 수 있습니다:

![이미지](/assets/img/2024-07-13-CodeIgniter4QueryBuilderunionandunionAllmethods_6.png)

<div class="content-ad"></div>

다음의 연관 배열에서 주의해야 할 점은 $builder-`unionAll()이 모든 행을 반환하기 때문에 직원 및 고객 테이블에서 'John Doe' 행이 모두 포함된다는 것입니다:

![이미지](/assets/img/2024-07-13-CodeIgniter4QueryBuilderunionandunionAllmethods_7.png)

CodeIgniter 4는 Query Builder를 사용하여 강력하고 안정적인 쿼리를 작성할 수 있도록 가능하게 하는 릴리스를 지속적으로 향상시키고 있습니다. 댓글란에 모델 쿼리에서 $builder-`union() 및/또는 $builder-`unionAll() 메서드를 사용하고 있거나 이식한 경우 알려주세요.

## 추가로 읽어볼만한 글

<div class="content-ad"></div>

- CodeIgniter 4에서 MySQL 마지막 삽입 ID를 검색하는 방법
- MySQL에서 예제와 함께 CodeIgniter 4 쿼리 매개변수 바인딩
- CodeIgniter 4 쿼리 빌더 join() 메서드 설명

이 게시물을 읽어 주셔서 감사합니다. 좋아하실 다른 분과 공유해주세요.

커피 한 잔으로 내 콘텐츠를 지원해주세요!

Josh Otwell은 PHP 개발자, SQL 전문가 및 기술 블로거/작가로 성장하기를 열정으로 삼고 있습니다.

<div class="content-ad"></div>

고지:이 게시물의 대부분의 예제는 개인 개발/학습 작업 환경에서 수행되었으며 제품 품질 또는 사용 준비 상태로 고려되어선 안됩니다. 특정 목표와 요구 사항은 다를 수 있습니다. 언제나 그렇지만, 할 수 있다고 해서 반드시 해야 하는 것은 아닙니다. 내 의견은 제 개인적인 것입니다.

더 많은 도움이 필요할 때

- 블로그를 시작하고 싶나요? Digital Owl’s Prose 블로그에는 WordPress가 사용되었습니다. 제공된 요금제로 돈을 절약해보세요.
- 브랜드, 제품 또는 서비스가 주목받을 필요가 있나요? OpenLampTech 뉴스레터의 저렴한 분류 광고를 통해 원하는 주목도를 얻을 수 있습니다.
- 다음 웹 애플리케이션 또는 WordPress 사이트를 위한 호스팅이 필요하신가요? 저는 호스팅 지원업체 Hostinger를 적극 추천하며 본인의 낚시 사이트를 호스팅하는 데 사용했습니다. 그들의 서비스는 최고 수준이며 무료 SSL을 제공합니다.
- 자학으로 개발자가 온 것로 깨달은 5가지 사실

공시: 이 게시물에 포함된 일부 서비스 및 제품 링크는 제휴 링크입니다. 해당 링크를 통해 구매하시는 경우 추가 비용 없이 수수료를 받을 수 있습니다.

<div class="content-ad"></div>

## PHP 및 MySQL 개발자를 위한 뉴스레터

OpenLampTech 뉴스레터를 구독하면 전자책 "모두를 위한 10가지 MySQL 팁"을 무료로 받을 수 있습니다.

OpenLampTech 뉴스레터에 저렴한 분류 광고를 게재하여 귀하의 브랜드, 제품 또는 서비스에 필요한 주목을 받으세요. 지원해 주셔서 감사합니다!

2022년 12월 7일에 https://joshuaotwell.com에서 최초로 출판되었습니다.