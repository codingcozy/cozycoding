---
title: "판타지 축구 전략 조합 최적화 문제"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-21 17:05
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Fantasy Football (The Combinatorial Optimisation Problem)"
link: "https://medium.com/@antbr/fantasy-football-the-combinatorial-optimisation-problem-efaff9b4b50b"
isUpdated: false
---


## FPL: 기사 1

우리 영국 동포들은 잘 알고 계시겠지만, 올해도 많은 사람들이 영국의 최고 조건인 프리미어 리그를 시청하기 시작하는 시기입니다.
이와 함께 판타지 프리미어 리그가 제공됩니다. 여기서 팬들은 선수들로 가상의 팀을 구성하여 시즌이 진행되는 동안 내면의 호세 무리뉴를 연출할 수 있습니다.

자연스럽게 데이터 과학자로서 나의 첫 번째 본능은 나와 경쟁하고 있는 친구들을 통계로 깎아내리는 것입니다. 다행히 FPL은 무료이고 간단한 API를 대중에게 제공합니다. 이 기사의 나머지 부분에서는 첫 번째 주인 Gameweek 1로 돌아가 가장 높은 점수를 획득한 선수 조합을 찾아보려고 합니다.

## 데이터 얻기

<div class="content-ad"></div>

이 실험에는 Python을 사용할 것입니다. 먼저, API에서 데이터를 가져올 수 있습니다:

```js
import requests
base_url = 'https://fantasy.premierleague.com/api/'
data = requests.get(base_url+'bootstrap-static/').json()
```

요소 객체를 살펴보면 데이터를 더 자세히 살펴볼 수 있습니다. 이 객체에는 고유한 축구 선수에 해당하는 각 플레이어 객체의 목록이 포함되어 있습니다. 다음과 같이 모하메드 살라(Mohamed Salah)의 항목을 찾을 수 있습니다 (일부 반환 필드는 명확성을 위해 생략되었습니다):

```js
players = data['elements']
pprint([player for player in players if player['web_name'] == 'M.Salah'])

>>>[{'element_type': 3,
     'event_points': 14,
     'first_name': 'Mohamed',
     'id': 328,
     'second_name': 'Salah',
     'selected_by_percent': '34.3',
     'team': 12,
     'total_points': 14,
     'web_name': 'M.Salah'}]
```

<div class="content-ad"></div>

## 문제

이전에 말씀드린 대로, 최대 점수를 획득할 수 있는 선수들의 최적 조합을 찾고 싶다고 언급했습니다. 이는 단순히 가장 많은 점수를 획득한 n명의 선수를 반환하는 것만큼 간단하지 않습니다. 즉, 일부 제약 조건을 준수해야 합니다. FPL의 규칙에 따르면 팀은 총 15명의 선수로 구성되어야 하며, 그 중 2명은 골키퍼, 5명은 수비수, 5명은 중앙 미드필더이고 3명은 공격수여야 합니다. 또한 각 선수는 구매 비용이 있어 팀의 총 예산 100백만 파운드에 기여합니다.

수학적으로 이러한 문제는 배낭 문제(Knapsack problem)라고 불립니다. 간단히 말해, 최대 용량을 가진 '가방'을 다양한 질량의 물건으로 채워 가장 큰 용량에 최대한 가까워지는 것을 목표로 합니다.

## 해결책

<div class="content-ad"></div>

PuLP, 선형 프로그래밍 모듈을 사용할 것입니다. 우리는 값들을 제공하고 제약 조건을 정의하면 문제의 해결책을 찾아줍니다. 먼저, 이번 주에 각 선수의 ID, 비용, 포지션 및 이번 주에 얻은 포인트에 해당하는 튜플 목록을 생성할 것입니다:

```js
# 튜플 목록 생성
player_info = [(player['id'],
                player['now_cost'],
                player['element_type'],
                player['event_points']) for player in players]

# 관련 데이터 추출
pids = [pid[0] for pid in player_info]
costs = [cost[1] for cost in player_info]
position = [position[2] for position in player_info]
points = [points[3] for points in player_info]
```

그런 후에 PuLP를 사용하여 문제와 제약 조건을 정의할 것입니다. 데이터에서 포지션이 번호로 되어 있습니다. GK=1, DEF=2, MID=3, FWD=4이며, 비용 값은 십만 단위로 되어 있습니다 (£100m에 해당하는 값은 1000입니다).

```js
import pulp
# 문제 정의
problem = pulp.LpProblem("selectPlayers", pulp.LpMaximize)

# 각 선수를 선택하기 위해 0 또는 1로 결정 변수 정의
x = pulp.LpVariable.dicts("select", pids, cat="Binary")

# 목적 함수: 총 포인트 최대화
problem += pulp.lpSum([points[i] * x[pids[i]] for i in range(len(pids))])

# 제약 조건 1: 총 비용은 예산 내여야 함
problem += pulp.lpSum([costs[i] * x[pids[i]] for i in range(len(pids))]) <= 1000

pos = {'1': 2, '2': 5, '3': 5, '4': 3}  # 포지션별로 필요한 선수 수
# 제약 조건 2: 포지션 캡
for val in pos:
    problem += pulp.lpSum([x[pids[i]] for i in range(len(pids)) if position[i] == int(val)]) >= pos[val]

# 제약 조건 3: 정확히 15명의 선수를 선택해야 함
problem += pulp.lpSum([x[pids[i]] for i in range(len(pids))]) == 15

# 문제 해결
problem.solve()
```

<div class="content-ad"></div>

문제를 올바르게 정의했다면 최적의 해결책을 찾을 수 있어요:

```js
[('Havertz', 4, 12),
 ('Saka', 3, 12),
 ('Onana', 3, 10),
 ('Wissa', 4, 11),
 ('Veltman', 2, 8),
 ('Welbeck', 4, 12),
 ('Alexander-Arnold', 2, 8),
 ('M.Salah', 3, 14),
 ('Ederson M.', 1, 8),
 ('Kovačić', 3, 11),
 ('Martinez', 2, 7),
 ('Mazraoui', 2, 8),
 ('Joelinton', 3, 11),
 ('Pope', 1, 9),
 ('Pedro Porro', 2, 9)]

총 점수:  150
주장 보너스 적용 후 총 점수: 164
총 비용:  950
```

이번 주 가장 높은 점수를 받은 팀이 127점을 받았다면, 164점은 정말 잘 한 일이에요.

이 기사는 판타지 프리미어 리그 API 내에서 데이터 과학을 사용하는 시리즈의 일부를 이루고 있어요. 미래의 기사에서는 회귀, 데이터 퓨전, 그리고 의사 결정을 위한 선호도 기반 분석 등에 대해 더 다룰 예정이에요.