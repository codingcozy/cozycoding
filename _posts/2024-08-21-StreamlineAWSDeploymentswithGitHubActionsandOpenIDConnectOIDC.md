---
title: "GitHub Actions와 OpenID Connect (OIDC)를 활용한 AWS 배포 자동화하는 방법"
description: ""
coverImage: "/assets/img/2024-08-21-StreamlineAWSDeploymentswithGitHubActionsandOpenIDConnectOIDC_0.png"
date: 2024-08-21 17:55
ogImage: 
  url: /assets/img/2024-08-21-StreamlineAWSDeploymentswithGitHubActionsandOpenIDConnectOIDC_0.png
tag: Tech
originalTitle: "Streamline AWS Deployments with GitHub Actions and OpenID Connect (OIDC)"
link: "https://medium.com/gitconnected/streamline-aws-deployments-with-github-actions-and-openid-connect-oidc-39892d74b8f5"
isUpdated: true
updatedAt: 1724245346306
---


현대의 DevOps에서는 보안과 자동화가 효율적인 업무 흐름을 유지하는 데 중요합니다. 이 중 특히 AWS로 애플리케이션을 배포하는 과정에서 이것이 더욱 두드러집니다. 전통적으로 이 프로세스는 CI/CD 파이프라인에서 장기간 사용되는 자격 증명(즉, AWS 액세스 키)을 사용하는 것을 포함하고 있는데, 이는 제대로 관리되지 않으면 위험할 수 있습니다. OpenID Connect (OIDC)은 저장된 자격 증명 없이 GitHub Actions가 직접 AWS와 인증하는 더 안전하고 간소화된 접근 방식을 제공합니다.

![이미지](/assets/img/2024-08-21-StreamlineAWSDeploymentswithGitHubActionsandOpenIDConnectOIDC_0.png)

이 기사에서는 GitHub Actions로 애플리케이션을 안전하게 AWS로 배포하기 위해 OIDC 기반 인증을 설정하는 방법을 살펴보겠습니다.

# 왜 OIDC를 사용해야 할까요?

<div class="content-ad"></div>

OpenID Connect (OIDC)은 OAuth 2.0 프로토콜 위에 구축된 신원 계층으로, 클라이언트가 권한 부여 서버에서 수행한 인증을 기반으로 사용자의 신원을 확인할 수 있게 해줍니다. GitHub Actions에서 OIDC를 사용하면 리포지토리에서 하드코딩된 비밀을 제거하여 인증 프로세스를 간소화할 수 있습니다.

GitHub Actions와 AWS에서 OIDC를 사용하는 주요 이점은 다음과 같습니다:

- 강화된 보안: OIDC를 사용하면 GitHub에 장기간 보관된 AWS 자격 증명을 저장할 필요가 없어 공격 표면을 줄입니다.
- 동적 자격 증명: 토큰은 수명이 짧고 동적으로 생성되므로 필요한 경우에만 유효합니다.
- 간소화된 관리: 자격 증명을 수동으로 처리할 필요가 없으며, 모든 것이 자동화되어 CI/CD 파이프라인 내에서 통합되어 있습니다.

# GitHub Actions 및 AWS에서 OIDC가 작동하는 방법

<div class="content-ad"></div>

위에 도표는 나의 고요한 영지로 나를 초대합니다.

테이블 태그를 마크다운 형식으로 바꾸시지요.

<div class="content-ad"></div>

## 단계 1: GitHub Actions를 위한 IAM 역할 생성하기

먼저 GitHub Actions가 가정할 수 있는 AWS 내 IAM 역할을 생성하세요. 이 역할은 배포 작업에 필요한 권한을 가지게 됩니다 (예: S3, Lambda, CloudFormation).

- AWS 관리 콘솔에서 IAM 서비스로 이동하여 새 역할을 생성합니다.
- 신뢰할 수 있는 엔티티의 유형으로 웹 신원을 선택합니다.
- 공급자로 GitHub를 선택하고 워크플로우를 트리거할 저장소를 지정합니다.
- 역할에 필요한 정책을 첨부합니다.
- 역할을 완료하고 생성한 뒤, 역할 ARN을 메모하세요.

## 단계 2: GitHub Actions 워크플로우 구성하기

<div class="content-ad"></div>

