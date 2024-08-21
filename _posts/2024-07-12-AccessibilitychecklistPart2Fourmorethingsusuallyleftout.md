---
title: "접근성 체크리스트 Part 2 종종 빠뜨리는 4가지 항목"
description: ""
coverImage: "/assets/img/2024-07-12-AccessibilitychecklistPart2Fourmorethingsusuallyleftout_0.png"
date: 2024-07-12 22:05
ogImage: 
  url: /assets/img/2024-07-12-AccessibilitychecklistPart2Fourmorethingsusuallyleftout_0.png
tag: Tech
originalTitle: "Accessibility checklist (Part 2): Four more things usually left out"
link: "https://medium.com/user-experience-design-1/accessibility-checklist-part-2-four-more-things-usually-left-out-57a10832381f"
isUpdated: true
---




## 접근성 체크리스트

![image](/assets/img/2024-07-12-AccessibilitychecklistPart2Fourmorethingsusuallyleftout_0.png)

이전 글에서는 보통 접근성 체크리스트에 포함되지 않는 다섯 가지를 나열했습니다. 동일한 글에서 '접근성 체크리스트'란 개념이 실제로 존재하지 않는다고 설명했습니다.

그것은 여전히 사실입니다 — 그리고 '접근성 체크리스트'라는 제목을 계속 사용하는 이유는 해당 글에서 설명했습니다.

<div class="content-ad"></div>

하지만, 간단히 말해서, 준수 체크리스트는 있지만 "접근성 체크리스트"는 잘못된 명칭입니다. 왜냐하면 접근성은 지속적인 프로세스이기 때문이죠. 진정한 접근성 체크리스트는 영원히 계속될 것입니다.

세탁물을 세탁하는 것처럼, 끊임없이 진행됩니다.

그래서, 대부분의 체크리스트가 소홀히 하는 네 가지 사항을 더 소개하겠습니다 (하지만 소홀히 하지 말아야 할 것들). 원래 다섯 가지에 대해 이야기할 예정이었지만, 이미 기사가 충분히 길었습니다.

## #1: 레이블의 텍스트와 접근 가능한 이름은 (거의) 동일해야 합니다

<div class="content-ad"></div>

다시 말해, 시각 장애가 없는 사용자가 볼 수 있는 제어용 레이블 텍스트와 그 레이블을 소비하는 보조 기술(AT)에 대한 텍스트는 거의 동일합니다.

![image](/assets/img/2024-07-12-AccessibilitychecklistPart2Fourmorethingsusuallyleftout_1.png)

## 관련 WCAG 기준

해당 성공 기준은 2.5.3: 명칭으로 레이블입니다.

<div class="content-ad"></div>

이 부분은 종종 누락되는 편입니다. 이에 대한 두 가지 이유가 있다고 생 생각해요:

- 자동 접근성 도구들이 이를 자동으로 감지하지 못하는 경우가 많아요
- 이 부분은 대개 개발자가 접근성에 대해 충분히 알지만 그게 오히려 해로울 때 위반되곤 해요

## 위반 예시

작성자 노트: 여기 예시 #1을 업데이트하여 요구 사항을 더 잘 반영하도록 수정했어요. Fritz Hamburgler 님의 지적에 감사드립니다.

<div class="content-ad"></div>

위반 예시 #1: 레이블의 접근 가능한 이름을 혼동시키는 레이블 텍스트의 단어를 섞음

```js
<label for="txtPhoneNumber01">전화번호 <span class="visually-hidden">Aaron Aardvark를 위한</span></label>
...
<label for="txtPhoneNumber02">전화번호 <span class="visually-hidden">Bill Bixby를 위한</span></label>
```

레이블의 보이는 텍스트는 "전화번호"입니다.

첫 번째 레이블의 접근 가능한 이름은 "Aaron Aardvark를 위한 전화번호"이고, 두 번째는 "Bill Bixby를 위한 전화번호"입니다.

<div class="content-ad"></div>

이 예에서는 Bootstrap의 visually-hidden 클래스를 사용했습니다. 익숙하지 않다면, 이 클래스는 요소를 시각적으로 숨기지만 보조 기술에서는 유지되도록 하는 클래스입니다. (Bootstrap을 추천하는 것은 아닙니다. 그냥 매우 흔한 UI 프레임워크입니다).

