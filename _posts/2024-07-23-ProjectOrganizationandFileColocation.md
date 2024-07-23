---
title: "Nextjs 14 프로젝트 구성 및 파일 배치 방법"
description: ""
coverImage: "/assets/nextjs.png"
date: 2024-07-23 20:10
ogImage: 
  url: /assets/nextjs.png
tag: Tech
originalTitle: "Project Organization and File Colocation"
link: "https://nextjs.org/docs/app/building-your-application/routing/colocation"
---


# 프로젝트 구조화 및 파일 동시 배치

라우팅 폴더 및 파일 규칙 외에 Next.js는 프로젝트 파일을 어떻게 구성하고 동시 배치할지에 대해 규범을 갖지 않습니다.

이 페이지에서는 기본 동작 및 프로젝트 구조화에 사용할 수 있는 기능을 공유합니다.

- 기본적으로 안전한 동시 배치
- 프로젝트 구조화 기능
- 프로젝트 구조화 전략

<div class="content-ad"></div>

## 기본적으로 안전한 공존

앱 디렉토리에서 중첩된 폴더 계층 구조가 경로 구조를 정의합니다.

각 폴더는 URL 경로의 해당 세그먼트에 매핑되는 경로 세그먼트를 나타냅니다.

그러나 경로 구조가 폴더를 통해 정의되더라도 route.js 파일이나 page.js 파일이 경로 세그먼트에 추가되기 전까지 경로에 대해 공개적으로 접근할 수 없습니다.

<div class="content-ad"></div>

아래는 마크다운 형식으로 변환한 내용입니다.


![ProjectOrganizationandFileColocation_0](/assets/img/2024-07-23-ProjectOrganizationandFileColocation_0.png)

그리고 경로가 공중에 공개되더라도, 클라이언트로 전송되는 것은 페이지.js 또는 route.js에서 반환한 콘텐츠만 전송됩니다.

![ProjectOrganizationandFileColocation_1](/assets/img/2024-07-23-ProjectOrganizationandFileColocation_1.png)

즉, 프로젝트 파일은 앱 디렉토리의 route 세그먼트 내에 안전하게 공존할 수 있으며 실수로 route를 통해 접근되지 않습니다.


<div class="content-ad"></div>


<img src="/assets/img/2024-07-23-ProjectOrganizationandFileColocation_2.png" />

> 좋아 알아두기:
이것은 모든 페이지 디렉토리와 다릅니다. 페이지 디렉토리에 있는 모든 파일은 경로로 간주됩니다.
프로젝트 파일을 app에 함께 배치할 수 있지만 반드시 해야 하는 것은 아닙니다. 원하는 경우 app 디렉토리 외부에 유지할 수도 있습니다.

## 프로젝트 조직 특징

Next.js는 프로젝트를 구성하는 데 도움이 되는 여러 기능을 제공합니다.


<div class="content-ad"></div>

### 개인 폴더

프라이빗 폴더는 밑줄과 함께 폴더의 이름을 작성하여 만들 수 있습니다: _폴더이름

이것은 해당 폴더가 라우팅 시스템에서 고려되지 않아야 함을 나타내며, 따라서 해당 폴더와 그 하위 폴더 모두 라우팅에서 제외됩니다.

![프로젝트 조직 및 파일 동시 위치 이미지](/assets/img/2024-07-23-ProjectOrganizationandFileColocation_3.png)

<div class="content-ad"></div>

앱 디렉토리의 파일들은 기본적으로 안전하게 함께 둘 수 있기 때문에 공동 위치에 대한 개인 폴더는 필요하지 않습니다. 그러나 다음과 같은 경우에 유용할 수 있습니다:

- UI 로직과 라우팅 로직 구분하기.
- 프로젝트 및 Next.js 생태계 전반에 걸쳐 내부 파일을 일관되게 구성하기.
- 코드 편집기에서 파일을 정렬하고 그룹화하기.
- 장래의 Next.js 파일 규칙과의 잠재적인 이름 충돌 피하기.

