---
title: "Progressive Web Applications PWA로 첫 오프라인 웹 앱 만드는 방법"
description: ""
coverImage: "/assets/img/2024-06-22-BuildYourFirstOfflineWebAppftProgressiveWebApplications_0.png"
date: 2024-06-22 00:15
ogImage: 
  url: /assets/img/2024-06-22-BuildYourFirstOfflineWebAppftProgressiveWebApplications_0.png
tag: Tech
originalTitle: "Build Your First Offline Web App ft. Progressive Web Applications 🌟"
link: "https://medium.com/javascript-in-plain-english/build-your-first-offline-web-app-ft-progressive-web-applications-fa9298f70d4b"
isUpdated: true
---





![이미지](/assets/img/2024-06-22-BuildYourFirstOfflineWebAppftProgressiveWebApplications_0.png)

제목과 간단한 소개로 이미 아이디어를 얻으셨겠지만, 오늘은 Progressive Web Application (PWA)에 대해 배우겠습니다. PWA는 이름에서 알 수 있듯 기존 애플리케이션을 더 나은 방향으로 발전시킨 것이며, 이 기사에서는 PWA에 대한 간략한 소개를 제공하여 다음 프로젝트에서 쉽게 시작할 수 있도록 도와드리겠습니다.

## 기사 하이라이트

- 설치 가능한 웹 앱
- 오프라인 지원 및 캐싱
- 백그라운드 동기화
- 푸시 알림
- 네트워크 가로채기
- 자원 캐싱

<div class="content-ad"></div>

## 웹 앱을 프로그레시브 웹 앱으로 변환하기

웹 앱의 몇 가지 주요 특성들이 해당 앱을 PWA로 만들어줍니다. 반응성, 설치 가능성, 오프라인 기능, 백그라운드 동기화, 알림 등이 있습니다. 기존 애플리케이션을 PWA로 변환하려면 비즈니스 요구에 맞게 PWA의 특성을 하나씩 추가하면 됩니다.

반응성은 CSS와 미디어 쿼리를 추가하여 달성할 수 있습니다. manifest.json 파일 추가에 대한 설명은 이 문서를 참조하십시오. manifest.json에는 설치된 앱이 기기에서 보일 모습을 정의하는 다양한 속성이 있습니다. 예를 들어, 이름, 아이콘, 설치 배너, 방향 등을 정의합니다.

브라우저와 운영 체제에 크게 의존하는 오프라인 지원과 알림과 같은 다른 기능들은 서비스 워커가 필요합니다. 이에 대해 다음 섹션에서 더 자세히 이야기해보겠습니다.

<div class="content-ad"></div>

# 서비스 워커와 그 생명주기

서비스 워커는 브라우저에 의해 관리되고 제어되는 다른 스레드에서 작동합니다. 사용자가 귀하의 앱을 사용하는 동안 해당 브라우저에서 서비스 워커를 지원하지 않을 수 있으므로 해당 경우에 대비하여 엣지 케이스를 피할 수 있는 체크를 추가해야 합니다.

서비스 워커의 생명주기는 기기에 설치된 일반 네이티브 응용 프로그램과 거의 동일합니다. 이 유사성을 유지하면 서비스 워커에 대한 이해와 작업이 훨씬 더 쉬워집니다.

![이미지](/assets/img/2024-06-22-BuildYourFirstOfflineWebAppftProgressiveWebApplications_1.png)

<div class="content-ad"></div>

이제 서비스 워커가 무엇인지 명확해졌으니, 그들의 주요 기능 몇 가지를 살펴보겠습니다.

# 오프라인 지원 및 캐싱 (네트워크 가로채기 및 백그라운드 동기화)

앱을 완전히 오프라인으로 만들려면 앱에서 네트워크를 사용할 수 없을 때 GET 및 POST 요청을 처리해야합니다. 한 가지씩 살펴보겠습니다.

## 캐싱

<div class="content-ad"></div>

서비스 워커를 사용하면 fetch API를 통해 수행되는 모든 네트워크 요청을 가로챌 수 있으며, 해당 요청을 브라우저 캐시에 저장하여 캐시할 수 있습니다.

![이미지](/assets/img/2024-06-22-BuildYourFirstOfflineWebAppftProgressiveWebApplications_2.png)

위 예시에서 볼 수 있듯이, 우리는 네트워크 요청을 가로채고 캐시에 해당 항목이 있는 경우에는 다시 사용하고, 그렇지 않으면 서버에 요청을 하여 데이터를 가져와서 캐시에 저장하는 것을 볼 수 있습니다.

## 정적 자산 캐싱

