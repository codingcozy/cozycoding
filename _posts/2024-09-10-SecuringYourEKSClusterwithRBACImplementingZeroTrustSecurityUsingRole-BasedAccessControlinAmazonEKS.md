---
title: "영어 제목 Securing Your EKS Cluster with RBAC Implementing Zero Trust Security Using Role-Based Access Control in Amazon EKS한글 제목 Amazon EKS에서 RBAC를 활용한 제로 트러스트 보안 구현하기 클러스터 보안 강화 방법"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-09-10 19:08
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Securing Your EKS Cluster with RBAC Implementing Zero Trust Security Using Role-Based Access Control in Amazon EKS"
link: "https://medium.com/@priyanshubhatt18/securing-your-eks-cluster-with-rbac-implementing-zero-trust-security-using-role-based-access-234b31c7f3bc"
isUpdated: false
---


조사에 따르면 현재 조직의 약 97%가 Public Cloud를 사용하고 있으며 약 83%의 회사 업무가 이제 클라우드 인프라로 전환되었습니다. 새로운 방식을 채택하는 조직 수가 증가함에 따라 응용 프로그램을 저장하고 배포하는 공격 및 악성 코드도 늘어나고 있습니다. 배포 또는 환경 설정 시 작은 구성 오류가 발생하면 막대한 데이터 손실이 발생할 수 있습니다.

Kubernetes는 가장 많이 사용되는 신뢰할 수 있는 Orchestration Tool로 개발자들에게 익숙한 이름이며, 서비스 중심 응용 프로그램 인프라의 여러 측면을 간소화합니다. API 서버는 Kubernetes 제어 평면의 핵심으로 모든 자원을 유효성 검사합니다. 클라이언트로부터 보내진 모든 요청은 먼저 API 서버로 전송되며, 인증을 거쳐 CRUD(생성, 읽기, 업데이트, 삭제) 작업이 허용됩니다. 따라서 API 서버로 들어오는 이러한 요청을 필터링할 수 있다면 클러스터를 거의 완전히 보호할 수 있습니다. API 서버가 클러스터의 게이트웨이로 불리기 때문입니다.

이 글에서는 Role Back Access Control (RBAC)을 사용하여 EKS 클러스터를 어떻게 보호할 수 있는지에 대해 알아보겠습니다. RBAC은 사용자가 Kubernetes 자원에 어떤 것에 액세스하고 어디에서 액세스할 수 있는지를 검증하는 사용 권한 단계입니다.

먼저, Amazon Elastic Kubernetes Service(EKS)가 무엇인지 알아보겠습니다.

<div class="content-ad"></div>

## Amazon Elastic Kubernetes Service 및 그 기능?

Amazon Elastic Kubernetes Service(EKS)는 온프레미스 및 AWS에서 Kubernetes를 실행할 수 있는 관리형 Kubernetes 서비스입니다. Amazon EKS를 사용하면 스케일링, 신뢰성, EKS 애드온, AWS 인프라의 고가용성, 그리고 IAM과의 AWS 보안 통합을 자동으로 관리할 수 있습니다. 또한 IAM과의 클러스터 관리, 롤백 액세스 제어(RBAC), AWS Virtual Private Cloud(VPC)와의 통합도 가능합니다.

EKS는 Kubernetes 제어 평면의 내구성을 유지하기 위해 다중 가용 영역에 걸쳐 복제함으로써 가용성을 보장합니다. Kubernetes 커뮤니티의 일부 플러그인을 사용하여 EKS는 기존 애플리케이션과 완전히 호환되는 통합을 제공합니다. Amazon EKS의 여러 이점 중 하나인 AWS IAM과 RBAC의 통합은 클러스터 내 사용자의 이동을 제어하여 클러스터에 대한 제로 트러스트 보안 접근 방식을 구현하는 데 중요한 보안 요소입니다.

## 롤백 액세스 제어를 사용한 권한 부여

<div class="content-ad"></div>

