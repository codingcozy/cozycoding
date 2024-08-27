---
title: "유틸리티-퍼스트 CSS 프레임워크, Tailwind CSS 사용 방법 가이드"
description: ""
coverImage: "/assets/img/2024-08-26-ACompleteGuidetoTailwindCSSTheUtility-FirstCSSFrameworkThatsChangingWebDesign_0.png"
date: 2024-08-26 17:18
ogImage: 
  url: /assets/img/2024-08-26-ACompleteGuidetoTailwindCSSTheUtility-FirstCSSFrameworkThatsChangingWebDesign_0.png
tag: Tech
originalTitle: "A Complete Guide to Tailwind CSS The Utility-First CSS Framework Thats Changing Web Design"
link: "https://medium.com/@vansh.khandelwal06/a-complete-guide-to-tailwind-css-the-utility-first-css-framework-thats-changing-web-design-0189d58940a1"
isUpdated: true
updatedAt: 1724744081729
---


현대 웹 개발에서는 CSS 프레임워크가 프로젝트 간 일관성을 유지하고 속도를 높이는 데 중요한 역할을 합니다. 수많은 프레임워크 중에서 Tailwind CSS는 독특한 유틸리티 중심 접근 방식으로 높은 유연성, 효율성 및 디자인 일관성을 제공하여 주목받고 있어요. 부트스트랩이나 파운데이션과 같은 전통적인 프레임워크와 달리, Tailwind는 사전 구축된 구성 요소에 의존하지 않습니다. 대신, 개발자가 쉽게 완전히 사용자 정의된 디자인을 생성할 수 있도록 하는 저수준 유틸리티 클래스를 제공합니다.

# Tailwind CSS란?

Tailwind CSS는 포괄적인 사전 정의된 클래스 세트를 제공하는 유틸리티 중심 CSS 프레임워크입니다. 이러한 클래스들은 레이아웃, 간격, 타이포그래피, 색상, 테두리, 그림자 등 스타일링의 거의 모든 측면을 다룹니다. 개발자들은 커스텀 CSS를 작성하는 대신, 이 유틸리티 클래스들을 HTML에 직접 적용하여 원하는 스타일을 달성할 수 있어요.

예를 들면:

<div class="content-ad"></div>

```js
<!-- 전통적인 CSS 방식 -->

<div class="card">
  <h2 class="title">안녕, 세상아!</h2>
  <p class="content">이것은 샘플 텍스트입니다.</p>
</div>
```

```js
/* 사용자 정의 CSS */

.card {
  padding: 1rem;
  background-color: #f9fafb;
  border-radius: 0.5rem;
}

.title {
  font-size: 1.25rem;
  font-weight: bold;
  margin-bottom: 0.5rem;
}

.content {
  font-size: 1rem;
  color: #6b7280;
}
```

Tailwind CSS를 사용하면 유틸리티 클래스를 직접 사용하여 동일한 디자인을 구현할 수 있습니다:

```js
<!-- Tailwind CSS 방식 -->

<div class="p-4 bg-gray-100 rounded-lg">
  <h2 class="text-xl font-bold mb-2">안녕, 세상아!</h2>
  <p class="text-gray-600">이것은 샘플 텍스트입니다.</p>
</div>
```

<div class="content-ad"></div>

테일윈드의 유틸리티 클래스는 사용자 정의 CSS를 작성할 필요없이 컴포넌트를 스타일링할 수 있게 해줍니다. 모든 것이 HTML 내부에서 인라인으로 처리되기 때문에 개발 속도가 더 빨라지고 직관적으로 작업할 수 있습니다.

# 테일윈드 CSS를 선택해야 하는 이유

테일윈드 CSS의 장점은 다음과 같습니다:

