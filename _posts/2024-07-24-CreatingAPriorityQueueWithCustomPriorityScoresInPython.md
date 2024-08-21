---
title: "Python에서 사용자 정의 우선순위 점수를 사용하는 우선순위 큐 만드는 방법"
description: ""
coverImage: "/assets/img/2024-07-24-CreatingAPriorityQueueWithCustomPriorityScoresInPython_0.png"
date: 2024-07-24 11:51
ogImage: 
  url: /assets/img/2024-07-24-CreatingAPriorityQueueWithCustomPriorityScoresInPython_0.png
tag: Tech
originalTitle: "Creating A Priority Queue With Custom Priority Scores In Python"
link: "https://medium.com/gitconnected/creating-a-priority-queue-with-custom-priority-scores-in-python-6faa02304b56"
isUpdated: true
---




![이미지](/assets/img/2024-07-24-CreatingAPriorityQueueWithCustomPriorityScoresInPython_0.png)

우선 순위 큐는 어떤 시점에서도 가장 높은 우선 순위 요소가 먼저 나오는 큐입니다.

가정상의 응급 클리닉을 상상해보세요.

다양한 부상 정도를 가진 환자들이 무작위로 들어갑니다.

<div class="content-ad"></div>

그리고 의사가 있는 경우에는 가장 심각한 부상을 입은 환자를 치료해야 합니다.

환자가 먼저 도착한 것에 관계없습니다.

이것은 우선 순위 큐입니다. 부상의 심각성을 우선 순위 점수로 생각해 보세요.

# Python에서 우선 순위 큐

<div class="content-ad"></div>

우선순위 큐를 만드는 가장 쉬운 방법 중 하나는 힙을 사용하는 것입니다.

힙(최소 힙)은 자식이 부모보다 크거나 같은 이진 트리입니다.

최대 힙은 자식이 부모보다 작거나 같은 이진 트리이지만, 여기서는 최소 힙에만 초점을 맞추겠습니다.

내장된 heapq 모듈을 사용하여 최소 힙을 만들 수 있습니다:

<div class="content-ad"></div>

```js
import heapq

min_heap = [5, 4, 3, 2, 1]

# 이 코드 라인은 'min_heap'을 최소 힙으로 변환합니다.
heapq.heapify(min_heap)
```

참고 - Python의 heapq에서 힙은 리스트로 표시되므로 출력하면 리스트처럼 보이는 것에 대해 걱정할 필요가 없습니다.

# 일부 heapq 작업

최소 힙에서 요소를 제거(팝)하는 작업입니다. 각 시간에 가장 작은 요소가 나온다는 것에 주목하세요. 이 작업은 로그 시간에 발생합니다.

<div class="content-ad"></div>

```js
import heapq

min_heap = [5, 4, 2, 1, 3]
heapq.heapify(min_heap)

x = heapq.heappop(min_heap)
print(x)  # 1

x = heapq.heappop(min_heap)
print(x)  # 2

x = heapq.heappop(min_heap)
print(x)  # 3
```

우리의 min 힙에 무언가를 삽입(푸시)합니다. 이것도 로그 시간에 발생합니다.

```js
import heapq

min_heap = [5, 4, 2, 1, 3]
heapq.heapify(min_heap)

heapq.heappush(min_heap, -1)  # min 힙에 -1을 추가
heapq.heappush(min_heap, 0)   # min 힙에 0을 추가
heapq.heappush(min_heap, 6)   # min 힙에 6을 추가

for _ in range(5):
    print(heapq.heappop(min_heap))

# -1 0 1 2 3
```

# heapq는 min 힙만 지원합니다. 그렇다면 max 힙을 어떻게 구현할까요?

<div class="content-ad"></div>

최대 힙을 구현하려면 리스트에 튜플이 포함되어야 합니다.

튜플 안에는 (우선순위 점수, 실제 값)이 들어 있습니다.

```python
import heapq

values = [5, 2, 4, 1, 3]

max_heap = [(-i, i) for i in values]

print(max_heap)

# [(-5, 5), (-2, 2), (-4, 4), (-1, 1), (-3, 3)]
```

이제 힙을 구성합니다.

<div class="content-ad"></div>

```js
heapq.heapify(max_heap)
```

그리고 아이템들을 pop할 때 가장 높은 값이 항상 먼저 나온다는 점을 주의하세요.

