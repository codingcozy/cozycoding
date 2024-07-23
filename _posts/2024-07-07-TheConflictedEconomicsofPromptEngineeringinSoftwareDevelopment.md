---
title: "소프트웨어 개발에서 프롬프트 엔지니어링의 경제적 갈등 2024년 전망"
description: ""
coverImage: "/assets/img/2024-07-07-TheConflictedEconomicsofPromptEngineeringinSoftwareDevelopment_0.png"
date: 2024-07-07 03:23
ogImage: 
  url: /assets/img/2024-07-07-TheConflictedEconomicsofPromptEngineeringinSoftwareDevelopment_0.png
tag: Tech
originalTitle: "The Conflicted Economics of Prompt Engineering in Software Development"
link: "https://medium.com/@dnastacio/economics-prompt-engineering-2aebc03acb58"
---


"If you are nothing without this suit, then you shouldn’t have it." – Tony Stark

The buzz around prompt engineering is heating up and with the introduction of foundation models trained in programming languages, software engineers wasted no time delving into prompt-assisted code generation.

With a diverse background in architecture, design, operations, and programming, as a software engineer, I’m excited about the prospects of integrating AI into software development. Stay tuned for an upcoming article where I’ll dive deeper into this topic.

The ultimate success of this venture will hinge on the effort required to explain it and the quality of the solutions it produces.

<div class="content-ad"></div>

일부 문제는 작업의 작은 부분에 국한되어 있으며 설명하기 쉽고 해결책 역시 사소합니다. 문제 해결은 학습 모델이 과적합되는 지점까지 다룹니다.

다른 문제는 시스템의 다른 부분들과 복잡한 관계를 맺고 있어 인식이 어렵고, 그 해결책에는 "그것에 달린다"로 시작하는 설명이 포함되는 트레이드 오프가 포함됩니다.

해당 문제에 관한 일화적인 증거는 인공지능의 환각이나 실패율을 보여줍니다. 이는 인간이 만들었다면 "인적자원", "성과 향상 계획", 혹은 심지어 "요오드 검사"와 같은 단어를 떠올리게 할 것입니다. 

이야기가 너무 길어져서 두 부분으로 나누었습니다.

<div class="content-ad"></div>

- 파트 1 (이야기) : 코드 작성의 경제학. 여기에서는 실제 사례 연구를 통해 비트가상 AI 코드 어시스턴트의 성능을 전통적이고 도움 없는 방법과 비교하는 것으로, 비트가상 소프트웨어 엔지니어링 문제를 해결하는 짧고 장기적인 경제를 살펴봅니다.
- 파트 2 : 대규모 시스템 생성 및 운영 경제. 이 이야기에서는 인간이 이미 뛰어나는 코딩 지원에 AI 훈련 노력을 집중하는 대신 전체 시스템의 생성 및 운영 기술적 측면 주변의 자연어에 집중하는 이유를 다룹니다.

# 어려운 문제에 대한 훈련

복잡한 문제에 대한 AI 훈련의 원칙은 여전히 동일합니다. AI에게 충분한 예제를 보여주어 패턴을 추론하고 데이터가 수집된 상황과 유사한 상황에 대한 결과를 예측하도록 합니다.

프로그래머가 AI 모델에게 "URL의 내용을 읽는 Python 함수"를 요청하면, 54백만 개의 GitHub 리포지토리에서 수집한 159 기가바이트의 Python 코드 소스에서 훈련된 AI 모델이 매우 유사한 해결책이 있는 것을 알 수 있습니다. 그 요청의 약간 변경된 의문어와 거의 동일한 것에 대한 주석이 바로 그 훈련 세트에 있습니다.

<div class="content-ad"></div>

풍부한 교육 자료가 충분하지 않다면, 교육 데이터에는 기본 주제를 다루는 공개 도메인의 여러 기사도 포함되어 있습니다. 이는 쓰고 이해하기 쉽기 때문에 일부이다.

