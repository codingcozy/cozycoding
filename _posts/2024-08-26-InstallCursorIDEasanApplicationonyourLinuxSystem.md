---
title: "리눅스 시스템에 어플리케이션으로 Cursor IDE 설치하기"
description: ""
coverImage: "/assets/img/2024-08-26-InstallCursorIDEasanApplicationonyourLinuxSystem_0.png"
date: 2024-08-26 19:25
ogImage: 
  url: /assets/img/2024-08-26-InstallCursorIDEasanApplicationonyourLinuxSystem_0.png
tag: Tech
originalTitle: "Install Cursor IDE as an Application on your Linux System"
link: "https://medium.com/@zohebabai/install-cursor-ai-as-an-application-on-a-linux-system-b859e7d28f5f"
isUpdated: false
---


만약 저와 같다면, 항상 프로그래밍 경험을 향상시킬 수 있는 새로운 도구를 찾고 계실 것입니다. 최근에 Andrej Karpathy의 트윗에서 Sonnet 3.5과 함께 VS Code Cursor를 사용하는 방법에 대해 알게 되었는데, 이에 호기심이 생겼습니다.

만약 리눅스 기기에서 Cursor Code Editor를 사용해보고 싶다면, 공식 웹사이트에서 AppImage를 다운로드할 수 있습니다.

![이미지](/assets/img/2024-08-26-InstallCursorIDEasanApplicationonyourLinuxSystem_0.png)

하지만 매번 AppImage를 수동으로 실행하는 것이 불편하다면, 올바른 위치에 계십니다. 이 안내서에서는 Cursor IDE를 시스템에 완벽히 통합하여 일반적인 리눅스 응용 프로그램처럼 사용하는 방법을 안내하겠습니다. 이 간단한 단계를 따르면, 다른 데스크톱 응용 프로그램처럼 원활하게 작동하는 Cursor IDE를 사용할 수 있을 것입니다.

<div class="content-ad"></div>

시작해 봅시다!

# 단계별 설치 가이드

## 단계 1: Cursor IDE를 위한 폴더 생성

가장 먼저, Cursor IDE를 위한 전용 폴더를 생성해 봅시다. 터미널을 열고 다음 명령을 실행하세요:

<div class="content-ad"></div>

```js
mkdir -p ~/Applications/cursor
```

이 명령은 홈 폴더 내 "Applications" 디렉토리 안에 새 폴더 "cursor"를 생성합니다.

## 단계 2: 최신 버전의 Cursor IDE 다운로드

이제 다음 명령을 사용하여 Cursor IDE의 최신 버전을 다운로드할 것입니다:

<div class="content-ad"></div>

```bash
wget -O ~/Applications/cursor/cursor.AppImage "https://downloader.cursor.sh/linux/appImage/x64"
```

이 명령어는 Cursor IDE AppImage를 다운로드하여 방금 만든 "cursor" 폴더에 저장합니다.

## 단계 3: AppImage 실행 가능하게 만들기

AppImage가 실행 가능한지 확인하려면 다음 명령을 실행하십시오:

<div class="content-ad"></div>

```js
chmod +x ~/Applications/cursor/cursor.AppImage
```

보통 이 단계는 필요하지 않을 수 있습니다. AppImage는 이미 실행 가능해야 하지만, 두 번 확인하는 것이 항상 좋습니다! 😉

## 단계 4: 쉬운 액세스를 위한 심볼릭 링크 생성

터미널 어디에서나 Cursor IDE를 실행할 수 있도록 심볼릭 링크를 만들어 봅시다:

<div class="content-ad"></div>

```js
sudo ln -s ~/Applications/cursor/cursor.AppImage /usr/local/bin/cursor
```

이제 터미널에서 cursor를 입력하여 애플리케이션을 실행할 수 있습니다. 정말 멋지죠? 😎

## 단계 5: Cursor IDE 아이콘 추가

더 멋지게 만들기 위해 Cursor IDE 아이콘을 추가해 보겠습니다. 이 이미지를 다운로드하세요:


<div class="content-ad"></div>

`<img src="/assets/img/2024-08-26-InstallCursorIDEasanApplicationonyourLinuxSystem_1.png" />`

그리고 ~/Applications/cursor/ 폴더에 cursor-icon.png으로 저장하세요.

## 단계 6: 데스크톱 항목 만들기

이제 Cursor IDE를 응용 프로그램 메뉴에서 쉽게 사용할 수 있도록 데스크톱 항목을 만들어 보겠습니다:

<div class="content-ad"></div>

```js
vim ~/.local/share/applications/cursor.desktop
```

이 명령어를 입력하면 Vim 텍스트 편집기가 열립니다. 아래 코드를 파일에 복사하여 붙여넣기하세요:

```js
[Desktop Entry]
Name=Cursor
Exec=/home/[your_username]/Applications/cursor/cursor.AppImage
Icon=/home/[your_username]/Applications/cursor/cursor-icon.png
Type=Application
Categories=Utility;Development;
```

실제 사용자 이름으로 [your_username]을 교체해주세요. 그리고 :wq를 입력한 후 Enter 키를 눌러 변경 사항을 저장하고 Vim을 종료하세요.

<div class="content-ad"></div>

