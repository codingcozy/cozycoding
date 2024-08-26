---
title: "AWS Elastic Load Balancer와 Terraform를 활용한 고트래픽 애플리케이션 관리 실전 랩"
description: ""
coverImage: "/assets/img/2024-08-26-ManagingHigh-TrafficApplicationswithAWSElasticLoadBalancerandTerraformAHands-OnLab_0.png"
date: 2024-08-26 19:01
ogImage: 
  url: /assets/img/2024-08-26-ManagingHigh-TrafficApplicationswithAWSElasticLoadBalancerandTerraformAHands-OnLab_0.png
tag: Tech
originalTitle: "Managing High-Traffic Applications with AWS Elastic Load Balancer and Terraform A Hands-On Lab"
link: "https://medium.com/@meriemterki/managing-high-traffic-applications-with-aws-elastic-load-balancer-and-terraform-a-hands-on-lab-b2f5d1f7ef81"
isUpdated: false
---


<img src="/assets/img/2024-08-26-ManagingHigh-TrafficApplicationswithAWSElasticLoadBalancerandTerraformAHands-OnLab_0.png" />

# 소개

이 블로그에서는 AWS Elastic Load Balancer (ELB)와 Terraform을 사용하여 확장 가능하고 고가용성 아키텍처를 만드는 방법을 안내합니다. VPC, 서브넷, 보안 그룹, EC2 인스턴스, ELB 및 Auto Scaling 그룹을 포함한 필요한 모든 것을 설정하고, Terraform 코드를 main.tf, variables.tf, outputs.tf 및 providers.tf로 구성합니다.

# 필수 조건

<div class="content-ad"></div>

- AWS 계정: 활성화된 AWS 계정이 필요합니다.
- Terraform 설치: 로컬 머신에 Terraform이 설치되어 있어야 합니다.
- 기본 지식: AWS 서비스(EC2, VPC, ELB)와 Terraform에 대한 기본 지식이 필요합니다.

# 프로젝트 구조

Terraform 코드에 들어가기 전에 프로젝트 디렉토리를 조직화해봅시다:

![프로젝트 구조](/assets/img/2024-08-26-ManagingHigh-TrafficApplicationswithAWSElasticLoadBalancerandTerraformAHands-OnLab_1.png)

<div class="content-ad"></div>

# 아키텍처 다이어그램

![Architecture Diagram](/assets/img/2024-08-26-ManagingHigh-TrafficApplicationswithAWSElasticLoadBalancerandTerraformAHands-OnLab_2.png)

# 프로젝트 단계

## 단계 1: Terraform 구성 설정하기

<div class="content-ad"></div>

## 1.1. 변수를 variables.tf에 정의하기

먼저, 우리가 테라폼 코드 전반에서 사용할 변수들을 정의해봅시다.

```js
#variables.tf


/*EC2 변수는 EC2 인스턴스의 AMI ID, 인스턴스 유형 및 VPC ID와 같은 값을 저장하는 데 사용할 수 있습니다. 
우리 테라폼 코드에서 이러한 값은 EC2 인스턴스를 생성하고 구성하는 데 사용됩니다. */

variable "ami" {
  description = "EC2 인스턴스의 AMI"
  type        = string
  default     = "ami-0715c1897453cabd1"
}

# 런치 템플릿 및 ASG 변수
variable "instance_type" {
  description = "런치 템플릿 EC2 인스턴스 유형"
  type        = string
  default     = "t2.micro"
}


#이 사용자 데이터 변수는 스크립트가 서버에 아파치를 구성한다는 것을 나타냅니다.
variable "ec2_user_data" {
  description = "스크립트가 서버에 아파치를 구성한다는 것을 나타냅니다"
  type        = string
  default     = <<EOF
#!/bin/bash
# 우분투에 아파치 설치
sudo apt update -y
sudo apt install -y apache2
sudo cat > /var/www/html/index.html << EOF
<html>
<head>
  <title> Apache 2023 Terraform </title>
</head>
<body>
  <p> Meriem의 AWS EC2 Auto-Scaling Group을 이용한 온라인 스토어에 오신 것을 환영합니다!
</body>
</html>
EOF
}


/*이 VPC는 인터넷이나 VPC 내 다른 리소스에서 액세스할 수 있어야 하는 리소스를 배포하는 데 사용할 수 있습니다.
이 변수는 VPC의 CIDR 블록을 정의합니다. 기본 값은 10.0.0.0/16입니다.
*/

# VPC 변수
variable "vpc_cidr" {
  description = "VPC CIDR 블록"
  type        = string
  default     = "10.10.0.0/16"
}

#이 Public 서브넷은 인터넷에서 액세스해야 하는 리소스에 사용됩니다.
variable "public_subnet_cidr" {
  description = "Public 서브넷 CIDR 블록"
  type        = list(string)
  default     = ["10.10.0.0/24", "10.10.2.0/24"]
}

#이 Private 서브넷은 인터넷에서 액세스할 필요가 없는 리소스를 배포하는 데 사용할 수 있습니다.
variable "private_subnet_cidr" {
  description = "Private 서브넷 CIDR 블록"
  type        = list(string)
  default     = ["10.10.3.0/24", "10.10.4.0/24"]
}

#이것은 환경 변수입니다
variable "environment" {
  description = "배포를 위한 환경 이름"
  type        = string
  default     = "terraformEnvironment"
}

# 이것은 지역 변수입니다
variable "aws_region" {
  description = "AWS 리전 이름"
  type        = string
  default     = "us-east-1"
}
```

