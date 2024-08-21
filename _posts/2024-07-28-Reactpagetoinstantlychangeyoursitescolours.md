---
title: "React로 사이트 색상을 즉시 변경하는 방법"
description: ""
coverImage: "/assets/img/2024-07-28-Reactpagetoinstantlychangeyoursitescolours_0.png"
date: 2024-07-28 13:43
ogImage: 
  url: /assets/img/2024-07-28-Reactpagetoinstantlychangeyoursitescolours_0.png
tag: Tech
originalTitle: "React page to instantly change your sites colours"
link: "https://dev.to/elsyng/internal-page-to-instantly-change-your-sites-colours-p2a"
isUpdated: true
---




## 목표

저는 디자이너가 몇 가지 슬라이더를 사용하여 사이트 색상을 변경하고 즉시 전체 웹사이트와 모든 구성 요소가 어떻게 보일지 확인할 수 있는 내부 페이지를 만드는 것이 얼마나 간단하고 효과적일지 궁금했습니다.

- 내부: 페이지를 비밀로 유지할 필요는 없지만 사용자에게 어떤 메뉴나 링크를 통해 알릴 필요는 없습니다.
- 효과적: 디자이너에게 노력 없이 즉각적이고 좋은 시각을 제공해야 합니다.

## 사용된 기술

<div class="content-ad"></div>

- React, typescript, styled-components,
- 그리고 당연히: html, css 및 javascript.

## OKLCH 색상 공간

여러 가지 CSS 색상 공간을 비교한 후, 이 기능에 이상적인 OKLCH를 선택했습니다. 왜냐하면:

- OKLCH는 색상 공간(또는 색상 함수)으로, 한 번에 밝기 "L"와 색상 강도 "C"(또는 크로마)에 대해 고정된 값을 선택한 경우, 객체의 색조 "H"를 자유롭게 변경할 수 있고, 그 색은 이러한 일정한 L과 C와 잘 어울리게 유지됩니다. 이게 바로 우리의 사용 사례에 딱 맞습니다.
- 따라서 각 색에 대해 우리는 먼저 우리가 좋아하는 고정된 L과 C 값을 선택한 다음, 슬라이더를 사용하여 H 값을 조절합니다.

<div class="content-ad"></div>

OKLCH에 대한 더 많은 정보:

- 웹 문서,
- 다음 사진이 포함된 블로그:

![Reactpagetoinstantlychangeyoursitescolours_0.png](/assets/img/2024-07-28-Reactpagetoinstantlychangeyoursitescolours_0.png)

### OKLCH 매개변수

<div class="content-ad"></div>

- 밝기 L: 검은색은 0%, 흰색은 100%입니다.
- 색상 강도 (또는 색도) C: 회색의 경우 0, 최대 값이 없으며, 일반적으로 0.1 또는 0.5 이하입니다.
- 색조 (색 자체) H: 0부터 360까지 (360 = 0).
- (투명도를 제공하는 네 번째 매개변수 알파는 우리가 여기서 사용하지 않을 것입니다.)

예시:

```js
color: oklch(70% 0.3 150) /* 밝은 초록색 */
```

## 사이트를 위한 전역 index.css 파일

<div class="content-ad"></div>

```js
:root {
  /* OKLCH 색조 값: */
  --color1: 144; /* 녹색 */
  --color2: 90; /* 갈색 */

  --color-primary: oklch(65% 0.1 var(--color1));
  --color-primary-lighter: oklch(70% 0.1 var(--color1));
  --color-disabled: oklch(70% 0.05 var(--color1));

  --color-border: oklch(76% 0.1 var(--color2));
  --color-border-darker: oklch(64% 0.17 var(--color2));

  --color-border-gray: #aaa;
  --color-border-light: #ccc;

  --color-whitish: #f0f0f0;
  --color-blackish: #444444;
}

button {
  background-color: var(--color-primary);
  border: 1px solid var(--color-primary);
}

th, td {
  border: 1px solid var(--color-border-light);
}
```

## 디자인 페이지 - 색상 탭

<img src="/assets/img/2024-07-28-Reactpagetoinstantlychangeyoursitescolours_1.png" />


<div class="content-ad"></div>

이 탭의 섹션:

- 색깔: 각 색깔을 나타내는 네 개의 상자 (흰색/검정색 fg/bg로).
- 색조 선택: 각각 주요 색조에 대한 두 개의 슬라이더.
- 샘플 구성요소: 이 페이지에서 새로운 색깔을 사용하여 확인할 수 있는 일부 구성요소.

색상을 변경하면 웹사이트 전체를 둘러보며 새로운 디자인을 즉시 확인할 수 있습니다.

새 값으로 만족할 때, index.css에서 색조 값을 업데이트할 수 있습니다:

<div class="content-ad"></div>

```js
  /* OKLCH HUE VALUES: */
  --color1: 144; /* green */
  --color2: 90; /* brown */
```

## ColoursTab 컴포넌트

```js
import styled from "styled-components";

import {
  ColourShift,
  ComponentCardButton,
  ComponentCardPaging,
  PageTabContent,
  SectionTitle,
} from "../components";

const Section = styled.div`
  display: flex;
  flex-direction: column;
  gap: 10px;
