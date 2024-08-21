---
title: "CSS Grid 마스터하기 Auto-Fill 대 Auto-Fit 비교"
description: ""
coverImage: "/assets/img/2024-08-21-MasteringCSSGridAuto-FillvsAuto-Fit_0.png"
date: 2024-08-21 16:54
ogImage: 
  url: /assets/img/2024-08-21-MasteringCSSGridAuto-FillvsAuto-Fit_0.png
tag: Tech
originalTitle: "Mastering CSS Grid Auto-Fill vs Auto-Fit"
link: "https://medium.com/@ezeco26/mastering-css-grid-auto-fill-vs-auto-fit-ed5ad94148c1"
isUpdated: false
---


![이미지](/assets/img/2024-08-21-MasteringCSSGridAuto-FillvsAuto-Fit_0.png)

이번 튜토리얼에서는 "auto-fit"과 "auto-fill"의 사용법, 차이점 및 반응형 그리드를 만들 때 언제 사용해야 하는지에 대해 알아볼 것입니다.

## "auto-fill"이란 무엇인가요?

그리드 템플릿 열(grid-template-columns)에 auto-fill을 사용하면 최소값과 최대값에 따라 컨테이너 내에 가능한 한 많은 열을 자동으로 생성합니다. 항목에 필요한 공간보다 많은 공간이 있다면, 비어있는 열을 계속 추가하여 행을 채울 것입니다.

<div class="content-ad"></div>

```js
<div class="grid-container">
  <div class="grid-item"></div>
  <div class="grid-item"></div>
  <div class="grid-item"></div>
</div>
```

```js
.grid-container {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 20px;
}

.grid-item {
  background-color: #b94444;
  padding: 100px;
}
```

<img src="/assets/img/2024-08-21-MasteringCSSGridAuto-FillvsAuto-Fit_1.png" />

<div class="content-ad"></div>

여기서 자동 채우기는 가능한 한 많은 200px 열을 만들고 추가 공간은 빈 열로 출력됩니다(그리드는 공간을 채워야 했기 때문에 빈 열을 생성했습니다).

# "자동 맞춤(fit)"이란 무엇인가요?

자동 맞춤은 자동 채우기와 동일하지만 한 가지 주요한 차이가 있습니다: 빈 열을 축소하여 그리드 항목이 늘어나고 남은 공간을 채우게 합니다. 이는 콘텐츠가 반응형이 되어 다양한 화면 크기에 맞게 적응하고 간격 없이 조정하고 싶은 경우에 자동 맞춤이 이상적입니다.

예시:

<div class="content-ad"></div>

```js
<div class="grid-container">
  <div class="grid-item"></div>
  <div class="grid-item"></div>
  <div class="grid-item"></div>
</div>
```

```js
.grid-container {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 20px;
}

.grid-item {
  background-color: #b94444;
  padding: 100px;
}
```

<img src="/assets/img/2024-08-21-MasteringCSSGridAuto-FillvsAuto-Fit_2.png" />

여기서 auto-fit은 200px의 최소 크기 내에서 가능한 많은 열을 만들지만, 추가 공간이 있으면 기존 열이 확장되어 컨테이너를 채우도록 허용합니다 (우리의 3개 그리드 아이템이 컨테이너에 맞게 늘어졌습니다).

<div class="content-ad"></div>

# "자동 채우기"와 "자동 맞춤"의 사용 사례

사례 1: 반응형 사진 갤러리

반응형 사진 갤러리를 만들고 있다고 가정해보세요. 자동 채우기를 사용하면 이미지의 균일한 크기와 간격을 유지할 수 있습니다. 사진이 늘어나지 않아 균일한 이미지 간격을 유지할 수 있습니다.

사례 2: 동적 콘텐츠 그리드

<div class="content-ad"></div>

대시보드나 제품 페이지를 상상해보세요. 여기서 항목들이 동적으로 변할 수 있습니다. "오토핏"은 "내가 당신이야, 내가 할게"라고 말하는 것이죠. 이는 격자 항목이 항상 배정된 공간을 채우도록 하여 원활한 반응형 레이아웃을 만들어줍니다.

간단히 말해서...

- Auto-fill을 배포하여 격자 내에서 항목 크기와 간격을 일관되게 유지하고 추가 공백을 신경 쓰지 않습니다.
- Auto-fit을 배포하여 항목이 늘어나고 컨테이너를 채우도록 허용하여 더 원활한 가장자리 간 디자인을 가능하게 합니다.

더 많은 자료:

<div class="content-ad"></div>

- CSS Grid에 관한 MDN Web Docs
- W3Schools: CSS Grid 레이아웃
- CSS Tricks: 그리드에 대한 완전한 가이드