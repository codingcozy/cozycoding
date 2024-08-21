---
title: "나를 3배 더 빠르게 만든 단축키들"
description: ""
coverImage: "/assets/img/2024-07-14-TheseShortcutsMadeMe3xFaster_0.png"
date: 2024-07-14 00:54
ogImage: 
  url: /assets/img/2024-07-14-TheseShortcutsMadeMe3xFaster_0.png
tag: Tech
originalTitle: "These Shortcuts Made Me 3x Faster"
link: "https://medium.com/gitconnected/these-shortcuts-made-me-3x-faster-8a5cb3f260e3"
isUpdated: true
---




![TheseShortcutsMadeMe3xFaster_0](/assets/img/2024-07-14-TheseShortcutsMadeMe3xFaster_0.png)

몇 년 전, 경험이 풍부한 엔지니어 한 명이 가능한 한 마우스 사용을 피하라고 조언해 주었습니다. 손을 마우스로 옮길 때마다, 나에게 필요한 지 여부를 묻거나 대신 키보드 단축키를 사용할 수 있는지 생각했습니다.

지난 몇 년 동안 그의 조언을 따라오면서, IntelliJ IDEA를 매일 사용할 것으로 예상되는 응용 프로그램을 매우 효율적으로 사용할 수 있게 되었습니다. 이 글에서는 내가 매일 사용하는 몇 가지 단축키를 다룰 것입니다.

IntelliJ IDEA를 사용하고 있지만, 이러한 단축키는 VSCode에서도 사용할 수 있다고 확신합니다.

<div class="content-ad"></div>

## 왜 단축키를 사용해야 할까요?

답은 간단합니다: 효율성 때문입니다. 소프트웨어 엔지니어로서, 대부분의 시간을 IDE를 사용하여 보낼 것입니다. 빨라지기 위한 모든 기술을 알고 있다면 매일 몇 초를 절약할 수 있습니다. 이는 시간이 지남에 따라 일, 심지어 월로 누적될 것입니다. 그러니 함께 알아봅시다.

## Command + W: 선택 확장

저가 가장 많이 사용하는 단축키 중 하나는 "선택 확장"입니다. 이 단축키를 누를 때마다 IntelliJ IDEA는 선택 영역을 더 큰 구문 단위로 확장합니다. 똑똑하게 계속해서 큰 코드 블록을 선택합니다.

<div class="content-ad"></div>

