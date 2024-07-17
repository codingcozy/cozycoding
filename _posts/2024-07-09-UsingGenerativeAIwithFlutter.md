---
title: "Flutter에서 생성형 AI 사용하는 방법"
description: ""
coverImage: "/assets/img/2024-07-09-UsingGenerativeAIwithFlutter_0.png"
date: 2024-07-09 22:33
ogImage: 
  url: /assets/img/2024-07-09-UsingGenerativeAIwithFlutter_0.png
tag: Tech
originalTitle: "Using Generative AI with Flutter"
link: "https://medium.com/flutter-community/using-generative-ai-with-flutter-20327e22e8aa"
---


## 플러터 앱에 Google의 Gemini SDK 통합하기

<img src="/assets/img/2024-07-09-UsingGenerativeAIwithFlutter_0.png" />

이 문서의 동반 프로젝트: generative_ai_with_flutter

# 소개

<div class="content-ad"></div>

지난 몇 년 동안, 창조형 AI는 개발자 세계에서 가장 많이 이야기된 주제 중 하나였어요. 이 대화는 초기의 간단한 모델과 영향을 받는 사람 수가 적었던 2010년대 초반부터 중반까지 시작되었지만, 현재에 이르러서는 지수 함수적으로 성장하면서 컴퓨팅 파워가 기기에서 더 쉽게 이용 가능하게 되었고, 이전에 성능을 제공하던 모델들을 제압하는 새로운 유형의 AI 모델이 등장했어요.

상기한 발전으로, 대부분의 앱에서 특정 창조적 기능을 위해 AI가 사용될 것은 불가피했어요. 이러한 모델들은 상당히 무거우며 많은 메모리와 컴퓨팅 파워가 필요하기 때문에 이를 생성하는 회사들에 의해 주로 API로 노출됩니다.

하지만 저는 이 게시물을 작성하는 시점을 기준으로, 전화기와 같은 작은 기기에서 실행될수 있는 초기 모델들이 있습니다. 미래에는 전화기가 AI 연산을 별도로 수행할 수 있는 추가 하드웨어와 함께 제공될 가능성이 높습니다. 이것은 GPU가 기기에서 점점 더 많은 비주얼 요구 사항을 처리하기 위해 등장했던 것과 유사합니다.

지금은 Flutter 앱에 이러한 API를 통합하여 개발자들이 앱에서 기반으로 한 AI 기능을 만들 수 있도록 초점을 맞추고 있습니다. 이 게시물에서는 구글의 창조형 AI SDK에 대해 자세히 살펴보겠지만, 동일한 원칙이 다른 SDK에도 적용될 수 있다고 믿습니다. 이 기사는 Flutter 앱에 창조형 AI를 추가하는 데 초점을 맞추지만, 무엇을 다루고 있는지 알아야 하는 것은 항상 중요하다고 생각해요. 제가 차를 운전하기 전에 클러치 어셈블리가 어떻게 작동하는지 알아야 했었던 것처럼 말이죠. 따라서 이 게시물에는 창조형 AI 혁명을 일으킨 모델의 배경도 포함되어 있습니다: 대형 언어 모델(Large Language Models)입니다.

<div class="content-ad"></div>

# 대형 언어 모델 (LLM) 알아보기

구글의 제미니(Gemini)나 OpenAI의 ChatGPT와 같이 AI 세계를 폭풍 속으로 몰아넣은 기본 모델은 Large Language Model(LLM)이라고 합니다. 신경망(Neural Networks)에 대해 많은 해가 지난 지금도 세상을 변화시킬 것이라고 말해왔습니다.

하지만, 몇 년 동안 이 분야에서 어느 정도 진전이 있었기는 했지만, 그것이 많은 사람들의 삶에 직접적인 영향을 미친 적은 드물었습니다. LLM은 신경망을 기반으로 하지만, 다양한 기술적 발전으로 인해 자연어 이해 및 생성 분야에서 이전의 모델들을 뛰어넘을 수 있게 되었습니다.

그 중 하나의 중요한 기술적 발전은 "Attention is All You Need" 논문에서 소개된 트랜스포머(Transformer) 아키텍처입니다. 트랜스포머는 시퀀스 내의 장거리 종속성을 효율적으로 처리할 수 있게 해주어, 언어 번역, 텍스트 요약, 대화 생성과 같은 작업에 매우 효과적입니다.

