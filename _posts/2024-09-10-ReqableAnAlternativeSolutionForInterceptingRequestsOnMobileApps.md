---
title: "Reqable - 모바일 앱에서 요청을 가로채는 대안적인 솔루션"
description: ""
coverImage: "/assets/img/2024-09-10-ReqableAnAlternativeSolutionForInterceptingRequestsOnMobileApps_0.png"
date: 2024-09-10 19:43
ogImage: 
  url: /assets/img/2024-09-10-ReqableAnAlternativeSolutionForInterceptingRequestsOnMobileApps_0.png
tag: Tech
originalTitle: "Reqable  An Alternative Solution For Intercepting Requests On Mobile Apps"
link: "https://medium.com/@_betterversion/reqable-an-alternative-solution-for-intercepting-requests-on-mobile-apps-fb4d5e06f360"
isUpdated: false
---


![이미지](/assets/img/2024-09-10-ReqableAnAlternativeSolutionForInterceptingRequestsOnMobileApps_0.png)

네트워크 트래픽을 분석하기 위해서 요청을 가로채는 것은 모바일 펜테스팅과 악성 코드 분석의 중요한 부분입니다. 보통, 나는 이 트래픽을 캡처하고 분석 및 펜테스팅을 위해 버프 스위트를 통해 라우팅하는 데 ProxyDroid를 사용합니다. ProxyDroid는 대부분의 트래픽을 가로채고 분석하는 데 도움을 주며, 보안 취약점을 식별하고 모바일 애플리케이션을 효과적으로 테스트하는 데 도움이 됩니다.

그러나 ProxyDroid는 많은 모바일 앱에서 데이터 저장 및 동기화를 위해 사용되는 인기 있는 Firebase로 전송된 트래픽을 캡처하지 못합니다. 이는 Firebase와 관련된 보안 취약점을 식별하고 해결하기 어렵게 만들어 분석 및 테스트 과정에 공백을 만듭니다.

이 문제를 해결하기 위해 대안 솔루션인 Reqable을 찾았습니다.

<div class="content-ad"></div>

Reqable은 Firebase로 전송된 트래픽을 포함해 모든 종류의 트래픽을 캡처하고 분석할 수 있는 강력하고 유연한 도구입니다. Reqable을 펜테스팅 프로세스에 통합함으로써 분석 범위를 확대하고 네트워크 트래픽을 누락하지 않도록 보장할 수 있습니다.

# 설정

Reqable을 설치하는 것은 매우 간단합니다. 여기서는 Reqable을 설치하는 환경으로 WSA (Windows Subsystem for Android™️)를 사용합니다. Reqable은 공식 웹사이트(https://reqable.com/en-US/download)에서 다운로드할 수 있습니다.

먼저, 애플리케이션을 다운로드하고 실행하세요.

<div class="content-ad"></div>


![이미지](/assets/img/2024-09-10-ReqableAnAlternativeSolutionForInterceptingRequestsOnMobileApps_1.png)

이후에 이용 약관을 수락하세요. Reqable은 두 가지 운영 모드를 제공하며, 여기서는 스탠드얼론 모드를 사용 중입니다.

![이미지](/assets/img/2024-09-10-ReqableAnAlternativeSolutionForInterceptingRequestsOnMobileApps_2.png)

다음으로, 트래픽을 캡쳐하려면 Reqable Magic Service (RMS)를 설치해야 합니다. 간단히 RMS 서비스를 다운로드하여 설치하면 설정이 완료됩니다.


<div class="content-ad"></div>

아래는 작업이 완료되면 표시되는 화면입니다:

<div class="content-ad"></div>


![Step 5](/assets/img/2024-09-10-ReqableAnAlternativeSolutionForInterceptingRequestsOnMobileApps_5.png)

Continue by installing the certificate to decrypt HTTPS traffic. Click the yellow button at the bottom right; the application will prompt you to install the certificate.

![Step 6](/assets/img/2024-09-10-ReqableAnAlternativeSolutionForInterceptingRequestsOnMobileApps_6.png)

![Step 7](/assets/img/2024-09-10-ReqableAnAlternativeSolutionForInterceptingRequestsOnMobileApps_7.png)


<div class="content-ad"></div>


![이미지](/assets/img/2024-09-10-ReqableAnAlternativeSolutionForInterceptingRequestsOnMobileApps_8.png)

애플리케이션은 다양한 기기 유형에 대한 설치 안내서를 제공합니다. WSA의 경우 빠른 설치를 위해 Magisk를 사용합니다. 패키지를 다운로드한 후 Magisk를 통해 모듈을 간단히 설치하세요.

![이미지](/assets/img/2024-09-10-ReqableAnAlternativeSolutionForInterceptingRequestsOnMobileApps_9.png)

또는 WSA에 이미 Burp의 인증서가 설치되어 있는 경우 Burp의 인증서를 가져올 수도 있습니다.


<div class="content-ad"></div>

만약 모든 것이 설정되었다면, 단순히 트래픽을 캡처하기 위해 오른쪽 하단에 있는 노란색 버튼을 클릭하세요.

이러한 단계를 거쳐 Reqable을 설치하고 네트워크 트래픽을 캡처하고 분석할 수 있게 되었습니다. 이는 펜테스팅과 보안 분석의 효과를 향상시킵니다.

<div class="content-ad"></div>

# 트래픽 캡처

여기서 안드로이드 애플리케이션을 사용하여 Firebase를 통해 트래픽을 캡처하는 테스트를 진행했습니다. Reqable은 Firebase 요청을 성공적으로 캡처했는데, 이는 ProxyDroid가 할 수 없었던 작업입니다 😦.

![이미지](/assets/img/2024-09-10-ReqableAnAlternativeSolutionForInterceptingRequestsOnMobileApps_12.png)

Reqable은 안드로이드에서 요청을 캡처하기 위해 프록시 대신 VPN 기반 방법을 사용하여 기존의 프록시 방법으로 처리할 수 없는 Firebase와 같은 요청을 가로챌 수 있습니다.

<div class="content-ad"></div>

Reqable은 연결용 협력 모드가 있는 PC 애플리케이션도 제공합니다. 이 모드에서 Android에서 모든 트래픽이 PC로 라우팅되어 펜테스팅이나 분석이 더 쉽고 편리해집니다. 이를 통해 네트워크 트래픽 분석이 최적화되어 세부 네트워크 요청을 검토하고 보안 취약점을 더 효과적으로 식별할 수 있습니다.

![이미지](/assets/img/2024-09-10-ReqableAnAlternativeSolutionForInterceptingRequestsOnMobileApps_13.png)

Windows, macOS 또는 Linux용 애플리케이션을 다운로드한 후 원격 디바이스 기능을 사용하여 연결하면 됩니다. PC에서 직접 요청을 캡처하고 수정할 수 있습니다.

![이미지](/assets/img/2024-09-10-ReqableAnAlternativeSolutionForInterceptingRequestsOnMobileApps_14.png)

<div class="content-ad"></div>

# 더 많은 내용 보기 🚀 :

- Function calling: The solution for a flexible and efficient RAG system
- Building an RAG system integrated with Function Calling (ChatGPT-4)
- Handbook for Improving Retrieval Augmented Generation (RAG) Applications
- Maximizing the Power of LLM by Using ReAct Agent
- How I Improve My JavaScript Coding Skills Every Day