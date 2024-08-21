---
title: "ArgoCD를 사용해 App of Apps 패턴과 Helm Umbrella Charts로 Kubernetes에 Kube-Prometheus-Stack, Loki, Vault 설치하는 방법"
description: ""
coverImage: "/assets/img/2024-07-01-InstallKube-Prometheus-StackLokiandVaultonKubernetesusingAppofAppsPatternandHelmUmbrellaChartswithArgoCD_0.png"
date: 2024-07-01 00:23
ogImage: 
  url: /assets/img/2024-07-01-InstallKube-Prometheus-StackLokiandVaultonKubernetesusingAppofAppsPatternandHelmUmbrellaChartswithArgoCD_0.png
tag: Tech
originalTitle: "Install Kube-Prometheus-Stack, Loki, and Vault on Kubernetes using App of Apps Pattern and Helm Umbrella Charts with ArgoCD"
link: "https://medium.com/devops-dev/install-kube-prometheus-stack-loki-and-vault-on-kubernetes-using-app-of-apps-pattern-and-helm-959d02f21959"
isUpdated: true
---





쿠버네티스 배포에서 GitOps 패턴을 사용하는 것은 현대 인프라 및 복잡한 응용 프로그램 배포에서의 게임 체인저입니다. argocd와 같은 도구를 사용하여 응용 프로그램을 git 저장소에 추적하여 저장소에 저장된 매니페스트와 동기화하여 DevOps 및 인프라 코드 관행을 향상시킵니다.

이전 글에서 argocd가 무엇인지와 argo cd를 사용하여 클러스터를 부팅하는 방법을 설명했습니다. 이번 글에서는 argocd를 사용하여 helm 차트를 배포하는 데의 한계와 해결 방법을 살펴보겠습니다.

또한 이 전략을 시연하기 위해 Argocd 앱 오브 앱스 전략을 사용하여 Prometheus, Grafana, Loki 및 Hashicorp의 Vault를 설치할 것입니다.

# ArgoCD Recap

<div class="content-ad"></div>

ArgoCD 용어에서 이 용어는 무엇을 의미합니까? 그것에 들어가기 전에 우리가 먼저 ArgoCD가 무엇인지 간단히 소개하겠습니다. ArgoCD는 쿠버네티스 클러스터로의 지속적인 전달 및 배포를 보장하는 도구입니다. 이는 쿠버네티스 컨트롤러로 클러스터에 설치되어 쿠버네티스 리소스를 추적하고 해당 리소스의 상태를 미리 정의된 매니페스트와 비교합니다. 이러한 매니페스트는 응용 프로그램 상태의 단일 진실의 원천이며 ArgoCD는 응용 프로그램이 Git SCM에 정의된 것과 클러스터의 실제 상태와 동기화되도록 보장합니다.

전형적인 예는 쿠버네티스 배포 파일을 git에 저장하고 해당 파일을 argo cd 앱에 연결할 때 발생합니다. ArgoCD는 클러스터에서 배포가 동기화되어 있고 깃허브 저장소에 미리 정의된 것과 일치하는지 확인합니다. 따라서 심지어 클러스터에서 해당 배포를 삭제해도 ArgoCD가 다시 생성하여 Git에 정의된 매니페스트와 동기화되도록 보장합니다.

# 앱의 앱

이전 기사에서 ArgoCD CLI 도구를 사용하여 샘플 ArgoCD 앱을 만들었습니다. 콘솔 UI를 사용하여 앱을 만들 수도 있지만 여러 애플리케이션을 만들어야 하는 상황을 생각해보십시오. UI를 사용하여 계속해서 앱을 만들어야 하거나 이러한 ArgoCD 앱을 만들기 위해 CLI 명령을 계속 실행해야 할 것입니다.

<div class="content-ad"></div>

Another issue is that creating apps using UI or CLI doesn't align with GitOps practices. We need a method to generate our ArgoCD apps in a Git repository and deploy Kubernetes resources from there as well.

