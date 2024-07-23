---
title: "내 코드를 주석 처리하지 않는 진짜 이유"
description: ""
coverImage: "/assets/img/2024-07-14-TheRealReasonIDontCommentMyCode_0.png"
date: 2024-07-14 00:52
ogImage: 
  url: /assets/img/2024-07-14-TheRealReasonIDontCommentMyCode_0.png
tag: Tech
originalTitle: "The Real Reason I Don’t Comment My Code"
link: "https://medium.com/gitconnected/the-real-reason-i-dont-comment-my-code-2c23da5a00c4"
---


거절을 받아도 내가 배울 건 단 한 가지도 없다.
![TheRealReasonIDontCommentMyCode_0](/assets/img/2024-07-14-TheRealReasonIDontCommentMyCode_0.png)

이전에 나의 경력 초반에는 꽤 불안했지만, 어떤 면에서는 게으름을 부리기도 했다.

나의 그룹 리더로부터 명세를 받자마자, 나는 프로그래밍 문장을 작성하는 데 덤덤하게 움직였다.

<div class="content-ad"></div>

어느 날 어떤 일을 이루기 위해 재촉했지만, 전체적인 해결책을 고안하지 않고 기계가 내 명령을 따르도록 하는 즉각적인 만족을 추구했습니다.

모델은 객체와 매우 유사한 구체적인 경계를 가지고 있습니다. 모델에는 인터페이스가 있습니다. 모델은 그 인터페이스를 통해 서로 통신합니다. 그리고 서로 일관되어야 합니다.

모델은 반사와 패턴 인식을 필요로하기 때문에 찾기 어렵습니다. 모든 것이 '캡슐화'라는 용어로 묶여 있는데, 이 용어의 실제 의미는 저에겐 알려지지 않은 것이었습니다.

'나는 게으른 개발자였다'라고 말할 때, 그것은 코딩을 기피했다는 의미가 아니었습니다.

<div class="content-ad"></div>

타협이 있었던 점은 모델링과 객체 설계에 대한 어떤 토론이나 나를 정신적으로 피로하게 만들었다는 것이었습니다. 화면을 가로지르는 참새를 날려야 한다면, 참새 구조와 비행 코드를 조합할 수 있었습니다.

하지만 OOP를 배웠음에도, 참새나 하늘 같은 클래스를 만들고 fly()라는 함수를 작성하여 재사용성을 어떻게 활용할 수 있는지 자연스럽게 떠오르지 않았습니다.

제 경력이 후에 진행될수록 캡슐화에 대해 더 나아졌습니다. 그러나 흥미롭게도, 초기에 그것에 대한 무지가 나의 주석에 대한 성향에 영향을 미쳤습니다.

<div class="content-ad"></div>

있어도 코멘트에 대해 독립적인 의견을 가지고 있지 않은 서비스 회사 프로그래머로서, 저는 컴퓨터 과학 학위가 없습니다.

상당히 잘 설계된 고효율 코드에 처음 노출되었을 때, 나는 내가 한 모든 변경 사항에 주석을 달라고 권고받았습니다.

좋은 출처에서 배우기보다는 주석 템플릿을 찾아내어 복사하고 붙이기하기 시작했습니다.

여기 첫 번째 보석이었습니다:

<div class="content-ad"></div>

```js
// 조는 회사가 안전하다고 느끼면 Account 객체를 공개해도 괜찮다고 생각할 것 같아요. 일단 지금은 그냥 넘어갔어요.

- 조한테 보낸 메시지인가요, 아니면 회사한테 보낸 건가요? 조가 회사보다 권한을 선포한 건가요, 아니면 그 반대입니까?
- 다른 말로 하면, 추상화란 좋은 소프트웨어 실천 방법이라는 것을 다른 방법으로 표현한 것인가요? 그런데 여기서는 그럴 필요가 없는 거죠.

그런데 더 복잡한 문장을 발견했어요:

```  


<div class="content-ad"></div>

제게는 너무나 간단한 문제라 여겨졌기 때문에, 'Lead'라는 클래스가 'Lead.cpp'의 맨 위에 위치해 있어 1200줄이 넘는 LOC를 가지고 있어서 이것을 읽기가 좀 귀찮았습니다.

