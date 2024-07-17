---
title: "Postgres 실습  온도 상승 문제 해결 방법 쉽게 배우기"
description: ""
coverImage: "/assets/img/2024-07-12-PostgresPracticeRisingTemperatureEasy6_0.png"
date: 2024-07-12 22:40
ogImage: 
  url: /assets/img/2024-07-12-PostgresPracticeRisingTemperatureEasy6_0.png
tag: Tech
originalTitle: "Postgres Practice — Rising Temperature (Easy) #6"
link: "https://medium.com/@tomas-svojanovsky/postgres-practice-rising-temperature-6-b2155b6917d8"
---



![image](/assets/img/2024-07-12-PostgresPracticeRisingTemperatureEasy6_0.png)

## Tables

![image](/assets/img/2024-07-12-PostgresPracticeRisingTemperatureEasy6_1.png)

## The Problem


<div class="content-ad"></div>

## 예시

![예시 이미지](/assets/img/2024-07-12-PostgresPracticeRisingTemperatureEasy6_3.png)

## 해결책

<div class="content-ad"></div>

The trick lies in the self-join, which allows us to compare values within the same table.

Imagine that ww.recordDate represents today's date, and we want to find a date which is one day before. This means that ww.recordDate is today, and w.recordDate is yesterday. With this presumption, we expect that ww.temperature (today) is higher than w.temperature (yesterday).

```js
SELECT ww.id
FROM Weather w
JOIN Weather ww on w.recordDate = (ww.recordDate - 1)
WHERE ww.temperature > w.temperature;
```

Another way to solve this is by using the LAG window function. We will prepare all the necessary data in one result set. In the second step, we compare the temperatures and dates. It's a similar approach to a self-join but uses the LAG function.

<div class="content-ad"></div>

<img src="/assets/img/2024-07-12-PostgresPracticeRisingTemperatureEasy6_4.png" />

```js
WITH PreviousWeatherData AS
(
    SELECT 
        id,
        recordDate,
        temperature, 
        LAG(temperature, 1) OVER (ORDER BY recordDate) AS PreviousTemperature,
        LAG(recordDate, 1) OVER (ORDER BY recordDate) AS PreviousRecordDate
    FROM 
        Weather
)
SELECT 
    id 
FROM 
    PreviousWeatherData
WHERE 
    temperature > PreviousTemperature
AND 
    recordDate = PreviousRecordDate + 1;
```

## 데이터

만약 직접 시도해보고 싶다면 데이터를 생성하고 삽입하는 쿼리를 추가할게요.

<div class="content-ad"></div>


```js
CREATE TABLE Weather (
    id INT,
    recordDate DATE,
    temperature INT,
    PRIMARY KEY (id)
);

INSERT INTO Weather (id, recordDate, temperature) VALUES
(1, '2015-01-01', 10),
(2, '2015-01-02', 25),
(3, '2015-01-03', 20),
(4, '2015-01-04', 30);
```

Leetcode challenge — https://leetcode.com/problems/rising-temperature/description/
