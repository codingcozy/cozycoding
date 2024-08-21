---
title: "소프트웨어 아키텍처 패턴 10가지 요약 정리 개발자가 알아야 할 필수 패턴들"
description: ""
coverImage: "/assets/img/2024-07-14-10CommonSoftwareArchitecturalPatternsinanutshell_0.png"
date: 2024-07-14 00:46
ogImage: 
  url: /assets/img/2024-07-14-10CommonSoftwareArchitecturalPatternsinanutshell_0.png
tag: Tech
originalTitle: "10 Common Software Architectural Patterns in a nutshell"
link: "https://medium.com/towards-data-science/10-common-software-architectural-patterns-in-a-nutshell-a0b47a1e9013"
isUpdated: true
---




**대기업 규모 시스템이 어떻게 설계되는지 궁금했던 적이 있나요? 주요 소프트웨어 개발이 시작되기 전에, 우리는 원하는 기능과 품질 속성을 제공해 줄 적합한 아키텍처를 선택해야 합니다. 따라서, 우리는 디자인에 적용하기 전에 다양한 아키텍처를 이해해야 합니다.**

![10CommonSoftwareArchitecturalPatternsinanutshell_0](/assets/img/2024-07-14-10CommonSoftwareArchitecturalPatternsinanutshell_0.png)

# 아키텍처 패턴이란?

위키피디아에 따르면,

<div class="content-ad"></div>

이 기사에서는 다음 10가지 일반적인 아키텍처 패턴을 간단히 설명하겠습니다. 각 패턴의 사용법, 장단점을 포함할 것입니다.

- 계층화 패턴
- 클라이언트-서버 패턴
- 마스터-슬레이브 패턴
- 파이프-필터 패턴
- 브로커 패턴
- 피어 투 피어 패턴
- 이벤트 버스 패턴
- 모델-뷰-컨트롤러 패턴
- 블랙보드 패턴
- 해석자 패턴

# 1. 계층화 패턴

이 패턴은 프로그램을 구성할 수 있는 구조를 제공하는 데 사용될 수 있습니다. 각 계층은 특정 추상화 수준에 위치한 하위 작업 그룹으로 분해될 수 있습니다. 각 계층은 다음 상위 계층에 서비스를 제공합니다.

<div class="content-ad"></div>

일반 정보 시스템에서 가장 흔히 발견되는 4개의 레이어는 다음과 같습니다.

- 프레젠테이션 레이어 (UI 레이어로도 알려짐)
- 애플리케이션 레이어 (서비스 레이어로도 알려짐)
- 비즈니스 로직 레이어 (도메인 레이어로도 알려짐)
- 데이터 액세스 레이어 (지속성 레이어로도 알려짐)

## 사용법

- 일반 데스크톱 애플리케이션
- 전자 상거래 웹 애플리케이션

<div class="content-ad"></div>

![이미지](/assets/img/2024-07-14-10CommonSoftwareArchitecturalPatternsinanutshell_1.png)

# 2. 클라이언트-서버 패턴

이 패턴은 서버와 여러 클라이언트로 구성됩니다. 서버 구성 요소는 여러 클라이언트 구성 요소에 서비스를 제공합니다. 클라이언트는 서버에 서비스를 요청하고, 서버는 해당 클라이언트에 필요한 서비스를 제공합니다. 또한 서버는 계속해서 클라이언트 요청을 수신합니다.

## 사용법

<div class="content-ad"></div>

- 온라인 응용 프로그램은 전자 메일, 문서 공유 및 은행 업무와 같은 것들이 있습니다.


![](/assets/img/2024-07-14-10CommonSoftwareArchitecturalPatternsinanutshell_2.png)


## 3. 마스터-슬레이브 패턴

이 패턴은 마스터(master)와 슬레이브(slave) 두 가지 구성 요소로 구성되어 있습니다. 마스터 구성 요소는 동일한 슬레이브 구성 요소 사이에서 작업을 분배하고, 슬레이브가 반환한 결과를 통해 최종 결과를 계산합니다.

<div class="content-ad"></div>

## 사용법

- 데이터베이스 복제에서 마스터 데이터베이스는 권위 있는 원본으로 간주되며, 슬레이브 데이터베이스는 동기화됩니다.
- 컴퓨터 시스템의 버스에 연결된 주변 장치(마스터 및 슬레이브 드라이브).

![이미지](/assets/img/2024-07-14-10CommonSoftwareArchitecturalPatternsinanutshell_3.png)

# 4. Pipe-filter 패턴

<div class="content-ad"></div>

이 패턴은 데이터 스트림을 생성하고 처리하는 시스템을 구성하는 데 사용될 수 있습니다. 각 처리 단계는 필터 구성 요소 내에 포함됩니다. 처리할 데이터는 파이프를 통해 전달됩니다. 이러한 파이프는 버퍼링이나 동기화 목적으로 사용할 수 있습니다.

## 사용 예

- 컴파일러. 연이어 필터는 렉시컬 분석, 구문 분석, 의미 분석 및 코드 생성을 수행합니다.
- 생명정보학의 워크플로우.

![2024-07-14-10CommonSoftwareArchitecturalPatternsinanutshell_4.png](/assets/img/2024-07-14-10CommonSoftwareArchitecturalPatternsinanutshell_4.png)

<div class="content-ad"></div>

## 5. 중개자 패턴

중개자 패턴은 분리된 구성 요소를 가진 분산 시스템을 구조화하는 데 사용됩니다. 이러한 구성 요소는 원격 서비스 호출을 통해 서로 상호 작용할 수 있습니다. 중개자 구성 요소는 구성 요소들 간의 통신을 조정하는 역할을 합니다.

