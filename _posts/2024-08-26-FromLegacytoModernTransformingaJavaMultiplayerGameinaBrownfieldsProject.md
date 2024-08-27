---
title: "레거시에서 현대로 브라운필드 프로젝트에서 Java 멀티플레이어 게임 변환"
description: ""
coverImage: "/assets/img/2024-08-26-FromLegacytoModernTransformingaJavaMultiplayerGameinaBrownfieldsProject_0.png"
date: 2024-08-26 18:44
ogImage: 
  url: /assets/img/2024-08-26-FromLegacytoModernTransformingaJavaMultiplayerGameinaBrownfieldsProject_0.png
tag: Tech
originalTitle: "From Legacy to Modern Transforming a Java Multiplayer Game in a Brownfields Project"
link: "https://medium.com/@katlegobarayi07/from-legacy-to-modern-transforming-a-java-multiplayer-game-in-a-brownfields-project-d00ac00bed0e"
isUpdated: true
updatedAt: 1724743579575
---


# 소개

![이미지](/assets/img/2024-08-26-FromLegacytoModernTransformingaJavaMultiplayerGameinaBrownfieldsProject_0.png)

브라운필드 프로젝트를 수행하는 것은 소프트웨어 개발의 시간 캡슐에 다이빙하는 것과 같습니다. 종종 지저분하고 오래된 레거시 코드에 직면하게 되지만 이는 오래된 것을 현대적인 도구와 방법으로 되살리는 기회이기도 합니다. 최근 진행한 프로젝트는 바로 그것이었습니다. 초기에는 소켓 프로그래밍을 사용하여 구축된 Java 기반 멀티플레이어 로봇 게임을 살려냈습니다.

이 글에서는 이 레거시 게임을 현대화하는 과정을 안내하겠습니다. 팀원들과의 매일 스탠드업 미팅 및 회고부터 GitLab, Visual Studio Code, Maven, CI/CD 및 Docker를 활용하는 방법까지, 우리가 직면한 어려움과 활용한 도구, 그리고 협업이 프로젝트의 성공에 어떤 역할을 한 것인지에 대해 공유하겠습니다.

<div class="content-ad"></div>

## 레거시 프로젝트: 배경 및 도전

우리가 상속받은 게임은 로봇 전투 멀티플레이어 게임으로, 로봇(클라이언트)이 서버에 연결되어 세계를 탐험하고 장애물을 피하며 전투에 참여하는 게임입니다. 아이디어는 탄탄하지만, 코드베이스는 최적화에서 멀리 떨어져 있었습니다. 기술 부채로 가득차고, 구조가 부재하며, 최신 개발 방법이 누락되어 있었습니다. 우리의 임무는 더 나은 코드를 개선하고 새로운 기능을 도입하여 이 프로젝트를 현대화하는 것입니다.

## 브라운필드 프로젝트의 도전

![이미지](/assets/img/2024-08-26-FromLegacytoModernTransformingaJavaMultiplayerGameinaBrownfieldsProject_1.png)

<div class="content-ad"></div>

브라운필드 프로젝트는 도전적인 면으로 유명합니다:

- 기술적 부채: 코드가 난잡했으며, 비효율적인 알고리즘과 최소한의 문서화가 있었습니다.

- 오래된 방법: 자동화된 빌드 프로세스나 지속적 통합, 그리고 컨테이너화 기능이 없어서 모든 것이 수동적이었습니다.

- 기능의 한계: 게임의 기능이 기본적이며, 데이터를 지속적으로 저장하거나 쉽게 확장할 수 있는 능력이 부족했습니다.

<div class="content-ad"></div>

## 적절한 도구로 워크플로우 혁신화

이 프로젝트를 해결하기 위해 현대적인 도구와 관행을 활용했습니다:

- Visual Studio Code/IntelliJ 및 Maven을 사용한 개발

![이미지](/assets/img/2024-08-26-FromLegacytoModernTransformingaJavaMultiplayerGameinaBrownfieldsProject_2.png)

<div class="content-ad"></div>

비주얼 스튜디오 코드/인텔리J는 코딩 및 리팩토링을 위한 다재다능한 환경을 제공하여 개발 허브로 자리 잡았습니다:

- 리팩토링: 우리는 비주얼 스튜디오 코드/인텔리J의 강력한 리팩토링 도구를 사용하여 코드를 정리하고 더 모듈화되고 유지보수가 용이한 형태로 만들었습니다.