## 단계 7: 업데이트 스크립트 생성하기

Cursor IDE를 손쉽게 업데이트하려면, 업데이트 스크립트를 생성해보겠습니다:

```js
vim ~/Applications/cursor/update-cursor.sh
```

아래 코드를 스크립트에 복사하여 붙여넣기하세요:

<div class="content-ad"></div>

```bash
#!/bin/bash

APPDIR=~/Applications/cursor
APPIMAGE_URL="https://downloader.cursor.sh/linux/appImage/x64"

wget -O $APPDIR/cursor.AppImage $APPIMAGE_URL
chmod +x $APPDIR/cursor.AppImage
```

이후, :wq를 입력하고 마지막으로 변경 사항을 저장하고 Vim을 종료하려면 Enter 키를 누르세요.

## 단계 8: 업데이트 스크립트를 실행 가능하게 만들기

업데이트 스크립트를 실행 가능하게 하려면 다음 명령을 실행하세요:

<div class="content-ad"></div>


```js
chmod +x ~/Applications/cursor/update-cursor.sh
```

## 단계 9: 부팅시 Cursor IDE 업데이트 서비스 만들기

컴퓨터를 시작할 때마다 Cursor IDE를 자동으로 업데이트하는 서비스를 만들어 봅시다:

```js
vim ~/.config/systemd/user/update-cursor.service
```

<div class="content-ad"></div>

다음의 코드를 서비스 파일에 복사하여 붙여넣기 해주세요:

```js
[Unit]
Description=Update Cursor

[Service]
ExecStart=/home/[your_username]/Applications/cursor/update-cursor.sh
Type=oneshot

[Install]
WantedBy=default.target
```

다시 한 번, [your_username]을 실제 사용자 이름으로 바꿔주세요.

그리고 :wq를 입력하고, 마지막으로 변경 사항을 저장하고 Vim을 종료하려면 Enter 키를 눌러주세요.

<div class="content-ad"></div>

만약 파일을 저장하는 중에 E212: 쓰기용으로 파일을 열 수 없다는 오류가 발생하면 systemd 디렉토리 자체가 존재하지 않는 것입니다. 따라서 먼저 디렉토리를 생성해봅시다.

```js
mkdir -p ~/.config/systemd/user
vim ~/.config/systemd/user/update-cursor.service
```

나머지 단계는 동일합니다.

## 단계 10: 업데이트 서비스를 활성화하고 시작하세요.

<div class="content-ad"></div>

마지막으로, 다음 명령어로 업데이트 서비스를 활성화하고 시작하세요:

```js
systemctl --user enable update-cursor.service
systemctl --user start update-cursor.service
```

만약 이 단계에서 systemctl 오류를 발견하면, 먼저 상태를 확인해보세요.

```js
systemctl --user status
```

<div class="content-ad"></div>

상태를 표시해주세요: running.

그리고 만약 "Failed to connect to bus: Connection refused"와 같은 오류를 발견하면, dbus 서비스를 수동으로 시작해보세요.

```bash
eval $(dbus-launch --sh-syntax)
```

이를 통해 dbus 세션을 시작할 수 있고, 그 후에 systemctl --user 명령을 다시 실행해볼 수 있습니다.

<div class="content-ad"></div>

만약 셸 환경이 올바르게 구성되지 않았다면, 환경 변수를 수동으로 설정해야 할 수도 있습니다.

다음 줄을 .zshrc 파일에 추가해주세요:

```js
export XDG_RUNTIME_DIR="/run/user/$(id -u)"
export DBUS_SESSION_BUS_ADDRESS="unix:path=${XDG_RUNTIME_DIR}/bus"
```

이 줄들을 추가한 후에 .zshrc 파일을 다시 로드하세요:

<div class="content-ad"></div>

```js
source ~/.zshrc
```

시스템을 시작할 때 systemd 사용자 서비스를 시작하도록 구성되어 있는지 확인하세요:

```js
loginctl show-session $(loginctl | grep $(whoami) | awk '{print $1}') -p Type
```

출력 결과가 Type=x11이 아닌 경우, 세션이 systemd에 의해 관리되지 않는 것을 의미하며, 시스템 및 데스크톱 환경에 따라 추가 구성이 필요할 수 있습니다.

<div class="content-ad"></div>

만약 시스템에 dbus-launch가 설치되어 있지 않다면, 먼저 설치해야 합니다.

```js
sudo apt-get update && sudo apt-get upgrade
sudo apt install dbus-x11
```

시스템을 다시 시작하세요.

```js
sudo reboot
```

<div class="content-ad"></div>

지금 상태를 다시 확인해주세요

```js
systemctl --user status
```

상태가 **State: running**으로 표시되어야 합니다.

# 커서를 즐기세요!

<div class="content-ad"></div>

그리고 끝났어요! Linux 기계에 성공적으로 Cursor IDE를 설치했어요. 🎉 이제 AI 보조 코딩의 힘을 누리고 프로그래밍의 미래를 직접 경험할 수 있어요.

Cursor Docs 페이지에서 자세한 정보를 확인해보세요.

즐거운 코딩하고, AI가 함께하기를 바랍니다! 🖖😄.

도움이 되었다면 👏로 박수를 보내주세요!!!