하지만 세계에서 가장 많이 사용되는 프로그래밍 언어가 아닌 프로그래밍 언어를 사용하거나 조직 내부에 작성된 맞춤형 기밀 소스 코드를 사용하여 구성 요소를 결합하는 경우에는 어떻게 될까요?

이러한 상황에서는 AI를 훈련하기 위한 자료가 적어지며, 결과적으로 교육 데이터의 빈 곳이 생기고 결과적으로 프롬프트 결과의 품질이 감소합니다. 다음 섹션에서 사례 연구를 통해 이 점을 설명하겠습니다. 또한 저작권이 있는 소스 코드나 액세스 제어된 소스 코드를 다룰 때, 일부 콘텐츠는 교육 데이터에서 제외되어 도메인 커버리지의 공백이 커지고 답변의 품질이 저하될 수 있습니다.

이는 논의 중에 누군가가 손을 들고 묻는 지점입니다:

<div class="content-ad"></div>

예, 기존의 모델을 세밀하게 조정하는 것은 훈련된 AI 모델을 특정 훈련 세트의 독특한 특성에 적응시키는 인기 있는 해결책입니다. 모델을 세밀하게 조정하는 것은 더 큰 AI 모델에서 시작하여 조직 내에서 보다 가까운 예시를 보여줌으로써 수행하는 활동으로, 이는 모델을 처음부터 훈련시키는 것보다 비용의 한 부분 - 몇 분의 일 -밖에 들지 않습니다.

더 나은 결과를 얻는 동안 비용의 소액 부담은 경제적으로 좋아보일 수 있지만, 이 저렴한 가격표의 출발 참조점이 수천만 달러 수준이며 매년 4-5배씩 증가하여 컴퓨팅 비용만으로도 매월 수천 달러에 이르기 때문에 염두에 두어야 합니다.

반박의 첫 번째 부분은 사실입니다 - 로컬 LLM을 훈련하거나 세밀하게 조정할 수 있습니다 - 그러나 무시할 수 있는 주말 프로젝트와 실제 소프트웨어 엔지니어링을 분리하는 것은 두 번째 부분입니다.

공개된 기초 모델을 세밀하게 조정하는 경제학적 가치는 끔찍합니다.

<div class="content-ad"></div>

프로토타입에서 호스팅 솔루션으로 이동하는 데는 프로토타입에 소요된 노력의 27배가 필요할 수 있습니다. 프로토타입을 마치고 나면 호스팅 비용을 고려해야 합니다. 클라우드 공급업체로부터 컴퓨팅 파워를 빌리거나 자체 하드웨어를 호스팅해야 합니다. 그 후에는 세부 튜닝이 모델의 언어 부분을 개선하는 수단이지만 최종 시스템에 지식을 추가하는 방법은 아님을 인지해야 합니다.

실제로 세계의 모든 예제가 모든 가능한 함수 서명 또는 CLI 명령을 완전히 다루지 않기 때문에 나는 각 프롬프트에 그 지식을 주입하기 위해 RAG 패턴 (검색 보충 생성)을 살펴봐야 합니다. 이를 통해 AI 모델에 보낸 각 프롬프트에 대한 보충을 세부 조정하는 데 데이터베이스, 쿼리 엔진 및 일정 시간이 필요합니다.

그 세밀하게 조정된 RAG 시스템을 갖춘 모델을 사용하고자 한다면 그 솔루션 주변에 머신 러닝 파이프라인을 개발해야 합니다. 그 파이프라인을 설계하는 동안 모델 성능을 추적할 지표를 결정하고, 해당 지표를 수집하는 방법, 의견에 응답하는 프로세스를 만들고, 업데이트를 전개하기 위한 추가 프로세스 등을 고려해야 합니다.

그것만으로 충분하지 않다면 노력에 최적의 방법을 결정하기 위해 대안적 세밀 조정 방법을 실험해야 할 수도 있습니다.

<div class="content-ad"></div>


![Image](/assets/img/2024-07-07-TheConflictedEconomicsofPromptEngineeringinSoftwareDevelopment_0.png)

