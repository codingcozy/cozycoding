---
title: "Postgres Lateral Query 테이블 하위 그룹에서 행 추출하는 방법"
description: ""
coverImage: "/assets/img/2024-07-10-PostgresLateralQueryExtractingrowsfromtablesub-group_0.png"
date: 2024-07-10 03:05
ogImage: 
  url: /assets/img/2024-07-10-PostgresLateralQueryExtractingrowsfromtablesub-group_0.png
tag: Tech
originalTitle: "Postgres Lateral Query: Extracting rows from table sub-group"
link: "https://medium.com/@krishnaraam/postgres-lateral-query-extracting-rows-from-table-sub-group-b53722d7c3ad"
---


## 리소스 수명주기 관리: 측면 쿼리의 실용적 사용 사례 기반 설명

## 전제 조건

독자는 REST API, PostgreSQL 및 UI/UX 디자인 원칙에 대해 알고 있어야 합니다.

## 소개

<div class="content-ad"></div>

이 기사에서는 포스트그레스에서 측면 쿼리를 사용하여 테이블 행을 반복하고 논리 하위 그룹에서 특정 행을 반환하는 방법에 대해 설명하겠습니다. 데이터 검색 부분을 이해하는 것만 관심이 있는 독자는 문제 설명 섹션으로 건너뛸 수 있습니다.

## 자원 수명주기 관리

자원은 문서, 이미지 또는 기타 구조화되거나 비구조화된 데이터가 될 수 있습니다.

수명주기는 자원이 거치는 단계입니다.

<div class="content-ad"></div>

리소스 라이프사이클 관리는 해당 리소스가 모든 단계를 거치도록 하는 프로세스들을 의미합니다. 리소스 전환은 "프로모션"이라는 단계를 통해 이뤄집니다.

예를 들어, 템플릿 라이프사이클 관리를 살펴봅시다.
개발자가 웹사이트를 구축하여 템플릿을 디자인하고 이를 출력하기 위한 API를 제공한다고 가정해봅시다. DEV, STAGE 그리고 PROD 세 가지 환경에 프린터가 준비되어 있는 상황이라고 가정해보죠.

전형적인 템플릿 디자인은 다음과 같습니다.
1. 템플릿이 처음 디자인되면, DEV 라이프사이클 단계에 있다고 말합니다. DEV 환경의 프린터에서만 인쇄할 수 있습니다.
2. DEV 단계에서 디자인을 완료한 후, 디자이너는 해당 템플릿을 STAGE 환경으로 프로모션합니다. 이제 STAGE 프린터에서 인쇄할 수 있습니다.
3. STAGE에서 인쇄가 성공적으로 이루어지면, 디자이너는 템플릿을 PROD 환경으로 프로모션하여 실제 PROD 프린터 전체에서 인쇄할 수 있습니다.

<div class="content-ad"></div>

미래에 템플릿에 변경이 필요한 경우 DEV에서 재설계하고 STAGE와 PROD로 프로모션해야 한다는 점을 유의해야 합니다.

이것은 본질적으로 템플릿 라이프사이클 관리입니다. 이 프로세스는 이미지, 문서 등 모든 종류의 리소스에 적용될 수 있습니다.

# 문제 발생

리소스 라이프사이클 관리에서는 모든 라이프사이클 단계에 있는 최신 리소스를 표시해야 합니다.
UI는 어떤 라이프사이클 단계에 어떤 리소스를 표시할지를 결정할 수 있습니다.

<div class="content-ad"></div>

## 리소스 스키마

![Resource Schema](/assets/img/2024-07-10-PostgresLateralQueryExtractingrowsfromtablesub-group_0.png)

- id: 기본 키
- lifecycle: 개발, 스테이징 또는 프로덕션
- name: 리소스 이름
- team: 해당 리소스에 속한 팀 이름
- version: 리소스 버전 번호

## API 작업

<div class="content-ad"></div>

- **Promote:** When a resource is promoted, the version number is increased, and the current state is saved in the resource table.

- **Resource Fetch:** This API fetches the most recent resource based on the team, name, and lifecycle for display. The fetched data represents the resource with the highest version number and requires a database query to retrieve it.

## Resource Data

**Table Name:** `resource`  
**Teams:** alpha, beta, and gamma  
**Resources:** `resource_1`, `resource_2`, `resource_3`, and `resource_4`  
**Lifecycles:** DEV, STG, and PRD  

