---
title: "WCAG 22 파싱  채택되지 않은 성공 기준 "
description: ""
coverImage: "/assets/img/2024-07-12-WCAG22ParsingtheSuccessCriterionthatdidntmakeit_0.png"
date: 2024-07-12 22:01
ogImage: 
  url: /assets/img/2024-07-12-WCAG22ParsingtheSuccessCriterionthatdidntmakeit_0.png
tag: Tech
originalTitle: "WCAG 2.2 Parsing — the Success Criterion that didn’t make it"
link: "https://medium.com/user-experience-design-1/wcag-2-2-parsing-the-success-criterion-that-didnt-make-it-ab8d4904328e"
isUpdated: true
---




## 성공 기준 4.1.1 구문 분석

![이미지](/assets/img/2024-07-12-WCAG22ParsingtheSuccessCriterionthatdidntmakeit_0.png)

나의 마지막 기사에서 W3C가 WCAG 2.2에 도입한 새로운 성공 기준(SC)에 대해 언급했습니다. 그러나 한 가지 SC인 SC 4.1.1 구문 분석이 선정되지 않았습니다.

"SC 4.1.1 구문 분석"이 무엇이었죠?

<div class="content-ad"></div>

성공 기준 (SC) 4.1.1은 세계적인 웹 consortium (W3C)이 WCAG 2.0에서 도입했습니다. 여기 WCAG 2.1의 현재 텍스트입니다:

이 SC는 A 레벨 기준이었으며, WCAG 준수를 요구했습니다.
실제로 필요한 것은 다음과 같습니다:
- "잘 형성된" 마크업. 예를 들어, 여는 `section` 태그는 대응하는 `section` 종료 태그를 가져야 합니다.
- 요소의 적절한 중첩
- 페이지에서 id 속성 값은 고유해야 합니다.
- 요소에 중복된 속성이 포함될 수 없습니다. 따라서 `p` 요소는 두 개의 class 속성을 가질 수 없습니다.

<div class="content-ad"></div>

예를 들어, 다음은 SC 4.1.1을 통과할 것입니다:

```js
<section id="contactInfo">
<h2>Contact us:</h2>
<ul>
<li style="color:#A5A; text-decoration:underline">Phone: (123) 456-7890</li>
<li>Email: <a href="mailto:someone@help.com">someone@help.com</a></li>
<li>Carrier Pigeon: Coo three times facing north</li>
</ul>
</section>
```

하지만 다음은 통과하지 못할 것입니다:

```js
<section id="contactInfo">
<h2 id="contactInfo">Contact us:
<ul>
<li style="color:#A5A" style="text-decoration:underline">Phone: (123) 456-7890</li>
<li>Email: <a href=mailto:someone@help.com>someone@help.com</a>
<li>Carrier Pigeon: Coo three times facing north</li></li>
</ul>
```

<div class="content-ad"></div>

위의 두 번째 예시에 문제가 있는 부분은 다음과 같습니다:

- 닫는 `section` 태그(`/section`)가 없습니다.
- 닫는 `h2` 태그(`/h2`)가 없습니다.
- 첫 번째 목록 항목(`li`)에 스타일 속성이 두 개 있습니다.
- `a` 요소의 href 속성 값에 따옰은 따옴표가 없습니다.
- 세 번째 목록 항목이 잘못된 위치에 중첩되어 있습니다.
- `section` 요소의 id 속성이 `h2` 요소에 중복되어 있습니다.

# SC 4.1.1이 해결하려고 한 것은 무엇인가요?

잘못된 마크업으로 인해 접근성에 문제가 발생하여 페이지가 정상적으로 작동하지 않았습니다 — 특히, 스크린 리더의 경우입니다. 그러나 SC 4.1.1이 발행된 이후, 브라우저는 잘못된 마크업을 처리하는 데 더 능숙해졌습니다.

<div class="content-ad"></div>

이는 브라우저가 나쁜 마크업을 "고쳐"주지는 않는다는 것을 의미하는 것은 아닙니다. 그저 브라우저가 더 유연하게 구문 분석할 수 있다는 것을 의미합니다. 때로는 문제가 해결되고 때로는 그렇지 않습니다.

따라서 위의 잘못된 마크업을 브라우저에 넣었을 때 어떻게 표시되는지 확인해 봅시다.

다음은 Chrome에서의 모습입니다 (Edge에서도 동일합니다):

![image](/assets/img/2024-07-12-WCAG22ParsingtheSuccessCriterionthatdidntmakeit_1.png)

<div class="content-ad"></div>

이제 Chrome과 Edge가 마크업을 어떻게 수정했는지 살펴보세요:

```js
<section id="contactInfo">
## Contact us:
<ul>
<li style="color: #A5A;">Phone: (123) 456-7890</li>
<li>Email: <a href="someone@help.com">someone@help.com</a></li>
<li>Carrier Pigeon: Coo three times facing north</li>
</ul></section>
```

잘 못된 마크업에서 브라우저가 몇 가지 문제를 수정한 것을 알 수 있습니다:

- `section` 요소에 이제 올바른 위치에 닫는 태그가 있습니다.
- `h2` 요소에 이제 닫는 태그가 있지만 올바른 위치에는 아닙니다. 결과적으로 모든 텍스트가 머릿말 레벨 2가 됩니다.
- 첫 번째 `li`에는 첫 번째 스타일 속성만 있습니다. 다른 하나( text-decoration: underline )는 삭제되었습니다.
- href 속성 값은 이제 큰따옴표로 둘러싸여 있습니다.
- 세 번째 리스트 항목(`li`)이 두 번째 리스트 항목과 올바르게 형제로 배치되었습니다.

<div class="content-ad"></div>

당신이 생각하고 있을 것 같아요. "와, 대단하네요. 브라우저가 중첩, 닫는 태그 누락, 그리고 `a` 요소의 href 속성에 대한 따옴표 누락도 고쳤네요. 하지만 제목을 제대로 수정하지 않았고 중복된 id 속성 값을 고려하지 않았네요. 그래서 SC 4.1.1은 그대로 WCAG 2.2에 유지되어야 하는 건 아닐까요?" 

이유에 대해 설명해 드릴게요. 

# SC 4.1.1 파싱이 WCAG 2.2에 포함되지 않았을까요?

## 중복성

<div class="content-ad"></div>

위의 예시에서 브라우저가 모두 잡지 못했지만, 이미 잘못된 부분이 다른 기준을 위반하고 있습니다.

`h2` 요소의 경우 다음 기준이 위반됩니다:
- SC 2.4.6 제목과 레이블
- SC 2.4.10 섹션 제목

중복된 id 속성 값의 경우 사용 방식에 따라 다음 기준이 위반이 됩니다:

<div class="content-ad"></div>

- SC 1.3.1 정보 및 관계
- SC 2.4.4 링크 목적 (컨텍스트 내)
- SC 2.4.6 제목 및 레이블
- SC 2.5.3 이름에 레이블
- SC 3.3.2 레이블 또는 지시사항
- SC 4.1.2 이름, 역할, 값
- 그리고 아마도 몇 가지 다른 요소들도 있을 수 있겠죠.

그렇다면 저희 예시는 모두 위 조건들을 어긴 것일까요? 아니요, 물론이죠. 사실, 마크업에서 동일한 id 속성 값을 가진 여러 요소가 있는 것은 가능하며 최근에 폐기된 SC 4.1.1을 제외하고는 어겨지지 않습니다. 하지만 그렇게 하는 것은 좋지 않습니다. 어떠한 경우에도 id 속성 값을 고유하게 유지하세요.

이를 무시할 수 있는 유일한 방법은 사용자가 상호작용하는 컨트롤과 관련된 어떠한 요소에서도 중복 id 속성을 사용하지 않은 경우뿐입니다.

개발자를 위한 특이사항: 항상 사용자 상호작용이 있는 모든 요소에 id 속성을 부여하는 것을 권장합니다. 심지어 그 id를 JavaScript나 코드 비하인드에서 사용하지 않더라도요. 이는 향후 자동화된 테스트, 디버깅 및 개발에 도움이 됩니다. 매우 작은 시간 투자가 오랜 기간에 걸쳐 많은 시간을 절약해 줄 수 있습니다. 그러니 꼭 포함하고, 고유하게 유지하세요.

<div class="content-ad"></div>

<img src="/assets/img/2024-07-12-WCAG22ParsingtheSuccessCriterionthatdidntmakeit_2.png" />

이 모든 것은 W3C가 SC 4.1.1이 더 이상 필요하지 않다고 결정한 이유였습니다. SC 4.1.1의 의미 있는 위반은 적어도 한 가지 이상의 다른 기준 위반이 발생할 것이라고 판단했기 때문입니다. 그래서 그들은 이것이 불필요한 중복이라고 결정했습니다.

## 혼란

이것은 W3C의 공식 발표에는 없어 보입니다. 그러나 SC 4.1.1의 원래 의도에 대한 오해가 있었던 모양입니다.

<div class="content-ad"></div>

SC 4.1.1의 출현으로 SC 4.1.1을 테스트할 수 있는 신뢰할 만한 도구가 없어 어려움을 겪었던 것도 사실입니다. 또한 그 범위에 대한 오해가 많았죠.

크리스토퍼 스트로브의 기사 "성공 기준 4.1.1의 곤란한 삶과 비극적인 죽음"에서 말하듯이:

# 단순히 삭제하는 대신 왜 폐기로 하지 않는 걸까요?

명확히 말하자면 SC 4.1.1은 WCAG 2.2에서 "사라져 없어진"으로 분류되어 있습니다. WCAG 2.2에서 영구적으로 삭제되는 대신 그 분류를 유지할 가능성이 높습니다. 그래서 "폐기"라는 용어를 사용하는 것입니다 - "제거"와 혼동을 피하기 위해서요.

<div class="content-ad"></div>

이것은 단순히 보존뿐만 아니라 WCAG 2.2를 역방향 호환 유지하는 데 관련이 있습니다. SC 4.1.1 Parsing을 제거하고 그 자리에 SC 4.1.2 Name, Role, Value를 이동한다면 매우 혼란스러울 것입니다.

게다가 테스트 문서 등 다양한 문서들이 즉시 오래되어 버릴 수 있습니다. SC 4.1.1이 사라져야 한다면, 내 의견으로는 폐기하는 것이 최선일 것입니다.

심지어 WCAG 2.0 및 WCAG 2.1에서도 이를 폐기하는 것에 대해 고려하고 있습니다. FAQ 페이지에 다음과 같이 나와 있습니다:

# SC 4.1.1의 소멸에 의해 나는 격려받고 있습니다

<div class="content-ad"></div>

SC 4.1.1가 폐기되어야 하는지 여부와 관계없이, 그것에 대해 매우 긍정적으로 생각합니다.

SC 4.1.1이 WCAG에 계속 포함되어야 하는지에 대한 논쟁은 어느정도 진행되었었죠 (기사 맨 끝에 링크를 통해 확인할 수 있어요); 그러나 W3C가 최종적으로 그것을 폐기하기로 결정함으로써, 그들이 기존 기준을 재평가하여 여전히 필요한지 결정할 의사가 있다는 것을 보여줍니다.

오랫동안 존재해온 기준에 익숙함만을 위해 그것에 고수하기는 쉬운 일입니다 — 하지만 그들은 그렇지 않았어요. W3C는 기술의 변화로 인해 해당 기준이 더 이상 필요하지 않다고 인정하고, WCAG 2.2를 업데이트 했어요.

접근성 옹호자로서 항상 해야 하는 것처럼, 우리는 문제를 인간적인 측면에서 바라보아야 합니다. 부적절한 마크업으로 인해 누가 곤란해할까요?

<div class="content-ad"></div>

스크린 리더를 사용하는 사람들, 시각 장애를 가진 사람들, 인지적 도전을 겪는 사람들 그리고 다른 사람들에게 형태가 잘못된 마크업이 무게를 놓을 수 있나요? 네. 그렇습니다.

하지만 다른 기준들이 형태가 잘못된 마크업으로 인한 문제를 해결할 수 있는 경우에 SC 4.1.1을 유지해야 할까요? 그렇지 않은 것으로 보입니다.

그래서 저는 W3C가 이제 SC 4.1.1을 사용하지 않는 것에 박수를 보내고 있습니다.

# 결론

<div class="content-ad"></div>

만약 성공 기준이 웹을 더 접근 가능한 곳으로 만들어주지 못한다면, 재평가되고 사용이 중단되어야 합니다.

결국, 중요한 건 사람들입니다. 접근성 전문가들과 개발자들에게 명백히 혼란을 주는 성공 기준을 유지하는 것은 아무도 도와주지 않습니다. 특히 다른 기준에서 이를 잡을 수 있거나 이 내용의 접근성에 영향을 미치지 않는 경우에는 더욱 그렇습니다.

# 링크

## SC 4.1.1에 대한 심층적인 탐구

<div class="content-ad"></div>

- (WCAG GitHub) 제안: Success Criterion 4.1.1 재구성
- (WCAG GitHub) Success Criterion 4.1.1은 WCAG 2.2에서 삭제됨
- 에드리언 로셀리에 의한 4.1.1에 관한 411 정보
- SC 4.1.1 구문 분석 이해 (WCAG 2.1)
- WCAG FAQ 페이지: 왜 WCAG 2.2에서 Success Criterion 4.1.1 Parsing이 사용되지 않게 되었나요? WCAG 2.0 및 2.1의 Parsing은 어떤가요?
- 크리스토프 스트로베에 의한 Success Criterion 4.1.1의 곤란한 삶과 한탄할 만한 죽음

## 제 다른 기사 목록

- WCAG 2.2 — 드디어 나왔습니다
- 접근성을 충족해야 할 필요로 전달하고 규칙을 따라야 하는 것으로 이해하면 안 됩니다
- 접근성은 오해되고 있습니다 — 수정합시다