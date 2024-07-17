---
title: "프로 개발자를 위한 Git 왜 선형 히스토리가 당신의 새로운 최고의 도구인가"
description: ""
coverImage: "/assets/img/2024-07-12-GitforProDevsWhyaLinearHistoryisYourNewBestFriend_0.png"
date: 2024-07-12 22:19
ogImage: 
  url: /assets/img/2024-07-12-GitforProDevsWhyaLinearHistoryisYourNewBestFriend_0.png
tag: Tech
originalTitle: "Git for Pro Devs: Why a Linear History is Your New Best Friend"
link: "https://medium.com/git-happy/git-for-pro-devs-why-a-linear-history-is-your-new-best-friend-df35215fc14"
---


![image](/assets/img/2024-07-12-GitforProDevsWhyaLinearHistoryisYourNewBestFriend_0.png)

안녕, 기술에 능한 개발자 여러분! 여러분을 위해 여기 있는데, 테크닉한 지혜를 조금 더 넣어 일상적인 코딩 생활을 확실히 만들어 드릴 의사 데릭 오스틴입니다🥳.

오늘은 git을 행복하게 하는 것으로, 소프트웨어 버전 관리 시스템 세계에서 침묵하는 영웅 중 하나인 선형 Git 히스토리 개념을 발굴할 것입니다.

어린이 책... 맵아체🦝에 대해 읽기 쉬울 정도로 깨끗한 커밋 로그와 소프트웨어 버전 히스토리를 만들기 위한 이 개념이 여러분을 놀라게 할 것입니다.

<div class="content-ad"></div>

그래서, 바로 시작해 봅시다!

## 선형 Git 기록이란 무엇인가요?

선형 Git 기록은 잘 유지되는 고속도로와 같습니다. A 지점에서 B 지점까지 혼동되는 부속 도로나 방향 전환 없이 직접적인 경로를 제공합니다.

각 커밋은 바로 뒤 이어지는 커밋을 앞서고, 서로 다른 버전들이 겹치거나 얽혀있는 것이 없습니다.

<div class="content-ad"></div>

선형이 아닌 히스토리와 대조적으로, 여러 명의 개발자가 별도의 브랜치를 만들어 각자의 커밋을 추가할 수 있는 비 선형 히스토리를 생각해보세요.

이러한 브랜치들이 다시 메인 브랜치로 병합될 때, 만들어지는 히스토리는 마치 mapaches 🦝 가 귀족 방문자로 보인다면서 Git 리포지토리에 얽힌 난잡한 정리된 얽힌 덩어리와 같을 수 있습니다. 다음 다이어그램에서 이 차이를 고려해보세요. 재미를 위해 메인 브랜치를 mapache 브랜치라고 부르겠습니다.

## 비선형 Git 히스토리 (병합 커밋)

```js
A---B---C---D---E feature
    /             \
F---G---H---I---J---K mapache
```

<div class="content-ad"></div>

여기서 커밋 레이블 G는 머지 커밋을 나타냅니다. 이 머지 커밋 내에서 다른 커밋 A-E는 커밋 F 이전에 발생했을 수 있습니다.

## 선형 Git 히스토리 (동일 브랜치 또는 리베이스 커밋)

```js
A---B---C---D---E---F---G---H---I---J---K---L---mapache
```

여기서의 차이점은 선형 Git 히스토리에서 초기 커밋으로 돌아가거나 시간 이동 없이 어떤 커밋이든 따라 갈 수 있다는 것입니다.

<div class="content-ad"></div>

# 선형 Git 히스토리 유지하기

이제 선형 히스토리의 매력을 확인했으니, 쉽게 추적할 수 있는 선형 타임라인이 있기 때문에 당신에게 순조롭게 질문하나가 있습니다. 어떻게 이를 실현할까요?

여러분의 새로운 베스트 프렌드, git rebase가 등장합니다.

적절하게 사용하면 리베이스는 브랜치의 히스토리를 다시 작성하여 메인 라인으로 병합되기 전에 도와줄 수 있습니다.

<div class="content-ad"></div>

실제로 어떻게 작동하는지 살펴보겠습니다. 현재 기능 브랜치를 나타내는 다음과 같은 저장소가 있는 것으로 상상해 보세요:

```js
git log --oneline

d4f6ad3 (HEAD -> feature): feat(photos): add mapache photos 🦝
9a8b7c1 fix(bug): fix issue #37
e7d6c5c (origin/main, origin/HEAD, main): feat(photos): add photo sort
```

우리는 기능 브랜치에서 작업하고 있으며, main에서 최신 변경 사항을 가져와서 이를 기능 커밋에 적용하고 싶습니다.

(명확히 하기 위해, 목록의 맨 위에 있는 두 개의 커밋인 d4f6ad3와 9a8b7c1은 우리가 main에 다시베이스하려는 기능의 커밋입니다.)

<div class="content-ad"></div>

여기에 해당하는 방법이 있습니다:

```js
git checkout feature
git rebase main
```

뭔가 이유로 인해 실제로 브랜치를 바꿔야 했는데, 위 명령을 실행하면 "현재 브랜치 feature는 최신 상태입니다"라는 결과가 나올 것입니다.