The economics of pushing through such an endeavor resemble those of a research project, involving significant learning curves on new technology and the additional engineering efforts in developing and maintaining the final solution. Moreover, arguing against the economic viability of training a localized model, there's always the possibility that the next iteration of the base model may surpass your entire investment.

If you're convinced that implementing and managing your own model for a small-scale localized deployment isn't a sound business move, consider a practical case of utilizing a code-generation assistant based on public foundation models—I employed three different AI systems during the experiment to ensure consistency in results.

## Case Study: Single Sign-On Configuration


<div class="content-ad"></div>

이 섹션은 최근 실제 예시를 기반으로 합니다. 여기서는 보안 및 규정상의 목적으로 인터넷에 직접 연결되지 않는 공격으로부터 격리된 환경(에어갭트 환경)을 위해 더 포괄적인 솔루션의 두 구성 요소를 패키징해야 하는 상황을 다루었습니다.

쿠버네티스 클러스터에서 HashiCorp Vault의 인스턴스를 구성하여 Keycloak에 호스팅된 OIDC 클라이언트를 사용하여 로그인을 안전하게 보호해야 했습니다. 기술적인 것에 익숙하지 않다면, HashiCorp Vault는 시크릿을 관리하는 서비스이며, Keycloak은 다른 시스템에 대한 액세스를 관리하는 서비스입니다. 이 솔루션에서는 Keycloak을 사용하여 Vault에서 시크릿에 액세스할 수 있는 사용자를 인가하고자 합니다.

이 통합은 프로그래밍 요소와 패키징 및 구성 측면이 결합된 "어려운 문제"로서, 외부 액세스 관리자를 사용하여 Vault에 대한 액세스를 보호하는 것은 흔하지 않은 시스템 조합입니다. 그러나 Vault는 가장 큰 클라우드 제공업체들의 호스팅 옵션을 비롯해 수십 가지 다양한 인증 도구와 작업할 수 있으므로 Keycloak을 사용하는 방법에 대한 공개 콘텐츠가 다른 대안들보다 훨씬 적을 수 있다는 점을 유의하시기 바랍니다.

<div class="content-ad"></div>

이러한 문제를 해결할 때, 나는 AI의 성능을 평가하기 위해 나중에 사용할 프레임워크를 따라 엄격하게 따르는 대신 다음 단계를 대략적으로 따릅니다.

- 두 제품의 문서를 찾습니다.
- API와 명령줄 인터페이스를 찾습니다.
- 함수와 매개변수를 읽습니다.
- 함수와 매개변수를 더 큰 문제에 매핑합니다.
- 메모장(또는 코드 편집기의 빈 페이지)에 초기 솔루션을 작성합니다.
- 문서와 함께 초안을 반복합니다.
- 지침을 자동화하는 프로그램을 작성합니다.
- 구성 요소에 대한 문서 또는 메모를 작성합니다.
- 솔루션을 일반적으로 사용 가능하다고 고려할 경우, 공개 블로그나 기술 노트를 작성합니다.

글쎄! 이것은 상당히 많은 작업인 것 같네요, 맞죠? 이제 AI에게 물어볼 시간입니다.

AI는 두 장 분량의 지침을 출력하여 도움이 되었고, 저를 5단계(초안을 작성)의 중간 지점으로 안내했습니다.

<div class="content-ad"></div>

해결책을 시도해 보았으나 그 단계에서 예상한 대로 여러 이유로 실패했습니다. 그러나 예상치 못한 문제로 인해 프로세스의 초기 단계로 되돌아가야 했습니다.

첫 번째 문제는 Vault가 Keycloak OIDC 발견 엔드포인트 URL의 사용자 정의 인증서를 신뢰하지 않는다고 불평했던 것입니다.

나는 Keycloak 서버가 사용자 정의 인증서를 사용한다는 것을 나타내는 초기 프롬프트를 수정했지만, AI는 이 변경을 무시하고 응답에 아무런 변화도 보이지 않았습니다. 더 명확하게 설명해도 무시될 것으로 예상되어 AI에게 직접 오류 메시지를 어떻게 해결할 것인지 물어보았습니다. AI는 올바른 매개변수가 포함된 업데이트된 명령을 보여주며 자신을 바로잡았습니다.

