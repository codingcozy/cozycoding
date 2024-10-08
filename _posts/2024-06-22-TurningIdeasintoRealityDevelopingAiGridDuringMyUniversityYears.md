---
title: "아이디어를 현실로 대학 시절 AiGrid 개발 이야기"
description: ""
coverImage: "/assets/img/2024-06-22-TurningIdeasintoRealityDevelopingAiGridDuringMyUniversityYears_0.png"
date: 2024-06-22 04:00
ogImage: 
  url: /assets/img/2024-06-22-TurningIdeasintoRealityDevelopingAiGridDuringMyUniversityYears_0.png
tag: Tech
originalTitle: "Turning Ideas into Reality: Developing AiGrid During My University Years"
link: "https://medium.com/@dizzpy/turning-ideas-into-reality-developing-aigrid-during-my-university-years-e3c43056e8fd"
isUpdated: true
---






![Image](/assets/img/2024-06-22-TurningIdeasintoRealityDevelopingAiGridDuringMyUniversityYears_0.png)

# 소개

대학생으로서 AiGrid를 개발하는 것은 저에게 놀라운 경험이었습니다. 저는 항상 프로젝트를 통해 지식을 공유하고 다른 사람들을 돕는 것을 원했습니다. 어느 날, 모든 AI 도구를 한 곳에 모은 플랫폼을 만들어보자는 아이디어가 떠올랐습니다. 인터넷에는 이와 유사한 많은 플랫폼이 있지만, 저는 독특한 기능을 갖춘 나만의 플랫폼을 만들고 싶었습니다. 이 프로젝트는 제 기술적 역량을 향상시킬 뿐만 아니라 프로젝트 관리와 사용자 중심의 디자인에 관한 소중한 통찰력을 제공하여 제 경력에서 중요한 이정표를 만들었습니다.

# 작동 방식


<div class="content-ad"></div>

사용자들은 이미지 생성, 텍스트 생성 등 다양한 기능을 위한 AI 도구를 탐색할 수 있습니다. 게다가, 사용자들은 플랫폼에 새로운 도구를 기여할 수도 있습니다. 사용자가 사이트에 새로운 도구를 제출하면, 저는 관리 앱에서 해당 제출을 받게 됩니다. 그 후에, 제출을 승인하거나 거절할 수 있습니다. 승인되면, 새로운 도구가 웹사이트에 표시됩니다.

# 컨셉

이 아이디어는 TikTok을 스크롤하면서 떠올랐어요. 사람들이 새로운 AI 도구를 공유하는 것을 보고, "왜 이러한 도구들이 모두 한 곳에서 찾아볼 수 있는 장소를 만들지 않았을까?" 라는 생각을 했습니다. 사용자들은 플랫폼에 새로운 도구를 추가함으로써 이에 기여할 수도 있습니다.

# 계획

<div class="content-ad"></div>

초기 계획 단계에서는 HTML, CSS 및 JavaScript을 사용하여 간단한 웹 인터페이스를 만들었습니다. 도구 데이터가 포함된 JSON 파일을 호스팅했고, 새로운 도구 제출은 이메일을 통해 수신하며, 직접 JSON 파일에 추가했습니다. 이 프로세스는 혼란스럽고 처리하기 어려웠습니다. 더 나은 솔루션을 위해 약 일주일 정도 계획을 하고, Eraser.io를 사용하여 프로젝트 계획을 하게 되었습니다. 그래서 기본 사이트를 ReactJS로 변환하기로 결정했습니다.

ReactJS 및 UI 프레임워크로 Tailwind CSS를 사용하여 웹 사이트를 개발했습니다. Tailwind를 배워야 했기 때문에 이 프로젝트를 기회로 삼았습니다. 데이터베이스로는 제 요구 사항에 맞는 현대적인 솔루션인 Firebase Firestore를 선택했습니다. 또한 관리자 패널을 작성하고, Flutter 및 Firebase Auth를 사용하여 Android 및 iOS 애플리케이션을 개발했습니다. 이제 사용자가 사이트를 통해 새 도구를 제출하면, 앱에 나타나서 승인 또는 거부할 수 있습니다. 앱에서 데이터베이스의 모든 도구를 관리할 수 있습니다.

# 개발 과정

![이미지](/assets/img/2024-06-22-TurningIdeasintoRealityDevelopingAiGridDuringMyUniversityYears_1.png)

<div class="content-ad"></div>

환경 설정하기

저는 React를 사용하여 프론트엔드, Firebase를 사용하여 데이터베이스를 포함한 필수 도구 및 라이브러리를 갖춰 개발 환경을 설정하기 시작했습니다. 모바일 앱의 경우, Material Design 3과 함께 Flutter와 Dart를 사용했습니다. 코드 관리를 위해 GitHub를 통한 버전 관리 설정은 중요한 요소였습니다.

UI/UX 디자인하기

직관적이고 매력적인 UI/UX를 디자인하는 것이 우선이었습니다. 제가 Wireframing과 Prototyping에 Figma를 사용하여 인터페이스가 사용자 친화적이고 시각적으로 매력적인지를 보장했습니다. 디자인 반복 과정 중 사용자 피드백을 통해 플랫폼 전체적인 외관과 느낌을 정제하는 데 도움이 되었습니다.

<div class="content-ad"></div>

# 미래 개선 사항

앞으로는 리더보드, 사용자 계정 등 더 많은 기능을 추가할 계획입니다.

# 이 프로젝트에서 배운 점

이 프로젝트를 통해 개인으로 프로젝트를 처음부터 끝까지 계획하고 실행하는 방법을 배웠습니다. ReactJS 및 Tailwind를 사용하여 웹 개발에서 경험을 쌓았으며 Firebase Firestore를 웹 및 모바일 애플리케이션에 연결하고, Flutter를 사용하여 크로스 플랫폼 앱을 만들고 GitHub를 통해 버전 관리를 하는 방법을 익혔습니다.

<div class="content-ad"></div>

AiGrid을 탐험하고 피드백을 공유해보세요. 플랫폼을 향상시키는 데 귀중한 제안이 될 것이며, AI 도구 및 자원의 주요 목적지로 만드는 데 도움이 될 것입니다.

Dizzpy | 즐거운 코딩! 🖥️🥰