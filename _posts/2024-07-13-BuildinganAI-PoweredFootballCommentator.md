---
title: "AI가 적용된 축구 해설 시스템 만드는 방법"
description: ""
coverImage: "/assets/img/2024-07-13-BuildinganAI-PoweredFootballCommentator_0.png"
date: 2024-07-13 21:35
ogImage: 
  url: /assets/img/2024-07-13-BuildinganAI-PoweredFootballCommentator_0.png
tag: Tech
originalTitle: "Building an AI-Powered Football Commentator"
link: "https://medium.com/better-programming/building-an-ai-powered-football-commentator-6dbff5af9a88"
isUpdated: true
---




![이미지](/assets/img/2024-07-13-BuildinganAI-PoweredFootballCommentator_0.png)

만약 FIFA나 PES와 같은 게임을 한 적이 있다면, 때로는 사전 녹음된 코멘터리로 인해 원하는 만큼 몰입할 수 없는 경우가 있을 것입니다.

코멘터리가 팀 명단을 읽고 있는데 갑자기 선수의 이름을 소리치며 중단하는 경우가 있습니다.

또는 몇 시즌 동안 여러 차례 동일한 경기 요약을 들었을 수도 있습니다.

<div class="content-ad"></div>

이런 사소한 귀찮음들이 있으면, 이 경험을 개선할 방법이 없을까 궁금해졌어요.

그래서 저는 이 문제를 해결하는 컨셉을 만들어보기로 결심했어요.

원하는 흐름은 다음과 같았어요:

- 대부분 구조화되지 않은 축구 데이터(예: 경기 통계)를 입력하기
- 해당 통계를 사용하여 숙련된 해설자 스타일로 생성된 해설 스크립트를 가지고 있기
- 그 후 실제 같은 소리를 내며 생성된 스크립트를 읽어주는 목소리가 있는 것입니다.

<div class="content-ad"></div>

이 프로젝트를 위해 몇 가지 다른 기술을 선택했습니다:

- 프론트엔드 및 일부 API에는 React/Next.js를 사용했습니다 (React로 구축해 본 적이 없어요. Angular 개발자로서 React가 무슨 소란인지 궁금해서 선택했습니다)
- 코멘터리 생성에는 GPT 3.5 Turbo를 사용했습니다. 세대에서 놀라운 성과를 보여주며 토큰 생성 비용이 저렴했습니다.
- ElevenLabs 텍스트 음성 변환 서비스를 사용해 실제 음성이 합성 로봇 음성이 아닌 것처럼 생성했습니다.

구축은 실제로 놀랍게도 상당히 간단했어요. 특히 React/Next.js에 초보자인 저에게는요. 저는 약 하루 반 정도의 시간을 들여 완전하고 작동하는 컨셉 증명을 만들었습니다.

만약 코드를 탐색해 보시려면 프로젝트를 오픈 소스로 만들었어요. 직접 실행하려면 OpenAI와 ElevenLabs 도구의 API 키, 그리고 ElevenLabs의 음성 ID가 있는 .env.local 파일을 생성해야 합니다.

<div class="content-ad"></div>

여기 .env.local 파일 구조에 대한 예시입니다:

```js
OPENAI_API_KEY=
NEXT_PUBLIC_ELEVEN_LABS_API_KEY=
NEXT_PUBLIC_ELEVEN_LABS_VOICE_ID=
```

프로젝트에 대한 주요 생각은 다음과 같습니다.

![프로젝트 이미지](https://miro.medium.com/v2/resize:fit:1200/1*UUlS1rX8mWQCm2dNcJJ4ZA.gif)

<div class="content-ad"></div>

# 프로덕션 사용 사례에 로컬 LLM이 필요할 수 있습니다

스크립트 생성을 위해 OpenAI의 GPT-3.5 LLM은 매우 정확한 억양과 적절한 열정으로 훌륭한 결과물을 만들어 냈습니다. 그러나 API 호출은 완료하는 데 상당한 시간이 소요되었습니다. 실제로 저는 어플리케이션을 Vercel이나 Netlify에 데모로 배포했을 때, 둘 다 요청에 대한 시간 초과를 걸고 추가 유료 업그레이드(또는 때로는 지원팀과의 대화)가 필요했습니다.

이 시간 지연은 실시간 게임에 자동 코멘터리를 주입하고 싶다면 충분하지 않을 것으로 예상됩니다. 그러나 경기가 끝난 후 뉴스 보도나 인터뷰와 같은 컷씬에서 사용할 수 있습니다.

코멘터리가 더 입체적으로 느껴지도록 만든다면, 더 빠르게 반응할 수 있는 로컬 호스팅된 LLM이 필요할 것으로 상상합니다.

<div class="content-ad"></div>

하지만 저는 이미 유명한 GitHub 저장소 중 일부가 그 기능을 소개하고 있다는 것을 보고 멀지 않은 곳에 지역 LLMs를 보게 될 것이라고 생각합니다.

# 환각은 여전히 문제입니다

예를 들어, 아스날 여자 축구 경기 결과를 입력했습니다. 제가 제공한 데이터는 꽤 모호했지만, GPT-3.5가 그냥 아무렇게나 만들어 냈단 점이 있었습니다.

예를 들어, 생성된 스크립트에는 원래 경기 데이터에서 제공하지 않은 선수가 언급되었습니다. 이 이름이 어디에서 나왔는지 헷갈렸고, 구글링을 통해 완전히 다른 팀의 선수였음을 알게 되었습니다.

<div class="content-ad"></div>

내가 매치에 대한 더 많은 맥락을 제공했다면 아마이런 상황은 발생하지 않았을텐데, 이 도구의 한계를 테스트하고 좋은 스크립트를 만들기 위해서는 얼마나 많은 도움이 필요한지 알아보고 싶었습니다.

# 목소리를 세밀하게 조정하면 훌륭한 결과물이 나올 것입니다

이 개념 증명을 구축할 때, ElevenLabs의 목소리 디자인 기능을 사용했는데, 중년의 영국 사람이며 뚜렷한 명백한 우굴거림이 있는 목소리를 제공했습니다. 전통적인 축구 해설자로부터 예상할 수 있는 스타일입니다.

그러나 만약 나는 그들의 목소리 클로닝 기능을 사용하고 (그리고 필요한 허가를 가지고 있었다면), 실제 해설자의 목소리를 복제하여 매치의 진정한 느낌을 얻는 것은 매우 간단할 것입니다.

<div class="content-ad"></div>

스포츠 게임인 FIFA나 PES와 같은 맥락에서, 이 음성 복제 및 스크립트 생성 형식을 사용하면 해설자가 오디오 스니펫을 녹음하는 작업을 몇 시간동안하는 것을 대신하여 음성 복제 한 분으로 변환하여 필요 시 오디오를 생성할 수 있습니다.

# 데모

여기 응용 프로그램이 해설을 생성하는 예시가 있습니다. 완벽하지는 않지만, 이것이 두 일 안에 가능하다면, 몇몇 노력을 기울인다면 어떤 가능성이 될 수 있는지 상상해 보세요.