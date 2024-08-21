---
title: "Riverpod을 사용하는 Flutter 앱의 최고의 폴더 구조 추천"
description: ""
coverImage: "/assets/img/2024-07-09-Bestfolderstructureforflutterappwithriverpod_0.png"
date: 2024-07-09 09:46
ogImage: 
  url: /assets/img/2024-07-09-Bestfolderstructureforflutterappwithriverpod_0.png
tag: Tech
originalTitle: "Best folder structure for flutter app with riverpod"
link: "https://medium.com/devstudio/best-folder-structure-for-flutter-app-with-riverpod-ba72ceb780b3"
isUpdated: true
---




큰 규모의 복잡한 앱을 개발 중이라면, 폴더 구조를 고려하는 것이 중요합니다. 그렇지 않으면 조직되지 않은 코드는 오랜 기간에 걸쳐 진행을 방해할 수 있습니다.

![이미지](https://miro.medium.com/v2/resize:fit:1280/1*H1rNLlDnWtI7yf2WqtHbcw.gif)

Flutter의 각 상태 관리 라이브러리에는 Riverpod를 포함한 여러 폴더 구조 패턴이 있습니다. 그러나 우리가 공유하는 이 패턴은 이해하기 쉽고 테스트하고 디버그하고 확장하기 쉽습니다. 또한 다른 구조에서 쉽게 이전할 수 있도록 하여 코드 정리 및 리팩터링이 쉽습니다. 이것이 실제로 얼마나 간단한지 자세히 살펴보겠습니다.

![이미지](https://miro.medium.com/v2/resize:fit:1280/1*a0ttqJt3hu_wdKU35B3U6A.gif)

<div class="content-ad"></div>

먼저 앱을 기능 단위로 나눠보세요. 예를 들어, 당신의 앱이 네 가지 기능을 갖고 있다면, 구조는 다음과 같을 것입니다:

![앱 구조](/assets/img/2024-07-09-Bestfolderstructureforflutterappwithriverpod_0.png)

앱 레벨의 구조에는 세 가지 레이어가 포함되어 있습니다:

- 프레젠테이션 레이어
- 로직 레이어
- 데이터 레이어

<div class="content-ad"></div>

더 나은 조직을 위해, 각 기능에 대한 구조를 수정해보세요:   

![Best Folder Structure for Flutter App with Riverpod](/assets/img/2024-07-09-Bestfolderstructureforflutterappwithriverpod_1.png)

- Presentations :- 화면에 렌더링되는 위젯 코드가 포함된 폴더입니다.
- States :- 위젯이 렌더링을 다르게 하기 위해 적응하는 모든 상태가 포함된 폴더입니다.
- Notifiers :- 위젯 상태를 관리하기 위한 비즈니스 로직이 들어있는 폴더입니다.
- Services :- 애플리케이션 상태를 관리하기 위한 비즈니스 로직이 들어있는 폴더입니다.
- Models :- 리포지토리에서 나오는 데이터를 공유하기 위한 데이터 모델이 들어있는 폴더입니다.
- Repositories :- 데이터베이스 또는 서버에서 들어오고 나가는 데이터를 처리하는 폴더입니다.
- Data Providers :- 가짜 데이터, 서버 호출 또는 데이터베이스 호출과 같은 모든 데이터 소스가 들어있는 폴더입니다.

![Folder Structure for Flutter App](https://miro.medium.com/v2/resize:fit:996/1*wc22UXuiFzRRT-iX2VtJYw.gif)

<div class="content-ad"></div>

그게 전부에요! 필요한 만큼 기능을 추가하고 필요없는 것은 쉽게 삭제할 수 있습니다. 각 레이어를 개별적으로 테스트하고 문제가 있는 폴더만 디버깅하고 빠르게 해결해 보세요:

![이미지](https://miro.medium.com/v2/resize:fit:996/1*gm3_WY9iYBgAlWE03742vw.gif)

여기 예제를 살펴볼 수도 있습니다. 저희가 차후 게시할 기사에서 여러분을 만나길 희망합니다.