---
title: "Flutter 알림 마스터하기 awesome_notification 패키지 가이드 "
description: ""
coverImage: "/assets/img/2024-06-21-MasteringFlutterNotificationsAGuidetoawesome_notificationPackagePart-IStepbyStepGuide_0.png"
date: 2024-06-21 22:46
ogImage: 
  url: /assets/img/2024-06-21-MasteringFlutterNotificationsAGuidetoawesome_notificationPackagePart-IStepbyStepGuide_0.png
tag: Tech
originalTitle: "Mastering Flutter Notifications: A Guide to awesome_notification Package Part-I (Step by Step Guide)"
link: "https://medium.com/@shubhamsoni82422/mastering-flutter-notifications-a-guide-to-awesome-notification-package-part-i-step-by-step-4bda734d114a"
isUpdated: true
---





- Flutter 앱에서 원활하고 강력하며 사용자 친화적인 알림 경험을 위해 awesome_notification의 모든 기능을 활용해보세요.

![이미지](/assets/img/2024-06-21-MasteringFlutterNotificationsAGuidetoawesome_notificationPackagePart-IStepbyStepGuide_0.png)

Flutter에서 push 알림의 힘을 활용하는 것은 사용자 참여도와 유지율 향상에 중요합니다. 이러한 동적 알림은 사용자에게 실시간 업데이트 및 맞춤화된 상호작용을 제공하여 더 입체적이고 빠른 앱 경험을 조성합니다. 시각적으로 매력적이고 기능이 풍부한 알림을 만들 수 있는 awesome_notifications은 Flutter 개발자들이 사용자를 매혹시키고 앱이 계속해서 주목받을 수 있도록 돕습니다. 오늘날의 경쟁적인 모바일 환경에서 성공을 위한 키패드인 push 알림으로 앱의 커뮤니케이션 전략을 높여보세요.

이 튜토리얼을 위한 프로젝트 구조를 시작해봅시다. 이 이미지에서 기본 구조를 확인할 수 있습니다: -

<div class="content-ad"></div>

![이미지](/assets/img/2024-06-21-MasteringFlutterNotificationsAGuidetoawesome_notificationPackagePart-IStepbyStepGuide_1.png)

홈 페이지와 제품 상세 페이지 두 개의 페이지를 만들었습니다.

Flutter에서 멋진 푸시 알림의 세계로 여정을 시작하기 위해, 우리의 Flutter 앱이 사용자에게 멋진과 동적인 알림을 만드는 데 필요한 도구를 갖추도록 보장하는 필수 종속성을 pubspec.yaml 파일에 추가하는 것으로 시작해봅시다.

pubspec.yaml 파일을 열고 다음 라인을 추가해주세요:

<div class="content-ad"></div>

아래 이미지는 Flutter에 최신 버전의 awesome_notifications 및 http 패키지를 가져와 통합하라고 알려줍니다.

awesome_notifications 패키지는 매력적이고 기능이 풍부한 알림을 만들기 위한 해결책으로 사용되고, http 패키지는 알림 워크플로에 필요한 모든 HTTP 요청을 용이하게 처리할 것입니다.

터미널에서 flutter pub get을 실행하여 이러한 종속성을 가져오고 설치하는 것을 기억하세요.

기본 작업이 완료되었으므로, 이제 awesome_notifications 패키지가 제공하는 끝없는 가능성을 탐험할 준비가 되었습니다. 즉각적인 알림, 예약된 경고 및 더 많은 기능을 만들어보자!

<div class="content-ad"></div>

우리의 플러터 프로젝트의 핵심은 main.dart 파일에 있습니다. 이 파일은 우리 멋진 알림 시스템의 초기화를 조정하는 진입점입니다. 즐거운 알림 경험을 위한 무척 중요한 코드 조각을 살펴보겠습니다.

main.dart에는 클래스 구성 메서드를 호출하고 라우트 생성기 클래스를 생성하여 material 앱 섹션에 할당하는 내용이 담겨 있습니다.

main.dart

![이미지](/assets/img/2024-06-21-MasteringFlutterNotificationsAGuidetoawesome_notificationPackagePart-IStepbyStepGuide_3.png)

<div class="content-ad"></div>

RouteGenerator.dart

이 클래스에 뷰 파일을 추가하고 네비게이션에 연결하세요.

<img src="/assets/img/2024-06-21-MasteringFlutterNotificationsAGuidetoawesome_notificationPackagePart-IStepbyStepGuide_4.png" />

애플리케이션 알림 유틸리티 클래스의 중요한 부분은 이 클래스에 있습니다. 사용자에게 멋진 알림을 제공하기 위해 몇 가지 속성과 메서드를 만들었으니 알림 유틸리티를 시작해보겠습니다.

