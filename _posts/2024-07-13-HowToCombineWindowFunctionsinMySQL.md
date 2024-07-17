---
title: "MySQL에서 윈도우 함수를 결합하는 방법"
description: ""
coverImage: "/assets/img/2024-07-13-HowToCombineWindowFunctionsinMySQL_0.png"
date: 2024-07-13 21:51
ogImage: 
  url: /assets/img/2024-07-13-HowToCombineWindowFunctionsinMySQL_0.png
tag: Tech
originalTitle: "How To Combine Window Functions in MySQL"
link: "https://medium.com/gitconnected/how-to-combine-window-functions-in-mysql-c4efa608b282"
---


윈도우 함수는 행 그룹에 작용하여 그룹의 각 행마다 값을 반환합니다. 이는 여러 가지 다른 윈도우 함수로 가능한데, 이를 동일한 수준에서 결합할 수 있을까요? 이 기사에서 더 알아보세요.

![이미지](/assets/img/2024-07-13-HowToCombineWindowFunctionsinMySQL_0.png)

## 배경 이야기와 이유

최근 오라클 공간 함수를 이용해 지리적 라인 문자열을 구성하는 쿼리를 작업했습니다. 함수 매개변수 중 하나로, 라인 문자열 상의 시작 위치(좌표)에서 각 좌표 간격마다 실행 거리 측정을 총계해야 했습니다.

<div class="content-ad"></div>

이것은 SUM() 및 OVER()를 사용하여 롤링 총합을 계산하는 간단한 사용 사례 같아요. 맞아요, 맞습니다.

거의요.

## 샘플 데이터

간단히 하기 위해 예제에서는 복잡한 북위, 동위 또는 선형 참조를 사용하지 않고 대신 블로그 게시물에 이 간단한 데이터 세트를 사용하겠습니다:

<div class="content-ad"></div>

```js
SELECT * FROM coords_measure;
```

<img src="/assets/img/2024-07-13-HowToCombineWindowFunctionsinMySQL_1.png" />

저는 대부분의 LAMP 스택 애플리케이션에서 MySQL 또는 MariaDB를 사용하고 블로그를 운영하기 때문에 이 예제에서도 MySQL을 사용할 것입니다 (원래 오라클 SQL에서 필요한 요구 사항이었음에도 불구하고).

결과 집합의 마지막 행에 start_segment, end_segment 및 segment_length 열에 대해 NULL 값을 가진 것을 알 수 있습니다.

<div class="content-ad"></div>

그 위치가 라인 문자열의 끝점(좌표)이며 실제 길이 측정이 없기 때문입니다. 말 그대로 '막다른 곳'이라고 할 수 있어요.

## MySQL 창 함수를 이용한 롤링 SUM()

롤링 합계를 계산하는 것은 MySQL v8을 사용할 수 있으므로 그다지 어렵지 않아요.

다음 쿼리에서 보듯이, SUM() 집계 함수를 오버() 절과 특정 순서와 함께 사용하면 이 정보를 얻을 수 있어요:

<div class="content-ad"></div>


<img src="/assets/img/2024-07-13-HowToCombineWindowFunctionsinMySQL_2.png" />

롤링 합계 계산이 있지만, 이 문맥에서는 잘못되었습니다. 마지막 2행의 롤링 합계 값이 195.83으로 나타납니다.

그러나 특정 요구 사항에 따라 실제 필요한 것은 마지막 행의 롤링 합계 계산에만 195.83이 있고, 첫 번째 행에는 0이어야 합니다.

귀하의 브랜드, 제품 또는 서비스가 OpenLampTech 뉴스레터에 저렴한 분류 광고로 필요한 주목을 받을 수 있습니다. 지원해 주셔서 감사합니다!


<div class="content-ad"></div>

거의 마치 모든 누적 합계 값들을 한 행 아래로 이동해야 하는 것처럼 보입니다.

하지만 왜 그럴까요?

이 지리적 요구 사항에서는 각 좌표 지점에서 거리가 시작 또는 첫 번째 좌표 (행 1)부터 누적되어야 합니다.

따라서, 행 1은 실제로 0의 거리 측정 값을 가져야 합니다. 왜냐하면 그것은 라인 문자열의 시작점이기 때문이며 (거리 값이 없습니다 - 시작점이기 때문), 행 2는 이 두 좌표 사이에 측정된 거리 값을 가지고 있어야 합니다. 왜냐하면 이 거리는 다시 시작점 (좌표 1)으로 돌아가는 거리이기 때문입니다.

<div class="content-ad"></div>

## MySQL LAG() 창 함수

LAG() Window Function을 사용하면 결과 세트의 현재 작업 행에서 이전 행에 있는 데이터에 액세스할 수 있습니다. LAG() 함수 호출에서 제공된 2번째 매개변수에 따라 1(기본값) 또는 그 이상의 행에 액세스할 수 있습니다.