<div class="content-ad"></div>

만약 캐시해야 할 엔드포인트가 무엇인지 모르는 경우, 매 요청을 가로채는 이벤트 리스너를 설치하는 것이 매우 효과적입니다. 그러나 CSS, JavaScript 파일 및 이미지와 같은 경우 우리가 정확히 URL을 알고 있는 경우 내장 함수를 사용하여 모든 입력 엔드포인트를 캐시하고 캐시에 저장할 수 있습니다.

![이미지](/assets/img/2024-06-22-BuildYourFirstOfflineWebAppftProgressiveWebApplications_3.png)

여기에서 우리가 논의한 것은 단지 비즈니스의 성격에 따른 기본 캐싱 전략뿐입니다.

다음은 널리 사용되는 몇 가지 인기있는 캐싱 기술 목록입니다.

<div class="content-ad"></div>

- 캐시 후 네트워크 (먼저 캐시에 리소스가 있는지 확인한 후 있으면 거기서 서비스하고, 그렇지 않으면 네트워크에 도달).
- 네트워크 후 캐시 (먼저 리소스를 네트워크를 통해 검색할 수 있는지 확인하고 가능하면 그것에 우선순위를 부여하고, 네트워크 요청이 실패하면 캐시된 리소스로 대체).
- 캐시와 네트워크 (이 전략에서는 먼저 캐시에 리소스가 있는지 확인한 후 있으면 거기에서 서비스하고, 동시에 서버에서 리소스를 가져오는 네트워크 호출을 하여 이전에 서비스된 리소스로 대체합니다).

원하는 캐시 전략을 사용하거나 애플리케이션의 특성에 기반하여 고유한 전략을 고안할 수 있습니다.

## 배경 동기화

이전 섹션에서 주로 GET 요청을 가로채는 방법에 대해 설명했습니다. 이번 섹션에서는 앱 외부에서 데이터를 받는 POST 요청과 함께 작동하는 방법에 대해 논의하겠습니다. 네트워크가 사용 불가능한 경우 아무것도 할 수 없습니다. 이를 처리하려면 POST 데이터를 어딘가에 저장해야 합니다(IndexedDB 또는 캐시) 그러고나서 앱이 다시 온라인 상태가 되면 동기화합니다.

<div class="content-ad"></div>


![image](/assets/img/2024-06-22-BuildYourFirstOfflineWebAppftProgressiveWebApplications_4.png)

![image](/assets/img/2024-06-22-BuildYourFirstOfflineWebAppftProgressiveWebApplications_5.png)

제가 첫 번째로 하는 작업은 SyncManager의 사용 가능 여부를 확인하는 것입니다. 사용 가능하다면 'sync-new-posts' 태그로 요청을 대기열에 넣습니다 (이 요청은 나중에 서비스 워커가 동기화 이벤트를 식별하는 데 사용될 것입니다). 이 동기화 요청은 나중에 브라우저에 의해 자동으로 처리되어 네트워크 상황이 적절할 때 즉시 요청을 전송할 것입니다.

![image](/assets/img/2024-06-22-BuildYourFirstOfflineWebAppftProgressiveWebApplications_6.png)


<div class="content-ad"></div>

# 푸시 알림

알림은 OS 수준의 기능입니다. 알림의 몇 가지 특성은 모든 OS에 공통되며 비즈니스 요구에 따라 사용자에게 표시할 알림 종류를 결정하고 그에 따라 알림을 빌드하는 옵션을 전달할 수 있습니다.

다른 기능과 마찬가지로 브라우저나 기기에서 해당 기능을 사용할 수 없는 경우에는 앱에 확인 및 예비조치를 취해주세요.

## 푸시 알림 설정하기 (구독)

<div class="content-ad"></div>

알림을 표시하려면 서비스 워커 등록 객체에 있는 showNotification 함수를 사용하면 됩니다.

![image](/assets/img/2024-06-22-BuildYourFirstOfflineWebAppftProgressiveWebApplications_7.png)

푸시 알림(서버를 통해 트리거)을 하려면 vapid 키를 사용하여 클라이언트를 서버와 인증하는 구독을 설정해야 합니다 (vapid 키는 웹 푸시를 통해 생성됩니다).

- 팔로드 백엔드 인증을 위해 퍼블릭 키(클라이언트용) 및 프라이빗 키(서버용)를 생성하여 프론트엔드를 백엔드와 인증합니다.
- 사용자가 앱에서 푸시 알림을 선택하면 pushManager의 getSubscription 함수를 사용하여 구독 세부정보를 생성합니다. 이 정보는 아래와 같이 나타납니다.

