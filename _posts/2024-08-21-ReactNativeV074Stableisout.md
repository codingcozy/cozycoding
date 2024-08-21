---
title: "React Native V0.74 에 추가된 기능 정리"
description: ""
coverImage: "/assets/img/2024-08-21-ReactNativeV074Stableisout_0.png"
date: 2024-08-21 18:08
ogImage: 
  url: /assets/img/2024-08-21-ReactNativeV074Stableisout_0.png
tag: Tech
originalTitle: "React Native V0.74  Stable is out "
link: "https://medium.com/@anisurrahmanbup/react-native-v0-74-stable-is-out-8943ea367217"
isUpdated: true
updatedAt: 1724245499855
---


헬로 React Native 개발자 여러분,

최근에 React Native 세계에서 흥미로운 소식이 있어서 알려드립니다. V0.74 버전이 릴리스되었는데, 1600개 이상의 커밋이 있었습니다. 여기에는 다음과 같은 하이라이트가 있습니다:

- Yoga 3.0
- 새로운 아키텍처: 기본적으로 브리지 없음
- 새로운 아키텍처: 일괄적인 onLayout 업데이트
- 새 프로젝트를 위한 Yarn 3

각 하이라이트에 대해 자세히 살펴보겠습니다.

<div class="content-ad"></div>

<img src="/assets/img/2024-08-21-ReactNativeV074Stableisout_0.png" />

# Yoga 3.0

먼저 React Native에서 요가가 무엇인지 이해해 봅시다.

## Yoga — 레이아웃 엔진

<div class="content-ad"></div>

요가는 Meta에서 개발한 오픈 소스 레이아웃 엔진입니다. UI 요소(버튼, 텍스트, 이미지 등)가 사용자 인터페이스 내에서 어떻게 배치되고 위치하는지를 나타내는 엔진입니다.

요가는 각 UI 요소에 대해 다음 네 가지를 계산합니다.

- 위치
- 크기
- 정렬
- 간격

요가를 사용하면 다양한 화면 크기와 방향에 맞춰 반응형 레이아웃을 만들 수 있습니다. 또한 React Native에서 CSS Flexbox라는 널리 사용되는 개념을 구현합니다. 따라서 이미 요가가 React Native 유연한 UI의 핵심(♥︎)임을 느끼실 수 있습니다.

<div class="content-ad"></div>

## Yoga 3.0 — 무슨 변화가 있을까요?

지난 React Native 버전들의 모두에 걸쳐 몇 가지 올바르지 않은 레이아웃 동작이 있었습니다. Yoga 3는 그것들을 모두 해결했습니다. 가장 흔한 문제 중 하나는 'row-reverse' 스타일이 제대로 작동하지 않았던 것입니다.

아래 이미지를 살펴봅시다. 왼쪽은 V0.73에서, 오른쪽은 V0.74에서 가져온 것입니다.

![React Native](/assets/img/2024-08-21-ReactNativeV074Stableisout_1.png)

<div class="content-ad"></div>

위 이미지에서 `Container/`가 있고 그 안에 `Parent/` 컴포넌트가 있으며 두 개의 `Child/` 컴포넌트가 내부에 있습니다.

그런 다음에 `Parent/` 컴포넌트에 이 스타일을 적용했습니다.

```js
// <Parent/> 컴포넌트를 위한 스타일
style={
      flexDirection: 'row-reverse',
      backgroundColor: 'dodgerblue',
      flex: 1,
      marginLeft: 100,
      marginRight: 20,
      marginVertical: 20,
      alignItems: 'center'
}
```

`Parent/`에 100픽셀의 marginLeft을 추가한 것을 알았나요? 네, 하지만 React Native V0.73에서의 출력물(왼쪽)을 보십시오. 이미지의 왼쪽에는 100픽셀의 왼쪽 마진이 아니라 우측에 마진이 생깁니다!! 이해되시죠? 이제 React Native V0.74의 출력물(오른쪽)을 살펴보겠습니다. 훌륭합니다, V0.74에서는 왼쪽에 완벽한 100픽셀 마진이 보이며 두 개의 `Child/` 컴포넌트가 반전되었습니다 🚀

