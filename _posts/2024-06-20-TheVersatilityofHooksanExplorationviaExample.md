---
title: "훅의 다양성 예시를 통한 탐구"
description: ""
coverImage: "/assets/img/2024-06-20-TheVersatilityofHooksanExplorationviaExample_0.png"
date: 2024-06-20 14:10
ogImage: 
  url: /assets/img/2024-06-20-TheVersatilityofHooksanExplorationviaExample_0.png
tag: Tech
originalTitle: "The Versatility of Hooks: an Exploration via Example"
link: "https://medium.com/@heydarianc/the-versatility-of-hooks-an-exploration-via-example-f7b602eb2e0b"
isUpdated: true
---





## 소개: 프리랜스 React 기회

아리조나 주립대학교에서 그래픽 정보 기술 석사 과정을 졸업한 후, 전방향 개발 기술을 강화하여 더 다재다능한 UX 디자이너가 되고 싶었습니다. React는 인기 있는 프레임워크였기 때문에 문서를 읽고 여러 튜토리얼을 실행하는 것으로 시작했습니다. 튜토리얼을 몇 개 완료한 후, 프리랜스 프론트엔드 개발의 세계로 발을 들이게 되었습니다.

내 첫 번째 프리랜스 프로젝트 중 하나는 별자리 계산기인 AstronoMe이었습니다. 클라이언트는 사용자에게 별자리를 알려주고 성격과 특이한 점에 대한 추가 정보를 제공하는 웹 애플리케이션을 원했습니다. 기본적인 디자인 미팅을 거친 후 클라이언트와 합의한 바에 따르면 프로젝트는 멀티페이지 React 애플리케이션이어야 했습니다. AstronoMe은 "홈", "퀴즈", "결과" 페이지로 이루어진 총 3개의 페이지 애플리케이션이었습니다. 독자들은 애플리케이션의 코드를 따라 가며 UI를 검토하기 위해 아래 링크를 클릭할 수 있습니다: AstronoMe 애플리케이션 링크와 AstronoMe 코드 링크

## 구현: AstronoMe 웹 애플리케이션

<div class="content-ad"></div>

AstronoMe 웹 애플리케이션은 함수형 컴포넌트를 사용하여 구축되었습니다. 함수형 컴포넌트는 유연하며 클래스 컴포넌트보다 더 간편한 방법으로 컴포넌트를 작성하는 방식입니다. 함수형 컴포넌트는 Hooks에 접근할 수 있어 상태(state)와 효과(effects)를 조작할 수 있는 함수를 사용할 수 있습니다. 이 문서는 "퀴즈" 및 "결과" 페이지의 구현과 Hooks에 의존하는 부분에 초점을 맞춥니다.

"퀴즈" 페이지에는 두 개의 큰 부트스트랩 "select" 요소와 사용자를 "결과" 페이지로 이동시키는 버튼이 특징으로 나타납니다. 두 "select" 요소를 사용하여 사용자가 태어난 월과 일을 선택할 수 있습니다. "결과" 페이지에는 사용자의 별자리와 해당 별자리에 해당하는 정보가 들어있는 컴포넌트가 표시됩니다.

AstronoMe 웹 애플리케이션의 구현은 "useState," "useEffect," "useNavigate," 그리고 "useLocation" 네 가지 Hooks에 의존합니다.
## Hooks에 대해 더 알아보기

<div class="content-ad"></div>

"useState" Hook을 사용하여 사용자의 출생 월과 출생 날짜 변수의 값을 설정했습니다. 또한 사용자가 출생 일자를 선택한 후 사용자의 별자리를 설정하는 데도이 Hook을 사용했습니다.

"useEffect" Hook은 페이지를 렌더링 한 후 if-else 문에서 비교 작업을 수행하는 데 사용되었습니다. 이러한 문에서 "signState"가 설정되었습니다.

<img src="/assets/img/2024-06-20-TheVersatilityofHooksanExplorationviaExample_0.png" />

"useNavigate" Hook을 사용하여 사용자를 "Result" 페이지로 이동하면서 해당 페이지로 데이터를 전달했습니다. 여기서 "navigate" 함수가 매우 중요했는데, 이는 다양한 signature를 갖고 있기 때문입니다. 이 구현에서는 두 개의 인수를 사용한 signature를 사용했는데, URL 및 추가 매개 변수였습니다. "Quiz" 페이지의 경우, 두 번째 매개 변수는 상태 객체의 "signState"였습니다. "signState"는 "Result" 페이지로 전송되어 사용자에게 렌더링 될 메시지 구성 요소를 결정하는 데 사용되었습니다.

<div class="content-ad"></div>

<img src="/assets/img/2024-06-20-TheVersatilityofHooksanExplorationviaExample_1.png" />

마지막으로, "useLocation" Hook은 "Result" 페이지에서 "Result" 페이지로 전송된 상태 객체에 액세스하는 데 사용되었습니다. "signState"는 상태 객체에서 비구조화되어 사용되어 사용자에게 개성 정보를 렌더링하는 데 사용되었습니다.

<img src="/assets/img/2024-06-20-TheVersatilityofHooksanExplorationviaExample_2.png" />

## 훅을 사용하여 오류 처리하기

<div class="content-ad"></div>

웹 애플리케이션에서 오류 가능성이 매우 낮았습니다. 사용자가 옵션을 선택하는 것으로 제한되어 데이터 입력으로 인해 발생할 수 있는 문제를 피할 수 있었습니다. 처음에는 "선택" 요소 두 개를 모두 비워 둔 경우에 대한 오류 케이스가 식별되었습니다. 나중에 테스트를 진행하면서 예전 동료 중 한 명이 "선택" 요소 중 하나를 선택하지 않은 채 다른 하나는 선택한 경우의 추가 오류 케이스를 지적했습니다. 이로 인해 "결과" 페이지의 렌더링이 예측할 수 없게 되었습니다.

이 오류를 해결하기 위해 "Quiz" 페이지의 else 문에 "signState"를 빈 문자열로 설정했습니다. 게다가 오류 컴포넌트의 메시지를 업데이트하여 두 가지 오류 케이스를 모두 다루는 언어를 사용했습니다. 사용자를 "Quiz" 페이지로 되돌리는 버튼의 "onClick" 이벤트 내에서 "useNavigate" 훅을 사용했습니다. 이러한 오류 컴포넌트의 변경으로 사용자가 무엇이 잘못되었는지 진단하고 빠르게 오류를 해결할 수 있는 기회를 제공했습니다.

## 결론: Hooks의 힘

Hooks를 사용하면 React 애플리케이션의 라이프사이클의 다양한 단계에서 변수를 조작하고 변경을 실행할 수 있는 능력을 갖게 됩니다. 이는 Java에서의 간단한 설정 방법과 유사하게 작동하며 어떤 함수가 실행되는 시점도 결정할 수 있습니다. Hooks는 매우 다양하며 함수형 컴포넌트를 React 개발에서 강력한 자산으로 만듭니다.

<div class="content-ad"></div>

Hooks에 대해 자세히 알아보려면 React Hooks를 소개하는 기사 링크를 확인해보세요!