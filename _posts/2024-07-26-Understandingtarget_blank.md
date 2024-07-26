---
title: "target_blank 속성의 이해와 사용법"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-07-26 11:46
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Understanding target_blank"
link: "https://dev.to/bridget_amana/understanding-targetblank-m12"
---


텍스트 링크의 target="_blank"에 관해 읽은 이후에 접근성과 사용자 편의성을 고려해 만들어진 것을 이해하게 되었어요. 이 속성의 주요 목적은 링크를 새 탭에서 열 수 있게 해서 사용자가 원래 페이지의 이용성을 유지할 수 있는 데에 도움을 주는 것이죠. 그러나 모든 상황에 적합한 것은 아니며 중요한 보안 고려 사항이 따르는 겁니다. 언제 target="_blank"를 사용할 지, 그리고 그 단점에 대해 더 깊이 알아볼까요?

#### target="_blank"의 목적

HTML 앵커(`a`) 태그의 target="_blank" 속성은 브라우저가 연결된 문서를 새 탭에서 열도록 지시하는 것입니다. 이는 원래 페이지를 방해받지 않고 사용자가 브라우징을 계속할 수 있도록 사용자 경험을 향상시킬 수 있어요. 여기 간단한 예시가 있어요:

```js
<a href="https://www.learnmore.com" target="_blank">Learn more</a>
```

<div class="content-ad"></div>

#### target="_blank"를 적절히 사용하는 경우

처음에는 제가 작업 중인 모든 프로젝트에서 모든 (`a`) 요소에 target="_blank"를 사용했습니다. 하지만, 모든 페이지가 다른 탭에서 열 필요가 없다는 것을 깨달았어요. 이렇게 사용해야 하는 경우는 아래와 같아요:

- 외부 링크: 외부 웹사이트로 연결하는 링크는 target="_blank"를 사용하는 것이 좋습니다. 외부 링크를 새 탭에서 열면 사용자가 사이트를 떠나지 않고도 쉽게 여러분의 사이트로 돌아올 수 있어요.
- 보충 정보: 링크가 현재 콘텐츠를 보충하는 추가 정보를 제공하는 경우 새 탭에서 열면 유용할 수 있어요. 예를 들어 용어 해설 또는 현재 페이지의 이해를 높이는 관련 기사에 대한 링크일 경우입니다.
- 양식 및 문서: 양식, 문서 또는 다른 다운로드 가능한 콘텐츠에 대한 링크는 새 탭에서 열면 편리합니다. 사용자가 현재 보고 있는 페이지를 벗어나지 않고 양식을 다운로드하거나 작성할 수 있어요.

하지만, 모든 링크가 새 탭에서 열 필요는 없어요. target="_blank"를 사용해서는 안 되는 경우도 있어요:

<div class="content-ad"></div>

- 내부 링크: 동일한 사이트 내에서 이동하는 링크는 일반적으로 target="_blank"를 사용해서는 안 됩니다. 이러한 링크는 사용자가 사이트를 원확하게 이동하도록 도와야 하며 그들의 브라우징 경험을 조각내지 않아야 합니다.
- 내비게이션 링크: 메뉴 항목이나 내비게이션 링크는 일반적으로 일관된 사용자 경험을 유지하기 위해 동일한 탭에서 열어야 합니다.

#### target="_blank" 사용의 단점

target="_blank"를 사용하는 것은 유익할 수 있지만 잠재적인 보안 취약점을 도입할 수도 있습니다:

- 보안 위험: 탭나빠짐(tabnabbing)이 주요 위험 중 하나입니다. target="_blank"로 열린 새 탭이 원본 페이지를 조작하여 악의적인 사이트로 리디렉트시킬 수 있습니다. 이것은 사기 공격에 악용될 수 있습니다.
- 사용자 제어: 사용자 환경설정을 무시하는 것은 침입적일 수 있습니다. 사용자는 종종 브라우저 설정이나 바로 가기를 사용하여 링크가 열리는 방식을 결정하는 것을 선호합니다.

<div class="content-ad"></div>

#### noopener 및 noreferrer를 사용하여 보안 강화하기

target="_blank"와 관련된 보안 위험을 완화하기 위해서는 rel="noopener noreferrer" 속성을 사용하는 것이 중요합니다:

- noopener: 이를 통해 새로 열린 페이지가 window.opener 속성에 접근하는 것을 막아 원래 페이지의 잠재적인 조작을 방지합니다. 이것은 tabnabbing에 대한 주요 방어 수단입니다.
- noreferrer: 이 속성은 새 페이지가 참조 페이지에 대한 어떤 정보도받지 않도록 보장합니다. 이것은 비교적 중요도가 낮을 수 있지만, 또 다른 개인 정보 보호와 보안을 추가하는 레이어를 제공합니다.

보안을 강화하기 위해 링크를 구성하는 방법은 다음과 같습니다:

<div class="content-ad"></div>


[Visit Example](https://www.example.com){:target="_blank" rel="noopener noreferrer"}


noopener와 noreferrer를 결합하여 target="_blank"의 이점을 제공하면서도 사이트와 사용자를 잠재적인 위협으로부터 안전하게 보호할 수 있습니다.

이전 기사에서 다른 target 속성 값에 대해 이야기한 내용을 확인하세요.

다음에 만날 때까지, 당신의 동네 웹 개발자 🫡