Kubernetes에서는 권한 부여가 최소 권한 제공 규칙을 따릅니다. 즉, 모든 계정이 완전한 액세스를 부여받는 것이 아니라 특정 네임스페이스 내에서 제한된 액세스를 부여받아야 합니다. 각 사용자에 대한 작업에 대한 정책과 규칙이 만들어집니다. 기본적으로 클러스터를 만든 사람에게 관리자 권한이 주어집니다. 사용자는 이동 및 업무에 대한 그룹 기반 역할 또는 개별 역할이 할당될 수 있으며, 이러한 역할들은 각 사용자에게 바인딩됩니다. 이를 역할 바인딩이라고 합니다.

RBAC 권한 부여는 rbac.authorization.k8s.io API 그룹을 사용하여 권한 부여 결정을 내립니다. AWS는 aws-iam-authenticator를 사용하여 Amazon EKS Kubernetes 환경의 일반 사용자를 사용하도록 외부 서비스 기능을 제공합니다. 이를 위해 IAM 자격 증명을 사용하여 클러스터에 인증 레이어를 제공합니다. 이는 Kubernetes 특별 위원회(SIG)에서 유지 관리하며, 웹훅 서비스는 aws-auth ConfigMap을 확인하여 IAM 신원이 클러스터 사용자와 일치하는지 확인합니다. 서비스 계정은 Kubernetes에서 관리되며, 일부에 바운드되어 있으며, 클러스터 생성 중에 생성됩니다. Amazon EKS는 토큰 인증 웹훅을 사용하여 요청을 인증하지만 최종 결정은 권한 부여를 위해 RBAC에 의해 이루어집니다.

RBAC를 사용하여 만들 수 있는 일부 역할은 다음과 같습니다:

- 클러스터 역할: 이러한 규칙은 클러스터 수준에서 적용됩니다. 즉, 기본적으로 네임스페이스가 설정되지 않습니다.

<div class="content-ad"></div>

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: Power-User
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "watch", "list"]
```

2. Roles: Roles are specified a particular namespace and wrt to the policies it is assigned to Users. These are namespace specific Roles.

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: namespace1
  name: User-space1
rules:
- apiGroups: [""]
  resources: ["pods" , "services"]
  verbs: ["get", "watch", "list"]
```

Roles then Bind to in different aspects, Role Binding is grant of Roles to users at namespace level whereas Cluster Binding grants access cluster wide. Role Binding can even reference a Cluster Role and Bind that to the namespace of Role Binding.

<div class="content-ad"></div>

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: pods-using
subjects:
- kind: Group
  name: Lead
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: Power-User
  apiGroup: rbac.authorization.k8s.io
```

## 아마존 EKS에서 이렇게 할 수 있어요:

IAM과 RBAC 사이의 통합 방법은 aws-auth-ConfigMap입니다. 이는 IAM 원칙(역할/사용자)과 Kubernetes(사용자/그룹)의 권한을 사용하여 클라이언트에 액세스 권한을 부여하는 완전한 인증 및 권한 부여 단계를 정의합니다.

- 네임스페이스 생성하기


<div class="content-ad"></div>

Namespace은 쿠버네티스 클러스터 안에 있는 가상 클러스터라고 생각할 수 있어요. 하나의 쿠버네티스 클러스터 안에 여러 개의 네임스페이스를 가질 수 있고, 서로 논리적으로 격리되어 있어요. 네임스페이스는 조직, 보안 및 성능에 도움을 줄 수 있어요!

```js
kubectl create namespace DemoNamespace

kubectl run --generator=run-pod/v1 nginx --image=nginx -n DemoNamespace
```

- IAM에서 사용자 만들기:

AWS는 사용자의 자원 접근을 제어하여 사용자에게 저렴한 고수준의 데이터 보안을 제공해요. 여기서는 IAM을 통합하므로 구성을 시작하는 데 필수인 사용자(유저)를 생성할 거예요. 우리는 Tony라는 사용자를 생성하고 tony.sh 스크립트를 사용해서 만들 거에요.

<div class="content-ad"></div>

```js
IAM_USER=Tony
aws iam create-user --user-name $IAM_USER

cat << EoF > $IAM_USER.sh
export AWS_SECRET_ACCESS_KEY=$(aws iam create-access-key --user-name $IAM_USER \
 --query AccessKey.[SecretAccessKey] --output text)