<div class="content-ad"></div>

notification configuration 코드를 시작하기 전에, 이미지를 통해 멋진 notification 패키지에 의해 제공되는 매개변수를 확인할 수 있고, 이러한 매개변수를 사용하여 push notification을 탐색해보세요. 첫 번째 이미지에서는 단순한 notification만 볼 수 있고, 다른 이미지에서는 사용자 지정 버튼이 있는 notification을 볼 수 있습니다.


![First Image](/assets/img/2024-06-21-MasteringFlutterNotificationsAGuidetoawesome_notificationPackagePart-IStepbyStepGuide_5.png)

![Second Image](/assets/img/2024-06-21-MasteringFlutterNotificationsAGuidetoawesome_notificationPackagePart-IStepbyStepGuide_6.png)


Notification_utils.dart

<div class="content-ad"></div>


![이미지](/assets/img/2024-06-21-MasteringFlutterNotificationsAGuidetoawesome_notificationPackagePart-IStepbyStepGuide_7.png)

이제 사용자에게 알림 권한을 확인합니다. 이미 권한이 허용되어 있으면 네 개의 상자를 보여주는 홈 화면 UI가 표시됩니다. 권한이 허용되었는지 아닌지 확인하는 코드를 추가했습니다. 매우 간단한 내용이니 이 코드를 따라해보세요:-

![이미지](/assets/img/2024-06-21-MasteringFlutterNotificationsAGuidetoawesome_notificationPackagePart-IStepbyStepGuide_8.png)

코드와 주요 기능을 사용한 멋진 알림 패키지를 확인해봅시다.


<div class="content-ad"></div>

🚨 주요 기능:

1️⃣ 즉시 알림: 몇 줄의 코드로 손쉽게 즉시 알림을 생성하고 표시할 수 있어요! ⚡️

2️⃣ 예약 알림: 특정 시간에 전달될 알림을 예약해서 미리 계획하세요. 📅⏰

3️⃣ JSON 데이터 알림: JSON 데이터를 활용하여 동적으로 알림을 사용자 정의해보세요. 🧩📤

<div class="content-ad"></div>

4️⃣ 사용자 정의 버튼 알림: 사용자 상호작용을 더욱 향상시키기 위해 알림 내에 사용자 정의 버튼을 통합하세요. 🎛️📲

- . 즉시 알림: 몇 줄의 코드로 쉽게 즉시 알림을 생성하고 표시할 수 있습니다! ⚡️

![Screenshot](/assets/img/2024-06-21-MasteringFlutterNotificationsAGuidetoawesome_notificationPackagePart-IStepbyStepGuide_9.png)

2). 예약 알림: 알림을 특정 시간에 전달할 수 있도록 예약함으로써 미리 계획하세요. 📅⏰, 이 코드에서는 이 메소드를 호출한 후 1분 후에 알림이 도착하도록 예약 설정을 합니다. 이 기능을 이용하여 사용자에게 정기적으로 업데이트를 알리는 리마인더 또는 전자상거래 프로젝트나 피트니스 앱에서 일정 시간에 사용자에게 알리기 위해 사용할 수 있습니다. 예약된 알림을 다룰 때에는 안드로이드 manifest 파일에서 권한을 몇 가지 추가할 수 있습니다. 아래 이미지에서 해당 권한을 확인할 수 있습니다.

<div class="content-ad"></div>


![이미지 1](/assets/img/2024-06-21-MasteringFlutterNotificationsAGuidetoawesome_notificationPackagePart-IStepbyStepGuide_10.png)

![이미지 2](/assets/img/2024-06-21-MasteringFlutterNotificationsAGuidetoawesome_notificationPackagePart-IStepbyStepGuide_11.png)

3). JSON 데이터 통지: JSON 데이터를 활용하여 알림을 동적으로 사용자 정의하세요. 🧩📤 메서드 호출 시 jsonData를 전달할 수 있습니다. 자세한 내용은 홈페이지에서 확인해주세요. 그러면 메서드가 어떻게 작동하는지 쉽게 이해하고 확인할 수 있습니다.

![이미지 3](/assets/img/2024-06-21-MasteringFlutterNotificationsAGuidetoawesome_notificationPackagePart-IStepbyStepGuide_12.png)


<div class="content-ad"></div>

4). 사용자 상호작용을 더욱 향상시키기 위해 맞춤 버튼을 알림에 포함하여 사용하세요. 🎛️📲