다음으로 솔루션을 유효성 검사하고 Keycloak에 저장된 사용자로 Vault에 로그인하자, 사용자를 식별하기 위해 Vault UI가 사용자의 이메일이나 전체 이름 대신 숫자 식별자를 표시하는 것을 발견했습니다. 사용자 친화적일 뿐만 아니라, 알아볼 수 있는 이름은 사용자가 서로 다른 권한을 갖는 여러 계정에 액세스하는 경우에도 추가적인 보안 층입니다. (시스템 운영에서 일반적인 경우)

<div class="content-ad"></div>

앞서 언급한 것을 바탕으로 이미지 태그를 Markdown 형식으로 변경할 수 있습니다.

<div class="content-ad"></div>

그 단계에서 AI에게 Keycloak 관련 단계를 자동화하는 쉘 스크립트를 생성하도록 요청했습니다. 그 결과, "curl"과 "jq"와 같은 저수준 유틸리티를 기반으로한 존경받을 만한 일련의 쉘 스크립트 명령이 생성되었습니다. 그러나 조금 불쾌한 측면으로, AI는 스크립트에 모든 URL과 자격 증명 자리 표시자를 하드 코딩하여, 이는 인간 개발자에게 좋지 않은 방식일 것입니다.

그러나 더 심각하고 미묘한 문제도 있었습니다: Keycloak은 이미 관리자용 CLI를 갖고 있었습니다. "curl"로 REST 엔드포인트에 직접 액세스하고 "jq"로 JSON 응답을 처리하는 방식은 작동했지만, 제품 API에 대한 미래의 변경 사항에 민감하며 읽기(및 유지 관리)가 더 어렵다는 코드를 생성했습니다.

경제학적 관점에서 이 모듈은 눈에 띄는 실수를 발견한 사람들이 생겼을 때 재작성 및 재테스트해야 했을 것이며, 이는 아마도 Keycloak의 첫 API 변경과 동시에 일어날 것입니다. 또한 코드를 작성하는 동안 전체 관리용 CLI의 존재를 간과해 헤맬 경우 (제 경우) 평판상의 결과가 있을 것입니다.

나는 AI와 함께 진행했습니다. 그 CLI의 존재를 지적하지 않았습니다. 신기하게도, AI는 결국 전에 제시한 프롬프트에서 잘못된 접근 방식을 제안했음에도 불구하고 해당 CLI를 사용한 적절한 해결책을 제안하며 구현 전략을 전환했습니다.

<div class="content-ad"></div>

모든 실행 단계가 완료된 상황에서 AI의 도움이었던 8단계(문서 작성)가 가장 인상적이었습니다.

다음 프롬프트를 사용했습니다:

나는 거의 완성된 기술 문서를 받았습니다. 제품 문서 페이지에 대한 참조가 누락된 선행 요건 부분과 이전 단계에서 본 환각이 동일하게 나타났습니다. 그러나 AI 도움은 나와 다른 많은 소프트웨어 개발자들에게 일반적인 고통점을 해소해 주었습니다: 창의적인 세션의 끝에서 문서를 작성하는 것, 이때 뇌가 해결책을 창출하는 데 초점을 맞추고 최종 해결책을 설명하는 데 그리 많이 맞추지 않는 곳에서 — 아, 그리고 피곤하기까지 합니다.

(가볍게 편집된) 응답의 버전을 읽을 수 있습니다. 전체 공개를 위해 URL 링크의 마크다운 몇 부분을 수정해야 했지만, 그것만으로도 몇 분 걸렸습니다.

<div class="content-ad"></div>

# AI vs Traditional Method: Who Emerged Victorious?

Hey there! Today, let's dive into my typical workflow breakdown and highlight whether the AI or the good old human method clinched a win at each stage. I'll also share some insights on each outcome.

## Step 1: Researching product documentation

🏆 Winner: AI