![GIF](https://miro.medium.com/v2/resize:fit:1400/1*nDN1DwG4dKa7eg_P8FlHyg.gif)

위의 GIF에서 보듯이 Command + W를 연속으로 누를 때마다 커서는 항상 같은 위치에 남아 있으며, 각 누름에 따라 더 많은 코드가 선택되어 코드 블록이 확장되는 방법을 보여줍니다.

## Command + 화살표 위/아래: 다음/이전 변경

파일에서 내가 한 변경 사항으로 이동하는 것을 종종 좋아합니다 (Git 차이를 기반으로). 이렇게 하면 파일을 빠르게 탐색하고 작업한 부분을 찾을 수 있습니다.

<div class="content-ad"></div>

![GIF of Git changes](https://miro.medium.com/v2/resize:fit:1400/1*Nz4nBOQTaHLUA5exGGlTBw.gif)

위의 GIF에서 파일의 두 변경 사항 사이를 어떻게 이동하는지 확인할 수 있습니다.

## Command + \: 오른쪽 분할

많은 경우에 오른쪽에 두 번째 편집기 보기를 열어 다른 파일을 확인하는 것을 좋아합니다.

<div class="content-ad"></div>

![Markdown](https://miro.medium.com/v2/resize:fit:1400/1*OsR4XIroz33I-0jXgcxwuA.gif)

다양한 편집기 간에 원활하게 이동하려면 몇 가지 단축키를 사용합니다.

- Command + \: 우측에 편집기 열기
- Command + E: 최근 파일 개요 열기. 추가 파일을 다른 편집기에서 열 수 있습니다.
- Command + Option + 오른쪽 화살표/왼쪽 화살표: 커서로 왼쪽 및 오른쪽 편집기 사이를 이동
- Command + Option + \: 우측 편집기 닫기

## Command + K: 작업 찾기

<div class="content-ad"></div>

IntelliJ IDEA에서는 자주 "동작 찾기" 단축키를 사용합니다. 이를 통해 IDE 내에서 다양한 작업을 효율적으로 수행할 수 있습니다. 주로 다음과 같은 간단한 Git 네비게이션 작업에 사용됩니다:

- 새 브랜치 생성
- 브랜치 전환
- 변경 사항 숨김/보관
- 변경 내용 커밋

이는 Git을 사용하는 가장 정교한 방법은 아니지만, IntelliJ IDEA의 내장 기능을 사용하면 빠르고 효율적입니다.

![IntelliJ IDEA Find Action](https://miro.medium.com/v2/resize:fit:1400/1*Jc8x-u4J23IzS8MRHKZd2A.gif)

<div class="content-ad"></div>

위 GIF에서 다음을 수행합니다:

- Command + K: 동작 찾기
- "branches"를 입력한 후 Enter 키를 누릅니다.
- 주 브랜치로 이동한 후 Enter 키를 누릅니다.
- 오른쪽 확장 팝업에서 "New branch from main"을 선택하고 Enter 키를 누릅니다.
- 브랜치 이름을 입력한 후 Enter 키를 누릅니다.

자주 사용하는 다른 작업은 현재 변경 사항을 보관하는 것입니다. 이는 Git stash와 유사하지만 IntelliJ의 내부 UI를 사용합니다. Unshelving할 때 변경 사항(차이점)을 보여주는 등 Git stash 사용보다 몇 가지 이점이 있습니다.

![위 이미지 링크](https://miro.medium.com/v2/resize:fit:1400/1*xYk9_S3uY2U9jF80ChSffg.gif)

<div class="content-ad"></div>

위 GIF에서:

- 파일을 수정합니다
- Command + K를 눌러 "Shelve changes"라고 입력을 시작합니다
- 이제 파일은 초기 상태로 돌아갑니다
- 다시 제 변경 사항을 되살립니다.

## 새 파일 생성

항상 자주 사용하는 단축키 조합을 사용하면, 현재 편집기에서 보이는 현재 파일과 동일한 디렉토리에 새 파일을 만들 수 있습니다.

<div class="content-ad"></div>

![GIF](https://miro.medium.com/v2/resize:fit:1400/1*_UoXjHJrO1l9L6xM0YAKhg.gif)

위의 GIF에서 확인할 수 있는 내용은 다음과 같습니다:

- Option + F1: 왼쪽의 Project View에서 현재 파일 열기.
- Control + Enter: 생성할 수 있는 엔티티 목록을 보여주는 팝업이 열립니다. "newfile.tsx"라는 이름의 평범한 파일을 생성하려면 다시 Enter 키를 누릅니다. 그리고 파일이 Git으로 추적되어야 하는지 묻는 메시지가 나타나면 다시 Enter 키를 누릅니다.

## Command + Option + M: Extract Method

<div class="content-ad"></div>

![GIF](https://miro.medium.com/v2/resize:fit:1400/1*-wNWZwXm4VPfUxrK7sb90A.gif)

위의 GIF에서는 두 줄의 코드를 강조표시하고 이를 함수로 추출하는 것을 보여줍니다. 함수 시그니처는 올바른 타입으로 자동으로 생성됩니다.

## Command + T: 터미널 전환

![GIF](https://miro.medium.com/v2/resize:fit:1400/1*2loMJk_0EAKfukvvqIeMfg.gif)

<div class="content-ad"></div>

대신에 Command + Tab과 같은 다른 애플리케이션으로 전환하는 대신, 저는 제 IDE 내에서 모든 작업을 하는 것을 좋아해요. 그래서 저는 Command + T로 터미널을 전환합니다.

## 키 반복 증가

이것은 기술적으로 바로 가기로 분류되지는 않지만, 키 반복을 증가시킴으로써 소스 코드를 더 빠르게 탐색할 수 있습니다.

![image](https://miro.medium.com/v2/resize:fit:1400/1*ugouz46B-gJ1MQBLhaQAAQ.gif)

<div class="content-ad"></div>

위 GIF에서 파일을 위/아래로/왼쪽/오른쪽으로 탐색해보세요.

Hadi Hariri의 비디오를 강력 추천합니다. IntelliJ IDEA에 대한 다양한 팁과 요령을 소개하고 있습니다. 그의 통찰은 효율성을 향상시키는 데 매우 가치 있습니다.

가장 많이 사용하는 단축키는 무엇인가요? 궁금하네요.