지금부터 제가 최고로 선호하는 주요 기능은 더 나은 사용자 경험을 위한 것입니다. 이 멋진 알림이 도착했을 때 맞춤 버튼을 추가하여 필요에 따라 더 많은 사용자 경험을 제공할 수 있습니다. 사용자의 이동 부분에 대해 알아보시죠. 한 예를 통해 이해해보겠습니다. 전자 상거래 프로젝트를 작업 중이라고 가정해보세요. 두 개의 버튼 중 하나는 "지금 구매"이고 다른 하나는 "장바구니에 추가"입니다. 이 경우 사용자가 "지금 구매" 버튼을 클릭하면 사용자가 체크 아웃 화면 또는 페이지로 이동할 것이고, "장바구니에 추가"를 클릭하면 제품이 장바구니에 추가되고 사용자는 장바구니 화면으로 이동하게 됩니다. 이것이 기본 설명입니다. 이제 코드로 돌아가 봅시다. 여기 createCustomNotificationWithActionButtons() 메서드명이 있습니다.

![이미지](/assets/img/2024-06-21-MasteringFlutterNotificationsAGuidetoawesome_notificationPackagePart-IStepbyStepGuide_13.png)

이제 거의 모든 코드 작업을 마쳤습니다. 성공적으로 알림이 생성되거나 화면에 도착한 후 알림을 탭하면 사용자로 이동하게 만들어야 합니다. 그래서 이 코드를 추가했습니다. 위 코드를 따라해 주세요.

<div class="content-ad"></div>

onActionRecivedMethod은 사용자를 새 페이지로 리디렉션하거나 유효한 컨텍스트를 사용해야 할 때에만 필요합니다. 병렬 격리본은 유효한 컨텍스트를 갖고 있지 않기 때문에, 실행을 주 격리본으로 리디렉션해야 합니다.


![Image](/assets/img/2024-06-21-MasteringFlutterNotificationsAGuidetoawesome_notificationPackagePart-IStepbyStepGuide_14.png)


onActionRecivdeImplementationMethod은 사용자를 탐색할 때 실행될 것이므로, 이 방법을 사용하면 사용자에게 특정 페이지의 뷰나 화면을 정의할 수 있습니다. 또한, 사용자가 커스텀 버튼을 클릭하는 동안 사용자를 탐색할 수도 있으므로, 커스텀 버튼과 함께 알림을 생성할 때 키와 레이블을 전달하면 됩니다. 키를 사용하여 버튼을 클릭했을 때 페이지를 리디렉션할 조건이나 구성을 설정할 수 있으므로, 이 코드를 추가합니다. 아래 코드를 확인할 수 있습니다.


![Image](/assets/img/2024-06-21-MasteringFlutterNotificationsAGuidetoawesome_notificationPackagePart-IStepbyStepGuide_15.png)


<div class="content-ad"></div>

이제 우리는 onActionRecivedMethod, onNotificationCreatedMethod, onNotificationDisplayedMethod, onDismissActionReceivedMethod과 같은 멋진 알림 패키지에서 제공하는 이벤트를 수신하는 리스너를 설정했습니다. 이 메서드들을 사용하여 알림의 생성, 표시 및 해제를 감지할 수 있습니다. initState()에서 Homeview에서 startListeningNotificationEvents 메서드를 호출합니다.

![image](/assets/img/2024-06-21-MasteringFlutterNotificationsAGuidetoawesome_notificationPackagePart-IStepbyStepGuide_16.png)

이제 알림에 대한 논리 부분을 생성하고 추가했습니다. 이제 UI 부분으로 이동하여 사용자 인터페이스에서 화면을 만들겠습니다. 홈 화면과 제품 세부 정보 화면을 추가했습니다. 홈 페이지에는 아래와 같이 4개의 상자가 있는 GridView를 만듭니다.

![image](/assets/img/2024-06-21-MasteringFlutterNotificationsAGuidetoawesome_notificationPackagePart-IStepbyStepGuide_17.png)

<div class="content-ad"></div>

Home_view.dart에 대한 설명입니다.

Build 메소드 내에서 제목을 구현하고, notificationType에 알림 유형 서브 위젯을 추가합니다. GridView.extent를 사용하여 4가지 상자를 만들고 각각의 탭 기능에는 Gesture Detector 위젯을 사용하며, 탭할 때 해당하는 알림 클래스 메소드를 호출합니다.

![이미지](/assets/img/2024-06-21-MasteringFlutterNotificationsAGuidetoawesome_notificationPackagePart-IStepbyStepGuide_18.png)

첫 번째 상자에서 사용자가 즉시 통지를 탭하면 즉시 로컬 통지가 전송되며, 기기에 통지가 표시되고, 동시에 리디렉션도 수행됩니다.

<div class="content-ad"></div>

첫 번째 상자는 인스턴트 알림 생성을 통해 사용자가 상자를 탭할 때 utils 클래스의 createInstantNotification 메서드가 호출되고 알림이 전송됩니다.

