---
title: "WASM Flutter Web 정리"
description: ""
coverImage: "/assets/img/2024-06-19-WASMFlutterWebLetsunderstandit_0.png"
date: 2024-06-19 14:21
ogImage: 
  url: /assets/img/2024-06-19-WASMFlutterWebLetsunderstandit_0.png
tag: Tech
originalTitle: "WASM + Flutter Web —Let’s understand it!"
link: "https://medium.com/@abhishekdoshi26/wasm-650ca52f04df"
isUpdated: true
---





## 지식을 향상시키고 플러터 웹 앱에서 WASM을 사용하는 시간입니다!

![이미지](/assets/img/2024-06-19-WASMFlutterWebLetsunderstandit_0.png)

## WebAssembly(WASM)가 무엇이며 어떻게 작동합니까?

WebAssembly(약어: Wasm)은 스택 기반 가상 머신을 위한 이진 명령 형식입니다. Wasm은 프로그래밍 언어에 대한 휴대용 컴파일 타겟으로 설계되어 웹 상의 클라이언트 및 서버 애플리케이션에 배포가 가능합니다.

<div class="content-ad"></div>

간단히 말해, 웹어셈블리(WASM)는 웹사이트가 컴퓨터에 설치된 프로그램과 거의 동일한 속도로 고성능 코드를 실행할 수 있게 해주는 기술입니다. 이 기술은 다른 언어로 작성된 코드를 사용하여 웹 애플리케이션이 더 효율적으로 작동할 수 있도록 돕습니다. 이 코드는 웹 브라우저가 이해하고 빠르게 실행할 수 있는 특별한 형식으로 변환됩니다. 이를 통해 웹 앱은 게임, 비디오 편집 및 데이터 처리와 같은 복잡한 작업에 특히 빠르고 강력해집니다.

![웹어셈블리 이미지](/assets/img/2024-06-19-WASMFlutterWebLetsunderstandit_1.png)

## Flutter + WASM

명확히 말하자면, WASM은 HTML 및 CanvasKit과 같이 Flutter를 위한 렌더링 엔진이 아닙니다. 대신, WASM은 웹 환경에서 효율적으로 실행되어야 하는 코드의 컴파일 대상입니다.

<div class="content-ad"></div>

## Flutter Web에서 WebAssembly(WASM)의 역할

우리가 이야기했던 대로, WASM은 Flutter Web의 컴파일 대상입니다. 이것은 웹 브라우저에서 거의 네이티브 속도로 실행되도록 해주는 이진 명령 형식입니다. 주로 성능이 중요한 웹 앱을 컴파일하여 브라우저에서 더 빠르게 실행할 수 있도록 사용됩니다!
WASM은 렌더러 자체가 아니기 때문에, 웹 앱은 기본 웹 렌더러를 사용하거나 지정한 렌더러를 사용할 것입니다.

PS: 이제 Flutter 팀은 --web-renderer=auto를 canvaskit으로 변경했습니다. 이는 웹 렌더러를 지정하지 않으면 자동으로 canvaskit을 사용한다는 것을 의미합니다. 이전에는 모바일 브라우저에서 앱을 실행할 때 HTML 렌더러를 사용하고 데스크톱 브라우저에서 앱을 실행할 때 CanvasKit 렌더러를 사용했습니다. (이슈 링크)

## Flutter와 함께 WASM 사용하는 방법?

<div class="content-ad"></div>

플러터에서 WASM을 사용하기 전에 2024년 6월 14일 현재 많은 패키지가 WASM을 지원하지 않는다는 점을 유념해야 합니다. 패키지가 웹 기반 의존성/패키지/라이브러리를 사용하는 경우(dart:html, dart:js 등), 해당 앱은 WASM에서 컴파일할 수 없습니다.

이제 플러터 웹 앱에서 WASM을 사용하려면 먼저 플러터 SDK를 3.22 또는 그 이상의 버전으로 업그레이드해야 합니다.

플러터 SDK를 설치한 후 다음 명령을 실행하여 플러터 웹 앱을 WASM으로 빌드합니다. flutter build web --wasm.

## 플러터 + WASM 사용 시 주목할 점

<div class="content-ad"></div>

플러터와 함께 WASM을 사용할 때 특히 성능 측면에서 몇 가지 뚜렷한 개선 사항이 있을 수 있어요:

- 빠른 실행: 복잡한 계산, 애니메이션 또는 데이터 처리 작업과 같이 계산이 많이 필요한 앱 부분은 WASM의 네이티브 실행 속도로 인해 훨씬 빠르게 실행될 거예요.
- 부드러운 애니메이션: WASM으로 만들어진 애니메이션 및 전환 효과는 특히 그래픽 중심 애플리케이션에서 더 부드러워질 수 있어요. 스크롤이 더 부드러워진 것도 눈에 띌 거예요.
- 로드 시간 감소: WASM 모듈은 효율적인 실행을 위해 최적화되어 있고 작기 때문에 웹 앱의 초기 로드 시간이 더 빨라질 수 있어요.

## WASM의 단점

WASM은 성능 문제와 스크롤 문제를 해결하는 데 도움이 되지만, 일반적으로 몇 가지 WASM의 단점도 있어요:

<div class="content-ad"></div>

- WASM에는 내장된 가비지 수집기가 포함되어 있지 않습니다. 이는 가비지 수집(JavaScript나 Dart와 같이 가비지 수집에 크게 의존하는 언어)에 의존하는 언어들이 WASM으로 컴파일될 때 메모리 관리를 위해 추가적인 처리가 필요함을 의미합니다.
- 아직 WASM을 위한 모든 패키지가 지원되는 것은 아닙니다.
- WASM 모듈은 응용 프로그램의 전체 메모리 풋프린트를 증가시킬 수 있으며, 이는 제한된 리소스를 갖는 기기에서 성능에 영향을 줄 수 있습니다.
- 플러터 + WASM에서 여전히 많은 문제가 있습니다. 최신 정보는 여기서 확인해주세요.

# 이 글을 즐겁게 읽으셨기를 바랍니다!

의문이 있으시다면 언제든지 @AbhishekDoshi26으로 메시지 남겨주세요.
자세한 내용은 abhishekdoshi.dev에서 확인해보세요 💙