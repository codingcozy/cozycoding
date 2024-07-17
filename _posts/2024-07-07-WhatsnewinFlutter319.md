---
title: "Flutter 319의 새로운 기능은 무엇일까"
description: ""
coverImage: "/assets/img/2024-07-07-WhatsnewinFlutter319_0.png"
date: 2024-07-07 22:20
ogImage: 
  url: /assets/img/2024-07-07-WhatsnewinFlutter319_0.png
tag: Tech
originalTitle: "What’s new in Flutter 3.19"
link: "https://medium.com/flutter/whats-new-in-flutter-3-19-58b1aae242d2"
---


오늘은 새로운 Flutter 릴리즈인 Flutter 3.19을 소개합니다. 해당 릴리즈에는 새로운 Dart SDK for Gemini, 위젯 애니메이션에 세밀한 제어를 추가할 수 있는 위젯, Impeller의 업데이트로 렌더링 속도가 향상되었으며, 딥링크를 구현하는 도구, Windows Arm64 지원 등이 포함되어 있습니다!

Flutter 커뮤니티는 계속해서 성장 중이며, 168명의 커뮤니티 멤버가 1429개의 풀 리퀘스트를 머지했으며, 43명의 커뮤니티 멤버가 첫 번째 Flutter 풀 리퀘스트를 커밋했습니다!

이번 릴리즈에 기여한 Flutter 커뮤니티의 새로운 추가 기능과 개선 사항에 대해 알아보려면 계속 읽어주세요!

# AI 통합

<div class="content-ad"></div>

## Gemini Google AI Dart SDK 베타 릴리스

Google AI Dart SDK가 베타로 출시되었습니다. 이 릴리스를 통해 Dart 또는 Flutter 앱에 Google의 최신 AI 모델 계열인 Gemini을 활용한 생성적 AI 기능을 구축할 수 있습니다. 이제 pub.dev에 google_generative_ai 패키지가 있습니다. Google AI Dart SDK를 활용하여 어떻게 개발할 수 있는지에 대한 자세한 내용은 이 블로그 포스트를 통해 확인하거나 Dart 퀵 스타트로 바로 이동할 수 있습니다.

![이미지](/assets/img/2024-07-07-WhatsnewinFlutter319_0.png)

# Framework

<div class="content-ad"></div>

## 스크롤 개선 사항

플러터는 이제 두 손가락으로 드래그하면 스크롤 속도가 두 배 빨라졌었어요. 이제 MultiTouchDragStrategy.latestPointer로 기본 ScrollBehavior를 구성하여 손가락 수에 상관없이 스크롤 동작을 할 수 있어요. 이 변경 사항에 대한 자세한 내용은 이주 가이드를 참조해주세요.

또한, SingleChildScrollView 및 ReorderableList의 버그 수정을 완료했으며, 여러 개의 신고된 충돌 및 예기치 않은 동작을 해결했어요.

2차원 스크롤링에서, 이제 양방향으로 스크롤이 진행 중일 때 드래그하거나 탭하면 예상대로 스크롤 동작이 멈추도록 문제를 해결했어요.

<div class="content-ad"></div>

최근 릴리스 이후에 업데이트된 two_dimensional_scrollables 패키지의 TableView 위젯은 더 세련되어지며, 병합된 셀을 지원하고, 3.16의 안정적인 릴리스 이후 2D foundation의 새로운 기능들을 더 채택했습니다.

## AnimationStyle

Flutter 커뮤니티 구성원인 @TahaTesser가 기여해 주셔서 Flutter에는 MaterialApp, ExpansionTile, PopupMenuButton 등의 기본 애니메이션 동작을 재정의할 수 있는 AnimationStyle 위젯이 추가되었습니다. 이를 통해 개발자들은 애니메이션 곡선과 지속 시간을 재정의할 수 있는 기능을 제공받을 수 있게 되었습니다.

## SegmentedButton.styleFrom

<div class="content-ad"></div>

플러터 커뮤니티 멤버인 @AcarFurkan은 다른 버튼 유형에서 제공되는 것과 같이 styleFrom 정적 유틸리티 메서드를 추가했습니다. 이 메서드를 사용하면 SegmentedButton의 ButtonStyle을 빠르게 생성하여 다른 세그먼트 버튼과 공유하거나 앱의 SegmentedButtonTheme을 구성하는 데 사용할 수 있습니다.

## 적응형 스위치

