---
title: "시니어 엔지니어처럼 Git 사용 하는 방법"
description: ""
coverImage: "/assets/img/2024-07-14-UseGitlikeaseniorengineer_0.png"
date: 2024-07-14 00:49
ogImage: 
  url: /assets/img/2024-07-14-UseGitlikeaseniorengineer_0.png
tag: Tech
originalTitle: "Use Git like a senior engineer"
link: "https://medium.com/gitconnected/use-git-like-a-senior-engineer-ef6d741c898e"
isUpdated: true
---




저는 팀과 프로젝트를 넘나드는 동안 Git의 이러한 기능들을 연구해왔어요. 몇 가지 작업 흐름(예: squash 하는 것)에 대한 의견을 개발 중이지만, 핵심 도구는 강력하고 유연하며(스크립트로 작성할 수도 있어요!).

# Git 로그 확인하기

기본적으로 Git 로그를 확인하려면 상당히 난해해요.

## git log은 기본적이에요

<div class="content-ad"></div>

`git log` 명령어를 사용하면 일부 정보를 얻을 수 있지만, 이 정보는 너무 자세하고 보통 필요한 정보가 아닙니다.

![UseGitlikeaseniorengineer_0.png](/assets/img/2024-07-14-UseGitlikeaseniorengineer_0.png)

정말로, 이 로그들은 누구에게도 인상을 주지 않아요. 지루하죠. 그리고 지금 당장 필요하지 않은 정보로 가득 차 있어요. 당신은 프로젝트에서 무슨 일이 있었는지에 대해 높은 수준의 이해를 얻고 싶은 중이에요.

<div class="content-ad"></div>

There’s a better way.

## git log with more visibility

By using the `git log --graph` command along with the `--format` option, we can easily obtain a clear summary of the git commits in our project.

```shell
git log --graph --format=format:'%C(bold blue)%h%C(reset) - %C(bold green)(%ar)%C(reset) %C(white)%an%C(reset)%C(bold yellow)%d%C(reset) %C(dim white)- %s%C(reset)' --all
```

<div class="content-ad"></div>

**마크다운 포맷으로 변경해주세요:**

많이 이쁜 로그들이네요! 옆에 가지 가지 나무까지 있어요.

이 로그들은 누가 어떤 작업을 했는지, 변경 사항이 언제 있었는지, 그리고 당신의 변경 사항이 전체 그림 속에 어디에 들어가는 지 보여줍니다.

`--graph`는 왼쪽에 나무 그래프를 추가합니다. 가장 세련된 그래프는 아니지만, 프로젝트의 가지에서의 변경 사항을 시각화하는 데 도움이 됩니다. (여기에서 문서를 읽어보세요.)

<div class="content-ad"></div>

--format 옵션을 사용하면 로그의 형식을 사용자 정의할 수 있습니다. 미리 정의된 형식을 선택하거나 이 예시와 같이 자체 형식을 작성할 수도 있습니다. (여기에서 문서를 확인하세요.)

--all 옵션은 로그에 모든 참조, 태그 및 브랜치를 모두 포함시킵니다 (원격 브랜치 포함). 모두가 필요하지 않을 수 있으므로 필요에 맞게 조정하세요. (여기에서 문서를 확인하세요.)

더 많은 정보를 얻으려면 git-log 문서를 참조하여 git 로그를 더욱 효율적으로 활용해 보세요. →

# 특정 커밋 이해하기

<div class="content-ad"></div>

특정 커밋에서 무슨 일이 벌어졌는지 알고 싶을 때가 많을 거예요. git show 명령어는 커밋에서의 변경 사항을 요약하여 보여주는데, 특정 파일의 변경 사항도 확인할 수 있어요.

## 커밋의 요약 보기

```js
git show <commit> --stat
```

![UseGitlikeaseniorengineer_2.png](/assets/img/2024-07-14-UseGitlikeaseniorengineer_2.png)

<div class="content-ad"></div>

--stat 플래그를 사용하면 커밋 요약과 변경된 파일, 그리고 변경 내용에 대한 세부 정보가 표시됩니다.

## 커밋에 대한 특정 파일 변경 사항 보기

특정 파일의 특정 라인 변경 사항을 살피고 싶을 때는 파일 경로와 함께 git show를 사용하세요.

```js
git show <commit> -- <filepath>
```

<div class="content-ad"></div>

Markdown 포맷에 따라 이미지를 변환하세요.

![Specific line changes for your file](/assets/img/2024-07-14-UseGitlikeaseniorengineer_3.png)

파일의 변경 사항을 보여줍니다. 기본값은 변경된 라인과 함께 변경된 라인이 파일에서 어디에 있는지 알려주는 앞뒤로 추가된 3개의 라인을 보여줍니다.

Git-show 문서에서 귀하의 Git 커밋 이해력 레벨을 높이는 방법에 대한 자세한 정보를 찾아보세요. → 

# 변경 사항 만들기

<div class="content-ad"></div>

프로젝트에서 브랜치를 만들고 브랜치에 변경 사항을 적용한 뒤 이 변경 사항을 다시 메인 브랜치에 병합할 준비가 되었습니다. 그 동안 다른 엔지니어가 동일한 파일을 변경했을 수도 있습니다. 😱

만약 GitHub와 같은 서비스를 사용 중이라면, PR에서 병합 충돌이 있는지 알려줄 것입니다.

![UseGitlikeaseniorengineer_4](/assets/img/2024-07-14-UseGitlikeaseniorengineer_4.png)

Git은 메인 브랜치에 다시 변경 사항을 밀어 넣기 전에 병합 충돌을 해결하도록 요청할 것입니다. 이는 다른 사람들이 하는 중요한 작업을 모두 파괴하고 싶지 않기 때문에 좋은 절차입니다.

<div class="content-ad"></div>

로컬에서 문제를 해결하기 시작하려면 일반적으로 두 가지 방법 중 하나를 선택합니다: 병합 또는 리베이스.

## git merge 대 git rebase

당신의 브랜치에 통합하고 싶은 메인 브랜치의 변경 사항이 있을 때, 해당 변경 사항을 병합하거나 다른 지점에서 브랜치를 리베이스할 수 있습니다.

병합은 하나의 브랜치에서 변경 사항을 가져와 다른 브랜치로 병합하는 단일 병합 커밋을 만듭니다.

<div class="content-ad"></div>

To merge your branch with the main branch in your Git repository, use the following command:


git merge origin/main your-branch


On the other hand, if you want to adjust the point at which your branch branched off from the base branch, use rebase like this:


git rebase origin/main your-branch


Typically, you'd opt for rebase when there are changes in the main branch that you want to incorporate into your branch. On the other hand, you would use merge when you have changes in your branch that you want to integrate back into the main branch.

<div class="content-ad"></div>

## 스쿼시할까 말까

과거에는 스쿼시 찬성이었습니다. 그러나 Derek Austin 박사의 기사가 제 생각을 바꾸게 했습니다! 🥳 그의 의견 외에는 추가할 내용이 없다고 생각합니다.

# 자료

- git-log 문서
- git-show 문서
- Git 병합 대신 Git 리베이스를 사용해야 하는 시점은 언제인가요? (스택 오버플로우)