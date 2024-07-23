---
title: "월간 가계 예산을 위한 스타일리시한 HTML 테이블 쉽게 만드는 방법"
description: ""
coverImage: "/assets/img/2024-07-23-CreatingaStylishHTMLTableforYourMonthlyHomeBudget_0.png"
date: 2024-07-23 11:24
ogImage: 
  url: /assets/img/2024-07-23-CreatingaStylishHTMLTableforYourMonthlyHomeBudget_0.png
tag: Tech
originalTitle: "Creating a Stylish HTML Table for Your Monthly Home Budget"
link: "https://medium.com/@andythemyth/creating-a-stylish-html-table-for-your-monthly-home-budget-6acda4d6318d"
---


매달 집 계산을 관리하는 것은 재정 건강을 유지하는 데 중요합니다. 예산을 조직화하고 시각화하는 효과적인 방법 중 하나는 CSS로 스타일링된 HTML 테이블을 사용하는 것입니다. 이 문서에서는 세련되고 기능적인 HTML 테이블을 만들어 매월 지출 내역을 추적하는 방법을 안내합니다.

![Creating a Stylish HTML Table for Your Monthly Home Budget](/assets/img/2024-07-23-CreatingaStylishHTMLTableforYourMonthlyHomeBudget_0.png)

## HTML 구조

먼저, 우리의 HTML 테이블의 기본 구조를 만들어 봅시다. 이 테이블에는 다양한 지출 항목에 대한 카테고리와 계획된 금액, 실제 금액 및 차액을 나타내는 열이 포함될 것입니다.

<div class="content-ad"></div>

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>월간 가계 예산</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <h1>월간 가계 예산</h1>
    <table class="budget-table">
        <thead>
            <tr>
                <th>카테고리</th>
                <th>계획액</th>
                <th>실제액</th>
                <th>차액</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>임대료/모기지</td>
                <td>$1,200</td>
                <td>$1,200</td>
                <td>$0</td>
            </tr>
            <tr>
                <td>유틸리티</td>
                <td>$150</td>
                <td>$140</td>
                <td>-$10</td>
            </tr>
            <tr>
                <td>식료품</td>
                <td>$400</td>
                <td>$420</td>
                <td>+$20</td>
            </tr>
            <tr>
                <td>교통비</td>
                <td>$100</td>
                <td>$80</td>
                <td>-$20</td>
            </tr>
            <tr>
                <td>엔터테인먼트</td>
                <td>$200</td>
                <td>$220</td>
                <td>+$20</td>
            </tr>
            <tr>
                <td>기타</td>
                <td>$100</td>
                <td>$90</td>
                <td>-$10</td>
            </tr>
        </tbody>
    </table>
</body>
</html>
```

## CSS 스타일링

이제 CSS를 추가하여 테이블을 시각적으로 더 매력적으로 표현하겠습니다. CSS는 테이블의 모양, 색상, 테두리, 패딩 및 호버 효과를 처리할 것입니다.

```js
body {
    font-family: Arial, sans-serif;
    background-color: #f4f4f4;
    color: #333;
    margin: 0;
    padding: 20px;
}
h1 {
    text-align: center;
    color: #4CAF50;
}
.budget-table {
    width: 80%;
    margin: 0 auto;
    border-collapse: collapse;
    box-shadow: 0 5px 15px rgba(0,0,0,0.3);
}
.budget-table thead tr {
    background-color: #4CAF50;
    color: #fff;
    text-align: left;
}
.budget-table th, .budget-table td {
    padding: 12px 15px;
    border: 1px solid #ddd;
}
.budget-table tbody tr:nth-of-type(even) {
    background-color: #f3f3f3;
}
.budget-table tbody tr:hover {
    background-color: #f1f1f1;
}
.budget-table td:nth-of-type(4) {
    font-weight: bold;
}
.budget-table td:nth-of-type(4):before {
    content: "$";
}
```

<div class="content-ad"></div>

## 설명

- HTML 구조:

- `table` 요소는 표를 정의합니다.
- `thead`와 `tbody`는 표의 헤더와 본문을 구분하여 구조와 스타일링을 더 잘할 수 있게 합니다.
- `th`와 `td`는 각각 표 헤더와 데이터 셀에 사용됩니다.

- CSS 스타일링:

<div class="content-ad"></div>

- 본문 스타일링: 본문은 간단하고 깔끔한 글꼴과 배경색을 사용하여 스타일링되었습니다.
- 테이블 스타일링: .budget-table 클래스를 사용하여 테이블을 스타일링합니다. 너비, 여백, 테두리 병합 및 박스 그림자 설정됩니다.
- 헤더 스타일링: thead는 녹색 배경과 흰색 텍스트로 스타일링되어 헤더를 더욱 돋보이도록 만들어줍니다.
- 셀 여백 및 테두리: 셀에 여백과 테두리가 추가되어 가독성을 높입니다.
- 행 스트라이핑 및 호버 효과: 짝수 행에는 연한 배경색이 지정되고 상호작용을 위해 호버 효과가 추가되었습니다.
- 차이 열: 차이 열은 명확성을 위해 굵은 텍스트와 달러 기호 접두사로 강조되었습니다.

# 결론

이 가이드를 따라 매월 가계예산을 관리할 수 있는 시각적으로 매력적이고 기능적인 HTML 테이블을 만들 수 있습니다. HTML과 CSS의 조합을 통해 테이블이 구조적이면서도 세련되어 지출을 추적하고 분석하기가 더 쉬워집니다. 스타일을 사용자 맞게 조정하여 선호도와 요구 사항에 더 잘 맞도록 수정해보세요. 행복한 예산 관리되세요!