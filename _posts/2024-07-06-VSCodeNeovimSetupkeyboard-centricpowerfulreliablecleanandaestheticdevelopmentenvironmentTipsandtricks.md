---
title: " VSCode와 Neovim 설정 키보드 중심, 강력하고 신뢰성 있는, 깔끔하고 미적인 개발 환경 설정 팁과 요령"
description: ""
coverImage: "/assets/img/2024-07-06-VSCodeNeovimSetupkeyboard-centricpowerfulreliablecleanandaestheticdevelopmentenvironmentTipsandtricks_0.png"
date: 2024-07-06 03:32
ogImage: 
  url: /assets/img/2024-07-06-VSCodeNeovimSetupkeyboard-centricpowerfulreliablecleanandaestheticdevelopmentenvironmentTipsandtricks_0.png
tag: Tech
originalTitle: "😎 VSCode + Neovim Setup: keyboard-centric, powerful, reliable, clean, and aesthetic development environment. Tips and tricks"
link: "https://medium.com/@nikmas_dev/vscode-neovim-setup-keyboard-centric-powerful-reliable-clean-and-aesthetic-development-582d34297985"
isUpdated: true
---





여기서는 현재 사용 중인 개발 환경 설정과 그 철학을 공유하려고 해요. 제 최신 방법과 통찰을 계속 업데이트해 나갈 거에요.

## 철학 정의

생산성, 성능, 신뢰성, 청결함, 미학 — 이 모든 게 중요하답니다.

### 생산성

<div class="content-ad"></div>

"생산성"이라고 말할 때, 코드를 편집하고 편집기를 빠르게 탐색할 수 있는 능력을 말합니다. 이 방식을 통해 여러분은 한 시간 안에 작업을 완료하고 나머지 일곱 시간을 바에서 팀 리드와 완벽한 프로그래밍 언어 (DreamBerd)를 논의하는 데 쏟을 수 있습니다. 그러나 여러분의 업무가 일곱 시간을 연구하고 10줄의 코드를 작성하는 데 한 시간을 쓰는 경우, 이 방식이 여러분에게 맞지 않을 수 있습니다. 어쨌든, 저는 프로젝트의 코딩 부분에서 효율적으로 전진하는 것이 멋지다고 생각하며, 주로 구현에 시간을 많이 할애할 때 흐름을 최적화하는 것이 자연스러울 것이라고 믿습니다.

코드를 편집하고 편집기를 신속하게 이동하기 위해, 제가 의존하는 접근 방식은 다음과 같습니다: 키보드를 사용하여 수행할 수 있는 모든 작업은 키보드를 사용해야 합니다. 어느 정도 훈련을 거친 후, 키보드에서 손가락의 움직임이 마우스를 움직이는 시도보다 빠를 것이라는 사실을 깨닫게 됩니다.

## 강점

"강점"이라고 말할 때, 편집기가 개발 프로세스 전반에 도움을 주는 기능으로 가득한 것을 의미합니다. 이에는 코드 제안, 자동 완성, 빠른 수정, 정의로 이동, 구문 강조, 커뮤니티 플러그인, AI 지원 등이 포함됩니다. 이러한 기능들은 개발 프로세스를 더 빠르고 즐거운 것으로 만들기 위해 설계되었습니다.

<div class="content-ad"></div>

"기본 기능인 코드 제안, 자동 완성, 빠른 수정 및 정의로 이동과 같은 핵심 기능은 보통 특정 언어 서버에 의해 구현됩니다. 코드 편집기는 Language Server Protocol(LSP)를 통해 이 언어 서버와 통신하며 LSP 클라이언트로 작동합니다.

## 신뢰성

‘신뢰성’이라고 말할 때는 여러분의 편집기 뒤에 큰 커뮤니티가 있다는 것을 의미합니다. 무언가 잘못되면 문제가 빨리 해결될 것을 기대할 수 있으며, 새로운 도구와 접근 방식이 나타나면 편집기에서 빠르게 구현될 것을 기대할 수 있습니다.

## 깔끔함"

<div class="content-ad"></div>

