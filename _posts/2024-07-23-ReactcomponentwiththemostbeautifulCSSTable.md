---
title: "가장 아름다운 CSS 테이블을 갖춘 React 컴포넌트 만들기 방법"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-07-23 21:12
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "React component with the most beautiful CSS Table"
link: "https://medium.com/javascript-quantum/react-component-with-the-most-beautiful-css-table-99fe7eca2c60"
isUpdated: true
---




<img src="https://miro.medium.com/v2/resize:fit:1400/1*8LgPQKrUmpSEvnocHcnVzA.gif" />

안녕하세요! 제 아이디어는 다음을 사용하여 가장 아름다운 표를 만드는 것이었습니다:

✅ 아름다운 폰트 (Lato).

✅ 전환, 효과 및 CSS 애니메이션.

<div class="content-ad"></div>

리액트를 데스크톱 (MacOS, Windows 또는 Linux) 네이티브 앱에 통합하는 방법을 배우려면 제 글을 읽어보세요:👇

## React Table CSS 핵심 개념 설명

변수 (:root 및 var()):
:root 선택기 내에서 정의된 CSS 변수는 색상 및 글꼴과 같은 재사용 가능한 값을 저장합니다. 이러한 변수를 사용하면 주제 관리를 간소화하고 var() 함수를 사용하여 쉽게 업데이트하고 참조할 수 있어 스타일시트 전체에서 일관성을 보장합니다.

전환 (Transitions):
전환 속성은 지정된 CSS 속성에 대한 부드러운 변경을 설정된 기간 동안 적용합니다. hover 효과에 사용되며 box-shadow 및 transform과 같은 속성을 애니메이션화하여 시각적으로 매력적이고 반응형 사용자 상호 작용 경험을 만듭니다.

<div class="content-ad"></div>

박스 그림자:
box-shadow 속성은 요소 주변에 그림자 효과를 추가합니다. 요소를 둥글게 만들어 깊이를 부여하여 테이블의 디자인을 향상시킵니다. 호버에 다른 그림자 강도를 적용하여 동적이고 매력적인 시각 효과를 만들어냅니다.

미디어 쿼리:
미디어 쿼리는 다양한 화면 크기에 디자인을 조절하여 반응성을 보장합니다. 패딩 및 폰트 크기와 같은 속성을 작은 화면에 맞게 조정함으로써 테이블의 가독성과 사용성을 향상시킵니다.

호버 효과:
호버 효과는 :hover 가상 클래스를 사용하여 마우스 포인터가 요소 위에 있을 때 요소의 스타일을 변경합니다. 배경색 변경 및 확대 축소를 포함한 이러한 효과는 상호작용적인 피드백을 제공하여 사용자 경험을 향상시킵니다.

## 파일명: css/ScientificClassificationTable.css

<div class="content-ad"></div>

```css
@import url('https://fonts.googleapis.com/css2?family=Lato:wght@300;400;700&display=swap');

:root {
    --primary-color: #171716;
    --secondary-color: #abffb5;
    --hover-color: #87e4a1;
    --font-color: #333;
    --background-color: #f5f5f5;
    --box-shadow-color: rgba(0, 0, 0, 0.1);
    --box-shadow-hover-color: rgba(0, 0, 0, 0.2);
}

.table-container {
    width: 100%;
    overflow-x: auto;
    margin: 0 auto;
    padding: 1rem;
    box-sizing: border-box;
    font-family: 'Lato', sans-serif;
    background-color: var(--background-color);
}

.classification-table {
    width: 100%;
    border-collapse: collapse;
    background-color: var(--background-color);
    color: var(--font-color);
    box-shadow: 0 4px 8px var(--box-shadow-color);
    border-radius: 8px;
    overflow: hidden;
    transition: box-shadow 0.3s ease, transform 0.3s ease;
    text-align: left;
}

.classification-table:hover {
    box-shadow: 0 8px 16px var(--box-shadow-hover-color);
    transform: translateY(5px);
}

.thead_specie {
    background-color: var(--primary-color);
    color: #fff;
    text-transform: uppercase;
    letter-spacing: 1px;
}

.thtaxon,
.thtaxon2 {
    padding: 12px;
    font-weight: 700;
    border-bottom: 2px solid var(--secondary-color);
}

.tbody tr:nth-child(even) {
    background-color: var(--secondary-color);
}

.tbody tr:hover {
    background-color: var(--hover-color);
    cursor: pointer;
    transform: scale(1.002);
    transition: background-color 0.3s ease, transform 0.3s ease;
}

.trtaxon td {
    border: 1px solid #ddd;
    padding: 12px;
    font-weight: 400;
    transition: background-color 0.3s ease, transform 0.3s ease;
    text-align: left;
}

.trtaxon td:hover {
    background-color: var(--hover-color);
}

.trtaxon td:first-child {
    font-weight: 700;
}

@media screen and (max-width: 600px) {
    .thtaxon,
    .thtaxon2 {
        padding: 10px;
        font-size: 14px;
    }
    .video{
        width: 100%;
    }
    .trtaxon td {
        padding: 10px;
        font-size: 14px;
    }
}
```