그리고 와라! 이제 우리의 feature 브랜치는 main과 최신 상태로 유지되며, 우리의 커밋은 깔끔하게 쌓여있어서 선형적인 히스토리를 유지하게 됩니다.

<div class="content-ad"></div>


```js
git checkout main
git rebase feature
```

깃허브 데스크탑에서도 브랜치 변경이 되지 않았다고 하니 깃허브 데스크탑에서도 작업하면 좋을 것 같아요. 그리고 `Rebase current branch…` 메뉴 명령어가 있습니다.

우리 Git Log™️를 살펴보면:

```js
git log --oneline

d4f6ad3 (HEAD -> main, main) feat(photos): add mapache photos 🦝
9a8b7c1 fix(bug): fix issue #37
e7d6c5c (origin/main, origin/HEAD) feat(photos): add photo sort
``` 


<div class="content-ad"></div>

만일 Git Log™️ 명령어에 대해 다시 알아보고 싶다면, 제가 마ージ와 스쿼시 커밋을 논의하면서 작성한 내용을 참고하세요. (스포일러: 비선형 병합을 선호합니다!)

보다 진지하게, 변경 사항을 원격 서버에 푸시하여 마무리하겠습니다. 현재 git status -u의 결과는 다음과 같습니다:

```js
On branch main
Your branch is ahead of 'origin/main' by 2 commits.
  (use "git push" to publish your local commits)
```

"git push" 명령어를 실행하거나 GitHub Desktop에서 "Push Origin"을 클릭하여 변경 사항을 푸시할 수 있습니다. 그런 다음 git log --oneline 명령어를 다시 실행해보겠습니다:

<div class="content-ad"></div>


```js
d4f6ad3 (HEAD -> main, origin/main, origin/HEAD, feature) feat(photos): add mapache photos 🦝
9a8b7c1 fix(bug): fix issue #37
e7d6c5c feat(photos): add photo sort
```

휴, 이제 안전하게 서버에서 리베이스된 변경 사항을 모두 볼 수 있습니다. 실제 세계에서는 아마도 GitHub에서 풀 리퀘스트를 통해 리베이스를 진행할 것입니다.

# 왜 선형적인 Git 히스토리가 필요할까요?

좋은 질문이네요, 친절한 독자 여러분! 그리고 여기에 와 주셔서 감사합니다. 프로젝트와 팀에 대해 선형적인 Git 히스토리를 선호하는 몇 가지 이유가 있습니다:


<div class="content-ad"></div>

- 간단함: 선형 이력은 한 눈에 쉽게 이해할 수 있습니다. 더 이상 일련의 이벤트를 이해하기 위해 git log --graph을 시도할 필요가 없어요.
- 이진 분할 가능성: 버그를 찾아야 할 때 git bisect가 신뢰할 수 있는 동료입니다. 그리고 선형 이력과 가장 잘 작동해요.
- 청결함: 선형 이력으로 모든 변경 사항이 각자의 커밋을 가지고 있고, 모든 커밋에 명확한 목적이 있어요. 이를 통해 리뷰가 쉬워져요.
- 교육적: 프로젝트 초보자(또는 심지어 미래의 자신)들이 프로젝트 이력을 쉽게 읽고 변경 사항의 순서를 이해할 수 있어요.

나는 이러한 이유들을 모두 이해하지만, 커밋을 리베이스하는 습관을 들이지 않아 왔고, 스쿼시 커밋을 좋아하지 않아요.

내가 방금 언급한 그 기사에 댓글을 남기신 독자들 사이에 공감이 있는 것은 선형적인 "스쿼시 + 리베이스"가 인기가 있다는 것이에요.

다음 주에는 git happy에서 다시 비선형 이력에 대해 다룰 거예요. 따라와 주시거나 해당 퍼블리케이션을 팔로우해주시면 발행되는 즉시 알림을 받으실 거예요.

<div class="content-ad"></div>

# 결론: 명백한 Git 기록의 중요성

여기까지입니다! 선형적인 Git 기록을 수용함으로써 프로젝트를 깨끗하고 관리하기 쉽게 유지할 수 있으며, "mapache-free" 🦝 상태를 유지할 수 있습니다.

기억해 주세요, Git은 단순히 코드 버전을 저장하는 도구가 아니라 커밋 기록을 통해 코드가 어떤 변화를 겪었는지를 전달하는 도구입니다.

기억해야 할 것은 선형적인 기록에서 각 커밋이 엄격한 연대순, 선형적인 시간 순서대로 발생했다는 것입니다.

<div class="content-ad"></div>

Git을 재베이스하는 대신 병합 커밋을 사용하면 비선형 기록이 생성됩니다.

선형 Git 기록은 잘 쓰여진 이야기와 같습니다. 당신 뒤를 이어오는 사람에게 프로젝트가 겪은 모든 변화와 역전을 안내해줍니다.

내 코딩 동료들아, 코드를 간단하게 유지하고, 기록을 선형으로 유지하며, 마지막으로, 너의 맵라치들을 잘 먹여라! 다음 번에 또 만나요. 즐거운 코딩되세요!

Dr. Derek Austin 🥳 올림.

<div class="content-ad"></div>

테이블 태그를 마크다운 형식으로 변경해주세요.