- 빌드 자동화를 위한 메이븐: 메이븐은 빌드 프로세스 자동화, 의존성 관리, 팀원들의 환경에서 일관된 빌드를 보장하는 데 중요한 역할을 했습니다.

2. 깃랩을 활용한 프로젝트 관리와 협업

<div class="content-ad"></div>


![image](/assets/img/2024-08-26-FromLegacytoModernTransformingaJavaMultiplayerGameinaBrownfieldsProject_3.png)

GitLab was our primary platform for managing the project:

- Version Control: The project was hosted on GitLab, which allowed for smooth version control, branching, and collaboration.

- Issue Tracking: We used GitLab issues to track user stories and tasks, ensuring that every aspect of the project was documented and prioritized.


<div class="content-ad"></div>

- CI/CD 파이프라인: GitLab의 CI/CD 도구는 테스트 및 배포 프로세스를 자동화하여 개발 워크플로우를 크게 개선했습니다.
   
3. 매일 스탠드업 회의와 회고

![이미지](/assets/img/2024-08-26-FromLegacytoModernTransformingaJavaMultiplayerGameinaBrownfieldsProject_4.png)

저희 팀은 프로젝트를 제 시간에 유지하는 데 중요한 역할을 한 애자일 방법론을 준수했습니다.

<div class="content-ad"></div>

- 매일 스탠드업 미팅: 매일 각 팀원이 이룬 성과, 다음에 할 일, 그리고 마주한 어려움 등에 대해 간단히 논의하는 스탠드업 미팅을 가졌습니다. 이러한 회의를 통해 모두가 일정에 맞추고 문제점을 조기에 파악할 수 있었습니다.

- 회고: 각 스프린트가 끝날 때마다 우리는 회고를 진행하여 잘된 점, 그렇지 않은 점, 그리고 개선 방안에 대해 검토했습니다. 이 지속적인 피드백 루프는 우리의 접근 방식을 조정하고 팀 협업을 개선하는 데 중요한 역할을 했습니다.

4. 수용 테스트 및 지속적 통합

![이미지](/assets/img/2024-08-26-FromLegacytoModernTransformingaJavaMultiplayerGameinaBrownfieldsProject_5.png)

<div class="content-ad"></div>

우리의 프로세스에는 품질 보증이 처음부터 내장되어 있었습니다:

- 수용 테스트: 우리는 각 새로운 기능이 프로젝트 요구사항을 충족하는지 확인하기 위해 수용 테스트를 구현했습니다. 이는 테스트 주도 개발 (TDD) 접근 방식의 일부였으며, 테스트는 코드가 구현되기 전에 작성되었습니다.

- 지속적 통합: 이러한 테스트는 GitLab CI/CD 파이프라인의 일부로 자동으로 실행되어, 주 코드베이스에 영향을 주기 전에 문제를 잡아 냈습니다.

5. 동료 지원 및 지식 공유

<div class="content-ad"></div>

프로젝트 동안 동료 지원이 귀중했습니다:

- 코드 리뷰: 우리는 정기적으로 코드 리뷰를 실시했는데, 팀원들이 피드백을 제공하고 개선 사항을 제안했습니다. 이를 통해 코드 품질을 향상시키는 데 도움이 되었을 뿐만 아니라 팀 간 지식 공유도 촉진했습니다.

- 페어 프로그래밍: 특히 어려운 작업을 위해 자주 페어 프로그래밍을 활용했는데, 이를 통해 우리는 서로의 강점을 결합하고 문제를 더 효율적으로 해결할 수 있었습니다.

<div class="content-ad"></div>

6. 도커화 및 웹 API 개발

![image](/assets/img/2024-08-26-FromLegacytoModernTransformingaJavaMultiplayerGameinaBrownfieldsProject_7.png)

애플리케이션을 현대화하면서 일관성과 외부 접근성이 중요해졌습니다:

- 도커 컨테이너: 도커를 사용하여 애플리케이션을 컨테이너화하여 개발자의 컴퓨터나 프로덕션 환경에서 동일하게 실행될 수 있도록 보장했습니다.

<div class="content-ad"></div>

- Javalin을 사용한 WebAPI: 게임의 기능을 확장하기 위해 Javalin을 사용하여 WebAPI를 개발했습니다. 이 API를 통해 외부 시스템이 게임 상태를 조회하고 로봇에 명령을 내릴 수 있었습니다.