<div class="content-ad"></div>

요가 2에서는 'row-reverse' flex-direction을 적용하면 컴포넌트 내부에 "margin"이나 "padding" 또는 "border"가 있는 경우 해당 컴포넌트의 가장자리도 뒤집힌다는 점이 있었어요. 그러나 Yoga-3에서는 이 문제가 완벽하게 해결되었어요 💯

Yoga-3에서는 Yoga-2에 누락된 몇 가지 중요한 스타일링 구성 요소가 추가되었어요.

- alignContent 스타일을 위한 `space-evenly` 속성
- position 스타일을 위한 `static` 속성

# 새 아키텍처: 기본적으로 브릿지 없음

<div class="content-ad"></div>

## 이전 아키텍처

React Native은 이전에 JavaScript와 네이티브 모듈 간에 통신하기 위해 브릿지를 사용했습니다. 모듈은 C++, Objective C, Java 또는 Kotlin으로 작성되어 카메라, 센서 등과 같은 네이티브 기능에 액세스합니다. 불행히도, 브릿지에는 일부 제한 사항이 있습니다.

주요 제한 사항 중 하나는 한 계층이 다른 계층과 통신할 때마다 데이터를 직렬화(JS 객체를 JSON 문자열로 변환)하고 역직렬화(JSON 문자열을 다시 JS 객체로 변환)해야 한다는 점입니다. 변환에 시간이 소요되므로 이 프로세스는 통신 흐름에 성능 문제를 추가합니다.

## 새 아키텍처 — 성능 향상기

<div class="content-ad"></div>

좋은 소식은 React Native 팀이 브릿지를 JSI (JavaScript Interface)라는 인터페이스로 교체할 수 있었다는 것입니다. 이는 C++로 작성되었으며 JS 코드에서 사용할 수 있는 모든 네이티브 기능을 제공하며, 데이터 직렬화 또는 역직렬화 없이 네이티브 메서드를 호출할 수 있어 앱이 빠릅니다.

## 이전 아키텍처 종속성 제거

JSI는 새 아키텍처의 핵심 부분입니다. 앱에서 JSI (새 아키텍처)를 최대한 활용하려면 먼저 기존 브릿지 아키텍처에 대한 앱 종속성을 제거해야 합니다. 이를 위해 React Native 팀이 새 아키텍처의 세 가지 기둥을 소개했는데, 이를 통해 브릿지에 대한 완전한 종속성 제거가 가능합니다.

- TurboModules: 브릿지로부터 네이티브 호출에 대한 앱 종속성 제거 (V0.68에서 출시)
- Fabric Renderer: 브릿지로부터 컴포넌트 렌더링에 대한 앱 종속성 제거 (V0.68에서 출시)
- Bridgeless Mode: 브릿지로부터 그 외 모든 것에 대한 앱 종속성 제거 (즉, React Native 런타임의 나머지 부분: 오류 처리, 전역 이벤트 핸들러, 타이머 등) (V0.73에서 출시)

<div class="content-ad"></div>

## V0.74에서 새로운 변화는 무엇인가요?

V0.74부터, 새 아키텍처를 활성화하면 'Bridgeless Mode'가 자동으로 활성화되는 것을 알 수 있습니다. 그러나 새 아키텍처 자체는 여전히 기본적으로 활성화되지 않았습니다.

V0.74에서 React Native 앱에서 새 아키텍처를 활성화하면 Metro Log에서 아래와 같이 두 줄을 볼 수 있습니다:

![ReactNativeV074Stableisout_2](/assets/img/2024-08-21-ReactNativeV074Stableisout_2.png)

<div class="content-ad"></div>

그게 다에요 🚀. React Native V0.74부터는 새 아키텍처를 활성화한 후에 Bridgeless 모드를 수동으로 활성화할 필요가 없어졌어요 💯

# 새 아키텍처: 일괄 처리된 onLayout 업데이트

또 다른 훌륭한 소식은 React Native 팀이 새 아키텍처 Bridgeless 모드를 기본으로 만들 뿐만 아니라 이 아키텍처를 개선하여 일괄 처리된 onLayout 업데이트(단일 렌더링에서 여러 업데이트 실행)를 처리할 수 있게 했다는 것이에요. 이 최적화는 렌더링 중에 레이아웃 관련 계산을 최소화하여 성능을 향상시킵니다.

