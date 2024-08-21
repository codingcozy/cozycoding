---
title: "AWS에서 SSH 사용 중지 개발보안운영 (DevSecOps) 정리"
description: ""
coverImage: "/assets/img/2024-08-21-StopusingSSHinAWSHeresWhyADevSecOpsPerspective_0.png"
date: 2024-08-21 17:56
ogImage: 
  url: /assets/img/2024-08-21-StopusingSSHinAWSHeresWhyADevSecOpsPerspective_0.png
tag: Tech
originalTitle: "Stop using SSH in AWS Heres Why A DevSecOps Perspective"
link: "https://medium.com/aws-tip/stop-using-ssh-in-aws-heres-why-a-devsecops-perspective-a320a14612f8"
isUpdated: true
updatedAt: 1724245367601
---


## 보안 | AWS | EC2

# 무엇을 해결하려고 하는 걸까요?

어디서 10.0.0.0/8 또는 더 나쁜 경우인 0.0.0.0/0에서 오픈된 포트 22를 사용하는 보안 그룹을 몇 번이나 보았습니까? 너무 많이 봤습니다! 그런데 왜, 왜 우리가 더 나은 대안이 있는 2024년에도 SSH를 사용하고 있을까요? 보안 전문가로서 내가 종종 “더 나은 작업 방법”을 설득하려고 하지만 자주 실패합니다. 사람들은 빠르고 쉬운 작업 방식을 좋아하고 이해합니다! 그러나 보안 사고가 계속 증가하는 시대에 우리는 설계 상 보안이 내장된 더 나은 작업 방식을 찾아야 합니다.

게다가 YouTube에서 기술 동영상을 보면 SSH가 "내 IP"로 열린 공개 EC2 인스턴스를 종종 볼 수 있습니다. 이렇게 하는 콘텐츠 제작자를 나 비난하지는 않습니다. 누군가에게 특정 작업을 어떻게 수행하는지 보여주고 싶어하기 때문입니다. 그러나 많은 개발자들은 이것이 유일한 작업 방법이라고 가정하기 때문에 보안 전문가가 그들이 EC2에 연결하는 방법을 변경해야 한다고 말하면 종종 저항을 보입니다.

<div class="content-ad"></div>

안녕하세요! EC2 인스턴스에 안전한 액세스를 제공하는 방법을 알아보려고 하시나요? 키페어나 퍼블릭 IP 주소, 잘못 구성된 보안 그룹 없이, 더 나은 로깅과 보안을 누리는 방법으로요!

이에 대한 해답은 무엇일까요? 시스템 관리자 및 세션 관리자입니다.

# 시스템 관리자 (SSM)란?

시스템 관리자가 제공하는 일부 서비스 중 일부는 이와 같습니다:

<div class="content-ad"></div>

- 패치 관리
- 소프트웨어 배포
- EC2 인벤토리
- 설정 관리
- 세션 관리자

# 세션 관리자란?

# SSH 대신 세션 관리자를 사용하는 이유:

- IAM 정책을 사용한 관리된 노드에 대한 중앙 집중식 액세스 제어
- 보안 그룹에서 수신 포트 22 또는 3389(Windows)이 열리지 않음
- 핑거프린트 호스트나 SSH 키를 관리할 필요 없음
- 세션 활동 로깅 및 감사 기록

<div class="content-ad"></div>

로그 기능은 두 가지 수준에서 작동합니다. 먼저 모든 세션은 AWS CloudTrail에 기록되어 활동을 다른 AWS 서비스 및 이벤트 유형과 일관되게 감사할 수 있습니다. 이는 세션이 활성화된 상태에서 수행된 활동에 부인 방지 기능을 제공합니다.

두 번째로, 키 스트로크를 CloudWatch 또는 Amazon S3와 같은 중앙 집중식 로깅 시설로 캡처할 수 있습니다. 따라서 EC2 인스턴스를 통해 터미널에서 수행된 모든 작업을 기록해야 하는 특정 규정 요구 사항이 있으면 그러한 작업을 할 수 있습니다. 또한 보안 운영팀이 사건 조사를 수행할 때 알려진 의심스러운 활동을 기반으로 경고를 생성할 수 있기 때문에 매우 유익합니다. 예를 들어 권한 상승과 같은 활동을 대상으로 합니다.

