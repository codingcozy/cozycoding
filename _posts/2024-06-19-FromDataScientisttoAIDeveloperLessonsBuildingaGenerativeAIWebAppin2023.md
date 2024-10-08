---
title: "2023년 AI 개발자가 알아둬야할 내용 정리"
description: ""
coverImage: "/assets/img/2024-06-19-FromDataScientisttoAIDeveloperLessonsBuildingaGenerativeAIWebAppin2023_0.png"
date: 2024-06-19 14:46
ogImage: 
  url: /assets/img/2024-06-19-FromDataScientisttoAIDeveloperLessonsBuildingaGenerativeAIWebAppin2023_0.png
tag: Tech
originalTitle: "From Data Scientist to AI Developer: Lessons Building a Generative AI Web App in 2023"
link: "https://medium.com/towards-data-science/from-data-scientist-to-ai-developer-lessons-building-an-generative-ai-web-app-in-2023-95959a00a474"
isUpdated: true
---





## 수천 명의 사용자를 대상으로 하는 AI 웹 앱을 구축하고자 하는 데이터 과학 열정가를 위한 기술 팁 안내서

![이미지](/assets/img/2024-06-19-FromDataScientisttoAIDeveloperLessonsBuildingaGenerativeAIWebAppin2023_0.png)

만약 대학교나 온라인 강좌 중 하나를 통해 데이터 과학의 세계로 모험을 떴다면, 아마도 ML/AI 소프트웨어 제품을 만들어 사람들이 사용할 수 있는 꿈을 품어 보았을 것입니다. 우리 CS 친구들이 쉽게 코딩하는 것처럼 말이죠.

하지만 풀 스택 웹 개발에 손을 대보면 구성, 배포, 터미널 명령 및 서버 등과 같이 굉장히 어렵게 느껴질 수 있는 장벽에 봉착하게 될 겁니다.

<div class="content-ad"></div>

![2024-06-19-FromDataScientisttoAIDeveloperLessonsBuildingaGenerativeAIWebAppin2023_1.png](/assets/img/2024-06-19-FromDataScientisttoAIDeveloperLessonsBuildingaGenerativeAIWebAppin2023_1.png)

이런 것은 아주 잘 알아요. 도움을 받을 수 없이 시간을 보내다 보니, 저에게 기능이 있는 소프트웨어 앱을 만들어낼 수 없을 거라는 나의 열등감만 깊어졌어요.

그런데 정확히 1년 전인 1월 21일, 여권 문제와 취소된 여행으로 예상치 못하게 여는 주말이 생겼고, AI 앱을 만드는 여행에 나섰어요. 이 여행은 예상치 못한 곳으로 나를 이끌었죠 — 세계 반대편에 있는 공동 창업자와 팀을 이루고, 샌프란시스코 스타트업 가속기에 참가하게 되어, 결국 수천 명의 사용자를 확보하고 상당한 연간 수익을 창출했어요 (우리 앱 'Podsmart!'을 확인해보세요. 우리는 팟캐스트를 요약해요).

![2024-06-19-FromDataScientisttoAIDeveloperLessonsBuildingaGenerativeAIWebAppin2023_2.png](/assets/img/2024-06-19-FromDataScientisttoAIDeveloperLessonsBuildingaGenerativeAIWebAppin2023_2.png)

<div class="content-ad"></div>

하지만 무엇보다 중요한 것은, 그것은 당혹스러운 소프트웨어 개발 세계를 형식적인 컴퓨터 과학/소프트웨어 엔지니어링 배경 없이 탐험하며 고통스러운 여정이었습니다.

따라서 내 첫 번째 소프트웨어 제품을 만들며 지난 일년을 돌아보면, 수천 명의 사용자를 위해 기능적인 웹 앱을 구축하려는 데이터 과학 애호가를 위한 일부 기술적 팁을 모았습니다.

이 가이드는 내가 일 년 동안 겪은 고난과 배움에서 탄생했으며, 1년 전의 나에게 전하고 싶었던 조언을 대변합니다.

참고: 이 팁은 내 개별적인 경험을 바탕으로 한 것이며, 다른 사람들에게는 다르게 작용할 수 있습니다. 또한 여기서 권장된 도구들과는 어떠한 관계나 제휴도 없음을 밝힙니다.

<div class="content-ad"></div>

## 목차

