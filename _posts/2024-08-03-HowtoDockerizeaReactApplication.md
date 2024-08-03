---
title: "React 애플리케이션을 Docker로 배포하는 방법"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-03 19:26
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "How to Dockerize a React Application"
link: "https://dev.to/sh20raj/how-to-dockerize-a-react-application-19kc"
---


## React 애플리케이션을 도커화하는 방법

React 애플리케이션을 도커화하면 개발 워크플로우를 간소화할 뿐만 아니라, 개발의 다양한 단계에서 일관된 환경을 제공하고 배포 프로세스를 간단하게 만들 수 있습니다. 본 안내서는 도커 환경 설정부터 Docker 이미지 빌드 및 실행까지 React 애플리케이션을 도커화하는 단계를 안내합니다.

### 준비물

- 도커: 컴퓨터에 Docker가 설치되어 있는지 확인하세요. Docker의 공식 웹사이트에서 다운로드할 수 있습니다.
- React 애플리케이션: create-react-app이나 다른 방법을 사용하여 생성된 React 애플리케이션이 있어야 합니다. 없는 경우 create-react-app을 사용하여 기본 앱을 생성할 수 있습니다.

<div class="content-ad"></div>

```js
npx create-react-app my-react-app
cd my-react-app
```

### Step 1: 도커 파일 생성

도커 파일은 애플리케이션을 위해 도커 이미지를 빌드하는 방법에 대한 일련의 명령을 포함하는 스크립트입니다. React 애플리케이션의 루트 디렉토리에 다음 내용을 포함하여 Dockerfile이라는 파일을 생성하십시오:

```js
# 공식 Node 런타임을 부모 이미지로 사용
FROM node:20-alpine

# 작업 디렉토리 설정
WORKDIR /app

# package.json 및 package-lock.json 파일을 작업 디렉토리로 복사
COPY package*.json ./

# 종속성 설치
RUN npm install

# 나머지 애플리케이션 코드를 작업 디렉토리로 복사
COPY . .

# React 앱 빌드
RUN npm run build

# React 앱을 제공하기 위해 간단한 서버 설치
RUN npm install -g serve

# 서버 실행 명령 설정
CMD ["serve", "-s", "build"]

# 포트 3000 노출
EXPOSE 3000
```

<div class="content-ad"></div>

### 단계 2: .dockerignore 파일 생성하기

.dockerignore 파일은 파일을 Docker 이미지로 복사할 때 무시해야 할 파일 및 디렉토리를 지정합니다. 이를 통해 이미지 크기를 줄이고 빌드 프로세스를 가속화할 수 있습니다. 다음 내용으로 루트 디렉토리에 .dockerignore 파일을 생성하세요:

```js
node_modules
build
.dockerignore
Dockerfile
.git
.gitignore
```

### 단계 3: Docker 이미지 빌드하기

<div class="content-ad"></div>

React 애플리케이션을 위한 Docker 이미지를 생성하려면, 애플리케이션의 루트 디렉토리로 이동한 다음 다음 명령어를 실행하세요:

```js
docker build -t my-react-app .
```

이 명령은 Docker에게 현재 디렉토리(.)를 컨텍스트로 사용하여 my-react-app 태그로 이미지를 빌드하라고 알려줍니다.

### 단계 4: Docker 컨테이너 실행

<div class="content-ad"></div>

Docker 이미지가 만들어지면 다음 명령어를 사용하여 컨테이너에서 실행할 수 있습니다:

```js
docker run -p 3000:3000 my-react-app
```

이 명령어는 로컬 머신의 포트 3000을 컨테이너 내의 포트 3000에 매핑하여 브라우저에서 http://localhost:3000을 통해 React 애플리케이션에 액세스할 수 있게 합니다.

### 단계 5: Docker Compose (선택사항)

<div class="content-ad"></div>

여러 컨테이너를 관리하거나 더 많은 구성을 추가하려면 Docker Compose를 사용할 수 있어요. 다음 내용을 포함하는 루트 디렉토리에 docker-compose.yml 파일을 생성해 보세요:

```js
version: '3'

services:
  react-app:
    build: .
    ports:
      - "3000:3000"
```

docker-compose.yml 파일에 정의된 서비스를 시작하려면 다음 명령어를 실행해 주세요:

```js
docker-compose up
```

<div class="content-ad"></div>

### 결론

위 단계를 따라하셨다면 React 애플리케이션을 성공적으로 Docker화했습니다. 애플리케이션을 Docker화함으로써 서로 다른 환경 간의 일관성을 보장할 뿐만 아니라 배포 프로세스를 간소화하여 애플리케이션을 보다 쉽게 관리하고 확장할 수 있게 됩니다.

### 추가 자료

- Docker 문서
- React 문서
- Docker Compose 문서

<div class="content-ad"></div>

프로젝트의 특정 요구에 맞게 Dockerfile 및 Docker Compose 구성을 자유롭게 사용자 정의해 주세요. 즐거운 Docker 활용 되세요!

https://t.me/boost/sopbots