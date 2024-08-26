---
title: "이번 주의 React 소식 197 Waku, Effect, TanStack, Framer Motion, use() 등의 최신 기술 소식을 알려줘"
description: ""
coverImage: "/assets/img/2024-08-26-ThisWeekInReact197WakuEffectTanStackFramerMotionusePreactValtioAstroThreejsNitrogen_0.png"
date: 2024-08-26 20:03
ogImage: 
  url: /assets/img/2024-08-26-ThisWeekInReact197WakuEffectTanStackFramerMotionusePreactValtioAstroThreejsNitrogen_0.png
tag: Tech
originalTitle: "This Week In React 197  Waku, Effect, TanStack, Framer Motion, use(), Preact, Valtio, Astro, Three.js, Nitrogen..."
link: "https://dev.to/sebastienlorber/this-week-in-react-197-waku-effect-tanstack-framer-motion-use-preact-valtio-astro-threejs-nitrogen-3efl"
isUpdated: false
---


안녕하세요 모두!

Theodo Apps(이전 BAM)의 Cyril과 Matthieu입니다👋, Seb 대신 React와 React Native 세계의 최신 소식을 전해드리고 있습니다.

아직 한주가 조용했지만 여전히 여러분에게 훌륭한 업데이트가 있습니다. Waku는 이제 React Server Actions를 지원하며, TanStack/Router 사용 팁도 있고, React가 완전한 스택 프레임워크가 되는 과정을 탐구합니다. 또한 React Native 0.75에서 무엇이 새로운지 확인해보세요! NitroModules와 react-native-webGPU에 대한 몇 가지 업데이트도 있습니다. 참여하시고 즐기세요!

파트너 컨퍼런스 React Advanced London (🇬🇧 런던 - 10월 25일 & 28일)도 확인해보세요. 우리는 선진 React 토크의 아이디어를 정말 좋아하고, 지금까지 라인업이 실망시키지 않았습니다!👌

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

💡 매주 이메일을 받으려면 공식 뉴스레터를 구독해보세요!

![이미지](/assets/img/2024-08-26-ThisWeekInReact197WakuEffectTanStackFramerMotionusePreactValtioAstroThreejsNitrogen_0.png)

## 💸 후원사

![이미지](/assets/img/2024-08-26-ThisWeekInReact197WakuEffectTanStackFramerMotionusePreactValtioAstroThreejsNitrogen_1.png)

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

React PR 리뷰가 컴포넌트 혼돈으로 변하고 있나요?

React PR 리뷰를 간략화하고 팀의 주요 코드 품질 측면에 초점을 맞출 전략을 발견하세요. 의존성 관리, 코드 조정 및 기타 사항을 포함하여 모든 PR을 검토하며, 팀이 아키텍처 결정, 컴포넌트 설계 및 성능 최적화에 집중할 수 있도록 합니다.

CodeRabbit은 다음을 지원합니다:

- Best Practice Enforcement: CodeRabbit의 공통 React 안티패턴을 식별하고 최상의 실천 방식을 제안하는 능력을 활용하여 우수한 코드 품질을 보장하세요.
- Component Structure Review: 구체적인 컴포넌트 구조 요약을 활용하여 더 나은 구성을 위한 제안을 받아보세요.
- Actionable Code Review Comments: 1-클릭 커밋 제안으로 PR에서 코드 수정을 수행하세요.

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

CodeRabbit은 모든 오픈 소스 저장소에 무료로 제공됩니다. 오늘 바로 시작해보세요!

## ⚛️ React

![React](/assets/img/2024-08-26-ThisWeekInReact197WakuEffectTanStackFramerMotionusePreactValtioAstroThreejsNitrogen_2.png)

Waku v0.21 - React Server Actions에 대한 완벽한 지원

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