- 원하는 것을 만들고 싶다면
- YouTube 웹 개발 튜토리얼의 위험성
  - 팁 #1: React 대신 Next.js 사용
  - 팁 #2: 스타일링을 위해 Bootstrap 대신 Tailwind CSS 선택
- 데이터 과학 마인드셋의 함정
  - 팁 #3: 백엔드로 Flask 대신 FastAPI 선택하고 응답 모델을 엄밀히 정의
  - 팁 #4: JavaScript 대신 TypeScript 사용
- 배포에 대해
  - 팁 #5: 백엔드에 GPU 백엔드로 모델 사용
  - 팁 #6: 백엔드 배포에 AWS Lambda, 프론트엔드에 Vercel 사용
- 삶을 편하게 만들기
  - 팁 #7: React를 사용하여 직접 랜딩 페이지를 만들지 말기
  - 팁 #8: 사용자 인증 및 결제를 위해 Firebase + Stripe 사용
  - 팁 #9: 에러 모니터링을 위해 Sentry 구현
- 결론

# 원하는 것을 만들고 싶을 때

해당 기능을 갖춘 웹 앱을 만들려면 사용자가 상호 작용할 수 있는 웹 인터페이스(프론트엔드 또는 클라이언트)와 데이터 처리, 데이터 저장, ML/AI 모델 호출을 담당하는 서버(백엔드)가 필요합니다.

<div class="content-ad"></div>

(스트림릿에 대해 들어본 적이 있을 수 있어요. 가장 간단한 데모에 적합하지만, 실제로 사용할 수 있는 제품 앱을 만들기에는 사용자 정의 가능성이 부족합니다)

# 유튜브 웹 개발 튜토리얼의 위험성

데이터 과학자로서 소프트웨어 개발의 여러 측면이 나를 불안하게 만듭니다. 망가진 구성에 몇 일을 소비할 위험을 맞이하는 것과 같은 것들이 그 중 하나에요. 무언가가 고장났는데 왜 고장났는지 알 수 없고 어떻게 고칠지 모르는 것보다 더 답답한 것은 없죠.

결과적으로, React 프로젝트 설정, 백엔드 또는 웹사이트 배포 등 전체 프로세스를 세밀하게 보여주는 유튜브 같은 튜토리얼에 절박하게 의존하게 되었습니다.

<div class="content-ad"></div>

돌아봤을 때, 두 가지 주요 단점이 있었어요:

첫째, 여러 개의 상반된하고 잠재적으로 오래된 튜토리얼로 인한 혼란(예를 들어, 새로운 React 버전이 출시될 때). 이로 인해 종종 더 이상 동작하지 않음을 깨달을 때까지 튜토리얼을 따라가게 되었어요.

둘째, 대부분의 튜토리얼은 초보자를 위한 멋진 수업 데모를 만드는 데 중점을 두고 있어요. 그래서 성능 상한선이 낮은 프레임워크를 사용하며 생산 및 확장성에 부족한 코딩 패턴을 강화하게 되죠. 고개를 돌아보니, YouTube 튜토리얼에서 나쁜 코딩 습관을 많이 털어놓는 모습을 보았어요. 그 습관들은 이제 앱을 수천 명의 사용자에게 제공하는 실제 제품으로 발전하는 데 방해가 되고 있죠.

실패로부터 최고의 교훈을 얻는다고 생각해요. 이러한 프로세스는 짜증 나지만 전해 동안 저에게는 막대한 학습 경험이 되었어요. 아마 나의 실패로부터 배울 때 많은 시간을 절약하실 수 있을 거예요.

<div class="content-ad"></div>

## 팁 #1: React 대신에 Next.js를 사용하세요

![이미지](/assets/img/2024-06-19-FromDataScientisttoAIDeveloperLessonsBuildingaGenerativeAIWebAppin2023_3.png)

많은 YouTube 튜토리얼들이 React를 선호하며, 처음에는 그에 따랐습니다.

그러나 결국 사이트의 SEO(검색 엔진 최적화) 성능을 향상시키고 싶어졌습니다 — 이는 더 많은 사용자 획득에 매우 중요합니다. React의 한계인 메타 태그를 동적으로 변경할 수 없고 서버 측 렌더링이 없는 등의 제약은 귀찮은 작업을 요구하며, Next.js로 변경해야 했습니다. 변경 후에는 성능 차이가 확연히 날밤과 같았습니다.

<div class="content-ad"></div>