This is where the concept of "app of apps" shines. It's an approach introduced by the ArgoCD team, where you can create a single app using the UI or CLI. This app's sole purpose is to fetch other ArgoCD app manifests from a GitHub repository.

![Screenshot](/assets/img/2024-07-01-InstallKube-Prometheus-StackLokiandVaultonKubernetesusingAppofAppsPatternandHelmUmbrellaChartswithArgoCD_0.png)

By adopting this strategy, all your ArgoCD app configurations are stored in Git, ensuring adherence to GitOps practices. This way, all application configurations are fully managed within Git.

<div class="content-ad"></div>

그래서 “우산 차트 전략”은 무엇일까요?

## Helm 차트를 ArgoCD로 배포하는 것의 단점들

ArgoCD로 자체 관리되는 Helm 차트를 배포하는 것은 게임 체인저입니다. 그러나 한 가지 단점이 있습니다. ArgoCD는 직접 관리하지 않는 Helm 차트에 값을 전달하는 것을 허용하지 않습니다. 예를 들어 bitnami helm 차트를 설치하고자 할 때 자체 값 파일을 전달하려는 경우, ArgoCD 앱 구성에서 직접 지원하지 않습니다.

Helm 값들을 전달하려면 argocd 앱 구성 내부에 직접 넣어야 합니다. 이것이 우리가 원하는 바가 아닙니다. 값이 많은 경우에는 들여쓰기를 어떻게 관리해야 할까요? 동일한 차트의 여러 가지 변형을 만들어야 하는 경우에는 어떻게 해야 할까요?

<div class="content-ad"></div>

# 해결책: 우산 헬름 차트 사용하기

이 문제를 해결하기 위해 argocd에 값 파일을 전달하는 전략으로 헬름 차트 배포에서 사용되는 우산 차트라고 불리는 전략을 활용합니다. 이 전략을 사용하면 단일 헬름 차트를 생성하고 다른 차트를 레포지토리에서 종속성으로 추가하여 설치할 수 있습니다.

![이미지](/assets/img/2024-07-01-InstallKube-Prometheus-StackLokiandVaultonKubernetesusingAppofAppsPatternandHelmUmbrellaChartswithArgoCD_1.png)

주 차트에 이러한 관리되는 외부 차트를 종속성으로 추가하면 값을 첨부할 수 있게 되고, 이후 우산 차트 아래의 해당 값을 argocd 앱으로 지정하여 이러한 앱을 관리할 때 회복력을 확보할 수 있으며, 사용자 지정 값 파일을 argocd 앱 매니페스트에 원활하게 추가할 수 있습니다.

<div class="content-ad"></div>

이 개념을 실제로 보기 위해 데모를 배포해 보겠습니다. 함께 따라오려면 작동하는 쿠버네티스 클러스터가 필요합니다. 그런데 없는 경우 AWS EKS에서 클러스터를 생성하기 위한 테라폼 파일을 얻을 수 있는 프로젝트 저장소가 제공됩니다.

# 준비물

이 데모를 따라가려면 다음이 필요합니다.

- 쿠버네티스 클러스터 (없는 경우 테라폼이 제공됩니다)
- Kubectl 설치하기

<div class="content-ad"></div>

현재 이미 실행 중인 클러스터가 없다면, 아래의 사전 준비물이 필요합니다:

- Terraform 설치
- AWS 자격 증명

# 쿠버네티스 클러스터 프로비저닝 (옵션)

이 단계에서는 이 기사의 소개 섹션에 제공된 저장소의 terraform 폴더에 있는 terraform 구성을 사용하여 AWS EKS에서 terraform을 사용하여 Kubernetes 클러스터를 프로비저닝합니다.

<div class="content-ad"></div>

디렉토리로 이동한 후 다음 명령을 실행하세요:

```js
terraform init
terraform apply --auto-approve
```

이 terraform 설정은 VPC, 서브넷, EKS 클러스터, 노드 그룹, 3개의 노드 (SPOT)를 생성합니다.