```js
import heapq

values = [5, 2, 4, 1, 3]
max_heap = [(-i,i) for i in values]

heapq.heapify(max_heap)

for _ in range(4):
    score, val = heapq.heappop(max_heap)
    print(val)

# 5 4 3 2
```

^ 이 방법으로, 우리는 heapq 를 사용하여 쉽게 max heap을 만들었어! heapq는 기본적으로 min 힙만 지원하는데, 이런 방식을 사용하면 작은 값이 아닌 큰 값이 먼저 나와서 사용할 수 있지!

<div class="content-ad"></div>

코드를 추가할 때는 (우선 순위 점수, 실제 값) 튜플을 전달해야 합니다. 그렇지 않으면 힙이 올바르게 작동하지 않을 수 있습니다.

```python
import heapq

values = [5, 2, 4, 1, 3]
max_heap = [(-i, i) for i in values]

heapq.heapify(max_heap)

heapq.heappush(max_heap, (-6, 6))      # 6 추가
heapq.heappush(max_heap, (-100, 100))  # 100 추가

for _ in range(4):
    score, val = heapq.heappop(max_heap)
    print(val)

# 100 6 5 4
```

# 더 복잡한 값 다루기

```python
students = [
    {'name': 'tom', 'score': 77, 'height': 1.78},
    {'name': 'jerry', 'score': 97, 'height': 1.88},
    {'name': 'jessie', 'score': 87, 'height': 1.48},
    {'name': 'james', 'score': 17, 'height': 1.58},
]
```

<div class="content-ad"></div>

학생들의 우선 순위 큐를 만들고 싶다고 가정해보겠습니다. 힙(heap)에서 가장 키가 큰 학생이 먼저 나오게 될 것입니다.

우리는 먼저 요소들을 (우선 순위 점수, 실제 값)을 포함하는 튜플로 변환해야 합니다.

```js
max_heap = [(-s['height'], s) for s in students]

print(max_heap)

'''
[
  (-1.78, {'name': 'tom', 'score': 77, 'height': 1.78}), 
  (-1.88, {'name': 'jerry', 'score': 97, 'height': 1.88}), 
  (-1.48, {'name': 'jessie', 'score': 87, 'height': 1.48}), 
  (-1.58, {'name': 'james', 'score': 17, 'height': 1.58})
]
'''
```

^ 우선 순위 점수가 음의 키(height)인 것을 주목하세요. 이는 학생들의 키를 고려하는 최대 힙(max heap)을 원하기 때문입니다.

<div class="content-ad"></div>

다음으로, 우리는 heapify를 진행합니다. 학생들을 pop할 때 가장 키큰 학생이 먼저 나온다는 것을 주목하세요.

```python
import heapq
heapq.heapify(max_heap)

for _ in range(4):
    score, val = heapq.heappop(max_heap)

'''
{'name': 'jerry', 'score': 97, 'height': 1.88}
{'name': 'tom', 'score': 77, 'height': 1.78}
{'name': 'james', 'score': 17, 'height': 1.58}
{'name': 'jessie', 'score': 87, 'height': 1.48}
'''
```

# 결론

heapq를 최대한 활용하고 사용자 정의하려면, 각 요소를 (우선 순위 점수, 실제 값) 튜플로 변환해야 합니다.

<div class="content-ad"></div>

그리고 우리는 우선 순위 점수에 대해 원하는 대로 할 수 있어요.

# 결론

이해하기 쉽고 명확했으면 좋겠어요

# 저를 지원하고 싶다면

<div class="content-ad"></div>

- 저의 Substack 뉴스레터에 가입해주세요. 링크: https://zlliu.substack.com/ — 저는 매주 Python과 관련된 이메일을 보내고 있어요.
- 제 책 '101 Things I Never Knew About Python'을 구매해주세요. 링크: https://payhip.com/b/vywcf
- 이 이야기를 응원하는 박수 50번!
- 당신의 생각을 남겨주세요. 댓글을 통해 소통해요!
- 이 이야기에서 제일 마음에 드는 부분을 강조해주세요.

감사합니다! 이 작은 조치들이 큰 도움이 되고, 정말 감사드려요!

YouTube: https://www.youtube.com/@zlliu246

LinkedIn: https://www.linkedin.com/in/zlliu/