이 적응형 컴포넌트는 macOS 및 iOS에서 네이티브로 보이고 느껴지며 다른 곳에서는 Material Design 룩 앤 필을 갖습니다. 이는 Cupertino 라이브러리에 의존하지 않으므로 모든 플랫폼에서 API가 완전히 동일합니다.

Switch.adaptive 생성자 API 페이지에서 적응형 스위치 풀 리퀘스트 및 라이브 예제를 확인해주세요.

<div class="content-ad"></div>

## 의미 속성 접근성 식별자

SemanticsProperties에 추가된 새로운 접근성 식별자는 네이티브 접근성 계층에서 의미 노드를 식별하는 데 사용됩니다. Android에서는 리소스 ID로 표시되며, iOS에서는 UIAccessibilityElement.accessibilityIdentifier로 설정됩니다. 엔진과 프레임워크를 거쳐 이루어진 이 변경에 기여해 주신 @bartekpacia 커뮤니티 멤버님께 감사드립니다.

## 텍스트 위젯 상태 접근성이 증가했습니다

TextField 및 TextFormField에 MaterialStatesController 지원을 추가하여 MaterialState 변경을 듣도록 할 수 있습니다.

<div class="content-ad"></div>

## 취소 기록 스택

일본어 키보드에서 취소/다시 실행 기록이 사라질 수 있는 문제를 해결했습니다. 이제 UndoHistory 스택에 푸시되기 전에 항목을 수정할 수 있습니다.

# 엔진

## 임펠러 진행상황

<div class="content-ad"></div>

안녕하세요! 안드로이드 OpenGL 미리보기에 관한 내용입니다.

3.16 안정 버전에서는 Vulkan을 지원하는 안드로이드 기기에서 Impeller를 시도해 볼 것을 사용자들에게 제안했습니다. 이로써 현장에서 77%의 안드로이드 기기를 커버할 수 있게 되었습니다. 지난 몇 달 동안 Impeller의 OpenGL 백엔드를 Vulkan 백엔드와 기능적으로 동등하게 만드는 작업을 진행해왔습니다. 예를 들어, MSAA 지원을 추가함으로써 Flutter 앱이 거의 모든 안드로이드 기기에서 올바르게 렌더링되도록 할 수 있게 되었습니다. 곧 지원될 예정인 사용자 정의 쉐이더 및 외부 텍스처의 완전한 지원과 같은 일부 기능을 제외하고는요.

Flutter 개발자 여러분께서는 최신 안정 버전으로 업그레이드하고 Impeller를 활성화할 때 관찰된 모든 결함에 대한 이슈를 제출해 주시기를 요청합니다. 이 단계에서의 피드백은 Android에서 Impeller가 성공적으로 작동하고, 올해 나중에 릴리스에서 기본 렌더러로 자신 있게 설정할 수 있도록 보장하는 데 매우 중요합니다. 안드로이드 하드웨어 생태계는 iOS 생태계보다 다양합니다. 그러므로 Impeller에 관한 가장 유익한 피드백은 문제가 발생한 구체적인 기기 및 안드로이드 버전에 대한 자세한 정보를 포함해야 합니다.

그리고 잊지 말아야 할 점은, Impeller의 Vulkan 백엔드는 Skia와는 다른 디버깅 기능을 디버그 빌드에서 추가로 제공하며, 이로 인해 추가적인 런타임 오버헤드가 발생합니다. 그러므로 Impeller의 성능에 대한 피드백은 프로필 또는 릴리스 빌드에서 제공하는 것이 중요합니다. DevTools의 시간대와 해당 기기에서 Skia 백엔드와의 비교가 포함된 버그 리포트가 있어야 합니다. 마지막으로, 언제나 작은 재현 가능한 테스트 케이스를 제시하는 피드백에 대해 매우 감사드립니다.

<div class="content-ad"></div>

로드맵

렌더링 성능을 확보한 후, Impeller의 안드로이드 미리보기 기간 동안 우리의 주된 초점은 성능에 있습니다. 점진적인 개선을 계속하고 있지만, 몇 가지 큰 개선 사항도 진행 중입니다. Vulkan 서브패스를 활용한 작업을 통해 고급 블렌드 모드의 성능을 크게 향상시킬 것으로 기대됩니다. 더불어 CPU에서 항상 모든 경로를 테셀레이션하는 렌더링 전략을 변경하여 스텐실-커버 방식으로 전환함으로써, Impeller의 CPU 사용량을 크게 줄일 것으로 예상됩니다. 마지막으로, 가우시안 블러링의 새로운 구현을 통해 Skia 구현의 처리량을 맞추고, iOS에서 블러링의 관용적 사용을 개선할 것으로 예상됩니다.

