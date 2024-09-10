---
title: "Ruby on Rails AWS 서비스 활용하기 쉽고 빠른 시작 가이드 "
description: ""
coverImage: "/assets/img/2024-09-10-LeveragingAWSServiceswithRubyonRails_0.png"
date: 2024-09-10 19:15
ogImage: 
  url: /assets/img/2024-09-10-LeveragingAWSServiceswithRubyonRails_0.png
tag: Tech
originalTitle: "Leveraging AWS Services with Ruby on Rails "
link: "https://medium.com/@rajputlakhveer/leveraging-aws-services-with-ruby-on-rails-a456897bb378"
isUpdated: false
---


아마존 웹 서비스(AWS)는 컴퓨팅 파워, 저장, 데이터베이스, 머신 러닝 및 고급 데이터 분석까지 다양한 서비스를 제공하는 포괄적인 클라우드 컴퓨팅 플랫폼을 제공합니다. AWS 서비스를 Ruby on Rails과 통합하면 개발자들이 확장 가능하고 견고하며 비용 효율적인 애플리케이션을 구축할 수 있습니다.

본 블로그에서는 Ruby on Rails로 자주 사용되는 AWS 서비스들에 대해 자세히 살펴보고, 그 사용 사례를 강조하며 이를 Rails 앱에 통합하는 방법에 대한 예시를 제공하겠습니다. 시작해봅시다! 🔥

![이미지](/assets/img/2024-09-10-LeveragingAWSServiceswithRubyonRails_0.png)

아마존 S3는 언제든지 어떤 양의 데이터든 저장 및 검색할 수 있는 매우 확장 가능한 객체 저장 서비스입니다. Ruby on Rails 앱에서 S3는 일반적으로 이미지, 비디오 및 문서와 같은 에셋을 저장하는 데 사용됩니다.

<div class="content-ad"></div>

루비 온 레일즈와의 통합

먼저 Gemfile에 `aws-sdk-s3` 젬을 추가하세요:

```js
gem 'aws-sdk-s3', '~> 1.96'
```

다음으로, `config/initializers/aws_s3.rb`에 AWS S3를 위한 이니셜라이저를 생성하세요.

<div class="content-ad"></div>

```js
// Aws 구성 업데이트
Aws.config.update({
   region: 'us-west-2',
   credentials: Aws::Credentials.new(
   ENV['AWS_ACCESS_KEY_ID'],
   ENV['AWS_SECRET_ACCESS_KEY']
 )
})

// S3_BUCKET 설정
S3_BUCKET = Aws::S3::Resource.new.bucket(ENV['S3_BUCKET_NAME'])
```

예시: 파일 업로드

S3를 Rails ActiveStorage와 통합하거나 파일 업로드를 위해 `aws-sdk-s3` 젬을 직접 사용할 수 있습니다.

```js
// 파일 및 S3 객체 업로드
file = params[:file]
s3_object = S3_BUCKET.object("uploads/#{file.original_filename}")
s3_object.upload_file(file.path, acl: 'public-read')
```

<div class="content-ad"></div>

간단한 예제를 통해 Rails 앱에서 직접 S3로 파일을 업로드할 수 있습니다.

Amazon EC2는 클라우드에서 확장 가능한 컴퓨팅 용량을 제공합니다(간편한 클라우드 컴퓨터). Rails 애플리케이션은 EC2 인스턴스에 배포할 수 있어 서버 환경을 완전히 제어할 수 있습니다.

예: EC2에 Rails 앱 배포하기

1. EC2 인스턴스 시작: Ubuntu 또는 Amazon Linux 인스턴스 선택.
2. 인스턴스에 Ruby 및 Rails 설치하기.
3. 웹 서버로 Nginx와 Passenger 설치하기.
4. Rails 구성: 인스턴스에 Rails 애플리케이션 및 데이터베이스 설정하기.

<div class="content-ad"></div>

EC2 인스턴스에서 Ruby와 Rails를 설정하는 기본 스크립트입니다:

```bash
sudo apt update
sudo apt install -y curl gpg
curl -sSL https://rvm.io/mpapis.asc | gpg --import -
curl -sSL https://get.rvm.io | bash -s stable
source ~/.rvm/scripts/rvm
rvm install 3.1.2 # 원하는 Ruby 버전 설치
gem install rails
```

Ruby와 Rails를 설치한 후에는 앱 저장소를 복제하고 프로덕션 환경에 설정할 수 있습니다.

Amazon RDS를 사용하면 클라우드에서 관계형 데이터베이스를 쉽게 설정, 운영 및 확장할 수 있습니다. PostgreSQL, MySQL, MariaDB와 같은 데이터베이스를 사용할 수 있으며, 이들은 Ruby on Rails와 자주 함께 사용됩니다.

<div class="content-ad"></div>

예시: Rails와 RDS 연결하기

`database.yml` 파일을 수정하여 RDS 인스턴스에 연결하세요:

```js
production:
  adapter: postgresql
  encoding: unicode
  pool: 5
  database: my_database
  username: <%= ENV['RDS_USERNAME'] %>
  password: <%= ENV['RDS_PASSWORD'] %>
  host: <%= ENV['RDS_HOST'] %>
  port: 5432
```

