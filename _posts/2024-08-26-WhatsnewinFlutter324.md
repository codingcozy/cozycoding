---
title: "플러터 3.24 버전에 새롭게 추가된 기능 정리"
description: ""
coverImage: "/assets/img/2024-08-26-WhatsnewinFlutter324_0.png"
date: 2024-08-26 19:45
ogImage: 
  url: /assets/img/2024-08-26-WhatsnewinFlutter324_0.png
tag: Tech
originalTitle: "Whats new in Flutter 3.24"
link: "https://medium.com/flutter/whats-new-in-flutter-3-24-6c040f87d1e4"
isUpdated: true
updatedAt: 1724742410397
---


## 플러터 GPU, 멀티뷰 임베딩, 그리고 더 많은 혜택을 만나보세요

![이미지](/assets/img/2024-08-26-WhatsnewinFlutter324_0.png)

최신 플러터 업데이트에 오신 것을 환영합니다! 플러터 3.24에는 앱 개발 경험을 높이기 위한 흥미로운 새로운 기능과 개선 사항이 가득합니다. 이번 릴리스에서는 Flutter GPU의 미리보기를 강조하며, 이를 통해 Flutter에서 직접 고급 그래픽 및 3D 씬을 구현할 수 있습니다. 웹 앱은 이제 여러 개의 Flutter 뷰를 포함하여 앱의 다양성을 높일 수 있습니다. 마지막으로, 수익을 극대화할 수 있도록 비디오 광고 수익화 지원이 추가되었습니다.

지난 몇 달 동안, 플러터 커뮤니티는 매우 활발했습니다. 852개의 프레임워크 커밋과 615개의 엔진 커밋이 있었습니다. 이번 릴리스를 가능하게 한 새로운 49명의 기여자를 환영합니다. 여러분의 헌신과 열정이 플러터를 전진시키는 원동력입니다.

<div class="content-ad"></div>

그래서 지금 바로 시작해서 플러터 커뮤니티가 이번 최신 릴리스에 추가한 모든 새로운 기능들과 개선 사항을 탐색해 보세요!

# 플러터 프레임워크

## 새로운 슬리버

이번 릴리스에서는 다이나믹 앱바 동작을 위해 함께 구성할 수 있는 새로운 슬리버가 추가되었습니다:

<div class="content-ad"></div>

- PinnedHeaderSliver
- SliverResizingHeader

이 새로운 슬리버를 사용하면 사용자가 스크롤할 때 플로팅, 고정된 상태로 유지되거나 크기가 조절되는 헤더를 만들 수 있습니다. 이 새로운 슬리버들은 기존의 SliverPersistentHeader와 SliverAppBar 슬리버와 비슷하지만 더 간단한 API를 가지고 있어 더 큰 효과를 내기 위해 결합할 수 있습니다.

이 새로운 슬리버들에는 새로운 샘플 코드가 포함되어 있습니다. 예를 들어, PinnedHeaderSliver의 API 문서에는 iOS Settings 앱의 앱 바와 비슷한 효과를 재현하는 예제가 있습니다: 

<img src="/assets/img/2024-08-26-WhatsnewinFlutter324_1.png" />

<div class="content-ad"></div>

## 쿠퍼티노 라이브러리 업데이트

이번 릴리즈에서는 CupertinoActionSheet의 믿음성을 향상시켰습니다. 액션 시트 버튼을 터치할 때 진동 피드백이 제공됩니다. 버튼의 글꼴 크기와 굵기가 이제 네이티브 버전과 일치합니다.

![이미지](/assets/img/2024-08-26-WhatsnewinFlutter324_2.png)

또한 CupertinoButton에 새로운 포커스 속성을 추가했으며, 비활성 상태의 CupertinoTextField 색상을 사용자 정의할 수 있습니다.

<div class="content-ad"></div>

큐퍼티노 도서관을 새롭게 리프레시하고 있습니다. 앞으로 더 많은 업데이트를 기대해주세요!

## TreeView

2차원 스크롤 가능한 패키지에서 TreeView 위젯을 출시했습니다. 나무가 자라면서 모든 방향으로 스크롤할 수 있는 효율적인 스크롤 트리를 구축하기 위한 여러 동반 클래스도 함께 제공됩니다. 이 패키지에 포함된 샘플 앱도 TableView와 TreeView 위젯을 사용하는 새로운 예제들로 업데이트되었습니다.

<img src="/assets/img/2024-08-26-WhatsnewinFlutter324_3.png" />