다음으로, AWS 인증에 OIDC(OIDC for AWS)를 사용하도록 GitHub Actions Workflow를 구성하세요.

- GitHub 저장소에서 workflow YAML 파일(.github/workflows/deploy.yml과 같은)을 생성 또는 업데이트하세요.
- OIDC 토큰 요청 및 IAM 역할을 가정하는 단계를 추가하세요:

```yaml
deploy:
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    needs: lint
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Configure AWS credentials from OIDC
        uses: aws-actions/configure-aws-credentials@v4
        with:
          role-to-assume: arn:aws:iam::<계정>:role/github-ci
          aws-region: eu-central-1
```

위 예시에서 aws-actions/configure-aws-credentials@v4는 OIDC 인증 및 역할 가정을 처리하는 공식 GitHub 액션이라고 합니다.

<div class="content-ad"></div>

## 단계 3: AWS에서 특정 GitHub 저장소를 유효성 검사하십시오

OIDC 통합을 더욱 안전하게 유지하기 위해 AWS에서 IAM 역할을 가정할 수 있는 것이 특정 GitHub 저장소만 유효한지를 확인하는 것이 중요합니다. 이렇게 함으로써 미인가된 저장소가 AWS 리소스에 액세스하는 것을 방지할 수 있습니다.

저장소 유효성 검사 구현 방법:

AWS에서 IAM 역할을 설정할 때 역할의 신뢰 정책에 조건을 추가하여 GitHub 저장소와 브랜치를 유효성 검사할 수 있습니다. 다음은 특정 저장소의 메인 브랜치로의 액세스를 제한하는 예제 신뢰 정책입니다:

<div class="content-ad"></div>

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Federated": "arn:aws:iam::ACCOUNT_ID:oidc-provider/token.actions.githubusercontent.com"
      },
      "Action": "sts:AssumeRoleWithWebIdentity",
      "Condition": {
        "StringEquals": {
          "token.actions.githubusercontent.com:sub": "repo:your-org/your-repo:ref:refs/heads/main"
        }
      }
    }
  ]
}
```

## 단계 4: 배포 및 모니터링

워크플로우를 구성한 후 변경 사항을 리포지토리에 푸시합니다. 이렇게 하면 GitHub Actions 워크플로가 트리거되고, AWS 자격 증명을 하드 코딩할 필요 없이 배포가 진행되는 것을 확인할 수 있습니다.

리포지토리의 GitHub Actions 탭에서 프로세스를 모니터링할 수 있습니다. 역할 가정이나 권한에 문제가 있는 경우, GitHub 및 AWS에서 문제를 진단하고 해결하는 데 도움이 되는 자세한 로그를 제공합니다.


<div class="content-ad"></div>

# 결론

GitHub Actions를 AWS와 OIDC를 사용하여 통합하는 것은 배포를 관리하는 안전하고 효율적인 방법입니다. 정적 자격 증명이 필요 없어지고 OIDC를 활용함으로써 CI/CD 파이프라인의 보안성을 향상시키고 자격 증명 관리를 간소화할 수 있습니다. 클라우드 환경이 계속 발전함에 따라 OIDC와 같은 현대적인 인증 메커니즘을 채택함으로써 워크플로우가 견고하면서도 보안 위협에 강건해지도록 보장할 수 있습니다.

이 통합을 활용하여 배포 프로세스를 간소화하고 응용 프로그램을 구축하고 확장하는 데 더 많은 시간을 투자하세요.

참고 문헌:

<div class="content-ad"></div>

- [https://github.com/raileanualex/aws-intro/blob/main/.github/workflows/main.yml](https://github.com/raileanualex/aws-intro/blob/main/.github/workflows/main.yml)
- [https://aws.amazon.com/blogs/security/use-iam-roles-to-connect-github-actions-to-actions-in-aws/](https://aws.amazon.com/blogs/security/use-iam-roles-to-connect-github-actions-to-actions-in-aws/)
- [https://www.microsoft.com/en-us/security/business/security-101/what-is-openid-connect-oidc](https://www.microsoft.com/en-us/security/business/security-101/what-is-openid-connect-oidc)
- [https://docs.github.com/en/actions/security-for-github-actions/security-hardening-your-deployments/about-security-hardening-with-openid-connect](https://docs.github.com/en/actions/security-for-github-actions/security-hardening-your-deployments/about-security-hardening-with-openid-connect)