<div class="content-ad"></div>

AI가 나에게 각 과정의 유용한 링크를 제공해줬어. 이 링크는 지친 웹 검색 없이도 지침에 내제되어 있었고, 실행하는 데 더 적은 저항감을 주었어.

웹 검색은 유용한 블로그 글과 스택 오버플로 링크를 노출해줬지만, 나는 여러 항목(그리고 광고들)을 확인하고, 페이지를 여는 데에 그것들을 스캔하고, 필요한 문서 부분을 찾기 위해 몇 개의 링크를 제품 웹사이트에서 클릭해야 했어.

기존 과정은 장기적으로 장점이 있음을 언급해두는 게 중요하다 — 이에 대해서는 나중에 이야기할게 — 하지만 나는 지낸 시간과 즉각적인 결과만을 엄밀히 살펴봤어.

Step 2. API와 명령줄 인터페이스 찾기

<div class="content-ad"></div>

승리: 인공지능

이전 단계와 유사하게, 실제 문제의 맥락에서 링크를 요청하면 좋은 결과를 얻을 수 있습니다. 이 방법은 API 및 CLI가 사용되는 단계에 가까운 지침에 내장되어 있으므로 장점이 있습니다.

일반적으로 검색 조건을 작성하는 데 소요하는 시간보다 AI에게 문제를 적는 데 추가 시간을 써야 했지만, 이는 전체 활동에 비해 추가 시간이 아니었습니다.

단계 3. 함수 및 매개변수 읽기

<div class="content-ad"></div>

이미지 : 인간적인 방법

문서를 읽고 문제에 관련된 기술을 이해하는 것은 장기적으로 소유 비용을 최소화하면서 실행 가능한 해결책을 생산하는 데 중요한 측면입니다.

AI 초안은 일회용 프로토타입에서는 허용될 수 있지만 실제 제품에서는 용납할 수 없습니다. AI는 "oidc_claims_mapping" 매개변수의 환각이나 Keycloak 관리 CLI 주변의 일시적인 맹점과 같이 제품 문서의 의미 네트워크를 우리 두뇌나 자신의 회로에 만들어낼 수 없다는 증거에 의해 입증되었습니다.

단계 4. 기능과 매개변수를 문제에 매핑하기

<div class="content-ad"></div>

**승리: 인간의 방법**

AI 초안은 초안 솔루션의 일부 매개변수를 나타내 줬지만, 그 매개변수는 전체의 일부에 불과했습니다. 문서를 읽으면서 원래의 AI 초안의 품질을 평가하고 솔루션을 생산 시스템에 적응시키는 다른 대안 및 고려 사항에 대한 훨씬 나은 시각을 얻었습니다.

기술의 기본 구성에서 제공되는 전체 구성 범위를 이해하는 것이 솔루션이 안정적이고 생산 시스템 내에서 운영 비용이 가장 낮을지를 판단하는 유일한 방법입니다.

단계 5. 메모장(또는 빈 코드 편집기 페이지)에 솔루션 초안 작성하기

<div class="content-ad"></div>

승자: 인공지능

모든 반복 과정을 거친 끝에, 초기 초안은 최종 해결책의 90%를 포함하고 있었으며, 한 문장 프롬프트에서 이루어진 놀라운 노력이었습니다.

웹 검색을 통해 예제를 찾아보는 전통적인 방법을 사용하면 해당 초안을 작성하는 데 필요한 모든 기본 블록을 찾을 수 있었지만, 인공지능은 이들을 빠르게 연결하여 문제를 해결하는 단계적 기술 논문을 생성할 수 있었습니다.

Step 6. 문서와 함께 초안을 반복적으로 검토합니다.

<div class="content-ad"></div>

**승리: 인간의 방법**

저는 AI로부터 맡은 일들을 처리했습니다. 처음엔 AI가 (소수의 예외를 제외하고는) 훌륭한 첫 번째 초안을 개선할 수 없어서 일부를 맡았고, 주로 Vault CLI의 환영 파라미터와 Keycloak CLI의 일시적인 블라인드스팟 문제를 해결한 후에 AI의 정확성에 대한 신뢰를 잃었기 때문입니다.

