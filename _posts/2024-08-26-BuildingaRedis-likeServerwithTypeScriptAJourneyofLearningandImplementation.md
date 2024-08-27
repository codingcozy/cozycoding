---
title: "TypeScript로 Redis와 유사한 서버 구축 만드는 과정 정리"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-26 18:14
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Building a Redis-like Server with TypeScript A Journey of Learning and Implementation"
link: "https://medium.com/@vivekchiragshah2004/building-a-redis-like-server-with-typescript-a-journey-of-learning-and-implementation-457fb289c288"
isUpdated: true
updatedAt: 1724743691382
---


인하우스 TypeScript로 구축된 Redis.

# 소개

제로부터 Redis와 유사한 서버를 구축하는 여정에 돌입하는 것은 도전적이고 보람있는 경험이었습니다. 이 프로젝트를 통해 서버 개발의 복잡성에 대해 심층적으로 탐구하고, Redis 프로토콜에 대해 배우며 TypeScript를 활용한 경험을 쌓을 수 있었습니다. 이 게시물에서는 제가 구현한 주요 기능, 직면한 어려움, 그리고 그 과정에서 얻은 소중한 교훈을 소개하겠습니다.

# 배운 점

<div class="content-ad"></div>

- Redis 프로토콜 이해:
이 프로젝트에서는 Redis 프로토콜을 완전히 이해해야 했습니다. 특히 명령이 어떻게 구성되고 처리되는지에 대해 깊게 이해해야 했습니다. RESP(REDIS 직렬화 프로토콜)를 학습하여 Redis 클라이언트와 서버 간의 통신을 기반으로 하는 방법을 알게 되었습니다.
- TypeScript 마스터리:
TypeScript로 이 서버를 구축하는 것은 강력한 타이핑을 활용할 수 있게 해줬습니다. 이로 인해 코드베이스가 더 견고하고 오류가 발생할 가능성이 낮아졌습니다. TypeScript의 인터페이스, 타입 유니온, async/await를 포함한 기능에 대해 더 깊이 이해하게 되었습니다.
- 서버-클라이언트 아키텍처:
여러 클라이언트를 처리하고 다양한 명령에 응답할 수 있는 서버를 개발하는 것은 서버-클라이언트 아키텍처를 이해하는 뛰어난 연습이었습니다. 상태를 관리하고 트랜잭션을 처리하며 데이터 일관성을 보장하는 방법을 배웠습니다.
- 복제 및 지속성:
복제 및 지속성 메커니즘을 구현하는 것은 데이터 내구성과 분산 시스템 간 일관성의 중요성에 대해 배울 수 있었습니다.
- 문제 해결 및 디버깅:
복잡한 프로젝트와 마찬가지로 디버깅은 프로세스의 중요한 부분이었습니다. 각 버그는 학습 기회로 나타났으며, 접근 방식을 개선하고 서버의 전반적인 안정성을 향상시킬 수 있게 했습니다.

# 프로젝트 개요

## 기본 키-값 작업

SET 및 GET 명령:
기본 키-값 작업 구현은 첫 번째 단계였습니다. SET 명령은 키-값 쌍을 저장하고, GET은 주어진 키에 대한 값을 검색합니다. 또한 지정된 기간 이후에 키를 자동으로 삭제하는 TTL을 설정할 수 있는 PX 옵션을 지원했습니다.

<div class="content-ad"></div>

## 트랜잭션

MULTI, EXEC 및 DISCARD 명령:
트랜잭션 지원 기능을 구축하는 것은 어려웠지만 보람 있었습니다. MULTI를 구현함으로써 여러 명령을 대기열에 넣고 EXEC를 사용하여 원자적으로 실행할 수 있도록 허용했습니다. DISCARD 명령은 트랜잭션을 취소하고 명령 대기열을 지우는 기능을 합니다.

## 복제

REPLCONF 및 PSYNC 명령:
복제는 Redis의 핵심 기능이며 이를 구현하려면 마스터와 레플리카 간의 상태 동기화 방법을 이해해야 했습니다. REPLCONF와 PSYNC 명령은 마스터와 슬레이브 서버 간의 초기 핸드셰이크 중에 자동으로 처리됩니다.

<div class="content-ad"></div>

## 스트림 작업

XADD 및 XRANGE 명령어:
스트림을 구현함으로써 XADD를 사용하여 스트림에 항목을 추가하고 XRANGE를 사용하여 특정 범위 내에서 검색할 수 있었습니다. 이 기능은 리스트와 정렬된 집합의 측면을 결합한 것이라 특히 흥미로웠습니다.

## 리스트 작업 (새로 추가됨!)

LPUSH, RPUSH, LPOP, RPOP, LLEN, LRANGE, LTRIM, LMOVE, LINDEX, LSET, LREM, LINSERT, 그리고 LGETALL 명령어:
리스트 작업을 추가함으로써 서버를 확장하는 것은 중요한 추가였습니다. 이러한 명령어를 사용하면 리스트의 생성과 조작이 가능해지며 스택 및 큐 구현, 범위 추출, 그리고 보다 복잡한 데이터 관리 작업을 지원할 수 있습니다.

<div class="content-ad"></div>

## Pub/Sub (새로운 기능!)

구독, 구독 취소, 그리고 발행(PUBLISH) 명령어:
Pub/Sub 기능을 추가하면 서버가 실시간 메시징에 강력한 도구로 변합니다. 클라이언트는 이제 채널을 구독하여 메시지가 발행될 때 즉시 업데이트를 받아 이벤트 기반 아키텍처와 실시간 알림이 가능해집니다.

## 추가 명령어