## API 개선 사항

글리프 정보

<div class="content-ad"></div>

이 릴리스에는 dart:ui의 Paragraph 객체에 두 가지 새로운 메서드가 추가되었습니다: getClosestGlyphInfoForOffset과 getGlyphInfoAt이며, 각각 새로운 유형인 GlyphInfo 객체를 반환합니다. 새로운 GlyphInfo 유형에 대한 문서를 확인해보세요.

GPU 추적

Metal(IOS, macOS, 시뮬레이터)에서 Impeller 및 Vulkan을 지원하는 Android 장치에서, 이제 Flutter 엔진이 디버그 및 프로필 빌드에서 각 프레임의 GPU 시간을 타임라인에 보고합니다. GPU 프레임 타이밍은 DevTools에서 "GPUTracer" 제목 아래에서 확인할 수 있습니다.

![이미지](/assets/img/2024-07-07-WhatsnewinFlutter319_1.png)

<div class="content-ad"></div>

비틀거리의 GPU 추적은 Android 기기에서 GPU 타이밍 쿼리 가능 여부를 잘못 보고할 수 있기 때문에, 이 기능을 활성화하려면 안드로이드Manifest.xml 파일에 플래그를 설정해야 합니다.

```js
<meta-data
  android:name="io.flutter.embedding.android.EnableOpenGLGPUTracing"
  android:value="true" />
```

## 성능 최적화

특수화 상수

<div class="content-ad"></div>

팀은 Impeller에 전문화 상수를 지원하도록 추가했어요. 이 기능을 활용하면 Impeller의 셰이더에서 플러터 엔진의 압축되지 않은 이진 크기가 거의 350KB 정도 줄어들었어요.

배경 필터 속도 향상

그러나 아직 해야 할 일이 많이 남아있지만, 이 릴리스에는 Impeller의 배경 필터와 블러의 성능을 개선하는 멋진 두 가지 변경 사항이 포함되어 있어요. 특히, 오픈 소스 기여자인 @knopp는 Impeller가 온스크린 텍스처에서 읽기 기능을 잘못 요청하는 것을 발견했어요. 이 기능을 제거하니 복잡도에 따라 벤치마크에서 배경 필터가 포함된 장면이 20–70% 정도 성능이 향상되었어요.

또한, Impeller는 이제 모든 배경 필터에 대해 무조건 스텐실 버퍼를 저장하지 않아요. 대신 임시 레이어를 복원할 때 컬러 클립 영역 등의 영향을 받는 작업들이 기록되어 배경 필터에 대한 새 스텐실 버퍼에 반복되어 재생되어요.

<div class="content-ad"></div>


![이미지](/assets/img/2024-07-07-WhatsnewinFlutter319_2.png)

이 변경으로 Pixel 7 Pro에서 Vulkan 백엔드를 사용하는 Impeller에서 실행되는 고급 블렌드 모드에 대한 벤치마크가 개선되어, 평균 GPU 프레임 시간이 55ms 에서 16ms로 개선되었고, 90% 최상위 래스터 스레드 CPU 시간은 대략 110ms 에서 22ms로 개선되었습니다.

# 안드로이드

## 웹 딥링킹 유효성 검사 도구


<div class="content-ad"></div>

개발자들로부터 깊은 링킹(웹 URL에서 모바일 앱의 특정 페이지로 이동하는 것)이 항상 구현하기 어려웠고 오류가 발생하기 쉬웠다는 사실을 알게 되었습니다. 그래서 먼저 개발자가 잘못 구성된 링크를 이해하고 구현 지침을 제공하는 검증 도구를 만들었습니다. 초기 버전의 Flutter 딥링크 유효성 검사 도구가 이제 사용 가능하다는 점을 기쁘게 생려 드립니다!

이 초기 버전에서 Flutter 딥링크 유효성 검사 도구는 Android의 웹 확인을 지원합니다. 이는 assetlinks.json 파일의 설정을 확인하는 것을 의미합니다. DevTools를 열고 Deep Links 탭으로 이동한 다음 deeplinks를 포함하는 Flutter 프로젝트를 가져와주세요. 딥링크 검증 도구는 웹 파일이 올바르게 구성되었는지 여부를 알려줍니다. 자세한 정보는 딥링크 유효성 검사 도구 테스트 지침서를 참조해주세요.

