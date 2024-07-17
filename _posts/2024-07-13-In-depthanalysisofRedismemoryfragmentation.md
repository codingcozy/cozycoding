---
title: "Redis 메모리 단편화 심층 분석"
description: ""
coverImage: "/assets/img/2024-07-13-In-depthanalysisofRedismemoryfragmentation_0.png"
date: 2024-07-13 21:46
ogImage: 
  url: /assets/img/2024-07-13-In-depthanalysisofRedismemoryfragmentation_0.png
tag: Tech
originalTitle: "In-depth analysis of Redis memory fragmentation"
link: "https://medium.com/gitconnected/in-depth-analysis-of-redis-memory-fragmentation-25eeca1a85a2"
---


Redis 메모리 단편화

![이미지](/assets/img/2024-07-13-In-depthanalysisofRedismemoryfragmentation_0.png)

먼저 한 가지 질문을 살펴봅시다. Redis 인스턴스가 5GB의 데이터를 저장했고, 이제 2GB의 데이터가 삭제되었다고 가정해 봅시다. Redis 프로세스가 차지하는 메모리가 감소할까요?

답은: Redis 데이터가 3GB 정도 차지하더라도 여전히 약 5GB의 메모리를 차지할 수 있습니다.

<div class="content-ad"></div>

만약 maxmemory 매개변수가 설정되어 있지 않으면, Redis는 데이터를 삭제하기 위한 메모리 소거 전략을 트리거하지 않습니다.

Redis는 계속해서 새로 쓰여진 데이터를 위해 메모리를 할당할 것입니다. 할당하지 못하면 응용프로그램에서 오류를 보고할 수 있지만 당연히 다운 타임을 야기하지는 않을 것입니다.

데이터가 삭제되었는지 확인하고, top 명령어를 사용해서 볼 수 있습니다. 왜 여전히 많은 메모리를 차지하고 있는 걸까요?

해제된 메모리는 어디로 가는 걸까요?

<div class="content-ad"></div>

top 명령어를 사용하여 시스템 사용량을 확인할 때, 메모리가 여전히 높은 상태이고 Redis가 실제로 메모리를 해제하지 않았음을 알 수 있습니다.

메모리가 모두 어디로 갔을까요? 이때, info memory 명령어를 사용하여 Redis 메모리 관련 지표를 얻어야 합니다.

Redis 프로세스의 메모리 소비는 주로 다음과 같은 부분으로 구성됩니다:

- Redis 자체 시작에 의한 메모리 점유.
- 객체 데이터 메모리 저장.
- 버퍼 메모리: 주로 client-output-buffer-limit 클라이언트 출력 버퍼, 복사 백로그 버퍼 및 AOF 버퍼로 구성됩니다.
- 메모리 단편화.

<div class="content-ad"></div>


![Redis Memory Fragmentation](/assets/img/2024-07-13-In-depthanalysisofRedismemoryfragmentation_1.png)

Redis 자체 빈 프로세스에 의해 점유된 메모리는 매우 작고 무시할 수 있으며, 객체 메모리가 모든 데이터를 저장하는 가장 큰 메모리입니다.

버퍼에 대량의 트래픽 씬이 있는 경우 Redis 메모리가 불안정해져 제어하기 어려울 수 있으니 주의해야 합니다.

과도한 메모리 단편화는 사용 가능한 공간이 있음에도 불구하고 데이터를 저장할 수 없게 되는 결과를 낳습니다.


<div class="content-ad"></div>

Memory fragmentation = used_memory_rss actual used physical memory (RSS value) divided by used_memory actual stored data memory.

메모리 단편화란 무엇인가요?

메모리 단편화는 메모리 공간이 해제되어 있지만 데이터를 저장할 수 없는 상황을 의미합니다. 예를 들어, 여자친구와 함께 영화를 보기 위해 영화관에 간다고 가정해 봅시다. 당연히 함께 앉고 싶을 것입니다.

현재 8개의 좌석이 있고 4장의 티켓이 팔렸으며 4장이 더 구입 가능합니다. 하지만 우연히도 티켓을 구매한 사람들이 매우 이상하게도 한 좌석씩 떨어져 앉기로 했습니다.

<div class="content-ad"></div>

아직 4개의 좌석이 남아 있더라도, 연속된 두 좌석을 예매할 수 없습니다.

![이미지](/assets/img/2024-07-13-In-depthanalysisofRedismemoryfragmentation_2.png)

메모리 단편화를 일으키는 원인은 무엇인가요?

두 가지 주요 이유가 있습니다:

<div class="content-ad"></div>

- 메모리 할당기의 할당 전략.
- 키-값 쌍의 크기가 다르며 삭제 작업입니다.

