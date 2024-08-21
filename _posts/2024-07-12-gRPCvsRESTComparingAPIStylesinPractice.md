---
title: "gRPC vs REST 실전에서 API 스타일 비교하기"
description: ""
coverImage: "/assets/img/2024-07-12-gRPCvsRESTComparingAPIStylesinPractice_0.png"
date: 2024-07-12 22:38
ogImage: 
  url: /assets/img/2024-07-12-gRPCvsRESTComparingAPIStylesinPractice_0.png
tag: Tech
originalTitle: "gRPC vs. REST: Comparing API Styles in Practice"
link: "https://medium.com/better-programming/grpc-vs-rest-comparing-api-styles-in-practice-28d2a7c9a349"
isUpdated: true
---




<img src="/assets/img/2024-07-12-gRPCvsRESTComparingAPIStylesinPractice_0.png" />

이 기사에서는 REST 아키텍처와 최근에 등장한 gRPC라는 새로운 요소를 활용하여 만들어진 API에 대해 알아볼 것입니다. 우리의 목표는 이러한 아키텍처 스타일을 사용하여 API를 구축하는 방법을 이해하는 것입니다.

# REST란 무엇인가요?

2000년에 Roy Fielding은 박사학위 논문을 쓰고 "Representational State Transfer" 또는 REST라고 불리는 분산 시스템을 위한 아키텍처를 세계에 소개했습니다. 그는 웹 자체를 움직이는 원칙에 근거한 시스템 아키텍처에 대한 여러 제약사항을 개요로 설명했습니다. 목표는 관심사 분리와 확장성을 보장할 수 있도록 하는 분산 시스템 구축에 대한 수십 년의 지혜를 활용하는 것이었습니다.

<div class="content-ad"></div>

REST 철학의 중심에는 하이퍼미디어와 하이퍼텍스트 개념이 있습니다(HTTP의 "H"처럼). RESTful API를 통해 사용자는 URL로 식별된 리소스와 다양한 하이퍼미디어 표현(JSON, XML 또는 HTML과 같은)을 인코딩하여 상호 작용할 수 있습니다. 이러한 리소스를 수정하면 응용 프로그램 상태가 변경됩니다. 일반적으로 클라이언트는 RESTful 서비스를 구축하고 상호 작용하기 위해 프로토콜로서 HTTP를 사용합니다.

REST에 대한 종종 간과되지만 중요한 개념 중 하나는 응용 프로그램 상태의 엔진으로서의 하이퍼미디어(HATEOAS)입니다. 간단히 말해, 응용 프로그램의 상태를 변경하는 데 어떤 메서드를 호출할지 클라이언트가 선택하는 대신, 하이퍼미디어 자체가 클라이언트가 상호 작용할 수 있는 리소스와 그 시간을 결정합니다. 이는 현재 웹 페이지와 상호 작용하는 방법과 유사합니다. 사용자는 단일 URL을 방문하면 됩니다. 그리고 결과 페이지에서 제공된 링크는 사용자가 어디를 찾아볼 수 있는지 알려줍니다.

REST는 웹이 작동하는 데 필요한 모든 기술(특히 HTTP, TCP, URL 및 JSON, XML, HTML과 같은 하이퍼미디어 유형)을 활용하도록 설계되었습니다. 따라서 RESTful API를 구축하거나 사용하기 위해 특별한 소프트웨어가 필요하지 않습니다.

# gRPC란 무엇인가요?

<div class="content-ad"></div>

분산 클라이언트-서버 통신 방식인 "Remote Procedure Call" (RPC)은 적어도 80년대 초에 거슬러 올라가는 것은 아무것도 없는 새로운 것이 아닙니다. 그러나 구글의 gRPC라는 구현은 2015년에 발명되어 웹 시스템 간 통신을 가능하게 하는 성능 중심 프레임워크로 등장했습니다. 무엇보다도 최신 HTTP/2 프로토콜을 활용하기 위해 만들어진 gRPC는 클라이언트와 서버 간 양방향 통신을 지원하며 여러 개의 메시지를 요청 또는 응답하는 스트리밍 기능을 제공합니다. 이는 높은 성능과 처리량을 갖는 애플리케이션을 효율적으로 구축하는 데 매우 유용합니다. 성능은 gRPC의 가장 큰 장점입니다.