한 명의 시니어 개발자와의 치열한 토론 끝에, 저자가 Lead 클래스 내에 정적 필터링 함수를 두는 것을 옹호한 것을 알게 되었습니다. 그는 'Figgy' 팀이 외부 어댑터 클래스를 작성하여 필터링 작업을 수행하며 동시에 Lead 인터페이스 내의 모든 멤버를 요청하는 것을 혐오했습니다.

한 가지 의문이 들었습니다. 누군가가 1200줄을 넘는 복잡한 클래스를 작성할 수 있으면서, 최소한 어딘가에 (완전히 다른) 인터페이스 (.h 파일)를 공유 문서를 통해 공유하거나 신청한 팀과도 함께 공유하지 않을 정도로 게을러질 수 있다는 것이 제게는 이해가 가지 않았습니다.

당시에는 그런 질문을 제기할만한 정신력이 부족했습니다. 지금은 있지만, 그 정도의 노력을 기울일 만큼 그것을 중요하게 여기지는 않습니다.

<div class="content-ad"></div>

# 클라이언트 이메일이 댓글에 대한 취향을 굳혔다:

첫 직장에 1년이 지난 시점에서 난 데드라인을 쫓고 있었다. 컴퓨터 과학 학위가 없는 저에게 부족한 부분을 보충하기 위해, 나는 초과 근무를 하며 소프트웨어 설계, C++, SQL을 배우는 데 깊이 몰두했다.

어느 멋진 아침, 내 메일함에 한 통의 이메일이 착륙했고, 나는 나의 경력 신념 중 많은 것들이 굳어버리게 했다.

그것은 클라이언트 매니저로부터였다. 우리 소프트웨어 프로젝트에서 가장 높은 권한을 가진 분으로, 제 전체 지휘 연쇄인 그룹 리드와 팀 리드가 그에 대해 매주 두 번씩 엎드려 있던 분이었다.

<div class="content-ad"></div>

```js
프로젝트에 매우 가치 있는 기여를 한 XXX를 소개해 드리게 되어 기쁩니다.

그는 댓글 표준을 만들었어요!

우리 팀이 코드 트레이싱에 어려움을 겪고 있다는 것을 잘 알고 있어요.

참고용으로 댓글 표준을 첨부했습니다.

ABC 국제[클라이언트]와 DEF 서비스[서비스 회사]를 포함한 모두가 이를 따라야 합니다. 

기존 코드를 변경할 때 말이죠.

기회가 되어 Team ABC 국제가 훌륭한 일을 하고 있는 반면, 
우리 계약 파트너들로부터도 배울 점이 많다는 것을 언급하고 싶어요!

이런 실천이 우리 팀의 희망과 
더 나은 품질의 결과물을 바라게 하는데 기여하길 바랍니다. 
각 릴리스 때마다요!

함께 걷는 이 길을 응원합니다,

해피조
```
As we reminisce about it now, the letter could be cringe-worthy, but at that moment, it left me in awe. The entire office was filled with celebration vibes.

It was hard to believe that a product company manager would express such gratitude towards an offshore contractor earning only a fraction of his salary.

<div class="content-ad"></div>

원본 파일을 열기도 전에 밝은 동료를 축하하러 달려갔어요.

그는 가벼운 눈웃음으로 인사했어요. 조금 무례한 감이 느껴졌어요. 활짝 펴진 손가락으로 타이핑을 하고 있었죠 (아마도 감사 인사를 쓰고 있었을 거에요). 그가 바쁘게 일하고 있는데 내가 방해가 되는 것 같아서 그랬을지도 몰라요.

나는 내 자리로 돌아가서 주석 표준 첨부 파일을 열었어요:

```js
/// Start Comment: Added by XXX [개발자 이니셜]
/// mm.dd.yyyy
/// TRS-1234 추가 주석을 달아 가독성 향상 [티켓 식별자]
/// End Comment: Added by XXX [개발자 이니셜]
```

<div class="content-ad"></div>

