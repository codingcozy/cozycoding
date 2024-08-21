---
title: "Nx 모노레포에서 CICD 최적화 영향을 받는 앱만 배포하기"
description: ""
coverImage: "/assets/img/2024-08-21-OptimizingCICDinNxMonoreposDeployingOnlyAffectedApps_0.png"
date: 2024-08-21 17:41
ogImage: 
  url: /assets/img/2024-08-21-OptimizingCICDinNxMonoreposDeployingOnlyAffectedApps_0.png
tag: Tech
originalTitle: "Optimizing CI CD in Nx Monorepos Deploying Only Affected Apps"
link: "https://medium.com/@yuvrajkakkar1/optimizing-ci-cd-in-nx-monorepos-deploying-only-affected-apps-6073347033f6"
isUpdated: false
---


`<img src="/assets/img/2024-08-21-OptimizingCICDinNxMonoreposDeployingOnlyAffectedApps_0.png" />`

개발자가 수정한 앱만 배포되도록 설정하는 방법을 달성하려면, Nx의 내장 기능을 활용하여 변경 사항을 감지하고 영향을 받는 프로젝트에 대해서만 작업을 실행할 수 있습니다. 다음은 이를 설정하는 방법입니다:

## 1. Nx 영향을 받는 명령어

Nx는 Git 기록의 변경 사항을 기반으로 수정된 앱이나 라이브러리를 감지할 수 있는 "영향을 받는" 명령어 세트를 제공합니다. 이는 여러 프로젝트가 공존하는 모노 리포에서 특히 유용합니다.

<div class="content-ad"></div>

최신 변경 사항에 영향을 받은 애플리케이션만 빌드, 테스트 및 배포하려면 nx affected 명령어를 사용할 수 있어요.

# 2. GitHub Actions Example

다음은 GitHub Actions 워크플로를 구성하여 영향을 받은 애플리케이션만 배포하는 방법입니다:

```js
yaml
```

<div class="content-ad"></div>

```js
name: CI/CD
```

```js
on:
  push:
    branches:
      - main
jobs:
  affected:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Set up Node.js
        uses: actions/setup-node@v2
        with:
          node-version: '16'
      - run: npm ci
      - name: Build Affected Projects
        run: npx nx affected:build --base=main~1 --head=HEAD
      - name: Test Affected Projects
        run: npx nx affected:test --base=main~1 --head=HEAD
      - name: Deploy Affected Projects
        run: npx nx affected:deploy --base=main~1 --head=HEAD
```

해설:

- --base=main~1 --head=HEAD: 이 플래그들은 Nx에게 현재 HEAD(최신 커밋)와 main 브랜치의 이전 커밋을 비교하라고 지시합니다. Nx는 이러한 커밋 간의 변경 사항으로 영향을 받는 프로젝트를 결정합니다.
- npx nx affected:build: 영향을 받는 프로젝트만 빌드합니다.
- npx nx affected:test: 영향을 받는 프로젝트만 테스트합니다.
- npx nx affected:deploy: 영향을 받는 프로젝트만 배포합니다.

<div class="content-ad"></div>

# 3. 다양한 환경 다루기

여러 환경(예: 스테이징, 프로덕션)을 갖고 있다면, 영향을 받는 명령에 환경별 목표를 추가할 수 있습니다. 예를 들어, workspace.json에서 deploy:staging 또는 deploy:production 대상을 가질 수 있습니다.

```js
"deploy:staging": {
  "executor": "@nrwl/workspace:run-commands",
  "options": {
    "commands": [
      "echo 스테이징으로 배포 중",
      "some-deployment-command --env=staging"
    ]
  }
},
"deploy:production": {
  "executor": "@nrwl/workspace:run-commands",
  "options": {
    "commands": [
      "echo 프로덕션으로 배포 중",
      "some-deployment-command --env=production"
    ]
  }
}
```

그럼, 이렇게 사용할 수 있어요:

<div class="content-ad"></div>

```js
npx nx affected:deploy --target=deploy:staging --base=main~1 --head=HEAD
```

## 4. Advanced Scenario: CI/CD Pipeline for Each Developer

If each developer is working on different branches, you can extend this by setting up branch-specific deployments or using Nx’s `--base` flag to compare against their individual feature branches.

Example:

<div class="content-ad"></div>

```js
yaml
```

```js
on:
  push:
    branches:
      - main
      - feature/*
```

이렇게 하면 개발자가 자신의 기능 브랜치에 푸시할 때 영향을 받는 프로젝트만 빌드, 테스트 및 배포되어, 손대지 않은 애플리케이션의 불필요한 배포를 피할 수 있습니다.

# 5. 대규모 모노 레포지토리를 최적화하기

<div class="content-ad"></div>

- 캐싱 및 분산 빌드: Nx는 분산 빌드와 캐싱을 지원하여 변경되지 않은 빌드 아티팩트를 재사용함으로써 CI 프로세스를 가속화할 수 있습니다.
- 병렬 빌드: CI/CD 제공업체에서 지원하는 경우, 영향을 받는 다른 프로젝트 간에 병렬로 빌드 및 테스트를 실행할 수 있습니다.

이러한 설정은 개발자가 모노레포 내의 특정 앱에 변경 사항을 푸시할 때 해당 앱(및 필요한 경우 종속 항목)만 배포되도록하여 시간 및 리소스를 최적화합니다.