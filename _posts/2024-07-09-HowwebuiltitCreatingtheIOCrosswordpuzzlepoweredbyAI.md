---
title: "우리가 만든 방법 AI로 구동되는 I O 크로스워드 퍼즐 제작기"
description: ""
coverImage: "/assets/img/2024-07-09-HowwebuiltitCreatingtheIOCrosswordpuzzlepoweredbyAI_0.png"
date: 2024-07-09 09:51
ogImage: 
  url: /assets/img/2024-07-09-HowwebuiltitCreatingtheIOCrosswordpuzzlepoweredbyAI_0.png
tag: Tech
originalTitle: "How we built it: Creating the I O Crossword puzzle, powered by AI"
link: "https://medium.com/flutter/how-we-built-it-creating-the-i-o-crossword-puzzle-powered-by-ai-2210e39b04b9"
isUpdated: true
---




클래식 단어 게임에 재미와 도움이 되는 변화를 넣어보세요. 플러터(Flutter), 파이어베이스(Firebase) Genkit 및 Gemini API로! 

![2024-07-09-HowwebuiltitCreatingtheIOCrosswordpuzzlepoweredbyAI_0.png](/assets/img/2024-07-09-HowwebuiltitCreatingtheIOCrosswordpuzzlepoweredbyAI_0.png)

올해 I/O에서 Very Good Ventures는 구글의 플러터(Flutter) 및 파이어베이스(Firebase) 팀과 협력하여 Gemini API의 능력을 소개하는 독특한 디지털 경험을 만들었습니다.

교차 퍼즐의 재미와 도전은 단서를 해결하여 보드를 완성하는 것입니다. 단어와 단서를 위한 자연스러운 출발점은 올해 I/O에서 언급한 모든 것이었습니다. 게임 콘텐츠를 생성하기 위해, 우리는 키포인트 비디오들을 Gemini Advanced에 제공하고 이를 사용하여 이번 I/O에서 발표된 모든것에 대해 재미있게 학습할 수 있는 기술 관련 단어와 단서 목록을 생성하도록 요청했습니다.

<div class="content-ad"></div>

계속 읽어보면 Flutter로 UI를 어떻게 구축했는지 알 수 있고, GitHub에서 접근할 수 있는 게임의 오픈 소스 코드를 확인해보세요.

플레이 방법

퍼즐에 로그인하면 팀을 선택하라는 메시지가 표시됩니다. 단서에 올바르게 답하면 단어가 채워지고 셀이 팀 색상으로 변경됩니다. 단어를 해결할 때마다 팀이 점수를 획득하고, 힌트를 요청하지 않고 단어를 연속으로 해결하면 추가 보너스 점수를 받습니다.

힌트가 필요하신가요? 힌트 버튼을 클릭한 다음, 감춰진 단어에 대해 예/아니오 질문을 최대 열 개까지 할 수 있습니다. Gemini API가 개인적으로 질문에 응답하여 단어를 더 많이 채울 수 있도록 도와줍니다. 당신의 팀이 승리를 도모하는 데 도움이 되는 단어를 더 많이 찾을 수 있습니다!

<div class="content-ad"></div>


![이미지](/assets/img/2024-07-09-HowwebuiltitCreatingtheIOCrosswordpuzzlepoweredbyAI_1.png)

힌트 기능의 디자인은 다양한 기술이 함께 작동하여 문제를 해결하는 좋은 예입니다. 힌트는 Firebase Genkit으로 작동되며, 이는 I/O에서 발표된 AI 개발을 위한 새로운 프레임워크이며, Firebase Function으로 배포됩니다.

API로의 네트워크 요청에는 답을 포함하고 있으므로 클라이언트에서 응답을 숨기기 위해 간단한 Dart 백엔드를 구축했습니다. 이를 위해 Dart Frog 패키지를 사용하여 더 견고한 경험을 제공했습니다. Frog 백엔드는 Genkit을 호출하여 힌트를 얻고 데이터베이스에서 답을 검색합니다. 이렇게 하면 플레이어가 게임의 답을 그냥 네트워크 호출을 검사하여 알아내지 못하게 됩니다.

Genkit 흐름이 어떻게 구축되었는지에 대해 Firebase 심층 분석 블로그에서 자세히 읽을 수 있습니다.


<div class="content-ad"></div>


![Crossword Board](/assets/img/2024-07-09-HowwebuiltitCreatingtheIOCrosswordpuzzlepoweredbyAI_2.png)

플러터로 성능이 우수한 크로스워드 보드 렌더링하기

보드는 게임의 핵심 부분 중 하나입니다. 퍼즐에서 플레이어의 이동을 최적화하여 성능을 향상시키고 플레이어에게 최상의 사용자 경험을 제공하였습니다.

보드를 구축하기 위해 두 가지 옵션을 고려했습니다: 순수한 플러터 또는 Flame 게임 엔진 활용. 이 게임에서 Flame의 가장 매력적인 기능은 카메라 API로, 이를 사용하면 마스코트가 쉽게 움직이고 줌 컨트롤을 지원합니다. 그러나, 우리가 사용할 Flame의 유일한 기능이 그것이었기 때문에, 최종적으로 Flame과 같은 전체 게임 엔진을 사용하는 것은 이 상황에서 지나치게 복잡할 것이라고 결정했습니다.