나는 그것을 확인하기 싫었다. 그것은 널리 인정받았기 때문에(이메일 수신자가 60명이었다), 그것은 업계 표준이겠지!

단 4줄! 1시간 후, 나는 그것을 발명하지 못한 내 자신에게 의문을 품었다.

어쨌든, 그는 무엇을 했을까! 지금까지 그가 코딩을 하는 걸 거의 못 봤다. 네, 그의 결과물은 내 것보다 낫았지만, 그 뒤에는 강력한 이유가 있었다: 그는 도끼를 갈 시간이 더 있었기 때문에, 우리는 항상 나무를 베도록 시켜졌던 것이다.

대부분의 시간은 다른 부서를 빌런 것에 소비되었고, 나 같은 미약한 사람들을 자주 업신여기고, 나의 상사의 사무실을 방문하는 데에 시간이 들었다. (나는 아직 그 이유를 모른다).

<div class="content-ad"></div>

이 사람이 받은 건 찬성, 인정, 그리고 칭찬 뿐이었다.

반면에: 상사는 나와 거의 눈을 맞추지 않았다. 컴파일러는 LOC마다 나를 꾸짖었다. 악몽은 오류와 미해결 업무로 넘쳐났다.

나는 우수한 품질이 들어갈 여지가 없는 작업(코딩)를 고른 걸까? 그게 사실이라 할지라도, 저렴한 생각은 어떻게 생각하지 않았을까?

그리고 갑자기 깨달았다: 나는 항상 캡슐화에 서툴렀다. 그리고 밝은 동료는 그것을 잘했다.

<div class="content-ad"></div>

그가 표준으로 패키징한 항목들은 코드보다는 별거 아니었지만 소프트웨어 개발 과정에서의 목적인 추적성에 대한 방대한 중요성을 고려했다. 2:00 AM에 잠이 들기 전까지 머릿속에서 계속 생각이 났다. C++의 "//" 문자 외에도, 왜 표준에 추가 구분자가 필요한 것일까? '시작 주석'과 '끝 주석' 태그는 무엇을 의미할까? HTML의 새로운 주석 규칙인가? 내일 그에게 물어봐야겠다.

<div class="content-ad"></div>

The next day, our team lead enthusiastically rallied us to action: "Let's collaborate to squash all bugs before the big release! The more issues we fix, the smoother the launch will be. And hey, let's order some pizzas for those burning the midnight oil at the office." 

Surprisingly, the mastermind behind the innovative commenting standard remained mysteriously absent all day. 

As the day progressed, I realized I had forgotten to inquire about the delimiters with him.

In just two weeks, thanks to his proactive nature and go-getter attitude, he was promoted to a prestigious role overseas, where he would work closely with the client manager. Exciting times ahead!

<div class="content-ad"></div>

개발 사이클이 출시된 지 여섯 달이 지났을 때, 새로운 클라이언트 빌드 엔지니어로부터 이메일을 받았다: 컴파일이 엄청난 지연을 겪고 있다. 빨리 수정해 주세요!

저희 시니어 C++ 개발자가 이를 프로파일링하여, 주석 크기가 전체 코드베이스의 40%를 초과했음을 발견했다! 특정 클래스 파일에서는 주석이 코드를 초과했다.

팀 리드를 설득하는 데 하루가 걸렸는데, 우리의 위대한 발명품이 문제인 것을 발견했다: 너무 많은 주석 사용 규칙.

팀 리드는 확고했다: 믿겠어. 그러나 우리가 무엇인가를 뒤집을 수 없는 이유는 클라이언트가 우리를 감사히 여겼던 부분이기 때문이다. 기술과 비즈니스는 다르다.

<div class="content-ad"></div>

우리 시니어 개발자는 단호하게 말했다: 기술이 우리 사업의 핵심이다. 이 문제를 해결하기 전까지는 한 줄의 코드도 작성하지 않겠다고.

나는 그가 미쳤다고 생각했고, 그에게 엄격한 질책이 올 것이라 생각했다. 그러나 그의 용기를 보며 또 다른 두 명의 시니어 개발자가 그에게 동참했다.

