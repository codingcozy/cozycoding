---
title: "강력하면서도 간단한 캐싱 알고리즘 Active-Victim-Cache 소개"
description: ""
coverImage: "/blocktong.github.io/assets/no-image.jpg"
date: 2024-07-10 03:51
ogImage: 
  url: /blocktong.github.io/assets/no-image.jpg
tag: Tech
originalTitle: "A Simple But Powerful Caching Algorithm: Presenting the Active-Victim-Cache"
link: "https://medium.com/@drpicox/a-simple-but-powerful-caching-algorithm-eb55c04c5d8f"
---


캐시는 프로그램 실행을 드라마틱하게 가속화할 수 있지만, 메모리가 가득 찼을 때 어떤 데이터를 제거할지 결정해야 하는 중요한 과제에 직면합니다. 이 글에서는 구현을 간단하게 만들고 성능을 향상시키는 강력한 캐싱 알고리즘을 공개합니다. 복잡한 해결책으로 인한 머리 아픔 없이요!

프로그래밍에서 캐시는 프로그램 실행 속도를 높이는 데 사용되며, 주로 두 가지 주된 목적을 위해 활용됩니다:

- 다시로드하는 것을 피하기 위해 로드된 데이터를 저장하고,
- 반복을 피하기 위해 복잡한 계산 결과를 저장합니다.

그러나 한 가지 단점이 있습니다: 메모리는 한정되어 있고, 모든 것을 캐시에 넣을 수는 없습니다. 결국 캐시는 새로운 항목을 위해 공간을 확보하기 위해 데이터를 제거해야 합니다. 이것이 가장 중요한 질문 중 하나로 이어집니다: 캐시가 가득 찼을 때 무엇을 제거해야 할까요?

<div class="content-ad"></div>

이 질문에 대한 답변으로 다양한 알고리즘이 개발되었습니다. 그중 몇 가지 대표적인 것은 First-In-First-Out(FIFO), Least Frequently Used (LFU) 등이 있습니다. 다양한…