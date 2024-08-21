---
title: "AI 올인 컴퓨터 비전으로 포커 라이브스트림 변신하기"
description: ""
coverImage: "/assets/img/2024-08-21-All-inonAITransformingaPokerLivestreamwithComputerVision_0.png"
date: 2024-08-21 17:06
ogImage: 
  url: /assets/img/2024-08-21-All-inonAITransformingaPokerLivestreamwithComputerVision_0.png
tag: Tech
originalTitle: "All-in on AI Transforming a Poker Livestream with Computer Vision"
link: "https://medium.com/@tom_pitts/all-in-on-ai-transforming-a-poker-livestream-with-computer-vision-3ed7834706aa"
isUpdated: true
updatedAt: 1724246089402
---


![이미지](/assets/img/2024-08-21-All-inonAITransformingaPokerLivestreamwithComputerVision_0.png)

팬데믹 기간에는 산프란시스코의 도보로 다니는 거리를 떠나 라스베이거스의 먼지 투성이 교외로 이사했어요. 라스베이거스는 삶을 좀 더 천천히 즐길 수 있도록 도와주었지만 이 신 시티로 이사 온 것은 청년 시절에 가진 취향 중 하나, 포커에 다시 불을 지펴주었어요.

포커는 전략, 수학, 그리고 심리가 멋진 조화를 이뤄내며, 현실 돈을 걸었을 때 생기는 살짝의 아드레날린까지 함께 제공합니다. 지난 일 년 동안, 저의 포커 게임은 주로 유튜브와 인스타 그램에 방송되는 Absolute Nuts와 같은 게임에서 진행되었어요.

대부분의 생방송 포커 게임-월드 시리즈 오브 포커나 허슬러 카지노 라이브 같은-은 프로 방송을 위해 RFID가 지원되는 테이블과 카드를 사용합니다.

<div class="content-ad"></div>

아래는 마크다운 형식으로 변경하였습니다.


![All-inonAITransformingaPokerLivestreamwithComputerVision_1](/assets/img/2024-08-21-All-inonAITransformingaPokerLivestreamwithComputerVision_1.png)

이 설정은 프로덕션 크루와 전문 소프트웨어가 필요하여 카드, 베팅 및 기타 액션을 표시합니다. Absolute Nuts 크루는 더 낮은 프로덕션 스트림입니다. 그들은 기본적으로 핸드폰으로 게임을 스트리밍하기 시작하였고 현재는 몇 개의 카메라를 보유하고 있습니다.

![All-inonAITransformingaPokerLivestreamwithComputerVision_2](/assets/img/2024-08-21-All-inonAITransformingaPokerLivestreamwithComputerVision_2.png)

게임에 참여하는 것을 정말 즐깁니다만, 스트림 시청은 도전적일 수 있습니다. 카드의 시각적 표시가 명확하지 않으면 뷰어들이 종종 액션을 따르는 데 어려움을 겪을 수 있습니다.


<div class="content-ad"></div>

그때 갑자기 깨달았어요: 컴퓨터 비전을 활용해서 스트림을 더 업그레이드할 수 있을까? LLMs로 코딩하는 것이 훨씬 쉬워지고 있으며 컴퓨터 비전 모델도 진보하고 접근하기 쉬워지고 있어요.

세상이 ChatGPT 응답과 Stable Diffusion 세대에 매료되어 있는 가운데, 개발 속도와 실용적인 적용 가능한 모델의 속도가 조용히 혁명을 일으키고 있어요.

계획은 종이 위에서 간단했지만 실제로는 까다로웠어요. 기존 카메라를 사용해서 테이블 위의 카드를 모두 인식하고, 그 정보를 실시간으로 스트림에 삽입하는 것이었죠. 필요한 것은 약간의 카페인과 디버깅 중에 분노하지 않는 것 뿐이었어요.

![이미지](/assets/img/2024-08-21-All-inonAITransformingaPokerLivestreamwithComputerVision_3.png)