팀 리더는 말했다: 이게 동작하지 않으면, 곧 롤백해야 할 것이다 — 그에 대비하여 밤새도록 일해야 한다. 그런 상황이 오게 된다면, 야근 수당은 따로 지급되지 않을 것이라는 말은 필요없이 이해되었다.

그 변화는 하루만에 이루어졌고, 몇 가지 복잡한 찾기 및 바꾸기를 통해 작업이 완료되었다 (그 때 나는 정규 표현식을 모르고 있었다). 문제가 해결되었고 변경 사항이 배포되었다.

<div class="content-ad"></div>

The following day, the client build engineer shared that there had been a 16% decrease in compilation time. The firm stand taken by our senior developers had been proven right.

However, no thank-you note arrived. The team lead simply carried on as if nothing extraordinary had occurred.

I truly longed for the festivities that accompanied our impeccable commenting practices. It was akin to the thrill of discovering quantum theory, only for it to be disproven, leaving the universe devoid of excitement.

During a casual chat with my senior developers over coffee, I expressed my sympathy for them.

<div class="content-ad"></div>

"그 후회할 가치는 없어." 대석한 남자가 말했다. "코멘트 표준은 마케팅 장식일 뿐이야. 부적당한 사람들이 그것으로 자랑스럽게 여기지."

"하지만, 서비스업에선 이런 플레이가 필요한 법이야. 걔가 거기 파견된 이후로 몇몇 빌링 광고를 만들었다니까." 다른 개발자가 덧붙였다.

그들은 그 후 벌어진 일들을 되짚어보았다.

우리 밝은 동료는 똑똑한 코멘팅 표준을 단번에 떠올리지 않았다. 그는 클라이언트 매니저와 여러 이메일을 주고받았는데, 그 매니저는 자신의 임기 안에 뚜렷한 변화를 이뤄야 하는 강한 압박을 받았다.

<div class="content-ad"></div>

문제 목록을 공개한 그는, 저희 밝은 동료가 가장 쉬운 방법을 선택했습니다 — 추적 가능성입니다. 해결책은 주석 표준이었습니다.

그로 인해 그의 위치가 높아졌지만, 클라이언트 매니저의 경력을 구할 수는 없었습니다. 표준을 발표한 지 1개월만에, 클라이언트 매니저는 해고되었습니다. 변화를 위한 변화는 수용되지 않았습니다.

전체 극장을 반성하며, 코드 주석에 대한 제 입장은 확고해졌습니다. 전혀 쓸모 없다고는 생각하지 않았지만, 분명 제 시간과 관심할 가치가 없는 것으로 분류했습니다.

사용자의 투표 사례를 생각해보죠: 당신의 투표는 미미한 영향을 미칩니다. 선거 후에, 당신은 다른 사람들에게 누구에게 투표했는지 그리고 왜 그랬는지 알립니다. 이제, 공유한 것을 기반으로 듣는 사람들이 다음 투표 선호도를 변경할 가능성을 평가해보세요.

<div class="content-ad"></div>

수학적으로, 당신의 댓글이 팀에 현저한 영향을 미칠 확률은 동일한 패턴을 따른다.

## 읽기 전용 관계:

그래도 나는 다른 사람들이 쓴 댓글을 즐기는 데 방해가 되지 않았다.

우리의 고품질 C++ 코드는 허접한 이야기로 넘쳤다:

<div class="content-ad"></div>


// 세상에, 카운터도 쓰레드 안전하지 않다니.
// 휴가 뒤에는 정리를 해야겠군.


// FOR 루프는 항상 문제를 일으킨다.
// 그는 교육 대출을 상환하지 않아 해쉬맵 교실에서 몰아낼까?


// 제이크에게 금요일마다 맥주를 마시지 않을 거라고 말할게.

// 이럴수가, 또 또 또 또 - 또 또 또 있어!!!



그리고 나는 일하는 동안 몇 시간 동안 StackOverflow에서 읽은 소중한 정보들을 발견했다. 이를 팀원들과 함께 나누어 기쁨을 함께 했다.