- 신속한 개발 속도: HTML에서 직접 스타일을 적용할 수 있기 때문에 HTML과 CSS 파일을 번갈아가며 작업할 필요가 없습니다. 이로 인해 개발 시간이 단축되고 유지보수가 간단해집니다.
- 디자인 일관성: 유틸리티 클래스는 일관된 디자인 패턴을 강제합니다. 예를 들어 `p-4`는 패딩이 컴포넌트 전체에서 일관되게 적용되도록 합니다.
- 사용자 정의: 테일윈드는 매우 사용자 정의가 가능합니다. 설정 파일을 통해 브레이크포인트, 색상, 간격 스케일 등을 조정하여 디자인 요구 사항에 맞추 수 있습니다.
- 반응형 디자인: 테일윈드의 반응형 유틸리티를 사용하면 모바일 중심 디자인을 쉽게 구축할 수 있습니다. `md:bg-blue-500` 또는 `lg:text-2xl`와 같이 브레이크포인트별 클래스를 사용하여 반응형 레이아웃을 손쉽게 생성할 수 있습니다.
- 작은 파일 크기를 위한 PurgeCSS: 테일윈드 CSS는 제품용으로 사용되지 않는 CSS를 자동으로 정리하여 CSS 파일 크기를 최소화하고 더 빠른 로드 시간을 제공합니다.

<div class="content-ad"></div>

# Tailwind CSS 설정하기

프로젝트에서 Tailwind CSS를 시작해봅시다. Tailwind CSS를 통합하는 여러 가지 방법이 있습니다. CDN, npm 또는 Next.js 또는 Laravel과 같은 프레임워크를 사용하는 방법 등이 있습니다. 여기서는 유연성을 높이기 위해 npm을 사용하겠습니다.

## npm을 통해 Tailwind CSS 설치하기:

먼저, Tailwind를 설치하고 구성 파일을 초기화하세요.

<div class="content-ad"></div>

```js
npm install -D tailwindcss
npx tailwindcss init
```

이렇게하면 Tailwind 설정을 사용자 정의할 수있는 `tailwind.config.js` 파일이 생성됩니다.

## 프로젝트 구성하기:

`src/styles.css` (또는 기타 CSS 파일)에서 다음 Tailwind 지시문을 추가하십시오:

<div class="content-ad"></div>

```js
@tailwind base;
@tailwind components;
@tailwind utilities;
```

이러한 지시문은 Tailwind의 미리 만들어진 스타일과 유틸리티 클래스를 가져옵니다.

## Tailwind CSS 빌드하기:

Tailwind CLI를 사용하여 CSS를 컴파일하세요:

<div class="content-ad"></div>

```js
npx tailwindcss -i ./src/input.css -o ./dist/output.css --watch
```

Tailwind는 파일을 모니터링하고 변경 사항이 있을 때 자동으로 CSS를 컴파일합니다.

## PurgeCSS 설정하기 (선택 사항이지만 권장됨):

최종 빌드에 실제로 사용하는 CSS만 포함되도록 하려면 Tailwind의 구성이 기본적으로 PurgeCSS를 지원합니다.

<div class="content-ad"></div>

```js
// tailwind.config.js
module.exports = {
    purge: ['./src/**/*.html', './src/**/*.js'], // 필요에 따라 경로를 조정하세요
    theme: {
        extend: {},
    },
    plugins: [],
}
```

# Tailwind의 유틸리티 클래스 심층 분석

Tailwind CSS는 작고 단일 목적의 클래스를 결합하여 복잡한 인터페이스를 구축하는 것에 관한 것입니다. 몇 가지 일반적인 유틸리티 카테고리를 예제와 함께 탐색해 봅시다.

## 1. 간격 (패딩 & 마진)

<div class="content-ad"></div>

Tailwind는 패딩과 마진을 제어하기 위한 유틸리티 클래스를 제공합니다. 이러한 클래스들은 스케일을 따르며(e.g., `p-4`, `mt-2`), 각 숫자가 `rem`에서 특정 값에 해당합니다:

```js
<div class="p-4 mb-6">
  <p class="mt-2 text-gray-700">
    이 요소에는 패딩과 마진이 적용되어 있습니다.
  </p>
</div>
```

- `p-4`: 1rem(16px)의 패딩을 추가합니다.

- `mt-2`: 상단 여백 0.5rem(8px)을 추가합니다.

<div class="content-ad"></div>

- `mb-6`: 하단 여백을 1.5rem(24px) 추가합니다.

## 2. 색상 및 배경

