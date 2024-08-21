---
title: "루비온레일을 Docker로 만들어  Github Actions 활용해 AWS ECR에 배포하기"
description: ""
coverImage: "/assets/img/2024-08-21-MypersonalexperienceDockerizingaRubyonRailsApplicationDeploytoAWSECRusingGithubActions_0.png"
date: 2024-08-21 18:04
ogImage: 
  url: /assets/img/2024-08-21-MypersonalexperienceDockerizingaRubyonRailsApplicationDeploytoAWSECRusingGithubActions_0.png
tag: Tech
originalTitle: "My personal experience Dockerizing a Ruby on Rails Application Deploy to AWS ECR using Github Actions"
link: "https://medium.com/@igeadetokunbo/my-personal-experience-dockerizing-a-ruby-on-rails-application-deploy-to-aws-ecr-using-github-55927034362e"
isUpdated: true
updatedAt: 1724245667579
---


Dockerizing a Ruby on Rails application is a common request many developers have after reading about Dockerizing a Rust application. By containerizing a Ruby on Rails application, you can improve scalability, maintain consistency, isolate dependencies, and streamline deployment, which are all essential for modern development practices.

![Dockerizing a Ruby on Rails Application](/assets/img/2024-08-21-MypersonalexperienceDockerizingaRubyonRailsApplicationDeploytoAWSECRusingGithubActions_0.png)

Ruby on Rails is a robust framework with a vibrant ecosystem and strong community support, making it an excellent choice for web application development. When combined with Docker, developers can accelerate the process of building, deploying, and scaling web applications while ensuring speed and reliability.

The pairing of Ruby on Rails and Docker forms a solid base for web application development. Rails provides a feature-rich framework for coding, and Docker enables consistent application performance across different environments. Together, they offer a streamlined workflow for modern web development.

<div class="content-ad"></div>

아래는 Ruby on Rails 애플리케이션을 Docker 환경으로 구성하고 이를 AWS ECR로 배포하는 방법을 단계별로 안내한 가이드입니다.

## 단계 1: 환경 설정하기: Ruby on Rails, Docker 및 Docker-Compose 설치

Ruby on Rails를 사용하려면 먼저 Ruby를 설치해야 합니다. 이미 설치되어 있지 않다면 개발 환경에 Ruby, Docker Compose 및 Docker가 실행 중이어야 합니다.

- Ruby: Ruby를 설치하려면 다음 링크를 따라가세요. 다양한 운영 체제나 환경에 Ruby를 설치하는 방법이 설명되어 있습니다.
- Docker: Docker와 Docker Compose를 설치하려면 다음 링크를 따라가세요. 다양한 운영 체제에 Docker를 설치하는 방법이 설명되어 있습니다. Ruby를 성공적으로 설치한 후에는 Rails를 설치해야 합니다.

<div class="content-ad"></div>

아래 명령어를 사용하여 Rails를 설치할 수 있어요.

```shell
gem install rails
```

Rails가 성공적으로 설치되었는지 확인하려면 아래 명령어를 입력해보세요.

```shell
rails -v
```

<div class="content-ad"></div>

## 단계 2: 프로젝트 설정하기. 루비온레일즈에서 할일 앱

다음으로, 할일 앱이 될 새로운 레일즈 애플리케이션을 생성하세요. 아래 명령어를 실행하세요.

```js
rails new todo-app -d postgresql
cd todo-app
```

위 명령어를 실행한 후 todo-app이라는 폴더가 생성되며, 해당 폴더에는 레일즈 코드가 포함될 것입니다. 또한, -d postgresql 플래그는 PostgreSQL을 기본 데이터베이스로 설정합니다.

<div class="content-ad"></div>

## 단계 3: 데이터베이스 구성 설정하기

위의 플래그를 사용하여 명령을 실행한 후에는 데이터베이스 구성이 config/database.yml 위치에 생성됩니다. 이 파일에는 데이터베이스 연결 문자열에 대한 정보가 포함됩니다.

```js
default: &default
  adapter: postgresql
  encoding: unicode
  host: <%= ENV['DATABASE_HOST'] %>
  username: <%= ENV['DATABASE_USER'] %>
  password: <%= ENV['DATABASE_PASSWORD'] %>
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>

development:
  <<: *default
  database: todo_app_development

test:
  <<: *default
  database: todo_app_test

production:
  <<: *default
  database: todo_app_production
```