export AWS_ACCESS_KEY_ID=$(aws iam list-access-keys --user-name $IAM_USER \
 --query AccessKeyMetadata[0].AccessKeyId --output text)
EoF
```

- 클러스터에 사용자 매핑:

ConfigMaps를 사용하면 구성을 Pod 및 구성 요소와 분리할 수 있어서 Workload를 이동하기 쉽게 유지할 수 있습니다. kubectl edit configmap/aws-auth -n kube system을 사용하여 사용자를 configmap에 추가하고 사용자 섹션에 매핑하십시오.

```js
apiVersion: v1
data:
  mapRoles: |
    - rolearn: arn:aws:iam::<YOUR_AWS_ACCOUNT_NO>:role/InstanceRole-us-west-1-workers_asg1
      username: system:node:{EC2PrivateDNSName}
      groups:
        - system:bootstrappers
        - system:nodes
  mapUsers: |  <-- 이 섹션을 추가하세요
    - userarn: arn:aws:iam::<AWS ACCOUNT NO>:user/jack
      username: jack
kind: ConfigMap
metadata:
  creationTimestamp: "2022-3-23T14:23:54Z"
  name: aws-sample
  namespace: DemoNamespace
```

<div class="content-ad"></div>

커스텀 롤 생성: Role Binding에서 언급한 바와 같이, 동일한 방식으로 권한을 갖게 되는 역할을 생성할 것입니다. 이 역할은 정의된 네임스페이스에서 'get', 'watch', 'list' 권한을 갖게 될 것입니다.

```yaml
kind: Role
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  namespace: DemoNamespace
  name: pod-User
rules:
- apiGroups: [""] 
  resources: ["pods"]
  verbs: ["list", "get"]
- apiGroups: ["extensions", "apps"]
  resources: ["deployments"]
  verbs: ["get", "list"]
EOF
```

이러한 YAML 파일은 다음과 같이 적용됩니다:

kubectl apply -f `YAML_FILE_NAME.YML` (모든 YAML 파일이 이 명령어로 적용됩니다).

<div class="content-ad"></div>

Role Binding 작업 중입니다. Role Binding은 클러스터 내에서 사용자에게 권한을 부여하기 위해 역할을 사용자에게 바인딩하는 개념입니다. 

```js
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: pod-User
  namespace: DemoNamespace
subjects:
- kind: User
  name: Tony
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: pod-User
  apiGroup: rbac.authorization.k8s.io
kubectl apply -f <YAML_FILE_NAME.YML>
```

이제 환경이 모두 구성되었습니다. 설정을 테스트할 시간입니다.

```js
name: pod-User
  apiGroup: rbac.authorization.k8s.io
```

<div class="content-ad"></div>

```js
source tony.sh # 스크립트를 테스트하기 위해 사용
Kubectl get pods # 안돼요! 작동하지 않습니다. 이 사용자는 DemoNamespace 레벨만 보유하고 있어요
kubectl get pods -n DemoNamespace # 와우! 이건 작동할 거에요
kubectl create configmap my-config --from-file=path/to/bar # configmap에 대해 심지어 create 동사가 허용되지 않아 작동하지 않아요.
Kubectl delete deployment/nginx -n DemoNamespace # 이건 작동할 거에요
```

위에서 보여준 것처럼, 이러한 테스트 케이스들은 당신이 설정한 구성과 바인딩을 테스트하는 데 도움이 될 거에요.

## 결론

이 글에서 우리는 Amazon EKS에서 Role Based Access Control의 중요성과 그것을 어떻게 달성할 수 있는지에 대해 논의했어요. RBAC를 사용하여 권한을 부여하는 것은 Zero Trust Security Model을 구현하는 가장 좋은 방법 중 하나로 간주되며, RBAC는 핵심 보안 중 하나로 여겨지지만 설정에 완전히 의존하며, 사람들은 부정확한 설정을 만들어 클러스터 보안에 흠결을 초래하여 데이터 침해로 이어질 수 있어요.


<div class="content-ad"></div>

DevOps 도구에 대해 더 알고 싶다면 제 Terraform, ArgoCD, 그리고 Ansible 가이드를 확인해보세요!