이 도구가 깊은 링킹 구현 과정을 간소화하기 위한 첫 걸음이 되길 바랍니다. 향후 iOS에서도 웹 확인을 지원하고, iOS와 Android에서 앱 확인을 지원하기 위해 계속 노력하겠습니다!

![이미지](/assets/img/2024-07-07-WhatsnewinFlutter319_3.png)

<div class="content-ad"></div>

## Share.invoke 지원

안드로이드의 텍스트 필드 및 뷰에 기본 Share 버튼이 이전에 누락되었지만, 이번 릴리스에서 모든 기본 컨텍스트 메뉴 버튼이 각 플랫폼에서 사용할 수 있도록 지속적으로 노력하는 일환으로 추가되었습니다. 계속되는 작업은 PR #107578에서 확인할 수 있습니다.

## Native assets 기능

Flutter의 상호 운용성에 관심이 있는 경우 Android에서 Native assets를 통해 FFI 호출을 수행할 수 있습니다. 이는 네이티브 자산 지원을 위한 우리의 지속적인 노력의 일환입니다.

<div class="content-ad"></div>

## 텍스처 레이어 하이브리드 구성 (TLHC) 모드

Flutter 3.19에는 이제 Google 지도 및 텍스트 입력 확대기가 TLHC 모드에서 작동하도록 만드는 작업이 포함되어 있습니다. 이는 여러분의 앱에 대한 성능을 향상시킵니다. Google 지도를 사용 중이시라면 변경 사항을 테스트하고 피드백을 주시기를 권장합니다!

이 작업에는 Framework 또는 Engine 하위의 변경 사항이 포함되어 있지 않지만, PR 5408에 해당하는 작업 및 THLC를 테스트하는 단계를 확인할 수 있습니다.

## 시스템 전체의 맞춤형 텍스트 선택 툴바 버튼

<div class="content-ad"></div>

안녕하세요, 개발자님!

안드로이드 앱은 모든 텍스트 선택 메뉴에 나타나는 사용자 정의 텍스트 선택 메뉴 항목을 추가할 수 있습니다(텍스트를 길게 누르면 나타나는 메뉴). Flutter의 텍스트 필드 선택 메뉴는 이제 해당 항목을 포함하고 있습니다.

# iOS

## Flutter iOS 네이티브 폰트

Flutter 텍스트는 이제 조금 더 조밀하고 iOS에서 더 네이티브한 모습을 보입니다. Apple 디자인 가이드라인에 따르면, iOS에서 작은 글꼴은 휴대폰에서 더 쉽게 읽을 수 있도록 간격을 넓히고, 큰 글꼴은 공간을 너무 많이 차지하지 않도록 조밀하게 보여야 합니다. 이전에는 모든 경우에 작은 간격의 글꼴을 잘못 사용했었습니다. 이제 기본적으로 Flutter는 큰 텍스트에 대해 조밀한 글꼴을 사용할 것입니다.

<div class="content-ad"></div>


<img src="/assets/img/2024-07-07-WhatsnewinFlutter319_4.png" />

# DevTools

## DevTools 업데이트

이번 릴리스의 DevTools에 대한 주요 업데이트 내용은 다음과 같습니다:


<div class="content-ad"></div>

- 안드로이드에서 deeplinks 설정을 확인하는 새로운 기능과 화면을 DevTools에 추가했어요.
- 플러그인이 있는 앱에 유용한 플랫폼 채널 활동을 추적하는 '강화 추적' 메뉴에 옵션을 추가했어요.

![이미지](/assets/img/2024-07-07-WhatsnewinFlutter319_5.png)

- 연결된 앱이 없을 때에도 성능 및 CPU 프로파일러 화면을 사용할 수 있게 되었어요. 이전에 저장한 성능 데이터나 CPU 프로파일은 이 화면에서 볼 수 있게 다시 불러올 수 있어요.
- VS Code의 Flutter 사이드바에서 현재 프로젝트에 활성화되지 않은 새 플랫폼을 활성화할 수 있는 기능이 추가되었고, 사이드바의 DevTools 메뉴에는 DevTools를 외부 브라우저 창에서 열 수 있는 옵션이 추가되었어요.

더 자세한 내용은 DevTools 2.29.0, 2.30.0 및 2.31.0 릴리스 노트를 확인해주세요.

<div class="content-ad"></div>

# 데스크톱

## Windows Arm64 지원

