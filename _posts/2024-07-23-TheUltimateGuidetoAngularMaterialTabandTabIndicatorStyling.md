---
title: "2024년 최신 Angular Material 탭 및 Indicator 스타일링 궁극적인 가이드"
description: ""
coverImage: "/assets/img/2024-07-23-TheUltimateGuidetoAngularMaterialTabandTabIndicatorStyling_0.png"
date: 2024-07-23 21:16
ogImage: 
  url: /assets/img/2024-07-23-TheUltimateGuidetoAngularMaterialTabandTabIndicatorStyling_0.png
tag: Tech
originalTitle: "The Ultimate Guide to Angular Material Tab and Tab Indicator Styling"
link: "https://medium.com/the-clever-dev/the-ultimate-guide-to-angular-material-tab-and-tab-indicator-styling-e85851f7cd60"
isUpdated: true
---




앵귤러 머티리얼 탭 컴포넌트는 사용자 정의 테두리 스타일링, 탭 인디케이터 색상, 활성 색상 등을 가질 수 있습니다. 이 튜토리얼에서는 DOM을 탐색하고 원하는 탭 스타일을 추가할 수 있는 클래스를 찾아보겠습니다.

앵귤러 머티리얼 탭 컴포넌트는 전역 수준으로 스타일을 호이스팅하지 않으면 스타일링할 수 없습니다. 이는 앵귤러 머티리얼을 사용할 때 가장 큰 답답함 중 하나입니다. 이 요구 사항은 사용 중인 버전에 따라 영향을 받을 수 있습니다.

```js
@Component({ encapsulation: ViewEncapsulation.None });
```

이 튜토리얼에서 구축할 탭 컴포넌트는 다음과 같습니다:

<div class="content-ad"></div>


![이미지](/assets/img/2024-07-23-TheUltimateGuidetoAngularMaterialTabandTabIndicatorStyling_0.png)

이 Angular Material Tab 구성 요소 예제의 전체 코드는 리소스 섹션에 있습니다.

# Angular Material 탭 테두리 색상 변경 방법

Tab 구성 요소는 기능과 스타일을 부여하기 위해 여러 자식 요소로 구성됩니다. 템플릿에서는 mat-tab-group과 mat-tabs를 사용합니다. mat-tab-group은 mat-tab-header와 div 자식으로 렌더링되며 div는 탭 내용을 감싸고 있습니다.


<div class="content-ad"></div>

탭 주위에 테두리를 추가하려면 CSS 선택기를 사용하여 mat-tab-header를 선택해야 합니다. 탭 내용 주위에 테두리를 추가하려면 mat-mdc-tab-body wrapper 클래스를 대상으로 해야 합니다.

![Tab Header](/assets/img/2024-07-23-TheUltimateGuidetoAngularMaterialTabandTabIndicatorStyling_1.png)

위 스크린샷에서 탭 헤더 섹션에 빨간색 테두리를 추가했습니다. 이를 위해 mat-mdc-tab-header를 대상으로 했음을 확인할 수 있습니다:

```css
.mat-mdc-tab-header { border: 1px solid red; }
```

<div class="content-ad"></div>

Angular Material Table 헤더를 스타일링하는 방법을 안내해 드릴게요.

# Angular Material 탭 배경색 변경 방법

탭에 배경색을 추가하기 위해 여러 가지 CSS 셀렉터를 사용할 수 있습니다. 저는 .mat-mdc-tab.mdc-tab라는 간단한 셀렉터 콤보를 대상으로 선택했어요.

```js
.mat-mdc-tab.mdc-tab { background-color: grey; }
```

<div class="content-ad"></div>

<img src="/assets/img/2024-07-23-TheUltimateGuidetoAngularMaterialTabandTabIndicatorStyling_2.png" />

화면 스크린샷에 대상으로 한 div에 있는 클래스를 볼 수 있습니다.

# Angular Material 탭 콘텐츠 높이 및 배경색 변경 방법

탭 콘텐츠 영역이 매우 작고 글꼴 크기가 큰 콘텐츠가 맞지 않는 것을 발견했습니다. mat-mdc-tab-body 및 mat-mdc-tab-body-active 클래스를 대상으로 수정할 수 있습니다:

<div class="content-ad"></div>

```js
.mat-mdc-tab-body.mat-mdc-tab-body-active { min-height: 48px; background-color: coral; }
```
동일한 선택기로 배경색을 추가했습니다.

결과는 다음과 같습니다:

<img src="/assets/img/2024-07-23-TheUltimateGuidetoAngularMaterialTabandTabIndicatorStyling_3.png" />

<div class="content-ad"></div>

액티브 클래스를 타겟팅하여 활성 탭에만 스타일이 적용되도록 했어요.

# Angular Material 탭 활성 텍스트 색상 및 배경색 변경 방법

선택된 탭이 명확하게 나타나길 원한다면, 선택된 탭에 배경색 또는 사용자 지정 텍스트 색상을 추가할 수 있어요.

선택된 탭에 대해 적용된 액티브 클래스를 타겟하여 배경색을 추가할 수 있어요:

<div class="content-ad"></div>

```css
.mat-mdc-tab.mdc-tab { background-color: grey; &.mdc-tab-indicator--active { background-color: blue; } }
```

이 코드는 우리의 .mat-mdc-tab.mdc-tab 선택기 안에 중첩되어 있는 것을 주목해주세요.