<div class="content-ad"></div>

TreeSliver는 일차원 스크롤링을 위한 트리 빌딩을 위한 프레임워크에 추가되었습니다. TreeView 및 TreeSliver API는 서로 일치하여 사용 사례에 맞게 쉽게 전환할 수 있습니다.

## CarouselView

이 릴리스에는 Material Design 회전목마 위젯인 CarouselView가 포함되었습니다. CarouselView는 "비포함" 레이아웃을 제공하며 컨테이너의 가장자리로 스크롤되는 항목의 스크롤 가능한 목록을 제공하며 선행 및 후행 항목은 보이는 곳 밖으로 스크롤되고 보이는 곳 안으로 스크롤될 때 동적으로 크기를 변경할 수 있습니다.

![이미지](https://miro.medium.com/v2/resize:fit:1024/1*6ytqSvtR2TJzAE6LntHTGw.gif)

<div class="content-ad"></div>

## 위젯에서 더 많은 기능 사용 가능

이 릴리스에는 디자인 특정이 아닌 코어 위젯 로직을 Material 라이브러리에서 일반적인 용도로 Widgets 라이브러리로 이동하기 위한 일부 작업이 포함되어 있습니다. 이렇게 포함된 것은 다음과 같습니다:

- 피드백 위젯은 탭, 길게 누르기 등의 제스처에 대한 기기에서 햅틱 및 오디오 피드백의 쉬운 액세스를 제공합니다.
- ToggleableStateMixin 및 ToggleablePainter는 체크 상자, 스위치, 라디오 버튼과 같은 토글 위젯을 만들기 위한 기본 클래스입니다.

## AnimationStatus에 대한 향상된 enum 기능

<div class="content-ad"></div>

커뮤니티 멤버 @nate-thegrate의 훌륭한 기고로 AnimationStatus에 개선된 enum 기능이 추가되었습니다. 추가된 기능은 다음과 같습니다:

- isDismissed
- isCompleted
- isRunning
- isForwardOrCompleted

이러한 getter 중 일부는 AnimationController와 CurvedAnimation과 같은 Animation 하위 클래스에 이미 존재했습니다. 이제 이러한 상태 getter가 AnimationStatus에 추가되었습니다. 마지막으로, AnimationController에는 애니메이션 방향을 전환하는 토글 메서드가 추가되었습니다.

## SelectionArea 업데이트내용

<div class="content-ad"></div>

 
Flutter의 SelectionArea는 이제 마우스를 사용한 triple click과 터치 기기에서의 더블 탭과 관련된 더 많은 네이티브 제스처를 지원합니다. 기본적으로 SelectionArea 및 SelectableRegion 위젯은 이러한 새로운 제스처를 사용합니다.

Triple click

- Triple click + 드래그: 단락 블록에서 선택을 확장합니다.
- Triple click: 클릭한 위치의 단락 블록을 선택합니다.

![이미지](/assets/img/2024-08-26-WhatsnewinFlutter324_4.png)

<div class="content-ad"></div>

더블 탭

- 더블 탭 + 드래그: 단어 블록에서 선택 범위를 확장합니다 (네이티브 안드로이드/퓨시아/iOS 및 iOS 웹에서 지원됨).
- 더블 탭: 탭한 위치의 단어를 선택합니다 (네이티브 안드로이드/퓨시아/iOS 및 안드로이드/퓨시아 웹에서 지원됨).

![이미지](/assets/img/2024-08-26-WhatsnewinFlutter324_5.png)

# 엔진

<div class="content-ad"></div>

## 임펠러

성능 및 충실도 향상

임밴더의 최적 미리보기(opt-out) 제거를 iOS에서 안정적인 릴리스를 통해 예정되어 있으므로, 팀은 임펠러의 성능과 충실도를 개선하기 위해 매진하고 있습니다. 예를 들어, 텍스트 렌더링에 대한 긴 여러 개선사항 시리즈로 인해 이모지 스크롤링의 성능이 크게 향상되어 이모지 컬렉션을 커서 스크롤할 때의 지지직거림이 사라지게 되었는데, 이는 임펠러의 텍스트 렌더링 능력을 환상적으로 스트레스 테스트하게 되었던 것입니다.

또한 이 릴리스에서 발생한 몇 가지 문제들을 해결함으로써, 우리는 임펠러의 텍스트 렌더링의 충실도를 크게 향상시켰습니다. 특히, 텍스트 두께, 간격, 그리고 케링(kerning)은 이제 전통 렌더러의 텍스트의 충실도와 완전히 일치합니다.

<div class="content-ad"></div>


![Flutter Image 1](/assets/img/2024-08-26-WhatsnewinFlutter324_6.png)

![Flutter Image 2](/assets/img/2024-08-26-WhatsnewinFlutter324_7.png)

![Flutter Image 3](/assets/img/2024-08-26-WhatsnewinFlutter324_8.png)

![Flutter Image 4](/assets/img/2024-08-26-WhatsnewinFlutter324_9.png)


<div class="content-ad"></div>

안녕하세요!

Android 미리보기

이번 릴리스에서는 Android에서 Impeller를 계속해서 미리보기 중입니다. Android 14에서 발생한 버그로 인해 발생한 어려움으로 인해 미리보기 기간을 연장했습니다. 이 버그는 Impeller가 플랫폼 뷰에 사용하는 API에 영향을 미치며, Android 팀에서 이미 패치되었습니다. 그러나 많은 배포된 기기가 미래에도 패치되지 않은 Android 버전을 실행할 것으로 예상됩니다. 이러한 문제를 해결하기 위해 추가 API 이관 및 추가 안정적인 릴리스 주기가 필요합니다. Flutter 앱이 가능한 최대 범위의 기기에서 작동함을 보장하기 위해, 올해 후반에 안정적인 릴리스가 있을 때까지 Impeller를 기본 렌더러로 사용하지 않을 것입니다.

Android에서 Impeller 미리보기가 3.24 안정 주기를 통해 계속됨에 따라 Flutter 개발자분들께 최신 안정 버전으로 업그레이드하고, Impeller를 활성화했을 때 발견된 어떤 결함이나 문제에 대해 문제를 제기해 주시기를 요청합니다. 이 단계에서의 피드백은 Impeller가 Android에서 성공적이 되고, 올해 후반 릴리스에서 기본 렌더러로 확신을 가질 수 있도록 하는 데 매우 중요합니다. Android 하드웨어 생태계는 iOS 생태계보다 훨씬 다양합니다. 이에 Impeller에 대한 가장 유용한 피드백은 발생한 문제가 있는 구체적인 기기 및 Android 버전에 대한 상세한 정보를 포함해야 합니다.

## 이미지 축소에 대한 기본값 개선

<div class="content-ad"></div>

이번 릴리스에서는 이미지의 기본 FilterQuality가 FilterQuality.low에서 FilterQuality.medium으로 변경되었습니다. 대상 사각형보다 훨씬 큰 대형 이미지의 경우 FilterQuality.low를 사용하면 이미지가 '픽셀화'되어 렌더링이 FilterQuality.medium보다 느려질 수 있습니다. 앞으로 팀은 다양한 FilterQuality 레벨에 더 적합한 이름을 탐구할 예정입니다.

## Flutter GPU 미리보기

Flutter는 Flutter GPU를 통해 렌더링 기능에 대한 주요 업데이트를 도입했습니다. 이 저수준 그래픽 API를 사용하면 개발자는 네이티브 플랫폼 코드 없이 Dart 코드와 GLSL 셰이더를 사용하여 사용자 정의 렌더러를 만들 수 있습니다.

Flutter GPU를 통해 Flutter에서 직접 렌더링할 수 있는 범위가 확장되었습니다. 고급 그래픽 및 3D 장면을 구현할 수 있습니다. 현재 iOS, macOS, Android에서 지원되는 Impeller 렌더링 백엔드가 필요합니다. 초기 미리보기 단계이지만, Flutter GPU는 최종적으로 모든 Flutter 플랫폼을 지원할 계획입니다.

<div class="content-ad"></div>

API를 통해 렌더 패스 첨부 파일, 버텍스 스테이지 및 GPU로 데이터 업로드를 완전히 제어할 수 있습니다. 이 유연성은 2D 캐릭터 애니메이션부터 복잡한 3D 장면까지 고급 렌더링 솔루션을 만드는 데 필수적입니다.

개발자들은 메인 채널로 변경하고 flutter_gpu 패키지를 프로젝트에 추가함으로써 Flutter GPU를 사용할 수 있습니다. 다가오는 몇 달 동안 더 많은 기능과 안정성 향상을 볼 수 있을 것이며, flutter_scene와 같은 고수준 렌더링 라이브러리를 통해 이러한 고급 기능을 쉽게 사용할 수 있을 것입니다.

Flutter GPU에 대해 더 깊게 파고들고 프로젝트에서 이를 어떻게 활용할 수 있는지 알아보려면 자세한 Flutter GPU 블로그 게시물을 확인해보세요. 게임이나 복잡한 그래픽을 만드는 경우라도, Flutter의 새로운 GPU 기능은 제품에 강력한 선택지가 됩니다.

# 웹

<div class="content-ad"></div>

# Multi-view embedding

플러터 웹 애플리케이션은 이제 멀티 뷰 임베딩을 활용할 수 있습니다. 개발자들은 동시에 여러 HTML 요소로 콘텐츠를 렌더링할 수 있습니다. 이 기능인 "임베디드 모드" 또는 "멀티 뷰"는 기존 웹 애플리케이션에 플러터 뷰를 통합할 수 있는 유연성을 제공합니다.

멀티 뷰 모드에서 플러터 웹 애플리케이션은 시작 시 즉시 렌더링되지 않습니다. 대신 호스트 애플리케이션이 addView 메소드를 사용하여 첫 번째 "뷰"를 추가할 때까지 기다립니다. 호스트 애플리케이션은 이러한 뷰를 동적으로 추가하거나 제거할 수 있으며, 플러터는 위젯을 그에 맞게 조정합니다.

멀티 뷰 모드를 활성화하려면 flutter_bootstrap.js 파일 내의 initializeEngine 메소드에서 multiViewEnabled: true로 설정하십시오. 그런 다음 JavaScript에서 뷰를 관리할 수 있고, 필요에 따라 지정된 HTML 요소에 추가하거나 제거할 수 있습니다. 각 뷰 추가 및 제거는 플러터 내에서 업데이트를 트리거하여 동적 콘텐츠 렌더링이 가능합니다.

<div class="content-ad"></div>

이 기능은 여러 독립적인 Flutter 뷰가 필요한 복잡한 웹 애플리케이션에 Flutter를 통합하는 데 특히 유용합니다. 또한 각 뷰에 대한 사용자 지정 초기화 데이터를 지원하여 맞춤 구성 및 인터랙티브 경험을 가능하게 합니다.

Flutter 웹 애플리케이션에 멀티뷰 임베딩을 구현하기 위해 더 깊이 들어가려면 자세한 문서를 확인해보세요.

# 수익화

# 비디오 광고 수익화 지원

<div class="content-ad"></div>

우리는 Flutter 모바일 앱에서 인스트림 비디오 광고 수익을 지원하기 위해 새 Interactive Media Ads (IMA) 플러그인을 출시했어요. 이 새 IMA 플러그인은 기존 Google Mobile Ads (GMA) 플러그인 위에 Flutter 앱을 위한 새로운 광고 수익 기회를 제공하며, GMA 플러그인은 주로 디스플레이 광고 포맷을 지원해요.

인스트림 비디오 광고는 일반적으로 사용자에게 동영상 콘텐츠 재생 전(프리롤), 중간(미드롤) 또는 후(포스트롤)에 비디오 플레이어에서 보여집니다. 일부 인스트림 비디오 광고는 스킵할 수도 있어요.

<img src="/assets/img/2024-08-26-WhatsnewinFlutter324_10.png" />

Flutter IMA의 장점:

<div class="content-ad"></div>

- 플러터 앱에서 비디오 플레이어 콘텐츠를 원활하게 활용하세요. 예를 들어, 앱 사용자가 비디오 콘텐츠에서 재생을 클릭하면, 이제 Flutter IMA 플러그인을 구현하여 비디오 콘텐츠를 시작하기 전에 사용자에게 15초 광고를 먼저 보여줄 수 있습니다.
- IMA SDK의 네이티브 버전과 동일한 혜택을 누릴 수 있습니다. 이는 프리미엄 구글 광고 수요 및 산업 표준 규정(IAB VAST 등)에 대한 액세스를 포함합니다.

최초 출시된 버전은 현재 Android 및 iOS 플랫폼에서 프리롤 비디오 광고를 지원합니다. 중간 광고 지원은 곧 제공될 예정입니다. 플러터 앱 비디오 콘텐츠에서 새로운 IMA 플러그인을 탐색하기 시작하시기를 권장합니다. GitHub에서 문제 또는 우려 사항이 있는 경우 알려주세요.

리소스: 플러그인 가이드, 샘플 애플리케이션, 깃 레포지토리

# iOS

<div class="content-ad"></div>

# Swift Package Manager의 초기 지원

안녕하세요! Flutter는 현재 CocoaPods를 사용하여 네이티브 iOS 또는 macOS 종속성을 관리합니다.

Flutter 3.24은 Swift Package Manager를 초기 지원합니다. 이를 통해 여러 가지 이점이 제공됩니다:

1. Swift 패키지 생태계에 액세스할 수 있습니다. Flutter 플러그인들은 점점 성장하는 Swift 패키지 생태계를 활용할 수 있게 됩니다!
2. Flutter 설치가 간편해집니다. Swift Package Manager는 Xcode와 함께 제공됩니다. 앞으로 Flutter를 Apple 플랫폼에서 사용하기 위해 Ruby와 CocoaPods를 설치할 필요가 없어질 것입니다.

<div class="content-ad"></div>

플러그인 개발자분들께 Swift Package Manager를 지원하도록 플러그인에 추가해보고 경험에 대한 피드백을 제공해보시기를 권장합니다.

만약 Flutter의 Swift Package Manager 지원에 대한 피드백이 있으시다면 이슈를 제보해주세요.

# 생태계

# Shared Preferences 플러그인 업데이트

<div class="content-ad"></div>

shared_preferences 플러그인에 SharedPreferencesAsync와 SharedPreferencesWithCache 두 가지 새 API가 추가되었습니다. 가장 중요한 변경 사항은 Android 구현이 Shared Preferences 대신 Preferences DataStore를 사용한다는 것입니다.

SharedPreferencesAsync을 사용하면 사용자는 기기에 저장된 가장 최신의 환경 설정을 직접 얻을 수 있지만, 비동기적이며 캐시된 버전보다 약간 느립니다. 다른 시스템이나 격리된 환경 설정에 의해 업데이트될 수 있는 환경 설정에 유용합니다. 이는 캐시를 오래된 상태로 만들 수 있는 환경 설정에 유용합니다.

SharedPreferencesWithCache는 SharedPreferencesAsync를 기반으로 작성되었으며 사용자가 환경 설정의 로컬에 캐시된 복사본에 동기적으로 액세스할 수 있도록 합니다. 이는 이전 API와 유사하지만 이제 다른 매개변수로 여러 번 인스턴스화될 수 있습니다.

이러한 새로운 API는 미래에 현재의 SharedPreferences API를 대체하는 것을 목표로 합니다. 그러나 이것은 생태계에서 가장 많이 사용되는 플러그인 중 하나이며, 생태계가 새로운 API로 전환하는 데 시간이 걸릴 것을 알고 있습니다.

<div class="content-ad"></div>

# 플러터와 다트 패키지 생태계 정상회담 유럽 2024

![이미지](/assets/img/2024-08-26-WhatsnewinFlutter324_11.png)

2024년 플러터콘 유럽 행사의 일환으로, 우리는 첫 번째 오프라인 플러터와 다트 패키지 생태계 정상회담을 개최했습니다. 이는 2023년 8월에 열린 첫 번째 가상 정상회담을 계속했습니다. 토론회에서의 중요 포인트 요약을 여기서 확인해보세요.

다음 정상회담은 2024년 9월 20일 뉴욕시에서 개최되는 플러터콘 미국에서 열릴 예정이며, 참가자 및 기여자인 경우 플러터콘 미국 2024에 참석할 예정이라면 정상회담에 등록하여 자리를 예약해주세요.

<div class="content-ad"></div>

본 행사에서 패키지 개발자와 유지 관리자들이 모여 언컨퍼런스 스타일의 세션을 진행했어요. 아래는 논의된 주제들입니다:

- 세션 1 — 네이티브 상호 운용성의 과거, 현재 그리고 미래
- 세션 2 — 지속 가능한 패키지 유지 관리 모델
-  세션 3 — 패키지 생태계의 분열 해결

우리는 이 행사가 플러터와 다트 이벤트 중 일부로 열리게 되면, 커뮤니티 간 공개 토론을 위한 소중한 플랫폼이 된다고 믿어요. 중요한 도전 과제를 도출하고 해결책을 브레인스토밍하는 것에 도움을 줄 거에요. 앞으로 더 많은 행사를 개최해 나갈 예정이며, 커뮤니티와 함께 진행할 거에요.

# DevTools and IDEs

<div class="content-ad"></div>

이 릴리스에는 Flutter DevTools 도구 모음에 대한 몇 가지 흥미로운 개선 사항이 포함되어 있습니다.

만약 여러분의 Flutter 앱이 예상보다 더 많은 위젯을 빌드하고 있는지 궁금했다면, DevTools 성능 도구의 새로운 기능이 도움이 될 수 있습니다. 새로운 Rebuild Stats 기능을 사용하면 앱에서 위젯이 얼마나 자주 빌드되었는지, 심지어 특정 Flutter 프레임에서 빌드된 횟수에 대한 정보를 얻을 수 있습니다.

![DevTools Performance tool이 리빌드 통계를 추적하는 화면 샘플](/assets/img/2024-08-26-WhatsnewinFlutter324_12.png)

DevTools 성능 도구가 리빌드 통계를 추적하는 화면의 스크린샷입니다.

<div class="content-ad"></div>

네트워크 프로파일러와 플러터 딥 링크 도구와 같은 도구에 다듬음을 더했으며 중요한 버그 수정도 했습니다. 또한 IDE 내에서 DevTools를 사용할 때 더 나은 경험을 제공하기 위해 일반적인 개선 사항을 몇 가지 만들었습니다. 이야기는 IDE에 대해, DevTools 도구를 IDE 내에서 직접 사용할 수 있다는 것을 알고 계셨나요?

![이미지](/assets/img/2024-08-26-WhatsnewinFlutter324_13.png)

DevTools 화면이 VS Code 창 내에서 열립니다.

![이미지](/assets/img/2024-08-26-WhatsnewinFlutter324_14.png)

<div class="content-ad"></div>

안녕하세요! 개발자 여러분!

DevTools 화면은 Android Studio 도구 창 내에서 열립니다.

본 릴리스에는 VS Code의 Flutter 사이드바가 개선되어 더 쉽게 필요한 도구에 액세스할 수 있습니다. 개선된 사이드바에 액세스하려면 VS Code 및 Flutter 및 Dart 확장 프로그램의 최신 버전으로 업그레이드하세요.

![이미지](/assets/img/2024-08-26-WhatsnewinFlutter324_15.png)

Flutter 사이드바는 적응형이며 작업 공간에 맞게 조절됩니다.

감사합니다!

<div class="content-ad"></div>

이 릴리스에는 DevTools Extensions 프레임워크에 대한 주요 개선 사항이 포함되어 있습니다. 이제 당신의 패키지 종속성 중 하나에서 제공된 도구인 DevTools 확장 기능을 사용할 수 있습니다. Dart 또는 Flutter 테스트를 디버깅할 때 뿐만 아니라 아무것도 디버깅하지 않을 때라도 IDE에서 코드를 작성할 때도 사용할 수 있습니다. 따라서 이러한 사용자 journey 중 하나를 위해 도구를 사용하고 싶었던 경우, 이제 가능해졌습니다.

Flutter 3.24에 포함된 모든 업데이트에 대해 더 알아보려면 DevTools 2.35.0, 2.36.0 및 2.37.2의 릴리스 노트를 확인하세요.

# 주요 변경 사항 및 사용 중단 사항

이 릴리스의 주요 변경 사항에는 Navigator의 페이지 API 변경, PopScope에서의 제네릭 형식, Flutter 웹의 기본 렌더러 변경 및 새로운 사용 중단 사항이 소개되는 변경 사항이 포함되어 있습니다. 변경 사항 페이지에서 마이그레이션 지침의 전체 목록을 확인하세요.

<div class="content-ad"></div>

항상 수고 많으셨습니다. 테스트에 기여해 주신 커뮤니티 여러분, 진심으로 감사드립니다. 이러한 버전 변경 사항을 식별하는 데 도움을 주셨습니다. 더 자세한 내용을 알고 싶다면, 플러터의 버전 변경 정책을 확인해 보세요.

# 결론

플러터의 성공의 핵심은 여러분 — 우리 놀랄만한 커뮤니티에 있습니다. 여러분의 무수한 기여와 열정이 없었다면, 이번 릴리스는 불가능했을 것입니다. 이번 릴리스로 이루어진 구체적인 내용에 대해 자세히 살펴보시려면, Flutter 3.24의 릴리스 노트와 변경 로그를 확인해 주세요.

<div class="content-ad"></div>

플러터 3.24와 함께 다트 3.5가 이제 안정 채널에서 사용 가능합니다. 이 최신 버전의 플러터와 함께 하는 여정은 flutter upgrade를 실행하는 것만큼 간단합니다. 여러분이 만들어내는 것을 기다리고 있어요!