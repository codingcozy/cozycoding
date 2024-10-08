---
title: "개발자가 WCAG을 읽는 방법"
description: ""
coverImage: "/assets/img/2024-06-22-HowadevelopershouldreadWCAG_0.png"
date: 2024-06-22 15:54
ogImage: 
  url: /assets/img/2024-06-22-HowadevelopershouldreadWCAG_0.png
tag: Tech
originalTitle: "How a developer should read WCAG"
link: "https://medium.com/user-experience-design-1/how-a-developer-should-read-wcag-b1aec621b9d2"
isUpdated: true
---






![이미지](/assets/img/2024-06-22-HowadevelopershouldreadWCAG_0.png)

# 왜 WCAG를 읽기 어려운가요

WCAG는 가독성으로 유명하지 않아요.

접근성에 대해 처음 알게 되었을 때, 읽어보려 했어요. 몇 번의 낮잠과 커피 뒤에 결국 포기했죠. 그 이유는 프로그래밍 언어나 마크업 언어를 배우려고 시도한 것처럼 읽으려고 했기 때문이에요.


<div class="content-ad"></div>

그래서 왜 안 될까요? 나는 스스로 공부한 개발자니까요. C 언어를 스스로 배울 수 있다면, 웹 접근성도 충분히 다룰 수 있을 거에요, 맞죠?

웹 접근성 세계로 뛰어들려고 하는 개발자이거나 좀 더 접근성 있는 콘텐츠를 제공하려는 분이라면, 아마도 WCAG에 가서 다른 튜토리얼이나 안내랑 비슷하겠거니 생각할 것입니다.

스포일러: 아닙니다.

이건 어려울 수 있어요. 복잡해서 그런 게 아니라, 굉장히 추상적이고 모호하게 쓰여져 있어서 읽기 어려울 수 있어요. 예시를 찾아야 하고, 그 용어들만큼은 그냥... 음.

<div class="content-ad"></div>

WCAG은 마크업, 스타일 또는 스크립트를 중심으로 하지 않습니다. 원칙을 중심으로 하고 있습니다. 이것을 이해하면 더 쉬워집니다.

저는 웹 개발을 처음 배울 때 책을 사용했어요. 웹의 역사, 웹 서버 동작 방식 및 다른 필수적인 내용을 다 읽었을까요? 저도 아니에요!

"어떻게 모든 것을 만드는지 보여줘!"

그래서 "첫 번째 웹 페이지 만들기" 섹션으로 스킵하고 거기서부터 시작했어요. 만약 접근성에 대해 이와 같은 것을 찾고 있다면, 이것이 당신을 위한 것입니다.

<div class="content-ad"></div>

# WCAG을 읽는 방법을 추천합니다

다음 링크를 새 탭에서 열고 시작해보세요: [WCAG 2.2](링크)

페이지의 왼쪽 탐색 창에서 전체 목차를 찾을 수 있습니다. 매우 익숙해지세요 - 자주 사용하게 될 거예요.

<div class="content-ad"></div>

페이지를 처음부터 끝까지 읽지 않는 것이 좋습니다. 대신에 추가적인 공부의 기초가 되는 가장 관련성이 높은 부분을 읽는 것을 제안합니다.

당신이 예전처럼 나와 같다면, "무엇을 해야 할까요...?"라는 질문에 대한 답변을 원할 것입니다. 문서 자체, 역사, 또는 원칙들에 대한 세부사항은 아닙니다. 불행히도, 원칙에서 벗어날 수 없습니다. 참고하세요. 일반 아이디어에서 시작해서 구체적인 실행으로 이어나가세요. 이렇게 하면 미래에 WCAG를 쉽게 탐색할 수 있게 될 것입니다.

다음은 WCAG를 읽기를 제안하는 순서입니다:

- 원칙 (일급 번호 매겨진 점): 웹 접근성의 기초를 제공하는 주요 아이디어
- 지침 (이중 번호 매겨진 점): 성공 기준을 그룹화하는 기본 접근성 목표
- 구체적인 용어
- 성공 기준 (가장 관련성이 높은 것들): 준수를 측정하는 검사 가능한 기준
- SC 페이지 이해

<div class="content-ad"></div>

다시 말해서: 먼저 넓게, 그 다음으로는 깊게 학습하세요.