<div class="content-ad"></div>

학습 전략의 발전으로, 방대한 양의 텍스트 데이터로 사전 학습하고 구체적인 작업에 대해 세밀하게 조정하는 등의 접근법이 LLM의 성능을 현저히 향상시켰습니다. 이러한 모델들은 이제 인간과 유사한 언어 패턴을 현저한 정도로 모방하여 일관된 맥락적인 텍스트를 생성할 수 있게 되었습니다.

이전에는 볼 수 없었던 수준에서 텍스트를 이해하고 생성하는 능력을 갖춘 LLM은 고객 서비스, 콘텐츠 생성, 언어 번역을 포함한 다양한 산업을 형태를 바꾸고 있습니다. 이들은 기술과 상호작용하는 방식을 혁신하고 이전에 상상할 수 없었던 새로운 응용 프로그램과 기회를 열어주는 잠재력을 지니고 있습니다.

## 주의 집중 이해

이전 섹션에서 언급된 논문에서, 주의 집중은 모델이 입력 시퀀스의 다른 부분에 집중할 수 있도록 하는 메커니즘을 가리킵니다. 이 메커니즘은 일관된 맥락적인 텍스트를 이해하고 생성하는 데 중요합니다.

<div class="content-ad"></div>

어텐션 메커니즘은 입력 시퀀스의 다른 단어나 토큰에 가중치를 할당하여 현재 처리 중인 단어에 상대적으로 중요하거나 관련성을 제안합니다. 이러한 가중치는 현재 단어와 입력 시퀀스의 각 단어 간의 유사성에 기초하여 계산됩니다.

현재 단어와 더 유사한 단어는 높은 가중치를 받고, 관련성이 낮은 단어는 낮은 가중치를 받습니다. 이러한 단어 가중치는 이후에 논의할 임베딩과 관련이 있습니다.

# google_generative_ai 패키지

Google이 만든 google_generative_ai 패키지는 Google의 생성 모델 AI API를 래핑하는 것으로, Flutter 앱에서 다양한 AI 모델과 상호작용할 수 있게 해줍니다. 이 패키지는 다양한 사용 사례를 지원하며, 이 중 대부분을 이 글에서 다룰 것입니다.

<div class="content-ad"></div>

Flutter 앱에 패키지를 추가하려면 pubspec.yaml 파일에서 해당 종속성을 추가하세요:

```js
dependencies:
  google_generative_ai: ^0.2.2
```

이 패키지를 사용하기 위해 특별한 권한이 필요하지 않습니다.

# 패키지에서 지원하는 모델

<div class="content-ad"></div>


<img src="/assets/img/2024-07-09-UsingGenerativeAIwithFlutter_1.png" />

google_generative_ai 패키지는 세 가지 주요 모델을 지원합니다: Gemini, Embeddings 및 Retrieval. Gemini 모델의 전신인 PaLM 모델은 레거시 모델로 지원되며 곧 제거될 예정입니다. Gemini 모델에는 Pro 및 Pro Vision 두 가지 형태가 있습니다.

Pro 모델은 주로 텍스트 입력 프롬프트를 다루며, Pro Vision 모델은 멀티모달이며 텍스트와 이미지를 둘 다 입력으로 지원합니다. 아직 API에서 지원되지 않지만 곧 지원될 것으로 예상되는 Ultra 버전도 있습니다. 이 문서에서 나중에 자세히 살펴볼 Embedding 모델도 제공됩니다.

이 목록에서 마지막 모델은 AQA Retrieval 모델입니다. AQA는 Attributed Question Answering의 약자입니다. AQA 모델은 일반 Gemini 모델과는 다른 방식으로 작동합니다. 이 모델은 다음과 같은 특정 작업에 사용할 수 있습니다:


<div class="content-ad"></div>

- 소스를 기반으로 질문에 답변: 질문과 관련 소스 자료를 제공하면 AQA 모델이 소스와 직접적으로 관련된 답변을 추출합니다.
- 답변의 신뢰도 평가: AQA 모델은 단순히 답변을 제공하는 것뿐만 아니라 제공된 맥락을 기반으로 답변이 정확할 가능성을 추정합니다.

# API 키 가져오기

