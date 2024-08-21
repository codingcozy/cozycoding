---
title: "SafeLine Community Edition 동적 보호 기능 출시"
description: ""
coverImage: "/assets/img/2024-07-23-SafeLineCommunityEditionreleasesdynamicprotectioncapabilities_0.png"
date: 2024-07-23 21:30
ogImage: 
  url: /assets/img/2024-07-23-SafeLineCommunityEditionreleasesdynamicprotectioncapabilities_0.png
tag: Tech
originalTitle: "SafeLine Community Edition releases dynamic protection capabilities"
link: "https://dev.to/lulu_liu_c90f973e2f954d7f/safeline-community-edition-releases-dynamic-protection-capabilities-k9p"
isUpdated: true
---




SafeLine은 채이틴 테크가 10년 동안 섬세하게 개발한 WAF입니다. 그 핵심 탐지 능력은 지능적인 의미 분석 알고리즘으로 구동되며, 전문가들 사이에서 높은 인정을 받고 있어요. SafeLine Community Edition은 기업용 Ray Shield 제품의 파생 프로젝트입니다. 대형 전문 기업을 대상으로 한 복잡한 기능을 줄이고, 하드웨어 요구 사항을 낮추며, 사용법을 간소화하여 커뮤니티를 위해 특별히 설계된 무료 WAF 제품입니다.

![SafeLine Community Edition 이미지](/assets/img/2024-07-23-SafeLineCommunityEditionreleasesdynamicprotectioncapabilities_0.png)

동적 보호
동적 보호는 사용자가 볼 수 있는 콘텐츠를 변경하지 않고 웹 페이지에 동적 특성을 부여하는 것을 말합니다. 정적 페이지조차도 동적 무작위성을 갖게 됩니다. SafeLine이 처리하는 모든 웹 코드는 역방향 프록시로 동적으로 암호화되고 보호됩니다. 동적 보호는 다음과 같은 여러 효과를 달성할 수 있습니다:

- 프론트엔드 코드의 개인 정보 보호
- 크롤링 행위 차단
- 취약점 스캔 방지
- 악용 공격 방지...

<div class="content-ad"></div>

다이내믹보호 예시 - HTML
아래 이미지는 웹사이트의 일반 HTML이 어떻게 생겼는지 보여줍니다.

![Normal HTML](/assets/img/2024-07-23-SafeLineCommunityEditionreleasesdynamicprotectioncapabilities_1.png)

SafeLine의 다이내믹 보호를 거친 후, 위에서 언급한 HTML 코드는 아래 이미지와 같이 암호화됩니다.

![Encrypted HTML](/assets/img/2024-07-23-SafeLineCommunityEditionreleasesdynamicprotectioncapabilities_2.png)

<div class="content-ad"></div>

다이나믹 보호 예시 - JavaScript
또 다른 예시를 살펴봅시다. 아래 이미지는 웹 사이트의 일반 JavaScript가 어떻게 보이는지 보여줍니다.

![Normal JavaScript](/assets/img/2024-07-23-SafeLineCommunityEditionreleasesdynamicprotectioncapabilities_3.png)

SafeLine의 다이나믹 보호를 통과한 후, 위에서 언급한 JavaScript 코드는 아래 이미지처럼 암호화됩니다.

![Encrypted JavaScript](/assets/img/2024-07-23-SafeLineCommunityEditionreleasesdynamicprotectioncapabilities_4.png)

<div class="content-ad"></div>

동적 보호를 활성화한 후에는 웹 사이트의 HTML 및 JavaScript 코드가 각 방문마다 다른 무작위 형식으로 동적으로 암호화됩니다. 이는 크롤러 및 자동 공격 악용 프로그램을 효과적으로 차단할 수 있습니다.

동적 보호 예제 - 크롤러
크롤러가 대상 웹 사이트에서 중요 정보를 대량으로 추출하는 작업을 담당하고 있다고 가정해 봅시다. 크롤러의 일반적인 설계 접근 방식은 다음과 같습니다:

- http://ct.cn/info?id=666와 같은 중요 정보가 포함된 웹 페이지 찾기
- 웹 페이지 콘텐츠를 가져오기 위해 자동으로 요청 보내기
- HTML 구조를 구문 분석하여 페이지에서 핵심 정보 추출
- 더 많은 정보를 얻기 위해 ID를 탐색

동적 보호를 활성화한 후에는 웹 페이지 구조가 완전히 무작위화되어 크롤러의 작업을 방지합니다.

<div class="content-ad"></div>

동적 보호 예시 - 웹 취약성 스캐너
웹 취약성 스캐너가 있다고 가정해 봅시다. 보통 스캐닝 원칙은 다음과 같습니다:

- 1=1 및 1=2 조건 하에서 웹 페이지 응답 콘텐츠의 일관성을 판단하여 SQL 인젝션 취약성을 감지합니다.
- 웹 페이지 응답 콘텐츠가 페이로드의 특정 문자를 포함하는지 확인하여 RCE 취약성을 감지합니다.
- 웹 페이지 응답 콘텐츠에 오류 메시지나 민감한 정보가 포함되어 있는지 확인하여 정보 노출을 감지합니다.
- 로그인 시도에 성공한 경우와 실패한 경우의 응답 콘텐츠의 일관성을 평가하여 브루트 포싱을 시도합니다.

동적 보호를 활성화한 후, 웹 페이지 응답 콘텐츠는 각 방문마다 서로 다른 무작위 형태로 동적으로 암호화되어 스캐너의 판단 논리를 방해하고 취약성 스캔 작업을 방지합니다.