테일윈드는 텍스트, 배경, 테두리 및 기타 요소에 사용할 수 있는 다양한 색상 팔레트를 제공합니다:

```js
<div class="bg-blue-500 text-white p-4 rounded-lg">
  <h2 class="text-xl">테일윈드 CSS에 오신 것을 환영합니다!</h2>
</div>
```

<div class="content-ad"></div>

- `bg-blue-500`: 중간 파란색 배경색을 적용합니다.

- `text-white`: 텍스트 색상을 흰색으로 설정합니다.

- `rounded-lg`: 모퉁이를 둥글게 하는 큰 테두리 반지름을 추가합니다.

## 3. 텍스트 디자인

<div class="content-ad"></div>

테일윈드의 타이포그래피 유틸리티를 사용하면 글꼴 크기, 굵기, 줄 간격, 글자 간격 등을 제어할 수 있어요:

```js
<p class="text-lg font-semibold leading-relaxed tracking-wide text-gray-800">
  테일윈드를 사용하면 타이포그래피에 미세한 제어를 할 수 있어요.
</p>
```

- `text-lg`: 글꼴 크기를 1.125rem으로 설정합니다.

- `font-semibold`: 중간 굵기의 글꼴을 적용합니다.

<div class="content-ad"></div>

- `leading-relaxed`: 가독성을 높이기 위해 줄 간겹을 조정합니다.

- `tracking-wide`: 글자 간겹을 증가시킵니다.

## 4. Flexbox와 Grid

Tailwind는 Flexbox 및 Grid 유틸리티를 활용하여 반응형 레이아웃을 구축하는 데 뛰어납니다.

<div class="content-ad"></div>

```js
<div class="flex justify-between items-center">
  <div class="p-4 bg-green-200">Item 1</div>
  <div class="p-4 bg-green-300">Item 2</div>
  <div class="p-4 bg-green-400">Item 3</div>
</div>
```

- `flex`: Flexbox를 활성화합니다.

- `justify-between`: 자식 요소 사이에 공간을 분배합니다.

- `items-center`: 항목을 세로로 중앙에 정렬합니다.

<div class="content-ad"></div>

## 5. 반응형 디자인

**Tailwind**의 반응형 유틸리티를 사용하면 다양한 뷰포인트에서 스타일을 적용할 수 있어요:

```js
<div class="text-sm md:text-lg lg:text-2xl">
  화면 크기에 따라 텍스트 크기가 조절됩니다.
</div>
```

- `text-sm`: 모바일 화면에 작은 텍스트를 적용합니다.

<div class="content-ad"></div>

- `md:text-lg`: `md` (768px) 화면에서 텍스트 크기를 키웁니다.

- `lg:text-2xl`: `lg` (1024px) 화면에서 큰 텍스트 크기를 설정합니다.

# 고급 Tailwind 기능

## 1. 설정 파일을 사용하여 Tailwind 사용자 정의하기

<div class="content-ad"></div>

Tailwind의 구성 파일 (`tailwind.config.js`)은 색상부터 간격까지 모든 것을 사용자 정의할 수 있게 해줍니다:

```js
module.exports = {
  theme: {
    extend: {
      colors: {
        customBlue: '1E40AF',
      },
      spacing: {
        '72': '18rem',
      },
    },
  },
}
```

이 예제에서는 `customBlue`라는 사용자 정의 색상을 추가하고 간격 스케일에 새 값을 추가했습니다.

## 2. 다크 모드 지원

<div class="content-ad"></div>

테일윈드는 최소한의 구성으로 다크 모드를 쉽게 구현할 수 있도록 해줍니다:

```js
module.exports = {
  darkMode: 'class', // 혹은 'media'
  theme: {
    extend: {},
  },
}
```

HTML에서는 `dark` 클래스를 추가하여 다크 모드를 토글할 수 있습니다:

```js
<div class="bg-white dark:bg-gray-800">
  <p class="text-black dark:text-white">이 텍스트는 다크 모드를 지원합니다.</p>
</div>
```

<div class="content-ad"></div>

## 3. 테일윈드 플러그인