## React Component file: components/ScientificClassificationTable.tsx

```js
import React from 'react';
import '../css/ScientificClassificationTable.css';

interface ScientificClassificationProps {
    classification: {
        Domain: string;
        Kingdom: string;
        Phylum: string;
        Class: string;
        Order: string;
        Suborder: string;
        Superfamily: string;
        Family: string;
        Subfamily: string;
        Genus: string;
        Species: string;
    };
}

const ScientificClassificationTable: React.FC<ScientificClassificationProps> = ({ classification }) => {
    const order = [
        'Domain',
        'Kingdom',
        'Phylum',
        'Class',
        'Order',
        'Suborder',
        'Superfamily',
        'Family',
        'Subfamily',
        'Genus',
        'Species'
    ];

    return (
        <div className="table-container">
        <table className="classification-table">
            <thead className='thead_specie'>
                <tr>
                    <th className='thtaxon'>Rank</th>
                    <th className='thtaxon2'>Taxon</th>
                </tr>
            </thead>
            <tbody className='tbody'>
                {order.map(rank => (
                    <tr className='trtaxon' key={rank}>
                        <td>{rank}</td>
                        <td>{classification[rank as keyof typeof classification]}</td>
                    </tr>
                ))}
            </tbody>
        </table>
    </div>

    );
};

export default ScientificClassificationTable;
```

<div class="content-ad"></div>

기능 구성 요소:
ScientificClassificationTable은 화살표 함수를 사용하여 정의된 함수형 구성 요소입니다. 함수형 구성 요소는 클래스 구성 요소보다 간단하고 간결하여 상태 및 라이프사이클 관리를 위해 훅을 사용할 수 있습니다.

속성 분해:
이 구성 요소는 ScientificClassificationProps에서 classification 속성을 속성 분해합니다. 이 접근 방식은 속성의 속성에 쉽게 액세스할 수 있도록 하고 필요한 값을 직접 추출하여 코드의 가독성과 유지 관리성을 향상시킵니다.

데이터 매핑 및 렌더링:
계층 구조 순서 배열에는 순위의 계층 구조가 포함되어 있습니다. 이 구성 요소는 맵 함수를 사용하여이 배열을 반복하고 각 순위에 대해 테이블 행(`tr`)을 생성합니다. 이 동적 렌더링은 테이블이 제공된 데이터 구조를 반영하도록 보장합니다.

CSS 클래스 적용:
table-container, classification-table, thead_specie, thtaxon, thtaxon2, tbody 및 trtaxon과 같은 CSS 클래스가 HTML 요소에 적용됩니다. 이러한 클래스는 구성 요소를 외부 CSS 파일에 연결하여 스타일을 적용하고 시각적 표현을 향상시킵니다.

<div class="content-ad"></div>

"제가 만든 Learn to Earn Academy를 👉 www.ascendance.dev에서 확인해주세요"