<div class="content-ad"></div>

대체 솔루션을 찾기 위해 InteractiveViewer 위젯을 탐색했습니다. 이 위젯은 사용자 정의 크기의 캔버스에 단어를 렌더링하고 매트릭스 변환을 사용하여 마스코트를 자유롭게 이동시킬 수 있습니다. InteractiveViewer는 의존성이 적고 부하가 적어 우리가 필요로 하는 간단한 해결책이었으며, Flutter의 유연성과 성능을 빛내는 데 더 나은 방법을 제시할 수 있었습니다.

InteractiveViewer의 강력함과 유연성

InteractiveViewer는 기본 줌 제스처를 갖추고 있지만 더 직관적인 버튼을 데스크톱 환경에서 제공하려 했습니다. 매트릭스 변환을 활용하여 줌 컨트롤을 구현했는데, 먼저 스케일 변경을 계산하고 중심을 고정 참조점으로 사용하여 새로운 뷰포트를 업데이트했습니다:

![이미지](/assets/img/2024-07-09-HowwebuiltitCreatingtheIOCrosswordpuzzlepoweredbyAI_3.png)

<div class="content-ad"></div>

새로운 임시 뷰포트가 결정되면, 보드의 경계 내에 맞는지 확인해야 해요. 뷰포트가 보드보다 크거나 범위를 벗어나 위치할 때 고려해야 할 두 가지 시나리오가 있어요. 주어진 코드에서 보여지듯이, 뷰포트가 보드 내에 맞도록 줌 레벨이나 뷰포트의 위치를 조정하여 변화된 스케일 및 이동을 업데이트해요:


![이미지 설명](/assets/img/2024-07-09-HowwebuiltitCreatingtheIOCrosswordpuzzlepoweredbyAI_4.png)


마지막으로, 변환을 계산하고 InteractiveViewer 컨트롤러에 적용하세요:


![이미지 설명](/assets/img/2024-07-09-HowwebuiltitCreatingtheIOCrosswordpuzzlepoweredbyAI_5.png)


<div class="content-ad"></div>

위 코드로 InteractiveViewer의 줌 컨트롤을 확장하고 뷰포트를 우리의 요구에 맞게 변환했습니다.

Flutter에서 WebAssembly를 활용하여 성능 향상

올해 IO에서 Flutter 커뮤니티를 위한 큰 발표 중 하나는 Flutter 웹 앱을 위한 WebAssembly 지원이었습니다. 전 세계의 플레이어들이 동시에 게임을 즐길 때 성능은 중요한 요소였습니다. 특히 게임의 캐릭터 및 보드 애니메이션에 관련하여 웹어셈블리(Wasm)를 활용하여 성능 병목 현상을 줄이고 부드러운 프레임 속도를 유지했습니다.

![이미지](/assets/img/2024-07-09-HowwebuiltitCreatingtheIOCrosswordpuzzlepoweredbyAI_6.png)

<div class="content-ad"></div>

다트 백엔드 및 클라우드 런: 백엔드와 프론트엔드 간에 코드를 매끄럽게 공유하세요

모든 사용자에게 원활한 게임 경험을 제공하기 위해, 우리의 다트 백엔드는 Dart Frog 패키지로 구축되었으며 Google Cloud Run에 호스팅되어 있습니다. 이를 통해 자동 스케일링 기능을 활용하여 최적의 성능을 유지할 수 있습니다. 게임은 매번 사용자가 생성될 때, 플레이어가 단어를 제출하거나 힌트를 요청할 때와 같이 여러 호출을 수행하므로, 활성 플레이어 수에 관계없이 최상의 성능을 유지할 수 있습니다.

각 단서의 답변을 확인하기 위해 백엔드를 사용함으로써 우리는 크로스워드 퍼즐을 안전하게 지키고 부정 행위를 방지할 수 있었습니다. 구체적으로, 어플리케이션은 플러터 Firestore SDK를 사용하여 정보를 읽지만 데이터베이스는 다트 백엔드로부터만 변경을 허용합니다. 또한, 프론트엔드와 백엔드 사이에 같은 언어인 다트를 사용할 수 있어 빠른 개발이 가능했습니다.

예를 들어, 플레이어 데이터 모델에서 이 패턴이 적용되는 것을 볼 수 있습니다. Dart Frog API를 사용하여 플레이어를 생성합니다.

<div class="content-ad"></div>


![Image](/assets/img/2024-07-09-HowwebuiltitCreatingtheIOCrosswordpuzzlepoweredbyAI_7.png)

The app directly accesses the players leaderboard, reusing the same model and avoiding duplication and desynchronization:

![Image](/assets/img/2024-07-09-HowwebuiltitCreatingtheIOCrosswordpuzzlepoweredbyAI_8.png)

Start playing: Solve the I/O Crossword puzzle!


<div class="content-ad"></div>

혼자서 십자말풀이를 즐기시면서 가로와 세로로 모든 영광을 경험해보세요! 자세한 내용에 관심이 있는 분들은 오픈 소스 코드와 개발자 학습 경로를 확인하여 어떻게 만들어졌는지 살펴보세요. 구글 I/O 요약을 확인하여 올해 발표된 모든 정보에 대해 더 알아보세요!