어떤 것을 외우려고 하지 마세요. 이 안내서의 목적은 웹 접근성의 기본 원리를 이해하는 데 도움을 주는 것뿐입니다. 이 안내서를 완료하면, 필요한 구체적인 해결책을 쉽게 찾을 수 있을 것입니다.

## 원칙

아래에 나열된 원칙에서는, 웹 접근성 이해 안내서의 네 가지 원칙 섹션에서의 텍스트가 포함되어 있습니다.

<div class="content-ad"></div>

다음은 원칙들입니다:

- 인지 가능: 정보 및 사용자 인터페이스 구성 요소는 사용자가 인지할 수 있는 방식으로 제시되어야 합니다. 즉, 사용자는 제시되는 정보를 인식할 수 있어야 합니다 (모든 감각으로는 가시적이지 않아야 함).
- 작동 가능: 사용자 인터페이스 구성 요소 및 탐색은 작동 가능해야 합니다. 이는 사용자가 인터페이스를 작동할 수 있어야 함을 의미합니다 (사용자가 수행할 수 없는 상호 작용을 요구해서는 안 됨).
- 이해 가능: 정보 및 사용자 인터페이스의 작동은 이해할 수 있어야 합니다. 즉, 사용자는 정보뿐만 아니라 사용자 인터페이스의 작동도 이해할 수 있어야 합니다 (콘텐츠나 작동이 사용자의 이해를 넘어서서는 안 됨).
- 견고: 콘텐츠는 보조 기술을 포함한 다양한 사용자 에이전트에서 해석할 수 있는 충분히 견고해야 합니다. 이는 기술이 발전함에 따라 사용자가 콘텐츠에 접근할 수 있어야 함을 의미합니다 (기술 및 사용자 에이전트가 진화할 때에도 콘텐츠가 접근 가능하도록 유지되어야 함).

쉽죠?

WCAG가 사용자에 대해 언급할 때(예: 인지 가능: "... 사용자에게 제시되어야 함..."), 능력, 입력 메커니즘(마우스, 키보드, 음성 인식 등), 출력 메커니즘(모니터, 스크린 리더, 점자 디바이스 등)이 다양한 사용자를 가리킨다는 것을 기억해주세요.

<div class="content-ad"></div>

## 지침

한 단계 더 깊게 파는 것으로 가봅시다 — 바로 지침입니다. 두 번째 수준에서 (1.1 텍스트 대체, 1.2 시간 기반 미디어, 1.3 적응 가능 등) 이를 찾을 수 있을 겁니다.

WCAG 페이지와 이 페이지를 왔다갔다 하는 수고를 덜어드리기 위해, 여기에 그대로 적힌 내용을 안내해 드릴게요:

- 1.1 텍스트 대체: 비텍스트 콘텐츠에 대한 텍스트 대체물을 제공하여 큰 글씨, 점자, 음성, 기호 또는 간단한 언어 등 다른 형태로 변경할 수 있게 합니다.
- 1.2 시간 기반 미디어: 시간 기반 미디어에 대한 대체물을 제공합니다.
- 1.3 적응 가능: 정보나 구조를 잃지 않고 다른 방식(예: 간단한 레이아웃)으로 제공될 수 있는 콘텐츠를 생성합니다.
- 1.4 구별 가능: 전경과 배경을 분리하여 사용자가 내용을 쉽게 볼 수 있도록 합니다.
- 2.1 키보드 접근 가능: 모든 기능을 키보드로 이용할 수 있게 합니다.
- 2.2 충분한 시간: 사용자가 콘텐츠를 읽고 사용하는 데 충분한 시간을 제공합니다.
- 2.3 발작 및 신체 반응: 발작이나 신체 반응을 일으킬 수 있는 방식으로 콘텐츠를 디자인하지 않습니다.
- 2.4 탐색 가능: 사용자가 내비게이션을 도와주고 콘텐츠를 찾고 자신의 위치를 파악할 수 있는 방법을 제공합니다.
- 2.5 입력 방식: 키보드 이상의 다양한 입력을 통해 기능을 조작하기 쉽도록 합니다.
- 3.1 가독성: 텍스트 콘텐츠를 읽기 쉽고 이해하기 쉽도록 합니다.
- 3.2 예측 가능성: 웹 페이지가 예측 가능한 방식으로 표시되고 작동할 수 있도록 합니다.
- 3.3 입력 지원: 사용자가 실수를 피하고 수정하는 데 도움을 줍니다.
- 4.1 호환성: 보조 기술을 포함한 현재와 미래의 사용자 에이전트와의 호환성을 극대화합니다.

