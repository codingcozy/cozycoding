---
title: "TailwindCSS로 애니메이션 구현하는 방법"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-03 19:30
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Animating with TailwindCSS"
link: "https://dev.to/iamgoncaloalves/animating-with-tailwindcss-2gi9"
isUpdated: true
updatedAt: 1724246467016
---


웹 애플리케이션에서 사용자 경험을 향상시키는 데에는 애니메이션이 중요한 역할을 합니다. TailwindCSS는 애니메이션을 추가하는 과정을 간단하게 만들어 줍니다. 그렇지만 기본 옵션 이상을 원한다면 어떨까요? 이 글에서는 TailwindCSS 애니메이션을 확장하는 방법을 안내해 드리겠습니다. 이를 통해 사용자 정의 및 동적 애니메이션을 만들 수 있게 도와줍니다. 이를 위해 사용자 정의 CSS에만 의존하지 않고 TailwindCSS의 기능을 최대한 활용할 수 있습니다.

## TailwindCSS 애니메이션 이해하기

TailwindCSS는 spin, ping, bounce, pulse 네 가지 주요 애니메이션을 제공합니다. 이 애니메이션들은 구현하기 간답지만 종종 개발자들이 원하는 세부 설정과 제어를 제공하지 못하는 한계가 있습니다. 이러한 기본 옵션들이 편리하긴 하지만, 애플리케이션의 고유 요구 사항에 맞는 더 정교한 애니메이션을 필요로 할 수 있습니다.

### 사용자 정의가 필요한 이유

<div class="content-ad"></div>

많은 경우에 개발자들은 애니메이션을 조정하고 속도를 변경하거나 "wiggle"와 같은 독특한 효과를 적용하고 싶어합니다. 놀라운 소식은 TailwindCSS가 구성 파일을 통해 사용자 정의를 가능하게 하며, 새로운 애니메이션을 추가하고 그 특성을 정의할 수 있게 해준다는 것입니다.

## 확장된 애니메이션 설정

처음 시작하려면 TailwindCSS 구성 파일(tailwind.config.js와 같은 파일)을 찾아야 합니다. 기본 애니메이션을 확장하는 단계별 프로세스는 다음과 같습니다.

### 단계 1: 새로운 애니메이션 추가

<div class="content-ad"></div>

스핀 애니메이션의 느린 버전인 spin-slow을 만들고 싶다고 가정해보겠습니다. 먼저 Tailwind 구성 파일에 액세스해야 합니다:

```js
module.exports = {
  theme: {
    extend: {
      animation: {
        'spin-slow': 'spin 1s linear infinite',
      }
    }
  }
}
```

이 예시에서 기존의 spin 애니메이션을 참조하고 이를 1초로 지속되는 선형 이징을 유지하면서 조절했습니다.

### 단계 2: 고유한 애니메이션 생성

<div class="content-ad"></div>

기존 애니메이션을 수정하는 것 외에도 "wiggle" 효과와 같이 완전히 새로운 것을 만들 수 있습니다. 이를 위해 Tailwind 구성에서 keyframes를 정의할 것입니다:

```js
module.exports = {
  theme: {
    extend: {
      animation: {
        wiggle: 'wiggle 1s ease-in-out infinite',
      },
      keyframes: {
        wiggle: {
          '0%, 100%': { transform: 'rotate(-9deg)' },
          '50%': { transform: 'rotate(9deg)' },
        },
      },
    },
  }
}
```

이 구성은 요소를 왔다갔다 회전시키는 wiggle 애니메이션을 도입합니다.

## 플러그인을 사용하여 애니메이션 향상하기

<div class="content-ad"></div>

기본 TailwindCSS 애니메이션은 유용하지만 모든 요구사항을 충족시키지 못할 수 있습니다. 예를 들어 애니메이션 타이밍 조정, 딜레이, 또는 재생 상태를 제어할 필요가 있을 수 있습니다. 이때 플러그인이 필요합니다.

### TailwindCSS Animate 플러그인 설치하기

더 많은 애니메이션 제어를 위해 TailwindCSS Animate 플러그인을 사용할 수 있습니다. 이 플러그인을 설치하려면 다음 단계를 따르세요:

- npm을 통해 플러그인 설치:

<div class="content-ad"></div>

```js
   npm install tailwindcss-animate
```

- Tailwind 구성에 플러그인을 추가하세요:

```js
   module.exports = {
     plugins: [
       require('tailwindcss-animate'),
     ],
   }
```

이 플러그인은 TailwindCSS의 기능을 확장하여 애니메이션 지속 시간, 지연 및 재생 상태를 쉽게 정의할 수 있습니다.

<div class="content-ad"></div>

- 고급 애니메이션 기능 구현하기

플러그인을 설치하면 HTML 내에서 직접 지연 및 지속 시간과 같은 속성을 정의할 수 있습니다:

```js
<div class="animate-wiggle duration-75 delay-1000"></div>
```

이 예시는 75밀리초의 지속 시간과 1초의 지연을 가진 흔들림 애니메이션을 적용합니다.

<div class="content-ad"></div>

## 애니메이션 상태 관리

애니메이트 플러그인의 가장 강력한 기능 중 하나는 애니메이션이 실행 중인지 일시 중단 중인지를 제어할 수 있다는 것입니다. 클래스를 토글함으로써 사용자 상호 작용에 따라 애니메이션을 일시 중단시킬 수 있어 상호 작용 경험을 향상시킬 수 있습니다.

### 예시: 애니메이션 상태 토글

```js
let isRunning = true;

const toggleAnimation = () => {
  isRunning = !isRunning;
  document.querySelector('.animate-wiggle').classList.toggle('paused', !isRunning);
  document.querySelector('.animate-wiggle').classList.toggle('running', isRunning);
};
```

<div class="content-ad"></div>

이 코드 조각을 사용하면 클릭으로 애니메이션을 일시 중지하거나 재개할 수 있어 동적 사용자 인터페이스 요소를 제공합니다.

## 나와 소통하기

이 글을 좋아하시면 댓글을 남겨주세요. 그럼 제 날이 밝아질 거예요!

더 많은 콘텐츠를 보고 싶으시면 제 개인 블로그를 방문해주세요.

<div class="content-ad"></div>

제게 연락하고 싶으시다면 Twitter/X로 메시지를 보내주세요.

또한 저의 다른 활동은 여기서 확인하실 수 있어요.