![image](/assets/img/2024-06-19-FromDataScientisttoAIDeveloperLessonsBuildingaGenerativeAIWebAppin2023_4.png)

어떤 사람들은 React가 초보자에게 더 친숙하다고 말하지만, Vercel(Next.js 개발자)의 예들을 들 수 있는 등 온라인에는 다양한 Next.js 템플릿이 많습니다. 특히 AI 애플리케이션에 대한 것이죠. Next.js는 실제로 거의 모든 AI 애플리케이션에서 사용되는 현대적인 웹 프레임워크입니다.

## 팁 #2: 스타일링을 위해 부트스트랩 대신 Tailwind CSS를 선택하세요.

프론트엔드 UI 여정을 시작할 때, 처음에는 다른 사람들의 튜토리얼을 따라 부트스트랩으로 향했습니다. 왜냐하면 드롭다운이나 아코디언 같은 준비된 컴포넌트로 쉽게 작업할 수 있다는 유혹이 있었기 때문이죠.

<div class="content-ad"></div>


![이미지](/assets/img/2024-06-19-FromDataScientisttoAIDeveloperLessonsBuildingaGenerativeAIWebAppin2023_5.png)

하지만 잠시 후, 웹사이트가 매우 추려지고 현대적인 AI 데모 페이지와 비교했을 때 정말 추악해 보인다는 것을 깨달았어요. 그 부트스트랩 답은 느낌이 덜어져 있었는데, 그것은 수정이 어려운 일종의 미적 고집이었고, 혼동스럽게 명명된 CSS 클래스들에 뒤얽혀 있었어요. 그래서 결국, 다시 한번 용감을 내어 내 프론트 엔드 전체를 Tailwind CSS로 새롭게 만들기로 했죠. 3일이 걸렸어요.

![이미지](/assets/img/2024-06-19-FromDataScientisttoAIDeveloperLessonsBuildingaGenerativeAIWebAppin2023_6.png)

현대적이고 깔끔한 UI를 가진 AI 데모 페이지를 본 적이 있다면, 그들이 Tailwind CSS를 사용한 것일 확률이 매우 높아요.


<div class="content-ad"></div>


![2024-06-19-FromDataScientisttoAIDeveloperLessonsBuildingaGenerativeAIWebAppin2023_7.png](/assets/img/2024-06-19-FromDataScientisttoAIDeveloperLessonsBuildingaGenerativeAIWebAppin2023_7.png)

처음에는 Tailwind에 겁을 먹었어요. 그 긴 구성 요소 정의는 암호처럼 보이는 유틸리티 클래스들로 넘쳐나 초보자 친화적이지 않아 보였거든요. Tailwind에는 미리 구성된 구성 요소가 부족하고 유틸리티 클래스를 기억하는 것이 어려울 것으로 생각했어요. 그런데 이는 전혀 사실이 아니었어요! Tailwind CSS로 만들어진 많은 훌륭한 UI 컴포넌트 라이브러리가 있어요. 저는 Flowbite React를 사용했어요(필요한 모든 컴포넌트가 다 들어있어요!)

# 데이터 과학 마인드셋의 함정

데이터 과학을 공부하다 보니, 파이썬을 정말 좋아하게 되었어요. 파이썬의 간결하고 강력한 코드 구문이 매력적이었어요. 파이썬의 타입 추론 덕분에 모든 변수에 대해 타입을 정의하는 일에서 벗어날 수 있었어요(이 작업은 특히 자바 같은 기초 컴퓨터 과학 수업에서 만난 언어에서 어려운 작업이었어요).


<div class="content-ad"></div>

그래서 프론트엔드에는 JavaScript를 사용하고 백엔드에는 Python을 사용하여 API 엔드포인트의 유형을 필수적인 경우가 아닌 한 정의하지 않았어요.

그러나 어플리케이션이 복잡해지면서 프론트엔드와 백엔드 간 예상치 못한 다양한 유형 오류로 코딩 생산성이 떨어졌어요. CS 친구들이 명시적 유형의 중요성을 강조하는 이유를 이제야 이해하게 되었어요. 유형 정의에서의 세심함은 까다롭다는 것뿐만 아니라 필수적이라는 것을 깨달았답니다.

## 꿀팁 #3: 백엔드로 Flask 대신 FastAPI를 선택하고 응답 모델을 엄격하게 정의하세요