RESTful API와는 달리 RPC 기반 API는 인터페이스 정의 언어 (IDL)를 사용하여 프로시저(또는 메서드)를 정의합니다. 이를 통해 프로그래밍 언어에서 사용할 수 있는 서버 및 클라이언트 스텁 클래스를 인터페이스 정의로부터 생성하는 도구가 필요합니다. 구글은 gRPC를 위해 "Protocol Buffers" 또는 간단히 "protobufs"라고 불리는 자체 형식을 개발하여, 속도를 위해 설계된 매우 효율적인 와이어 직렬화 형식을 활용하고 있습니다.

# REST 및 gRPC를 사용한 주소록 예제

REST와 gRPC 아키텍처 스타일 간의 차이점을 보여주는 일련의 샘플 프로젝트를 살펴보겠습니다. 검토할 모든 프로젝트는 Svelte를 기반으로한 간단한 프론트엔드 UI와 NodeJS 백엔드 API를 사용하여 구현된 풀스택 주소록 앱입니다. 샘플 프로젝트 모노레포지터리는 GitHub의 anthonydmays/grpc-vs-rest에서 확인할 수 있습니다.

<div class="content-ad"></div>

간단하게 유지하기 위해 몇 가지 기능과 몇 가지 핵심 제약 사항이 구현되어 있습니다.

- 기본 CRUD 작업 구현. 여기서는 기능 요구 사항 측면에서 특별한 것이 없습니다. 단순히 모든 연락처를 나열하고 단일 연락처를 생성, 업데이트 또는 삭제할 수 있기를 바랍니다.
- 전체 스택 유형 안전성. 우리 전체 코드베이스가 유형 안전하며 자동완성과 같은 최신 IDE의 편의 기능을 지원하는지 확인할 수 있어야 합니다.
- 브라우저에서 사용 가능한 API. 모든 클라이언트가 브라우저 환경에서 API를 사용할 수 있도록하고 싶습니다.
- 단위 테스트 가능한 코드. 우리 API에 대한 단위 테스트를 비교적 쉽게 작성할 수 있어야 하며 우리가 기대하는 동작을 보장하기 위한 테스트를 수행해야 합니다.
- 최소한의 차이점. 스타일 간 이동 시 차이점을 강조하기 위해, 샘플 프로젝트 간의 차이를 최소화하기 위해 최선을 다하였습니다. 이 데모들은 제품 수준 시스템을 작성하는 방법을 보여주는 것이 아니라 각 스타일을 충분히 이해하는 데 도움을 줄 수 있도록 고안되었습니다.

# REST API 앱 검토

먼저 API 및 클라이언트를 실행해 봅시다. anthonydmays/grpc-vs-rest 리포지토리를 클론하고 터미널을 열어 rest-api-app 디렉토리로 변경하세요. 종속성을 설치하고 apiTypes 패키지를 빌드하는 README 지침을 따라주세요. 편의를 위해 아래 명령어를 제공합니다:

<div class="content-ad"></div>

```js
$ git clone https://github.com/anthonydmays/grpc-vs-rest
$ cd rest-api-app
$ npm install
$ npm run build:apiTypes
```

다른 터미널 창에서 npm run dev:api 명령어를 실행하여 서버를 실행하고 npm run dev:client를 사용하여 클라이언트를 실행합니다. 이제 http://localhost:5173로 이동하여 기본 UI를 볼 수 있어야 합니다.

API에 대해 한 번 주의를 기울여 봅시다. REST 원칙과 일치하도록 만든 API가 무엇인지 확인하기 위해 curl을 사용하여 단일 연락처를 검색할 것입니다. 응답을 통해 우리의 API가 REST 아키텍처 제약 조건을 만족시키는지 확인하겠습니다.

```js
$ curl -iX GET http://localhost:9090/v1/contacts/1
HTTP/1.1 200 OK
X-Powered-By: Express
Access-Control-Allow-Origin: *
Content-Type: application/json; charset=utf-8
Content-Length: 349
ETag: W/"15d-bXqO1UHKOU8wml7G/sca1xVRseU"
Date: Mon, 20 Feb 2023 22:54:24 GMT
Connection: keep-alive
Keep-Alive: timeout=5

{"resource":{"uri":"contacts/1","firstName":"Hedda","lastName":"Ready","email":"hready0@ftc.gov","phoneNumber":"919-521-1661","_links":{"self":{"href":"http://localhost:9090/v1/contacts/1","type":"GET"},"allContacts":{"href":"http://localhost:9090/v1/contacts","type":"GET"},"delete":{"href":"http://localhost:9090/v1/contacts/1","type":"DELETE"}}
```