서버는 자신의 기능(서비스 및 특성)을 중개자에 발행합니다. 클라이언트는 중개자에게 서비스를 요청하고, 중개자는 클라이언트를 등록부에서 적절한 서비스로 리디렉션합니다.

### 사용법

<div class="content-ad"></div>

- Apache ActiveMQ, Apache Kafka, RabbitMQ 및 JBoss Messaging과 같은 메시지 브로커 소프트웨어 있습니다.

![image](/assets/img/2024-07-14-10CommonSoftwareArchitecturalPatternsinanutshell_5.png)

### 6. Peer-to-peer pattern

이 패턴에서 개별 구성 요소는 피어로 알려져 있습니다. 피어는 다른 피어로부터 서비스를 요청하는 클라이언트로서 또는 다른 피어에게 서비스를 제공하는 서버로서 기능할 수 있습니다. 피어는 클라이언트로서 또는 서버로서 또는 둘 다로 행동할 수 있으며, 역할을 동적으로 변경할 수도 있습니다.

<div class="content-ad"></div>

## 사용법

- Gnutella와 G2 같은 파일 공유 네트워크
- P2PTV와 PDTP 같은 멀티미디어 프로토콜
- Bitcoin과 Blockchain과 같은 암호화폐 기반 제품

![이미지](/assets/img/2024-07-14-10CommonSoftwareArchitecturalPatternsinanutshell_6.png)

# 7. 이벤트 버스 패턴

<div class="content-ad"></div>

이 패턴은 주로 이벤트와 관련이 있으며 4가지 주요 구성 요소가 있습니다. 이벤트 원본, 이벤트 리스너, 채널 및 이벤트 버스입니다. 원본은 이벤트 버스의 특정 채널로 메시지를 발행합니다. 리스너는 특정 채널에 가입합니다. 리스너는 구독한 채널에 발행된 메시지를 먼저 확인할 수 있습니다.

## 사용 예

- Android 개발
- 알림 서비스

![이미지](/assets/img/2024-07-14-10CommonSoftwareArchitecturalPatternsinanutshell_7.png)

<div class="content-ad"></div>

# 8. Model-view-controller pattern

이 패턴, MVC 패턴으로도 알려져 있는데, 상호작용하는 애플리케이션을 3가지 부분으로 나눕니다.

- model — 핵심 기능과 데이터를 포함합니다.
- view — 정보를 사용자에게 표시합니다. (여러 가지 뷰를 정의할 수 있음)
- controller — 사용자로부터의 입력을 처리합니다.

이러한 방식으로 정보의 내부 표현을 사용자에게 제시되고 받아들이는 방식으로부터 분리합니다. 이는 컴포넌트를 분리시키고 효율적인 코드 재사용을 가능하게 합니다.

<div class="content-ad"></div>

## 사용 방법

- 주요 프로그래밍 언어로 구축된 월드 와이드 웹 응용 프로그램 아키텍처
- Django 및 Rails와 같은 웹 프레임워크

![이미지](/assets/img/2024-07-14-10CommonSoftwareArchitecturalPatternsinanutshell_8.png)

# 9. 블랙보드 패턴

<div class="content-ad"></div>

이 패턴은 결정적인 해결 전략이 알려지지 않은 문제에 유용합니다. 블랙보드 패턴은 3가지 주요 구성 요소로 구성되어 있습니다.

- 블랙보드 — 해결 공간에서 객체를 포함하는 구조화된 전역 메모리
- 지식 원천 — 자체 표현을 갖는 전문 모듈
- 제어 구성 요소 — 모듈을 선택, 구성 및 실행합니다.

모든 구성 요소가 블랙보드에 액세스할 수 있습니다. 구성 요소는 블랙보드에 추가되는 새로운 데이터 객체를 생성할 수 있습니다. 구성 요소는 특정 유형의 데이터를 블랙보드에서 찾습니다. 기존 지식 원천과 패턴 매칭을 통해 이를 찾을 수 있습니다.

## 사용법

<div class="content-ad"></div>

- 음성 인식
- 차량 식별 및 추적
- 단백질 구조 식별
- 음파 신호 해석.

![Interpreter pattern](/assets/img/2024-07-14-10CommonSoftwareArchitecturalPatternsinanutshell_9.png)

# 10. Interpreter pattern

이 패턴은 특정 언어로 작성된 프로그램을 해석하는 구성 요소를 설계하는 데 사용됩니다. 주로 특정 언어로 작성된 문장 또는 표현, 즉 프로그램 라인을 평가하는 방법을 지정합니다. 기본 아이디어는 해당 언어의 각 기호에 대한 클래스를 가지는 것입니다.

<div class="content-ad"></div>

## 사용 방법

- SQL과 같은 데이터베이스 쿼리 언어.
- 커뮤니케이션 프로토콜을 설명하는 데 사용되는 언어.

![이미지](/assets/img/2024-07-14-10CommonSoftwareArchitecturalPatternsinanutshell_10.png)

# 아키텍처 패턴 비교

<div class="content-ad"></div>

아래 표는 각 아키텍처 패턴의 장단점을 요약한 것입니다.

이 글이 유익했기를 바랍니다. 여러분의 생각을 듣고 싶어요. 😇

읽어주셔서 감사합니다. 😊

건배! 😃

<div class="content-ad"></div>

# 참고 자료

[해당 PDF 링크](https://www.ou.nl/documents/40554/791670/IM0203_03.pdf/30dae517-691e-b3c7-22ed-a55ad27726d6)