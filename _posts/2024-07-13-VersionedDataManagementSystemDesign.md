---
title: "버전 관리 데이터 시스템 디자인 방법"
description: ""
coverImage: "/assets/img/2024-07-13-VersionedDataManagementSystemDesign_0.png"
date: 2024-07-13 02:32
ogImage: 
  url: /assets/img/2024-07-13-VersionedDataManagementSystemDesign_0.png
tag: Tech
originalTitle: "Versioned Data Management System Design"
link: "https://medium.com/gitconnected/versioned-data-management-system-design-6e27598eb023"
isUpdated: true
---




![2024-07-13-VersionedDataManagementSystemDesign_0.png](/assets/img/2024-07-13-VersionedDataManagementSystemDesign_0.png)

# 소개

이전에, 분산 원장 시스템을 소개했습니다. 기술적인 측면에서 일관적으로 버전 기록을 지원하는 데이터 저장소를 구축하는 방법을 설명했습니다. 이를 확장하여, 이 게시물에서는 Git과 유사점을 비추며, 데이터 관리 워크플로우를 소개합니다.

# 데이터의 버전 기록

<div class="content-ad"></div>

분산 원장 시스템에서 논의된 대로, 우리는 각 데이터 조각에 대해 선형 기록을 유지합니다. 가장 최신 버전 번호를 가진 항목을 검색하여 현재 상태를 확인할 수 있습니다. 또한 특정 버전 번호를 통해 이전의 과거 상태에 액세스할 수 있습니다.

특정 버전에 대한 델타를 알기 위해서는 두 가지 방법이 있습니다: 1) 현재 상태와 이전 상태를 비교하거나 빼는 방법, 또는 2) 데이터 항목과 함께 델타를 저장하는 방법. 아래 다이어그램은 후자의 방법을 설명하고 있습니다.

![Diagram](/assets/img/2024-07-13-VersionedDataManagementSystemDesign_1.png)

금융 계정 데이터, 공급망 재고 데이터 및 중요 구성 데이터 등 다양한 유형의 데이터가 모든 과거 변이를 포괄적으로 기록하는 것에서 이점을 얻을 수 있습니다.

<div class="content-ad"></div>

구체적인 예로 은행의 입출금 계좌를 생각해보세요. 현재 잔액을 추적하는 것을 넘어, 모든 거래의 상세 기록에 접근할 수 있는 것이 중요합니다.

이 시스템은 소스 코드용 버전 관리 시스템과 밀접하게 닮아 있습니다. 두 시스템 모두 불변한 변경 로그를 유지하지만 핵심적인 차이점이 있습니다: 소스 코드의 경우 델타만 추적하지만, 버전별 데이터 시스템에서는 현재 상태를 추적하는 것이 더 중요합니다. 델타를 저장하는 것은 선택 사항이며, 현재 상태와 이전 상태를 비교하거나 빼는 방식으로 쉽게 파생할 수 있습니다.

게다가 위 다이어그램은 "상태"가 정수 숫자임을 보여줍니다. 하지만 실제로는 임의 키-값 저장소의 값이 될 수 있습니다.

# 요청 관리 워크플로우

<div class="content-ad"></div>

대부분의 경우, 과거 변이를 추적하는 능력이 일반적으로 충분합니다. 그러나 일부 상황에서는 변이의 안전을 보장하는 것이 균형을 이루는 것이 중요합니다. 다음은 우리가 마주한 몇 가지 예시입니다:

- 권한: 변이의 이니셔티에터가 변경을 실시할 권한이 있는 사람이 아닐 수 있습니다.
- 시뮬레이션 및 유효성 검사: 변경을 실시하기 전에 추가적인 유효성 검사나 시뮬레이션이 필요합니다.
- 미래 계획: 변경을 준비하고 나중에 변경을 실시하려고 합니다.

위와 같은 사용 사례들 중 어느 것이든 변이가 스테이징 모드에 있는 것이 유익합니다.

아래 그림과 같이 각 제안된 변이에 임시 버전 번호가 할당됩니다. 우리는 변이의 합법성을 검증하기 위해 시뮬레이션을 실행할 수 있습니다. 성공적인 시뮬레이션과 유효성 검사를 거친 후, 제안된 변이는 새로운 영구 버전 번호와 함께 주요 히스토리 장부에 통합됩니다.

<div class="content-ad"></div>

![Versioned Data Management System Design](/assets/img/2024-07-13-VersionedDataManagementSystemDesign_2.png)

We can call this process the request management workflow, similar to the code review process. The simulations and validations done on a staging version of the mutation can be likened to the unit tests carried out during a code review.

## Database Design

In this system, we need a minimum of three primary tables:

<div class="content-ad"></div>

- Entity ID 및 Version 번호의 복합 키인 주 키로 엔터티의 모든 버전을 유지하는 원장 테이블이 있습니다. 자세한 내용은 이전 게시물에서 확인할 수 있습니다.
- 변이 테이블은 아직 원장 테이블에 적용되지 않은 모든 보류 중인 변이를 추적하는 역할을 합니다.
- 유효성 테이블은 각 변이에 대한 유효성 결과를 기록하는 역할을 합니다. 유효성의 상태는 보류 중, 성공, 실패 중 하나일 수 있습니다. “검증자 이름” 열은 단일 변이에 대한 여러 유효성을 구별하는 데 사용됩니다. 마지막으로 “자세한 결과” 열에는 유효성의 성공 또는 실패 이유를 나타내는 자세한 메시지가 저장됩니다.

![2024-07-13-VersionedDataManagementSystemDesign_3.png](/assets/img/2024-07-13-VersionedDataManagementSystemDesign_3.png)

# API 디자인

요청 관리 워크플로를 지원하기 위해 다음 두 가지 주요 API가 있습니다.

<div class="content-ad"></div>

## (1) 돌연변이 API 제출

이 API에서는 entity id, 기본 버전 및 새 값이 주어지면 제출된 돌연변이에 대한 고유한 돌연변이 ID가 포함된 응답이 제공됩니다.

```js
service VersionedDataService {
  rpc SubmitMutation (MutationRequest) returns (MutationResponse);
}

message MutationRequest {
  string entity_id = 1;
  int32 base_version = 2;
  string new_value = 3;
}

message MutationResponse {
  string mutation_id = 1;
}
```

## (2) 돌연변이 확정

<div class="content-ad"></div>

이 API에서는 변이 ID가 주어지면 모든 유효성 검사가 성공적인 경우 변이가 원장에 적용됩니다.

```js
service VersionedDataService {
  rpc CommitMutation (CommitMutationRequest) returns (CommitMutationResponse);
}

message CommitMutationRequest {
  string mutation_id = 1;
}

message CommitMutationResponse {
  string message = 1;
}
```

이 게시물에서는 다른 중요하지 않은 API를 제외했습니다.

# 결론

<div class="content-ad"></div>

일반적으로, 데이터가 변경된 내용, 변경된 이유, 누가 언제 변경했는지 파악하기가 쉽지 않습니다. 트래킹은 보통 로그로써 나중에 추가됩니다. 이 프레임워크를 통해 명확한 추적 가능성을 위한 신뢰할 수 있고 일관된 방법을 수립했습니다.

한편, 대부분의 변이는 일회성으로 이루어지며 유효성 검사 없이 수행됩니다. 이 프레임워크는 변경 사항의 영향을 선제적으로 드러내고 회귀를 도입할 수 있는 변이를 방지함으로써 신뢰성을 강화했습니다.

오늘은 여기까지입니다. 읽어 주셔서 감사합니다!