문제는 식별 정보(여기서는 이름)가 시각적으로 숨겨졌다는 것입니다. 이로 인해 음성 입력 보조 기술을 사용하는 사용자들 중 일부가 원하는 컨트롤을 선택할 수 없게 됩니다.

위반 예시 #2: 버튼의 aria-label이 버튼의 텍스트와 다른 경우

```js
<button id="btnSave" aria-label="신용카드 정보 업데이트">저장</button>
```

<div class="content-ad"></div>

버튼의 텍스트는 "저장"입니다.

접근 가능한 이름은 "신용카드 정보 업데이트"입니다.

이것은 스크린 리더 사용자에게 추가적인 컨텍스트를 제공하기 위해 노력하는 선량한 의도의 개발자들에 의해 종종 발생하지만, 다른 보조 기술을 사용하는 사용자들을 방치하고 있다는 것을 의미합니다.

## 이것이 왜 중요한가요?

<div class="content-ad"></div>

이 성공 기준은 주로 스크린 리더기를 사용하는 사람들이 아닌 사용자들에게 도움을 주기 때문에 종종 간과되곤 합니다. 특히 음성 입력 기술을 사용하는 사람들에게 큰 도움이 됩니다. 그러나 이 그룹의 사용자들이 종종 무시당하는 경우가 많습니다.

접근 가능한 이름이 시각적 텍스트와 다를 경우 사용자가 기술을 사용하여 컨트롤을 클릭하도록 지시하면 작동하지 않을 수 있습니다. 이는 보조 기술이 접근 가능한 이름을 읽고 사용자가 시각적 텍스트를 읽기 때문입니다.

두 번째 예시에서는 버튼이 "저장"이라는 텍스트와 aria-label 속성값이 "신용카드 정보 업데이트"인 경우를 사용했습니다. 음성 입력 사용자가 "저장 버튼을 클릭하라"라고 말할 경우 작동하지 않을 수 있습니다. 사용자는 "신용카드 정보 업데이트 버튼을 클릭하라"고 말해야 합니다. 그러나 이 텍스트는 보이지 않으며 사용자가 크게 제약을 받습니다.

접근 가능한 이름이 시각적 텍스트의 일부일 것이라는 규칙이 있지만, 더 나아가 두 텍스트가 동일하도록 하는 것이 좋습니다. 따라서 레이블의 시각적 텍스트와 레이블의 접근 가능한 이름이 동일한지 확인해야 합니다.

<div class="content-ad"></div>

## 수업

- 컨트롤의 접근 가능한 이름은 시각적으로 보이는 이름과 일치해야 합니다.
- 문제를 찾기 위해 자동 접근성 도구에만 의존해서는 안 됩니다.
- 건강한 사람을 위한 미니멀리즘과 장애인을 위한 활기찬 요소는 둘 다에게 해로울 수 있습니다. 모든 사람에게 동일한 콘텐츠와 맥락을 제공하려 하십시오.

# #2: 제목이 제목 같이 보이기

여기에 추가할 수 있는 내용: "제목 같이 보이는 것은 모두 제목입니다."

<div class="content-ad"></div>


![image](/assets/img/2024-07-12-AccessibilitychecklistPart2Fourmorethingsusuallyleftout_2.png)

## Relevant Success Criteria

- SC 1.3.1 Info and Relationships (Level A)
- SC 2.4.10 Section Headings (Level AAA)


<div class="content-ad"></div>

## 왜 이게 중요할까요?

목차를 사용하는 이유는 무엇인가요? 왜 그냥 첫 페이지부터 시작해서 원하는 페이지를 찾을 때까지 넘기지 않나요?

목차를 사용하는 이유는 필요한 정보를 상대적으로 빠르게 찾을 수 있는 방법을 제공하기 때문입니다. 웹 페이지도 모든 사용자에게 동일한 방식으로 작동해야 합니다.

웹 페이지를 탐색하는 데 화면 판독기에 의존하는 사용자라면, 내가 어느 부분에 초점을 맞춰야 하는지를 결정하기 위해 당연히 페이지 내용을 대략적으로 파악하고 싶어할 것입니다.

<div class="content-ad"></div>

대부분의 화면 낭독기에서 "H" 키를 누르면 화면 낭독기가 다음 제목을 읽어줍니다. 사용자가 관심 있는 제목을 듣고 그 섹션을 찾아볼 수 있습니다.

제목이 없는 경우 관련 콘텐츠를 찾기가 매우 어려울 수 있습니다.