이 패키지를 사용하기 전에 Gemini API용 API 키를 가져와야 합니다. API는 개인 정보 보호 제한으로 인해 EU와 같은 특정 지역에서 사용할 수 없을 수 있으므로 지원되는 지역에 거주하는지 확인해 주세요.

API 키를 만들려면 Google AI Studio로 이동하여 Google 계정으로 로그인한 후 API 키 만들기 버튼을 선택하세요:

<div class="content-ad"></div>


![image](/assets/img/2024-07-09-UsingGenerativeAIwithFlutter_2.png)

프로젝트에 생성된 키를 사용할 수 있습니다.

# 모델 구성

패키지에서 모델을 초기화하려면 GenerativeModel 클래스를 사용하여 API 키와 사용할 AI 모델을 지정할 수 있습니다.


<div class="content-ad"></div>

```js
var model = GenerativeModel(
      model: 'gemini-pro',
      apiKey: GenAIConfig.geminiApiKey,
    );
```

모델이 응답을 생성하는 방식을 제어하는 매개변수 값이 포함된 모든 프롬프트를 모델에 보냅니다. 동일한 속성은 동일한 클래스의 generationConfig 매개변수를 통해 설정할 수 있습니다:

```js
var model = GenerativeModel(
      model: 'gemini-pro',
      apiKey: GenAIConfig.geminiApiKey,
      generationConfig: GenerationConfig(
        candidateCount: 2,
        stopSequences: ["END"],
        maxOutputTokens: 500,
        temperature: 10.0,
        topK: 1,
        topP: 0.5,
      )
    );
```

모델은 다른 매개변수 값에 따라 다른 결과를 생성할 수 있습니다. 아래 구성 매개변수를 조정할 수 있습니다:

<div class="content-ad"></div>

- 최대 출력 토큰 수: 토큰 한 개는 대략 네 글자에 해당합니다. "최대 출력 토큰 수" 설정은 응답이 생성할 수 있는 토큰의 상한선을 정의합니다. 예를 들어 100 토큰의 제한은 대략 60~80단어를 생성할 수 있습니다.
- 온도: 이 매개변수는 토큰 선택의 무작위성을 영향을 줍니다. 낮은 온도 설정은 더 결정적이거나 구체적인 응답이 필요한 프롬프트에 적합합니다. 반대로, 높은 온도는 더 다양하거나 상상력이 풍부한 출력물을 유도할 수 있습니다.
- TopK: TopK를 1로 설정하면 모델은 다음 토큰을 위해 어휘에서 가장 확률이 높은 토큰을 선택합니다 (탐욕적 디코딩). 그러나 TopK가 3인 경우 모델은 온도 설정에 따라 세 개의 가장 확률이 높은 옵션 중에서 다음 토큰을 선택할 수 있습니다.
- TopP: 이 매개변수는 가장 확률이 높은 것부터 시작하여 확률의 합이 TopP 임계값에 도달할 때까지 토큰 선택을 가능하게 합니다. 예를 들어, 토큰 A, B, C가 각각 0.3, 0.2, 0.1의 확률을 갖고 있는 경우, TopP 값이 0.5이면 모델은 다음 토큰으로 A 또는 B 중 하나를 선택할 것이며, 온도 설정을 이용하여 C를 고려하지 않습니다.
- 후보 개수: 생성할 고유한 응답의 최대 개수를 지정합니다. 후보 개수가 2인 경우 모델은 두 가지 다른 응답 옵션을 제공할 것입니다.
- 중지 시퀀스: 출력물에서 등장하는 문자 시퀀스를 지정하여 API에게 추가 답변 생성을 중단하도록 할 수 있습니다. 이렇게 말해, "END"라는 단어가 중지 시퀀스에 포함되어 있다고 가정해 봅시다. LLM에 의해 생성된 가상 결과가 시퀀스가 없다면 "START MIDDLE END TERMINATE"일 경우, 시퀀스를 추가한 후에는 "START MIDDLE"로 변경될 것입니다.

각 매개변수의 값을 세부적으로 조정하여 창조 모델을 사용자의 특정 요구에 맞게 맞춤 설정할 수 있습니다. LLM 개념 가이드는 대형 언어 모델 (LLM) 및 구성 가능한 매개변수에 대한 깊은 이해를 제공합니다.

# 안전 설정

