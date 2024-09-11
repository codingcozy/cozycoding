---
title: "S3 버킷 공개 액세스 차단하는 방법"
description: ""
coverImage: "/assets/img/2024-09-10-BlockPublicAccesstotheS3bucket_0.png"
date: 2024-09-10 19:07
ogImage: 
  url: /assets/img/2024-09-10-BlockPublicAccesstotheS3bucket_0.png
tag: Tech
originalTitle: "Block Public Access to the S3 bucket"
link: "https://medium.com/itnext/block-public-access-to-the-s3-bucket-77dc4e97e2d7"
isUpdated: true
updatedAt: 1726022687795
---


<img src="/assets/img/2024-09-10-BlockPublicAccesstotheS3bucket_0.png" />

## 소개

Amazon Simple Storage Service (AWS S3)는 어떤 크기의 데이터든 저장하고 얻는 데 강력하고 잘 알려진 도구입니다. 사용하기 쉽고 필요에 따라 성장할 수 있으며 빠르게 작동하기 때문에 다양한 종류의 데이터 저장 작업에 인기가 있습니다. 그러나 너무 유연하기 때문에 거기에 보관된 데이터가 안전한지 확인하는 것은 매우 중요한 문제가 됩니다.

많은 기업이 S3 버킷을 충분히 보호하지 못하고 노력하는 것을 목격해 왔습니다. 이는 종종 몇 가지 기본 규칙을 무시하기 때문입니다. 이 게시물에서는 S3 보안의 복잡성을 탐구하고 강화하는 방법에 대한 포괄적인 안내서를 제공하려고 합니다.

<div class="content-ad"></div>

S3는 다른 AWS 서비스와 비교했을 때 권한 부여에 대해 독자적인 스타일을 가지고 있어요. 여러 서비스에서는 사용자나 역할에 대한 올바른 권한만 설정하면 괜찮은 경우가 많지만, S3는 자체 규칙에 따라 움직이죠. S3가 누가 어떤 것을 볼 수 있는지를 결정하며, 이것은 보안 측면에서 엄격한 보안수칙을 가지고 있음을 의미해요. 하지만, 한 가지 주의할 점이 있어요. 이 유연성은 때로는 너무 많은 접근을 실수로 허용하거나, 미친듯이 버킷을 공개하고 말 수도 있죠. 따라서, S3의 권한 처리 방식은 것을 잠그는 데 유용하지만, 실수를 피하기 위해서 신중한 눈초리가 필요하다는 것을 명심해야 해요.

몇 가지 예시로 살펴보겠어요. 첫 번째 예시에서는, S3 규칙이 사용자에게 특정 권한을 부여합니다 — 아주 표준적인 내용이에요.

```js
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::123456789012:user/DummyUser"
            },
            "Action": [
                "s3:GetObject",
                "s3:PutObject",
                "s3:ListBucket",
                "s3:DeleteObject"
            ],
            "Resource": [
                "arn:aws:s3:::example-bucket",
                "arn:aws:s3:::example-bucket/*"
            ]
        }
    ]
}
```

그리고 두 번째 예시에서는, * 와일드카드를 추가하기만 하면, 누구나 S3에 대해 완전한 액세스 권한을 획득할 수 있어요. 이건 정말 무서운 일이죠.