## 단계 4: 젬 설치

<div class="content-ad"></div>

아래 명령을 실행하여 필요한 모든 젬을 설치하세요.

### 젬이란 무엇인가요?

```js
bundle install
```

## 단계 5: 할 일 스캐폴드 생성

<div class="content-ad"></div>

아래 명령어를 사용하여 Todo 앱에 대한 기본 뼈대 코드를 생성해보세요.

```js
rails generate scaffold Todo title:string completed:boolean
```

## 단계 6: 데이터베이스 마이그레이션

아래 마이그레이션 명령어를 실행하여 데이터베이스 스키마를 설정해보세요.

<div class="content-ad"></div>

```js
rails db:create db:migrate
```

## Step 7: 도커 파일 생성하기

루비 온 레일 애플리케이션을 컨테이너화하기 위해 Dockerfile을 작성해야 합니다. 프로젝트의 루트 디렉토리에 Dockerfile이라는 파일을 생성하세요. 이 파일에는 애플리케이션의 도커 이미지를 빌드하는 방법에 대한 지시사항이 포함될 것입니다.

새로 생성한 Dockerfile 파일을 열어서 아래의 코드를 파일에 복사하여 붙여넣으세요.

<div class="content-ad"></div>

```js
# Use an official Ruby runtime as a parent image
FROM ruby:3.2.2

# Install dependencies
RUN apt-get update -qq && apt-get install -y nodejs postgresql-client

# Set the working directory
WORKDIR /app

# Copy the Gemfile and Gemfile.lock into the container
COPY Gemfile Gemfile.lock /app/

# Install gems
RUN bundle install

# Copy the rest of the application code
COPY . /app

# Precompile assets for production
RUN RAILS_ENV=production bundle exec rake assets:precompile

# Expose port 3000 for the Rails app
EXPOSE 3000

# Start the Rails server
CMD ["rails", "server", "-b", "0.0.0.0"]
```

## 단계 8: .dockerignore 파일 만들기

Docker 이미지에 불필요한 파일이 추가되는 것을 방지하기 위해 루트 디렉토리에 새 파일을 만들어야 합니다. 파일 이름은 dockerignore로 하세요. .dockerignore 파일을 추가함으로써 도커 이미지의 크기를 줄이고 필요한 파일만 남게 됩니다.

새로 만든 파일 .dockerignore을 열고 아래 코드를 복사하여 붙여넣기하세요. 더 필요한 파일을 더 추가할 수 있습니다. 이것은 예시일 뿐입니다.

<div class="content-ad"></div>

```js
로그/*
임시/*
node_modules/*
.git
```

## 단계 9: 도커 컴포즈 설정

도커 컴포즈를 사용하면 루비 온 레일즈 애플리케이션 및 PostgreSQL 데이터베이스와 같은 종속 항목을 쉽게 실행할 수 있습니다. 이를 통해 데이터베이스 연결 문자열을 편리하게 구문 분석할 수 있습니다.

루트 디렉토리에 filenamedocker-compose.yml 파일을 생성합니다. 아래 코드를 새로 만든 docker-compose.yml 파일에 복사하여 붙여넣으세요.

<div class="content-ad"></div>


```yaml
version: '3.8'
services:
  db:
    image: postgres:13
    volumes:
      - db_data:/var/lib/postgresql/data
    environment:
      POSTGRES_PASSWORD: password
      POSTGRES_USER: postgres
      POSTGRES_DB: todo_app_development

  web:
    build: .
    command: bash -c "rm -f tmp/pids/server.pid && rails server -b 0.0.0.0"
    volumes:
      - .:/app
    ports:
      - "3000:3000"
    depends_on:
      - db
    environment:
      DATABASE_HOST: db
      DATABASE_USER: postgres
      DATABASE_PASSWORD: password
      DATABASE_NAME: todo_app_development

volumes:
  db_data:
```

## Step 10: Run the Ruby on Rails Application with Docker Compose

아래 명령을 실행하면 Docker 이미지를 빌드하고 PostgreSQL 데이터베이스를 시작하고 Rails 서버를 실행합니다.

```shell
docker-compose up
```

<div class="content-ad"></div>

당신이 좋아하는 브라우저에서 http://localhost:3000을 통해 Ruby on Rails 애플리케이션에 접속할 수 있어요!