<div class="content-ad"></div>

- **자원의 일관된 식별**: 우리 API는 URL을 통해 자원을 식별합니다. API를 통해 이용 가능한 모든 자원에 대해 우리가 사용하거나 조작할 수 있는 고유한 URL이 있습니다.
- **표현을 통한 자원 조작**: 우리는 HTTP GET 요청을 통해 http://localhost:9090/v1/contacts로 접속함으로써 연락처 목록을 볼 수 있습니다. OPTIONS 요청을 하면 이 자원에 POST로 새 연락처를 생성할 수도 있음을 확인할 수 있습니다. 연락처의 JSON 표현을 보내면 해당 연락처의 속성을 업데이트할 수 있습니다.
- **자기 기술 메시지**: 연락처 자원은 application/json 하이퍼미디어 형식을 사용하여 표현됩니다. 우리는 사용 가능한 필드와 기본 유형(string, 숫자, 배열 및 JSON이 지원하는 다른 유형 등)을 정확히 확인할 수 있습니다.
- **응용 상태의 하이퍼미디어 엔진(HATEOAS)**: 중요한 점은 자원이 우리에게 이 자원 또는 관련 자원에서 수행할 수 있는 다른 작업을 알려주는 링크를 제공한다는 것입니다.
- **캐싱 가능성**: HTTP만 사용하기 때문에 자원이 캐시될 수 있는지를 나타내는 데 필요한 모든 메커니즘을 이미 가지고 있습니다. 또한 ETag를 사용하여 동일한 정보를 전달할 수 있습니다.
- **상태 없음**: 클라이언트와 API 서버 간 상호 작용은 상태를 갖지 않는 상태입니다. 요청을 이해하는 데 필요한 모든 것이 요청 자체에 제공됩니다. 서버는 새 요청을 충족하기 위해 이전 요청이나 응답에 대한 상태를 저장하지 않습니다.

이제 클라이언트 앱을 살펴보면 프런트엔드 구현에 약간의 독특함이 있음을 알 수 있습니다. RESTful 제약을 준수하기 위해 API에서 제공된 하이퍼미디어 자원의 링크를 사용해야 합니다. 클라이언트에서 URL 구성이 발생하지 않는다는 사실을 강조하는 것이 중요합니다. API로부터 받은 것을 사용하기만 합니다.

![image](/assets/img/2024-07-12-gRPCvsRESTComparingAPIStylesinPractice_1.png)


<!-- packages/client/src/routes/+page.svelte -->

