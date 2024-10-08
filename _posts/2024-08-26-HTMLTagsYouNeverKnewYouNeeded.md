---
title: "개발자들이 잘 모르는 10가지 HTML 태그"
description: ""
coverImage: "/assets/img/2024-08-26-HTMLTagsYouNeverKnewYouNeeded_0.png"
date: 2024-08-26 17:39
ogImage: 
  url: /assets/img/2024-08-26-HTMLTagsYouNeverKnewYouNeeded_0.png
tag: Tech
originalTitle: "HTML Tags You Never Knew You Needed"
link: "https://medium.com/@jiadalfahmid/html-tags-you-never-knew-you-needed-30c06c0ba935"
isUpdated: true
updatedAt: 1724743546838
---


<img src="/assets/img/2024-08-26-HTMLTagsYouNeverKnewYouNeeded_0.png" />

안녕하세요! HTML은 기본적인 것 같지만, 웹 페이지를 변화시킬 수 있는 숨겨진 태그의 보물 창고가 있다고 말해드릴게요. 이 태그들은 첫 날에 배우는 것이 아니라, HTML 기술을 다음 수준으로 끌어올릴 수 있는 비밀 무기에요. 궁금하신가요? 함께 HTML에서 숨겨진 보석을 찾아보죠.

# 기본 사항: 요소와 태그

- 헤더와 단락: `h1`은 헤더에, `h2`는 부제목에, `p`는 단락에 사용해보세요. 이들을 `div`로 감싸서 정리해두면 좋아요.
- 스타일링: `strong`과 `em` 같은 태그는 텍스트의 모양 뿐만 아니라 중요성을 브라우저에 전달하여 접근성과 SEO에 영향을 줄 수 있어요.

<div class="content-ad"></div>

# Hidden Gems: Niche HTML Tags

일부 HTML 태그는 잘 알려지지 않았지만 굉장히 유용합니다. 웹 페이지를 더 상호 작용적이고 접근 가능하게 만드는 몇 가지 태그를 자세히 살펴보죠.

## 1. `abbr` — 약어 태그

누군가가 위에 마우스를 올리면 약어의 의미를 표시하는 방법이 궁금했던 적이 있나요? `abbr` 태그가 바로 그 역할을 합니다. 약어를 `abbr` 태그로 감싸고 전체 형태를 나타내는 제목 속성을 추가하면 됩니다. 이것은 접근성을 향상시키는 데 완벽한 방법이지만, 모바일 사용자는 호버 효과를 보지 못할 수도 있다는 점을 명심하세요.

<div class="content-ad"></div>

## 2. `code` – 코드 블록 표시하기

`p` 태그를 사용하여 코드를 모방하는 대신, `code` 태그를 사용하세요. 이 태그는 텍스트를 모노스페이스(font)로 자동 형식화하여 코드로 즉시 인식되도록 합니다. CSS와 함께 사용하여 외관을 더욱 향상시킬 수 있습니다.

## 3. `kbd` – 키보드 태그

단축키 또는 키를 표시할 때 `kbd` 태그를 사용하세요. `code` 태그와 같이 텍스트를 모노스페이스(font)로 감싸 코드로 만들며, 이를 CSS로 더욱 스타일링할 수 있습니다.

<div class="content-ad"></div>

## 4. `datalist` 및 `option` — 동적 입력 제안

사용자에게 입력하는 동안 제안이나 드롭다운 메뉴를 제공하고 싶나요? `datalist` 태그를 `option` 태그와 결합하여 이를 간단하게 할 수 있습니다. 입력 필드를 생성하고 datalist에 연결하면 사용자의 입력에 따라 제안이 나타납니다.

## 5. `dialog` — 빠른 팝업

빠른 팝업이나 모달이 필요한가요? `dialog` 태그를 사용하면 간단해집니다. 내용을 `dialog` 태그로 둘러싸고 약간의 JavaScript로 열 수 있습니다. 모형 작성이나 UI 요소 테스트에 이상적입니다.

<div class="content-ad"></div>

## 6. `details` 와 `summary` — 내장 드롭다운

이러한 태그들은 JavaScript나 CSS가 필요하지 않은 드롭다운 메뉴를 만듭니다. FAQ 섹션이나 콘텐츠의 가시성을 토글하는 데 사용할 수 있습니다. `details` 태그는 컨테이너로 작용하고, `summary` 태그는 클릭할 수 있는 제목 역할을 합니다. 정보를 구성하는 사용자 친화적인 방법입니다.

## 7. `time` — 시간 요소

`time` 태그는 소박하지만 강력합니다. 날짜와 시간을 실제 데이터 포인트로 이해하도록 검색 엔진을 돕습니다. 날짜나 시간을 이 태그로 감싸면, SEO가 약간 향상됩니다.

<div class="content-ad"></div>

## 8. `ruby`, `rt`, 그리고 `rp` — 인라인 주석

이 태그들은 단어 위에 작은 텍스트와 같은 주석을 표시하는 데 탁월합니다. 처음에는 아시아 언어에 사용되었지만, 다른 목적으로도 창의적으로 사용할 수 있습니다. `ruby` 태그는 래퍼(wrapper)이고, `rt`는 주석(annotation)을 제공하며, `rp`는 지원되지 않는 브라우저에 대한 대체 텍스트를 제공합니다.

## 9. `progress` — 진행률 표시줄

간단하고 CSS가 필요 없는 진행률 표시줄을 원하나요? `progress` 태그를 사용하세요. 최대(max)와 값(value) 속성을 설정하여 표시줄의 완료 수준을 정의하면 됩니다. 그럼 끝입니다.

<div class="content-ad"></div>

## 10. `미터` — 시각적 스케일

`progress`와 유사하게, `meter` 태그는 값의 범위를 시각화하는 데 사용됩니다. 낮은, 높은 및 최적 범위를 정의할 수 있으며 미터는 값에 따라 색상을 자동으로 조정합니다. 성능이나 결과를 보여주는 데 완벽합니다.

## 11. `fieldset`와 `legend` — 양식 요소 그룹화

관련된 양식 요소를 그룹화하려면 `fieldset`를 사용하십시오. 내부에 `legend`를 추가하여 그룹의 제목을 지정하면 자동으로 필드 세트의 테두리 내에 나타납니다. 양식을 깔끔하게 정리하는 좋은 방법입니다.

<div class="content-ad"></div>

# 마무리

HTML을 배우는 것은 기본을 이해하는 것 이상의 의미가 있습니다. 이러한 잘 알려지지 않은 태그들은 웹 페이지에 기능성, 접근성 및 스타일을 손쉽게 더할 수 있습니다. 도움이 되었다면 좋아요 버튼을 꾹 눌러주시고 웹 개발 및 코딩 여정에 관한 더 많은 콘텐츠를 구독해보세요. 읽어 주셔서 감사합니다. 다음 글에서 다시 만나요!

함께 배우고 성장해요!