# 솔루션 아키텍처 개요

다중 계정 환경을 위한 설계는 암호화 키를 포함할 때 간단하지 않습니다. 그러나 계정 프로비저닝 프로세스 중에 아래에 설명된 구성을 자동화하여 복잡성을 줄일 수 있습니다. 아래 설계는 개발자나 플랫폼 엔지니어에게 장애물을 만들지 않고 SSH 문제를 해결하는 방법을 명확히 설명해줄 것입니다.

<div class="content-ad"></div>

## 로깅 계정 (S3 및 CloudWatch)

로깅 계정은 보안 조직 단위(OU)에 있습니다. S3 버킷과 CloudWatch 로그 그룹이 설정되어 있으며 KMS 키를 사용하여 암호화됩니다.

## VPC 엔드포인트 (VPCe)

작업 부서 OU의 각 AWS 계정에는 VPC 엔드포인트가 구성되어 있어 트래픽을 AWS 클라우드 내부로 유지합니다. 이를 통해 인터넷을 통해 외부로 나가지 않고 VPCe를 통해 S3 버킷 및 CloudWatch 로그 그룹에 액세스할 수 있습니다.

<div class="content-ad"></div>

# EC2 Instances

EC2 인스턴스는 CloudWatch 및 S3로 로그를 전송할 수 있는 사용자 지정 인스턴스 프로필로 구성되어 있습니다. 또한 KMS 키를 사용하여 암호화할 수 있는 권한을 부여받았습니다. 또한 AWS에서 관리하는 IAM 정책이 있어 Systems Manager의 작동을 가능하게 합니다. 이 IAM 정책은 인스턴스 프로필에도 연결되어 있습니다.

# Systems Manager & Session Manager

세션 매니저 환경 설정은 위에 언급한 구성이 저장되어 있으며, 동일한 S3 버킷 또는 CloudWatch 그룹이 사용되도록 보장합니다.

<div class="content-ad"></div>

## SIEM으로 로그 수집하기 (선택 사항)

보안 정보 및 이벤트 관리 시스템(SIEM)은 여러 소스에서 로그를 수집하여 악의적인 활동에 대한 분석 및 경보를 제공할 것입니다. CloudWatch에서 경보를 설정하는 대신, 보안 분석가가 SIEM 내에서 규칙을 구성하는 것이 더 쉬울 수 있습니다. 이는 비용을 절약하기 위해 CloudWatch 로그를 빠르게 만료시킬 수 있음을 의미하며, 결국 로그는 어차피 외부 시스템으로 소화될 것입니다.

# 세션 녹화 사전 조건

- SSM 에이전트가 설치된 Amazon Machine Image (AMI)를 사용하십시오 (Amazon Linux 2023에는 기본적으로 설치되어 있음).
- AWS의 가이드를 따라 Systems Manager를 구성하십시오 (일부 설정은 내 테라폼 구성에 있습니다).
- ec2messages, ssm, ssmmessages, kms, logs 및 s3에 대한 VPC 엔드포인트가 구성되었는지 확인하십시오. 또한, S3에 대한 게이트웨이 엔드포인트도 필요하다는 것을 알게 되었습니다.

<div class="content-ad"></div>

# 세션 관리자 로그 설정 및 사용 방법

## Terraform 구성

아래는 이 데모의 핵심 측면을 구성하는 데 사용한 Terraform입니다. GitHub에서 해당 Terraform을 찾을 수 있습니다.

## ec2.tf — EC2 인스턴스 및 관련 설정 구성

<div class="content-ad"></div>

이 명령은 EC2를 VPC에 생성하고 모든 IP 주소로 egress를 허용하는 보안 그룹을 사용합니다. 주의할 점은 인바운드 포트 22가 없다는 것입니다. 또한 CloudWatch, S3 및 KMS에 권한을 부여하는 IAM 정책이 있습니다. 이는 로그를 전송하고 올바른 암호화 키를 사용할 수 있도록 보장합니다.