쿼리에서 LAG()를 사용하여 각 이전 행의 segment_length 값을 반환할 것입니다:

![이미지](/assets/img/2024-07-13-HowToCombineWindowFunctionsinMySQL_3.png)

<div class="content-ad"></div>

쿼리 결과를 통해 볼 때, 이전 행의 값이 없는 경우 LAG()는 NULL을 반환합니다.

내 경험상, 특정 순서에 의존한다면 LAG()에서 ORDER BY 절에 사용할 수 있는 의미있는 지표가 필요합니다.

이제 우리는 단순히 누적 합을 수행하죠?

동일 레벨에서 2개의 윈도우 함수를 결합할 수 있을까요?

<div class="content-ad"></div>

그 쿼리를 실행하면 여기에서 LAG()를 사용할 수 없다는 특정 오류가 발생합니다.

## MySQL 파생 테이블

FROM 절에서 테이블을 생성하는 SELECT 쿼리를 MySQL에서 파생 테이블이라고 합니다.

<div class="content-ad"></div>

LAG() 함수 출력의 rolling sum을 계산하기 위해서는 파생 테이블을 사용하여 생성 쿼리를 FROM 절에 배치해야 합니다:

![image](/assets/img/2024-07-13-HowToCombineWindowFunctionsinMySQL_4.png)

팁: MySQL 파생 테이블은 별칭을 가져야 합니다. 하지만 AS 키워드는 선택사항입니다.

좋은 실천으로, LAG() 윈도우 함수 호출에서 NULL을 고려하여 SUM() OVER()에서의 수학이 정확하거나 계산을 수행할 때 안전하게 수행되도록 해야 합니다. MySQL에서 간단한 방법은 IFNULL() 함수를 사용하는 것입니다.

<div class="content-ad"></div>

![그림](/assets/img/2024-07-13-HowToCombineWindowFunctionsinMySQL_5.png)

이제 쿼리 결과에서 보여지듯이, 각 좌표 간에 롤링 합으로 올바른 거리 측정 값을 가지고 있습니다.

추천 도서 읽기: SQL 전문가가 되는 데 도움이 되는 5권의 SQL 도서.

솔직히 말해서, 이 쿼리 결과를 가져오는 다른 방법이 있을 수 있지만, 특정 요구 사항을 충족시키기 위해 이 방법이 제대로 작동했습니다. 이 특정 쿼리를 작업하면서 LAG() 및 윈도우 함수를 사용한 롤링 합 계산에 대해 많은 것을 배웠으며, 여러분도 이를 통해 많은 것을 배우셨으면 좋겠습니다.

<div class="content-ad"></div>

이 글을 읽어 주셔서 감사합니다. 이 글을 즐길 수 있는 다른 사람과 공유해 주세요.

커피는 제가 가장 좋아하는 음료입니다!!!

Josh Otwell은 PHP 개발자 및 SQL 전문가로 성장하고 기술 블로거/작가로 활동하고 있습니다.

참고: 이 글의 대부분의 예시는 개인 개발/학습 워크스테이션 환경에서 수행되었으며 제품 품질이거나 사용 준비가 되어 있다고 보기에 적합하지 않습니다. 여러분의 특정 목표와 필요성은 상이할 수 있습니다. 항상 그렇듯이, 무언가를 할 수 있다고 해서 반드시 해야 하는 것은 아닙니다. 제 의견은 제 개인적인 것입니다.

<div class="content-ad"></div>

더 도와드릴 수 있는 방법

- 블로그를 시작하려고 하는가요? 디지털 올의 프로즈 블로그에는 워드프레스를 사용하고 있어요. 제공되는 요금제로 돈을 절약해봐요.
- 브랜드, 제품 또는 서비스에 합리적인 가격으로 광고를 할 수 있는 OpenLampTech 뉴스레터의 분류 광고를 통해 주목받아보세요.
- 다음 웹 응용프로그램이나 워드프레스 사이트를 위한 호스팅이 필요한가요? 저는 Hostinger를 적극 추천하며, 남아메리카 바스 낚시 사이트를 호스팅하는 데 사용하고 있어요. 그들의 서비스는 최고이고, 무료 SSL을 제공해요.
- 자학으로 개발자가 되면서 깨달은 5가지 진실

공지: 이 글의 일부 서비스 및 제품 링크는 제 제휴 링크입니다. 해당 링크 중 하나를 클릭하여 구매하시면 추가 비용 없이 커미션이 지급됩니다.

OpenLampTech 뉴스레터를 구독하면 제 eBook인 "모두를 위한 10가지 MySQL 팁"을 무료로 받아보세요.

<div class="content-ad"></div>

귀하의 브랜드, 제품 또는 서비스가 OpenLampTech 뉴스레터에 저렴한 분류 광고를 통해 필요한 주목을 받을 수 있습니다. 지원해 주셔서 감사합니다!

2022년 10월 12일에 https://joshuaotwell.com에서 최초로 발행되었습니다.