<div class="content-ad"></div>

![2024-06-22-BuildYourFirstOfflineWebAppftProgressiveWebApplications_8.png](/assets/img/2024-06-22-BuildYourFirstOfflineWebAppftProgressiveWebApplications_8.png)

이 정보는 데이터베이스에 저장되며 엔드포인트를 사용하여 브라우저에 요청이 전송됩니다. (크롬의 경우 fcm.googleapi...로 시작하는 브라우저마다 다를 수 있음), 서버는 브라우저의 엔드포인트로 요청을 보내며 브라우저 서버는 해당 요청을 브라우저로 전달하는 역할을 합니다.

![2024-06-22-BuildYourFirstOfflineWebAppftProgressiveWebApplications_9.png](/assets/img/2024-06-22-BuildYourFirstOfflineWebAppftProgressiveWebApplications_9.png)

프론트엔드 푸시 이벤트 리스너는 알림 요청이 도착하면 해당 요청을 수신하여 클라이언트 측 코드에 따라 작동합니다.

<div class="content-ad"></div>

![이미지](/assets/img/2024-06-22-BuildYourFirstOfflineWebAppftProgressiveWebApplications_10.png)

## 알림 옵션 사용자 정의

비즈니스 요구 사항에 맞게 사용자 지정할 수 있는 알림 옵션이 여러 가지 있습니다. 해당 문서를 확인하여 해당 옵션을 배우고 사용 중인 알림 옵션이 널리 지원되는지 확인하세요.

## 문제 해결하기 (생활을 쉽게 만들어 줄 수 있습니다)

<div class="content-ad"></div>

- 모든 작업을 정확히 수행했다고 생각하더라도 알림이 표시되지 않는 경우에는 기기 설정 및 웹 사이트 권한을 확인해보세요.
- 누군가 검사 탭에서 사이트 데이터를 지우면(아래 옵션에서) 구독이 사라지고 DB에서 사용할 수 없게 될 것입니다(기본적으로 알림이 작동을 멈출 것이므로 이 시나리오를 처리해야 합니다).

![Web App Image](/assets/img/2024-06-22-BuildYourFirstOfflineWebAppftProgressiveWebApplications_11.png)

## 알림 작업 청취

서비스 워커의 notificationclick 및 notificationclose에 두 가지 이벤트 리스너가 더 있습니다. 사용자가 알림과 상호 작용하는 것을 트리거할 것이며, 이는 사용자가 곧 알림 옵션에 따라 무언가를 수행하길 원할 경우 유용할 것입니다.

<div class="content-ad"></div>


![image](/assets/img/2024-06-22-BuildYourFirstOfflineWebAppftProgressiveWebApplications_12.png)

# 서비스 워커를 관리하는 편리한 도구: Workbox

지금까지 우리가 논의한 모든 코드와 기술은 우리가 직접 작성한 것이었습니다. 그러나 캐싱과 기타 기본적인 것들은 나중에 문제를 일으킬 수 있습니다.

- 코드의 캐시 버전을 업데이트할 때마다.
- 정적 자산을 수동으로 캐시 배열에 추가하기


<div class="content-ad"></div>

모든 이 기본 사항은 쉽게 잊혀질 수 있고 앱이 프로덕션에서 버그가 있는 것처럼 느껴질 수 있습니다. 그래서 이 문제를 피하고 다른 기본 서비스 워커 기능을 자동화하기 위해 workbox를 사용할 수 있습니다. workbox는 개발자의 삶을 쉽게 만들어주는 많은 내장 메소드를 제공하며 자주 사용되는 서비스 워커 기술을 구현할 수 있습니다.

## 전문가 팁

서비스 워커의 위치(URL 또는 파일 이름)를 변경하는 것을 피하십시오. 그렇게 하면 이전 서비스 워커와 병렬로 실행되는 다른 서비스 워커가 등록되는 문제가 발생할 수 있습니다. [참고]

이 문서에 대한 제안이나 교정 사항이 있으면 언제든지 환영합니다. 더 많은 글을 위해 팔로우 해주세요. 그 때까지 즐거운 코딩!❤.

<div class="content-ad"></div>

## 쉽게 설명한 영어 커뮤니티에 참여해 주셔서 감사합니다! 떠나시기 전에:

- 글쓴이를 튼튼히 응원하고 팔로우해 주세요 👏
- 팔로우하기: X | LinkedIn | YouTube | Discord | 뉴스레터
- 다른 플랫폼 방문하기: CoFeed | Differ
- PlainEnglish.io에서 더 많은 콘텐츠를 만나보세요.