<div class="content-ad"></div>

"그런데 어떻게 해야 할까요?"라고 궁금해하고 있을 수 있습니다. 유감스럽게도 답을 얻으려면 조금 더 깊이 파고들어야 할 것입니다. 성공 기준에서도 종종 일반적인 답변만 제공됩니다. 하지만 걱정하지 마세요 — 이 기사의 뒷부분에서 예시를 보여드릴 거에요.

## 구체적인 용어

성공 기준을 다루기 전에 몇 가지 용어에 대해 알아두면 좋을 것 같아요. 알겠어요, 알겠어요 — "그냥 진행하자!"라고 생각할 수 있겠죠? 중요한데요, WCAG에서 사용하는 몇 가지 용어는 개발자들이 사용하고 있는 용어들과 다른 의미를 가지고 있습니다.

컴포넌트/사용자 인터페이스 컴포넌트

<div class="content-ad"></div>

사용자들에게 단일 기능을 제어하는 것으로 인식되는 콘텐츠의 일부입니다. 일반적으로 입력 컨트롤입니다.

컨텍스트 변경

보조 기술 사용자들에게 혼란을 일으킬 수 있는 쉬운 변경들 — 이는 새 탭에서 열기, 초점을 갖는 요소 변경, 또는 페이지를 크게 변경하는 것을 포함할 수 있습니다.

제목들

<div class="content-ad"></div>

`h1`부터 `h6`까지, `th` 요소의 "헤더"와 혼동되지 않도록 주의해주세요.

이름

프로그래밍적으로 지정된 레이블 텍스트입니다. 이는 레이블의 텍스트, aria-label 속성의 값 또는 aria-labelledby로 지정된 레이블로 사용되는 텍스트일 수 있습니다. 이것은 name 속성과 혼동해서는 안 됩니다.

역할

<div class="content-ad"></div>

요소의 목적입니다. 이는 기본 HTML 요소를 사용하지 않을 때에만 관련이 있습니다. 예를 들어 `select` 컨트롤 대신 `div`를 사용할 때입니다. `select` 컨트롤의 역할은 내재적인데 (역할에 대해 추가 조치가 필요하지 않음), `select` 역할을 하는 `div`는 역할 속성이 필요합니다.

## 성공 기준

일부 웹 접근성 옹호자들은 이에 동의하지 않을 수 있지만, 첫 번째로 웹 접근성 가이드라인의 일부 성공 기준만 살펴보는 것을 권장합니다. 이것은 WCAG와 웹 접근성에 대한 기초를 이해하는 데 좋은 기반이 될 것입니다.

항상 나중에 다시 찾아올 수 있습니다. 당신을 웹 접근성 옹호자로 만들려는 것이 아닙니다. WCAG를 읽는 데 이끌어주기 위해 노력하고 있을 뿐입니다. 적어도 당신이 코드에 작은 변경을 가하여 보다 접근성이 더 좋은 콘텐츠를 제공하도록 영감을 받기를 희망합니다.

<div class="content-ad"></div>

제 경험 상 가장 일반적으로 위배되는 기준이 포함된 Criteria를 포함했습니다. 내가 포함한 Criteria에 동의하지 않는다면 알려줘도 괜찮아.

성공 기준(SC), 준수 수준(A는 최소한의 수준이며, 따라서 가장 중요함), 그리고 기본 설명을 나열하고 있어. 나는 Criteria의 아이디어를 전달하기 위해 detail이 아닌 항목을 포함하지 않고 있어:

