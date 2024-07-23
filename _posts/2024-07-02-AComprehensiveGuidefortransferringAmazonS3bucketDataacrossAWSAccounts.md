---
title: "AWS 계정 간 Amazon S3 버킷 데이터를 전송하는 포괄적인 가이드"
description: ""
coverImage: "/assets/img/2024-07-02-AComprehensiveGuidefortransferringAmazonS3bucketDataacrossAWSAccounts_0.png"
date: 2024-07-02 23:28
ogImage: 
  url: /assets/img/2024-07-02-AComprehensiveGuidefortransferringAmazonS3bucketDataacrossAWSAccounts_0.png
tag: Tech
originalTitle: "A Comprehensive Guide for transferring Amazon S3 bucket Data across AWS Accounts"
link: "https://medium.com/readytowork-org/a-comprehensive-guide-for-transferring-amazon-s3-bucket-data-across-aws-accounts-9c9f7325bcbe"
---


![Image](/assets/img/2024-07-02-AComprehensiveGuidefortransferringAmazonS3bucketDataacrossAWSAccounts_0.png)

# 개요

- AWS S3 간단 소개
- 계정 및 지역 간 데이터 이전의 중요성 및 사용 사례
- 허용 권한 및 비용을 고려해야 하는 단계별 가이드

# 전제 조건

<div class="content-ad"></div>

- AWS 계정 설정 (원본 및 대상 계정)
- IAM 역할 및 권한 (루트 사용자가 아닌 경우 충분한 권한)

## 소개

Amazon S3 (Simple Storage Service)은 어디서든 데이터를 저장하고 검색할 수 있는 확장 가능한 객체 저장 서비스입니다. 고 내구성, 보안 및 유연한 데이터 관리 기능을 제공하여 백업, 아카이빙, 대용량 데이터 분석 및 콘텐츠 배포에 이상적입니다.

## 중요성:

<div class="content-ad"></div>

- 원가 최적화: 데이터를 저장 비용이 낮은 지역으로 이전하거나 다른 계정으로 이동하여 원가를 더 효율적으로 관리하세요.

- 재해 복구: 데이터 중복성을 보장하고 여러 지역에 사본을 저장하여 복구 옵션을 확보하세요.

- 규정 준수 및 데이터 소유지: 데이터를 특정 지리적 범위 내에 유지함으로써 규제 요구 사항을 준수하세요.

- 성능 최적화: 데이터를 사용자나 응용프로그램에 더 가깝게 배치하여 지연 시간을 줄이세요.

## 활용 사례:

- 합병 및 인수: 다른 계정에서 데이터를 통합하세요.

- 지리적 확장: 전 세계적인 운영을 지원하기 위해 여러 지역에 데이터를 복제하세요.

- 계정 간 협업: 다른 비즈니스 부서나 파트너 간에 안전하게 데이터를 공유하세요.

- 데이터 생명주기 관리: 오래된 데이터를 보관 목적으로 다른 계정으로 이동하세요.

더 많은 정보는 AWS S3 문서를 참조하세요.

<div class="content-ad"></div>

# 스텝별 마이그레이션 가이드

## 스텝 1: 소스 계정에서 목적지 버킷 액세스용 DataSync IAM 역할 생성

- DataSync 작업 및 S3 읽기 액세스를 허용하는 정책을 가진 IAM 역할을 생성합니다.