리액트 서버 컴포넌트는 새로운 "리액트 프레임워크"의 새로운 파도를 일으켰어요. 조타이 (jotai, valtio의 창조자인 Daishi Kato가 만든 '와쿠(Waku)'도 그 중 하나에요. 이것은 Next.js 외부에서 서버 컴포넌트를 지원하는 최초의 프레임워크 중 하나였어요.

서버 액션을 추가하여, 와쿠에서는 이제 대부분의 리액트 19 기능을 지원하며 "API가 필요한 부분을 건너뛰고" 완전한 앱을 개발할 수 있어요.

![이미지](/assets/img/2024-08-26-ThisWeekInReact197WakuEffectTanStackFramerMotionusePreactValtioAstroThreejsNitrogen_3.png)

리액트부터 이펙트까지

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

마이클 아르 날디(Michael Arnaldi)는 Effect의 창시자로, Effect와 React에서 사용하는 모델이 어떻게 유사한지 설명했습니다. Effect는 강력한 툴킷이지만 시작하기가 다소 어렵다는데, 마치 새로운 언어처럼요. 이미 익숙한 것들과 유사점을 찾아내는 것이 도움이 될 것입니다.

Effect에서 React와 마찬가지로 작성하는 대부분의 코드는 "청사진(blueprint)"입니다. 즉, 프로그램이 직접 실행되지 않고 무엇을 해야 하는지를 선언하는 것입니다. 그런 다음 라이브러리(React 또는 Effect)가 실행을 처리해줍니다.

- 💸 TutorialKit — 백엔드 인프라를 구축하거나 관리하지 않고 즉시 대화형 튜토리얼을 생성합니다.
- 🗓️ reactjsday - 🇮🇹 베로나 - 10월 25일 - "TWIR" 코드로 10% 할인 받기. A, B, C가 발표할 예정입니다!
- 🗓️ Squiggle Conf - 🇺🇸 보스턴 - 10월 3일 & 4일 - "TWIR" 코드로 10% 할인 받기. 스피커가 발표 되었으니 컨퍼런스 웹사이트에서 살펴보세요.
- 📜 TanStack/Router를 8개월간 사용하면서 쌓인 팁: Swizec Teller가 loader, suspense query 또는 query를 사용할 때 적용하는 원칙들을 공유합니다.
- 📜 React는 (되는 중인) 풀 스택 프레임워크: 서버 컴포넌트(Server Components)와 서버 액션(Server Actions)의 추가로
- 📜 React에서 상태 동기화하기: useEffect가 아닌 컴포넌트를 분리하는 것으로 클래식한 버그를 "올바른 방법으로" 해결하는 방법을 설명합니다.
- 📜 React와 Framer Motion을 사용하여 Swipe Actions 컴포넌트 만들기
- 📜 useStateObject: useState 주위에 간편한 API
- 📜 순차적 요청을 처리하는 React Hook 만들기 - AbortController 사용
- 📜 React에서 PDF 생성하기 - html2pdf.js, React PDF, 그리고 Puppeteer
- 📜 Preact - Preset Vite와 함께 사전 렌더링
- 📜 좋은 리팩토링 vs 나쁜 리팩토링
- 📜 Valtio의 탄생 과정
- 📦 Astro v4.14 - 콘텐츠 파일에서 인텔리센스 지원하는 실험적인 콘텐츠 레이어 API
- 📦 react-three/xr 6.2.0: 제한 없는 공간, 깊이 감지, 그리고 보조 입력 소스 추가
- 📦 react18-use: Daishi Kato가 만든 React 18에서 작동하는 React 19 use hook의 shim입니다. useMemo 내에서 use(Context) 호출 가능 (실험적)하며, React 팀이 구현할 계획인 컨텍스트 셀렉터(context selectors)에 대한 더 나은 대안
- 📦 Formity: JSON 구조로 복잡한 양식 정의, 패키지가 무거운 작업을 처리하도록 함
- 📦 rspack-plugin-react-refresh v1.0
- 🎙️ PodRocket - Malte Ubl과 함께 Generative UI와 React 컴포넌트
- 🎥 Jack Herrington - React Web Devs를 위한 Secret React-Native Hack! - 패키지를 수정하여 서버 컴포넌트와 호환되지 않는 종속성 수정하기

## 💸 스폰서

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>


![이미지](/assets/img/2024-08-26-ThisWeekInReact197WakuEffectTanStackFramerMotionusePreactValtioAstroThreejsNitrogen_4.png)

Storybook, Playwright 및 Cypress에 대한 시각적 테스트

레이아웃 오류와 UI의 끈적한 느림에 지친 적이 있나요? Chromatic의 시각적 테스트를 사용하여 자신감 있게 빌드할 수 있습니다. 기능적 테스트에서 놓치는 버그인데요, 이를 잡아내어 정렬 오류부터 잘못된 색상 및 Z-인덱스 문제까지 해결해줍니다.

별도의 테스트 케이스나 구성이 필요하지 않습니다. Chromatic은 기존의 Storybook, Playwright 또는 Cypress 설정에 연결하여 응용 프로그램 UI의 시각적 테스트를 활성화합니다. 모든 테스트는 Chromatic의 초고속 캡처 클라우드 인프라에서 지원되며 별도 비용 없이 병렬로 실행됩니다.


<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

설정은 딱 2분이면 끝나요. 그리고 저희 기본 요금제에는 매월 5,000개의 무료 스냅샷이 포함돼 있어요. 오늘 바로 시작해보세요 »

## 📱 React-Native

React Native 0.75

이번 주에는 React Native 0.75가 릴리스되었는데, 핵심 팀은 생산 준비가 완료된 앱을 구축하기 위해 Expo와 같은 React Native 프레임워크를 사용하는 것을 강력히 권장했어요. 이러한 변화를 반영해, 핵심 React Native 패키지에서 /template 폴더가 제거되었고, 2024년 말까지 react-native init 명령어가 폐기될 예정이지만 둘 다 @react-native-community 패키지 내에서는 여전히 사용할 수 있어요. 이번 업데이트를 통해 성능이 향상되는 것뿐만 아니라 자동 연결 빌드 단계 중에 향상된 성능을 보여주고, 새 구조가 활성화되면 gap, columnGap, rowGap 및 translation 속성에 백분율 사용이 가능한 Yoga 3.1이 추가되었어요. 이러한 개선 사항을 통해 새 구조를 도입하는 것이 최신 기능 및 안정성 향상에 필수적임을 알 수 있어요.

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

## 🔀 기타

- 💸 리액트 네이티브 마스터리 - 리액트 네이티브 및 엑스포를 마스터하는 데 필요한 유일한 코스
- 👀 리액트 네이티브 0.76은 단일 병합된 동적 라이브러리 libreactnative.so로 출시됩니다. 이는 앱 크기와 시작 시간을 줄일 것입니다.
- 🐦 Nitrogen, NitroModules용의 새로운 코드 생성기: Marc가 Nitrogen이 스위프트 사양을 생성하고 Objective-C 및 C++ 코드를 건드리지 않고 구현할 수 있는 방법을 제시합니다 👏
- 🐦 리액트 네이티브웹용 타입이 DefinitelyTyped에 게시되었습니다.
- 🐦 리액트 네이티브-웹GPU 덕분에 RN에서 three.js 실행 가능
- 🐦 리액트 쓰리 파이버 컴포넌트로 구성된 expo-dom 데모
- 🗓️ 리액트 유니버스 컨퍼런스 - 🇵🇱 브로츠와프 - 9월 5-6일. "TWIR" 코드로 10% 할인 혜택 받으세요. 발표자: 에반 베이컨, 마크 로우사비, 츠베판 미코브, 오스카르 크와쉬니에프스키...
- 📜 리액트 네이티브 0.75가 엑스포 SDK 51로 함께 제공됩니다: SDK 52를 기다릴 필요 없이 버그 수정, 성능 향상, 새 아키텍처의 안정화를 낮은 업그레이드 비용으로 이용해보세요.
- 📜 Progressive Web Apps (PWA): 네이티브 모바일 앱으로 나아가는 한 가지 중간 단계: PWA와 네이티브 앱 간의 절충에 대한 최신 평가
- 📦 리액트 네이티브 키보드 컨트롤러 v1.13: 안드로이드 모달 및 리액트 네이티브 0.75를 지원합니다.
- 📦 Skip v1.0: 개발자가 Swift 및 SwiftUI로 네이티브 iOS 앱을 작성하고 동시에 네이티브 안드로이드 코드를 자동으로 생성할 수 있는 새로운 크로스플랫폼 프레임워크
- 📦 쇼핑카트 - Tophat은 이제 오픈소스로 공개되었습니다.
- 🎙️ RNR 304 - 사티지트 사후와 함께하는 리액트 내비게이션 V7 - 정적 API, 향상된 딥 링킹, 향상된 TypeScript 지원 등 리액트 내비게이션 7의 새로운 기능
- 🎥 Jolly Coding - 엑스포가 웹에서 네이티브로 마이그레이션을 더 쉽게 만들었습니다 - 엑스포 돔 데모

## 🤭 재미

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>


![2024-08-26-ThisWeekInReact197WakuEffectTanStackFramerMotionusePreactValtioAstroThreejsNitrogen_5.png](/assets/img/2024-08-26-ThisWeekInReact197WakuEffectTanStackFramerMotionusePreactValtioAstroThreejsNitrogen_5.png)

![2024-08-26-ThisWeekInReact197WakuEffectTanStackFramerMotionusePreactValtioAstroThreejsNitrogen_6.png](/assets/img/2024-08-26-ThisWeekInReact197WakuEffectTanStackFramerMotionusePreactValtioAstroThreejsNitrogen_6.png)

See ya! 👋