YouTube에서 파이썬 백엔드 튜토리얼을 검색하면 대부분의 비디오가 Flask를 지목할 것입니다. 하지만 망가진 시계가 하루에 두 번 맞는 것처럼, 나는 우연히도 FastAPI를 Python 백엔드로 선택했는데, 이는 분명히 올바른 결정이었던 것 같아요.

<div class="content-ad"></div>

웃기게도, 나는 FastAPI의 혜택을 완전히 무시했었어요. 얼마 전까지만 해도 POST 요청을 위한 Pydantic 클래스를 정의하는 필요성을 이해하지 못하고 그것을 도움보다는 번거로운 일로 생각했었죠.

FastAPI에는 몇 가지 혁신적인 장점이 있어요:

- 자동 생성된 API 문서 — 이것은 향후 온보딩할 엔지니어들 (또는 미래의 여러분)이 백엔드 구조를 이해하는 데 매우 유용할 거예요!
- 코드 작성이 쉬워짐 — FastAPI는 Json 스키마 위에 구축되어 있기 때문에 라우트를 정의하는 것이 Flask보다 훨씬 쉽고 간결해요 — 결과적으로, 저와 같은 초보자들에게도 학습 곡선이 더 낮아요
- 성능이 더 좋음 — FastAPI는 Flask보다 훨씬 빠르다고 하며 더 적은 메모리를 소비한다고 해요 — 제 앱은 대량의 페이로드를 보내기 때문에 이는 멋진 거에요

![이미지](/assets/img/2024-06-19-FromDataScientisttoAIDeveloperLessonsBuildingaGenerativeAIWebAppin2023_8.png)

<div class="content-ad"></div>

하지만 가장 중요한 것은 FastAPI의 타입 어노테이션이에요!

- FastAPI는 데이터 유효성 검사 라이브러리인 Pydantic 위에 구축되었어요. 이 라이브러리를 사용하면 속성이 있는 클래스로 데이터의 '형태'를 정의할 수 있어요.
- FastAPI를 사용하면 각 API 경로의 입력 및 출력 타입을 Python 타입 힌트와 Pydantic으로 정의된 클래스를 사용하여 주석으로 달 수 있어요.

이것은 각 경로의 출력이 일관된 데이터 구조를 갖도록 확인해줘요. 하지만 이 기능을 최대한 활용하려면...

## 팁 #4: 자바스크립트 대신 TypeScript를 사용하세요

<div class="content-ad"></div>

오랜 시간동안 저는 프론트엔드 페처 메소드를 수동으로 작성해 왔어요(또한 풀 스택 튜토리얼을 참고하여 배웠죠). 그래서 앱에 새로운 루트를 추가하는 것이 오랫동안 오류가 발생하기 쉬운 과정이었어요.

그래서 저의 대규모 기술 업계 소프트웨어 엔지니어 친구가 API 사양을 사용하여 Typescript 클라이언트 코드를 자동 생성할 수 있다고 말할 때, 제가 깜짝 놀랐던 것을 상상할 수 있죠. (더 많은 FastAPI 문서는 여기를 참조하고, 이 중 하나인 openapi-typescript-codegen을 확인해보세요)

한 순간에 이를 깨달았을 때, 이것이 두 가지 중요한 동시에 해결할 것을 알았어요: 수동 및 오류가 발생하기 쉬운 클라이언트 페처 코드 작성을 제거하고, 백엔드와 프론트엔드 간의 유형 일관성을 보장하기. 이렇게 함으로써 앱의 신뢰성을 저해하던 지속적인 유형 오류를 크게 줄였어요.

<div class="content-ad"></div>

물론 백엔드 라우트에 대한 유형 제약 조건을 설정하는 것은 프론트엔드에서 해당 유형 제약 조건을 강제하는 것만큼 중요합니다. 이는 당연히 TypeScript를 필요로 합니다.

그래서 저는 현재 FastAPI 백엔드에 대한 응답 모델을 정의하고 프론트엔드를 JavaScript에서 TypeScript로 변환하는 힘든 과정을 겪고 있어요. 시작부터 FastAPI와 TypeScript로 작업을 시작한다면 이런 과정을 피할 수 있어요!

# 배포 관련...

나의 데이터 과학 / 머신러닝 수업을 통해 구글 Colab를 사용하고 코드를 실행하는 것에 익숙해졌어요. 그래서 배포라는 생각만으로도 공포를 느끼게 되는건 이상하지 않아요. 그러나 Buildspace 가속기의 창립자가 말한 대로, 소프트웨어 앱을 전 세계에 액세스 가능하게 만들려면 “GTFOL” (Get The F Off Localhost)해야 합니다. 그래서 배포 과정이 가능한 쾌적하도록 하고 싶었죠.

