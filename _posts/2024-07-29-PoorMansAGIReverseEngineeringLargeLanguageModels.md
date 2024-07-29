---
title: "가난한 사람의 AGI 대형 언어 모델 역설계하는 방법"
description: ""
coverImage: "/assets/img/2024-07-29-PoorMansAGIReverseEngineeringLargeLanguageModels_0.png"
date: 2024-07-29 13:46
ogImage: 
  url: /assets/img/2024-07-29-PoorMansAGIReverseEngineeringLargeLanguageModels_0.png
tag: Tech
originalTitle: "Poor Mans AGI Reverse Engineering Large Language Models"
link: "https://medium.com/@pkr-peasy/reverse-engineering-large-language-models-a-systematic-approach-fc2bf6d4a7e8"
---


![PoorMansAGIReverseEngineeringLargeLanguageModels](/assets/img/2024-07-29-PoorMansAGIReverseEngineeringLargeLanguageModels_0.png)

대형 언어 모델(LLM)의 매혹적인 세계에서는, 그 작동 메커니즘을 이해하는 것이 어려워 보일 수 있습니다. 그러나 연구하는 과정을 통해, 이러한 모델이 어떻게 작동하는지 통찰을 얻을 수 있습니다. 이 블로그 글은 각 구성 요소를 미니 시스템으로 취급하는 체계적인 방법으로 LLM을 리버스 엔지니어링하는 방법을 살펴볼 것입니다. 자유 텍스트로 할 수 있는 일은 거의 무한합니다.

# 체계적 접근법: 입력, 출력 및 프로세스

어떤 시스템의 핵심에는 그 입력, 출력 및 프로세스가 있습니다. 이 렌즈를 통해 LLM을 바라보면 다음과 같은 것을 볼 수 있습니다:

<div class="content-ad"></div>

- Inputs: 시스템에 공급되는 초기 데이터.
- Outputs: 시스템에서 생성되는 최종 결과물.
- Processes: 입력을 출력으로 변환하는 단계 또는 작업.

시스템 내의 각 프로세스는 그 자체 내부에 입력, 출력 및 프롬프트를 포함한 미니 시스템입니다. 이 중첩 구조를 통해 LLMs의 복잡성을 더 관리하기 쉬운 단위로 분해할 수 있습니다.