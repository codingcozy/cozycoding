---
title: "시스템 디자인 방법 정리"
description: ""
coverImage: "/assets/img/2024-08-26-SystemDesignRevisionSeries1_0.png"
date: 2024-08-26 18:51
ogImage: 
  url: /assets/img/2024-08-26-SystemDesignRevisionSeries1_0.png
tag: Tech
originalTitle: "System Design Revision Series (1)"
link: "https://medium.com/@rishabhsoti16/system-design-revision-series-1-f1496291fc69"
isUpdated: true
updatedAt: 1724742612253
---


저희 시스템 디자인 복습 시리즈의 첫 번째 기사에 오신 것을 환영합니다! 이 시리즈는 완전한 시스템 디자인 개념을 빠르게 복습하고 숙달할 수 있도록 설계되었습니다. 저는 이를 간결하고 명료하게 유지하고 있으니, 급하게 인터뷰 준비를 하거나 빠르게 복습하기에 안성맞춤입니다. 경험이 많은 개발자이든 시스템 디자인에 새로 입문한 사람이든, 이 간결한 안내서를 통해 당신이 자신있게 인터뷰 질문에 답하고 핵심 개념을 너무 깊게 파헤치지 않고 이해할 수 있도록 준비할 것입니다.

![System Design Revision Series](/assets/img/2024-08-26-SystemDesignRevisionSeries1_0.png)

# 확장성

## 확장성이란 무엇인가요?

<div class="content-ad"></div>

테이블 태그를 마크다운 형식으로 변경해주세요.

<div class="content-ad"></div>

스케일 가능한 시스템은 수요 증가가 성능을 저하시키지 않도록 보장합니다.

## 효과적인 확장을 위한 10가지 전략

## 1. 수평 확장: 자원 추가

- 기존 기계의 RAM, CPU 및 저장 공간 확장.

<div class="content-ad"></div>

## 2. 수직 확장: 기계 추가

- 부하를 분산시키기 위해 서버 수를 늘립니다.

## 3. 부하 분산: 고르게 분배

- 병목 현상을 방지하기 위해 부하를 서버 전체에 고르게 분산시킵니다.

<div class="content-ad"></div>

## 4. 캐싱: 데이터 액세스 속도 향상

- 자주 액세스하는 데이터를 RAM과 같은 메모리에 저장합니다.

## 5. CDN (콘텐츠 전송 네트워크): 근접성이 중요합니다

- 사용자에게 더 가까운 위치에서 정적 에셋(이미지, 비디오)를 제공합니다.

<div class="content-ad"></div>

## 6. 파티셔닝: 분할 정복

- 효율적인 작업 부하 분산을 위해 데이터를 여러 서버로 분할합니다.
- 예시: 만약 데이터셋이 (A-Z)라면
  - (A-G) - Server 1
  - (H-O) - Server 2
  - (P-Z) - Server 3

## 7. 비동기 통신: 작업 우선 순위 지정

- 오랜 시간이 걸리거나 중요하지 않은 작업을 배경으로 옮깁니다.

<div class="content-ad"></div>

## 8. 마이크로서비스 아키텍처: 독립적인 스케일링

- 응용 프로그램을 개별적으로 확장할 수 있는 독립적인 서비스로 분해합니다.

## 9. 자동 확장: 동적 조정

- 현재 부하에 따라 서버 리소스를 자동으로 조정합니다.

<div class="content-ad"></div>

## 10. Multi-Region Deployment: Global Reach

- 여러 클라우드 지역에 애플리케이션을 배포하여 지연 시간을 줄이고 신뢰성을 향상시킵니다.

# 결론

확장성은 시스템이 성능 문제 없이 원활하게 성장하는 것을 의미합니다. 이러한 전략을 적용하여 견고하고 반응성이 뛰어나며 어떤 규모에도 준비된 시스템을 설계할 수 있습니다. 저희의 시스템 설계 개정 시리즈에서 더 많은 통찰력을 기대해 주세요!

<div class="content-ad"></div>

이 시리즈의 다른 기사 및 유익한 주제를 더 보려면 팔로우해주세요! 🚀📚