목차나 챕터, 페이지 번호가 없는 책을 상상해보세요. 그것이 제목이 없는 웹 페이지와 같습니다.

죄송하지만, 제목으로 사용되는 모든 것이 프로그래밍적으로 제목으로 작동해야 하는 이유에 초점을 맞춘 내용은 아니에요.

<div class="content-ad"></div>

만약 섹션의 제목으로 작용하지만 마크업에서는 제목이 아닌 굵은 텍스트가 있다면, AT(보조 기술)에 의존하는 사람들에게 어렵게 만드는 것입니다. 그래서 보이고 섹션의 제목으로 작용하는 모든 것은 마크업에서도 제목이어야 합니다.

똑같이, 마크업에서 실제 제목인데 해당 섹션과 구분되지 않는 스타일로 지정된 제목이 있다고 해보죠. 이제 시각적으로 페이지를 소비하지만 시각적으로 약한 시각력을 가진 사용자들에게 어렵게 만들었습니다.

기본 `h1` 스타일이 조금 과한 것을 이해합니다. 정상적인 텍스트와 다르게 설정하고 다른 제목 수준과 구별되도록 스타일을 지정해도 좋습니다. 다른 기사에서 이에 대해 논의할 수도 있겠죠.

요약하면, 제목은 페이지의 콘텐츠를 구분하는 목적을 가지고 있습니다. 프로그램적으로나 시각적으로 모두 제목임을 확인하도록 해주세요.

<div class="content-ad"></div>

## 위반 사례

위반 예시 1: 제목처럼 보이지만 실제로는 아닙니다 (이 줄 자체와 비슷하게)

부가 정보: 만약 Medium에서 가능했다면, 이전 섹션을 두 부분으로 나누고 위의 "위반 예시 1" 줄을 제목으로 설정했을 것이지만, Medium은 (내가 아는 한) 3단계 제목을 삽입할 수 있는 기능이 없습니다.

다음 이미지를 보면 'h1' 뒤에 'h2'가 따르는 것처럼 보입니다:

<div class="content-ad"></div>


![이미지](/assets/img/2024-07-12-AccessibilitychecklistPart2Fourmorethingsusuallyleftout_3.png)

그러나 다음과 같이 마크업이 되어 있으면, 화면 판독 독자에 의존하는 사용자는 두 번째 제목이 실제로 제목임을 알 수 없을 것입니다.

```javascript
<head>
<style>
.heading {
font-size: 1.5em;
}
</style>
</head>
<body>
<h1>접근할 수 없는 제목 예시</h1>
<div class="heading">일부 무의미한 텍스트 (접근할 수 없는 제목)</div>
<p>
  Lorem ipsum dolor sit amet consectetur adipisicing elit. Necessitatibus voluptatibus eaque natus minima, quos nam aperiam a itaque. Assumenda cumque fugiat qui laborum magni. Animi rerum asperiores non atque tempora.
  Veniam quas totam, blanditiis incidunt, qui ut esse dicta ex est commodi laboriosam, quos cumque exercitationem obcaecati recusandae eum praesentium sunt alias a. In eum aliquam impedit earum autem iusto!
  Cum, nesciunt itaque reiciendis ratione quidem dolorum dolorem eius. Impedit quos voluptate modi fugit ipsam. Officiis nobis debitis natus sint. Nesciunt numquam magnam eaque repellat dolorum earum! Laborum, fuga pariatur!
  Aliquid nisi cum dolore qui, facilis placeat itaque dicta sint earum minima corrupti voluptas fuga officia explicabo atque quisquam sequi laboriosam nobis iure delectus nostrum nemo mollitia at tempora. Iure.
  Quis tempora libero, officia quos repudiandae doloremque repellat suscipit itaque laboriosam eius cupiditate! Harum excepturi facere eos aspernatur eum rerum animi quia cumque architecto dolor ea, omnis vitae laborum pariatur.
</p>
</body>
```

위반 예시 #2: 실제 제목으로 보이지 않는 제목


<div class="content-ad"></div>

<img src="/assets/img/2024-07-12-AccessibilitychecklistPart2Fourmorethingsusuallyleftout_4.png" />

이제 "Some Lorem text..." 줄을 살펴봐주세요. 아래의 마크업에서 볼 수 있듯이, 이것은 `h2`이지만 `p` 텍스트처럼 스타일이 적용되어 있습니다. 거의 독립적인 문장처럼 보입니다.