<div class="content-ad"></div>

Floptician을 개념화하고 훈련 데이터 생성하기

Floptician이라고 명명한 프로젝트의 첫 번째 단계는 훈련 데이터를 생성하는 것이었습니다.

최근 Resorts World에서 하는 게임을 플레이했는데, 그들의 플레잉 카드는 Faded Spades Classic의 브랜드 버전이라는 것을 알고 있습니다. 프로젝트를 시작하기 위해 이 카드를 구입하고 스캔했습니다. 각 카드 이미지를 상대적으로 깔끔하고 투명한 배경으로 추출하는 간단한 스크립트를 작성했습니다.

그런 다음 여러 크기와 각도로 카드를 다양한 배경에 배치하는 스크립트를 작성하여 약 1,000개의 다른 이미지를 만들었습니다. 이제 컴퓨터 비전 모델에 대한 훈련 데이터를 보유했습니다.

<div class="content-ad"></div>

Floptician 컴퓨터 비전 모델 훈련

훈련 데이터가 준비되었으니, 이제 컴퓨터 비전 모델을 훈련하는 작업에 돌입했습니다. 저는 빠르고 정확성으로 유명한 인기있는 AI 기반 객체 감지 알고리즘인 YOLO (You Only Look Once) v8를 선택했습니다. YOLO는 한 번에 여러 객체를 식별하는 데 능숙하여 실시간 비디오 처리에 적합합니다.

모델을 훈련하는 과정은 다양한 위치와 조명 조건의 레이블이 달린 카드 이미지 데이터셋을 주입하는 것이었습니다. 몇 차례의 훈련과 조정 끝에 정확도가 뛰어나고 속도는 괜찮은 모델을 얻을 수 있었습니다. 더 나은 성능을 끌어낼 수 있었을까요? 아마도 그럴 수도 있습니다. 그러나 '이 정도면 충분하다'고 말할 때를 아는 것이 중요합니다.

![포커 라이브 스트림을 컴퓨터 비전으로 변형하는 중](/assets/img/2024-08-21-All-inonAITransformingaPokerLivestreamwithComputerVision_4.png)

<div class="content-ad"></div>

훈련된 모델이 준비되었으니, 다음 과제는 실시간 스트림 오버레이에 통합하는 것이었습니다.

프로토타입에서 제품으로: Python 앱 구축

Floptician의 핵심 과제는 비디오 프레임을 매끄럽게 캡처하고 컴퓨터 비전을 사용하여 분석하고 그 결과를 실시간으로 OBS 오버레이로 출력할 수 있는 시스템을 만드는 것이었습니다. 이를 위해 데이터 처리와 통신의 조화로운 파이프라인이 필요했습니다.

어플리케이션이 어떻게 작동하는지 더 자세히 살펴보겠습니다:

<div class="content-ad"></div>

<img src="/assets/img/2024-08-21-All-inonAITransformingaPokerLivestreamwithComputerVision_5.png" />