<div class="content-ad"></div>

## 팁 #5: GPU 백엔드에 모달 사용하기

만약 여러분이 직접 모델 (예: 머신 러닝 모델, 이미지 인식, Whisper를 위한 필기 인식, 또는 최근에는 오픈 소스 LLMs인 Llama와 같은)을 배포하고 싶다면, 모델을 호스팅할 GPU 클라우드 제공업체가 필요할 것입니다.

내 조언은 Modal을 선택하고 다시 뒤를 돌아보지 말라는 것입니다.

Modal은 최신 애플리케이션용 샘플 코드를 비롯한 최고의 문서 및 학습 자료가 갖춰져 있어요. 오픈 소스 LLMs를 세밀하게 조정해서 사용하거나, LLM 챗봇을 제공하는 것과 같은 다양한 응용 프로그램에 대한 최신 코드가 준비되어 있습니다.

<div class="content-ad"></div>

전체 팟캐스트 전사 앱을 시작했을 때 Modal의 샘플 오디오 전사 코드를 포크하여 시작했어요. 그래서 Modal 없이는 내 앱을 만들지 못했다고 할 수 없을 정도로 중요한 역할을 했어요.

![이미지](/assets/img/2024-06-19-FromDataScientisttoAIDeveloperLessonsBuildingaGenerativeAIWebAppin2023_10.png)

Modal은 사용자 친화성에서 빛을 발해요 (특히 배포를 싫어하는 사람으로서 그렇게 말하고 싶어요). 로컬 코드 편집기에서 클라우드 함수를 작성하고, 한 줄의 터미널 명령으로 배포할 수 있어요. 그 대시보드는 AWS와 비교했을 때 사용자 친화적이어서 앱 사용량을 추적하고 성능을 분석하고 오류를 쉽게 추적할 수 있어요.

마지막으로 Modal은 Lambda가 갖고 있지 않거나 구현하기 귀찮은 기능에 대한 탈출구로 작용해요. 예를 들어 파일 저장 (다음 포인트에서 유용할 것…)과 스케줄링 함수 등에 대한 기능에 사용될 때 특히 유용해요.

<div class="content-ad"></div>

## 팁 #6: 백엔드 배포에는 AWS Lambda, 프론트엔드에는 Vercel 사용하기

Python 백엔드를 호스팅할 때, Amazon EC2 또는 AWS Lambda를 사용해야 하는지 헷갈렸어요. 제 앱은 오디오 파일을 저장해야 하는데 파일이 커질 수 있어서, Lambda의 서버리스 아키텍처는 파일을 저장할 목적으로 디자인되지 않았습니다(2GB의 일시적 저장소가 있지만 무지성이었고 지속적이지 않았습니다). 그래서 Amazon EC2를 사용해야 할 것이라고 생각했죠. 그러나 EC2는 설정이 더 복잡하고 항상 켜져 있는 전용 인스턴스이기 때문에 더 비실측하고 확장하기 어려웠어요.

그런데 Modal의 무료 파일 저장소가 구조를 구성하기에 큰 도움이 되었고, Lambda와 호환되도록 백엔드를 구성하고 필요할 때 파일을 Modal에다 다운로드하고 저장할 수 있게 되었어요.

다행히도, 이 영상은 정말 도움이 되었고, 그들의 지침을 딱 따라하면 백엔드를 성공적으로 배포할 수 있어요.

<div class="content-ad"></div>

내 프론트엔드 작업에는 Vercel만으로도 충분했어요. 과정이 쉽고, 도메인 비용을 제외하고는 완전히 무료였어요.

# 삶이 더 쉬워지는 방법

개발 과정에서 막 애를 먹는 시간을 크게 절약할 수 있는 잡학지식 3가지 팁...

## 팁 #7: React를 사용하여 자체 랜딩 페이지를 구축하지 마세요

<div class="content-ad"></div>

Full-stack 튜토리얼들 때문에 내가 한 또 다른 실수를 해 버렸어. 리액트로 내 랜딩 페이지를 코드로 작성해야 한다고 속아서. 가능하긴 해 (그리고 나도 해 봤지만), 성능과 디자인 측면에서 한계가 있어. 정확히 성공적인 랜딩 페이지를 만들기 위해 필요한 중요한 특징들이야.