```js
<head>
<style>
.heading {
font-size: 1em;
}
</style>
</head>
<body>
<h1>접근할 수 없는 제목 예시</h1>
<div class="heading">Some Lorem text (접근할 수 없는 제목)</div>
<p>
  Lorem ipsum dolor sit amet consectetur adipisicing elit. Necessitatibus voluptatibus eaque natus minima, quos nam aperiam a itaque. Assumenda cumque fugiat qui laborum magni. Animi rerum asperiores non atque tempora.
  Veniam quas totam, blanditiis incidunt, qui ut esse dicta ex est commodi laboriosam, quos cumque exercitationem obcaecati recusandae eum praesentium sunt alias a. In eum aliquam impedit earum autem iusto!
  Cum, nesciunt itaque reiciendis ratione quidem dolorum dolorem eius. Impedit quos voluptate modi fugit ipsam. Officiis nobis debitis natus sint. Nesciunt numquam magnam eaque repellat dolorum earum! Laborum, fuga pariatur!
  Aliquid nisi cum dolore qui, facilis placeat itaque dicta sint earum minima corrupti voluptas fuga officia explicabo atque quisquam sequi laboriosam nobis iure delectus nostrum nemo mollitia at tempora. Iure.
  Quis tempora libero, officia quos repudiandae doloremque repellat suscipit itaque laboriosam eius cupiditate! Harum excepturi facere eos aspernatur eum rerum animi quia cumque architecto dolor ea, omnis vitae laborum pariatur.
</p>
</body>
```

실제 제목은 실제로 제목처럼 보여야 합니다. 이는 폰트가 더 크고 해당 콘텐츠와 적절한 여백이 있는 것을 의미합니다.

<div class="content-ad"></div>

## 수업

- 만약 제목이라면, 제목처럼 유지하세요.
- 만약 제목이 아니라면, 제목처럼 만들지 마세요.

# #3: 링크 텍스트는 이해하기 쉬워야 합니다

"여기를 클릭하세요"와 같은 텍스트로 링크를 사용해서는 안 됩니다. 링크 텍스트를 사용자에게 링크가 어디로 이동하는지 알려주도록 만드세요 — 가능하다면, 맥락을 벗어나서도요.

<div class="content-ad"></div>


![Image](/assets/img/2024-07-12-AccessibilitychecklistPart2Fourmorethingsusuallyleftout_5.png)

## Relevant WCAG Criteria

- Success Criterion 2.4.4 Link Purpose (In Context) (Level A)

- Success Criterion 2.4.9 Link Purpose (Link Only) (Level AAA)


<div class="content-ad"></div>

## 왜 이것이 중요한가요?

한 번도 가본 적이 없는 곳으로 운전하고 있다고 상상해보세요. 앞에 "여기서 나가세요"라고 적힌 표지판이 있습니다. 그것이 전부입니다. 그 출구가 어디로 이어지는지 말이나 유용한 정보를 전혀 제공하지 않는 것처럼 말이에요.

저처럼 새로운 장소로 운전할 때는 모든 단서가 필요한 것 같죠. 우리는 사용자를 여정으로 보냅니다. 어디에 있는지와 어디로 갈 수 있는지 알려주지 않고 맞추는 대신에 둘러보도록 합시다.

## 위반 사례들

<div class="content-ad"></div>

위반 예시 #1: 유명한 “Read more…” 링크

```js
<p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Necessitatibus 
voluptatibus eaque natus minima, quos nam aperiam a itaque. Assumenda cumque 
fugiat qui laborum magni. Animi rerum asperiores non atque tempora.</p>
[Read more...](https://lipsum.com)
```

“Read more…” 링크는 해당 텍스트 바로 아래에 있습니다. 그러나 프로그램적 연관성이 없습니다. 링크가 `p` 요소의 일부였으면 더 맞는 방식이었지만, 실제로는 그렇지 않습니다.

다음을 더 나은 방법으로 시도해보세요:

<div class="content-ad"></div>

```js
<p>로렘 입숨(Lorem Ipsum) 더미 텍스트는 사용자에게 어디로 링크가 이동하는지 정확히 알려주어 문단의 맥락을 보여줍니다.</p>

위반 사례 #2: "여기를 클릭하세요" 링크

<a href="https://stackoverflow.com">질문한 것에 대해 비난 받기</a>
```