이전 섹션에서 언급된 매개변수들은 LLM이 답변을 결정하는 방식을 제어하기 위한 것입니다. 하지만 사용자에게 부적절할 수 있는 콘텐츠를 차단하지는 않습니다. 이를 위해 API는 일부 유형의 콘텐츠를 차단할 수 있게 해주는 안전 설정을 나열합니다. 또한 이러한 차단될 확률 임계값을 결정하고 어떤 것들은 엄격한 제한을 두는 반면 다른 것들에 대해서는 관대한 제한을 두는 것도 가능합니다.

<div class="content-ad"></div>

여기 예시 입니다:

```js
var model = GenerativeModel(
      model: 'gemini-pro',
      apiKey: GenAIConfig.geminiApiKey,
      safetySettings: [
        SafetySetting(HarmCategory.dangerousContent, HarmBlockThreshold.medium),
        SafetySetting(HarmCategory.hateSpeech, HarmBlockThreshold.high),
      ],
    );
```

이 예시에서는 위험한 콘텐츠와 혐오 발언을 다른 임계값으로 차단하는 두 가지 안전 설정을 추가했습니다. 안전 설정에 추가할 수 있는 카테고리는 위험한 콘텐츠, 혐오 발언, 괴롭힘, 성적으로 은유된 콘텐츠가 있습니다.

# 텍스트에서 텍스트로의 생성

<div class="content-ad"></div>

Generative AI를 사용하는 사용자 대부분은 텍스트 생성을 위해 텍스트를 사용합니다. 이는 입력으로 프롬프트를 제공하고 생성된 텍스트를 응답으로 받는 것을 의미합니다. 이를 위해 GenerativeModel 인스턴스를 만들고 Gemini Pro와 같은 텍스트 대 텍스트 생성에 적합한 모델을 사용할 수 있습니다.

프롬프트는 Content 클래스의 인스턴스로 제공되며, 그런 다음 model.generativeContent() 메서드를 사용하여 응답 생성을 위해 API를 쿼리할 수 있습니다:

```js
var model = GenerativeModel(
      model: 'gemini-pro',
      apiKey: GenAIConfig.geminiApiKey,
    );
    
final content = [Content.text("여기에 프롬프트 입력")];
    
final response = await model.generateContent(content);
```

generateContent() 요청에 이전에 언급한 안전 설정 및 생성 구성을 전달하고 단일 쿼리에만 사용하려면 해당 설정을 사용할 수 있음에 유의하세요:

<div class="content-ad"></div>

```js
var model = GenerativeModel(
      model: 'gemini-pro',
      apiKey: GenAIConfig.geminiApiKey,
    );

final content = [Content.text("여기에 프롬프트를 입력하세요")];

final response = await model.generateContent(
  content,
  safetySettings: [...],
  generationConfig: GenerationConfig(...),
);
```

# 멀티모달 생성

멀티모달 생성은 텍스트, 이미지, 오디오 또는 기타 형식의 데이터와 같은 여러 모니얼리티에서 정보를 통합한 출력물을 생성하는 것을 의미합니다. 이 예시에서는 이미지와 텍스트 프롬프트를 제공하여 응답을 생성합니다:

```js
// 이미지 피커 플러그인에서 선택한 파일
XFile? _image;

var model = GenerativeModel(
      model: 'gemini-pro-vision',
      apiKey: GenAIConfig.geminiApiKey,
    );
    
var prompt = '여기에 프롬프트를 입력하세요';

var imgBytes = await _image!.readAsBytes();

// mime 플러그인에서 제공하는 룩업 함수
var imageMimeType = lookupMimeType(_image!.path);

final content = [
  Content.multi([
    TextPart(prompt),
    DataPart(imageMimeType, imgBytes),
  ]),
];

final response = await model.generateContent(content);
```

<div class="content-ad"></div>

이를 위해 콘텐츠에 두 개 이상의 부분을 제공해야 합니다. 여기에는 텍스트 부분과 이미지 부분이 있습니다. 우리는 비슷한 Content 인스턴스를 만들지만 Content.multi() 생성자를 사용하여 콘텐츠에 여러 부분이 있다는 것을 나타냅니다. DataPart 클래스를 사용하여 실제 이미지 데이터를 모델에 제공할 수 있습니다.

# 채팅 인터페이스 생성

