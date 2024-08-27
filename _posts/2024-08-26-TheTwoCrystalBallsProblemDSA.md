---
title: "DSA 두 개의 크리스털 공 문제"
description: ""
coverImage: "/assets/img/2024-08-26-TheTwoCrystalBallsProblemDSA_0.png"
date: 2024-08-26 18:13
ogImage: 
  url: /assets/img/2024-08-26-TheTwoCrystalBallsProblemDSA_0.png
tag: Tech
originalTitle: "The Two Crystal Balls Problem  DSA"
link: "https://medium.com/@leachtucker/the-two-crystal-balls-problem-dsa-80028a374073"
isUpdated: true
updatedAt: 1724742563830
---


아래는 Markdown 형식으로 변경해주세요.

![이미지](/assets/img/2024-08-26-TheTwoCrystalBallsProblemDSA_0.png)

# 문제

면접이나 DSA를 공부하면서 이 문제를 만난 적이 있을 수 있습니다. 이 문제와 다양한 해결책에 대해 이야기해봐요!

<div class="content-ad"></div>

위 문제를 컴퓨터 프로그램으로 모델링하는 방법에 대해 고민해 봅시다. 입력 목록이 있는 함수를 가정해보면, 여기서 인덱스는 높이를 나타내고 해당 인덱스의 요소 값은 그 거리에서 공이 깨지는지를 나타냅니다.

우리는 또한 문제의 제약 사항을 알고 있어야 합니다:

- 우리에게는 공이 두 개만 있습니다
- 한 번 공이 깨지면 다시 깰 수 없습니다

이러한 제약 사항을 우리 프로그램 안에 모델링하기 위해 다음 규칙으로 번역할 수 있습니다:

<div class="content-ad"></div>

- 최대한으로 참으로 평가되는 두 인덱스만 확인할 수 있습니다.

# 해결 방안

첫눈에 보면, 이 문제를 선형 검색으로 해결하는 것이 가장 간단해 보일 수 있습니다. 리스트의 시작부터 시작하여 한 번에 한 원소씩 이동합니다. 이동하면서 각 값에 대해 살펴보게 됩니다. 참으로 평가되는 값을 찾으면 해당 인덱스를 반환합니다.

이 해결책의 장점 중 하나는 두 개의 볼 중 하나만 사용해도 된다는 것입니다. 하지만, 최악의 경우에는 전체 입력 리스트의 모든 요소를 확인해야 하는 문제가 있습니다. 이는 가장 최적화된 방법은 아닙니다. 여기서 최악의 경우 실행 시간 복잡도는 O(n)입니다.

<div class="content-ad"></div>

```js
function findFirstBreak(levels: boolean[]): number {
    for (let i = 0; i < levels.length; i++) {
        const isBroken = levels[i];
        if (isBroken) {
            return i;
        }
    }

    return -1;
}
```

더 깊이 파보죠...

조금 더 생각해보면, 바이너리 서치와 같은 방법을 사용하면 더 최적화된 접근 방식을 찾을 수 있을 것 같습니다. 그러나 문제의 제약 사항에 잘 어울리지 않아서 바이너리 서치를 활용할 수는 없습니다. 최대로 할 수 있는 일은 true로 평가되는 두 개의 인덱스만 확인할 수 있다는 것입니다. 바이너리 서치는 처음 두 개의 확인이 true로 이어질 것을 보장하지 않습니다.

만약 우리가 list를 띄어넘어서 확인한다면 어떨까요? 어떤 아이디어가 떠오르고 있습니다. list 일부를 뛰어넘어서 해당 인덱스의 값을 확인한다고 가정해 봅시다. 만약 뛴 인덱스의 값이 true라면, 우리는 첫 번째 true 값이 마지막 확인과 새로운 뛴 인덱스 사이 어딘가에 있을 것이라는 것을 알 수 있습니다. 이는 우리가 마지막 확인과 가장 최근 점프 사이의 범위를 선형적으로 검색하여 첫 번째 true 값을 찾을 수 있다는 것을 의미합니다. 뛴 인덱스의 값이 false이면, 이는 우리가 다시 뛰어넘어야 한다는 것을 의미합니다.

<div class="content-ad"></div>

우리가 점프할 거리는 목록의 10%라고 가정해 봅시다. 즉, .1n 요소를 선형으로 검색해야 합니다. 이 경우의 시간 복잡도는 무엇인가요? O(n)입니다. 상수는 버리기 때문이죠. 음, 런타임 복잡도가 우리의 가장 간단한 해결책과 같다니 안타깝네요.

만약 우리가 입력값에 비례하지 않는 양으로 점프를 설정한다면 어떨까요? 아하! 이제 알았습니다. sqrt(n) 간격으로 목록을 확인하면, 우리는 목록의 sqrt(n)만 선형으로 검색하면 됩니다. 이는 우리의 해결책이 O(sqrt(n))의 런타임 복잡도를 달성할 수 있다는 것을 의미합니다.

우리는 이를 재귀 및 반복적 방식으로 모두 구현할 수 있습니다. 하지만 여기서는 재귀적 구현을 제공하겠습니다.

```js
function findFirstBreak(levels: boolean[]): number {
    const jump = Math.floor(Math.sqrt(levels.length));

    function helper(searchStart: number, interval: number): number {
        const currIdx = searchStart + interval;

        if (currIdx >= levels.length) {
            return -1;
        }

        const didBreak = levels[currIdx];
        if (didBreak && interval === 1) {
            return currIdx;
        } else if (didBreak && interval !== 1) {
            return helper(searchStart, 1);
        }

        return helper(currIdx, interval);
    }

    return helper(0, jump);
}
```