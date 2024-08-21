---
title: "놓치기 쉬운 멋진 AWS EC2 기능들"
description: ""
coverImage: "/assets/img/2024-07-13-AwesomeAWSEC2FeaturesYouMayHaveOverlooked_0.png"
date: 2024-07-13 21:55
ogImage: 
  url: /assets/img/2024-07-13-AwesomeAWSEC2FeaturesYouMayHaveOverlooked_0.png
tag: Tech
originalTitle: "Awesome AWS EC2 Features You May Have Overlooked"
link: "https://medium.com/better-programming/awesome-aws-ec2-features-you-may-have-overlooked-7bb8d8c55af3"
isUpdated: true
---





![AWS EC2](/assets/img/2024-07-13-AwesomeAWSEC2FeaturesYouMayHaveOverlooked_0.png)

AWS 에코시스템은 기능과 서비스의 거대한 집합체입니다. AWS가 제공하는 모든 것을 숙달하는 데 여러 생애가 걸린다고 해도 과언이 아닐 것입니다. 가장 인기 있는 제품 중 하나인 EC2와 효율적으로 작업하는 방법을 파악하는 것만으로도 상당한 도전이 될 수 있습니다. 가상 머신을 배포하고 이미지를 캡처하고 사용하며 클라우드 인프라를 구축하는 다양한 방법이 있습니다.

EC2의 복잡성 때문에 종종 간과되는 유용한 기능이 몇 가지 있으며, 이러한 숨겨진 보석들은 작업 흐름을 간소화하는 훌륭한 도구 모음을 제공합니다. 이 기사에서는 EC2 작업을 더 유연하고 재미있게 만드는 몇 가지 숨겨진 장점을 살펴보겠습니다.

# 인스턴스 메타데이터 사용하기


<div class="content-ad"></div>

EC2 인스턴스 메타데이터를 사용해보지 않았다면 인스턴스에 관한 매우 풍부한 정보를 놓치고 있을 수 있습니다. 인스턴스 메타데이터 API는 흥미로운 기능입니다. 대부분의 인스턴스 유형에서 기본적으로 실행되며 호스트 데이터를 많이 얻을 수 있는 간단한 인터페이스를 제공합니다.

인스턴스 메타데이터 엔드포인트는 인스턴스의 OS 내에서 로컬 전용 주소로 사용할 수 있습니다. 따라서 실제로 기계에 연결된 상태에서만 해당 주소에 접근할 수 있습니다:

```js
http://169.254.169.254/latest/meta-data
```

curl과 같은 유틸리티를 사용하여 명령줄에서 이 주소에 대한 웹 요청을 수행하면 상당히 중요한 데이터 포인트를 볼 수 있습니다.

<div class="content-ad"></div>

```js
$ curl http://169.254.169.254/latest/meta-data
ami-id
ami-launch-index
ami-manifest-path
block-device-mapping/
events/
hibernation/
hostname
identity-credentials/
instance-action
instance-id
instance-life-cycle
instance-type
local-hostname
local-ipv4
mac
metrics/
network/
placement/
profile
public-hostname
public-ipv4
public-keys/
reservation-id
security-groups
services
```

위의 목록을 활용하여 네트워크 IP 주소와 같은 빠른 정보를 얻을 수 있습니다. 예를 들어, 공개 IPv4 주소를 찾고 싶다면 다음을 실행할 수 있습니다:

```js
curl http://169.254.169.254/latest/meta-data/public-ipv4
> 1.2.3.4
```

이는 스크립트, 구성 관리 시스템 또는 로컬 인스턴스 정보가 필요할 수 있는 모든 것에 사용될 수 있습니다.

<div class="content-ad"></div>

이 API는 다른 호스트에서는 해당 주소에 연결할 수 없습니다. 이 기능은 로컬 머신에서만 사용할 수 있습니다. 이것은 서비스가 암호화되지 않았고 어떠한 인증도 제공하지 않기 때문에 유용합니다. 인스턴스에 대한 식별 정보가 풍부하게 제공되므로 외부에서 그 중 일부를 노출하는 데 주의해야 합니다.

더 자세한 인스턴스 메타데이터에 대한 내용은 여기에서 공식 문서를 확인해보세요.

# 스크린샷 얻기

인스턴스가 무엇을 하고 있는지 궁금했던 적이 있나요? EC2는 어딘가의 클라우드에서 실행되는 가상 머신들이기 때문에 직접 액세스할 수는 없습니다. 무슨 일이 일어나고 있는지 보려면 모니터를 연결해서 확인할 수 없습니다. 그렇다면 호스트에 문제가 발생하여 원격 연결이 불안정해진 경우 어떻게 문제해결할 수 있을까요?

<div class="content-ad"></div>

스크린샷 찍는 건 물론 필요한 도구야.