PING과 ECHO:
이러한 기본 명령어들은 구현하기 쉬웠습니다. PING은 서버의 가용성을 확인하고, ECHO는 클라이언트가 보낸 메시지를 반환합니다. 어떤 서버-클라이언트 상호작용에도 필수적인 명령어들입니다.

<div class="content-ad"></div>

## 서버 정보 및 지속성

INFO Command:
INFO 명령어는 서버 역할 및 복제 상태에 대한 세부 정보를 제공합니다. 개발 중에 서버를 모니터링하고 디버깅하는 데 중요한 역할을 했습니다.

RDB 파일 처리:
지속성을 구현하는 데는 서버 시작 시 RDB 파일에서 데이터를로드하는 작업이 포함되었습니다. 이 기능은 데이터가 서버 다시 시작시 살아남도록 보장하며, 서버에 내구성 레이어를 추가하는데 도움이 되었습니다.

Wait Command:
WAIT 명령어는 이전에 수행한 모든 쓰기 명령어가 특정 수의 복제본에 의해 확인될 때까지 블록합니다. 이 명령어는 복제본 간 데이터 일관성을 보장하는 데 필수적이었습니다.

<div class="content-ad"></div>

# 구현 방식

- 작은 규모부터 시작:
가장 기본적인 기능인 SET 및 GET을 구현하는 것으로 시작했습니다. 이는 견고한 기반을 제공하고 서버 구조를 이해하는 데 도움이 되었습니다.
- 점진적인 구축:
한 번에 한 가지씩 기능을 추가했습니다. 트랜잭션부터 시작하여 복제, 스트림, 리스트, Pub/Sub, 그리고 마지막으로 지속성까지 진행했습니다. 이러한 점진적인 방식을 통해 각 기능을 철저히 테스트한 후 다음 단계로 진행할 수 있었습니다.
- 테스트 및 디버깅:
각 기능을 구현한 후 redis-cli를 사용하여 철저하게 테스트했습니다. 이 실전 테스트는 버그를 식별하고 수정하는 데 중요했습니다.
- 코드베이스 개선:
프로젝트가 커짐에 따라 코드베이스를 리팩터링하여 가독성과 유지보수성을 향상시켰습니다. 큰 함수를 작고 관리하기 쉬운 조각으로 나누고 일관된 명명 규칙과 문서화를 보장하는 작업이 포함되었습니다.

# 명령 개요

- SET key value: 키-값 쌍을 저장합니다.
- GET key: 키에 대한 값 가져오기.
- MULTI: 트랜잭션 블록 시작.
- EXEC: 트랜잭션에 대기 중인 모든 명령 실행.
- DISCARD: 트랜잭션 취소.
- XADD stream_name * key value: 스트림에 항목 추가.
- XRANGE stream_name start end: 범위 내의 스트림에서 항목 검색.
- LPUSH key value [value…]: 리스트 시작 부분에 하나 이상의 값 추가.
- RPUSH key value [value…]: 리스트 끝 부분에 하나 이상의 값 추가.
- LPOP key: 리스트의 첫 번째 요소 삭제 및 반환.
- RPOP key: 리스트의 마지막 요소 삭제 및 반환.
- LLEN key: 리스트의 길이 반환.
- LRANGE key start stop: 리스트에서 요소 범위 반환.
- LTRIM key start stop: 명시된 범위로 리스트 자르기.
- LMOVE source destination LEFT|RIGHT LEFT|RIGHT: 요소를 다른 리스트로 원자적으로 이동.
- LINDEX key index: 리스트에서 지정된 인덱스의 요소 반환.
- LSET key index value: 리스트에서 지정된 인덱스에 요소의 값을 설정.
- LREM key count value: 지정된 값과 일치하는 요소를 리스트에서 제거.
- LINSERT key BEFORE|AFTER pivot value: 리스트에서 다른 요소 앞 또는 뒤에 요소 삽입.
- LGETALL key: 리스트의 모든 요소 반환.
- SUBSCRIBE channel: 메시지를 받기 위해 채널 구독.
- UNSUBSCRIBE channel: 채널 구독 취소.
- PUBLISH channel message: 채널에 메시지 게시.
- PING: 서버 가용성 확인.
- ECHO message: 전송된 동일한 메시지 반환.
- INFO: 서버 정보 제공.
- WAIT numslaves timeout: 이전의 쓰기 명령이 일정 수의 레플리카에서 인식될 때까지 차단.

<div class="content-ad"></div>

# 다음에는 무엇을 할까요?

제가 기능적인 Redis와 비슷한 서버를 만들었으니, 계속해서 완성도를 높이고 확장할 계획입니다. 추가적인 Redis 명령 지원, 성능 향상, 클러스터링과 같은 고급 기능 구현이 가능한 다음 단계가 될 것입니다.

# 저와 소통하기

- LinkedIn: Vivek Shah
- GitHub: Vivek-C-Shah
- Twitter: @ShVivek25
- 프로젝트 저장소: Redis-CLI-TypeScript

<div class="content-ad"></div>

# 결론

이 Redis와 유사한 서버를 구축하는 것은 도전적이지만 매우 교육적인 경험이었습니다. 서버-클라이언트 아키텍처, Redis 프로토콜 및 TypeScript의 강력한 기능에 대한 깊은 이해를 제공했습니다. 계속해서 학습하고 개발하는 것에 흥미를 느끼며, 이 프로젝트가 유사한 노력에 관심이 있는 다른 사람들에게 영감이 되거나 학습 자료로 사용될 수 있기를 희망합니다.

읽어 주셔서 감사합니다. LinkedIn에서 연락을 취하거나 GitHub에서 다른 프로젝트를 확인하거나 Twitter에서 업데이트를 더 많이 받아보세요!