"청결함"이라고 말할 때 저는 작업 공간을 메우는 아이콘, 버튼 및 레이블이 많은 것을 좋아하지 않습니다. 특히 사용하지 않거나 중요하지 않은 것들이 더욱 그렇습니다. 개발 프로세스 중에는 코드 자체와 현재 파일 및 해당 경로만을 볼 수 있기를 선호합니다. 가끔 다른 것들을 볼 필요가 있을 때는 단축키를 사용하여 탐색합니다.

## 미적 감각

"Aesthetics(미적 감각)"이라고 말할 때, 사용자 인터페이스 및 타이포그래피를 설계하는 데 많은 시간을 할애했기 때문에 매 기능과 여백이 1픽셀 더 필요한 곳, 그리고 모든 맞지 않는 부분을 주목합니다. 따라서, 정교하게 디자인된 UI를 가진 편집기를 사용하는 것이 더욱 즐거운 인터페이스를 제공받을 수 있다고 생각합니다.

# 철학 실행

<div class="content-ad"></div>

이제 개발 환경 뒤에 있는 철학을 정의했으니, 구현 방안을 살펴보겠습니다. 여기서 제 목표는 완벽한 설정 방법을 제시하는 것이 아니라, 건설 및 발전할 수 있는 토대로 활용할 수 있는 프레임워크, 방법 및 기회를 보여드리는 것입니다.

그래서 우리의 구현은 이 네 가지를 기반으로 할 것입니다: 열 손가락으로 타이핑, VSCode, Neovim 및 키보드 바로 가기입니다.

## 열 손가락으로 타이핑

코딩 생산성의 기초는 열 손가락으로 모두 타이핑하는 능력에 있습니다. 타이핑 속도가 느리다면 코딩 및 빠르게 이동하는 능력에 크게 제약이 생깁니다. 이 병목 현상을 피하기 위해 열 손가락으로 타이핑하는 데 능숙해지는 것이 중요합니다. 그러니 이미 열 손가락 타자 중 하나가 아니라면, 가입하기를 강력히 권장합니다.

<div class="content-ad"></div>

## 가상 키보드