<div class="content-ad"></div>

이제 위의 링크 텍스트를 본 사람이 나만 아는 사람이었다면 (찾아보지 않아도) 아마 스택 오버플로우로 링크될 것이라고 알게 될 거에요. 하지만 모든 사람들이 그것을 알고 있다고 기대할 수는 없죠.

이 링크는 충분한 정보를 제공하지 않아 "여기를 클릭하세요"와 같은 링크에 해당합니다.

위반 예시 #3: 잘못된 테이블의 링크

```js
| Team Roster | Color     |
|-------------|-----------|
| [Cardinals](https://redbirdrants.com/) | Red    |
| [Orioles](https://www.camdenchat.com/)  | Orange |
```

<div class="content-ad"></div>

표에 헤더가 없는 이유를 주목해주세요. 만약 화면 판독기가 링크에 도달하면 사용자에게 "카디널스, 링크"라는 메시지만 전달될 가능성이 높습니다. "카디널스"는 무엇을 의미할까요? 예상되는 테이블 헤더와의 연관성이 없습니다. 명확한 헤더가 포함된 적절한 테이블을 사용하면 사용자에게 관련 정보를 제공할 수 있습니다.

## 교훈

- 링크 텍스트를 명확하고 모호하지 않게 유지해주세요. 사용자가 링크를 클릭했을 때 정확히 어디로 이동할지 알 수 있어야 합니다.
- 가능하다면, 동일한 페이지에 서로 다른 목적지를 가진 동일한 텍스트의 다중 링크를 피하세요. 해결할 수 없는 경우에는 사용자가 추측하지 않아도 되도록 최대한 맥락을 제공해주세요.
- 귀하의 링크가 데이터 테이블에 있는 경우, 테이블이 올바른 마크업을 갖추고 필요에 따라 헤더를 사용해주세요.

# #4: 확대해도 콘텐츠가 손실되지 않습니다.

<div class="content-ad"></div>


![image](/assets/img/2024-07-12-AccessibilitychecklistPart2Fourmorethingsusuallyleftout_6.png)

## Relevant Success Criteria

- Success Criterion 1.4.4: Resize text (Level AA)
  
- Success Criterion 1.4.10: Reflow (Level AA)


<div class="content-ad"></div>

특정 예시에 대해서는 SC 1.4.4 뒤에 있는 내용에만 집중하겠습니다. 어떤 것들이 중요한지 알아보겠어요.

14px 글꼴을 모두 읽을 수 있는 사람은 없습니다. 그래서 웹 페이지를 200%로 확대하는 사용자도 모든 텍스트, 콘텐츠 및 기능에 여전히 액세스할 수 있어야 합니다.

확실하게 말하자면, 100% 확대에서 화면에 나타나는 모든 것이 200%에서도 화면에 그대로 나타나야 하는 것을 의미하는 것은 아닙니다. 사용자가 아래로 스크롤하여 볼 수 있다면 해당 정보에 액세스할 수 있어야 한다는 것입니다.

<div class="content-ad"></div>

페이지들은 충분히 반응성이 있어서 원활하게 확대되는 것에 대처할 수 있어야 해요. 이는 텍스트가 잘림 없이 유지되고 페이지에 기본적으로 있는 메뉴는 여전히 접근 가능하며, 기본적으로 제공되는 다른 콘텐츠도 여전히 이용 가능한 것을 의미해요.

## 위반 예시

이 예시들에 대해 마크업을 넣지 않을 거에요. 왜냐하면 이를 시도한 사람에게 접근성을 보장하지 못하는 예시를 보여주는 것은 어려울 수 있기 때문이에요. 그래서 간단한 예시로 설명할게요.

위반 예시 1: 확대 시 메뉴가 사라집니다.

<div class="content-ad"></div>

웹 페이지에서 상단에 탐색 메뉴가 있고 사용자가 200%로 확대하면 메뉴가 사라집니다. (햄버거 아이콘 및 기타 모습 없음)

위반 사례 #2: 확대 시 텍스트 크기가 커지지 않음

텍스트를 200%로 확대해도 글꼴 크기가 커지지 않습니다. 브라우저의 내장 확대 기능을 사용하면 이러한 일이 발생하지 않을 수도 있습니다. 현재의 브라우저는 페이지 전체와 그 안의 모든 것을 확대합니다.