이 경험은 다른 고급 엔지니어들이 이야기하고 읽은 내용과 일치합니다. AI 채팅이 요청에 대한 세부 정보를 더 추가하면 일반적으로 평평해지거나 더 붕괴되는 경향이 있다고 합니다.

7단계. 지침을 자동화하는 프로그램을 만드세요.

<div class="content-ad"></div>

Win: 인간적인 방법

Keycloak 관리용 CLI를 무시하는 초기 AI 실수를 제외하고 AI가 “getopt” 명령을 사용하여 셸 스크립팅 보일러플레이트를 생성하는 방법이 마음에 들었습니다.

하지만 저희 프로젝트의 셸 스크립트 복사본을 덮어쓰는 것이 효율적일 것이라고 생각됩니다. 이는 우리의 코딩 표준(저작권 표기, 명명 규칙, 메시지 서식, 오류 처리 등)과 더욱 일치할 것입니다.

Step 8. 최종 지시 사항을 마크다운 파일에 작성하세요.

<div class="content-ad"></div>

승리: 인공지능

스텝 "5"의 원본 글은 기술적인 기사의 뼈대만 있는 느낌이었습니다.

인공지능이 마크다운 형식으로 출력하도록 요청한 부분에서 문제가 발생했습니다. 전체 결과물을 마크다운 형식으로 지정했다 해도 중간에 리치 텍스트 렌더링 형식으로 전환하는 문제가 있었는데, 얼마나 구체적으로 요청하느냐에 상관없이 발생한 문제였습니다.

또한, 제품 설명서에 링크를 포함하도록 요청하는 방법을 여러 가지로 시도했지만, AI는 개선된 프롬프트에 대한 답변 품질을 향상시키지 못했습니다.

<div class="content-ad"></div>

하지만 그 문제들은 전체 콘텐츠에 비해 사소했습니다.

탐욕스럽지 말고 계속 수정하면서 지침서의 최종 버전을 얻는다면, AI는 완전히 새로 시작하는 것에 비해 시간을 절약할 수 있습니다.

단계 9. 공개 블로그나 기술 노트를 작성하세요.

시도하지 않음

<div class="content-ad"></div>

I believe the solution might be of interest to a broad audience, although admittedly it's a bit niche. However, due to time constraints, I was unable to delve into it. Even if I had the time, I'd still prefer to craft it myself with the assistance of AI rather than completely outsourcing the writing task to AI.

Nowadays, with AI capable of generating technical content instantly, it's hard to imagine someone preferring to go through content generated based on another person's prompt rather than instructing AI to produce tailored content for their specific needs.

**The Appeal of Short-Term (Potential) Gains**

In the short term, after recording four wins for each side and roughly estimating the time invested in the activity, I concluded that enlisting a generative coding AI into my workflow was a draw in terms of economic benefits.

<div class="content-ad"></div>

다양한 기술을 사용한 다양한 케이스 스터디들은 훈련 데이터가 해당 도메인을 얼마나 잘 다루었느냐에 따라 순수한 이득이나 손실 측면으로 기울어질 수 있습니다.

제 연구에서는 꽤 잘 알려진 제품인 Keycloak을 사용했는데, 이는 CNCF의 인큐베이션 프로젝트이기도 하며, 시크릿 관리의 사실상 표준입니다. 그래서 제가 가정하는 바는 훈련 데이터가 해당 영역을 충분히 아우르고 있는 것입니다.

AI는 처음에 중대한 맹점을 놓치기도 했고, 몇 가지 매개변수를 환각하기도 했습니다. 그러나 유용한 문서를 식별하고 문서 초안을 작성하는 자연어 영역에서는 그 실수를 보상했습니다.

이번 평가를 넘어서 AI는 또한 내가 부수적 작업에서 사용하는 단순한 Python 기반 함수들에 대한 초기 초안에서도 일반적으로 뛰어난 성과를 보이며, 우리 Git 저장소의 내용을 분석하는 작업 등을 할 때 일반적으로 몇 가지 편집만 필요하게 됩니다.

