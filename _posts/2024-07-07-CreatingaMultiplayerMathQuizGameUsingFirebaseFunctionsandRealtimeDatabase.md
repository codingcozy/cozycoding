---
title: "Firebase Functions와 Realtime Database를 이용한 멀티플레이어 수학 퀴즈 게임 만드는 방법"
description: ""
coverImage: "/uidev-css.github.io/assets/no-image.jpg"
date: 2024-07-07 02:46
ogImage: 
  url: /uidev-css.github.io/assets/no-image.jpg
tag: Tech
originalTitle: "Creating a Multiplayer Math Quiz Game Using Firebase Functions and Realtime Database"
link: "https://medium.com/@nikunjjoshi5022/creating-a-multiplayer-math-quiz-game-using-firebase-functions-and-realtime-database-2e25a03d3f4f"
---


인터랙티브 멀티플레이어 게임을 개발하려면 실시간 동기화와 서버리스 함수를 사용하여 게임 로직을 관리해야 합니다. Firebase는 실시간 데이터베이스와 Firebase Functions을 통해 이러한 애플리케이션에 이상적인 플랫폼을 제공합니다. 이 기사에서는 멀티플레이어 수학 퀴즈 게임을 디자인할 것입니다. 주요 기능, 데이터베이스 스키마, Firebase 실시간 데이터베이스, Firebase Functions을 자세히 살펴볼 것입니다.

프로젝트의 전체 소스 코드는 여기에서 확인할 수 있습니다.

## Firebase 실시간 데이터베이스

Firebase 실시간 데이터베이스는 클라우드 기반 NoSQL 데이터베이스로, 모든 클라이언트 간 데이터를 실시간으로 동기화합니다. 주요 기능은 다음과 같습니다:

<div class="content-ad"></div>

- 실시간 동기화: 데이터 변경 사항이 모든 연결된 클라이언트에 즉시 반영됩니다.
- 오프라인 지원: 로컬 데이터 지속성을 통해 오프라인 상태에서도 원활한 사용자 경험을 제공합니다.
- 보안 규칙: 데이터 액세스 및 유효성 검사에 대한 세밀한 제어.

## Firebase Functions

Firebase Functions은 이벤트에 응답하여 실행되는 서버리스 함수입니다. Firebase 기능을 확장하고 외부 서비스와 통합할 수 있습니다. 주요 장점은 다음과 같습니다:

- 확장성: 다양한 워크로드를 처리할 수 있도록 자동으로 확장됩니다.
- 트리거: 함수는 데이터베이스 업데이트 또는 인증 변경과 같은 다양한 Firebase 이벤트에 의해 트리거될 수 있습니다.
- 통합: 다른 Firebase 제품 및 서드파티 API와 쉽게 통합할 수 있습니다.

<div class="content-ad"></div>

## 수학 퀴즈 게임의 특징

- 실시간 매칭: 매칭 대기 중인 플레이어를 자동으로 짝지어 줍니다.
- 다이내믹 수학 문제: 각 게임 세션마다 고유한 수학 방정식을 생성합니다.
- 게임 관리: 게임 세션을 생성하고 관리하며, 플레이어의 상태와 점수를 업데이트합니다.
- 즉시 업데이트: 게임 상태와 플레이어 상호작용에 대한 실시간 업데이트를 보장합니다.

## 데이터베이스 스키마 설계

저희 게임은 Firebase 실시간 데이터베이스에서 다음 스키마를 사용할 것입니다.

<div class="content-ad"></div>

- /matchMaking: 매칭을 기다리는 플레이어 정보를 저장합니다.
- /inGame: 활성화된 게임 세션을 관리하며, 플레이어 데이터와 수학 문제를 포함합니다.
- /lock: 매칭 처리를 단 한 번에 한 인스턴스만 처리되도록 잠금 메커니즘을 구현합니다.

## 스키마 세부 정보:

```js
{
  "matchMaking": {
    "player1Id": {
      "info": {
        "userName": "플레이어1",
        "photoUrl": "url1"
      },
      "gameId": null
    },
    "player2Id": {
      "info": {
        "userName": "플레이어2",
        "photoUrl": "url2"
      },
      "gameId": null
    }
  },
  "inGame": {
    "gameId": {
      "players": {
        "player1Id": {
          "userName": "플레이어1",
          "photoUrl": "url1",
          "score": 0
        },
        "player2Id": {
          "userName": "플레이어2",
          "photoUrl": "url2",
          "score": 0
        }
      },
      "questionOptions": [
        {
          "numbers": [2, 3],
          "operator": "+",
          "correctAns": 5,
          "options": [5, 6, 7, 8]
        }
      ]
    }
  },
  "lock": false
}
```

## 함수 구현하기

<div class="content-ad"></div>

Firebase를 초기화 중: Firebase 서비스를 사용하려면 먼저 Firebase 앱을 초기화하고 Realtime Database에 대한 참조를 가져와야 합니다:

```js
import { initializeApp } from 'firebase-admin/app';
import { getDatabase } from 'firebase-admin/database';
import { onValueUpdated } from 'firebase-functions/v2/database';
import { logger } from 'firebase-functions';

initializeApp();
const db = getDatabase();
```

## 매칭 메이킹 처리하기

핵심 함수 startGame은 매칭 큐에서 플레이어를 쌍으로 매치하여 처리합니다. 여기에 개괄적인 내용이 있습니다:

<div class="content-ad"></div>