두 번째 상자는 일정된 알림을 가지고 있습니다. 이는 스케줄 알림 기능을 사용하여 트리거된 알림을 설정하는 것을 의미합니다. 이러한 기능은 전자 상거래 앱, 피트니스 앱, 리마인더 앱 등 다양한 애플리케이션에서 주로 볼 수 있습니다.

따라서 상자 안에서 사용자가 탭하면 notification_utils 클래스의 createScheduleNotification 메서드를 호출합니다. 이 코드에서는 탭한 후 1분 동안의 시간이 경과한 후에 알림이 표시되며, 필요에 맞게 사용자 정의할 수 있습니다.

![이미지](/assets/img/2024-06-21-MasteringFlutterNotificationsAGuidetoawesome_notificationPackagePart-IStepbyStepGuide_19.png)

<div class="content-ad"></div>

세 번째 상자에는 JSON 객체로 알림을 생성하고 API나 JSON 응답에 따라 사용자 정의하거나 동적으로 만들 수 있습니다. 이를 위해 호출 jsonDataNotification 메소드가 호출될 때 매개변수를 JSON 객체 형식으로 전달할 수 있습니다.

![이미지](/assets/img/2024-06-21-MasteringFlutterNotificationsAGuidetoawesome_notificationPackagePart-IStepbyStepGuide_20.png)

마지막으로 가장 좋아하는 기능 중 하나는 사용자 정의 버튼 알림입니다.

네 번째 상자에서 상자를 탭하면 알림 클래스에서 createCustomNotificationWithActionButtons가 호출되고 이 메서드에서 actionButtons를 추가하여 사용자에게 표시합니다. actionButton에서 위젯 목록을 제공하고 버튼에 대한 키와 레이블을 잊지 마세요. 키를 사용하여 버튼KeyInput을 식별하고 동작을 수행하고 레이블은 단순 표시 용도입니다.

<div class="content-ad"></div>


![2024-06-21-MasteringFlutterNotificationsAGuidetoawesome_notificationPackagePart-IStepbyStepGuide_21](/assets/img/2024-06-21-MasteringFlutterNotificationsAGuidetoawesome_notificationPackagePart-IStepbyStepGuide_21.png)

이 화면의 제품 상세 페이지 UI는 단지 제품 데이터와 수량, 제품 크기, 그리고 제품 가격을 보여줍니다.

![2024-06-21-MasteringFlutterNotificationsAGuidetoawesome_notificationPackagePart-IStepbyStepGuide_22](/assets/img/2024-06-21-MasteringFlutterNotificationsAGuidetoawesome_notificationPackagePart-IStepbyStepGuide_22.png)

어서요, 기다리던 것이 끝났습니다. 리다이렉션을 통해 멋진 알림 패키지를 성공적으로 구현했습니다.


<div class="content-ad"></div>

![image](https://miro.medium.com/v2/resize:fit:600/1*KBn0C6MmIzCdSEM_1xZrfQ.gif)

플러터 개발의 복잡한 태피스트리 속에서 우리의 awesome_notifications 탐험은 혁신과 무한한 잠재력으로 가득한 여정이었습니다. 즉시 알림부터 예약된 통지 및 JSON 데이터의 동적 매력까지, 우리는 플러터 앱을 참여의 전도사로 변화시킬 수 있는 도구들을 발견했습니다.

이 기사가 가이드 역할을 해 준 것으로서 부디 당신이 푸시 알림을 통해 몰입형 사용자 경험을 만들어가는 길을 밝혀 주었기를 바랍니다. 이 놀라운 논의를 마무리하면서 새롭게 얻은 지식이 얼마나 매료시키고 당신의 플러터 프로젝트가 디지털 환경에서 빛날 수 있는지 곰곰히 생각해 보시기 바랍니다.

awesome_notifications 패키지 내부 동작을 더 깊이 파고들고 싶은 분들을 위해 GitHub의 포괄적인 코드베이스를 살펴보시기를 초대합니다:

<div class="content-ad"></div>

GitHub 저장소

저와 함께하시면 플러터 개발 세계에서 계속되는 토론, 통찰, 그리고 미래 탐구를 경험할 수 있습니다:

LinkedIn 프로필

이 여정에 투자해 주신 여러분께 감사드립니다. 여러분의 플러터 노력이 혁신적이고 원활한 기능성을 갖추며, 사용자들로부터 진정으로 중요한 알림을 받을 수 있기를 바라겠습니다.

<div class="content-ad"></div>

플러터 우수성을 위한 이 여정에 참여해 주셔서 감사합니다. 다음 탐험 때까지 즐거운 코딩하세요! 알림이 항상 멋지고 효과적하기를 바라요! 🚀📲