<div class="content-ad"></div>

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "AWS": [
                    "arn:aws:iam::123456789012:user/DummyUser",
                    "*"
                ]
            },
            "Action": [
                "s3:GetObject",
                "s3:PutObject",
                "s3:ListBucket",
                "s3:DeleteObject"
            ],
            "Resource": [
                "arn:aws:s3:::example-bucket",
                "arn:aws:s3:::example-bucket/*"
            ]
        }
    ]
}
```

그리고 알아두세요, 때로는 사람들이 두 번째 예시처럼 실수로 문을 완전히 열어두곤 합니다, 아마 뭔가 빨리 해내기 위해서 그랬던 것일지도 몰라요. 그러나 그게 실수였든 의도적이었든 간에, 저희는 우리 데이터를 안전하고 건전하게 유지하기 위해 그것을 막아야 합니다.'

## 장벽 설정하기

AWS는 S3에서 당신의 것을 개인적으로 유지할 수 있게 해 주는 네 가지 간단하면서도 강력한 설정을 제공합니다. 플립.


<div class="content-ad"></div>

BlockPublicAcls: true

- 문 앞에 경비원이 있어요. 새 권한이 지나가지 못하도록 막아줘서 게이트 크래셔를 막아줘요.

BlockPublicPolicy: true

- 이 설정은 VIP 명단 같아요. 누군가가 일반 통행증을 가지고 있어도, 이 설정은 버킷 정책을 변경할 VIP 상태를 가지고 있는 지 확인해요. VIP 상태가 없으면 입장 불가능해요!

<div class="content-ad"></div>


IgnorePublicAcls: true

- Imagine if someone from the crowd shouts, “I have a pass!” This setting ensures that any public access passes are ignored, keeping the control in your hands.

RestrictPublicBuckets: true

- This one is like a curtain around your private party. Even if someone tries to peek in, they won’t see anything unless you’ve given them a special invite.


<div class="content-ad"></div>

올바른 권한(허가)을 가진 사람들만 출입하거나 변경할 수 있어요.

# 공개 액세스 차단하기

여기서는 S3 버킷의 공개 액세스를 차단하는 3가지 다른 방법에 대해 이야기할 거예요.

## CloudFormation

<div class="content-ad"></div>

S3 버킷을 생성하고 공개 액세스를 차단하는 작업은 AWS CloudFormation을 이용하여 간편하게 수행할 수 있습니다.

아래는 S3 버킷을 생성하고 공개 액세스를 차단하는 간단한 CloudFormation 템플릿 예제입니다:

```js
Resources:
  MyS3Bucket:
    Type: AWS::S3::Bucket
    Properties: 
      BucketName: "example-bucket-name"  # 고유한 버킷 이름으로 교체하세요
      PublicAccessBlockConfiguration:
        BlockPublicAcls: true
        BlockPublicPolicy: true
        IgnorePublicAcls: true
        RestrictPublicBuckets: true

  PublicAccessBlock:
    Type: AWS::S3::BucketPolicy
    Properties: 
      Bucket: !Ref MyS3Bucket
      PolicyDocument:
        Id: DenyAll
        Version: "2012-10-17"
        Statement:
          - Sid: DenyAll
            Effect: Deny
            Principal: "*"
            Action: "s3:*"
            Resource:
              - !Sub "${MyS3Bucket.Arn}"
              - !Sub "${MyS3Bucket.Arn}/*"
```

설명:

<div class="content-ad"></div>

- 자원(Resource): 이 템플릿의 주요 섹션으로, 우리가 생성하거나 관리하려는 AWS 자원을 정의합니다.
- MyS3Bucket: 이는 우리의 CloudFormation 템플릿 내에서 S3 버킷 자원의 논리적 이름입니다.
- 유형(Type): 생성하려는 AWS 자원의 유형을 지정합니다. 이 경우에는 S3 버킷입니다.
- 속성(Properties): 우리가 생성하는 S3 버킷의 속성을 포함하는 섹션입니다.
- BucketName: 이 속성은 S3 버킷의 이름을 지정합니다. "example-bucket-name"을 여러분의 고유한 버킷 이름으로 바꿔주세요.
- PublicAccessBlockConfiguration: 이 섹션에는 S3 버킷의 공개 액세스 차단 구성을 포함합니다.
- BlockPublicAcls, BlockPublicPolicy, IgnorePublicAcls, RestrictPublicBuckets: 이러한 속성들은 모두 공개 액세스가 모든 측면에서 차단되도록 true로 설정됩니다.
- PublicAccessBlock: 이는 버킷 정책으로 버킷에 대한 공개 액세스 없음을 보장합니다. 공개 액세스 규칙을 강제하는 추가적인 조치입니다.
- BucketPolicy: 이 섹션은 S3 버킷에서 모든 주체(Principal: "*")에 대해 모든 작업(s3:*)을 거부하는 정책 문서를 정의하며, 이를 통해 버킷에 대한 모든 공개 액세스가 차단됩니다.

"example-bucket-name"을 여러분의 고유한 버킷 이름으로 교체해주세요. 이 템플릿을 CloudFormation을 통해 실행하면 구성된대로 공개 액세스가 차단된 S3 버킷이 생성됩니다.

## AWS CDK

AWS CDK를 사용하여 TypeScript에서 S3 버킷을 생성하고 공개 액세스를 차단하는 것은 간단합니다. AWS CDK는 클라우드 인프라를 코드로 정의하고 AWS CloudFormation을 통해 제공하는 소프트웨어 개발 프레임워크입니다.

<div class="content-ad"></div>

여기에서는 어떻게 할 수 있는지 안내드리겠습니다:

```js
// 필요한 AWS CDK 패키지를 가져옵니다
import * as cdk from 'aws-cdk-lib';
import * as s3 from 'aws-cdk-lib/aws-s3';

