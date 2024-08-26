---
title: "파티션 테이블을 다루는 Rails에서의 함정과 모범 사례"
description: ""
coverImage: "/assets/img/2024-08-26-FromPitfallstoBestPracticesWorkingwithPartitionedTablesinRails_0.png"
date: 2024-08-26 19:10
ogImage: 
  url: /assets/img/2024-08-26-FromPitfallstoBestPracticesWorkingwithPartitionedTablesinRails_0.png
tag: Tech
originalTitle: "From Pitfalls to Best Practices Working with Partitioned Tables in Rails."
link: "https://medium.com/@drachevskii/avoid-issues-with-partitioned-table-in-rails-39a4fe337cca"
isUpdated: false
---


![Partitioned Table](/assets/img/2024-08-26-FromPitfallstoBestPracticesWorkingwithPartitionedTablesinRails_0.png)

파티션이 분할된 테이블을 생성하거나 업데이트하는 방법에 대한 가이드는 많지만, 작업하는 데 주의해야 할 점 및 도전 과제에 대한 정보는 거의 제공되지 않습니다. 이에 대한 좋은 예는 있지만, 오늘은 파티션이 분할된 테이블을 안전하고 효율적으로 다루는 방법에 대한 제 경험을 공유하고 싶습니다.

# 파티션 간 순차 스캔

가장 빈번하고 명백하지 않은 문제는 일반적인 DB 성능 측정 지표로는 알 수 없기 때문입니다. 순차 스캔은 쿼리가 파티션 구조를 최대한 활용하지 못할 때 주로 발생합니다. 관련 파티션만 스캔하는 대신, 데이터베이스는 모든 파티션을 스캔하여 성능을 크게 저하시킵니다.
SQL 쿼리를 수동으로 작성할 때 이 문제를 피하는 것은 비교적 쉽습니다. 파티션 키의 사용을 명시적으로 제어할 수 있기 때문입니다. 그러나 Rails에서는 일반적으로 SQL을 추상화하는 ActiveRecord를 사용하므로 생성된 쿼리를 자주 보지 못합니다.

<div class="content-ad"></div>

테이블이 created_at 필드로 분할된 일반적인 예시를 살펴봅시다:

```js
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    avatar_id INTEGER NOT NULL,
    created_at TIMESTAMP NOT NULL
) PARTITION BY RANGE (created_at);

CREATE TABLE users_2023_q1 PARTITION OF users
    FOR VALUES FROM ('2023-01-01') TO ('2023-04-01');

CREATE TABLE users_2023_q2 PARTITION OF users
    FOR VALUES FROM ('2023-04-01') TO ('2023-07-01');
```

사용자의 created_at 값을 지정하지 않고 사용자를 검색할 때 파티션 스캔이 발생할 것임을 쉽게 예상할 수 있습니다:

```js
# User.find(123)
EXPLAIN ANALYZE SELECT * FROM users WHERE id = 123 LIMIT 1;
...
Seq Scan on users_2023_q1  (cost=0.00..35.50 rows=5 width=100) (actual time=0.005..0.005 rows=0 loops=1)
Seq Scan on users_2023_q2  (cost=0.00..35.50 rows=5 width=100) (actual time=0.005..0.005 rows=0 loops=1)
...
# User.last(10)
...
Seq Scan on users_2023_q1  (cost=0.00..35.50 rows=5 width=100) (actual time=0.005..0.005 rows=0 loops=1)
Seq Scan on users_2023_q2  (cost=0.00..35.50 rows=5 width=100) (actual time=0.005..0.005 rows=0 loops=1)
...
```

<div class="content-ad"></div>

그러나 객체를 다룰 때도 동일한 순차 검색을 받게 된다는 점은 명확하지 않을 수 있습니다:

```js
# user = User.find_by(id: 123, created_at: '2023-01-01')
SELECT * FROM users WHERE id = 123, created_at = '2023-01-01' LIMIT 1;
# 파티션별로 seq scan 없음
# user.update(avatar_id: 123)
EXPLAIN ANALYSE UPDATE users SET avatar_id = 123 WHERE id = 123;
...
Seq Scan on users_2023_q1  (cost=0.00..35.50 rows=5 width=100) (actual time=0.005..0.005 rows=0 loops=1)
Seq Scan on users_2023_q2  (cost=0.00..35.50 rows=5 width=100) (actual time=0.005..0.005 rows=0 loops=1)
...
```

관계를 통해 사용자를 선택할 때도 동일한 문제가 발생합니다:

```js
# avatar = Avatar.find(123)
# user = avatar.user
SELECT * FROM users WHERE avatar_id = 123 LIMIT 1;
...
Seq Scan on users_2023_q1  (cost=0.00..35.50 rows=5 width=100) (actual time=0.005..0.005 rows=0 loops=1)
Seq Scan on users_2023_q2  (cost=0.00..35.50 rows=5 width=100) (actual time=0.005..0.005 rows=0 loops=1)
...
```

<div class="content-ad"></div>

ActiveRecord가 파티션을 완전히 지원하지 않아서 ActiveRecord 쿼리를 사용하는 방식과 해당 쿼리가 순차 스캔을 생성하는지 여부를 주의해야 합니다.

해결 방법:

ActiveRecord에서 제공하는 각종 편의 기능을 최소화하고 필요한 파티션을 찾는 데 도움이 될 수 있도록 명시적인 파티션 키를 사용해야 합니다:

```js
user = User.find_by(id: 123, created_at: '2023-01-01')
User.where(id: user.id, created_at: user.created_at).update_all(avatar_id: 123)

avatar = Avatar.find(123)
user = User.where(avatar_id: avatar.id, created_at: '2023-01-01')
```

<div class="content-ad"></div>

# 비효율적인 upsert_all 사용

레일즈에서 파티션 테이블을 다룰 때 또 다른 공통 문제는 upsert_all 메서드의 비효율적인 사용입니다. 이 문제는 upsert_all이 기본적으로 파티션 테이블에 최적화되어 있지 않기 때문에 발생합니다. 파티션 키가 unique_by 제약 조건이나 쿼리의 WHERE 절에 포함되지 않은 경우, PostgreSQL은 기록을 삽입하거나 업데이트할 올바른 파티션을 찾기 위해 모든 파티션을 스캔해야 할 수도 있습니다. 이는 여러 파티션을 가로지르는 불필요한 순차 스캔으로 인해 상당한 성능 저하를 초래할 수 있습니다.

이제 upsert_all을 사용하여 여러 사용자를 업데이트하려고 한다고 가정해 봅시다. 이 예제에서 unique_by: :id는 파티션 키(created_at)를 포함하지 않기 때문에 PostgreSQL은 올바른 파티션을 찾기 위해 모든 파티션을 스캔해야 할 수 있습니다:

```js
# User.upsert_all([
#   { id: 123, avatar_id: 456, created_at: '2023-01-15' },
#   { id: 124, avatar_id: 789, created_at: '2023-05-10' }
# ], unique_by: :id)

EXPLAIN ANALYZE
INSERT INTO users (id, avatar_id, created_at)
VALUES (123, 456, '2023-01-15'), (124, 789, '2023-05-10')
ON CONFLICT (id)
DO UPDATE SET avatar_id = EXCLUDED.avatar_id;
...
Seq Scan on users_2023_q1  (cost=0.00..35.50 rows=1 width=100) (actual time=0.005..0.005 rows=1 loops=1)
Seq Scan on users_2023_q2  (cost=0.00..35.50 rows=1 width=100) (actual time=0.005..0.005 rows=1 loops=1)
...

<div class="content-ad"></div>

문제를 피하는 방법:

upsert_all을 사용할 때 unique_by 절에 항상 파티션 키를 포함하세요. 이렇게 하면 PostgreSQL이 올바른 파티션을 효율적으로 찾아 삽입 또는 업데이트를 적용할 수 있으며 관련 없는 파티션을 스캔하지 않습니다:

# User.upsert_all([
#   { id: 123, avatar_id: 456, created_at: '2023-01-15' },
#   { id: 124, avatar_id: 789, created_at: '2023-05-10' }
# ], unique_by: %i[id created_at])

EXPLAIN ANALYZE
INSERT INTO users (id, avatar_id, created_at)
VALUES (123, 456, '2023-01-15'), (124, 789, '2023-05-10')
ON CONFLICT (id, created_at)
DO UPDATE SET avatar_id = EXCLUDED.avatar_id;

# 외부 키와 관련 자식 항목 처리하기

<div class="content-ad"></div>

파티션이 지정된 테이블과 연관 관계를 설정하는 것은 조금 까다로울 수 있습니다. ActiveRecord의 연관 관계(belongs_to, has_many 등)는 파티셔닝을 자동으로 처리해주지 않습니다. 특히 연관 관계에 파티션 키가 포함되지 않은 경우, 여러 파티션을 스캔하는 비효율적인 쿼리가 발생할 수 있습니다.

또한 파티션이 지정된 테이블을 다른 테이블과 조인하는 경우, 조인 조건에 파티션 키가 포함되지 않으면 복잡하고 느린 쿼리가 발생할 수 있습니다. 특히 조인이 여러 파티션에 흩어진 대규모 데이터 집합을 처리해야 할 때 문제가 될 수 있습니다.

다음은 이러한 문제를 피하는 방법입니다:

# 파티션 키를 사용하여 사용자 및 그들의 아바타를 가져오기
users_with_avatars = User.joins(:avatar)
                         .where("avatars.id = ? AND users.created_at = ?", 456, '2023-01-15')

<div class="content-ad"></div>

# Rails에서 분할된 테이블을 사용하는 최상의 방법

쿼리 최적화:

- 항상 EXPLAIN을 사용하여 분할된 테이블에서 쿼리의 성능을 분석하십시오. 또한 쿼리가 연속 스캔을 생성하는지 여부를 확인하는 유일한 방법이거나 거의 그렇습니다.
- 파티션 키를 포함하는 인덱싱 전략을 구현하여 쿼리가 특정 파티션을 대상으로 할 수 있도록 합니다.
- 파티션 키별로 명시적으로 필터링하는 쿼리를 작성하여 모든 파티션에 대해 불필요한 스캔을 피하십시오.

대량 삽입 및 업데이트 처리:

<div class="content-ad"></div>

- 대량 작업을 효율적으로 관리하기 위해 작은 배치로 나누어 처리하세요.
- upsert_all을 사용할 때는 성능 병목 현상을 피하기 위해 파티션 키를 포함시켜 주의깊게 사용하세요.

마이그레이션 관리:

- 파티션된 환경에서 스키마 변경이 각 분할에 적용되어야 하므로 마이그레이션 계획을 신중히 세우세요.
- 복잡한 작업에 대해 raw SQL을 사용하고 파티션을 추가, 제거 또는 수정할 때 데이터 무결성과 성능에 미치는 영향을 고려하세요.

테스트와 모니터링:

<div class="content-ad"></div>

- 제작 환경과 유사한 스테이징 환경에서 파티셔닝된 테이블 설정을 철저히 테스트하세요.
- 생산 환경에서 쿼리 및 작업이 효율적으로 실행되는지 모니터링 도구를 사용하여 파티션 성능을 확인하세요.

# 결론

파티션된 테이블을 사용하면 대규모 테이블의 지연 시간을 줄이는 좋은 도구가 될 수 있습니다. 하지만 실수하기 쉬우므로 조심히 사용해야 합니다. 이 내용이 Rails에서 파티션된 테이블을 다루는 방법을 이해하는 데 도움이 되었기를 바랍니다.