완료하는 데는 약 20분 정도 소요됩니다.

<div class="content-ad"></div>

![Kube-Prometheus](/assets/img/2024-07-01-InstallKube-Prometheus-StackLokiandVaultonKubernetesusingAppofAppsPatternandHelmUmbrellaChartswithArgoCD_2.png)

# ArgoCD 설치하기

클러스터가 프로비저닝되면 다음 명령어를 사용하여 클러스터에 연결합니다.

```js
aws eks update-kubeconfig --region us-east-1 --name realworld-cluster
```

<div class="content-ad"></div>

컨텍스트가 업데이트되면 다음 명령을 사용하여 kubectl을 사용하여 ArgoCD를 설치합니다:

```shell
kubectl create namespace argocd
```

```shell
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

![ArgoCD Installation](/assets/img/2024-07-01-InstallKube-Prometheus-StackLokiandVaultonKubernetesusingAppofAppsPatternandHelmUmbrellaChartswithArgoCD_3.png)

<div class="content-ad"></div>

많은 리소스를 생성합니다. 아래 명령을 사용하여 성공적으로 생성되었는지 확인해 주세요.

```js
kubectl get pods -n argocd
```

모든 파드가 실행 중인지 확인해주세요.


![이미지](/assets/img/2024-07-01-InstallKube-Prometheus-StackLokiandVaultonKubernetesusingAppofAppsPatternandHelmUmbrellaChartswithArgoCD_4.png)

<div class="content-ad"></div>

이 데모에서는 로드 밸런서가 비용이 많이 들기 때문에 쿠버네티스 서비스에서 로컬 머신으로 포트 포워딩을 사용하여 ArgoCD UI 대시보드에 액세스할 것입니다.

# ArgoCD UI 대시보드에 액세스하기

다음 명령을 사용하여 로컬 브라우저에서 ArgoCD UI에 액세스할 수 있습니다. 터미널을 실행한 채로 두고 다른 터미널을 열어 다른 명령을 실행해 주세요.


kubectl port-forward svc/argocd-server -n argocd 8080:443


<div class="content-ad"></div>

![2024-07-01-InstallKube-Prometheus-StackLokiandVaultonKubernetesusingAppofAppsPatternandHelmUmbrellaChartswithArgoCD_5.png](/assets/img/2024-07-01-InstallKube-Prometheus-StackLokiandVaultonKubernetesusingAppofAppsPatternandHelmUmbrellaChartswithArgoCD_5.png)

앞서 제시한 코드를 통해 localhost:8080에서 로컬 브라우저를 통해 argocd에 접속할 수 있습니다. SSL 인증서가 안전하지 않다는 오류가 발생할 경우, '고급'을 선택한 후 argoCD 사이트에 진입하십시오.

ArgoCD의 기본 비밀번호를 확인하려면 다음의 명령어를 다른 터미널에서 실행하십시오:

```js
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}"
```

<div class="content-ad"></div>

비밀번호를 base64로 출력합니다

![이미지](/assets/img/2024-07-01-InstallKube-Prometheus-StackLokiandVaultonKubernetesusingAppofAppsPatternandHelmUmbrellaChartswithArgoCD_6.png)

온라인 base64 도구 https://www.base64decode.org/ 를 사용하여 비밀번호를 디코딩할 수 있습니다.

디코딩된 비밀번호를 사용하여 username으로 admin을 사용하여 argocd에 로그인하세요.

<div class="content-ad"></div>

# 우산 차트 만들기

이 섹션에서는 자식 차트가 포함된 우산 차트를 만들어보겠습니다. 작업 중인 디렉토리에서 다음 명령을 사용하여 helm 차트를 초기화하세요:

```js
helm create 우산-차트
```

이 명령을 실행하면 샘플 nginx helm 차트가 초기화됩니다. 이 차트를 수정하고 다른 종속성을 추가할 것입니다. template 폴더와 values.yaml 파일을 삭제해도 괜찮습니다.

<div class="content-ad"></div>


apiVersion: v2
name: umbrella-chart
description: A Helm chart for Kubernetes



type: application
version: 0.1.0
appVersion: "1.16.0"
dependencies:
- name: loki-stack
  version: 2.10.2
  repository: https://grafana.github.io/helm-charts
- name: vault
  version: 0.28.0
  repository: https://helm.releases.hashicorp.com


In the dependencies block, you can see that we have added two external charts that we do not manage, the loki-stack and HashiCorp’s Vault. The loki-stack is a Grafana-managed chart that installs Grafana, Loki, Promtail, Prometheus, and some alerting managers.


<div class="content-ad"></div>

그래서 재미있는 부분이 여기에 있어요! 동일한 우산 helm 차트 폴더에 loki와 vault용 값을 위한 values 파일을 만들어보세요.

아래의 내용을 loki.yaml에 붙여넣기 해보세요.

```js
test_pod:
  enabled: true
  image: bats/bats:1.8.2
  pullPolicy: IfNotPresent