- 프레임 캡처 및 OBS 통합: 앱은 OBS 웹소켓 API를 사용하여 OBS와 통합되어 스트리밍 소프트웨어와의 원활한 상호 작용을 허용합니다. 최적의 성능을 위해 웹캠(또는 Windows의 가상 카메라)에서 프레임을 직접 캡처하여 OBS API보다 빠른 속도를 제공했습니다.
- 프레임 처리 및 카드 감지: 캡처된 프레임은 YOLO v8 모델에 공급되어 카드 감지를 수행합니다. BoardProcessor 및 CommunityCardDetector 구성요소는 YOLO 출력을 분석하여 유효한 포커 커뮤니티 카드 레이아웃을 식별합니다. 이 단계에서는 오류가 있는 카드 감지나 관련 없는 감지를 걸러내어 실제 커뮤니티 카드만 처리하도록 합니다. 시스템은 핸드의 진행을 추적하며 턴과 리버에서 새로운 카드를 추가합니다.
- 상태 관리: BoardProcessor는 커뮤니티 카드의 현재 상태를 유지하며 핸드의 다른 단계(프리플랍, 플랍, 턴, 리버) 간의 전환을 처리합니다. 이 견고한 상태 관리는 카드가 일시적으로 가려지거나 잘못 감지된 경우에도 시스템이 정확성을 유지할 수 있게 합니다.
- 실시간 통신: 감지된 카드 정보는 실시간으로 웹소켓 서버로 전송됩니다. 이 서버는 파이썬 백엔드와 프론트엔드 오버레이 간의 다리 역할을 수행합니다.
- HTML/JavaScript 오버레이: 사용자 정의된 HTML 페이지는 웹소켓 서버에서 카드 데이터를 받습니다. 이 페이지는 커뮤니티 카드를 동적으로 렌더링하여 전문 포커 방송의 세련된 모습을 모방합니다. 오버레이는 OBS에 브라우저 소스로 로드되어 비디오 스트림과 원활하게 통합됩니다.

도전과 해결책

- 동적 카드 레이아웃 감지: 우리는 다양한 게임 단계(플로프, 턴, 리버) 및 “Chihuahua”와 같은 특수 레이아웃에서 유효한 커뮤니티 카드 구성을 식별해야 했습니다. 카드 감지기를 구현하여 감지된 카드를 행으로 그룹화하고 그들의 공간적 관계를 분석하여 유효한 포커 보드를 인식하도록 하여 물리적 배치에 관계없이 감지된 카드를 정확히 해석합니다.
- 부분 가리기 강건성: 포커 딜러는 종종 커뮤니티 카드 일부를 실수로 가립니다. 이를 처리하기 위해 보드의 현재 알려진 상태를 유지하고 변경에 대한 높은 확신이 있을 때에만 업데이트하는 상태 머신을 구현했습니다.

<div class="content-ad"></div>


![image](/assets/img/2024-08-21-All-inonAITransformingaPokerLivestreamwithComputerVision_6.png)

Results and Future Improvements

![image](https://miro.medium.com/v2/resize:fit:640/1*i_skKte_dsfBMMPfzbzFsg.gif)

Last week I unveiled the Floptician, and we used it live on the stream. While I noticed a few areas for improvement, it worked remarkably well, and everyone was excited by the improved stream graphics.


<div class="content-ad"></div>

여기 개선 아이디어 몇 가지가 있어요:

- 모델 최적화: 작은 객체 탐지 모델을 사용하면 정확도를 크게 희생하지 않고 성능 또는 FPS를 개선할 수 있을 것 같아요.
- 플레이어 핸드 탐지: 쇼다운 시 플레이어 핸드를 탐지하고 표시하는 시스템을 확장하는 것은 멋질 것 같아요. 다만 객체 탐지와 교육에는 다른 접근 방식이 필요할 수도 있어요.
- 핸드 랭킹: 포커 핸드 랭킹을 포함할 수도 있어요. 쇼다운에서 커뮤니티 카드와 플레이어 핸드를 모두 감지한 후 가능할 거예요.

Floptician이 Absolute Nuts 라이브 스트림을 업그레이드하는 걸 보니 너무 기대돼요. 이 프로젝트는 저에게 큰 교훈을 주었어요. 문제 해결이 얼마나 접근하기 쉬워졌는지를 보여주는 증거죠.

LLMs를 사용한 코딩 지원, 현대적인 라이브러리, 사전 훈련된 모델의 조합으로 야심찬 프로젝트에 대한 진입 장벽이 크게 낮아졌어요. 전문가들의 팀이 여러 달 걸렸을 법한 일이, 저는 끈기를 조금 가지고 주의주의에 적극적으로 연구한 결과 몇 주만에 프로토타입을 만들었답니다.

<div class="content-ad"></div>

플로프티션의 코드는 GitHub에서 확인할 수 있어요.

최초 발행된 곳: https://www.linkedin.com.