간단한 텍스트 대 텍스트 생성과는 달리, 채팅은 이전 메시지를 추적하고 더 나은 메시지를 생성하기 위한 맥락으로 사용해야 합니다. 이 프로세스는 텍스트 대 텍스트 생성과 매우 유사하지만, 또한 모델.startChat()을 통해 ChatSession 인스턴스를 생성해야 합니다.

```js
var model = GenerativeModel(
      model: 'gemini-pro',
      apiKey: GenAIConfig.geminiApiKey,
    );
    
var chatSession = model.startChat();

final content = Content.text("여기에 프롬프트를 입력하세요");

var response = await chatSession.sendMessage(content);
```

<div class="content-ad"></div>

대화 세션에서 채팅 메시지를 보내려면 chatSession.sendMessage()을 사용할 수 있어요.

또한 대화 세션은 채팅 기록을 유지하고 chatSession.history를 통해 접근할 수 있어요:

```js
var model = GenerativeModel(
      model: 'gemini-pro',
      apiKey: GenAIConfig.geminiApiKey,
    );
    
var chatSession = model.startChat();

for (var content in chatSession.history) {

  for (var part in content.parts) {
    if(part is TextPart) {
      print(part.text);
    } else if (part is DataPart) {
      print('데이터 부분');
    }
  }

}
```

# 임베딩 사용하기

<div class="content-ad"></div>

내부적으로 LLMs는 많은 단어/텍스트 조각들 간의 의미, 사용법, 그리고 관계에 대한 큰 지도를 저장합니다. 이미지 생성을 위한 확산 모델과 같은 다른 모델들에도 이러한 것이 존재하여 이미지가 무엇을 묘사해야 하는지 더 잘 이해할 수 있게 됩니다.

텍스트 임베딩은 본질적으로 자연어 데이터에서 유도된 단어, 구, 또는 전체 문서의 수치적 표현입니다. 이러한 표현은 텍스트의 의미론적 및 문법적 특성을 저장합니다. 이 개념은 비슷한 의미나 맥락을 갖는 단어들은 유사한 수치적 표현을 가져야 한다는 아이디어에 근거합니다.

LLMs는 각 단어의 표현을 인코딩할 때 주변 단어들을 고려합니다. 이를 통해 다양한 맥락에서 단어 의미의 미묘한 차이를 포착할 수 있습니다. 예를 들어, "은행"이라는 단어는 금융 기관이나 강가의 한 쪽을 가리킬 수 있는데, LLMs는 이러한 의미를 맥락에 기반하여 구분할 수 있습니다.

Google에서 제공하는 임베딩 모델은 입력 데이터의 수치적 표현을 제공해줍니다. 이 데이터를 사용하여 두 개의 데이터 조각 간의 유사성이나 관계를 추론할 수 있습니다.

<div class="content-ad"></div>

임베딩 모델을 사용하는 것은 텍스트 대 텍스트 생성과 유사합니다. 예를 들어 'embedding-001' 모델은 주어진 텍스트 조각을 벡터 표현으로 변환할 수 있도록 해줍니다:

```js
var model = GenerativeModel(
      model: 'embedding-001',
      apiKey: GenAIConfig.apiKey,
    );
    
final content = Content.text("여기에 프롬프트를 입력하세요");

final response = await model.embedContent(content);
```

임베딩에 대해 더 알아보려면 Google의 이 코스를 시도해보세요.

# 결론

<div class="content-ad"></div>

지금은 Google의 Generative AI 원칙, Gemini SDK, 그리고 텍스트 생성 및 사진 추론 기능을 갖춘 AI 챗봇 시스템의 구현을 살펴보았습니다. Generative AI는 여러분의 앱 내에서 사용자 경험을 크게 향상시킬 수 있는 다양한 응용 프로그램을 제공합니다. 추가 정보 및 예시는 아래 링크된 GitHub 저장소에서 확인해주세요:

- Flutter를 활용한 Generative AI 저장소
- GitHub의 google_generative_ai 

질문이나 피드백이 있으시면 Twitter에서 @DevenJoshi7 또는 GitHub에서 저를 찾을 수 있습니다. Stream의 최신 소식을 받고 싶다면 더 많은 훌륭한 기술 콘텐츠를 제공하는 @getstream_io를 팔로우해주세요.