<div class="content-ad"></div>

main.tf 파일에서 아키텍처에 필요한 모든 리소스를 정의할 것입니다.

```js
# main.tf

# Terraform Resources

# AWS에서 새 VPC 생성
resource "aws_vpc" "vpc" {
  cidr_block = var.vpc_cidr
  tags = {
    Name = "TerraformVPC2023"
  }
}

# AWS 가용 영역
data "aws_availability_zones" "available" {
  state = "available"
}

# 보안 그룹 리소스
resource "aws_security_group" "alb_security_group" {
  name        = "${var.environment}-alb-security-group"
  description = "ALB Security Group"
  vpc_id      = aws_vpc.vpc.id
  ingress {
    description = "인터넷에서의 HTTP 허용"
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
  tags = {
    Name = "${var.environment}-alb-security-group"
  }
}

...

# 패밀리 애로우 이미지 확인
data "aws_ami" "ubuntu" {
  most_recent = true
  filter {
    name   = "name"
    values = ["ubuntu/images/hvm-ssd/ubuntu-jammy-22.04-amd64-*"]
  }
  filter {
    name   = "virtualization-type"
    values = ["hvm"]
  }
  owners = ["099720109477"]
}

# 시작 템플릿 및 ASG 리소스
resource "aws_launch_template" "launch_template" {
  name          = "${var.environment}-launch-template"
  image_id      = data.aws_ami.ubuntu.id
  instance_type = var.instance_type
  network_interfaces {
    device_index    = 0
    security_groups = [aws_security_group.asg_security_group.id]
  }
  tag_specifications {
    resource_type = "instance"
    tags = {
      Name = "${var.environment}-asg-ec2"
    }
  }
  user_data = base64encode("${var.ec2_user_data}")
}
```

## 1.3. outputs.tf에 출력값 정의

마지막으로, 로드 밸런서의 DNS 이름과 같은 중요한 값을 출력합니다.

<div class="content-ad"></div>

```js
#outputs.tf

# Terraform Outputs
output "alb_public_url" {
  description = "Application Load Balancer의 공개 URL"
  value       = aws_lb.alb.dns_name
}
```

## 1.4 providers.tf에서 프로바이더 정의

인프라 자원을 생성하고 관리하기 위해 Terraform이 사용할 클라우드 프로바이더를 지정합니다.

```js
#providers.tf

# Terraform Providers
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 4.0"
    }
  }
}
provider "aws" {
  region = "us-east-1"
}
```

<div class="content-ad"></div>

# 단계 2: 아키텍처 배포

이제 모든 테라폼 구성이 준비되었으니 아키텍처를 배포할 수 있습니다.

## 2.1. 테라폼 초기화

터미널에서 프로젝트 디렉토리로 이동하고 다음을 실행하세요:

<div class="content-ad"></div>

```js
terraform init
```

이 명령어는 Terraform 구성 파일이 포함된 작업 디렉토리를 초기화합니다.

## 2.2. 배포 계획 수립

다음 명령어를 실행하세요:

<div class="content-ad"></div>

```js
terraform plan
```

이 명령어를 실행하면 Terraform이 구성을 적용할 때 어떤 작업을 수행할지 보여줍니다.

## 2.3. 구성 적용

마지막으로, 인프라를 배포하기 위해 구성을 적용하세요:

<div class="content-ad"></div>

```js
terraform apply -auto-approve
```

# 단계 3: 애플리케이션 테스트

Terraform이 배포를 완료하면 로드 밸런서의 DNS 이름에 액세스하여 설정을 테스트할 수 있습니다:

```js
terraform output elb_dns_name
```

<div class="content-ad"></div>

DNS 이름을 복사하고 브라우저에서 열어보세요. 여기서 하나의 EC2 인스턴스에서 생성된 웹 페이지의 내용을 확인할 수 있어요. 이것으로 여러분의 로드 밸런서가 인스턴스 간 트래픽을 올바르게 분산하고 있는지 확인할 수 있습니다.

![이미지](/assets/img/2024-08-26-ManagingHigh-TrafficApplicationswithAWSElasticLoadBalancerandTerraformAHands-OnLab_3.png)

# 단계 4: 생성한 인프라를 제거하기

리소스를 제거하려면 터미널에서 다음 명령을 실행하세요. 여러분은 Terraform이 각 리소스의 상태를 새로 고친 다음 올바른 순서대로 제거하는 것을 확인할 수 있어요.

<div class="content-ad"></div>

```js
terraform destroy -auto-approve
```

![Image](/assets/img/2024-08-26-ManagingHigh-TrafficApplicationswithAWSElasticLoadBalancerandTerraformAHands-OnLab_4.png)

그리고 여기까지입니다!

# 사용된 자료:

<div class="content-ad"></div>

- [Terraform Up and Running](https://www.terraformupandrunning.com/)
- [Terraform Hands-On Labs on Udemy](https://www.udemy.com/course/terraform-hands-on-labs/)

If you want to access my notes and the flashcards I am building, you can find them [here](https://shadowed-bubble-139.notion.site/30-Day-Terraform-Challenge-dec28b59677845218d72dd4163751c87?pvs=4). I'll keep updating them as I progress through the challenge.

That’s all for today. Thank you for reading and following along! I hope this blog was helpful and worthwhile. Stay tuned for my next project in the cloud.

Let’s connect on [LinkedIn](https://www.linkedin.com/in/meriemterki/)! 👉