윈도우에서의 플러터는 커뮤니티 멤버인 @pbo-linaro의 훌륭한 노력 덕분에 Arm64 아키텍처에 대한 초기 지원을 환영합니다. 이번 초기 지원은 윈도우 Arm64 기기에서 네이티브로 실행되는 더 효율적이고 성능이 우수한 플러터 애플리케이션에 대한 길을 엽니다. 아직 개발 중이지만, GitHub 이슈 #62597에서 추적 가능한 진행을 보여주고 있으며, 이러한 변화는 플러터 개발자들이 자신들의 앱을 더 넓은 범위의 윈도우 기기에 최적화하기를 원하는 더 많은 가능성을 암시합니다.

# 생태계

<div class="content-ad"></div>

## 필수 이유 개인 정보 보호 매니페스트

플러터에 이제 iOS에 대한 개인 정보 보호 매니페스트가 포함되어 있어 애플의 다가오는 요구 사항을 충족합니다.

## 플러터와 다트 패키지 생태계의 진행 상황

놓치신 분들을 위해, 1월 블로그 포스트에서 플러터와 다트 패키지 생태계의 진행 상황을 확인해보세요.

<div class="content-ad"></div>

# 사용 중단 및 호환성 파괴 변경

## Windows 7 및 8 지원 중단

Flutter가 진화함에 따라, Dart 3.3 및 Flutter 3.19 릴리스에서 Windows 7 및 8을 지원 중단하고 최신 기술에 집중하기로 기쁨을 느낍니다. 이러한 변경은 Microsoft의 전략과 일치하여 Flutter를 최신 운영 체제에 향상시킬 수 있게 합니다. 개발자들이 필요로 하는 조정을 감사히 받아들이며, 이 전환기에 도움을 드리겠다는 다짐을 합니다. 이러한 변화로 지원되는 Windows 버전에서 더욱 안전하고 효율적이며 기능이 풍부한 개발 환경으로의 개척을 준비합니다. Flutter 생태계에서 함께 혁신을 이어가는 데 기여해주셔서 감사드립니다.

<div class="content-ad"></div>

3.16 안정 버전 릴리스 릴리스 노트에서 확인할 수 있듯이 글로벌 플래그 Paint.enableDithering가 제거되었습니다. 자세한 내용은 웹 사이트의 주요 변경 사항 공지를 참조하십시오.

## iOS 11 폐기

특정 네트워킹 API를 호출할 때 런타임 충돌이 발생하는 문제로 인해 Flutter는 더 이상 iOS 11을 지원하지 않습니다. 이는 Flutter 3.16.6 이후로 빌드된 응용 프로그램이 해당 장치에서 실행되지 않음을 의미합니다.

## 자동 렌더 모드 폐기

<div class="content-ad"></div>

이번 릴리스의 중요 변경 사항에는 v3.16 이후 만료된 사용되지 않는 API가 포함되어 있습니다. 영향 받는 모든 API 및 추가 컨텍스트와 이주에 대한 안내를 보려면 해당 릴리스의 폐기 안내서를 확인하세요. 이러한 폐기 사항의 많은 부분은 Flutter 수정에 의해 지원되며 IDE에서 빠른 수정이 가능합니다. 대량 수정은 dart fix 명령줄 도구를 사용하여 평가하고 적용할 수 있습니다.

항상 테스트에 기여해 주신 커뮤니티 여러분께 많은 감사드립니다. 이를 통해 우리는 이러한 중요 변경 사항을 식별할 수 있습니다. 더 알아보려면 Flutter의 중요 변경 사항 정책을 확인해 보세요.

이번 릴리스에서는 사용되지 않는 정책에 flutter_driver 패키지를 추가했습니다. 기존에 지원되는 flutter와 flutter_test 패키지도 계속 지원됩니다.

# 결론

<div class="content-ad"></div>

이 발표의 시작에서 돌출된 기여자들의 놀라운 수가 있습니다. 이는 우리의 놀라운 커뮤니티의 헌신과 노력을 직접적으로 증명한 Flutter가 강력하고 효율적인 툴킷으로 발전한 것입니다. 여러분 각자에게 진심으로 감사드립니다.

이 릴리스를 통해 이루어진 구체적인 성과에 대해 자세히 알아보기 위해 Flutter 3.19의 추가 사항을 포괄적으로 나열한 릴리스 노트 및 변경 로그를 확인하시기 바랍니다.

Flutter 3.19와 함께 Dart 3.3도 안정 채널에서 이제 사용 가능합니다. Flutter와의 최신 여정은 flutter upgrade 명령을 실행하는 것만큼 간단합니다.