## 코드베이스 현대화 과정

1. 코드 정제

첫 번째 작업은 기존 코드를 정리하는 것이었습니다.

<div class="content-ad"></div>

- 리팩터링: 코드 구조를 개선하고 큰 함수를 모듈화하며 클래스 계층 구조를 재구성하여 가독성과 유지 보수성을 향상시켰습니다.

- 문서화: 미래 개발자가 코드베이스를 더 쉽게 이해하고 작업할 수 있도록 자세한 주석과 문서가 추가되었습니다.

2. 게임 기능 향상

그 다음으로, 게임 기능을 확장하였습니다:

<div class="content-ad"></div>

- 로봇 명령: 우리는 로봇의 이동, 전투 참여, 그리고 상태 관리를 위한 명령을 구현했습니다.

- 멀티플레이어 상호작용: 게임은 게임 세계 내에서 여러 로봇 간의 동적 상호작용을 지원하도록 업그레이드되었습니다. 이는 전투와 장애물 회피를 포함합니다.

3. 메이븐과 메이크파일로 빌드 자동화

개발을 간편하게하기 위해:

<div class="content-ad"></div>

- 빌드 자동화를 위한 Maven: Maven을 통합하여 의존성 관리 및 빌드 프로세스를 자동화하였습니다.

- Makefile: Makefile을 생성하여 Maven 명령어 및 기타 작업을 캡슐화하여 빌드 및 배포 프로세스를 간소화하였습니다.

4. CI/CD 파이프라인 및 인수 테스트 구현

코드 품질을 보장하고 지속적인 배포를 위해:

<div class="content-ad"></div>

- CI/CD 파이프라인: 저희는 GitLab에 CI/CD 파이프라인을 구축하여 테스트를 자동으로 실행하고 성공적인 빌드 후에 애플리케이션을 배포했어요.

- 수용 테스트: 수용 테스트는 이 파이프라인의 일부로 실행되었고, 새로운 기능이 병합되기 전에 모든 요구 사항을 충족하는지 확인했습니다.

5. 지속적인 저장 및 WebAPI

마지막으로, 우리는 영속성 및 외부 액세스를 추가했어요.

<div class="content-ad"></div>

- SQLite Database: 저희는 게임 상태를 지속적으로 저장하고 복원할 수 있도록 SQLite 데이터베이스를 통합했습니다.

- WebAPI 개발: Javalin을 사용하여 외부 시스템이 게임의 기능과 데이터에 액세스할 수 있도록 WebAPI를 개발했습니다.

## 배운 점

이 프로젝트를 통해 레거시 시스템을 현대화하는 데 필수적인 몇 가지 중요한 교훈을 얻을 수 있었습니다.

<div class="content-ad"></div>

- 협업의 중요성: 매일 업무점검 회의, 회고 및 동료 지원은 과제를 극복하고 프로젝트 힘을 유지하는 데 중요했습니다.

- 현대적인 도구 사용의 중요성: GitLab, Visual Studio Code, Maven 및 Docker 사용은 코드베이스를 개선하고 일관된 신뢰할 수 있는 개발 방법을 보장하는 데 중요했습니다.

- 지속적인 개선: 정기 회고를 통해 계속해서 우리의 방법론을 개선하고 제품 및 팀 역동성을 향상시킬 수 있었습니다.

## 결론

<div class="content-ad"></div>

![이미지](/assets/img/2024-08-26-FromLegacytoModernTransformingaJavaMultiplayerGameinaBrownfieldsProject_8.png)

이번에 레거시 자바 게임을 살리는 일은 어려운 도전이었지만 놀라운 보람 있는 경험이었어요. 현대적인 도구, 애자일 방법론, 그리고 강력한 팀 협업을 결합하여 우리는 오래된 코드베이스를 견고하고 확장 가능하며 유지보수가 쉬운 애플리케이션으로 변모시켰어요. 이 프로젝트는 게임을 향상시킬 뿐만 아니라 복잡한 브라운필드 프로젝트에 대처하기 위한 현대적인 개발 방법론과 팀워크의 중요성을 강조했어요.

만약 당신이 브라운필드 프로젝트에 참여하고 있다면, 성공은 당신이 작성하는 코드뿐만 아니라 이를 지원하는 프로세스, 도구, 그리고 협업에 있는 것을 명심해주세요.