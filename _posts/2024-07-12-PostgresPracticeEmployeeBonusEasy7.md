---
title: "Postgres 실습  직원 보너스 문제 해결하기 쉬운 단계 7"
description: ""
coverImage: "/assets/img/2024-07-12-PostgresPracticeEmployeeBonusEasy7_0.png"
date: 2024-07-12 22:18
ogImage: 
  url: /assets/img/2024-07-12-PostgresPracticeEmployeeBonusEasy7_0.png
tag: Tech
originalTitle: "Postgres Practice — Employee Bonus (Easy) #7"
link: "https://medium.com/@tomas-svojanovsky/postgres-practice-employee-bonus-easy-7-8e562755eab5"
---



![Image](/assets/img/2024-07-12-PostgresPracticeEmployeeBonusEasy7_0.png)

## Tables

![Image](/assets/img/2024-07-12-PostgresPracticeEmployeeBonusEasy7_1.png)

## The Problem


<div class="content-ad"></div>


![이미지](/assets/img/2024-07-12-PostgresPracticeEmployeeBonusEasy7_2.png)

## 예시

![이미지](/assets/img/2024-07-12-PostgresPracticeEmployeeBonusEasy7_3.png)

## 해결책


<div class="content-ad"></div>

우리는 1000 미만의 급여는 1000보다 작지만 보너스가 없음을 의미한다는 점을 기억해야 합니다. 이러한 숫자와 NULL 값을 주의 깊게 살펴볼 것입니다.

만약 NULL 값을 포함하려면 LEFT JOIN을 수행하고 보너스가 1000보다 작거나 보너스가 NULL인지 확인하는 조건을 추가해야 합니다.

```js
SELECT e.name, b.bonus
FROM Employee e
LEFT JOIN Bonus b ON e.empId = b.empId
WHERE b.bonus < 1000 OR b.bonus IS NULL;
```

## 데이터

<div class="content-ad"></div>

해볼 수 있도록 자신이 시도하고 싶으면 테이블을 만들고 데이터를 삽입하는 쿼리를 추가하겠습니다.

```js
DROP TABLE IF EXISTS employee CASCADE;

CREATE TABLE Employee (
    empId INT PRIMARY KEY,
    name VARCHAR(255),
    supervisor INT,
    salary INT
);

INSERT INTO Employee (empId, name, supervisor, salary) VALUES
(3, 'Brad', NULL, 4000),
(1, 'John', 3, 1000),
(2, 'Dan', 3, 2000),
(4, 'Thomas', 3, 4000);

DROP TABLE IF EXISTS Bonus CASCADE;

CREATE TABLE Bonus (
    empId INT PRIMARY KEY,
    bonus INT,
    FOREIGN KEY (empId) REFERENCES Employee(empId)
);

INSERT INTO Bonus (empId, bonus) VALUES
(2, 500),
(4, 2000);
```

Leetcode 챌린지 — https://leetcode.com/problems/employee-bonus/description/