<div class="content-ad"></div>

암호화폐 전문가로서, AI 어시스턴트가 간단한 보조 작업에 집중하는 것이 가장 적절하다고 생각합니다. 그러나 저는 그것에 대한 열정이 주로 점진적인 유형이라고 생각합니다. 예를 들어 엑셀에서 데이터 피벗을 처음으로 마스터했을 때와 같은 열정입니다. 제가 일을 그만두고 저렴한 로봇 개발자들로 구성된 소프트웨어 회사를 시작한다는 종류는 아닙니다.

# 장기적인 손실은 파괴적일 수 있습니다

장기적으로, 프롬프트 기반 프로그래밍의 이득과 손실에 대한 전체 논쟁은 문제 해결 주기의 중요한 부산물인 지식 쌓기를 무시합니다.

문제를 해결할 때 전통적인 조사, 발견, 추상화, 프로그래밍 솔루션, 그리고 결과 코드를 유효성 검사하는 방법을 사용하는 것에 형성적인 면이 있습니다.

<div class="content-ad"></div>

해결책의 구성 요소를 조사하는 과정에서 우리는 서로 다른 상황에서 잘 작동하는 요소들 사이의 경계와 절충 사항이 가득한 전체 도메인 모델을 만들어 가고 있습니다.

질문에 대한 답변을 받는 것은 우리의 뇌가 그 답변의 구성 요소를 처리하고 조립하는 단계를 거치지 않게 합니다. 이런 의미에서, 코드 저장소에 빠르게 결과를 올리는 것은 기술을 쌓고 재능을 향상시키는 과정을 동시에 거치지 않게 되는 대가를 지게 됩니다.

AI 능력이 우리의 것보다 더 빠르게 성장한다는 강한 전제에 뒷받침된 무서운 절충안입니다.

“입력의 결과가 맞는지 확인하세요”로 끝나는 사용 사례들이 어느 정도까지 유효할지, 우리가 그 결과를 요청하는 것이 일반화되고 우리가 그 같은 결과에 도달하는 데 필요한 기술을 떨어뜨리는 과정이 집단적으로 진행되어 가는 시스템에서는 어떻게 될까요?

<div class="content-ad"></div>

일부 기술 애호가들은 생성적 AI가 훌륭한 학습 도구라고 주장합니다. 그럼에도 불구하고, 저는 종종 또 다른 기술을 배우는 중인 고령자들로부터 이러한 주장을 듣습니다. 이들은 여러 분야에 대한 폭넓은 지식을 바탕으로 새로운 기술을 배우려는 것입니다. 그들이 아닌 시작 단계의 사람들로부터 그러한 주장을 듣지 못하는 것이 이상하다고 느낍니다.

다른 반론들은 종종 “그러면 어셈블리로 코딩하는 것이 좋은가요?”라는 질문을 던지며 논쟁을 합니다. 사람들은 높은 추상화 언어 계층을 채택하는 것과 질문을 중시하며 추상화와 추론보다는 심리 과정을 선호하는 것 사이에 구분을 보지 못할까요?

또한, 도움 요청하는 것은 즉각적인 코딩의 핵심 기술인 동시에 진화된 기술이라는 것을 인정하는 사람들이 많지 않습니다.

일반적인 메시지는 산업 혁명의 영향을 받았을 수 있지만, 산업혁명은 타협되지 않은 자원을 해제합니다. 여기서는 그렇지 않다는 것을 알아야 합니다.

<div class="content-ad"></div>

Generative AI, in essence, leverages familiar resources that have been previously investigated and utilized in unique ways, primarily drawing from the controversial extraction of public-domain resources at the origin and emphasizing use-cases to solidify established expertise at the final stage.

However, this entire procedure jeopardizes the initial phases of talent development and poses a risk of devaluing the pool of content creators who supply the raw material crucial for AI operations.

Surely, there must be a more optimal approach.