// 새로운 Stack 클래스를 정의합니다
class MyS3Stack extends cdk.Stack {
  constructor(scope: cdk.Construct, id: string, props?: cdk.StackProps) {
    super(scope, id, props);

    // 공개 액세스가 차단된 새로운 S3 버킷을 생성합니다
    new s3.Bucket(this, 'MyS3Bucket', {
      bucketName: 'example-bucket-name',  // 고유한 버킷 이름으로 대체하세요
      blockPublicAccess: s3.BlockPublicAccess.BLOCK_ALL,  // 모든 공개 액세스를 차단합니다
    });
  }
}

// 앱과 스택을 인스턴스화합니다
const app = new cdk.App();
new MyS3Stack(app, 'MyS3Stack');
```

설명:

- 패키지 가져오기: 파일의 시작 부분에서 필요한 AWS CDK 패키지를 가져옵니다.
- Stack 정의: cdk.Stack를 확장하는 MyS3Stack 클래스를 생성합니다. 생성자 메서드는 AWS 리소스를 정의하는 곳입니다.
- S3 버킷 생성: 생성자 내부에서 s3.Bucket 생성자를 사용하여 새로운 S3 버킷을 생성합니다.
- 버킷 이름: bucketName 속성을 사용하여 버킷에 이름을 지정합니다. `example-bucket-name`을 고유한 버킷 이름으로 대체하세요.
- 공개 액세스 차단: blockPublicAccess 속성을 사용하여 S3 버킷에 대한 모든 공개 액세스를 차단합니다. 속성 값을 s3.BlockPublicAccess.BLOCK_ALL로 설정합니다.
- 앱 및 스택 인스턴스화: 마지막으로 cdk.App 클래스의 새 인스턴스 및 MyS3Stack 클래스의 새 인스턴스를 생성하여 배포 시 AWS 리소스를 정의하고 생성할 것입니다.

<div class="content-ad"></div>

이 스택을 배포하려면 코드를 파일에 저장하십시오 (예: MyS3Stack.ts), 터미널에서 이 파일이 있는 디렉토리로 이동한 다음 다음 명령을 실행하십시오:

```js
cdk deploy
```

이 명령은 CDK 앱을 컴파일하고 배포하여 스택에서 지정한대로 공개 액세스가 차단된 S3 버킷을 생성합니다.

## AWS 콘솔:

<div class="content-ad"></div>

- AWS 콘솔에 접속하세요.

- AWS 관리 콘솔에 로그인하세요.

2. S3으로 이동

- "서비스" 메뉴에서 S3 서비스로 이동하세요.

<div class="content-ad"></div>

3. 버킷 선택하기

- S3 대시보드에서 구성하려는 버킷 이름을 클릭하세요.

4. 액세스 권한 탭

- "권한" 탭을 찾아 클릭하세요.

<div class="content-ad"></div>

5. 공개 액세스 차단 설정

- "공개 액세스 차단" 섹션에서 "편집" 버튼을 클릭합니다.

6. 설정 구성

![이미지](/assets/img/2024-09-10-BlockPublicAccesstotheS3bucket_1.png)

<div class="content-ad"></div>


![이미지](/assets/img/2024-09-10-BlockPublicAccesstotheS3bucket_2.png)

- 새로운 액세스 제어 목록(ACL)을 통해 부여된 버킷과 객체에 대한 공개 액세스 차단
- 어떠한 액세스 제어 목록(ACL)을 통해 부여된 버킷과 객체에 대한 공개 액세스 차단
- 새로운 공용 버킷 또는 액세스 포인트 정책을 통해 부여된 버킷과 객체에 대한 공개 액세스 차단
- 어떠한 공용 버킷 또는 액세스 포인트 정책을 통해 부여된 버킷과 객체에 대한 공개 및 교차 계정 액세스 차단
- 모든 네 개의 상자를 선택하여 모든 공개 액세스를 차단합니다.

7. 변경사항 저장

- 상자를 모두 확인한 후 아래로 스크롤하여 “변경사항 저장” 버튼을 클릭하세요.
- 확인 팝업이 나타나면 “확인”이라고 입력한 다음 변경 사항을 적용하려면 “확인” 버튼을 클릭하세요.
