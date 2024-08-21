---
title: "올림픽 메달 순위 웹 컴포넌트 만드는 방법"
description: ""
coverImage: "/assets/img/2024-08-03-OlympicMedalRankingWebComponent_0.png"
date: 2024-08-03 18:15
ogImage: 
  url: /assets/img/2024-08-03-OlympicMedalRankingWebComponent_0.png
tag: Tech
originalTitle: "Olympic Medal Ranking Web Component"
link: "https://dev.to/dannyengelman/olympic-medal-ranking-web-component-2m57"
isUpdated: true
updatedAt: 1724246323347
---


## TL;DR

- 이 블로그 글은 HTML의 힘에 관한 것입니다.
- 실시간 예시: https://olympic-medal-ranking.github.io/
- Kevin Le가 만든 올림픽 데이터 API
GitHub Kevle1 - 엔드포인트
- 모든 나라 SVG 국기가 하나의 30 KB 파일에 표시됨
GitHub FlagMeister
- 웹 구성 요소 하나로 전체 올림픽 메달 순위 표시
GitHub `olympic-medal-ranking`

![올림픽 메달 순위](/assets/img/2024-08-03-OlympicMedalRankingWebComponent_0.png)

## `olympic-medal-ranking` 웹 구성 요소

<div class="content-ad"></div>

- 웹 구성 요소/사용자 지정 요소 기술은 모든 최신 브라우저에서 사용 가능한 표준 기술입니다.
- 웹 구성 요소는 개발자를 위한 장난감이 아니며 다른 기술과 비교할 수 없습니다.
- 웹 구성 요소는 HTML 사용자에게 권한을 부여합니다! 💪🏽
- 용어인 웹 구성 요소와 사용자 지정 요소는 서로 바꿔서 사용됩니다. 웹 구성 요소는 여러 기술을 포함하며, 사용자 지정 요소란 명확히 새로운 HTML 요소를 정의하는 API를 가리킵니다.

필요한 모든 HTML은 다음과 같습니다:

```js
<script src="https://olympic-medal-ranking.github.io/element.min.js"></script>

<olympic-medal-ranking></olympic-medal-ranking>
```

표시하려면:

<div class="content-ad"></div>

<img src="/assets/img/2024-08-03-OlympicMedalRankingWebComponent_1.png" />

### 깃발은 깃발인가요?

많은 사이트들이 국기를 사용하듯이, 올림픽 사이트 또한 각 나라의 국기를 PNG 파일로 사용합니다.
이는 올림픽 메달 순위 페이지를 위해 총 56개의 파일이 필요하며 압축하여 103KB가 필요합니다.

<img src="/assets/img/2024-08-03-OlympicMedalRankingWebComponent_2.png" />

<div class="content-ad"></div>


![FlagMeister Web Component](/assets/img/2024-08-03-OlympicMedalRankingWebComponent_3.png)

#### The FlagMeister Web Component (since 2018)