> 알아 두면 좋은 사실
프레임워크 규칙은 아니지만, 동일한 밑줄 패턴을 사용하여 개인 폴더 외부의 파일을 "private"로 표시하는 것도 고려해 볼 만합니다.
밑줄로 시작하는 URL 세그먼트를 만드려면 폴더 이름 앞에 %5F(밑줄의 URL 인코딩된 형태)를 접두어로 붙일 수 있습니다: %5F폴더이름.
개인 폴더를 사용하지 않는 경우 예상치 못한 이름 충돌을 방지하고자 Next.js 특수 파일 규칙을 알아두는 것이 도움이 될 수 있습니다.

### 라우트 그룹

<div class="content-ad"></div>

Route 그룹은 괄호로 폴더를 감싸서 생성할 수 있어요: (폴더이름)

이렇게 하면 이 폴더가 조직적인 목적으로 사용되며, 라우트의 URL 경로에 포함되지 않아야 함을 나타냅니다.

![그림](/assets/img/2024-07-23-ProjectOrganizationandFileColocation_4.png)

Route 그룹은 다음과 같은 경우에 유용해요:

<div class="content-ad"></div>

- 라우트를 그룹으로 구성하기 (예: 사이트 섹션, 의도 또는 팀별로)
- 동일한 라우트 세그먼트 수준에서 중첩 레이아웃 활성화:
  - 동일한 세그먼트에서 여러 중첩 레이아웃 생성, 여러 루트 레이아웃 포함
  - 공통 세그먼트 내 일부 라우트에 레이아웃 추가
- 동일한 세그먼트에서 여러 중첩 레이아웃 생성, 여러 루트 레이아웃 포함
- 공통 세그먼트 내 일부 라우트에 레이아웃 추가

### src 디렉토리

Next.js는 응용 프로그램 코드(응용 프로그램 포함)를 선택적으로 src 디렉토리에 저장하는 것을 지원합니다. 이를 통해 프로젝트 구성 파일을 주로 프로젝트 루트에 두고 응용 프로그램 코드를 분리할 수 있습니다.

![Project Organization and File Colocation](/assets/img/2024-07-23-ProjectOrganizationandFileColocation_5.png)

<div class="content-ad"></div>

### 모듈 경로 별명

Next.js는 모듈 경로 별명을 지원하여 깊게 중첩된 프로젝트 파일 사이의 가져오기를 더 쉽게 읽고 유지할 수 있습니다.

```js
// 이전
import { Button } from '../../../components/button'
 
// 이후
import { Button } from '@/components/button'
```

## 프로젝트 구성 전략

<div class="content-ad"></div>

Next.js 프로젝트에서 파일과 폴더를 조직하는 방식에는 "옳고 그른" 방법이 없습니다.

다음 섹션은 일반적인 전략에 대한 매우 고수준 개요를 제공합니다. 간단히 말해, 가장 중요한 것은 본인과 팀원들에게 잘 맞는 전략을 선택하고 프로젝트 전체에서 일관성을 유지하는 것입니다.

> 참고사항: 아래 예시에서는 components 및 lib 폴더를 일반적인 플레이스홀더로 사용하고 있습니다. 이들의 네이밍은 특별한 프레임워크의 의미를 갖지 않으며, 프로젝트에서는 ui, utils, hooks, styles 등 다른 폴더를 사용할 수 있습니다.

### 앱 외부에 프로젝트 파일 저장하기

<div class="content-ad"></div>

이 전략은 프로젝트의 루트에 공유 폴더에 모든 애플리케이션 코드를 저장하고 앱 디렉토리는 순수하게 라우팅 용도로 유지하는 방식입니다.

![이미지](/assets/img/2024-07-23-ProjectOrganizationandFileColocation_6.png)

### 상위 폴더에 프로젝트 파일 저장

이 전략은 앱 디렉토리의 루트에 공유 폴더에 모든 애플리케이션 코드를 저장합니다.

<div class="content-ad"></div>

아래는 Markdown 형식으로 변경한 것입니다.


![](/assets/img/2024-07-23-ProjectOrganizationandFileColocation_7.png)

### 기능 또는 라우트별로 프로젝트 파일 분리하기

이 전략은 전역으로 공유된 애플리케이션 코드를 루트 앱 디렉토리에 저장하고, 보다 구체적인 애플리케이션 코드를 해당 코드를 사용하는 라우트 세그먼트로 분할합니다.

![](/assets/img/2024-07-23-ProjectOrganizationandFileColocation_8.png)