만약 온라인이고 정상적인 상태라고 알려진 인스턴스가 왜 응답하지 않는지 알 수 없어 머리카락을 잡아뜯고 있다면, 여기 문제에 대한 단서를 제공해 줄 가장 빠른 방법이야. 스크린샷을 찍으면 인스턴스의 콘솔 출력을 확인할 수 있어 부트 프로세스를 검사하여 에러를 발견할 수 있단다.

![img](/assets/img/2024-07-13-AwesomeAWSEC2FeaturesYouMayHaveOverlooked_1.png)

실행 중인 가상 머신의 스크린샷을 찍고 싶다면, 인스턴스에서 우클릭하고 모니터 및 문제 해결로 이동한 뒤, get instance screenshot을 클릭하면 돼.

<div class="content-ad"></div>

스크린샷을 사용하여 문제를 진단하는 것은 네트워크 문제, 디스크 마운트 문제 및 부팅 중에 발생할 수 있는 많은 기기 관련 오류를 해결하는 데 매우 유용합니다.

# 존별 인스턴스 유형 가용성 확인

새 인스턴스를 특정 존에 시작하려고 시도했는데 다음과 같은 메시지가 표시되었던 적이 있으신가요?

“요청한 구성은 현재 지원되지 않습니다.”

<div class="content-ad"></div>

이 오류는 매우 암호화되어 있고 실질적으로 유용한 문제 해결 데이터를 제공하지 않지만, 더 많은 정보를 알아내는 방법이 있습니다.

특정 인스턴스 유형을 특정 존으로 로드하려고 할 때 해당 존에서 해당 인스턴스 유형을 사용할 수 없는 경우가 있습니다. 이는 로컬 존, 웨이브렝스 존 또는 AWS에서 출시한 새로운 존을 활용하는 경우 발생할 수 있습니다. 전문 존은 초기 롤아웃 단계에서 특히 제한된 인스턴스 유형을 제공할 수 있습니다.

그렇다면 사용 가능한 인스턴스 유형을 어떻게 결정할까요? 답을 찾기 위해 방대한 문서를 살펴볼 필요 없이 언제든지 해당 존에서 사용 가능한 유형을 간단히 알아낼 수 있습니다. aws-cli을 사용하여 해당 존에서 어떤 유형이 현재 사용 가능한지 알아낼 수 있습니다:

```js
aws ec2 describe-instance-type-offerings --location-type "availability-zone" --filters Name=location,Values=us-east-1a --region us-east-1
```

<div class="content-ad"></div>

위의 명령을 실행하면 us-east-1a 존에 있는 모든 사용 가능한 인스턴스 유형이 표시됩니다. 다른 특정 존에 관심이 있는 경우 존 및 해당 지역을 업데이트하면 해당 존에서 지원되는 인스턴스 유형 목록을 볼 수 있습니다.

이 목록을 찾아내어 원하는 인스턴스 유형이 해당 존에 있는지 확인하는 것은 매우 간단할 것입니다.

# 데이터 라이프사이클 관리자를 사용한 롤링 스냅숏

인스턴스를 모두 백업하는 방법에 대해 궁금했던 적이 있나요? 일회성 스냅숏을 찍는 것은 특정 수를 넘어서면 지속할 수 없으며 오래된 버전을 톤으로 유지하면 저장 비용이 급속히 증가할 수 있습니다. 그렇다면 모든 인스턴스 볼륨이 안전하고 건전한 상태인지 확실히 하는 가장 간단한 방법은 무엇일까요?

<div class="content-ad"></div>

데이터 라이프사이클 관리자를 사용하세요.

AWS EC2의 데이터 라이프사이클 관리자를 활용하면 특정 인스턴스의 볼륨에 대한 정책(태그 사용)을 만들어 자동 스냅샷을 찍은 후 일정 수의 스냅샷만 유지할 수 있습니다.

![이미지](/assets/img/2024-07-13-AwesomeAWSEC2FeaturesYouMayHaveOverlooked_2.png)

이 서비스는 여러 문제들을 해결해주는데, 그 중에서도 가장 중요한 것은 스냅샷의 자동 생성과 일정 기간 동안 보존하는 기능입니다. 이제는 수동으로 스냅샷을 생성하거나 삭제할 필요가 없습니다. 인스턴스에 적절한 태그를 달아주기만 하면 해당 볼륨에 롤링 스냅샷이 만들어집니다.

<div class="content-ad"></div>

이제 문제가 발생하더라도 데이터가 안전하게 보호되어 필요한 곳에 정확히 위치하고 있음을 확신할 수 있습니다.

DLM에 대한 자세한 구현 가이드는 이전 포스트인 "AWS 데이터 수명주기 관리자로 쉽게 디스크 스냅숏 생성하기"를 확인해보세요.

더 읽을 거리를 찾고 계신다면, 아래 몇 가지 다른 아티클도 확인해보세요:

- 알아야 할 기본 TLS 인증서 명령어
- 꼭 사용해야 하는 5가지 클라우드 API 개발 도구