Many moons ago I created a SINGLE 30 KB Web Component file de-hydrating all country flags as SVG (in IMG tags) (https://flagmeister.github.io/)

![Olympic Medal Ranking Web Component](/assets/img/2024-08-03-OlympicMedalRankingWebComponent_4.png)


<div class="content-ad"></div>

아래의 HTML 요소를 사용하면 됩니다:


```js
<script src="https://flagmeister.github.io/elements.flagmeister.min.js" />
```

```css
<style>
  img { width:100px; border: 1px solid grey }
</style>
```


<h1><flag-olympic></flag-olympic>Olympic Medal Ranking - Top 5</h1>
<flag-cn></flag-cn>
<flag-fr></flag-fr>
<flag-jp></flag-jp>
<flag-au></flag-au>
<flag-gb></flag-gb>


모든 국기 웹 구성요소는 추가적인 HTML을 생성합니다:


<img src="/assets/img/2024-08-03-OlympicMedalRankingWebComponent_5.png" />


<div class="content-ad"></div>

<img src="/assets/img/2024-08-03-OlympicMedalRankingWebComponent_6.png" />

### 웹 구성 요소는 라이브러리/프레임워크와 다릅니다

웹 구성 요소를 정의하는 파일이 로드되는 시점이 중요하지 않습니다.
`script` 태그를 사용하여 파일의 어디에든 배치할 수 있습니다.
임포트 문은 언제든지 사용할 수 있습니다.
DOM 페이지에 있는 모든 기존 정의되지 않은 요소들은 자동으로 업그레이드됩니다.

CSS의 :defined도 참고하세요 - https://developer.mozilla.org/en-US/docs/Web/CSS/:defined

<div class="content-ad"></div>

<img src="/assets/img/2024-08-03-OlympicMedalRankingWebComponent_7.png" />

### 올림픽 데이터

Kevin Le는 올림픽 데이터 API를 간결한 JSON 형식으로 단순화했습니다.

그리고 요청에 따라 몇 분 내에 ISO-3166-Alpha2 코드를 추가하여 국기와 일치시켰습니다. 왜냐하면 IOC - 국제 올림픽 위원회가 자체 국가 코드를 사용하며 ISO 표준과 100% 일치하지 않기 때문입니다.

- GitHub: https://github.com/kevle1/paris-2024-olympic-api
- API 엔드포인트: https://api.olympics.kevle.xyz/medals?iso_codes=true

<div class="content-ad"></div>

당신이 개발자이군요! 위의 텍스트를 친근하게 한국어로 번역해 드리겠습니다.


<img src="/assets/img/2024-08-03-OlympicMedalRankingWebComponent_8.png" />

<img src="/assets/img/2024-08-03-OlympicMedalRankingWebComponent_9.png" />

### 올림픽 메달 순위 웹 컴포넌트

우리는 국기 및 메달 데이터를 가져왔습니다;
이제 새로운 웹 컴포넌트로 메달 순위를 생성할 수 있습니다.


<div class="content-ad"></div>

- 케빈 레의 JSON 엔드포인트를 가져와주세요.
- 모든 레코드를 반복하여 순위, 국기, 나라 이름, 메달 수를 표시해주세요.
- 순위, 국기, 나라 이름, 메달 수를 표시해주세요.

필요한 HTML은 다음과 같습니다:

```js
<script src="https://olympic-medal-ranking.github.io/element.min.js"></script>

<olympic-medal-ranking></olympic-medal-ranking>
```


<img src="/assets/img/2024-08-03-OlympicMedalRankingWebComponent_10.png" />

<div class="content-ad"></div>

## 웹 컴포넌트는 shadowDOM/shadowRoot에서 HTML을 생성합니다

웹 컴포넌트는 Shadow DOM을 사용하여 콘텐츠를 캡슐화하여 전역 스타일링과 충돌하는 ID 값을 방지합니다. 이는 CSS를 간결하게 유지하고 HTML 코드를 깔끔하게 유지하여 웹 컴포넌트가 작고 효율적으로 유지됩니다.

![image](/assets/img/2024-08-03-OlympicMedalRankingWebComponent_11.png)

### 전역 CSS로 shadowDOM에 스타일을 지정할 수 없습니다!

<div class="content-ad"></div>

볼록스! 
shadowDOM을 스타일링할 수는 있지만 Web 구성 요소 작성자가 part 속성으로 스타일링할 수 있도록 허용한 부분만 스타일을 적용할 수 있어요:

```js
<tr id="CHN" title="China">
  <td class="rank" part="rank">1</td>
  <td class="flag"><flag-cn is="flag-cn" alt="China">...</flag-cn></td> 
  <td part="countrycode"> CHN</td>
  <td part="countryname">China</td>
  <td class="medals gold" part="medal medalgold">11</td>
  <td class="medals silver" part="medal medalsilver">7</td>
  <td class="medals bronze" part="medal medalbronze">3</td>
  <td class="medals total" part="medal medaltotal">21</td>
</tr>
```

따라서 Web 구성 요소 작성자가 많은 part 속성을 추가했다면 Global CSS를 사용할 수 있어요.

```js
<style id="PARTS">
  olympic-medal-ranking {
    &::part(table) {
        max-width: 550px;
    }
    &::part(header) {
        font-size: 150%;
        color: goldenrod;
        background: lightgrey;
        text-shadow: 1px 1px 1px black;
    }
    &::part(medal) {
        font-weight: bold;
    }
    &::part(rank),
    &::part(countrycode),
    &::part(medaltotal) {
        font-weight: normal;
        color: grey;
    }
  }
</style>
```

<div class="content-ad"></div>


![Image 1](/assets/img/2024-08-03-OlympicMedalRankingWebComponent_12.png)

![Image 2](/assets/img/2024-08-03-OlympicMedalRankingWebComponent_13.png)

참고:

shadowDOM은 이제 10년 이상 된 기술입니다.
모든 `input`, `textarea`, `video` (그 외 많은 것들) 태그는 많은 해동안 shadowDOM을 사용해 왔습니다.
이것은 브라우저 벤더 전용 버전인 user-agent shadowDOM이며, 글로벌 CSS로 스타일을 지정할 수 없습니다 (브라우저 벤더가 스타일링을 위해 추가적인 후크를 프로그래밍하지 않는 한).


<div class="content-ad"></div>

마크다운 형식으로 테이블 태그를 변경해주세요.

<div class="content-ad"></div>

HTML 속성 flag, total 및 iocfilter으로 생성할 수 있습니다.

```js
<olympic-medal-ranking 
 flag="EU" 
 total="all"
 iocfilter="AUT,BEL,BUL,CRO,CYP,CZE,DEN,EST,FIN,FRA,GER,GRE,HUN,IRL,ITA,LAT,LTU,LUX,MLT,NED,POL,POR,ROU,SVK,SLO,ESP,SWE"
>유럽 연합 메달 랭킹</olympic-medal-ranking>
```

그리고 `thead`에 대한 EU 스타일링은 다음과 같습니다.

```js
<style>
  olympic-medal-ranking[flag="EU"] {
    &::part(thead) {
      background: #003399;
    }
    &::part(header) {
      background: inherit;
      color: gold;
      font-weight: bold;
    }
  }
</style>
```

<div class="content-ad"></div>


![Olympic Medal Ranking Web Component 16](/assets/img/2024-08-03-OlympicMedalRankingWebComponent_16.png)

![Olympic Medal Ranking Web Component 17](/assets/img/2024-08-03-OlympicMedalRankingWebComponent_17.png)

### If you do want the source...

This was it, a blog-post for HTML Users, no JavaScript code displayed


<div class="content-ad"></div>

위 웹 구성 요소 소스 코드는 다음 위치에서 확인할 수 있어요:  
[https://github.com/olympic-medal-ranking/olympic-medal-ranking.github.io](https://github.com/olympic-medal-ranking/olympic-medal-ranking.github.io)

해당 소스 코드는 Unlicense로 제공되어 있어, 자유롭게 사용할 수 있어요.

또는 웹 구성 요소를 사용하기 위한 JavaScript 소스가 포함된 JSFiddle을 포크할 수도 있어요:

![Olympic Medal Ranking Web Component](/assets/img/2024-08-03-OlympicMedalRankingWebComponent_18.png)