예를 들어, Ctrl 키를 누른 채 마우스 휠을 자기 쪽으로 굴릴 경우 확대됩니다. 또는 Ctrl 키를 누른 채 + (플러스) 버튼을 누르거나 브라우저의 메뉴를 사용할 수도 있습니다.

<div class="content-ad"></div>

브라우저 설정에서 텍스트 확대 기능이 여전히 제공됩니다. 그러나 때로는 쉽게 찾기 어렵습니다. 예를 들어, Chrome에서는 다음과 같이 할 수 있어요:

- Chrome 메뉴를 엽니다 (일반적으로 창의 오른쪽 상단에 있는 세 개의 세로 점 — 툴팁으로 "Google Chrome 사용자 지정 및 제어"라고 표시됩니다).
- 메뉴에서 "설정" 항목을 선택합니다.
- "글꼴 사용자 정의"를 검색하고 선택합니다.
- "글꼴 크기"의 값을 확인합니다 (슬라이더 위로 마우스를 가져가야 할 수도 있어요) 그리고 그 값을 원래 값의 두 배로 설정합니다. 제 기본 값은 16이었으니, 32로 변경했어요.

이제 이 작업을 완료했는데 텍스트의 크기가 변경되지 않는다면, 문제가 발생한 걸지도 몰라요.

글꼴 크기는 px나 pt와 같은 절대 단위가 아니라 상대 단위인 em이나 rem으로 설정해야 해요. Hajime Yamasaki Vukelic씨가 이에 대해 매우 상세한 기사를 썼어요 — 한 번 읽어보는게 좋을 거예요.

<div class="content-ad"></div>

위배 예시 #3: 페이지의 텍스트가 확대할 때 겹칩니다.

페이지를 200% 확대하면 페이지의 텍스트가 겹칩니다. 이는 일반적으로 CSS에서 절대 위치 지정을 사용할 경우에만 발생합니다. 그러니 그렇게 하지 마세요.

W3C에는 그런 모습에 대한 예시가 있습니다.

## 교훈

<div class="content-ad"></div>

- 마크업을 유연하게 유지해주세요.
- 확대를 200%로 조정하고 텍스트, 내용 및 기능이 여전히 이용 가능한지 확인해주세요.
- 폰트 크기를 픽셀이 아닌 상대 단위로 설정해주세요.
- 절대 위치 지정을 피해주세요.

# 결론

이 글에 대해 눈에 띄지 않았을 수 있는 한 가지는 WCAG 성공 기준에 대해 많은 시간을 들이지 않았다는 것입니다. 해당 기준들을 간략히 소개하고 마치기로했기 때문입니다.

이것은 접근성보다는 규정 준수를 강조하는 대신 더 중요한 것은 접근성에 중점을 두고 싶기 때문입니다. WCAG가 제공하는 정보는 중요하지만, 접근성에 대한 완전한 표준을 의도한 것은 아니었습니다. WCAG의 "G"가 가르쳐주는 것처럼, 이들은 가이드라인입니다 - 접근성이 더 많은 내용으로 이어지는 길을 안내해주는 유용한 정보입니다.

<div class="content-ad"></div>

WCAG의 완전한 준수는 100% 접근성을 의미하지 않아요. 이전에 여러 번 언급한 적이 있듯이, 접근성은 지속적인 과정입니다 — 건강과 비슷한 면이 있죠. 누군가가 자신이 100% 건강하다고 주장하는 것은 터무니없을 것입니다.

올바르게 식사하고 운동을 하며 건강하지 않은 것들을 피하는 노력이 계속되어야 합니다. 이는 지속적인 여정이죠. 접근성에도 똑같이 적용되는 이야기입니다. 절대적으로 '목적지'에 도달하지는 않겠지만, 항상 더 가까워질 수는 있어요.

여정을 계속해 나가요!

# 링크

<div class="content-ad"></div>

## 기사에서 언급된 내용

- WCAG (현재)
- 부트스트랩의 visually-hidden 클래스
- W3C의 Resize Content 예시
- Desktop 화면은 없다고 하는 하지메 야마사키 부케리치의 말

## 내 기사 중 일부

- 접근성 체크리스트 (제1부): 일반적으로 제외되는 다섯 가지 항목
- 요구되는 항목으로 전달되어야 할 접근성에 대한 의사소통 및 규칙으로 따라야 할 것이 아님
- 접근성을 오해하고 있습니다 — 그것을 해결합시다