다음으로, 역할을 생성하고 IAM 정책과 필수 AmazonSSMManagedInstanceCore 정책을 연결합니다. (이는 AWS에서 관리하는 정책으로, SSM 서비스가 EC2 인스턴스의 SSM 에이전트와 통신할 수 있도록 필요합니다).

마지막으로 인스턴스 프로필이 생성되어 IAM 역할에서 가져온 권한이 EC2에 연결됩니다.

```js
# EC2 인스턴스 생성
resource "aws_instance" "ssm_recording_example" {
  ami                    = data.aws_ami.amazon_linux_2023.id
  instance_type          = "t2.micro"
  iam_instance_profile   = aws_iam_instance_profile.ec2_ssm_instance_profile.name
  subnet_id              = data.aws_subnet.subnet.id
  vpc_security_group_ids = ["${aws_security_group.session_manager_security_group.id}"]
  metadata_options {
    http_endpoint               = "enabled"
    http_tokens                 = "required" # IMDSv2 사용 강제
    http_put_response_hop_limit = 1
  }
  tags = {
    Name        = "Session Manager Demo"
    Environment = "Development"
    Owner       = "Medium Article"
  }
}
# 보안 그룹 생성
resource "aws_security_group" "session_manager_security_group" {
  name        = "Session Manager Security Group"
  description = "Session Manager Security Group"
  vpc_id      = data.aws_vpc.main.id
  tags = {
    Name : "Session Manager Security Group"
  }
  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}
# EC2 IAM 정책 생성
resource "aws_iam_policy" "session_manager_recording_policy" {
  name        = "SSM-SessionManager-S3-KMS-Logging"
  description = "SSM Session Manager logging to S3 with KMS encryption"
  policy      = <<EOF
  {
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PutObjectsBucket",
      "Action": [
        "s3:PutObject",
        "s3:PutObjectAcl"
      ],
      "Effect": "Allow",
      "Resource": "${aws_s3_bucket.log_bucket.arn}/*"
    },
    {
      "Sid": "ListBucketAndEncryptionConfig",
      "Action": [
        "s3:GetEncryptionConfiguration"
      ],
      "Effect": "Allow",
      "Resource": "${aws_s3_bucket.log_bucket.arn}"
    },
    {
      "Sid": "S3KMSSessionManagerKMS",
      "Effect": "Allow",
      "Action": [
        "kms:Decrypt",
        "kms:GenerateDataKey*"
      ],
      "Resource": [
        "${aws_kms_key.log_key.arn}"
      ]
    },
        {
            "Sid": "cloudwatch",
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogStream",
                "logs:PutLogEvents",
                "logs:DescribeLogGroups",
                "logs:DescribeLogStreams"
            ],
            "Resource": "*"
        }
  ]
}
EOF
}
# IAM 역할 생성
resource "aws_iam_role" "ec2_ssm_role" {
  name = "ec2_ssm_role"
  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Effect = "Allow"
        Principal = {
          Service = "ec2.amazonaws.com"
        }
        Action = "sts:AssumeRole"
      }
    ]
  })
  managed_policy_arns = ["${aws_iam_policy.session_manager_recording_policy.arn}", "arn:aws:iam::aws:policy/AmazonSSMManagedInstanceCore"]
}
# 인스턴스 프로필 생성
resource "aws_iam_instance_profile" "ec2_ssm_instance_profile" {
  name = "ec2_ssm_instance_profile"
  role = aws_iam_role.ec2_ssm_role.name
}
```

<div class="content-ad"></div>

## kms.tf — CloudWatch 및 S3 암호화에 사용됩니다

이 섹션은 다중 계정 사용에 필요한 KMS 키, 키 별칭 및 관련 권한 정책을 생성합니다.