// 이 코드는 최악이야, 너도 알고 나도 안다.
// 지금은 지나가고, 당신이 바보라고 나중에 불러.

// 나는 이 코드의 책임이 없다.
// 나를 강제로 이렇게 작성하도록 했다, 나의 의지에 반해서.

// 만약 이 주석이 사라지면 프로그램이 폭발할 것이다.

// 가끔 컴파일러가 내 주석을 모두 무시하는 것 같다.

// 이 모든 코드, 나의 모든 작업을 내 아내, 다린에게 바친다. 
// 그녀는 나와 우리의 세 아이와 개를 지원해야 할 것이다. 
// 그것이 공개되면

// 취중진담, 나중에 고치기

#define TRUE FALSE
// 행복한 디버깅 시간이 되길

// 미안하지만, 우리의 공주는 다른 성에 있다.

const int TEN=10; // 10의 값이 변할 것 같냐...

long long ago; /* 아주 먼 우주 어딘가에서 */

# 재귀를 이해하려면 이 파일의 맨 아래를 보세요
# 재귀를 이해하려면 이 파일의 맨 위를 보세요

/* 당신은 이걸 이해한 것이 아닙니다 */

// 왜 이게 작동하는지 확신할 수 없어

// ie 브라우저용 해킹 (ie가 브라우저라고 가정)



위의 주석들은 밝은 동료의 구분자 태그를 떠올리게 했다.

<div class="content-ad"></div>

```js
/// <remarks>
/// 이 코드는 그의 엉망인 디자인을 피해 모바일 컨트롤에서 페이징이 작동하도록 하는 것입니다. 
/// 주된 문제는 모든 것을 처리할 수 있길 희망했던 BindCompany() 메서드입니다. 그는 죽었으면 좋겠네요.
/// </remarks>
```

마침내, 나에게 가장 공감되는 것을 찾았습니다:

```js
// 당신을 위한 주석 없음 - 작성하기 어려웠으므로 읽기도 어려워야 합니다
```

# 결론:

<div class="content-ad"></div>

내가 성장할수록 댓글을 쓰는 것에 점점 덜 흥미를 느끼게 되었다. 몇몇 회사들은 이 습관 때문에 나를 거부했지만, 나는 스스로를 바꾸기를 거부했다.

내 혐오의 첫 번째 부분은 그 실천의 의미 없음이었다.

나는 더 높은 수준의 프레임워크로 전환함에 따라 (Clean Code 운동을 무장하여 종종 그것들을 직접 작성하며), 매 명제가 더 많은 의미를 지니기 시작했다. 만약 명제가 자명하다면, 그 의미를 줄일 수밖에 없는 댓글을 왜 써야 하는가?

댓글을 포기하는 내 결심의 두 번째 부분은 더 개인적이었다.

<div class="content-ad"></div>

몹시 좌절한 코더들의 창의력을 전부 지켜본 적이 있습니다. 항상 그 마법을 팀에서 다시 만들고 싶어했죠.

하지만, 그런 상호작용은 팀원들이 의미 있는 연결을 만들 때에만 일어납니다. 다른 사람들이 당신의 코멘트를 읽지 않는다면, 심지어 읽는다 해도 어떤 피드백도 주지 않는다면, 그것이 의미가 있을까요?

일시적인 의식의 일부를 원격 기계에 저장하는 이유가 무엇인가요?

게다가, 우리의 훌륭한 코멘트 표준 프로젝트처럼, 여러분의 악명 높은 수다스레드가 대폭 증가한다면, 누군가는 성능 구원을 위해 나설 것입니다.

<div class="content-ad"></div>

```js
// 만약 다시 이것을 보면 직장에 총을 가져올 것이다
```

Pen Magnet이 게시할 때마다 이메일을 받고 싶나요? 그럼 여기를 클릭해서 구독자 목록에 이름을 올려보세요.

Pen Magnet은 인기 있는 시니어 개발자 인터뷰 eBook의 저자입니다.

<div class="content-ad"></div>

시니어 개발자 인터뷰를 위한 포괄적인 접근 방식 (40개 이상의 예시 질문)