릭트는 실제 AI 앱 인터페이스 같은 사용자 정의 기능에만 더 나아. 랜딩 페이지에는 정적 콘텐츠만 있는데, 대신 Webflow나 Framer 같은 노코드 사이트 빌더를 사용해 랜딩 페이지를 신속하게 만들어야 해 (그리고 랜딩 페이지 제작은 디자이너에게 아웃소싱해서 다른 작업에 집중할 수 있게 해야해!)

## 팁 8: Firebase + Stripe로 사용자 인증과 결제 처리하기

사용자 인증에 관해서는 다시 한 번 선택할 수 있는 옵션이 많아서 혼란스러울 수 있어. 나는 사용자 인증을 처리할 뿐만 아니라 사용자 가입 상태에 따라 액세스를 제어하기 위해 결제 시스템과 통합하는 솔루션이 필요했어.

<div class="content-ad"></div>

수 일 동안 다양한 인증 솔루션을 시도하고 실패한 후에 auth0 등을 찾아 사용해보기도 했지만, Stripe + Firebase가 잘 작동한다는 것을 알게 되었습니다. Firebase에는 Stripe 통합이 있어 결제가 성공하면 사용자의 구독 상태를 업데이트하고, Firebase의 React 클라이언트는 클라이언트 측 인증, Python 클라이언트는 서버 액세스 제어를 잘 처리합니다. 두 개의 동영상(여기와 여기)을 따라하면 이를 앱에 성공적으로 구현할 수 있습니다.

## Tip #9: 에러 모니터링을 위해 Sentry 구현하기

몇 달 동안 제 앱을 사용하는 사용자가 만난 버그에 대해 전혀 모르고 있었습니다. 버그를 발견한 후에나 AWS Cloudwatch 인터페이스를 통해 백엔드 버그를 찾으려고 노력합니다.

![이미지](/assets/img/2024-06-19-FromDataScientisttoAIDeveloperLessonsBuildingaGenerativeAIWebAppin2023_11.png)

<div class="content-ad"></div>

내 공동 창업자가 클라우드 앱의 성능 모니터링과 오류 추적을 위한 도구인 Sentry를 소개해 줄 때까지 계속되었습니다. 프론트엔드와 백엔드를 위해 초기화하기 정말 쉽고, Slack과 통합하여 즉시 오류 알림을 받을 수도 있어요. 그저 인증 시간 초과와 같은 사소하지만 빈번한 오류로 무료 플랜의 월별 오류 예산을 소진하지 않도록 주의하세요. 그것이 제게 일어난 일이었죠 — 그래서 중요한 버그에 대한 로그를 찾고 싶어졌을 때 유료 플랜을 구독해야 했습니다.

보너스 팁 #10: Spotify의 API를 사용하여 웹 앱을 만들려고 시도하지 마세요! Spotify의 API를 통합하여 사용자가 저장된 팟캐스트를 로드할 수 있다고 가정하고 2개월 동안 제 앱을 낭비했어요. 하지만 이를 제품화하려면 할당량 확장 요청을 신청해야 하는데, Spotify가 검토하는 데 한 달 이상 소요됩니다. 그리고 당신의 앱이 AI/ML 모델을 포함하고 있다면 신청이 거의 거부될 가능성이 높아요 (제 앱이 실제로 Spotify 데이터를 활용하여 어떠한 모델을 훈련시키지 않았음에도 불구하고, 그러한 용어가 개발자 정책에서 금지되어 있기 때문입니다).

# 결론

이 기술 가이드가 데이터과학 애호가들을 위한 웹 앱 개발의 일부 측면을 명확하게 해주길 바랍니다.

<div class="content-ad"></div>

만약 이 게시물이 도움이 된다면:

- 다른 제 미디엄 기사도 확인해보세요: AI를 사용해 긴 텍스트를 요약하는 방법, 딥 러닝을 사용해 음악을 생성하는 방법
- 제 어플을 사용해보세요 — Podsmart은 팟캐스트와 유튜브 영상의 필기본문과 요약을 제공하여 바쁜 지식인들에게 듣기 시간을 절약해줍니다
- LinkedIn이나 Twitter/X로 제 팔로우를 하고, 메시지나 댓글로 연락해주세요! 데이터 과학과 AI에 관한 모든 아이디어를 공유해보고 싶어합니다

읽어주셔서 감사합니다!