테일윈드의 플러그인 시스템은 기능을 확장시킵니다. 예를 들어, `@tailwindcss/typography` 플러그인을 사용하여 내용이 많은 사이트에서 더 나은 서체 스타일링을 할 수 있습니다:

```js
npm install @tailwindcss/typography
```

구성에 추가해 보세요:

<div class="content-ad"></div>

```js
module.exports = {
  plugins: [
    require ('@tailwindcss/typography'),
  ],
}
```

이제 HTML 콘텐츠에 `prose` 클래스를 적용하여 아름답게 스타일이 적용된 텍스트를 만들어보세요:

```js
<article class="prose">
  <h1>Tailwind CSS로 에세이 작성이 쉬워집니다</h1>
  <p>
      Tailwind의 typography 플러그인은 콘텐츠를 스타일링하는 아름다운 기본 설정을 제공합니다.
  </p>
</article>
```

# Tailwind CSS에 대한 흔한 오해들

<div class="content-ad"></div>

1. "그냥 스타일 속성들(inline styles)의 강화된 버전이에요": Tailwind의 유틸리티 클래스가 인라인 스타일과 비슷하게 보일 수 있지만, 더 강력해요. Tailwind 유틸리티는 반응형이고 사용자 정의가 가능하며 재사용이 가능하므로 훨씬 더 깔끔하고 유지보수가 용이한 코드를 작성할 수 있어요.

2. "Tailwind가 HTML을 너무 많은 클래스로 팽창시킨다": Tailwind는 HTML에 더 많은 클래스를 추가해야하지만, 이는 더 나은 확장성, 일관성, 그리고 디자인 제어를 제공해요. 게다가 CSS에서 `@apply`와 같은 도구를 사용하거나 JavaScript 프레임워크(예: React)에서 재사용 가능한 구성 요소를 추출하여 반복을 줄일 수 있어요.

3. "고유한 디자인 만들기가 어렵다": Tailwind는 매우 사용자 정의가 가능해요. 기본 스타일에 국한되지 않고, 자체 색상 팔레트, 간격 스케일 등을 정의할 수 있어 완전히 고유한 디자인을 만들 수 있어요.

# 실제 예시와 사용 사례

<div class="content-ad"></div>

대부분의 기업과 프로젝트들이 스타트업부터 대기업까지 Tailwind CSS를 채택해왔어요. Tailwind는 특히 다음과 같은 상황에서 유용해요:

- 빠른 프로토타이핑: Tailwind는 프로토타입을 빠르게 구축하고 이를 손쉽게 제품 준비가 된 디자인으로 변환할 수 있어요.

- 디자인 시스템: Tailwind의 일관성과 모듈성은 확장 가능한 디자인 시스템을 구축하는 데 좋은 선택이에요.

- 싱글 페이지 애플리케이션(SPA): React, Vue, Svelte와 같은 프레임워크로, Tailwind의 유틸리티 중심의 접근 방식은 컴포넌트 스타일링에 완벽하게 맞아요.

<div class="content-ad"></div>

# 결론

Tailwind CSS는 단순히 또 다른 CSS 프레임워크가 아닙니다. 현대적인 웹 인터페이스를 구축하는 새로운 방법을 제공하는 강력한 도구입니다. 유틸리티 중심의 마음가짐을 채택함으로써 개발 워크플로우를 최적화하고, 컨텍스트 전환을 줄이며, 프로젝트 전체에서 디자인 일관성을 유지할 수 있습니다. 첫 프로젝트를 구축하려는 초심자든, 효율적인 디자인 방법을 찾는 경험 많은 개발자든, Tailwind CSS는 한 번 시도할 가치가 있습니다.

## 출처:

- https://tailwindcss.com/
- https://www.geeksforgeeks.org/introduction-to-tailwind-css/
- https://github.com/tailwindlabs/tailwindcss
- https://www.freecodecamp.org/news/what-is-tailwind-css-a-beginners-guide/

<div class="content-ad"></div>


![A Complete Guide to Tailwind CSS](/assets/img/2024-08-26-ACompleteGuidetoTailwindCSSTheUtility-FirstCSSFrameworkThatsChangingWebDesign_0.png)