- SC 1.1.1 비텍스트 콘텐츠 (수준 A): 사용자에게 제공되는 모든 비텍스트 콘텐츠에는 해당 목적을 충족하는 대체 텍스트가 제공돼야 해 (예외 사항이 있음).
- SC 1.3.1 정보 및 관계 (수준 A): 프레젠테이션을 통해 전달되는 정보, 구조, 및 관계는 프로그램적으로 결정될 수 있거나 텍스트로 제공돼야 함.
- SC 1.3.3 감각적 특징 (수준 A): 내용을 이해하고 작동하기 위한 지침은 모양, 색상, 크기, 시각적 위치, 방향, 또는 소리와 같은 구성 요소의 감각적 특성에만 의존해서는 안 돼.
- SC 1.4.1 색상 사용 (수준 A): 정보 전달, 동작 표시, 응답 유도, 또는 시각적 요소 구별의 유일한 시각 수단으로 색상을 사용해서는 안 돼.
...

아직 따라 오고 있어? 멋져! 이제 본론으로 들어가 보자.



<div class="content-ad"></div>

## 성공 기준 페이지 이해하기

![이미지](/assets/img/2024-06-22-HowadevelopershouldreadWCAG_2.png)

각 성공 기준과 관련된 “이해” 링크가 함께 제공됩니다. 이를 통해 해당 기준 및 이를 충족하는 방법에 대한 매우 유용한 정보를 얻을 수 있습니다. 일반적으로 다음과 같은 내용을 포함합니다:

- 의도: 기준의 목적 및 해결을 시도하는 문제
- 혜택: 기준을 충족할 때 누가 혜택을 받고 어떻게
- 예시
- 자료: 기준에서 다루는 주제에 대한 비공식 자료
- 기법: 기준을 충족하는 해결책으로의 링크 — 일반적으로 충분한(기준을 충족하는 해결책), 자문(필수적이지 않은 기법, 단지 제안만), 실패(기준을 충족하지 못하는 예시)으로 구분됩니다. 특정 해결 방안을 찾고 있다면, 여기에서 찾을 수 있습니다.
- 주요 용어
- 테스트 규칙: 기준 테스트에 도움이 되는 부분적 검사 규칙입니다. 기억하세요: 테스트 규칙 중 하나라도 실패하면 관련 기준도 실패입니다 — 하지만 모든 테스트 규칙을 통과하는 것이 항상 기준을 충족한다는 의미는 아닙니다.

<div class="content-ad"></div>

실제 기준을 충족(또는 미달)한 예제를 찾고 싶다면, 해당 내용은 기법 섹션에서 찾을 수 있습니다.

“어떻게 충족시킬 것인지” 링크에 대해 궁금할 수도 있습니다. 저는 이 링크를 사용하지 않는데, 보통 “이해” 페이지에서 이미 제공되는 정보를 포함하고 있고, 자주 업데이트되지 않는 것으로 보입니다.

# 결론

웹 개발자이며 보다 접근성 있는 콘텐츠를 제공하려고 한다면, 감사합니다! 웹은 여러분과 같은 개발자가 더 많이 필요합니다.

<div class="content-ad"></div>

이 글은 개발자를 접근성 옹호자나 전문가로 만들기 위한 것은 아닙니다. 이 글은 올바른 일을 하려는데 WCAG의 분량과 언어에 압도되는 개발자들을 위해 썼어요.

사용법에 익숙해지면, 접근성 질문에 빨리 대답할 수 있게 될 거에요.

기억해 주세요: WCAG를 만족하는 것은 내용을 규정에 맞게 만드는 것뿐이고, 접근성은 단지 시작에 불과해요. 접근성은 개선하고 콘텐츠의 이용 가능성을 확대하기 위한 끝없는 노력이에요.

요약하면, WCAG는 시작에 불과해요.

<div class="content-ad"></div>

# 링크

## WCAG

- WCAG 2.2
- WCAG 2 개요
- WCAG 이해를 위한 소개
- WCAG 2.2 충족 방법 (빠른 참고)

## 더 많은 자료

<div class="content-ad"></div>

- 디지털 접근성으로 향하는 길, Xurxe Toivo García가 쓴 글
- 더 나은 개발자가 되는 방법: 접근성을 고려하여 빌드하기, India의 글

## 나의 관련 기사

- 접근성에 대한 관심은 필수 사항이 아닙니다
- 접근성 체크리스트 (제 1부): 일반적으로 놓치는 다섯 가지 항목
- 접근성 체크리스트 (제 2부): 일반적으로 놓치는 네 가지 항목
- WCAG 2.2 — 드디어 여기에 있습니다