다음으로는 활성 탭의 레이블을 활성화하려고 합니다. 이를 위해서는 올바른 선택기를 얻기 위해 DOM을 탐색해야 합니다:

<img src="/assets/img/2024-07-23-TheUltimateGuidetoAngularMaterialTabandTabIndicatorStyling_4.png" />


<div class="content-ad"></div>

해당 요소를 대상으로 한 복잡한 선택기를 주목해주세요. 이 선택기는 Angular Material이 탭의 텍스트 색상을 스타일링하는 데 사용하는 선택기와 동일하며, 마찬가지로 특정성을 유지하기 위해 동일한 선택기를 사용해야 했습니다. 아래는 코드 예시입니다:

```js
.mat-mdc-tab:not(.mat-mdc-tab-disabled).mdc-tab--active .mdc-tab__text-label { color: orange; }
```

# Angular Material 탭 인디케이터 색상 및 높이 변경 방법

선택된 탭은 그 아래에 밑줄이 생깁니다. 개발자 도구를 통해 해당 요소를 볼 수 있는데, 이는 클래스가 tab-indicator인 div입니다. 때때로 이를 활성 탭 inkbar라고도 합니다.

<div class="content-ad"></div>

탭 지시자를 스타일링하는 데 필요한 선택자가 복잡합니다. 가장 좋은 방법은 개발 도구를 열고 Angular Material이 이미 탭 지시자에 사용 중인 선택자를 확인하는 것입니다:

![이미지](/assets/img/2024-07-23-TheUltimateGuidetoAngularMaterialTabandTabIndicatorStyling_5.png)

Angular Material이 색상을 설정하는 데 사용하는 SCSS 변수 --mdc-tab-indicator-active-indicator-color를 확인할 수 있습니다. 우리는 이 색상을 사용자 정의할 수 있습니다. 위의 스크린샷에서 변수에 대한 새 색상 값을 이미 설정하고, border-top-width를 2px에서 4px로 업데이트했습니다. 아래는 코드입니다:

```js
.mdc-tab-indicator__content--underline { --mdc-tab-indicator-active-indicator-color: green; } .mdc-tab-indicator .mdc-tab-indicator__content.mdc-tab-indicator__content--underline { border-top-width: 4px; }
```

<div class="content-ad"></div>

이 코드의 가장 좋은 점은 매우 ‘테마틱’ 하다는 것입니다. 사용자 정의 클래스를 만드는 대신 기존 SCSS 셀렉터를 대상으로 하는 것이 테마와 잘 맞는다고 생각합니다.

# Angular Material 탭 인디케이터 제거 방법

탭 인디케이터를 원치 않는다면 display: none으로 제거할 수 있습니다. 그러나 mdc-tab-indicator 클래스를 대상으로 하는 것보다 더 구체적인 선택자가 필요합니다. 그것만으로는 충분히 명확하지 않기 때문에, 부모 요소를 대상으로 하는 것이 더 구체적이 될 수 있습니다:

```js
.mdc-tab .mdc-tab-indicator { display: none; }
```

<div class="content-ad"></div>

화면의 표시를 덮어쓰는 매개변수입니다. 표시 flex가 지표에 존재해요.

![스크린샷](/assets/img/2024-07-23-TheUltimateGuidetoAngularMaterialTabandTabIndicatorStyling_6.png)

이제 위의 스크린샷에서 지표가 제거된 것을 볼 수 있습니다.

# 자료들

<div class="content-ad"></div>

Angular Material Snackbar 튜토리얼에서는 적절한 선택기를 찾기 위해 DOM을 깊이 파고들어야 했습니다.

이 튜토리얼의 전체 코드는 다음과 같습니다:

```js
// styled-tabs.ts import { Component, ViewEncapsulation } from "@angular/core"; import { DemoMaterialModule } from "../material-module"; @Component({ selector: "styled-tabs", templateUrl: "styled-tabs.html", styleUrls: ["styled-tabs.scss"], encapsulation: ViewEncapsulation.None, standalone: true, imports: [DemoMaterialModule] }) export class StyledTabs { } // styled-tabs.html <mat-tab-group> <mat-tab label="First"> Content 1 </mat-tab> <mat-tab label="Second"> Content 2 </mat-tab> <mat-tab label="Third"> Content 3 </mat-tab> </mat-tab-group> // styled-tabs.scss .mat-mdc-tab-header { border: 1px solid red; } .mat-mdc-tab.mdc-tab { background-color: grey; &.mdc-tab-indicator--active { background-color: blue; } } .mat-mdc-tab-body.mat-mdc-tab-body-active { min-height: 48px; background-color: coral; } .mat-mdc-tab:not(.mat-mdc-tab-disabled).mdc-tab--active .mdc-tab__text-label { color: orange; } .mdc-tab-indicator__content--underline { --mdc-tab-indicator-active-indicator-color: green; } .mdc-tab-indicator .mdc-tab-indicator__content.mdc-tab-indicator__content--underline { border-top-width: 4px; } .mdc-tab .mdc-tab-indicator { display: none; }
```

# Medium 구독을 고려하십니까?

<div class="content-ad"></div>

만약 이 글이 도움이 되었고 Medium 구독을 원하신다면, 제 추천 링크를 통해 가입해 주시면 감사하겠습니다. 이를 통해 저를 재정적으로 지원하여 더 많은 개발 관련 글을 작성할 수 있게 됩니다.