다음으로, 발생한 실제 이유에 대해 설명합니다.

# 1. 메모리 할당기의 할당 전략.

Redis의 기본 메모리 할당기는 jemalloc을 사용하며, 선택적 할당기는 다음과 같습니다: glibc, tcmalloc.

<div class="content-ad"></div>

메모리 할당기는 요청에 따라 할당할 수 없지만 할당을 위해 메모리 블록의 고정 범위를 사용합니다.

예를 들어, 8바이트, 16바이트..., 2KB 및 4KB와 같이 고정 값에 가장 가까운 응용 프로그램 메모리가 있을 때, jemalloc은 고정 값에 가장 가까운 공간을 할당합니다.

이렇게 함으로써 메모리 단편화가 발생할 수 있습니다.

예를 들어, 프로그램이 1.5 KB만 필요한 경우, 메모리 할당기는 2KB 공간을 할당하며, 이 때 0.5KB는 단편화됩니다.

<div class="content-ad"></div>

이 작업을 수행하는 목적은 메모리 할당의 수를 줄이는 것입니다. 예를 들어, 데이터를 저장할 공간으로 22바이트를 신청하면, jemalloc은 32바이트를 할당합니다.

나중에 10바이트를 쓰려면 운영 체제로부터 공간을 다시 신청할 필요가 없습니다. 이전에 요청한 32바이트를 사용할 수 있습니다.

키가 삭제될 때, Redis는 즉시 메모리를 운영 체제에 반환하지 않습니다. 이는 기저 메모리 할당기의 관리 때문에 발생합니다. 예를 들어, 대부분의 삭제된 키는 여전히 다른 유효한 키와 동일한 메모리 페이지에서 할당됩니다.

또한, 비어있는 메모리 블록을 재사용하기 위해 할당기는 원래 5GB 데이터 중 2GB를 삭제합니다. 인스턴스에 데이터가 다시 추가되면 Redis의 RSS가 안정적으로 유지되어 너무 많이 증가하지 않습니다.

<div class="content-ad"></div>

이전 삭제로 인해 해제된 2GB 메모리를 기본적으로 메모리 할당기가 재사용했기 때문입니다.

## 2. 키-값 쌍의 크기가 다르고 삭제 작업

메모리 할당기는 고정된 크기에 따라 메모리를 할당하므로 할당된 메모리 공간은 일반적으로 실제 데이터가 차지하는 크기보다 큽니다. 이는 단편화를 발생시키고 메모리의 저장 효율을 저하시킵니다.

또한, 키-값 쌍의 빈번한 수정 및 삭제는 메모리 공간의 확장 및 해제로 이어집니다. 예를 들어, 원래 32바이트를 차지했던 문자열이 이제 20바이트를 차지하도록 수정되면, 해제된 12바이트는 빈 공간이 됩니다.

<div class="content-ad"></div>

다음 데이터 저장 요청이 13바이트 문자열을 적용해야 할 경우, 방금 해제된 12바이트 공간을 사용할 수 없어서 단편화가 발생할 수 있습니다.

단편화의 가장 큰 문제점은 총 공간이 충분히 크지만 이러한 메모리가 연속적이지 않아 데이터를 저장할 수 없을 수 있다는 것입니다.

어떻게 이 문제를 해결할 수 있을까요?

우선, 메모리 단편화가 발생했는지 여부를 판단해야 하며, 이전 info memory 명령으로 알려진 mem_fragmentation_ratio 지표에 주목하여 메모리 단편화 비율을 나타냅니다.

<div class="content-ad"></div>

```js
mem_fragmentation_ratio = used_memory_rss / used_memory
```

만약 ` mem_fragmentation_ratio ` 값이 1보다 크고 1.5보다 작으면 합리적이다고 생각할 수 있습니다. 그러나 값이 1.5보다 크다면, 50% 이상의 조각화가 발생한 것이므로, 과다한 조각화 문제를 해결하기 위해 조치를 취해야 합니다.

# 1. 재부팅하기.

가장 간단한 방법은 재부팅하는 것입니다. 영속화가 활성화되어 있지 않다면, 데이터가 손실될 수 있습니다.

<div class="content-ad"></div>

영속성을 활성화하면 데이터를 복원하기 위해 RDB 또는 AOF를 사용해야 합니다. 하나의 인스턴스만 있는 경우 대량 데이터는 복구 단계 중에 서비스를 오랫동안 제공하지 못할 수 있으며 가용성이 크게 감소할 수 있습니다.

# 2. 메모리 조각 자동 정리

Redis 버전 4.0 이후로 메모리 단편화 정리 메커니즘이 제공됩니다.