이동: [AWS IAM 콘솔](https://console.aws.amazon.com/iam/)

<div class="content-ad"></div>

- 새 역할을 생성합니다.
- 서비스로 DataSync를 선택하면 필요한 권한이 자동으로 부여됩니다.
- 계속 진행하고 역할에 이름을 지어주고 역할을 생성합니다.

![Step 1](/assets/img/2024-07-02-AComprehensiveGuidefortransferringAmazonS3bucketDataacrossAWSAccounts_1.png)

## 단계 2: 대상 계정에서 S3 버킷 정책 업데이트

- 대상 버킷 정책을 업데이트하여 소스 계정의 DataSync 역할이 대상 버킷에 액세스할 수 있도록 합니다. 그렇지 않으면 소스 버킷이 대상 버킷에 액세스할 수 없습니다. 이를 달성하려면 다음 정책을 포함한 버킷 정책을 업데이트하십시오.

<div class="content-ad"></div>

```js
{
  "Version": "2008-10-17",
  "Statement": [
    {
      "Sid": "DataSyncCreateS3LocationAndTaskAccess",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::source-account:role/source-datasync-role"
      },
      "Action": [
        "s3:GetBucketLocation",
        "s3:ListBucket",
        "s3:ListBucketMultipartUploads",
        "s3:AbortMultipartUpload",
        "s3:DeleteObject",
        "s3:GetObject",
        "s3:ListMultipartUploadParts",
        "s3:PutObject",
        "s3:GetObjectTagging",
        "s3:PutObjectTagging"
      ],
      "Resource": [
        "arn:aws:s3:::destination-bucket",
        "arn:aws:s3:::destination-bucket/*"
      ]
    }
  ]
}
```

여기서 다음을 번역해 주세요:

source-account - 프로필 오른쪽 상단에서 확인할 수 있는 소스 계정 ID

source-datasync-role - 단계 1에서 생성한 역할 이름


<div class="content-ad"></div>

**destination-bucket** - `대상 버킷 이름`

![Link Text](/assets/img/2024-07-02-AComprehensiveGuidefortransferringAmazonS3bucketDataacrossAWSAccounts_2.png)

## Step 3: 대상 계정에서 S3 버킷의 ACL 비활성화

대상 버킷 설정에서 S3 버킷의 ACL을 비활성화합니다.

<div class="content-ad"></div>

- 버킷의 상세 페이지에서 권한 탭을 선택하세요.
- Object Ownership 아래에서 편집을 선택하세요.
- 이미 선택되어 있지 않다면 ACL 비활성화(권장) 옵션을 선택하세요.

![이미지](/assets/img/2024-07-02-AComprehensiveGuidefortransferringAmazonS3bucketDataacrossAWSAccounts_3.png)

또는 CloudShell에서 다음 명령어를 실행하여 이 작업을 수행할 수 있습니다. 여기서 'destination-bucket'을 자신의 버킷 이름으로 대체하세요.

```js
aws s3api put-bucket-acl --bucket destination-bucket --acl bucket-owner-full-control
```

<div class="content-ad"></div>

## Step 4: 원본 계정에서 데이터 동기화 위치 만들기

- 원본 및 대상 버킷을 위한 DataSync 위치를 만듭니다.

아래 링크를 클릭하여 DataSync 위치를 만드세요: [DataSync 시작 페이지](https://console.aws.amazon.com/datasync/home?#/getStarted)

<div class="content-ad"></div>

그럼 새로운 소스 위치를 만들고 선택한 후, 클라우드 쉘에서 목적 위치를 만들어야 합니다.  

![이미지 1](/assets/img/2024-07-02-AComprehensiveGuidefortransferringAmazonS3bucketDataacrossAWSAccounts_5.png)  

![이미지 2](/assets/img/2024-07-02-AComprehensiveGuidefortransferringAmazonS3bucketDataacrossAWSAccounts_6.png)  

![이미지 3](/assets/img/2024-07-02-AComprehensiveGuidefortransferringAmazonS3bucketDataacrossAWSAccounts_7.png)  

<div class="content-ad"></div>

이제는 다양한 계정 간 위치 필요로 인해 DataSync 콘솔 인터페이스를 통해 목적지 위치를 생성하고 선택할 수 없습니다. 따라서 CloudShell을 사용하여 이를 수행하고 콘솔 인터페이스에서 선택합니다.

```js
aws datasync create-location-s3 \
  --s3-bucket-arn arn:aws:s3:::destination-bucket \
  --region destination-bucket-region
  --s3-config '{
    "BucketAccessRoleArn":"arn:aws:iam::source-account-id:role/source-datasync-role"
  }'
```

다시 한 번 다음 자격 증명으로 대체하십시오:

목적지 버킷 - `목적지 버킷 이름`

<div class="content-ad"></div>

이미지 태그를 마크다운 형식으로 변경하겠습니다.


source-account -` 오른쪽 상단 프로필을 가리키면 찾을 수 있는 소스 계정 ID입니다.

source-datasync-role -` 우리가 단계 1에서 생성한 역할 이름

성공하면 아래와 유사한 결과가 나타나며 위치가 생성될 것입니다:

```js
{
  "LocationArn": "arn:aws:datasync:us-east-2:123456789012:location/loc-abcdef01234567890"
}
```

<div class="content-ad"></div>


![Step 5: In Your Source Account, Create and Start Your DataSync Task](/assets/img/2024-07-02-AComprehensiveGuidefortransferringAmazonS3bucketDataacrossAWSAccounts_8.png)

Give your Task a name and in most cases default configuration must be fine, you can tweak some settings as per your need.

![DataSync Task Creation](/assets/img/2024-07-02-AComprehensiveGuidefortransferringAmazonS3bucketDataacrossAWSAccounts_9.png)


<div class="content-ad"></div>


![image](/assets/img/2024-07-02-AComprehensiveGuidefortransferringAmazonS3bucketDataacrossAWSAccounts_10.png)

Review the configuration and then create a task.

In some cases, you may encounter an error saying "permission denied" or something similar. This could occur if your bucket data is encrypted. In such situations, additional permissions need to be granted to the IAM role being used.

To address this issue, navigate to the IAM role we created, then add an inline policy to its permissions. Select S3 & KMS permissions, which are crucial for accessing and reading encrypted bucket objects.


<div class="content-ad"></div>

![image](/assets/img/2024-07-02-AComprehensiveGuidefortransferringAmazonS3bucketDataacrossAWSAccounts_11.png)

If you don't want to grant full permissions, the following permissions should suffice:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:ListBucket",
                "kms:Decrypt",
                "kms:GenerateDataKey"
            ],
            "Resource": [
                "arn:aws:s3:::source-bucket-name",
                "arn:aws:s3:::source-bucket-name/*"
            ]
        }
    ]
}
```

After setting up the permissions, you can proceed to create and start the task for transferring your data.

<div class="content-ad"></div>


![이미지](/assets/img/2024-07-02-AComprehensiveGuidefortransferringAmazonS3bucketDataacrossAWSAccounts_12.png)

이제 데이터 전송 작업을 시작하고 CloudWatch를 통해 로그를 모니터링할 수 있습니다.

모든 설정을 마치셨습니다. 이제 작업을 시작하고 로그를 확인할 수 있으며, 성공적인 전송 후 "성공" 상태 메시지가 표시됩니다.

![이미지](/assets/img/2024-07-02-AComprehensiveGuidefortransferringAmazonS3bucketDataacrossAWSAccounts_13.png)


<div class="content-ad"></div>

축하해요 🎉, 이제 목적지 계정에서 모든 S3 데이터에 접근할 수 있어요.