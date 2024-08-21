---
title: "긴급 상황에서도 유용한 Git 명령어 마스터하는 방법"
description: ""
coverImage: "/assets/img/2024-07-12-LifesavingGitCommandsMasteringEmergencySituations_0.png"
date: 2024-07-12 22:00
ogImage: 
  url: /assets/img/2024-07-12-LifesavingGitCommandsMasteringEmergencySituations_0.png
tag: Tech
originalTitle: "Lifesaving “Git” Commands: Mastering Emergency Situations"
link: "https://medium.com/@codeeverywhere/lifesaving-git-commands-mastering-emergency-situations-0a9ccebcc421"
isUpdated: true
---




![이미지](/assets/img/2024-07-12-LifesavingGitCommandsMasteringEmergencySituations_0.png)

# 소개

개발자들 사이에서 널리 쓰이는 버전 관리 시스템인 Git은 강력한 도구입니다. 이를 통해 변경 사항을 원활하게 추적하고 프로젝트에서의 협업 작업을 할 수 있으며 모든 수정과 업데이트의 완전한 이력을 유지할 수 있습니다. 하지만 때로는 문제가 발생할 수 있습니다 — 잘못된 브랜치 병합, 실수로 커밋 삭제, 또는 다른 실수들이 발생할 수 있습니다. 고급 Git 명령어를 알고 있다면 위기를 극복할 수 있습니다. 여기에는 모든 개발자가 알아야 할 "비밀" 생명을 구할 수 있는 Git 명령어 가이드가 있습니다.

# 1. git reflog — 분실된 커밋 복구하기

<div class="content-ad"></div>

Git의 가장 강력한 재해 복구 기능 중 하나는 참조 로그(reflog)입니다. 이 명령은 저장소의 브랜치 머리를 업데이트한 모든 변경 사항의 대장을 제공합니다. 특히 리베이스(rebase)나 히스토리를 수정한 경우에는 유실된 커밋을 찾는 데 귀중한 도구입니다.

사용 예시:

```js
git reflog
git reset --hard HEAD@{index}  # 특정 시점의 상태로 리셋
```

# 2. git reset — 변경 사항 되돌리기

<div class="content-ad"></div>

만약 최근 커밋이 문제를 일으키거나 의도치 않았다면, git reset이 해결책이 될 것입니다. 특정 상태로 되돌아갈 수 있도록 해줍니다. (--soft, --mixed, --hard와 같은 플래그에 따라) 변경 사항을 취소하거나, 이전 커밋으로 되돌릴 수 있으며 모든 변경 사항의 히스토리를 삭제할 수 있습니다.

사용 예시:

```js
git reset --hard HEAD~1  # 가장 최근 커밋을 되돌립니다
```

# 3. Branch 비교를 위한 git checkout

<div class="content-ad"></div>

두 브랜치를 비교해야 할 때는 merge base와 함께 git checkout을 사용하면 아주 편리합니다. 이는 두 브랜치의 공통 조상을 찾아줘서 중립적인 입장에서 차이를 살펴볼 수 있게 해줍니다.

사용 예시:

```js
git checkout -b new-branch $(git merge-base master develop)  # master와 develop의 merge base로부터 새 브랜치 생성하기
```

# 4. git bisect — 버그 정확히 찾기

<div class="content-ad"></div>

버그 도입 원인을 분리하기 위해 git bisect은 커밋 기록을 바이너리 검색하는 작업을 자동화합니다. Git은 이력에서 좋은지 나쁜지 표시하여 정확한 버그 도입 커밋을 찾아냅니다.

사용 예시:

```js
git bisect start
git bisect good [좋은_커밋_ID]  # 이 커밋을 좋은 것으로 표시합니다
git bisect bad [나쁜_커밋_ID]  # 이 커밋을 나쁜 것으로 표시합니다
git bisect reset  # Bisect 모드에서 나옵니다
```

# 5. git stash — 일시적으로 변경사항 저장하기

<div class="content-ad"></div>

git stash는 로컬 수정 사항을 보관하고 작업 디렉토리를 HEAD 커밋과 일치하도록 되돌립니다. 미완료된 작업을 커밋하지 않고 빠르게 브랜치 간에 작업 내용을 전환해야 할 때 이상적입니다.

사용 예시:

```js
git stash push -m "작업 중"
git stash pop  # 보관된 변경사항 복원
```

# 6. git cherry-pick — 특정 커밋 적용

<div class="content-ad"></div>

만약 한 브랜치에서 다른 브랜치로 특정 커밋을 적용해야 한다면, git cherry-pick이 정말 유용합니다. 이를 사용하면 개별 커밋을 선택하여 현재 작업 중인 브랜치에 적용할 수 있습니다.

사용 예시:

```js
git cherry-pick <커밋 ID>
```

# 7. git revert — 커밋을 되돌리기 위해 히스토리를 재작성하지 않는 방법

<div class="content-ad"></div>

커밋에서 변경 사항을 되돌리면서 최근 기록을 보존하려면 git revert를 사용하면 지정된 커밋을 되돌리는 새 커밋을 생성합니다. 커밋 기록을 변경하지 않기 때문에 공유 저장소에서 안전하게 사용할 수 있습니다.

사용 예시:

```js
git revert <커밋ID>
```

# 결론

<div class="content-ad"></div>

이러한 고급 Git 명령어를 이해하고 숙달하면 귀하의 저장소를 효과적으로 관리하고 잠재적으로 파괴적인 상황을 방지하는 능력을 크게 향상시킬 수 있습니다. 이러한 도구들을 Git 기술 세트에 통합함으로써 견고하고 관리하기 쉽고 오류에 강건한 코드베이스를 유지할 수 있습니다.