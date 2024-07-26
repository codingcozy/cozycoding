---
title: "contenteditabletrue 속성을 가진 div에 placeholder 텍스트 추가하는 방법"
description: ""
coverImage: "/assets/img/2024-07-26-Addplaceholdertexttodivwithcontenteditabletrue_0.png"
date: 2024-07-26 11:44
ogImage: 
  url: /assets/img/2024-07-26-Addplaceholdertexttodivwithcontenteditabletrue_0.png
tag: Tech
originalTitle: "Add placeholder text to div with contenteditabletrue"
link: "https://dev.to/axorax/add-placeholder-text-to-div-with-contenteditabletrue-aa6"
---


contenteditable 속성을 사용해본 적이 있을 수 있습니다. 많은 곳에서 사용됩니다. 이는 textarea와 같은 것보다 훨씬 더 좋은 대안입니다. 아무 div에 contenteditable="true"을 추가하면 입력 필드처럼 작동합니다. 이 글에서는 contenteditable이 기본적으로 placeholder 속성을 지원하지 않으므로 placeholder를 추가하는 방법을 보여드리겠습니다.

## 필요한 코드

```js
div[contenteditable] {
  &[placeholder]:empty::before {
    content: attr(placeholder);
    z-index: 9;
    line-height: 1.7;
    color: #555;
    word-break: break-all;
    user-select: none;
  }

  &[placeholder]:empty:focus::before {
    content: "";
  }
}
```

여기에 필요한 모든 코드가 있습니다! 그러나 CSS에만 해당 코드를 추가하면 작동하지 않습니다. HTML에 placeholder 속성을 추가하고 div가 보이게 하는 것도 필요합니다.

<div class="content-ad"></div>

## 전체 코드 / 예시

HTML

```js
<div contenteditable="true" placeholder="Axorax YouTube 채널을 구독해주세요! :D"></div>


CSS

<div class="content-ad"></div>

```
```js
div[contenteditable] {
  /* 기본 스타일 */
  width: 20rem;
  height: 15rem;
  padding: 1rem;
  background: #292929;
  color: #fff;

  /* 플레이스홀더 코드 */
  &[플레이스홀더]:empty::before {
    content: attr(플레이스홀더);
    z-index: 9;
    line-height: 1.7;
    color: #555;
    word-break: break-all;
    user-select: none;
  }

  &[플레이스홀더]:empty:focus::before {
    content: "";
  }
}
```

<img src="/assets/img/2024-07-26-Addplaceholdertexttodivwithcontenteditabletrue_0.png" />

이 정도입니다. 유용하게 사용하시기 바랍니다!