```js
export const startGame = onValueUpdated('/matchMaking', async (event) => {
  const matchMakingRef = db.ref('/matchMaking');
  const lockRef = db.ref('/lock');

  try {
    const lockResult = await lockRef.transaction(lock => {
      if (!lock) {
        return true;
      }
      return;
    });

    if (!lockResult.committed) {
      logger.info('Lock already acquired by another instance');
      return;
    }

    const snapshot = await matchMakingRef.once('value');
    const matchMaking = snapshot.val();
    const matchMakingKeys = Object.keys(matchMaking || {});

    if (matchMakingKeys.length >= 2) {
      let player1, player2;

      for (let i = 0; i < matchMakingKeys.length; i++) {
        const player = matchMaking[matchMakingKeys[i]];
        if (!player1 && !player.gameId) {
          player1 = player;
          player1.id = matchMakingKeys[i];
        } else if (!player2 && !player.gameId) {
          player2 = player;
          player2.id = matchMakingKeys[i];
        }
        if (player1 && player2) {
          break;
        }
      }

      if (player1 && player2) {
        const firstPlayerId = player1.id;
        const secondPlayerId = player2.id;
        const firstPlayerInfo = {
          userName: player1.info.userName,
          photoUrl: player1.info.photoUrl,
          score: 0,
        };
        const secondPlayerInfo = {
          userName: player2.info.userName,
          photoUrl: player2.info.photoUrl,
          score: 0,
        };

        const playerKeyValue = {
          [firstPlayerId]: firstPlayerInfo,
          [secondPlayerId]: secondPlayerInfo
        };

        const gameId = `${firstPlayerId}_${secondPlayerId}`;
        const inGameRef = db.ref(`/inGame/${gameId}`);

        const equations = generateEquations();

        await inGameRef.set({
          players: playerKeyValue,
          questionOptions: equations
        });

        const updates = {
          [`/matchMaking/${firstPlayerId}`]: { gameId, opponentUuid: secondPlayerId },
          [`/matchMaking/${secondPlayerId}`]: { gameId, opponentUuid: firstPlayerId }
        };

        await db.ref().update(updates);
      }
    }
  } catch (error) {
    logger.error('Error processing matchmaking:', error);
  } finally {
    await lockRef.set(false);
  }
});
```

주요 단계:

- Lock Handling: /lock에서 race condition을 방지하기 위해 트랜잭션을 사용합니다.
- Matchmaking: 매칭 대기열에서 두 플레이어를 선택하고 짝을 지어줍니다.
- 게임 설정: 새로운 게임 세션을 초기화하고 플레이어 상태를 업데이트합니다.

## 수학 문제 생성하기

<div class="content-ad"></div>

게임을 위해 동적 수학 문제를 생성하기 위해 generateEquations를 사용합니다. 각 문제는 두 숫자, 연산자 및 객관식 옵션으로 구성됩니다:

```js
function generateEquations() {
  const equations = [];
  for (let i = 1; i <= 30; i++) {
    const num1 = Math.floor(Math.random() * (i > 10 ? 100 : 10));
    const num2 = Math.floor(Math.random() * (i > 10 ? 100 : 10));
    const operators = ["+", "-", "*"];
    const operator = operators[Math.floor(Math.random() * operators.length)];
    const result = eval(`${num1} ${operator} ${num2}`);
    const options = generateOptions(result);

    equations.push({ numbers: [num1, num2], options, operator, correctAns: result });
  }
  return equations;
}
```

## 객관식 옵션 생성

게임을 더 도전적으로 만들기 위해 generateOptions를 사용하여 올바른 답변과 임의의 부정확한 옵션을 섞은 가능한 답안 목록을 생성합니다.

<div class="content-ad"></div>

```js
function generateOptions(correctAnswer) {
  const options = [correctAnswer];
  while (options.length < 4) {
    const wrongOption = correctAnswer + (Math.floor(Math.random() * 5) - Math.floor(Math.random() * 5));
    if (!options.includes(wrongOption)) options.push(wrongOption);
  }
  return options.sort(() => Math.random() - 0.5);
}
```

## 함수 배포하기

함수를 배포하려면 Firebase CLI를 사용하세요:

```js
firebase deploy --only functions
```

<div class="content-ad"></div>

## 결론

이 튜토리얼에서는 Firebase Functions와 Firebase 실시간 데이터베이스를 활용하여 멀티플레이어 수학 퀴즈 게임을 만들었습니다. 다음을 다루었습니다:

- 기능: 게임의 주요 기능을 설명했습니다.
- 데이터베이스 스키마: 실시간 데이터베이스에서 사용되는 구조와 노드를 상세히 설명했습니다.
- Firebase 실시간 데이터베이스: Firebase의 실시간 데이터 동기화를 사용하는 이점을 강조했습니다.
- Firebase Functions: 서버리스 함수가 게임 로직과 매칭을 처리하는 방법을 논의했습니다.
- 주요 코드 컴포넌트: 코드베이스의 중요한 부분을 살펴보았습니다.

이 프레임워크는 더 복잡한 방정식, 인증 및 게임 이력 추적과 같은 추가 기능으로 확장할 수 있습니다. 전체 소스 코드는 [이 링크](링크)에서 확인할 수 있습니다.

<div class="content-ad"></div>

## 다음 단계

- UI 개발: React 또는 Flutter와 같은 프레임워크를 사용하여이 백엔드와 상호 작용하는 프론트 엔드를 구축합니다.
- 고급 기능: 사용자 인증 및 개인화 된 게임 이력 지원 추가.
- 확장성: 대규모 플레이어 및 세션을 처리하기 위한 확장 전략 탐색.

코드를 실험하고 필요에 맞게 수정하는 것에 자유롭게 도전하세요. 즐거운 코딩 되세요!