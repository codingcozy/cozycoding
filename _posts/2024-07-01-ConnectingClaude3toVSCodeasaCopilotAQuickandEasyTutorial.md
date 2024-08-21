---
title: "Claude 3을 VSCode에 코파일럿으로 연결하는 빠르고 쉬운 튜토리얼"
description: ""
coverImage: "/assets/img/2024-07-01-ConnectingClaude3toVSCodeasaCopilotAQuickandEasyTutorial_0.png"
date: 2024-07-01 21:35
ogImage: 
  url: /assets/img/2024-07-01-ConnectingClaude3toVSCodeasaCopilotAQuickandEasyTutorial_0.png
tag: Tech
originalTitle: "Connecting Claude 3 to VSCode as a Copilot: A Quick and Easy Tutorial"
link: "https://medium.com/towards-aws/connecting-claude-3-to-vscode-as-a-copilot-a-quick-and-easy-tutorial-c011d33f500c"
isUpdated: true
---





![2024-07-01-ConnectingClaude3toVSCodeasaCopilotAQuickandEasyTutorial_0.png](/assets/img/2024-07-01-ConnectingClaude3toVSCodeasaCopilotAQuickandEasyTutorial_0.png)

안녕하세요! 이번 튜토리얼에서는 Anthropic의 새로운 혁신적인 Claude 3 모델을 사용하여 Visual Studio Code를 Copilot에 연결하는 다양한 방법에 대해 설명하겠습니다.

먼저, VSCode에 설치된 CodeGPT 확장 프로그램이 필요합니다. 아래 링크를 통해 다운로드할 수 있습니다:

또한, VSCode Marketplace에서 직접 확장 프로그램을 검색하고 설치할 수도 있습니다.

<div class="content-ad"></div>

![image](https://miro.medium.com/v2/resize:fit:1400/1*UW94rI4_pqYVJikJDoOZWA.gif)

설치 후에는 Claude 3를 CodeGPT를 통해 사용할 수 있는 다음 3가지 옵션 중 하나를 선택할 수 있습니다.

### #1 — Anthropic API Key

다음 링크를 사용하여 Anthropic에 계정을 생성하십시오: [https://console.anthropic.com/](https://console.anthropic.com/)

<div class="content-ad"></div>

![ConnectingClaude3toVSCodeasaCopilotAQuickandEasyTutorial_1](/assets/img/2024-07-01-ConnectingClaude3toVSCodeasaCopilotAQuickandEasyTutorial_1.png)

Then go to the “Get API Keys” menu.

![ConnectingClaude3toVSCodeasaCopilotAQuickandEasyTutorial_2](/assets/img/2024-07-01-ConnectingClaude3toVSCodeasaCopilotAQuickandEasyTutorial_2.png)

Select “Create Key”.

<div class="content-ad"></div>

![image1](/assets/img/2024-07-01-ConnectingClaude3toVSCodeasaCopilotAQuickandEasyTutorial_3.png)

Next, in VSCode, open CodeGPT and select Anthropic as the provider.

![image2](/assets/img/2024-07-01-ConnectingClaude3toVSCodeasaCopilotAQuickandEasyTutorial_4.png)

When selecting the model, the extension will prompt you to add your API Key. Paste it and click on “Connect”.

<div class="content-ad"></div>

![이미지](/assets/img/2024-07-01-ConnectingClaude3toVSCodeasaCopilotAQuickandEasyTutorial_5.png)

다음으로, Anthropic 서비스를 통해 사용 가능한 Claude 3 모델 중 하나를 선택합니다.

![이미지](/assets/img/2024-07-01-ConnectingClaude3toVSCodeasaCopilotAQuickandEasyTutorial_6.png)

서비스 제작자가 제공하는 API를 통해 직접 Claude 3를 즐기세요.

<div class="content-ad"></div>

# #2 — Amazon Bedrock

Amazon Bedrock을 사용하여 Claude 3를 연결하려면 AWS 계정이 필요합니다. 계정을 만들려면 다음 링크를 방문해주세요: [https://aws.amazon.com/](https://aws.amazon.com/)

![Connecting Claude 3 to VS Code as a Copilot: A Quick and Easy Tutorial](/assets/img/2024-07-01-ConnectingClaude3toVSCodeasaCopilotAQuickandEasyTutorial_7.png)

AWS 계정 내 Bedrock 섹션으로 이동한 후 사용 가능한 모델 섹션에서 Claude 3 모델을 활성화하세요. Bedrock를 구성하여 모델을 활성화하는 방법에 대한 단계별 가이드가 있는 이 기사를 확인해보세요:

<div class="content-ad"></div>

모델이 활성화되면 VSCode를 열고 CodeGPT에 액세스하여 제공자로 Bedrock을 선택할 수 있습니다.

![이미지](/assets/img/2024-07-01-ConnectingClaude3toVSCodeasaCopilotAQuickandEasyTutorial_8.png)

AWS 계정의 연결 세부 정보를 추가하세요.

![이미지](/assets/img/2024-07-01-ConnectingClaude3toVSCodeasaCopilotAQuickandEasyTutorial_9.png)

<div class="content-ad"></div>

마지막으로, 모델 선택기에서 Claude 3 모델을 선택하세요.

![Claude 3](/assets/img/2024-07-01-ConnectingClaude3toVSCodeasaCopilotAQuickandEasyTutorial_10.png)

# #3 — CodeGPT Agent

CodeGPT 에이전트를 통해 Claude 3에 액세스하려면 다음 링크에서 무료 계정을 생성해야 합니다: [https://app.codegpt.co](https://app.codegpt.co)

<div class="content-ad"></div>

마켓 플레이스 섹션으로 이동해주세요.

이미지:
![image1](/assets/img/2024-07-01-ConnectingClaude3toVSCodeasaCopilotAQuickandEasyTutorial_11.png)

마켓 플레이스의 모델 카테고리에서 Claude 3을 검색하고, 클릭한 후 "이 에이전트 사용해보기"를 클릭해주세요.

이미지:
![image2](/assets/img/2024-07-01-ConnectingClaude3toVSCodeasaCopilotAQuickandEasyTutorial_12.png)

<div class="content-ad"></div>

![Connecting Claude 3 to VSCode as a Copilot: A Quick and Easy Tutorial](/assets/img/2024-07-01-ConnectingClaude3toVSCodeasaCopilotAQuickandEasyTutorial_13.png)

이제 VSCode에서 CodeGPT를 열고 공급자로 CodeGPT Plus를 선택하세요.

![Connecting Claude 3 to VSCode as a Copilot: A Quick and Easy Tutorial](/assets/img/2024-07-01-ConnectingClaude3toVSCodeasaCopilotAQuickandEasyTutorial_14.png)

모델 섹션에서 Claude 3을 선택하세요.

<div class="content-ad"></div>

마지막 단계입니다! 이제 Visual Studio Code에서 Copilot으로 사용할 강력한 Claude 3 모델을 즐길 수 있습니다.

![이미지](https://miro.medium.com/v2/resize:fit:1400/1*5N5PTEBunEqJStJNrbFJcg.gif)