Redis의 경우 연속된 메모리 공간이 여러 불연속 공간으로 나눠지면 운영 체제가 먼저 데이터를 이동하고 이어 붙여서 원래의 데이터가 차지하던 공간을 해제하여 연속된 빈 메모리 공간을 형성합니다.

<div class="content-ad"></div>


![image](/assets/img/2024-07-13-In-depthanalysisofRedismemoryfragmentation_3.png)

자동 클리닝은 좋지만, 무모하게 하지 마세요. 운영 체제는 데이터를 새 위치로 이동하고 원래 공간을 해제하기 위해 리소스를 소비해야 합니다.

Redis의 데이터 운영 명령은 단일 스레드로 작동하기 때문에 데이터가 복사되고 이동되면, 잔해를 청소한 후에 요청이 처리될 수 있어 성능이 저하됩니다.

성능에 미치는 영향을 피하고 자동 클리닝을 실현하는 방법은 무엇일까요?


<div class="content-ad"></div>

좋은 질문이에요! 메모리 단편화 정리와 마무리 시점을 제어하는 데 다음 두 가지 매개변수를 사용하세요. 너무 많은 CPU를 사용하지 않도록 주의하고 Redis 처리 요청에 대한 단편화 정리의 성능 영향을 줄일 수 있어요.

자동 메모리 단편화 기능을 활성화하세요.

```js
CONFIG SET activedefrag yes
```

이것은 자동 클리닝을 활성화하는 것뿐이에요. 클리닝 작업이 트리거되기 위해서는 클리닝이 동시에 다음 두 조건을 충족해야 해요.

<div class="content-ad"></div>

# 1. 클리닝 조건.

- active-defrag-ignore-bytes 200mb: 메모리 단편화로 인해 메모리 소비량이 200MB에 도달하면 클리닝이 시작됩니다.
  
- active-defrag-threshold-lower 20: 메모리 단편화 공간이 Redis가 시스템에 할당한 공간의 20%를 초과하면 클리닝이 시작됩니다.

# 2. 성능에 영향을 피하기.

<div class="content-ad"></div>

정리해 보겠습니다.

청소 시간이 확보되어 있고, 성능에 미치는 청소의 영향을 제어해야 합니다. 먼저 CPU 리소스를 할당하여 청소 작업이 정상적으로 수행되고 Redis 처리 요청에 미치는 성능 영향을 피할 수 있도록 할 것입니다.

- active-defrag-cycle-min 20: 자동 조각 모음 과정 중에 CPU 시간의 비중은 20% 이상이어야 합니다. 이를 통해 청소 작업이 정상적으로 수행될 수 있습니다.

- active-defrag-cycle-max 50: 자동 청소 프로세스에서 CPU 시간의 비중은 50%를 초과해서는 안 됩니다. 초과할 경우 Redis가 차단되고 높은 latency가 발생하는 것을 피하기 위해 청소가 즉시 중지됩니다.

요약하면, 성능에 미치는 영향을 최소화하고 청소 작업이 원활하게 이루어질 수 있도록 CPU 리소스 할당을 설정할 것입니다.

<div class="content-ad"></div>

만약 Redis에서 데이터를 저장하는 데 사용되는 메모리가 운영 체제에 의해 할당된 메모리보다 훨씬 작을지라도 데이터가 저장되지 않는 경우, 메모리 단편화가 많을 수 있습니다.

info memory 명령을 사용하여 메모리 단편화 비율 지표 mem_fragmentation_ratio가 정상인지 확인해보세요.

그런 다음 자동 정리를 활성화하고 정리 타이밍과 CPU 자원 사용량을 합리적으로 설정하세요. 이 메커니즘은 메모리 복사를 포함하며 Redis 성능에 잠재적인 위험을 노출시킵니다.

Redis 성능이 느려진 경우, 단편을 정리하는 데 문제가 있는지 확인하세요. 그렇다면 active-defrag-cycle-max 값을 줄이세요.

<div class="content-ad"></div>

만약 이런 이야기를 좋아하고 제게 지원하고 싶다면, 박수를 보내주세요.

여러분의 지원은 제게 매우 중요합니다. 감사합니다.

# 코딩 실력을 업그레이드하세요

저희 커뮤니티의 일원이 되어 주셔서 감사합니다! 떠나시기 전에:

<div class="content-ad"></div>

- 👏 이야기에 박수를 보내고 작가를 팔로우하세요 👉
- 📰 "Level Up Coding" 게시물에서 더 많은 콘텐츠 확인하기
- 🔔 팔로우하기: Twitter | LinkedIn | 뉴스레터

🚀👉 "Level Up" 재능 집단에 가입하고 멋진 직업을 찾아보세요