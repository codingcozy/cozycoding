---
title: "AWS EKS를 활용한 Kubernetes를 이용한 3계층 애플리케이션 컨테이너화와 배포 방법"
description: ""
coverImage: "/assets/img/2024-08-21-HowtoContainerizeandDeployaThree-TierApplicationonAWSEKSwithKubernetes_0.png"
date: 2024-08-21 17:52
ogImage: 
  url: /assets/img/2024-08-21-HowtoContainerizeandDeployaThree-TierApplicationonAWSEKSwithKubernetes_0.png
tag: Tech
originalTitle: "How to Containerize and Deploy a Three-Tier Application on AWS EKS with Kubernetes"
link: "https://medium.com/@amanpathakdevops/how-to-containerize-and-deploy-a-three-tier-application-on-aws-eks-with-kubernetes-327c104f9c9f"
isUpdated: false
---


![이미지](https://miro.medium.com/v2/resize:fit:1400/1*9_I9zmesMevMBGqZ__28Zw.gif)

# 소개

Kubernetes 클러스터에 세 단계 애플리케이션을 배포하는 것은 어려울 수 있지만, 기본 동작 원리를 이해하면 훨씬 쉬워집니다. 이 안내서에서는 Docker를 사용하여 애플리케이션을 컨테이너화하고 Kubernetes 매니페스트를 사용하여 Kubernetes (EKS) 클러스터에 배포하는 단계를 안내합니다. 이후에는 비슷한 애플리케이션을 배포하는 명확한 로드맵을 갖게 될 것입니다.

# 필수 조건

<div class="content-ad"></div>

시작하기 전에 다음 사항을 준비해 두었는지 확인해주세요:

- 도커 지식: Dockerfile 생성 및 이미지 관리에 익숙해야 합니다.
- 쿠버네티스 기초 지식: 파드, 서비스, 배포 등에 대한 이해가 필요합니다.
- AWS 계정: EKS 설정에 필요합니다.
- 명령 줄 도구: Docker, kubectl 및 AWS CLI가 설치되어 있고 구성되어 있는지 확인하세요.

쿠버네티스 클러스터에 세 단계 애플리케이션을 배포하기 전에 디렉토리 구조를 이해해야 합니다. 따라서 따라하면서 혼란스럽지 않을 것입니다.

아래 스니펫에 표시된 모든 디렉토리에는 세 가지 범주가 있습니다.

<div class="content-ad"></div>

첫 번째 Source Code는 디렉토리 이름이 backend 및 student-teacher-app입니다.

두 번째 및 세 번째는 동일하지만 DevOps에 속하는 다른 사용 사례가 있습니다.

DevOps 디렉토리에는 도커 및 쿠버네티스 Manifest가 있습니다. 이름에서 알 수 있듯이, 우리는 먼저 애플리케이션을 컨테이너화한 다음, Kubernetes Manifest의 도움으로 Kubernetes(EKS) 클러스터에 배포할 것입니다.

![이미지](/assets/img/2024-08-21-HowtoContainerizeandDeployaThree-TierApplicationonAWSEKSwithKubernetes_0.png)

<div class="content-ad"></div>

디렉토리 구조를 이해하셨으면 좋겠어요.

이 글을 계속 읽으시기 전에 하나 말씀 드릴게요. 제 주요 목표는 도커 파일과 쿠버네티스 매니페스트를 처음부터 만들어가면서 프론트엔드, 백엔드, 데이터베이스가 서로 통신하는 3계층 애플리케이션을 배포하는 방법을 가르쳐드리는 거에요. 이 방법을 이해하면 개발자에게 조금만 물어보면 다른 어떤 애플리케이션도 쉽게 배포할 수 있게 될 거예요. 한가지 더, 쿠버네티스 배포 파일에 우리가 추가할 수 있는 기능들이 정말 많지만, 우리 주요 목표는 애플리케이션을 배포하고 데이터를 저장하고 삭제하고 요청하는 것이에요. 문제가 없다면, 이는 우리 애플리케이션을 완벽하게 배포한 거죠.

# 애플리케이션을 컨테이너화

# 데이터베이스 도커 파일

<div class="content-ad"></div>

데이터베이스 애플리케이션부터 시작해 봅시다. 일반적으로 조직은 RDS 및 DocumentDB와 같은 관리 클라우드 서비스를 사용합니다. 그러나 우리의 경우, 관리되지 않는 서비스와 어떻게 작동하는지 이해해야 합니다.

우리의 데이터베이스 도커 파일은 더 많은 지시사항이 필요하지 않아 매우 간단합니다. 우리는 기본 이미지, 노출된 포트, 데이터를 지속시키는 볼륨, MySQL 서버를 실행하는 CMD 지시를 제공했습니다.

여기 한 가지 더 중요한 관찰이 있습니다. 동일한 기본 이미지 이름으로 쿠버네티스 배포 파일을 직접 배포할 수 있지만 다시 말하지만, 이것은 이해 목적을 위한 것입니다. 프로세스를 이해하면 효율적이고 효과적인 방법을 찾을 수 있을 것입니다.

```js
FROM mysql:latest
# MySQL 포트 노출
EXPOSE 3306
# MySQL 데이터를 위한 명명된 볼륨 정의
VOLUME /var/lib/mysql
# MySQL 서버 실행하는 명령어
CMD ["mysqld"]
```

<div class="content-ad"></div>

도커 파일 준비가 완료되었다면 도커 이미지를 만들어 도커 허브 저장소로 푸시할 수 있습니다.

아래 명령어를 따라 이미지를 빌드하고 도커 허브로 푸시하세요.

도커 이미지를 도커 허브에 푸시하려면, 도커 로그인 명령어를 실행해야 합니다. 이때 도커 허브의 Personal Access Token(PAT)이 필요합니다.

```js
docker build -t <your_dockerhub_username>/mysql-image:<tag> .
docker push <your_dockerhub_username>/mysql-image:<tag>
```

<div class="content-ad"></div>

# 백엔드 Dockerfile

이제 백엔드 Dockerfile을 작성할 차례입니다. 우리의 코드는 nodejs로 되어 있기 때문에 Dockerfile에서 베이스 이미지로 node를 사용할 것입니다.

컨테이너 내에서 의존성을 설치하고 코드를 빌드해야 합니다. 그를 위해 별도의 작업 디렉토리인 app을 사용할 것입니다. 그 다음에 packages.json 파일을 작업 디랙토리 내에 복사할 것입니다. 그 다음 npm install 명령어를 실행하여 의존성을 설치할 것입니다.

의존성이 설치된 후에는 현재 시나리오에서 서버 코드인 server.js를 복사해야 합니다.

<div class="content-ad"></div>

서버.js 파일을 복사한 후 CMD를 통해 백엔드 응용 프로그램을 실행할 것입니다.

```js
# 공식 Node.js 이미지를 기본 이미지로 사용
FROM node:14
# 컨테이너 내부의 작업 디렉토리 설정
WORKDIR /app
# package.json 및 package-lock.json을 작업 디렉터리로 복사
COPY package*.json ./
# 의존성 설치
RUN npm install
# 나머지 응용 프로그램 코드를 작업 디렉터리로 복사
COPY . .
# 응용 프로그램을 실행하는 명령어
CMD ["node", "server.js"]
```

도커 파일이 준비되면 도커 이미지를 생성하고 도커 허브 저장소에 푸시할 수 있습니다.

아래 명령을 따라 이미지를 빌드하고 도커 허브에 푸시할 수 있습니다.

<div class="content-ad"></div>

```js
도커 빌드 -t <your_dockerhub_username>/backend-image:<tag> .
도커 푸시 <your_dockerhub_username>/backend-image:<tag>
```

# 프론트엔드 Dockerfile

이 도커 파일을 멀티 스테이지로 빌드할 것입니다. 주된 이유는 도커 이미지의 크기를 줄이기 위함입니다. 보통 이 방법은 실시간 프로젝트에서 사용됩니다. 그러니 여러분도 시도해 보세요.

프론트엔드 도커 파일에서는 일반적인 명령어를 사용하여 일반적인 리액트 애플리케이션을 빌드합니다. 만약 'ENV'나 'npm' 업데이트와 같은 추가된 내용을 보신다면, 이는 제 애플리케이션이 상당히 오래되어 새로운 의존성을 갖는 새 패키지가 필요하기 때문입니다.


<div class="content-ad"></div>

첫 번째 단계가 완료되면, 새로운 스테이지 내부에 빌드 폴더 내용을 복사하여 첫 번째 단계에서 node_modules 폴더에 설치된 종속성으로 인해 크기를 줄일 수 있습니다.

빌드 폴더를 복사한 후, `npm start` 명령을 통해 애플리케이션을 실행할 수 있습니다.

```js
# 스테이지 1: Node.js를 사용하여 애플리케이션 빌드
FROM node:21-alpine3.17 as build
WORKDIR /app
COPY . .
ENV NODE_OPTIONS= - openssl-legacy-provider
RUN npm install
RUN npm update
RUN npm run build
# 스테이지 2: 애플리케이션 실행
FROM node:21-alpine3.17
WORKDIR /app
COPY --from=build /app /app
ENV NODE_OPTIONS= - openssl-legacy-provider
CMD ["npm", "start"]
```

도커 파일이 준비되면 도커 이미지를 생성하여 도커 허브 저장소에 푸시할 수 있습니다.

<div class="content-ad"></div>

아래 명령어를 따라 이미지를 빌드하고 Docker Hub에 푸시하세요.

```js
docker build -t <your_dockerhub_username>/frontend-image:<tag> .
docker push <your_dockerhub_username>/frontend-image:<tag>
```

여기서 우리는 우리의 애플리케이션을 컨테이너화했습니다.

# 쿠버네티스 매니페스트로 컨테이너화된 애플리케이션 배포하기

<div class="content-ad"></div>

어떤 YAML 또는 매니페스트 파일을 생성하기 전에, 전체 3계층 응용 프로그램을 배포하기 위해 사용할 몇 가지 사항을 설명하려고 합니다. 모든 세 응용 프로그램에 대한 배포를 위해 배포를 사용할 것입니다. 세 응용 프로그램 간의 연결이 필요합니다. 이는 Kubernetes Service 객체를 통해 이루어질 것입니다. 지속적인 저장소가 필요한데, 이를 위해 PersistentVolume 및 PersistentVolumeClaim을 사용할 것입니다. 따라서 Pod가 문제로 인해 삭제되더라도 데이터가 손실되지 않습니다. 또한, 백엔드 응용 프로그램이 데이터베이스 자격 증명 등을 필요로 할 때 최선의 방법을 따르기 위해 Kubernetes Secrets를 사용할 것입니다. AWS SSM, Vaults 등의 도구를 사용하여 자격 증명을 더 안전하게 보관할 수 있습니다.

# 데이터베이스 Kubernetes 매니페스트

저는 자세한 설명과 함께 매니페스트 파일을 제공할 것입니다.

네임스페이스 - 이 파일은 새로운 네임스페이스를 생성할 것입니다. 격리된 네임스페이스에 응용 프로그램을 배포하는 것이 혼란을 피하기 위한 좋은 방법입니다.

<div class="content-ad"></div>

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: mern
  labels:
    name: mern
```

Pv- 이 파일은 포드가 삭제된 후에도 데이터가 남아있는 지속적인 볼륨을 만듭니다. 그러나 PVC 없이는 작동하지 않습니다.

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: db-pv
  namespace: mern
spec: 
  capacity: 
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  storageClassName: standard
  hostPath:
    path: /mnt/data
```

Pvc- 이 파일은 특정 지속적인 볼륨(PV)로부터 포드에 의해 스토리지를 요청하는 지속적인 볼륨 클레임을 만듭니다.

<div class="content-ad"></div>

```js
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: db-pvc
  namespace: mern
spec: 
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
  storageClassName: standard
```

Secrets- 이 파일은 응용 프로그램을 위한 시크릿을 생성합니다. 이제 시크릿은 데이터베이스 이름, 데이터베이스 암호 및 데이터베이스 호스트(Kubernetes 서비스)와 관련이 있습니다. 이는 백엔드 및 데이터베이스 배포 파드에서 모두 사용됩니다.

```js
apiVersion: v1
kind: Secret
metadata: 
  name: db-credentials
  namespace: mern
type: Opaque
data:
  host: ZGF0YWJhc2U= #database
  user: cm9vdA== #root
  password: bXlzcWwxMjM= #mysql123
  database: c2Nob29s #school
```

Deployment- 이 파일은 데이터베이스 파드의 레플리카를 생성하는 배포를 만듭니다. 이 글에서 이전에 푸시한 이미지 이름과 같은 몇 가지 사항을 관찰해야 합니다. 또한 데이터베이스 배포에 시크릿 및 PVC를 첨부했습니다.

<div class="content-ad"></div>

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: database
  namespace: mern
  labels:
    role: database
    env: dev
spec:
  replicas: 1
  selector:
    matchLabels:
      role: database
  template:
    metadata:
      labels:
        role: database
    spec:
      containers:
      - name: database
        image: avian19/mysql-mern:2
        imagePullPolicy: Always
        ports:
        - containerPort: 3306
        env:
        - name: MYSQL_ROOT_PASSWORD
          valueFrom:
            secretKeyRef:
              name: db-credentials
              key: password
        - name: MYSQL_DATABASE
          valueFrom:
            secretKeyRef:
              name: db-credentials
              key: database
        volumeMounts:
        - name: db-storage
          mountPath: /var/lib/mysql
      volumes:
      - name: db-storage
        persistentVolumeClaim:
          claimName: db-pvc
```

Service- This file will create a service where that will help to allow the connection to the database pods from any other pods. In our case, we need to make a communication between the backend and the database.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: database
  namespace: mern
spec:
  ports:
  - port: 3306
    protocol: TCP
  type: ClusterIP
  selector:
    role: database
```

# Backend Kubernetes Manifest


<div class="content-ad"></div>

배포- 이 파일은 백엔드 파드에 대해 레플리카를 생성하는 배포를 만듭니다. 이전 글에서 푸시한 이미지 이름과 같은 사항을 주목해야 합니다. 또한 백엔드 배포에 시크릿을 첨부했습니다.

```js
apiVersion: apps/v1
kind: Deployment
metadata:
  name: backend
  namespace: mrn
  labels:
    role: backend
    env: dev
spec:
  replicas: 1
  selector:
    matchLabels:
      role: backend
  template:
    metadata:
      labels:
        role: backend
    spec:
      containers:
      - name: backend
        image: avian19/backend:1
        imagePullPolicy: Always
        ports:
        - containerPort: 3500
        livenessProbe:
          httpGet:
            path: backend/
            port: 3500
          initialDelaySeconds: 3
          periodSeconds: 10
        env:
        - name: host
          valueFrom:
            secretKeyRef:
              name: db-credentials
              key: host
        - name: user
          valueFrom:
            secretKeyRef:
              name: db-credentials
              key: user
        - name: password
          valueFrom:
            secretKeyRef:
              name: db-credentials
              key: password
        - name: database
          valueFrom:
            secretKeyRef:
              name: db-credentials
              key: database
```

서비스- 이 파일은 백엔드 파드로부터 다른 모든 파드로의 연결을 허용하는 서비스를 생성합니다. 우리의 경우, 데이터베이스와 프론트엔드 파드 간의 통신을 수립해야 합니다.

백엔드 파드는 포트 3500에서 실행되며 이것이 targetPort입니다. 그러나 우리는 백엔드 애플리케이션에 80포트로 액세스하고 싶습니다. 따라서 이를 구성했습니다. 이것은 우리의 인그레스 부분에서 도움이 될 것입니다.

<div class="content-ad"></div>

```yaml
apiVersion: v1
kind: Service
metadata:
  name: backend
  namespace: mern
spec:
  ports:
  - name: backend-port
    port: 80
    protocol: TCP
    targetPort: 3500
  selector:
    role: backend 
```

# 백엔드가 데이터베이스 파드와 어떻게 연결될 것인가요?

네, 이것은 서비스지만 백엔드 파드에 제공해야 할 것이 있습니다. 그래서 데이터베이스 파드와 통신하는 방법을 찾을 것입니다.

그렇다면 어디에 제공했는지 확인해보겠습니다.

<div class="content-ad"></div>

만약 관찰하신다면, 우리는 데이터베이스 쿠버네티스 매니페스트 파일에서 secrets를 생성했습니다. 거기서 호스트를 ZGF0YWJhc2U= #database로 제공했습니다. 그래서 데이터베이스(인코딩된 값 ZGF0YWJhc2U=)는 데이터베이스 쿠버네티스 매니페스트 파일에 생성한 서비스입니다. 이 데이터베이스 서비스는 백엔드 파드가 데이터베이스 파드와 통신하는 데 도움이 될 것입니다.

# 프론트엔드 쿠버네티스 매니페스트

배포- 이 파일은 프론트엔드 파드를 위해 복제를 생성하는 것입니다. 이 글에서 이전에 푸시한 이미지 이름 같은 것들을 주목할 필요가 있습니다. 또한, REACT_APP_API_BASE_URL 환경 변수를 프론트엔드 배포에 추가하여 백엔드 파드와 연결할 수 있도록 했습니다.

```js
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend
  namespace: mern
  labels:
    role: frontend
    env: dev
spec:
  replicas: 1
  selector:
    matchLabels:
      role: frontend
  template:
    metadata:
      labels:
        role: frontend
    spec: 
      containers:
      - name: frontend
        image: avian19/frontend-mern:8
        imagePullPolicy: Always
        ports:
        - containerPort: 3000
        env:
        - name: REACT_APP_API_BASE_URL
          value: "http://<your-domain-name>/backend"
        - name: NODE_OPTIONS
          value: "--openssl-legacy-provider"
```

<div class="content-ad"></div>

Service- 이 파일은 프론트엔드 파드와 다른 파드에서 연결을 허용하는 서비스를 생성합니다. 우리의 경우에는 백엔드와 프론트엔드 파드 간의 통신을 할 수 있어야 합니다.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: frontend
  namespace: mern
spec:
  ports:
  - port: 3000
    protocol: TCP
  type: ClusterIP
  selector:
    role: frontend
```

# 프론트엔드가 백엔드 파드와 어떻게 연결될 것인가요?

네, 이것은 서비스입니다. 하지만 백엔드 파드에 제공해야 할 것이 있습니다. 따라서 백엔드 파드와 통신하는 방법을 찾을 것입니다.

<div class="content-ad"></div>

그래서, 여기에 제공한 것이 있습니다.

만약 당신이 그것을 관찰하면, 우리는 백엔드 쿠버네티스 매니페스트 파일에서 env REACT_APP_API_BASE_URL을 제공했습니다. 그래서, 내가 말하고 싶은 것은, 백엔드가 바로 우리가 백엔드 쿠버네티스 매니페스트 파일에서 생성한 서비스입니다. 이 백엔드 서비스는 백엔드 포드가 백엔드 포드와 통신하는 데 도움이 될 것입니다.

하지만 이번에는 도메인 이름이 서비스 이름과 함께 표시됩니다. 왜 그러죠?

당신은 그 방법으로 작업할 수도 있습니다. 그러나 여기서 우리는 백엔드 서비스 이름과 함께 완전한 도메인 이름을 사용할 새로운 접근 방식을 시도해 볼 것입니다.

<div class="content-ad"></div>

# 어떻게 진행될까요?

클러스터 외부에 응용 프로그램을 배포할 예정이므로 로드 밸런서를 사용할 것입니다. 인그레스 컨트롤러가 도와줄 것입니다. 따라서 클러스터 외부에 백엔드 응용 프로그램을 노출할 것입니다(최선의 방법은 아닙니다).

# 인그레스 쿠버네티스 매니페스트

응용 프로그램을 클러스터 외부로 노출하는 유일한 파일이 있습니다. 인그레스를 통해 프론트엔드 및 백엔드를 배포하고 있습니다. 기본 경로(/)에서 프론트엔드가 배포되며 /backend 경로에서 백엔드가 배포됩니다. 참고로, 인터넷을 향한 로드 밸런서를 배포하고 있으며, 이는 인터넷에 응용 프로그램을 배포하기 위해 사용됩니다. 한편, 내부 로드 밸런서라는 다른 유형의 로드 밸런서도 있으며 이를 사용하여 백엔드 응용 프로그램을 배포할 수 있습니다. 그러나 여기서는 프론트엔드 및 백엔드 응용 프로그램을 인터넷을 향한 로드 밸런서에 모두 배포하고 있습니다.

<div class="content-ad"></div>

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: mern-alb-dev
  namespace: mern
  annotations:
    kubernetes.io/ingress.class: alb
    alb.ingress.kubernetes.io/listen-ports: '[{"HTTP": 80}]'
    alb.ingress.kubernetes.io/scheme: internet-facing
    alb.ingress.kubernetes.io/target-type: ip
    alb.ingress.kubernetes.io/rewrite-target: /$2
spec:
  ingressClassName: alb
  rules:
    - host: <your-domain-name>
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: frontend
                port:
                  number: 3000
          - path: /backend
            pathType: Prefix
            backend:
              service:
                name: backend
                port:
                  number: 80
```

이론은 충분합니다. 이제 직접 실습을 해보고 몇 분 내에 애플리케이션을 배포해 봅시다.

도커 허브에 푸시된 이 세 개의 도커 이미지가 있습니다.

![이미지](/assets/img/2024-08-21-HowtoContainerizeandDeployaThree-TierApplicationonAWSEKSwithKubernetes_1.png)


<div class="content-ad"></div>

세 개의 팟이 모두 동작 중입니다

![image](/assets/img/2024-08-21-HowtoContainerizeandDeployaThree-TierApplicationonAWSEKSwithKubernetes_2.png)

세 개의 서비스가 모두 존재합니다

![image](/assets/img/2024-08-21-HowtoContainerizeandDeployaThree-TierApplicationonAWSEKSwithKubernetes_3.png)

<div class="content-ad"></div>

Ingress가 성공적으로 조정되었습니다

![image1](/assets/img/2024-08-21-HowtoContainerizeandDeployaThree-TierApplicationonAWSEKSwithKubernetes_4.png)

mern 네임스페이스의 모든 리소스를 확인하기 위해 명령을 실행할 수 있습니다

![image2](/assets/img/2024-08-21-HowtoContainerizeandDeployaThree-TierApplicationonAWSEKSwithKubernetes_5.png)

<div class="content-ad"></div>

당신이 좋아하는 브라우저에서 인그레스에 제공된 도메인에 방문할 수 있어요. 다만 도메인 제공업체에서 레코드를 추가하고 이를 전파시켜야 해요.

애플리케이션이 배포되었어요.

![이미지 1](/assets/img/2024-08-21-HowtoContainerizeandDeployaThree-TierApplicationonAWSEKSwithKubernetes_6.png)

![이미지 2](/assets/img/2024-08-21-HowtoContainerizeandDeployaThree-TierApplicationonAWSEKSwithKubernetes_7.png)

<div class="content-ad"></div>


![image](/assets/img/2024-08-21-HowtoContainerizeandDeployaThree-TierApplicationonAWSEKSwithKubernetes_8.png)

# 결론

Kubernetes에서 세 단계 응용 프로그램을 배포하는 것은 보다 간단합니다. 이 안내서를 통해 응용 프로그램을 컨테이너화하고 EKS 클러스터에 배포하는 방법을 배웠습니다. 이 기본 지식은 당신이 나아가면서 더 복잡한 배포에 자신감을 가질 수 있도록 도와줄 것입니다.

GitHub의 이 프로젝트 저장소- https://github.com/AmanPathak-DevOps/MERN-Stack-Project


<div class="content-ad"></div>

LinkedIn에서 계속 연결하고 싶다면: [LinkedIn Profile](LinkedIn 프로필 링크)

GitHub를 통해 최신 소식을 받아보세요: [GitHub Profile](GitHub 프로필 링크)

DevOps 및 클라우드에 관한 트렌드 기술을 논의하고 싶은가요?
Discord 서버 참여하기 - [Discord Server](https://discord.gg/jdzF8kTtw2)

다른 궁금한 사항이 있으시면 언제든지 연락주세요.

<div class="content-ad"></div>

즐거운 학습 되세요!