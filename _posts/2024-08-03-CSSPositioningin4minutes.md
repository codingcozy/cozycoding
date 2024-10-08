---
title: "4분 안에 배우는 CSS 요소 정렬 방법"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-03 18:22
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "CSS Positioning in 4 minutes"
link: "https://medium.com/@aravind16101800/css-positioning-in-4-minutes-e15f798c26c6"
isUpdated: true
updatedAt: 1724246417614
---



![image](https://miro.medium.com/v2/resize:fit:692/1*LwYwQwvunsVYyr75FeslvA.gif)

CSS나 CSS 작업을 아직 많이 하지 않은 경우 요소의 위치 지정에 어려움을 겪을 수 있습니다. 당신의 삶을 쉽게 만들기 위해 CSS 위치 지정 속성인 Absolute, Relative, Static, Fixed, Sticky 및 Inherit와 같은 것들이 있습니다. 이 게시물에서는 여섯 가지 CSS 위치 지정 속성과 예제를 통해 어디에 사용해야 하는지 배워보겠습니다. 이 게시물의 비디오 버전이 필요하다면 나의 유튜브 비디오를 참조하세요.

# 1. Static

기본적으로 모든 HTML 요소는 정적 위치를 갖습니다. 즉, 요소는 일반 페이지 흐름에 고정되며 그 배치는 문서 구조에 의해 결정됩니다. 왼쪽, 오른쪽, 위, 아래 및 z-index 속성은 정적 위치를 갖는 요소에 영향을 미치지 않습니다. 본질적으로 이러한 요소들은 위치 지정 속성이 존재하지 않는 것처럼 동작합니다.


<div class="content-ad"></div>

# 2. Relative

상대 위치를 가지는 요소는 정적 요소처럼 문서 흐름에서 원래 위치를 유지합니다. 그러나 정적 위치와 달리 좌우, 상하, z-index 속성이 영향을 미칩니다. 이러한 속성들은 요소를 지정된 방향으로 "움직입니다".

# 3. Absolute

절대 위치를 가지는 요소는 일반적인 문서 흐름에서 제거됩니다. 다른 요소들은 절대 위치를 가진 요소가 존재하지 않는 것처럼 작동합니다. 이를 통해 좌우, 상하, z-index 속성을 사용하여 정확한 배치가 가능합니다. 요소는 가장 가까운 상대 위치 조상(즉, 정적 이외의 위치 속성을 가진 상위 요소)을 기준으로 배치되거나 해당 조상이 없는 경우 초기 포함 블록을 기준으로 배치됩니다.

<div class="content-ad"></div>

# 4. Fixed

고정 위치를 갖는 요소는 절대 위치를 갖는 요소와 마찬가지로 문서 흐름에서 제거됩니다. 그러나 절대 위치와 달리 고정 위치는 뷰포트에 상대적이며 특정 부모 요소에 상대적이 아닙니다. 이는 페이지를 스크롤해도 해당 요소가 같은 위치에 남아 있어서 지속적인 내비게이션 막대나 고정 헤더를 만드는 데 유용합니다.

# 5. Sticky

sticky 위치 지정 값은 상대 및 고정 위치 지정의 측면을 결합합니다. sticky 위치 지정을 갖는 요소는 뷰포트의 스크롤 위치가 지정 임계값에 도달할 때까지 상대적으로 처리되다가 해당 지점에 도달하면 고정되어 뷰포트에 대해 해당 위치에 남습니다. 뷰에서 사라질 때까지 고정된 위치에 유지됩니다.

<div class="content-ad"></div>

# 6. 상속

position 속성의 inherit 값은 요소가 부모로부터 위치 값을 상속받도록 허용합니다. 특히 일관성이 중요한 복잡한 레이아웃에서 요소가 부모와 동일한 위치 지정 규칙을 따를 때 유용합니다.

# 결론

CSS 위치 지정을 이해하는 것은 유연하고 동적인 웹 레이아웃을 만드는 데 중요합니다. static, relative, absolute, fixed, sticky 및 inherit가 어떻게 작동하는지 알면 웹 페이지의 요소 배치와 동작을 더 잘 제어할 수 있습니다. 이러한 위치 지정 값들을 실험하여 디자인에 원하는 레이아웃과 기능을 구현해보세요.