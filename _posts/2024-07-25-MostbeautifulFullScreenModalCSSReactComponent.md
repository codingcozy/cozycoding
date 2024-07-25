---
title: "가장 아름다운 전체 화면 모달 만들기 CSS  React 컴포넌트 방법"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-07-25 11:46
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Most beautiful Full Screen Modal CSS  React Component"
link: "https://medium.com/javascript-quantum/most-beautiful-full-screen-modal-css-react-component-e739349fea32"
---


좋아요, 저는 학습용 백과사전👉 Quantum Compass를 개발 중이에요. 제가 매일의 개발 일지와 진행 상황을 공유하고 있어요. 나의 코딩과 데이터를 매일 발전시키겠다는 약속을 하고 싶어요. #dailyChallenge

![이미지](https://miro.medium.com/v2/resize:fit:1400/1*4TPAqgb4_lHbEE3-fuyspQ.gif)

## 일일 도전 React + 글쓰기 및 디자인

그래서, 제 아이디어는 매일 생성하는 것이에요:

<div class="content-ad"></div>

✔️ 새로운 리액트 컴포넌트

✔️ 좋은 기사

✔️ 더 나은 3D, 더 나은 아트, 더 나은 애니메이션, 더 나은 3D 모델

저의 프로젝트에 집중하고 싶습니다. 예술과 과학적 사실에 중점을 두고, 과학 논문, 책, 그리고 우리 신뢰할 수 있는 과학적 소스를 참고하고 싶습니다.

<div class="content-ad"></div>

네, 제 아이디어는 제작한 코드를 공유하여 이야기를 강화하고 진행 상황에 대해 투명하게 전달하는 것입니다.

# 리액트 컴포넌트 사이버펑크 풀스크린 모달

```js
import React, { useState } from 'react';
import '../css/anatomy.css'; // CSS를 import하는 것을 잊지 마세요

interface Anatomy {
    anatomy: {
    title: string;
    image: string;
    src: string;
    author: string;
    type: string;
    license: string;
    license_url: string;
}

const AnatomyComponent: React.FC<Anatomy> = ({ anatomy }) => {  
    const [isModalOpen, setIsModalOpen] = useState(false);

    const openAnatomyModal = () => {
        setIsModalOpen(true);
    };

    const closeAnatomyModal = () => {
        setIsModalOpen(false);
    };

    return (
        <div>
            {anatomy && (
                <div>
                    <div
                        id="openAnatomyModal"
                        className="anatomyModal"
                        style={{ display: isModalOpen ? 'flex' : 'none' }}
                        onClick={closeAnatomyModal}
                    >
                        <div className="AnatomyModalImageDiv">
                            <img className="AnatomyModalImage" src={anatomy.image} alt="Anatomy" />
                        </div>
                        <div className="anatomyTitle">{anatomy.title}</div>
                        <a href={anatomy.license_url}>
                            <div className="anatomyAuthor">{anatomy.author}</div>
                        </a>
                        <div className="anatomyLicense">{anatomy.license}</div>
                    </div>

                    <div className="anatomy" onClick={openAnatomyModal}>
                        <img className='image_animal' src={anatomy.image} alt="Anatomy Thumbnail" />
                    </div>
                </div>
            )}
        </div>
    );
};

export default AnatomyComponent;
```

## 리액트 코드 설명:

<div class="content-ad"></div>

```js
// React와 useState 훅을 가져와서 상태를 관리하는 데 사용합니다.
// 스타일링을 위해 CSS 파일을 가져옵니다.

import React, { useState } from 'react';
import '../css/anatomy.css'; 

// Firebase에서 Timestamp를 가져옵니다(코드에서는 사용되지 않음)
import { Timestamp } from 'firebase/firestore'; 

## 2. 해부학 인터페이스 정의

// 해부학 인터페이스를 정의합니다.
interface Anatomy {
    anatomy: { 
        title: string; 
        image: string;
        src: string; 
        author: string; 
        type: string; 
        license: string; 
        license_url: string;
    }
}
```

<div class="content-ad"></div>

이 부분은 우리 컴포넌트가 프롭으로 받을 해부 객체(anatomy)의 구조를 정의합니다. 이는 해부가 제목, 이미지, 작성자 등과 같은 여러 문자열 속성을 포함하도록 보장합니다.

## 3. AnatomyComponent 만들기

```js
const AnatomyComponent: React.FC<Anatomy> = ({ anatomy }) => {
  const [isModalOpen, setIsModalOpen] = useState(false);

  const openAnatomyModal = () => {
    setIsModalOpen(true);
  };

  const closeAnatomyModal = () => {
    setIsModalOpen(false);
  };

  return (
    <div>
      {anatomy && (
        <div>
          <div
            id="openAnatomyModal"
            className="anatomyModal"
            style={{ display: isModalOpen ? 'flex' : 'none' }}
            onClick={closeAnatomyModal}
          >
            <div className="AnatomyModalImageDiv">
              <img className="AnatomyModalImage" src={anatomy.image} alt="Anatomy" />
            </div>
            <div className="anatomyTitle">{anatomy.title}</div>
            <a href={anatomy.license_url}>
              <div className="anatomyAuthor">{anatomy.author}</div>
            </a>
            <div className="anatomyLicense">{anatomy.license}</div>
          </div>

          <div className="anatomy" onClick={openAnatomyModal}>
            <img className="image_animal" src={anatomy.image} alt="Anatomy Thumbnail" />
          </div>
        </div>
      )}
    </div>
  );
};
```

컴포넌트 정의:

<div class="content-ad"></div>

```javascript
const AnatomyComponent: React.FC<Anatomy> = ({ anatomy }) => {
```

- 우리는 이전에 정의한 구조와 일치하는 anatomy prop을 받는 AnatomyComponent라는 함수형 React 컴포넌트를 생성합니다.

Modal 상태 관리:

```javascript
const [isModalOpen, setIsModalOpen] = useState(false);
```

<div class="content-ad"></div>

- isModalOpen이라는 상태 조각을 만들기 위해 useState를 사용합니다. 이 상태는 모달(팝업)이 표시되는지 여부를 제어하는 데 도움이 됩니다. 초기에는 false(숨겨짐)로 설정됩니다.

모달을 열고 닫는 함수:

```js
const openAnatomyModal = () => {
  setIsModalOpen(true);
};

const closeAnatomyModal = () => {
  setIsModalOpen(false);
};
```

이 두 함수는 간단히 isModalOpen 상태를 true 또는 false로 변경하여 모달을 보이거나 숨길 수 있습니다.

<div class="content-ad"></div>

컴포넌트 렌더링:

```js
return (
  <div>
    {anatomy && (
      <div>
        <div
          id="openAnatomyModal"
          className="anatomyModal"
          style={{ display: isModalOpen ? 'flex' : 'none' }}
          onClick={closeAnatomyModal}
        >
          <div className="AnatomyModalImageDiv">
            <img className="AnatomyModalImage" src={anatomy.image} alt="Anatomy" />
          </div>
          <div className="anatomyTitle">{anatomy.title}</div>
          <a href={anatomy.license_url}>
            <div className="anatomyAuthor">{anatomy.author}</div>
          </a>
          <div className="anatomyLicense">{anatomy.license}</div>
        </div>

        <div className="anatomy" onClick={openAnatomyModal}>
          <img className='image_animal' src={anatomy.image} alt="Anatomy Thumbnail" />
        </div>
      </div>
    )}
  </div>
);
```

조건부 렌더링:

```js
{anatomy && (
  <div>
```

<div class="content-ad"></div>

- 이렇게 하면 해부 속성이 제공된 경우에만 구성 요소의 나머지 부분이 렌더링됩니다.

모달 구조:

```js
<div
  id="openAnatomyModal"
  className="anatomyModal"
  style={{ display: isModalOpen ? 'flex' : 'none' }}
  onClick={closeAnatomyModal}
>
```

- 이 div는 모달을 나타냅니다. 인라인 스타일을 사용하여 isModalOpen에 기반하여 디스플레이 속성을 제어합니다. isModalOpen이 true이면 모달이 표시됩니다 (display: flex); 그렇지 않으면 숨겨집니다 (display: none). 모달을 클릭하면 closeAnatomyModal이 트리거됩니다.
- 모달 내부에는 이미지, 제목, 작성자 (링크가 포함된), 라이선스 정보를 표시합니다.
- 썸네일:

<div class="content-ad"></div>

```js
AnatomyComponent를 내보내기
```  

<div class="content-ad"></div>

마침내, AnatomyComponent를 내보내어 다른 애플리케이션 부분에서 가져와 사용할 수 있습니다.

아래에서 아름다운 CSS 테이블을 만드는 방법을 확인할 수 있어요 👇

# CSS 스타일시트 전체 코드

```js
/* 사이버펑크 테마 색상 */
:root {
    --primary-color: #0ff;
    --secondary-color: #f0f;
    --background-color: #111;
    --text-color: #fff;
    --modal-background-color: rgba(0, 0, 0, 0.9);
    --border-glow: 0 0 10px var(--primary-color);
  }

  
  .anatomyModal {
    display: none;
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: var(--modal-background-color);
    z-index: 1000;
    justify-content: center;
    align-items: center;
    animation: fadeIn 0.5s ease-in-out;
  }
  
  .AnatomyModalImageDiv {
    text-align: center;
  }
  
  .AnatomyModalImage {
    max-width: 90%;
    max-height: 80%;
    border: 2px solid var(--primary-color);
    box-shadow: var(--border-glow);
    animation: blinkBorder 1.5s infinite;
  }
  
  .anatomyTitle, .anatomyAuthor, .anatomyLicense {
    margin: 10px 0;
    text-align: center;
    color: var(--secondary-color);
    animation: textGlow 1.5s ease-in-out infinite alternate;
  }
  
  .anatomy {
    cursor: pointer;
    transition: transform 0.3s ease;
  }
  
  .anatomy img {
    border: 2px solid var(--primary-color);
    box-shadow: var(--border-glow);
  }
  
  .anatomy:hover {
    transform: scale(1.05);
  }
  
  @keyframes fadeIn {
    from {
      opacity: 0;
    }
    to {
      opacity: 1;
    }
  }
  
  @keyframes blinkBorder {
    0%, 100% {
      box-shadow: 0 0 5px var(--primary-color);
    }
    50% {
      box-shadow: 0 0 20px var(--secondary-color);
    }
  }
  
  @keyframes textGlow {
    0% {
      text-shadow: 0 0 5px var(--secondary-color);
    }
    100% {
      text-shadow: 0 0 20px var(--primary-color);
    }
  }
```

<div class="content-ad"></div>

# 1. 변수 및 테마 설정 :root

```js
:root {
  --primary-color: #0ff;
  --secondary-color: #f0f;
  --background-color: #111;
  --text-color: #fff;
  --modal-background-color: rgba(0, 0, 0, 0.9);
  --border-glow: 0 0 10px var(--primary-color);
}
```

개념: 변수

- 하는 일: CSS 변수를 사용하여 사용자 정의 속성(변수)을 정의합니다.
- 유용성: 변수를 사용하면 CSS 전체에서 색상 및 기타 값을 쉽게 관리하고 재사용할 수 있습니다. 주요 색상을 변경하고 싶을 때 한 곳에서 업데이트만 하면 됩니다.

<div class="content-ad"></div>

## 2. 스타일링과 레이아웃

```js
.anatomyModal {
  display: none;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: var(--modal-background-color);
  z-index: 1000;
  justify-content: center;
  align-items: center;
  animation: fadeIn 0.5s ease-in-out;
}
```

컨셉: 레이아웃 및 위치 지정

- 작동 방식: 기본적으로 모달을 숨깁니다 (display: none) 그리고 화면 전체를 덮는 고정 위치를 왼쪽 상단에 설정합니다 (width: 100%; height: 100%). 배경 색상 변수를 사용하고 flexbox 속성을 이용하여 콘텐츠를 가운데 정렬합니다.
- 유용성: 고정 위치 지정을 사용하면 페이지 스크롤 시에도 모달이 제자리에 유지됩니다. 콘텐츠를 가운데 정렬하는 것은 모달을 멋지게 표현하는 데 중요합니다.

<div class="content-ad"></div>

## 3. 애니메이션 및 전환

```js
@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

@keyframes blinkBorder {
  0%, 100% {
    box-shadow: 0 0 5px var(--primary-color);
  }
  50% {
    box-shadow: 0 0 20px var(--secondary-color);
  }
}

@keyframes textGlow {
  0% {
    text-shadow: 0 0 5px var(--secondary-color);
  }
  100% {
    text-shadow: 0 0 20px var(--primary-color);
  }
}
```

개념: 애니메이션

- 기능: 이 키프레임은 다양한 요소에 대한 애니메이션을 정의합니다.
- fadeIn: 요소가 나타나는 opacity를 점진적으로 0에서 1로 변경합니다.
- blinkBorder: 요소의 box-shadow를 번갈아 변경하여 깜박이는 효과를 만듭니다.
- textGlow: 텍스트 그림자를 변경하여 텍스트를 반짝이게 만듭니다.

<div class="content-ad"></div>

```js
.anatomy {
  cursor: pointer;
  transition: transform 0.3s ease;
}

.anatomy:hover {
  transform: scale(1.05);
}
```

개념: 트랜지션

- 기능: anatomy 클래스에 대한 부드러운 트랜지션을 설정합니다. 해당 요소를 약간 확대합니다 (transform: scale(1.05)) 0.3초 동안 ease 트랜지션으로.
- 유용성: 트랜지션과 애니메이션은 UI를 더 동적이고 매력적으로 만들어 사용자 상호작용에 시각적 피드백을 추가합니다.

이 세 가지 주요 개념을 이해함으로써 — 테마화를 위한 변수, 구조화를 위한 레이아웃 및 위치 지정, 동적 효과를 추가하는 애니메이션 및 트랜지션 — 아름다운 반응형 디자인을 만들 수 있습니다.

<div class="content-ad"></div>

X 몬 스미 트하면 돼요.