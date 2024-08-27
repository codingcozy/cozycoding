---
title: "네오빔(Neovim)의 트러블 플러그인 문제를 해결하는 방법"
description: ""
coverImage: "/assets/img/2024-08-26-TheTroublePlugininNeovim_0.png"
date: 2024-08-26 18:08
ogImage: 
  url: /assets/img/2024-08-26-TheTroublePlugininNeovim_0.png
tag: Tech
originalTitle: "The Trouble Plugin in Neovim"
link: "https://medium.com/@adam-drake-frontend-developer/the-trouble-plugin-in-neovim-70978a360b63"
isUpdated: true
updatedAt: 1724742565847
---


<img src="/assets/img/2024-08-26-TheTroublePlugininNeovim_0.png" />

저는 한동안 Neovim에서 trouble.nvim 플러그인을 사용해왔는데, 개발 경험을 정말 향상시켜 주었습니다. 저는 대부분 Typescript로 작업을 하고 있는데, 그에 따라 하루 중에 많은 유형 오류를 만나게 됩니다.

큰 코드베이스에서 작업할 때 중요한 부분 중 하나는 업데이트해야 할 올바른 파일을 찾고, 그 파일에서 수정해야 할 올바른 코드를 찾는 것입니다. 여기서 Trouble이 필요한 것입니다. 현재 오류나 경고의 목록을 편리하게 제공하며, 빠르게 찾을 수 있도록 도와줍니다.

## Trouble과 함께하기

<div class="content-ad"></div>

파일에서 경고 또는 오류가 발생한 경우, 최신 버전에서는 환경 설정에 따라 변경 가능한 Trouble 창을 열 수 있습니다. 이 창에는 해당 파일(또는 Neovim에서 열린 모든 파일)에 있는 모든 오류/경고가 나열됩니다.

이 목록을 통해 특정 항목을 선택하면 그 코드의 특정 줄로 이동할 수 있습니다. 이렇게 하면 오류/경고가 있는 위치로 빠르고 쉽게 이동하여 한 번에 하나씩 수정할 수 있습니다.

## Trouble 설정하기

Trouble GitHub 페이지에는 Trouble 플러그인을 설정하고 구성하는 방법에 대한 많은 지침이 있습니다. Lazy 플러그인 관리자를 사용하는 경우 기본 설정 및 설치 방법은 다음과 같습니다:

<div class="content-ad"></div>

```json
{
  "folke/trouble.nvim",
  opts = {}, -- 기본 옵션의 경우 사용자 정의 설정 부분을 참고하십시오.
  cmd = "Trouble",
  keys = {
    {
      "<leader>xx",
      "<cmd>Trouble diagnostics toggle<cr>",
      desc = "진단 (Trouble)",
    },
    {
      "<leader>xX",
      "<cmd>Trouble diagnostics toggle filter.buf=0<cr>",
      desc = "버퍼 진단 (Trouble)",
    },
    {
      "<leader>cs",
      "<cmd>Trouble symbols toggle focus=false<cr>",
      desc = "심볼 (Trouble)",
    },
    {
      "<leader>cl",
      "<cmd>Trouble lsp toggle focus=false win.position=right<cr>",
      desc = "LSP 정의/참조 등 (Trouble)",
    },
    {
      "<leader>xL",
      "<cmd>Trouble loclist toggle<cr>",
      desc = "위치 목록 (Trouble)",
    },
    {
      "<leader>xQ",
      "<cmd>Trouble qflist toggle<cr>",
      desc = "퀵픽스 목록 (Trouble)",
    },
  },
}
```

솔직히 말해서, 나는 이 기본 구성을 바꿀 필요가 없었어. Trouble에서 얻는 주요 사용 예는 '진단 (Trouble)' 명령과 키맵 `leader`xx을 사용하는 것이야. 내 코드에 일부 형식 오류가 있을 때마다 이 키맵을 누르면 이러한 오류 목록이 별도의 창에 표시돼.

예를 들어, 아래와 같이 보일 거야:

<img src="/assets/img/2024-08-26-TheTroublePlugininNeovim_1.png" />


<div class="content-ad"></div>

저는 Vim 명령어를 통해 이 목록을 탐색하면 Neovim에서 열린 파일에 특정 코드가 강조됩니다. 이 플러그인 덕분에 코드에서의 오류와 경고를 효율적으로 탐색할 수 있게 되었습니다.

# 결론

코드에서의 오류는 즐겁지 않지만 개발 중에 이러한 오류를 발견하는 것이 중요합니다. 앱이 와일드에 출시되고 사용자가 오류를 발견할 때 기다리는 것보다 훨씬 나은 선택입니다.

Trouble은 이를 돕고 전반적인 개발 경험을 향상시켜줄 수 있는 플러그인입니다.

<div class="content-ad"></div>

# 제 주간 업데이트를 구독해보세요!

## 이 글이 마음에 드셨나요?

이 블로그 글이 도움이 되셨다면, 최신 콘텐츠를 받아보세요! 저의 최신 콘텐츠를 이메일로 받아보실 수 있습니다. 새로운 글은 매주 월요일 중앙 유럽 시간 오전 8시 30분에 게시됩니다.

## 무엇을 받게 될까요?

<div class="content-ad"></div>

가입하시면 다음과 같은 혜택을 누리실 수 있어요:

- 흥미로운 발견: 최신 도구와 라이브러리에 대한 정보를 가장 먼저 받아보세요.
- 해법 안내서: 단계별 기사로 개발 스킬을 향상시킬 수 있어요.
- 의견 글: 프런트엔드 개발 세계에 대한 사고를 자극하는 통찰론을 엿보세요.

## 저희 커뮤니티에 가입해보세요

저는 체코 공화국 프라하라는 활기차고 다채로운 도시에 가족과 함께 살고 있어요. 제 블로그는 단순히 기사뿐만 아니라 혁신과 학습을 사랑하는 유사한 개발자들이 모여있는 커뮤니티에요.

<div class="content-ad"></div>

## 나에 대해

저는 React와 TypeScript를 전문으로 하는 열정적인 프론트엔드 개발자입니다. 제 전문 경력은 자바스크립트 생태계 내에서 새로운 도구와 라이브러리를 탐험하고 정복하는 데 중점을 두고 있습니다.

제 블로그에서 원본 블로그 게시물을 확인해주세요.

관심이 있다면 LinkedIn과 Github를 확인해보세요!