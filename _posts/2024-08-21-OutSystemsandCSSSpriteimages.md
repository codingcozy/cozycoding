---
title: "아웃시스템 및 CSS 스프라이트 이미지 활용하기"
description: ""
coverImage: "/assets/img/2024-08-21-OutSystemsandCSSSpriteimages_0.png"
date: 2024-08-21 16:50
ogImage: 
  url: /assets/img/2024-08-21-OutSystemsandCSSSpriteimages_0.png
tag: Tech
originalTitle: "OutSystems and CSS Sprite images"
link: "https://medium.com/@mariana.m.junges/outsystems-and-css-sprite-images-bbc578711033"
isUpdated: false
---


CSS 스프라이트 이미지는 성능을 최적화하는 기술로, OutSystems 문서에서 최고의 실천 사례로 언급되고 있어요 (여기). 여러 작은 이미지들을 하나의 큰 이미지로 결합하고 CSS를 사용하여 이미지의 특정 부분을 제어함으로써, 개발자들은 서버로 보내는 HTTP 요청 수를 줄일 수 있어요. 이는 페이지 로드 시간을 개선하는 데 특히 중요하며, 느린 네트워크에서 특히 사용자 경험을 향상시킵니다.

OutSystems에서 CSS 스프라이트 이미지를 사용하면 애플리케이션의 프론트엔드 성능을 최적화할 수 있어요. OutSystems를 사용하면 개발자들이 빠르게 복잡한 애플리케이션을 구축할 수 있으므로, 이러한 애플리케이션이 효율적으로로드되도록 하는 것이 중요해요. 스프라이트를 통해 개별 이미지 요청의 수를 줄이면, 로드 시간뿐만 아니라 전체 애플리케이션 성능을 개선할 수 있어요.

CSS 스프라이트를 사용해야 하는 이유:

1. 로드 시간: 이미지를 하나의 스프라이트로 결합하면 서버 요청 수가 줄어들어 사용자의 로드 프로세스가 더 빨라져요.

<div class="content-ad"></div>

2. 최적화된 자원 관리: OutSystems 애플리케이션은 종종 동적 콘텐츠를 포함하며, 스프라이트를 사용하여 그래픽 자원의 전달을 효율적으로 관리하고 최적화할 수 있습니다.

3. 사용자 경험: 빠른 로드 시간은 더 부드럽고 반응성 있는 사용자 인터페이스로 이어지며, 사용자 참여와 만족을 유지하는 데 중요합니다.

4. 간편화된 유지보수: 모든 작은 이미지를 하나의 파일로 결합하면 이미지를 관리하고 업데이트하는 작업이 더 쉬워지며, 장기적으로 복잡성을 줄일 수 있습니다.

프로젝트에 CSS 스프라이트 이미지를 통합하는 것은 응용 프로그램의 성능과 사용자 경험에 크게 기여하는 모베스트 프랙티스입니다.

<div class="content-ad"></div>

OutSystems 문서에서 언급한대로 "이 베스트 프랙티스는 대량의 이미지를 포함하고 있으며 고트래픽 공개 어플리케이션에서만 사용해야 합니다." 

아래에서 CSS Sprite에 관한 텍스트를 읽을 수 있습니다.

![CSS Sprite images](/assets/img/2024-08-21-OutSystemsandCSSSpriteimages_0.png)

CSS Sprite 이미지

<div class="content-ad"></div>

설명

서버 요청 수를 줄이고 화면 로드 시간을 개선하기 위해 여러 이미지를 대체하는 CSS 스프라이트를 사용하세요.

해결책

동일한 화면에 자주 사용되는 이미지들을 하나의 이미지(CSS 스프라이트)로 결합해보세요. CSS 배경 이미지와 배경 위치 속성을 활용하여 원하는 이미지 세그먼트를 표시할 수 있습니다. 이 기술에 대한 자세한 설명은 여기에서 확인하세요.

<div class="content-ad"></div>

중요성

스프라이트를 사용하면 이미지를 가져오는 HTTP 요청 수가 크게 줄어들어 화면 로딩 속도를 높일 수 있습니다.

참고

이 모범 사례는 많은 이미지가 있는 고트래픽 공개 애플리케이션에서만 사용해야 합니다.

<div class="content-ad"></div>

CSS 스프라이트를 생성하고 유지하는 노력은 무시할 수 없는 양이 필요합니다. 더구나 CSS 스프라이트를 사용하는 것은 개별 이미지보다 오류 발생 가능성이 더 높습니다.

출처: OutSystems

알겠어요, 사용 사례를 분석하고 OutSystems에서 한 것처럼 CSS 스프라이트를 사용하는 것이 적절하다고 확인했습니다. 여기 단계별 안내서를 제공합니다:

1. 사용될 모든 이미지를 포함하는 이미지를 만듭니다.

<div class="content-ad"></div>

예를 들어:

![image](/assets/img/2024-08-21-OutSystemsandCSSSpriteimages_1.png)

위 예시에서는 3개의 이미지 파일(하트, 별, 무지개)이 각각 따로 있었지만, 이제는 한 파일에 3개의 이미지가 옆으로 나란히 배치되어 있습니다.

2. 이제 아키텍처에 따라 이 단일 파일을 OutSystems에 있는 해당 모듈 리소스로 가져와야 합니다.

<div class="content-ad"></div>


![이미지](/assets/img/2024-08-21-OutSystemsandCSSSpriteimages_2.png)

3. 그런 다음, 우리는 각 위치에 표시하려는 이미지 부분을 "선택"하는 CSS를 작성해야 합니다.

![이미지](/assets/img/2024-08-21-OutSystemsandCSSSpriteimages_3.png)

![이미지](/assets/img/2024-08-21-OutSystemsandCSSSpriteimages_4.png)


<div class="content-ad"></div>

해당 클래스를 적용하면 이미지가 나타납니다.

한 가지 팁: CSS 스프라이트를 위한 이미지 생성기가 있습니다.

아웃시스템즈 웹사이트의 문서를 읽고 최선의 방법을 따르세요.

자, 당신! 이 기술에 익숙한가요? 이전에 아웃시스템즈에서 CSS 스프라이트를 사용해 보셨나요?

<div class="content-ad"></div>

이미지 성능 최적화 방법은 다양합니다; 이것은 OutSystems의 최고의 실천 방법에 대한 설명입니다.