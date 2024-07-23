---
title: "2024년에 Styled Components가 구식일 수 있는 이유"
description: ""
coverImage: "/assets/img/2024-07-23-WhyStyledComponentsMightBeObsoletein2024_0.png"
date: 2024-07-23 21:14
ogImage: 
  url: /assets/img/2024-07-23-WhyStyledComponentsMightBeObsoletein2024_0.png
tag: Tech
originalTitle: "Why Styled Components Might Be Obsolete in 2024"
link: "https://medium.com/javascript-in-plain-english/why-styled-components-might-be-obsolete-in-2024-the-rise-of-css-modules-react-styling-a2d7d5f9c5d8"
---


프론트엔드 개발의 끊임없이 발전하는 세상에서, Styled Components와 CSS Modules 사용에 대한 논쟁은 여전히 화두가 되고 있어요.

2024년을 맞아, 이 두 가지 인기 있는 스타일링 방법을 다시 평가하고, 각각의 장단점을 이해하며 어떤 것이 현대 웹 개발에 더 적합한지 파악하는 것이 중요해요.

(StyledComponents가 소개된 이후) 지난 몇 년 간, 트위터에서는 CSS Module을 극복할 수 있는지에 대한 토론이 계속되어 왔어요.

그래서 이 기사에서는 두 도구를 좀 더 현실적으로 살펴봐서 당신의 요구에 맞는 도구를 현명하게 선택할 수 있도록 도와 드릴게요. 😊

<div class="content-ad"></div>

# 기본 개념 이해

## 스타일드 컴포넌트 💅

스타일드 컴포넌트는 React 및 React Native용 라이브러리로, 애플리케이션에서 컴포넌트 수준의 스타일을 사용할 수 있게 해줍니다. 템플릿 리터럴을 활용하여 컴포넌트를 스타일링하며, JavaScript 내에서 스타일을 시원하게 통합할 수 있습니다.

예시:

<div class="content-ad"></div>

아래는 Markdown 형식으로 변환된 것입니다.

![이미지](/assets/img/2024-07-23-WhyStyledComponentsMightBeObsoletein2024_0.png)

## CSS Modules 🐙

CSS Modules은 CSS를 작성하는 인기 있는 방법으로, 기본적으로 클래스 이름을 로컬로 스코프 지정합니다. 이 기술은 스타일이 의도된 컴포넌트에만 적용되도록 보장하여 스타일 충돌 위험을 줄입니다.

예시:

<div class="content-ad"></div>


![Image](/assets/img/2024-07-23-WhyStyledComponentsMightBeObsoletein2024_1.png)

# Bottlenecks and Advantages 🔮

## Styled Components

## Advantages:


<div class="content-ad"></div>

- 동적 스타일링: Styled Components는 동적 스타일링에서 뛰어납니다. 스타일된 컴포넌트에 속성을 전달하여 쉽게 스타일을 변경할 수 있습니다.

![이미지](/assets/img/2024-07-23-WhyStyledComponentsMightBeObsoletein2024_2.png)

- 클래스 이름 충돌 없음: Styled Components는 고유한 클래스 이름을 생성하여 클래스 이름 충돌 가능성을 제거합니다.
- 컴포넌트 기반 스타일링: 컴포넌트 중심 접근 방식을 권장하여 더 잘 구성되고 유지 관리 가능한 코드로 이어질 수 있습니다.

## 병목 현상:

<div class="content-ad"></div>

- 성능 우려사항: 대규모 응용 프로그램에서 여러 스타일 구성 요소가있을 때 런타임 성능에 대한 우려가 있습니다.
- 도구 및 디버깅: 스타일 구성 요소를 디버깅하는 것은 때로는 전통적인 CSS와 비교하여 더 어려울 수 있습니다. 스타일은 JavaScript 내에 포함되어 있기 때문입니다.

# CSS Modules 🎈

## 장점:

- 지역 스타일: CSS Modules를 사용하면 스타일이 지역적으로 적용되어 의도하지 않은 스타일 충돌의 위험을 줄일 수 있습니다.
- 간편함 및 익숙함: 전통적인 CSS를 선호하는 개발자들에게 CSS Modules는 익순하고 직관적인 방식을 제공합니다.
- 빌드 성능: CSS Modules는 일반적으로 빌드 성능이 우수합니다. 런타임이 아니라 빌드 시간 동안 처리되기 때문입니다.

<div class="content-ad"></div>

## 병목 현상:

- 전역 스타일 관리: CSS 모듈을 사용하면 전역 스타일과 테마를 관리하기가 더 번거로울 수 있으며 추가적인 설정이 필요할 수 있습니다.
- 동적 스타일링 제한: CSS 모듈은 Styled Components와 비교할 때 동적 스타일링에 대해 유연성이 떨어질 수 있습니다.

# 현대 웹 개발에는 무엇이 더 나은가요?

이 질문에 대한 답변은 일관되지 않습니다. 프로젝트의 구체적인 요구 사항과 맥락에 크게 의존합니다. 다양한 시나리오를 기반으로 다음과 같이 구분해 볼 수 있습니다:

<div class="content-ad"></div>

## 스타일드 컴포넌트를 사용하는 경우:

- 동적 스타일링이 필요할 때: 프롭이나 테마에 기반한 다양한 동적 스타일링이 필요한 경우, 스타일드 컴포넌트가 더 나은 선택일 수 있습니다.
- 컴포넌트 중심적인 접근 방식을 선호할 때: 스타일드 컴포넌트는 스타일을 컴포넌트와 직접 통합하여 모듈식이고 유지보수가능한 코드베이스로 이어질 수 있습니다.
- 새 프로젝트에 작업 중일 때: 새 프로젝트를 시작하고 리액트와 현대적인 접근 방식을 채택하고 싶다면, 스타일드 컴포넌트가 훌륭한 선택일 수 있습니다.

## CSS 모듈을 사용하는 경우:

- 전통적인 CSS를 선호하는 경우: 전통적인 CSS에 더 익숙한 경우, CSS 모듈은 가파른 학습 곡선 없이 스코프 스타일을 추가하는 간단한 방법을 제공합니다.
- 성능이 중요한 경우: 성능이 핵심 요소인 대규모 애플리케이션에서는 CSS 모듈이 더 나은 빌드 및 런타임 성능을 제공할 수 있습니다.
- 기존 CSS와 통합해야 하는 경우: 기존의 전통적인 CSS를 사용하는 기존 코드베이스와 통합해야 하는 경우, CSS 모듈을 사용하면 스코프 스타일을 쉽게 추가할 수 있습니다.

<div class="content-ad"></div>

# 산업 채택 👻

이러한 접근 방식을 사용하는 유명 회사들을 이해하면 그 효과와 인기에 대한 추가 통찰력을 제공할 수 있습니다.

## Styled Components를 사용하는 회사들

- Twitter: Twitter은 동적 테마 처리와 모듈식 컴포넌트 기반 아키텍처를 다룰 수 있는 능력 때문에 Styled Components를 채택했습니다.
- Airbnb: Airbnb는 React와의 유연성 및 통합을 위해 프론트엔드 개발에서 Styled Components를 널리 사용하고 있습니다.
- Gatsby: 인기 있는 정적 사이트 생성기인 Gatsby는 React 컴포넌트를 스타일링하기 위해 Styled Components를 권장하고 활용하고 있습니다.

<div class="content-ad"></div>

## CSS Modules를 사용하는 기업

- Facebook: Facebook은 범위 지정 스타일을 관리하고 전역 CSS 충돌을 피하기 위해 다양한 프로젝트에 CSS Modules를 통합했습니다.
- GitHub: GitHub은 플랫폼 전반에 걸쳐 일관되고 충돌이 없는 스타일링 시스템을 유지하기 위해 CSS Modules를 사용합니다.
- Uber: Uber는 웹 애플리케이션에서 스타일 캡슐화 및 성능을 보장하기 위해 CSS Modules를 활용하고 있습니다.

# 결론

2024년 현재 Styled Components와 CSS Modules은 React 애플리케이션에 스타일을 적용하는 강력한 도구로 남아 있습니다. Styled Components는 현대적이고 동적인 스타일링 방식을 제공하는 반면, CSS Modules은 익숙하고 성능 친화적인 솔루션을 제공합니다.

<div class="content-ad"></div>

각 접근 방식의 강점과 한계를 이해하고 산업 동향을 고려하여, 귀하의 개발 요구에 최적인 결정을 내릴 수 있습니다.

즐거운 코딩하세요! 🍺

# 평문으로 간단히 🚀

In Plain English 커뮤니티의 일원이 되어 주셔서 감사합니다! 계속 하시기 전에:

<div class="content-ad"></div>

- 작가에게 박수를 보내고 팔로우해주세요 👏️️
- 팔로우하기: X | LinkedIn | YouTube | Discord | 뉴스레터
- 다른 플랫폼 방문하기: CoFeed | Differ
- 더 많은 컨텐츠는 PlainEnglish.io에서 확인하세요