`;

// ------------------------------------
type BoxProps = {
  c?: string;
  fg: string;
  bg: string;
};

const Box = ({ c, fg, bg }: BoxProps) => {
  return (
    <p
      style={{
        width: "130px",
        padding: "6px 6px 9px",
        backgroundColor: `var(--color-${bg})`,
        color: `var(--color-${fg})`,
        overflowWrap: "anywhere",
        borderRadius: "4px",
      }}
    >
      {c}
    </p>
  );
};

// ------------------------------------
type ColourProps = {
  colour: string;
};

const Colour = ({ colour }: ColourProps) => {
  return (
    <div
      style={{
        display: "flex",
        gap: "10px",
        justifyContent: "space-between",
        //
      }}
    >
      <Box c={colour} fg="whitish" bg={colour} />
      <Box c={colour} fg="blackish" bg={colour} />
      <Box c={colour} fg={colour} bg="whitish" />
      <Box c={colour} fg={colour} bg="blackish" />
    </div>
  );
};

// ------------------------------------
const ColoursTab = () => {
  return (
    <PageTabContent>
      <Section>
        <SectionTitle>Colours</SectionTitle>
        <p>
          <b>global variable: color1</b>
        </p>
        <Colour colour="primary" />
        <Colour colour="primary-lighter" />
        <Colour colour="disabled" />
        <br />

        <p>
          <b>global variable: color2</b>
        </p>
        <Colour colour="border" />
        <Colour colour="border-darker" />
        <br />

        <p>
          <b>misc</b>
        </p>
        <Colour colour="border-gray" />
        <Colour colour="border-light" />
        <Colour colour="blackish" />
        <Colour colour="whitish" />
      </Section>

      <Section>
        <SectionTitle>Hue selection (oklch, 0..359)</SectionTitle>
        <ColourShift name="color1" />
        <ColourShift name="color2" />
      </Section>

      <Section>
        <SectionTitle>Sample components</SectionTitle>
        <ComponentCardButton isRow />
        <ComponentCardPaging />
      </Section>
    </PageTabContent>
  );
};

export default ColoursTab;
```

## ColourShift 컴포넌트

<div class="content-ad"></div>

<img src="/assets/img/2024-07-28-Reactpagetoinstantlychangeyoursitescolours_2.png" />

아래는 코드입니다:

```js
import { useEffect, useState } from "react";
import styled from "styled-components";

import { useCssGlobalVariables } from "../hooks";
import GradientBar from "./GradientBar";

const Container = styled.div`
  border: 1px solid var(--color-border-gray);
  border-radius: 8px;
  box-shadow: var(--box-shadow);
`;

const InputContainer = styled.div`
  padding: 1px 10px;
`;

type InputProps = {
  $colour: string;
};

const Input = styled.input<InputProps>`
  width: 100%;
  accent-color: ${({ $colour }) => $colour};
`;

// ------------------------------------
type Props = {
  name: string;
};

const ColourShift = ({ name: name }: Props) => {
  const {
    getCssGlobalVariable,
    setCssGlobalVariable,
  } = //
    useCssGlobalVariables(`--${name}`);

  const [hue, setHue] = useState("");

  // ---------------------------------
  useEffect(() => {
    const h = getCssGlobalVariable();
    setHue(h);
    // eslint-disable-next-line react-hooks/exhaustive-deps
  }, []);

  const onChange = (h: string) => {
    setCssGlobalVariable(h);
    setHue(h);
  };

  // ------------------------------------
  return (
    <Container data-testid="ColourShift">
      <GradientBar title={name} hue={hue} />

      <InputContainer>
        <Input
          type="range"
          $colour={`oklch(70% 0.1 ${hue})`}
          min="0"
          max="359"
          value={hue}
          onChange={(e) => onChange(e.target.value)}
        />
      </InputContainer>
    </Container>
  );
};

export default ColourShift;
```

## GradientBar 컴포넌트

<div class="content-ad"></div>

ColourShift의 하위 구성요소입니다. ColourShift의 상단 반부에 색상 스케일을 보여줍니다.

그라데이션은 제대로 작동하려면 0과 359 사이에 추가적인 중간 값이 필요합니다.

```js
import styled from "styled-components";

const Container = styled.div`
  background: linear-gradient(
    to right,
    oklch(65% 0.1 0),
    oklch(65% 0.1 179),
    oklch(65% 0.1 359)
  );
  width: 100%;
  padding: 3px 10px 6px;
  display: flex;
  justify-content: space-between;
  border-top-left-radius: 6px;
  border-top-right-radius: 6px;

  * {
    color: var(--color-whitish);
  }
`;

const TitleAndHue = styled.div`
  display: flex;
  gap: 6px;
`;

const Hue = styled.p`
  width: 30px;
`;

// ------------------------------------
type Props = {
  title: string;
  hue: string;
};

const GradientBar = ({ title, hue }: Props) => {
  return (
    <Container data-testid="GradientBar">
      <p>0</p>

      <TitleAndHue>
        <p>{title}:</p>
        <Hue>{hue}</Hue>
      </TitleAndHue>

      <p>359</p>
    </Container>
  );
};

export default GradientBar;
```

## useCssGlobalVariables 커스텀 훅

<div class="content-ad"></div>

이 커스텀 후크는 CSS 전역 변수를 읽고 업데이트하는 데 필요한 JavaScript를 캡슐화합니다.

```js
const useCssGlobalVariables = (name: string) => {
  const getCssGlobalVariable = () => {
    const root = document.querySelector(":root");
    if (!root) return "";

    const style = getComputedStyle(root);
    return style.getPropertyValue(name);
  };

  // ------------------------------------
  const setCssGlobalVariable = (value: string) => {
    const root = document.querySelector(":root");
    if (!root) return null;

    (root as HTMLElement).style.setProperty(name, value);
  };

  // ------------------------------------
  return { getCssGlobalVariable, setCssGlobalVariable };
};

export default useCssGlobalVariables;
```

## 데모

디자인 페이지 ➤ 색상 탭