```

```js
loki:
  enabled: true
  isDefault: true
promtail:
  enabled: true
  config:
    logLevel: info
    serverPort: 3101
    clients:
      - url: http://{ .Release.Name }:3100/loki/api/v1/push
fluent-bit:
  enabled: true
grafana:
  enabled: true
  sidecar:
    datasources:
      label: ""
      labelValue: ""
      enabled: true
      maxLines: 1000
  image:
    tag: 10.3.3
prometheus:
  enabled: true
  isDefault: true
filebeat:
  enabled: true
  filebeatConfig:
    filebeat.yml: |
      # logging.level: debug
      filebeat.inputs:
      - type: container
        paths:
          - /var/log/containers/*.log
        processors:
        - add_kubernetes_metadata:
            host: ${NODE_NAME}
            matchers:
            - logs_path:
                logs_path: "/var/log/containers/"
      output.logstash:
        hosts: ["logstash-loki:5044"]
logstash:
  enabled: false
  image: grafana/logstash-output-loki
  imageTag: 1.0.1
  filters:
    main: |-
      filter {
        if [kubernetes] {
          mutate {
            add_field => {
              "container_name" => "%{[kubernetes][container][name]}"
              "namespace" => "%{[kubernetes][namespace]}"
              "pod" => "%{[kubernetes][pod][name]}"
            }
            replace => { "host" => "%{[kubernetes][node][name]}"}
          }
        }
        mutate {
          remove_field => ["tags"]
        }
      }
  outputs:
    main: |-
      output {
        loki {
          url => "http://loki:3100/loki/api/v1/push"
          #username => "test"
          #password => "test"
        }
        # stdout { codec => rubydebug }
      }
# proxy is currently only used by loki test pod
# Note: If http_proxy/https_proxy are set, then no_proxy should include the
# loki service name, so that tests are able to communicate with the loki
# service.
proxy:
  http_proxy: ""
  https_proxy: ""
  no_proxy: ""
```

<div class="content-ad"></div>

```yaml
server:
  affinity: ""
  ha:
    enabled: true
    raft: 
      enabled: true
```

폴더 구조가 아래 스크린샷과 일치해야 합니다. 기본 차트 구성의 템플릿 폴더와 값을 삭제한 경우 기준으로 합니다.

![스크린샷](/assets/img/2024-07-01-InstallKube-Prometheus-StackLokiandVaultonKubernetesusingAppofAppsPatternandHelmUmbrellaChartswithArgoCD_7.png)

<div class="content-ad"></div>

여기서는 두 종속성을 가진 헬름 우산 차트를 성공적으로 생성했습니다.

# ArgoCD 어플리케이션 생성하기

이 섹션에서는 Loki 스택 및 Hashicorp Vault를 위한 ArgoCD 어플리케이션을 생성할 것입니다. /apps라는 폴더를 만들고, 그 안에 loki.yml 파일을 생성하십시오. 아래 내용을 붙여넣기 하세요:

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: loki
spec:
  destination:
    name: ''
    namespace: monitoring
    server: 'https://kubernetes.default.svc'
  source:
    path: umbrella-chart
    repoURL: 'https://github.com/philcz16/app-of-apps'
    targetRevision: HEAD
    helm:
      valuesFiles:
        - loki.yaml
  project: default
```

<div class="content-ad"></div>

argocd 마니페스트를 확인하면 umbrella-chart 폴더의 깃허브 저장소를 가리킵니다. 그런 다음 앞서 생성한 loki.yml 값을 파일로 지정합니다. 이 방법을 사용하면 ArgoCD 앱에서 특별히 관리하지 않는 애플리케이션을 위해 값을 파일로 쉽게 지정할 수 있습니다.

이제 HashiCorp Vault 차트에 대해 동일한 작업을 수행해보겠습니다. /apps 폴더에 vault.yml 파일을 생성하세요.

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: vault
spec:
  destination:
    name: ''
    namespace: default
    server: 'https://kubernetes.default.svc'
  source:
    path: umbrella-chart
    repoURL: 'https://github.com/philcz16/app-of-apps'
    targetRevision: HEAD
    helm:
      valuesFiles:
        - vault.yaml
  project: default
```

마지막으로, 이 개념에 따라 다른 앱들을 만들어낼 ArgoCD 루트 앱을 생성합시다. ArgoCD UI(아직 포트포워드를 사용 중이라면 localhost:8080)로 이동하여 새로운 앱을 만들고 YAML 수정을 선택하세요. 아래의 YAML 내용을 붙여넣기하세요.

<div class="content-ad"></div>

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: app-of-apps
  namespace: argocd

spec:
  destination:
    namespace: argocd
    server: https://kubernetes.default.svc
  project: default
  source:
    path: apps
    repoURL: https://github.com/philcz16/app-of-apps
    targetRevision: HEAD
```

Click Save and click create. For this demo, the sync policy is set to manual, so we will sync the application using the ArgoCD UI. You can see that ArgoCD has picked up the two apps we created in git that will install the helm charts we specified.

![Screenshot](https://example.com/assets/img/2024-07-01-InstallKube-Prometheus-StackLokiandVaultonKubernetesusingAppofAppsPatternandHelmUmbrellaChartswithArgoCD_9.png)


<div class="content-ad"></div>

로키 스택과 볼트 앱을 동기화하기 전에, 로키 스택이 리소스를 만들 네임스페이스를 미리 생성해주세요.

```js
kubectl create ns monitoring
```

로키 애플리케이션을 클릭하고 동기화를 선택해주세요. 서버 측 적용을 확인해주세요. 동기화를 클릭하면 쿠버네티스 클러스터에 모든 모니터링 리소스가 생성됩니다.

![로키 앱](/assets/img/2024-07-01-InstallKube-Prometheus-StackLokiandVaultonKubernetesusingAppofAppsPatternandHelmUmbrellaChartswithArgoCD_10.png)

<div class="content-ad"></div>

한 번 동기화가 완료되면 "동기화 완료"라는 응답을 받아야 합니다.

![이미지](/assets/img/2024-07-01-InstallKube-Prometheus-StackLokiandVaultonKubernetesusingAppofAppsPatternandHelmUmbrellaChartswithArgoCD_11.png)

아래 명령어를 사용하여 모니터링 리소스가 성공적으로 생성되었는지 확인할 수 있습니다:

```js
kubectl get all -n monitoring
```

<div class="content-ad"></div>

![InstallKube-Prometheus-StackLokiandVaultonKubernetesusingAppofAppsPatternandHelmUmbrellaChartswithArgoCD_12](/assets/img/2024-07-01-InstallKube-Prometheus-StackLokiandVaultonKubernetesusingAppofAppsPatternandHelmUmbrellaChartswithArgoCD_12.png)

모든 Loki 리소스가 성공적으로 생성된 것을 확인할 수 있습니다. 이제 Vault 리소스를 생성해 봅시다. Vault 앱을 선택하고 동기화를 클릭하세요. 또한 서버 쪽 적용을 확인해주세요.

![InstallKube-Prometheus-StackLokiandVaultonKubernetesusingAppofAppsPatternandHelmUmbrellaChartswithArgoCD_13](/assets/img/2024-07-01-InstallKube-Prometheus-StackLokiandVaultonKubernetesusingAppofAppsPatternandHelmUmbrellaChartswithArgoCD_13.png)

Vault 리소스가 성공적으로 생성되었는지 확인하려면 기본 네임스페이스에서 생성된 리소스를 확인해주세요.

<div class="content-ad"></div>

마침내 GitOps 워크플로를 시연하기 위해, 우산 helm 차트에 그라파나와 프로메테우스를 배포할 하나의 더 앱을 추가해봅시다. 아래의 코드 블록을 dependencies 아래의 Chart.yml 파일에 추가하여 그라파나와 프로메테우스를 배포합니다.

```js
- name: kube-prometheus-stack
  version: 59.1.0
  repository: https://prometheus-community.github.io/helm-charts
```

또한 kube-prom.yml이라는 값 파일을 추가하세요. 해당 값 파일의 내용을 사용하거나 글의 저장소에 있는 것을 사용할 수 있습니다.

<div class="content-ad"></div>

`/apps` 폴더에 `kube-prom.yml` 파일을 생성하여 argocd 앱을 만들고 아래 내용을 붙여넣으세요.

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: kube-prometheus
spec:
  destination:
    name: ''
    namespace: monitoring
    server: 'https://kubernetes.default.svc'
  source:
    path: umbrella-chart
    repoURL: 'https://github.com/philcz16/app-of-apps'
    targetRevision: HEAD
    helm:
      valuesFiles:
        - kube-prom.yaml
  sources: []
  project: default
```

이 앱은 `umbrella-chart` 폴더의 `kube-prom.yaml` 값을 가리킵니다. 변경 사항을 저장하고 git 저장소에 푸시하세요.

새로운 변경 사항을 푸시하면 ArgoCD에 있는 앱을 동기화하며, 추가한 kube-prometheus 앱이 argocd가 새로 선택한 것을 알 수 있습니다.

<div class="content-ad"></div>

![Install Kube-Prometheus-Stack Loki and Vault on Kubernetes using App of Apps Pattern and Helm Umbrella Charts with ArgoCD_15.png](/assets/img/2024-07-01-InstallKube-Prometheus-StackLokiandVaultonKubernetesusingAppofAppsPatternandHelmUmbrellaChartswithArgoCD_15.png)

ArgoCD UI에서 모든 애플리케이션을 개별적으로 동기화합니다.

![Install Kube-Prometheus-Stack Loki and Vault on Kubernetes using App of Apps Pattern and Helm Umbrella Charts with ArgoCD_16.png](/assets/img/2024-07-01-InstallKube-Prometheus-StackLokiandVaultonKubernetesusingAppofAppsPatternandHelmUmbrellaChartswithArgoCD_16.png)

# 정리작업

<div class="content-ad"></div>

만약 리포지토리 코드베이스에 제공된 테라폼 스크립트를 따라오셨다면, /terraform 폴더로 이동한 뒤 아래 명령어를 실행해주세요:


terraform destroy --auto-approve


이 명령어는 테라폼을 사용해 생성된 모든 리소스를 제거합니다.

# 마무리

<div class="content-ad"></div>

이 기사에서는 ArgoCD를 사용하여 클러스터에 배포할 때 헬름 차트에 사용자 지정 값 파일을 지정하는 것의 단점을 확인했습니다. 어떤 경우에는 우산 차트 방법을 사용하면 ArgoCD를 사용하여 쿠버네티스 클러스터에 공개 helm 차트를 설치하는 데 완전한 제어력과 유연성을 확보할 수 있습니다.