환경 변수를 사용하여 사용자 이름, 비밀번호 및 호스트와 같은 중요 정보를 보호하세요.

<div class="content-ad"></div>

AWS Lambda는 서버를 프로비저닝하거나 관리하지 않고 코드를 실행할 수 있는 기능을 제공합니다. 람다는 마이크로서비스나 전용 서버가 필요하지 않은 작은 작업에 적합합니다. 예를 들어, Rails 앱에서 백그라운드 작업을 수행할 때 유용합니다.

예시: 백그라운드 작업에 람다 사용하기

이메일을 보내거나 보고서를 비동기적으로 생성하려고 할 때, 이 작업을 AWS 람다에 위임하여 Rails 앱에서 API 게이트웨이를 통해 람다 함수를 트리거할 수 있습니다.

```js
client = Aws::Lambda::Client.new(region: 'us-west-2')
client.invoke({
 function_name: "send_email_function",
 invocation_type: "Event",
 payload: { user_id: 123, email: 'test@example.com' }.to_json
})
```

<div class="content-ad"></div>

람다 함수는 다양한 작업을 처리할 수 있어서 레일즈 앱을 무난하게 확장할 수 있어요.

아마존 클라우드프론트는 전 세계적으로 낮은 대기 시간으로 자산(이미지, CSS, JS 파일)을 제공할 수 있는 콘텐츠 전송 네트워크(CDN)입니다. 클라우드프론트를 사용하여 레일즈 앱의 정적 콘텐츠를 캐싱하고 전달 시간을 단축할 수 있어요.

예시: 클라우드프론트를 통한 자산 제공

`config/environments/production.rb` 파일에서 클라우드프론트에서 자산을 제공하도록 구성하세요:

<div class="content-ad"></div>

```js
config.action_controller.asset_host = 'https://d1234567890.cloudfront.net'
```

이 설정은 CloudFront를 통해 자산을 제공하여 애플리케이션의 속도와 성능을 향상시킬 것입니다.

Elastic Beanstalk는 Rails 앱을 실행하는 데 필요한 인프라를 자동으로 관리하는 플랫폼 서비스(PaaS)입니다. Beanstalk을 사용하면 서버나 확장에 대해 걱정할 필요 없이 코드 작성에 집중할 수 있습니다.

예: Elastic Beanstalk에 Rails 앱 배포하기

<div class="content-ad"></div>

1. EB CLI 설치: pip install awsebcli

2. Beanstalk 환경 생성:
eb init
eb create

Elastic Beanstalk은 앱을 배포하고 서버를 프로비저닝하며 로드 밸런싱 등을 관리해주어 레일즈 개발자에게 강력한 도구로 작용합니다.

Amazon SNS는 SMS, 이메일 또는 푸시 알림을 통해 알림을 보내는 데 사용됩니다. 레일즈 앱에서 사용자 알림, 오류 경고 또는 마케팅 메시지를 보내는 데 최적입니다.

<div class="content-ad"></div>

예시: SNS를 이용하여 이메일 알림 보내기

```js
sns = Aws::SNS::Client.new(region: 'us-west-2')
sns.publish({
  topic_arn: 'arn:aws:sns:us-west-2:123456789012:my-topic',
  message: '주문이 발송되었습니다!',
  subject: '주문 업데이트'
})
```

또한 사용자를 주제에 구독시켜 여러 플랫폼 간 대규모 통신을 가능하게 할 수 있습니다.

Amazon Redshift는 대규모 데이터 분석을 다루는 Rails 애플리케이션에 적합한 강력한 데이터 웨어하우징 솔루션입니다.

<div class="content-ad"></div>

예시: 레일즈와 Redshift 통합

Redshift에 다른 데이터베이스와 마찬가지로 연결합니다. Redshift의 빠른 속도와 확장성은 대규모 데이터셋에 대한 복잡한 분석 쿼리를 실행하기에 이상적입니다.

AWS CodeDeploy는 EC2를 포함한 모든 인스턴스에 코드 배포를 자동화하여 레일즈 앱을 쉽게 업데이트하고 다운타임 없이 유지할 수 있도록합니다.

예시: 지속적 배포를 위한 CodeDeploy 사용

<div class="content-ad"></div>

1. EC2 인스턴스에 CodeDeploy 에이전트를 설치하세요.
2. 새 코드가 저장소에 푸시될 때마다 CodeDeploy를 구성하여 Rails 앱에 업데이트를 자동으로 배포하도록 설정하세요.

이를 통해 수동 개입 없이 항상 최신 버전의 Rails 앱을 유지할 수 있습니다.

AWS는 루비 온 레일과 함께 사용하기에 적합한 다양한 서비스를 제공하여 확장 가능하고 견고하며 비용 효율적인 애플리케이션을 구축할 수 있습니다. 파일 저장을 위해 S3, 데이터베이스를 위해 RDS, 또는 백그라운드 작업을 위해 Lambda를 사용하더라도 AWS를 Rails 앱에 통합하면 끝없는 가능성을 열게 됩니다.

AWS의 파워를 활용하여 운영 인프라 관리를 AWS가 처리하면서 멋진 기능 개발에 집중할 수 있습니다.

<div class="content-ad"></div>

행복한 코딩하세요! ✨