```js
# KMS 키 생성
resource "aws_kms_key" "log_key" {
  description         = "S3 로그 버킷용 KMS 키"
  enable_key_rotation = true
  policy              = <<POLICY
{
    "Version": "2012-10-17",
    "Id": "ExamplePolicy01",
    "Statement": [
        {
            "Sid": "AllowUseOfTheKey",
            "Effect": "Allow",
            "Principal": {
                "AWS": "*"
            },
            "Action": [
                "kms:Decrypt",
                "kms:GenerateDataKey"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "aws:PrincipalOrgID": "${var.organization_id}"
                }
            }
        },
        {
            "Sid": "EnableIAMUserPermissions",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::${var.account_id}:root"
            },
            "Action": "kms:*",
            "Resource": "*"
        },
        {
            "Sid": "AllowCloudWatchLogsUseOfTheKey",
            "Effect": "Allow",
            "Principal": {
                "Service": "logs.${var.region}.amazonaws.com"
            },
            "Action": [
                "kms:*"
            ],
            "Resource": "*"
        }
    ]
}
POLICY
}
# KMS 키 별칭 생성
resource "aws_kms_alias" "log_key" {
  name          = "alias/logging_key"
  target_key_id = aws_kms_key.log_key.id
}
```

## s3.tf - 버킷 설정

<div class="content-ad"></div>