<!-- 연락처 목록을 탐색하기 위한 페이징 컨트롤 -->
<nav>
  {#if data._links?.firstPage}
    <a href="?url={encodeURIComponent(data._links.firstPage.href)}">{'<<'}</a> |
  {/if}
  {#if data._links?.previousPage}
    <a href="?url={encodeURIComponent(data._links.previousPage.href)}">{'<'}</a> |
  {/if}
  {#if data._links?.nextPage}
    <a href="?url={encodeURIComponent(data._links.nextPage.href)}">{'>'}</a> |
  {/if}
  {#if data._links?.lastPage}
    <a href="?url={encodeURIComponent(data._links.lastPage.href)}">{'>>'}</a>
  {/if}
</nav>


<div class="content-ad"></div>

```js
/** 파일: packages/client/src/routes/+page.ts */


import { env } from '$env/dynamic/public';
import type { ListContactsResponse } from '@grpc-vs-rest/api-types';
import type { PageLoad } from './$types';

/** 페이지 데이터를 로드하는 핸들러입니다. */
export const load = (async ({ url }) => {
  // UI의 앵커 링크는 API에서 제공하는 내용에 따라 URL 매개변수를 구성합니다.
  const apiEndpoint =
    url.searchParams.get('url') ||
    env.PUBLIC_API_ENDPOINT ||
    'http://localhost:9090/v1/contacts';

  const res = (await (
    await fetch(`${apiEndpoint}`)
  ).json()) as ListContactsResponse;

  return res;
}) satisfies PageLoad;
```

# gRPC 살펴보기

조금 변화를 주어 gRPC API 구현을 살펴 보겠습니다. grpc-api-app 디렉토리에서 찾을 수 있습니다. 이 프로젝트의 구조는 rest 앱과 동일하며, 변경된 요소에 대해 집중하겠습니다. 먼저 apiTypes 프로젝트부터 시작해 보겠습니다.

서버와 클라이언트가 서로 통신하는 계약을 정의하기 위해 서비스, 함수, 매개변수 및 모델 정의별로 프로토를 정의합니다. 우리의 apiTypes 프로젝트에는 proto.contacts.v1 네임스페이스 아래에서 단일 프로토가 정의되어 있습니다.


<div class="content-ad"></div>

```js
/** file: packages/apiTypes/proto/contacts/v1/contacts.proto */

// 연락처 컬렉션을 관리하기 위한 API입니다.
service ContactsService {
  // 모든 사용 가능한 연락처를 나열합니다.
  rpc ListContacts(ListContactsRequest) returns (ListContactsResponse);

  // 특정 연락처를 검색합니다.
  rpc GetContact(GetContactRequest) returns (GetContactResponse);

  // 제공된 정보로 연락처를 업데이트합니다.
  rpc UpdateContact(UpdateContactRequest) returns (UpdateContactResponse);

  // 제공된 ID로 연락처를 삭제합니다.
  rpc DeleteContact(DeleteContactRequest) returns (google.protobuf.Empty);

  // 연락처를 생성합니다.
  rpc CreateContact(CreateContactRequest) returns (CreateContactResponse);
}
```

두 번째 큰 변화는 이제 자동으로 생성된 클라이언트 및 서버 스텁을 가지고 있다는 것입니다. 이 작업에서는 buf와 protobuf-ts 플러그인을 사용하여 관용적인 TypeScript 클래스와 객체를 생성하기로 결정했습니다. 이러한 클래스는 서버와 클라이언트에서 사용할 유형을 설명하는데 그치지 않고, 전선을 통해 메시지를 직렬화하고 전송하는 데 사용되는 실제 gRPC 구현도 포함되어 있습니다.

```js
# /proto/buf.gen.yaml
version: v1
plugins:
  - name: ts
    out: src/
    opt: generate_dependencies,long_type_string,server_generic,client_generic
```

다음으로 서버에 대해 이해해보겠습니다. protobuf-ts에서 생성된 단순한 서비스 인터페이스 덕분에 구현은 상당히 손쉽습니다. 이 데모를 위해 Express를 서버에서 제거하고 내장된 node:http2 모듈을 통해 서비스를 호스팅할 계획입니다.


<div class="content-ad"></div>

```js
/** 파일: packages/api/src/index.ts */

import api = require('@grpc-vs-rest/api-types');
import { Server, ServerCredentials } from '@grpc/grpc-js';
import { adaptService } from '@protobuf-ts/grpc-backend';
import { ServerCallContext } from '@protobuf-ts/runtime-rpc';
import {
  createContact,
  deleteContact,
  getContact,
  getContacts,
  getContactsCount,
  updateContact,
} from './contacts.js';

export class ContactsService implements api. IContactsService {
  async listContacts(
    request: api.ListContactsRequest,
    context: ServerCallContext,
  ): Promise<api.ListContactsResponse> {
    let { pageNumber, pageSize, orderBy } = request;
    pageSize = pageSize || 25;
    pageNumber = pageNumber ?? 0;
    const contacts = getContacts({ pageNumber, pageSize, orderBy });
    return {
      contacts,
      pageNumber,
      pageSize,
      orderBy,
      totalCount: getContactsCount(),
    };
  }
  // 남은 서버 메소드 구현하기.
}

const port = process.env.PORT || 9090;
const server = new Server();
server.bindAsync(
  `0.0.0.0:${port}`,
  ServerCredentials.createInsecure(),
  () => {
    server.start();
    server.addService(
      ...adaptService(api.ContactsService, new ContactsService()),
    );
    console.log(`server is running on 0.0.0.0:${port}`);
  },
);

```

이제 클라이언트를 실행하고 Wireshark와 같은 패킷 스니퍼를 사용하여 API 요청을 검사하면 이전 REST 방식으로 구현한 동일한 연락처 검색 메서드의 응답이 132바이트로 크게 줄어든 것을 확인할 수 있습니다. 이전에 전달된 349바이트의 JSON과 비교해보세요.

![그림](/assets/img/2024-07-12-gRPCvsRESTComparingAPIStylesinPractice_2.png)

더 주목할만한 중요한 변화가 하나 더 있습니다. 명확치 않은 점은 gRPC 클라이언트 코드가 서버 측에서 실행되어야 한다는 점입니다 (메인 페이지의 로드 함수가 서버 측 렌더링 페이지를 나타내도록 이름이 지정되었음을 유의해주세요). 이는 protobuf-ts에서 사용하는 JavaScript용 기본 gRPC 클라이언트 라이브러리 (grpc/grpc-node)가 NodeJS에서 실행되어야하기 때문입니다.


<div class="content-ad"></div>

HTTP/2를 지원하는 브라우저가 아직 제한적이에요. 그 말은 우리의 API가 초기에 설계한 대로 브라우저에서 접근할 수 없다는 의미예요.

# gRPC와 REST 스타일 결합하기

하지만 만약 몇 가지 변경을 통해 클라이언트가 gRPC API와 대화할 수 있도록 REST 서비스처럼 만들 수 있다면 어떨까요? 실제로 서비스는 여전히 RPC이지만 우리는 HTTP/1.1, URL 및 JSON을 통해 액세스할 수 있게 됩니다. 우리는 우리의 코드를 직접 작성할 필요 없이 이 작업을 손쉽게 수행할 수 있는 Envoy 프록시 서버를 사용할 수 있어 다행이에요!

필요한 모든 변경 사항은 이전 데모인 grpc-rest-app 구현에서 확인할 수 있어요. 먼저, 프록시 서비스가 우리의 gRPC 서비스 메서드를 올바른 URL에 대해 액세스하도록 도와주기 위해 proto 서비스 인터페이스를 업데이트해야 합니다. 이를 위해 Google API HTTP 라이브러리는 우리가 proto에 추가할 수 있는 주석을 제공해요. buf 도구를 사용하여 buf.yaml 파일에 googleapis 종속성을 플러그인으로 추가할 수 있어요.

<div class="content-ad"></div>

```js
// 파일: packages/apiTypes/proto/contacts/v1/contacts.proto

// 연락처 컬렉션을 관리하는 API입니다.
service ContactsService {
  // 모든 사용 가능한 연락처를 나열합니다.
  rpc ListContacts(ListContactsRequest) returns (ListContactsResponse) {
    option (google.api.http) = {
      get: "/v1/contacts"
    };
  }

  // 제공된 정보로 연락처를 업데이트합니다.
  rpc UpdateContact(UpdateContactRequest) returns (UpdateContactResponse) {
    option (google.api.http) = {
      put: "/v1/contacts/{id}"
      body: "contact"
    };
  }

  // 제공된 ID로 연락처를 삭제합니다.
  rpc DeleteContact(DeleteContactRequest) returns (google.protobuf.Empty) {
    option (google.api.http) = {
      delete: "/v1/contacts/{id}"
    };
  }

  // 간결함을 위해 다른 메소드는 생략되었습니다.
}
```

예제에서 볼 수 있듯이, 우리는 각 메소드에 대해 어떤 HTTP 동사와 URL 경로를 사용하여 액세스할 것인지를 나타내기 위해 google.api.http 어노테이션을 사용합니다.

이제 이러한 바인딩을 읽고 지정된 URI를 사용하여 요청을 수락하는 서비스 엔드포인트를 생성할 수 있는 Envoy 역방향 프록시 서버 인스턴스를 구축할 수 있습니다. 패키지/proxy 하위 폴더에 완전히 구성된 Docker Compose 이미지를 포함한 구현이 있습니다. 이를 작동시키려면 다음 세 가지가 필요합니다:

- Envoy. 저는 간편함을 위해 Docker 이미지를 사용했지만, 선호하는 패키지 관리자를 통해 설치하고 제공된 YAML을 한 줄의 변경으로 구성하여 직접 실행할 수도 있습니다 (세부 사항은 파일 주석 참조).
- gRPC-JSON 변환기 플러그인. HTTP 요청을 JSON 페이로드로 가로채고 이를 이진 프로토 형식의 gRPC로 인코딩된 메시지로 변환하기 위해 이 플러그인을 Envoy의 필터로 구성해야 합니다.
- 서비스용 프로토 설명자 파일. apiTypes 패키지 스크립트에서 buf 빌드 명령줄 옵션을 사용하여 사용 가능한 서비스 및 작업을 이해할 수 있도록 변환기 플러그인이 사용할 프로토 설명자 파일을 생성할 수 있습니다.


<div class="content-ad"></div>

API 및 클라이언트 서버를 이전과 같이 시작하는 것 외에도, 이제는 프록시 서버를 실행하기 위해 docker compose up 명령을 사용해야 합니다. 클라이언트 코드도 업데이트되어 백엔드 API로 직접 fetch 요청을 보내는 대신 프록시 서비스로 요청을 보내도록 설정되었습니다.

```js
/** file: packages/client/src/routes/+page.ts */

import { env } from '$env/dynamic/public';
import type { ListContactsResponse } from '@grpc-vs-rest/api-types';
import type { PageLoad } from './$types';

/** Handles loading data for the page. */
export const load = (async ({ url }) => {
  const baseUrl = env.PUBLIC_API_ENDPOINT || 'http://localhost:8080';
  const pageNumber = Number(url.searchParams.get('pageNumber')) || 0;
  const orderBy = url.searchParams.get('orderBy');
  const apiUrl = `${baseUrl}/v1/contacts?pageSize=25&pageNumber=${pageNumber}&orderBy=${orderBy}`;

  const res = (await (await fetch(apiUrl)).json()) as ListContactsResponse;

  return res;
}) satisfies PageLoad;
```

새로운 접근법의 유연성은 네이티브 gRPC를 사용하여 API와 통신하려는 서비스들이 그대로 사용할 수 있으면서, JSON 및 HTTP/1.1을 통해 통신해야 하는 클라이언트들이 뒤쳐지지 않을 수 있다는 것입니다.

Envoy를 사용하고 있기 때문에 또 하나의 멋진 기능을 사용할 수 있습니다. Envoy는 gRPC-Web도 기본 지원하는데, 이는 브라우저에서 gRPC 통신을 지원하기 위해 설계된 JavaScript 클라이언트입니다! 즉, gRPC 메시지를 HTTP/1.1을 통해 base64로 인코딩된 문자열이나 이진 protobuf로 보낼 수 있다는 것을 의미합니다. 메시지는 프록시를 통해 백엔드 서비스로 전달되며, 이를 통해 더 작고 효율적인 와이어 통신이 이루어지므로 성능이 향상될 것으로 기대됩니다.

<div class="content-ad"></div>

# 결론

이 포스트에서는 많은 내용을 다뤘습니다. 깃허브 레포를 좀 더 살펴보고 모든 것이 어떻게 조화롭게 어울리는지 확인해 보시기를 권장합니다.

gRPC 생태계는 여전히 진화 중이며, 새로운 도구와 기능이 계속해서 추가되어 개발자들에게 편의를 제공하고 있습니다. 최근 Buf가 gRPC 앱을 지원하는 새 라이브러리를 도입했고, 이 라이브러리에는 브라우저 지원을 포함한 gRPC, gRPC-Web, 그리고 RPC 통신을 위한 자체 Connect 프로토콜이 포함되어 있습니다. 다음 API 개발을 위해 gRPC를 탐색하고 있다면 한 번 살펴보시기를 권장합니다.

훌륭한 API를 디자인하는 것은 어렵습니다. 이 글이 여러분이 잘 확장되고 사용 사례를 지원하는 API를 구축하는데 도움이 되었기를 희망합니다.

<div class="content-ad"></div>

# 더 읽을거리

- Martin Nally에 의한 API 설계 시 gRPC vs REST: gRPC, OpenAPI 및 REST를 이해하고 사용할 때
- Roy Fielding에 의한 Architectural Styles and the Design of Network-based Software Architectures
- Aria Azadi Pour에 의한 Node.js 및 Typescript와 함께 gRPC 사용하기