## "onLayout" 속성

<div class="content-ad"></div>

리액트 네이티브에서 onLayout 속성은 컴포넌트의 레이아웃(위치) 변경을 처리하는 데 사용됩니다. 컴포넌트의 레이아웃이 변경될 때(마운팅, 크기 조정, 회전 또는 기타 요소로 인해) onLayout 콜백 함수가 트리거됩니다.

다음과 같이 이 속성을 사용하여 업데이트된 레이아웃 정보를 기반으로 작업을 수행할 수 있습니다.

```js
function App(){
  return (
      <View
         onLayout={() => {
          console.log('Component has been invoked 🚀')
         }
      />
   )
}
```

## "onLayout" 배치 업데이트는 어떻게 작동합니까?

<div class="content-ad"></div>

`App/` 컴포넌트가 아래와 같이 표시됩니다. 각 View는 mount되었을 때 onLayout 콜백 함수를 트리거합니다.

```js
function App() {
 const [state1, setState1] = useState(false)
 const [state2, setState2] = useState(false)
 const [state3, setState3] = useState(false)

 console.log('✅ COMPONENT RE-RENDERED .....')

 return (
  <View>
   <View
    onLayout={() => {
     console.log('FIRST View invoked')
     setState1(true) // View가 mount될 때 state1을 업데이트합니다
    }}></View>

   <View
    onLayout={() => {
     console.log('SECOND View invoked')
     setState2(true) // View가 mount될 때 state2를 업데이트합니다
    }}></View>

   <View
    onLayout={() => {
     console.log('THIRD View invoked')
     setState3(true) // View가 mount될 때 state3을 업데이트합니다
    }}></View>
  </View>
 )
}
```

지금, React Native V0.73에서는 아래와 같은 결과를 볼 수 있습니다 👇

<img src="/assets/img/2024-08-21-ReactNativeV074Stableisout_3.png" />

<div class="content-ad"></div>

"onLayout" 콜백을 실행할 때마다 전체 컴포넌트가 다시 렌더링된다는 걸 알아 보셨나요? 응, 예상치 못했죠.

이제 React Native V0.74에서 새 아키텍처를 활성화한 출력을 확인해 볼까요 👇

![React Native V0.74 with New Architecture](/assets/img/2024-08-21-ReactNativeV074Stableisout_4.png)

대단한 성능 🔥. 컴포넌트가 "onLayout" 콜백 실행에 대해 한 번만 다시 렌더링되었어요.

<div class="content-ad"></div>

아래 이미지에 있는 "onLayout" 배치 업데이트에 대한 좋은 요약 👇 

![이미지](/assets/img/2024-08-21-ReactNativeV074Stableisout_5.png) 

# 새로운 프로젝트용 Yarn 3 

Yarn 3는 React Native Community CLI로 초기화된 새 프로젝트의 기본 JavaScript 패키지 매니저입니다. 이는 이전에 기본으로 사용되던 Yarn Classic (1.x)을 대체합니다.

<div class="content-ad"></div>

Markdown 형식으로 변경해 주세요:


Yarn 3은 종속성을 설치하고 업데이트하는 프로세스를 가속화하며 종속성이 저장되는 방식을 최적화합니다.

# 이게 다에요 🙌

React Native 0.74 버전은 구성 요소 레이아웃 및 아키텍처에 중요한 개선 사항을 도입했으며, batched onLayout 업데이트 및 Yarn 3 통합을 포함하고 있습니다.

# 모든 최신 React Native 뉴스 🚀


<div class="content-ad"></div>

내 트위터 계정에서는 최신 React Native 뉴스에 대해 모두 다루고 있어요. 거기서 나를 팔로우하고 이 빠르게 발전하는 분야에 대해 최신 소식을 받아보세요. (내 트위터 계정: Anis).

또한 React Native의 최신 SDK 출시에 대한 내 깊은 R&D를 공개하고 있어요. (링크: https://github.com/anisurrahman072/React-Native-SDK-Research)