**Summary:**
1. `resource_1` from team alpha has three versions, all promoted from DEV to STAGE to PRODUCTION.
2. `resource_2` from team alpha has two versions, all promoted from DEV to STAGE to PRODUCTION.
3. `resource_3` from team beta has one version promoted from DEV to STAGE to PRODUCTION.
4. `resource_4` from team gamma has four versions, all promoted from DEV to STAGE to PRODUCTION.

<div class="content-ad"></div>


| id | name       | lifecycle | version | team  |
|----|------------|-----------|---------|-------|
| 12 | resource_1 | PRD       | 3       | alpha |

| 18 | resource_2 | PRD       | 2       | alpha |

| 21 | resource_3 | PRD       | 1       | beta  |

| 33 | resource_4 | PRD       | 4       | gamma |


## 최근에 프로모션된 자원 추출하기

UI에서는 자원의 최신 버전만 표시해야 합니다.

아래 강조된 텍스트는 추출해야 할 행입니다.

<div class="content-ad"></div>

![Postgres Lateral Query](/assets/img/2024-07-10-PostgresLateralQueryExtractingrowsfromtablesub-group_1.png)

우리는 이 과정을 직관적으로 다음과 같이 시각화할 수 있습니다.

## 해결책

이 작업에는 테이블을 반복하고 각 리소스 이름, 팀 및 라이프사이클의 가장 높은 버전을 가진 행을 찾아야 합니다. 이를 수행하는 다른 방법이 있을 수 있지만, 그 중 하나가 lateral query 방법입니다.

<div class="content-ad"></div>

```js
SELECT *
FROM resource
WHERE id IN (
    SELECT DISTINCT latest_resource.id
    FROM (
        SELECT resource_with_latest_version.id
        FROM resource AS r1
        INNER JOIN LATERAL (
            SELECT *
            FROM resource AS r2
            WHERE r2.team = r1.team
              AND r2.lifecycle = r1.lifecycle
              AND r2.name = r1.name
            ORDER BY r2."version" DESC
            LIMIT 1
        ) AS resource_with_latest_version ON TRUE
    ) AS latest_resource
);
```

```js
| id  | name       | lifecycle | version | team  |
|-----|------------|-----------|---------|-------|
| 6   | resource_1 | DEV       | 3       | alpha |
| 9   | resource_1 | STG       | 3       | alpha |
| 12  | resource_1 | PRD       | 3       | alpha |

| 14  | resource_2 | DEV       | 2       | alpha |
| 16  | resource_2 | STG       | 2       | alpha |
| 18  | resource_2 | PRD       | 2       | alpha |

| 19  | resource_3 | DEV       | 1       | beta  |
| 20  | resource_3 | STG       | 1       | beta  |
| 21  | resource_3 | PRD       | 1       | beta  |

| 25  | resource_4 | DEV       | 4       | gamma |
| 29  | resource_4 | STG       | 4       | gamma |
| 33  | resource_4 | PRD       | 4       | gamma |
```

# 분석

쿼리를 전체적으로 살펴보겠습니다.

<div class="content-ad"></div>

### 단계 1: (이름, 라이프사이클, 팀)으로 그룹화된 데이터 블록에서 최신 리소스 버전을 추출하기 위한 쿼리를 형성합니다.

```sql
SELECT *
FROM resource AS r2
WHERE r2.team = r1.team
  AND r2.lifecycle = r1.lifecycle
  AND r2.name = r1.name
ORDER BY r2."version" DESC
LIMIT 1
```

### 단계 2: Q1에 의해 일치하는 모든 행을 찾기 위해 테이블을 반복합니다.

```sql
SELECT resource_with_latest_version.id
FROM resource AS r1
INNER JOIN LATERAL (
    QUERY_1
) AS resource_with_latest_version ON TRUE
```

<div class="content-ad"></div>

**단계 3: 중복 제거**

중복 항목을 제거하는 distinct 키워드가 작업을 수행합니다.

```sql
SELECT DISTINCT latest_resource.id
FROM (
    Q2
) AS latest_resource
```

**단계 4: 테이블에서 선형 스캔하여 정확한 일치 항목 찾기**

<div class="content-ad"></div>

In step 3, we have identified the IDs of the rows that meet the criteria. With this information, we can now extract the details of these rows.

```sql
SELECT *
FROM resource
WHERE id IN (
    Q3
);
```

## References:

- [heap.io - PostgreSQL's Powerful New Join Type LATERAL](https://www.heap.io/blog/postgresqls-powerful-new-join-type-lateral)
- [PostgreSQL Docs - Table Expressions, LATERAL](https://www.postgresql.org/docs/current/queries-table-expressions.html#QUERIES-LATERAL)