여기에서 버킷이 생성되고, 버전 관리가 활성화되며(KMS 키가 정의되고 버킷 정책이 함께 설정됩니다. S3, KMS 및 EC2에 대한 리소스 및 ID 정책을 모두 정의하는 것이 복잡해 보일 수 있지만, 기억하세요, 이는 멀티 계정 구성이므로 올바른 권한을 설정하는 데 조금 더 많은 작업이 필요합니다.

```js
resource "aws_s3_bucket" "log_bucket" {
  bucket = var.bucket_name
}

resource "aws_s3_bucket_versioning" "log_bucket_versioning" {
  bucket = aws_s3_bucket.log_bucket.id
  versioning_configuration {
    status = "Enabled"
  }
}
resource "aws_s3_bucket_server_side_encryption_configuration" "log_bucket_encryption" {
  bucket = aws_s3_bucket.log_bucket.id
  rule {
    apply_server_side_encryption_by_default {
      sse_algorithm     = "aws:kms"
      kms_master_key_id = aws_kms_key.log_key.arn
    }
    bucket_key_enabled = true
  }
}
resource "aws_s3_bucket_policy" "log_bucket_policy" {
  bucket = aws_s3_bucket.log_bucket.id
  policy = <<POLICY
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "AWS": "*"
            },
            "Action": "s3:GetEncryptionConfiguration",
            "Resource": "${aws_s3_bucket.log_bucket.arn}",
            "Condition": {
                "StringEquals": {
                    "aws:PrincipalOrgID": "${var.organization_id}"
                }
            }
        },
        {
            "Effect": "Allow",
            "Principal": {
                "AWS": "*"
            },
            "Action": [
                "s3:PutObject",
                "s3:PutObjectAcl"
            ],
            "Resource": "${aws_s3_bucket.log_bucket.arn}/*",
            "Condition": {
                "StringEquals": {
                    "aws:PrincipalOrgID": "${var.organization_id}"
                }
            }
        }
    ]
}
POLICY
}
```

## cloudwatch.tf — 로그 그룹

이것은 로그 그룹을 만들고 KMS 키를 사용하여 암호화합니다. (실제 환경에서는 CloudWatch와 S3에 대해 전용 KMS 키를 사용하는 것이 가장 좋습니다.

<div class="content-ad"></div>

```js
resource "aws_cloudwatch_log_group" "session_manager_logs_group" {
  name              = "session_manager_logs"
  kms_key_id        = aws_kms_key.log_key.arn
  log_group_class   = "STANDARD"
  retention_in_days = 30
  tags = {
    Environment = "Sandbox"
  }
}
```

## data.tf — 나중에 사용할 데이터 입력 채우기

여기서 우리는 VPC의 태그 이름을 지정하여 AWS API를 terraform을 사용하여 쿼리하고 해당 속성, 즉 VPC ID를 검색할 수 있도록 합니다. VPC ID를 알고 있다면 이 데이터 소스가 필요하지 않습니다. 변수로 VPC ID를 참조하거나 terraform 구성에서 리소스로서 존재하는 경우 그곳에서 참조하십시오. (서브넷 ID도 동일합니다)

마지막으로 최신 AMI를 조회합니다. Amazon Linux 2023 이미지를 사용 중이라면 유용합니다. 나중에 사용할 AMI ID를 출력합니다.


<div class="content-ad"></div>

```js
데이터 "aws_vpc" "main" {
  필터 {
    name   = "tag:Name"
    values = ["VPC 이름 입력"]
  }
}

데이터 "aws_subnet" "subnet" {
  필터 {
    name   = "tag:Name"
    values = ["서브넷 이름 입력"]
  }
}

데이터 "aws_ami" "amazon_linux_2023" {
  most_recent = true
  소유자       = ["amazon"]

  필터 {
    name   = "name"
    values = ["amzn2-ami-kernel-5.10-hvm-*-x86_64-gp2"]
  }

  필터 {
    name   = "virtualization-type"
    values = ["hvm"]
  }

  필터 {
    name   = "architecture"
    values = ["x86_64"]
  }
}

출력 "ami_id" {
  value = data.aws_ami.amazon_linux_2023.id
}
```

## 변수

여기서 변수는 버킷의 이름, 조직 ID, 계정 ID 및 지역입니다. 여기서 당신의 terraform에 이미 있는 것에 따라 이 중 일부 또는 모두가 필요하지 않을 수 있습니다. 비슷하게, 코드를 재사용 가능하도록 만들기 위해 이 값들을 입력하는 대안적인 방법을 선택할 수도 있습니다. 예를 들어 CI 파이프라인 변수.
```js
변수 "bucket_name" {
  type    = string
  default = "medium-session-manager-article"
}

변수 "organization_id" {
  type    = string
  default = "조직 ID 입력"
}

변수 "account_id" {
  type    = string
  default = "계정 ID 입력"
}

변수 "region" {
  type    = string
  default = "eu-west-1"
}
```

<div class="content-ad"></div>

참고로, 현재 기록상 AWS terraform 공급자에 Session Manager Preferences에 대한 네이티브 terraform 리소스가 없습니다. 그러나 이 프로세스를 자동화하려면 사용할 수 있는 사용자 정의 모듈이 있습니다. 여기에서 찾을 수 있습니다. 다음 단계에서는 AWS 콘솔에서 스크린샷을 보여드리겠습니다.

- AWS에서 Systems Manager 서비스에 액세스하고, 노드 관리 아래에서 Session Manager를 찾을 수 있습니다. Preferences를 클릭한 다음 편집을 클릭하세요.

![이미지](/assets/img/2024-08-21-StopusingSSHinAWSHeresWhyADevSecOpsPerspective_0.png)

2. 일반 환경 설정 내에서 여러 옵션을 지정할 수 있습니다 (원하면 기본값으로 둘 수 있습니다):

<div class="content-ad"></div>

- 세션 타임아웃
- 최대 세션 지속 시간 — SSM 내에서 세션을 재개할 수 있는 옵션이 있으니 팀과 함께 논의할 사항입니다.
- TLS 대신 KMS 사용
- Linux EC2 인스턴스의 Run As 지정 — 특정 사용자를 가진 사용자 지정 AMI가 있는 경우 유용할 수 있습니다.

![image](/assets/img/2024-08-21-StopusingSSHinAWSHeresWhyADevSecOpsPerspective_1.png)

3. 로깅 옵션 — 중요!

여기서 로그를 보내고자 하는 대상을 지정합니다. CloudWatch, S3 또는 둘 다 지정할 수 있습니다.

<div class="content-ad"></div>


![이미지](/assets/img/2024-08-21-StopusingSSHinAWSHeresWhyADevSecOpsPerspective_2.png)

4. 이 경우에는 기본값을 사용합니다. 로그를 전송할 CloudWatch 로그 그룹을 선택한 후 저장을 클릭하세요.

![이미지](/assets/img/2024-08-21-StopusingSSHinAWSHeresWhyADevSecOpsPerspective_3.png)

5. 원하는 경우 로그를 S3로 전송할 수도 있습니다. 로그 보관 요구 사항이 있고 CloudWatch 비용을 최소화하려는 경우 유용할 수 있습니다.


<div class="content-ad"></div>

로그를 보내려는 버킷을 선택하고 옵션 프리픽스를 지정하세요. AccountID/session-logs/를 프리픽스로 사용하는 것을 제안드립니다. 이렇게 하면 어떤 AWS 계정의 로그인지 쉽게 파악할 수 있습니다. 기억하세요, 이 작업에 대한 Terraform 리소스가 없으므로 이 부분은 수동으로 하거나 계정 판매 프로세스의 일부로 해야 합니다.

![이미지](/assets/img/2024-08-21-StopusingSSHinAWSHeresWhyADevSecOpsPerspective_4.png)

# CloudWatch에 표시된 로그 예시

여기에 나의 EC2 인스턴스에 몇 가지 명령어를 입력하는 샘플이 있습니다.

<div class="content-ad"></div>

아래는 CloudWatch에서 해당 활동을 확인할 수 있습니다.

![CloudWatch Activity](/assets/img/2024-08-21-StopusingSSHinAWSHeresWhyADevSecOpsPerspective_6.png)

중요한 점은 세션이 AWS CloudTrail에 기록되어 감사 기능을 제공하며, 책임 추적 및 부인 방지를 위한 확인 기능을 제공한다는 것입니다.

<div class="content-ad"></div>

# 세션 관리자가 CloudWatch 및 S3에 활동을 기록하여 보안 포지션을 강화하는 방법

EC2 인스턴스로부터 키 스트로크를 캡처함으로써, 보안 팀은 특정 명령이 입력될 때 규칙을 작성할 수 있으며(root 권한 획득과 같은), 탐지 및 대응 능력을 지원받을 수 있습니다. 더 나아가, 로그를 SIEM에 보내면 위협 인텔리전스(IOC)를 포함하여 함의 지표를 보강할 수 있으며, 이는 의심스러운 도메인에 연결을 시도한 것(명령 및 제어 트래픽)이 발견될 수 있음을 나타낼 수 있습니다. 이는 데이터 유출을 시도하거나 암호화 채굴 플릿의 일부로 사용될 수도 있습니다.

이 솔루션의 보조적인 혜택은 변경 사항에 대한 기록을 유지할 수 있도록 하여 감사 및 규정 준수의 수준을 높여준다는 점입니다. 이를 통해 외부 감사인들은 무단 활동을 감지할 수 있도록 충분한 통제가 있음을 검토할 수 있습니다. 또한, SSM은 세션 관리자에 대한 액세스를 부여하기 위해 IAM 권한을 사용하므로 회사 보안 정책에 따라 콘솔 액세스가 제한되는 경우 예방적인 통제로 활용될 수 있습니다. 더 정교한 특권 액세스를 제공하는 다른 솔루션도 있음을 강조해야 합니다. Teleport가 그 중 하나입니다.

# 마지막으로

<div class="content-ad"></div>

AWS는 항상 더 안전한 방법으로 서비스에 액세스하는 방법을 모색하고 있지만, 이러한 새로운 작업 방식의 채택은 종종 어려운 문제입니다. 사람들은 자신의 방식에 익숙하며 어떻게 일하는 방법을 변경하기 어려워합니다. 터미널을 통해 연결하거나 베스천을 사용해오던 편리성은 이미 25년 이상의 전통이 자리 잡은 상태입니다.

Teleport와 같은 몇몇 제3자 업체가 좋은 작업을 하고 있긴 하지만, 네이티브 AWS 도구를 사용하는 것이 때로는 더 쉽습니다. Wiz와 같은 도구로 중앙 구성을 감사하고, CloudWatch나 S3와 같은 일관된 저장 솔루션에 기록할 수 있습니다.

이 문서에 대해 한 가지 주의할 점은 이미 환경에 높은 수준의 일관성 및 가드레일이 존재한다고 가정하는 것입니다. 개발자에게 올바른 IAM 역할 없이 EC2 인스턴스를 생성하도록 허용한다면, SSM이 단순히 작동하지 않습니다. 따라서 이러한 솔루션이 작동하기 위한 선행 요구 사항이 충족되지 않은 상태에서 설정이 배포되는 것을 방지하기 위해 허가 경계나 서비스 제어 정책과 같은 제어가 필요합니다. 심지어 인프라스트럭처를 코드로 스캔하여 이 솔루션이 작동하는 데 필요한 선행 조건이 충족되지 않도록 설정이 배포되는 것을 방지할 수도 있습니다.