스위치를 한 지 얼마 안 됐다면 손가락이 새로운 위치와 동작에 익숙해지는 데 어려움을 겪을 수 있습니다. 처음에는 이전 방식보다 타이핑 속도가 더 느릴 것이며, 이 때 당신을 괴롭힐 수도 있습니다. 그러나 이 기간을 극복하면서 점차 타이핑 속도가 증가하는 것을 분명히 느낄 수 있을 것입니다. 장기적으로 큰 도움이 될 것입니다. 특정 연습을 통해 열 손가락 타이핑을 가르치는 온라인 교육 프로그램을 찾는 것을 추천합니다. 타이핑 속도를 빠르게 하는 데 도움이 되는 리소스는 다음과 같습니다: [Typing Study](https://www.typingstudy.com).

## VSCode

2023년 스택 오버플로 개발자 설문 조사에 따르면 Visual Studio Code (VSCode)가 가장 인기 있는 편집기로 나타났습니다. 이는 대규모 커뮤니티가 지지하고 있다는 것을 의미하는데, 이는 대개 우리가 필요로 하는 모든 것을 갖추고 있다는 것을 의미합니다. 많은 사람들이 이를 개선하고 기능을 추가하며, 기능을 향상시키는 플러그인을 만들고 있습니다. 또한, VSCode는 무료이며 멋진 UI를 갖추고 있습니다. Chrome이나 JetBrains IDE보다 용량을 많이 차지하지 않으므로 RAM을 많이 소비하지 않을 것입니다. 

단계 1. Visual Studio Code 설치하기

<div class="content-ad"></div>

가장 먼저 VSCode의 공식 웹사이트에 방문하여 운영 체제에 맞는 편집기를 다운로드하세요.

Step 2. Zen Mode를 활성화하고, Centered Layout 및 Hide Line Numbers 옵션을 해제하세요.

설치 후에는 VSCode가 아마 이렇게 보일 것입니다:

기본적으로 이미 깔끔하게 보이지만, 저는 조금 더 깔끔하게 만드는 것을 선호합니다. 이를 위해 먼저 Zen Mode를 활성화하세요. Cmd+K Z를 누르거나(View → Appearance으로 이동하여 Zen Mode를 선택)하여 이렇게 만드실 수 있습니다:

<div class="content-ad"></div>

중앙 정렬된 레이아웃을 선호하지 않는 저는 여기 너무 공허하고 슬픔을 느낍니다. 이 문제를 해결해 보겠습니다. 명령 팔레트를 열기 위해 cmd+shift+p를 누르고 open user settings (json)을 입력하세요. 해당 항목을 보면 enter를 눌러 해당 항목으로 이동합니다.

그 후, 열린 settings.json 파일에 2개의 옵션을 설정해야 합니다:

```js
"zenMode.centerLayout": false,
"zenMode.hideLineNumbers": false
```

결과는 다음과 같습니다:

<div class="content-ad"></div>

이제 좀 깔끔해 보이네요. 또한 cmd + h로 왼쪽의 주 사이드바를 토글할 수 있습니다.

항상 상단에 열린 파일 패널을 유지하는 것에 문제가 없구나. 그리고 더불어:

항상 표시할 필요가 없다고 생각되는 다른 아이콘이나 패널이 있다면 숨기는 것도 해보세요.

단계 3. 선호하는 다크 테마 찾기

<div class="content-ad"></div>

완벽한 색상 테마의 주요 조건은 어둡게 설정되어야 한다는 것입니다. 이는 Black Lives Matter에도 관련이 있고, 야간에 눈이 넓게 벌어지고 거의 감겨버릴 수 있는 광선 폭발로부터 보호해줍니다.

기본 VSCode 다크 테마는 거의 완벽합니다. 제가 마음에 들지 않는 유일한 것은 이 강조 색상입니다:

![2024-07-06-VSCodeNeovimSetupkeyboard-centricpowerfulreliablecleanandaestheticdevelopmentenvironmentTipsandtricks_0.png](/assets/img/2024-07-06-VSCodeNeovimSetupkeyboard-centricpowerfulreliablecleanandaestheticdevelopmentenvironmentTipsandtricks_0.png)

왜 그런지는 모르겠지만, 이 색은 나에게 해부실이나 최소한 병원을 떠올리게 합니다. 이 일에 관련해서 "이 일에 열받는다"는 듯한 기운이 난답니다. 그래서 Github Dark Default로 변경하기로 결정했습니다. 이것이 바로 모습입니다:

<div class="content-ad"></div>

이 테마는 더 어두우면서도 침대 밑처럼 보입니다. 그래야 합니다.

4단계. 편집기에 JetBrains Mono 폰트 설정하기

JetBrains 사용자 출신으로서 JetBrains Mono 폰트와 우정이 깊어서 VSCode으로 가져오기로 결정했습니다. 편집기에서 이 폰트를 사용하려면 공식 웹사이트에서 다운로드하여 설치한 후 다음과 같이 설정 파일(settings.json)에 설정하세요:

```js
"editor.fontFamily": "JetBrains Mono"
```

<div class="content-ad"></div>

저는 글꼴 리거처(글자 연결이미지)를 활성화하는 것도 추천해드립니다. 이렇게 러스트(Rust) 코드베이스에서의 화살표가 더 매력적으로 보일 거에요:

![Keyboard-Centric, Powerful, Reliable, Clean, and Aesthetic Development Environment](/assets/img/2024-07-06-VSCodeNeovimSetupkeyboard-centricpowerfulreliablecleanandaestheticdevelopmentenvironmentTipsandtricks_1.png)

VSCode에서 글꼴 리거처(글자 연결이미지)를 활성화하려면 settings.json으로 가서 이 줄을 추가하세요:

```js
"editor.fontLigatures": true
```

<div class="content-ad"></div>

**5. 스타쉽 프롬프트 설치 및 구성**

스타쉽을 사용하면 당신의 터미널 프롬프트를 더 유익하고 매력적으로 만들 수 있습니다. 제가 사용하는 것처럼요:

![image](/assets/img/2024-07-06-VSCodeNeovimSetupkeyboard-centricpowerfulreliablecleanandaestheticdevelopmentenvironmentTipsandtricks_2.png)

더불어, 스타쉽은 러스트로 백업되어 있습니다.

<div class="content-ad"></div>

먼저 설치 단계를 진행해주세요.

기본적으로 Starship 프롬프트는 많은 정보를 표시합니다. 그러나 저는 현재 git 브랜치와 git 상태만으로 충분하다고 생각했기에 Starship 프롬프트를 맞게 수정했습니다.

프롬프트를 수정하려면 다음 파일을 생성해주세요: ~/.config/starship.toml. 제 설정과 같이 Starship 프롬프트를 설정하고 싶다면 다음 옵션들을 정의해주세요:


format = """
$directory\
$git_branch\
$git_status\
$line_break\
$shell\
$character
"""

[directory]
truncation_length = 10 
truncate_to_repo = false


<div class="content-ad"></div>

## Neovim

Neovim은 당신을 Gigachad로 만들어주는 요소입니다. 이 도구는 당신을 영화 속 진짜 해커로 바꿔줍니다. Neovim과 함께하면 개발 프로세스가 초월적이 되고 편집 속도는 비길 데 없이 빨라집니다.

### 1단계. 핵심 vim 키 바인딩 익히기

Neovim에서 가장 매력적인 것은 그 핵심 vim 키 바인딩입니다. 이 명령어를 사용하여 키보드만으로 코드베이스를 탐색하고 편집하며, 코드 부분을 이동하고 재배치하고 강조할 수 있습니다. 이 모든 것을 수행할 때 마우스를 사용할 필요 없이 키보드만으로 작업할 수 있죠. Neovim에 입문한 지 얼마 되지 않았다면, 기본적인 키 조합을 가르쳐 주는 튜토리얼, 게임 또는 치트 시트를 찾는 것을 강력히 추천합니다. 여러 온라인 자원이 당신을 안내해 줄 것이니 마음에 드는 것을 찾아 시작해보세요. 손가락으로 타이핑하는 것과 마찬가지로 처음에는 빠르지 않을 수 있고 키패드를 몇 개 잊어버릴 수도 있지만, 시간이 지날수록 이러한 조합에 익숙해지고 손의 연장으로 활용할 수 있게 될 것입니다.

<div class="content-ad"></div>

## 단계 2. VSCode Neovim 확장 프로그램 설치하기

VSCodevim이 아니라 VSCode Neovim 확장 프로그램을 설치하세요! VSCode marketplace에서 설치하고 “시작 가이드/설치” 부분을 완료하세요! 이 확장 프로그램의 핵심 장점은 완전히 내장된 Neovim 인스턴스를 사용한다는 것입니다. vim 에뮬레이션이 아니라는 것입니다. 이는 사용자 정의 init.lua 및 다양한 플러그인을 지원한다는 것을 의미합니다. 동시에, 이 확장 프로그램은 삽입 모드 및 편집기 명령을 위해 VSCode의 기본 기능을 보존하여 양쪽의 장점을 최대한 활용합니다. VSCode Neovim 확장 프로그램 문서를 차근차근 읽어 보는 것을 강력히 추천합니다.

## 단계 3. 보통 터미널 Neovim을 사용하지 않는 이유 이해하기

나는 매번 맞춤형 터미널 Neovim 설정을 처음부터 만드는 여러 시도를 했지만, 매번 편집기가 자연스럽게 가져야 할 기본적인 기능을 구성하는 데 소요된 시간에 좌절했거나, 일부 플러그인이나 그들의 상호 운용성으로 인해 작동하지 않는 기능이나 눈에 띄는 지연 현상을 겪었거나, 마음에 들지 않는 UI 부분을 수정할 수 없다는 것을 발견했습니다.

<div class="content-ad"></div>

나는 터미널 Neovim을 편집기로 사용하는 시도 중에 겪은 불쾌한 측면들이 날 깨닫게 했어요. 나는 Neovim을 백엔드로만 사용하는 것을 선호하는 것 같아요. 나에게는 클라이언트 부분이 즉시 사용할 수 있는 기본기능을 갖춘 완전한 편집기이면서, 멋진 UI를 갖추고 내 요구에 맞게 쉽게 사용자 정의할 수 있으며, '설치' 버튼을 클릭하여 플러그인 기능을 쉽게 사용할 수 있는 큰 플러그인 생태계가 있는 것이 좋아요. 현재, 나는 Neovim의 클라이언트로 VSCode가 이 역할에 적합하다고 생각해요, 그래서 계속 진행할 거에요.

**단계 4. VSCode용 Neovim 설정**

이 단계에서는 기본 Neovim 구성을 만들 것이며, 여기서는 vscode-neovim 확장 API를 사용하여 Neovim에서 VSCode 명령을 호출하는 몇 가지 사용자 정의 기본 키 맵과 바인딩을 정의할 거에요.

먼저, ~/.config/nvim(또는 Windows의 경우 ~/AppData/Local/nvim)로 이동하여 다음 파일 구조를 생성하세요:

<div class="content-ad"></div>

**init.lua** 파일에 아래 코드를 붙여넣으세요:

```js
if vim.g.vscode then
  -- VSCode Neovim
  require "user.vscode_keymaps"
else
  -- 일반 Neovim
end
```

VSCode와 관련된 설정을 일반 설정과 분리하여 각 환경에 미세한 제어를 할 수 있게 하고 문제를 방지할 수 있습니다.

이제 **vscode_keymaps.lua** 파일로 이동하여 몇 가지 기본 키맵을 정의해봅시다. 제 설정에 있는 몇 가지 예시를 보겠습니다:

<div class="content-ad"></div>

```js
local keymap = vim.keymap.set
local opts = { noremap = true, silent = true }

-- leader 키 다시 매핑하기
keymap("n", "<Space>", "", opts)
vim.g.mapleader = " "
vim.g.maplocalleader = " "

-- 시스템 클립보드에 복사하기
keymap({"n", "v"}, "<leader>y", '"+y', opts)

-- 시스템 클립보드에서 붙여넣기
keymap({"n", "v"}, "<leader>p", '"+p', opts)

-- 들여쓰기 처리 더 잘하기
keymap("v", "<", "<gv", opts)
keymap("v", ">", ">gv", opts)

-- 텍스트를 위아래로 이동하기
keymap("v", "J", ":m .+1<CR>==", opts)
keymap("v", "K", ":m .-2<CR>==", opts)
keymap("x", "J", ":move '>+1<CR>gv-gv", opts)
keymap("x", "K", ":move '<-2<CR>gv-gv", opts)

-- 붙여넣기 시 원래 복사한 내용 유지하기
keymap("v", "p", '"_dP', opts)

-- Vim 검색 후 하이라이팅 제거하기
keymap("n", "<Esc>", "<Esc>:noh<CR>", opts)
```

그리고 같은 파일에서, VSCode에 특화된 키 매핑을 추가해봅시다. 이러한 키 바인딩을 통해 VSCode 작업을 트리거할 수 있습니다. 내가 현재 사용 중인 키 바인딩은 다음과 같습니다:

```js
-- Neovim으로부터 vscode 명령 호출하기
keymap({"n", "v"}, "<leader>t", "<cmd>lua require('vscode').action('workbench.action.terminal.toggleTerminal')<CR>")
keymap({"n", "v"}, "<leader>b", "<cmd>lua require('vscode').action('editor.debug.action.toggleBreakpoint')<CR>")
keymap({"n", "v"}, "<leader>d", "<cmd>lua require('vscode').action('editor.action.showHover')<CR>")
keymap({"n", "v"}, "<leader>a", "<cmd>lua require('vscode').action('editor.action.quickFix')<CR>")
keymap({"n", "v"}, "<leader>sp", "<cmd>lua require('vscode').action('workbench.actions.view.problems')<CR>")
keymap({"n", "v"}, "<leader>cn", "<cmd>lua require('vscode').action('notifications.clearAll')<CR>")
keymap({"n", "v"}, "<leader>ff", "<cmd>lua require('vscode').action('workbench.action.quickOpen')<CR>")
keymap({"n", "v"}, "<leader>cp", "<cmd>lua require('vscode').action('workbench.action.showCommands')<CR>")
keymap({"n", "v"}, "<leader>pr", "<cmd>lua require('vscode').action('code-runner.run')<CR>")
keymap({"n", "v"}, "<leader>fd", "<cmd>lua require('vscode').action('editor.action.formatDocument')<CR>")
```

모든 설정이 완료되면, VSCode를 재시작하면 커스텀 키 매핑이 작동할 것입니다.

<div class="content-ad"></div>

### 단계 5. 코드 탐색 키 바인딩 탐색하기

VSCode Neovim 확장 프로그램 설명서의 해당 섹션에서 코드 탐색을 위한 미리 정의된 모든 키 바인딩 목록을 찾을 수 있습니다. 여기서 가장 중요하고 자주 사용하는 것들을 강조해 보겠습니다:

- gd — 정의로 이동하기;
- ctrl + o — 정의에서 돌아오기;
- Space + d — 현재 커서가 위치한 항목에 대한 세부 정보를 보여주는 hover 표시 (우리의 사용자 정의 `vscode_keymaps.lua`에서);
- Shift + k — hover가 표시되지 않으면 표시되고, 이미 표시된 경우에는 이에 초점을 맞추고, 그 후에 vim 명령인 j, k, gg, G를 사용하여 스크롤할 수 있습니다.
- Space + a — 퀵 픽스 목록을 보여줍니다 (우리의 사용자 정의 `vscode_keymaps.lua`에서). 또한 ctrl + n 및 ctrl + p를 사용해 목록을 이동하고, 원하는 작업을 선택하려면 Enter를 누를 수 있습니다.

### 단계 6. 탐색기 탐색 및 파일 조작 키 바인딩 조사하기

<div class="content-ad"></div>

한 번 cmd + shift + e를 눌러 익스플로러에 집중하면, 해당 부분을 탐색하고 폴더를 확장하거나 축소할 수 있습니다. 또한 특정 키 바인딩을 사용하여 파일 및 폴더를 선택하고, 생성하고, 이름을 바꾸고, 삭제하고, 복사하고, 이동할 수 있습니다.

VSCode Neovim 확장 도큐먼트의 익스플로러 탐색 섹션에서는 VSCode 익스플로러를 통해 이동하기 위한 모든 미리 정의된 키맵을 찾을 수 있습니다. 또한 파일 조작 섹션에서는 익스플로러 내의 파일 및 폴더에 작업을 수행하는 모든 바인딩을 찾을 수 있습니다. 여기서 가장 유용한 것들 몇 가지를 강조해 보겠습니다:

- j 및 k — 파일 및 폴더 간 아래로 및 위로 초점을 맞춥니다;
- h 및 l — 폴더를 축소하고 선택하거나 파일을 선택합니다;
- enter — 폴더 및 파일을 선택합니다;
- gg — 익스플로러의 첫 번째 항목에 초점을 맞춥니다;
- G — 익스플로러의 마지막 항목에 초점을 맞춥니다;
- a — 새 파일을 생성합니다;
- A — 새 폴더를 생성합니다;
- r — 파일/폴더의 이름을 변경합니다;
- d — 파일/폴더를 삭제합니다;
- y — 파일/폴더를 복사합니다;
- x — 파일/폴더를 잘라냅니다;
- p — 파일/폴더를 붙여넣습니다.

## 키보드 단축키

<div class="content-ad"></div>

**가장 중요한 포인트 중 하나로 남겨두죠.**

**첫 번째 단계 및 유일한 단계. 키보드를 중심으로 세계를 만드세요**

**5.**
**작업 공간에서 특정 위치로 이동하거나 작업을 수행해야 할 때, 마우스를 직접 잡지 말고 키보드 조합을 사용해 원하는 결과를 얻는 방법을 구글링해보세요. 이전에 말했듯이, 키 바인딩에 익숙해지면 마우스보다 작업을 훨씬 빠르게 처리할 수 있습니다.**

**오늘의 마지막 팁은:**

<div class="content-ad"></div>

If you're interested in exploring more of my software engineering content and staying updated on the latest news, feel free to subscribe to my Telegram channel [here](https://t.me/nikmas_group).

And for some programming humor and memes, check out my other Telegram channel [here](https://t.me/nikmas_group_humor).