---
title: "Flutter와 GraphQL 파트 1 - GraphQL 소개"
description: ""
coverImage: "/assets/img/2024-07-09-FlutterwithGraphQLPart1IntroductiontoGraphQL_0.png"
date: 2024-07-09 22:19
ogImage: 
  url: /assets/img/2024-07-09-FlutterwithGraphQLPart1IntroductiontoGraphQL_0.png
tag: Tech
originalTitle: "Flutter with GraphQL : Part 1 Introduction to GraphQL"
link: "https://medium.com/@debasmitasarkar.93/flutter-with-graphql-part-1-introduction-to-graphql-ab3d88ce35fb"
isUpdated: true
---





<img src="/assets/img/2024-07-09-FlutterwithGraphQLPart1IntroductiontoGraphQL_0.png" />

안녕하세요 여러분! 백엔드 응답에서 유사한 상황에 처해본 적이 있나요?
사용자 이름만 요청했는데 사용자의 7대 조상까지 모두 얻었어요! 농담이죠, 하지만 사실이기도 한거에요!

GraphQL의 세계에 오신 것을 환영합니다. 이곳에서는 서버로부터 요청한 것을 정확히 얻을 수 있어요. 이 시리즈는 여러분의 요청에 정확하게 맞춰져서, GraphQL의 놀라운 기능을 소개하고 Flutter 애플리케이션에 GraphQL을 어떻게 원활하게 통합할지 보여줄 것입니다.

## 이 시리즈의 내용 미리보기


<div class="content-ad"></div>

- GraphQL 소개:
기본부터 시작해서 GraphQL이 무엇이며 왜 멋진지를 다룰 거에요. 주요 개념을 배우고 REST와 비교하는 방법, 그리고 프로젝트에 가져다주는 이점에 대해 알게 될 거에요.
- Flutter 애플리케이션 생성 및 GraphQL 통합: Flutter 애플리케이션 설정 및 GraphQL 통합을 안내할 거에요. GraphQL 클라이언트와 코드 생성과 같은 도구에 어떻게 익숙해지는지를 배우게 될 거에요. 또한 이 시리즈를 통해 빌드할 프로젝트에 대해 설명할 거에요.
- 쿼리 및 뮤테이션 작성, 에러 처리, 자동 캐시 업데이트: 이 부분에서는 효율적인 GraphQL 쿼리 및 뮤테이션 작성 방법을 배울 거에요. 에러를 gracefully 처리하는 방법과 자동 캐시 업데이트를 활용해 앱 데이터가 신선하게 유지되도록 하는 방법도 다룰 거에요.
- 고급 GraphQL 기능, 구독 및 낙관적 UI: 마지막으로, 실시간 업데이트를 위한 구독과 낙관적 UI를 구현하는 등 고급 GraphQL 기능에 대해 자세히 다뤄볼 거에요. 이를 통해 앱이 더 빠르게 느껴지도록 사용자 경험을 향상시킬 수 있어요.

## 이번 파트에서 무엇을 배우게 되나요?

1부에서 막바지에 이르러,
1. GraphQL 기초개념, REST에 비해 갖는 장점, 주요 개념에 대한 튼튼한 이해를 통해 코딩 능력을 업그레이드할 수 있어요.

2. 하지만 기다려봐요, 추가로! 단순한 서버를 설정하여 쿼리를 테스트하는 방법을 안내할 거에요.

<div class="content-ad"></div>

## GraphQL이란 무엇인가요?

위 내용은 GraphQL의 공식 정의입니다. 하지만 제가 알고 있는 한에는 여러분이 본 글을 읽으셨을 때 좀 어렵게 느꼈을 거예요. 저희가 편안한 언어로 이야기해보겠습니다.

프론트엔드 개발자로서, 백엔드와 관련된 REST API를 다루면서 이 개념은 매우 기본적이에요.

1. 앱은 서버로부터 데이터를 요청하거나 REST API를 호출하여 데이터를 수정하려 합니다.
2. 이제, 서버는 그 요청에 대한 응답이나 오류를 되돌려줍니다.

<div class="content-ad"></div>



<img src="/assets/img/2024-07-09-FlutterwithGraphQLPart1IntroductiontoGraphQL_1.png" />

자세히 설명하자면, 간단한 도서 컬렉션 애플리케이션을 만들고 싶다고 가정해봅시다. 도서 세부 정보를 가져오기 위한 REST Api 엔드포인트가 필요하고, 도서의 저자만 필요하다면 해당 데이터를 얻기 위해 다른 엔드포인트를 호출해야 합니다.

<img src="/assets/img/2024-07-09-FlutterwithGraphQLPart1IntroductiontoGraphQL_2.png" />

그러나 백엔드가 GraphQL 서버를 사용한다면 무엇이 바뀔까요?



<div class="content-ad"></div>

1. 모든 요청이 GET, POST, PUT 또는 DELETE가 아닌 POST 요청으로 변환됩니다.
2. REST API에서 하던 것처럼 api/book?id=1 또는 api/author?bookid=1과 같이 다른 엔드 포인트를 호출할 필요가 없어요. 대신에 쿼리를 작성하면 됩니다. 걱정하지 마세요. 나중에 설명할게요.
3. 쿼리는 서버에서 필요한 필드를 포함할 거예요. 첫 번째 요청에서는 책의 id, 제목, 저자가 필요했기 때문에 쿼리는 이렇게 생겼어요.

![이미지](/assets/img/2024-07-09-FlutterwithGraphQLPart1IntroductiontoGraphQL_3.png)

4. 두 번째 요청에서는 특정 책의 저자 이름만 필요했습니다. 쿼리가 어떻게 변경되는지 살펴보세요.

![이미지](/assets/img/2024-07-09-FlutterwithGraphQLPart1IntroductiontoGraphQL_4.png)

<div class="content-ad"></div>

이것은 전반적인 아이디어였고, 쿼리, 뮤테이션 및 기타 사항에 대한 더 자세한 내용을 곧 알아볼 것입니다.

## GraphQL의 주요 구성 요소:

GraphQL에 대해 바로 알아야 할 중요한 세 가지가 있습니다. 스키마, 쿼리 및 뮤테이션입니다.

1. 스키마

<div class="content-ad"></div>

일반적으로, 기능을 구현하기 전에 백엔드 개발자와 프론트엔드 개발자는 객체나 응답에 대한 청사진에 동의합니다. 예를 들어, REST에서 endpoint api/book?id=1은 id, 제목, 작가 등 여러 세부 정보를 포함하는 책 객체를 반환했습니다.

GraphQL에서 객체에 대한 청사진은 schema.graphql이라는 파일에 덤프됩니다. 이 스키마에서는 모든 객체와 해당 필드, 객체 간의 관계, 허용되는 쿼리 및 뮤테이션의 종류 등을 찾을 수 있습니다. 일반적으로 백엔드 개발자가이 파일을 업데이트합니다.

![그림](/assets/img/2024-07-09-FlutterwithGraphQLPart1IntroductiontoGraphQL_5.png)

2. 쿼리
쿼리는 GraphQL 서버에서 데이터를 요청하는 방법입니다. 정확히 필요한 데이터와 그 형식을 지정합니다.

<div class="content-ad"></div>

REST에서는 데이터를 가져오기 위해 GET 요청을 사용합니다. 예를 들어, GET api/book?id=1은 책의 세부 정보를 가져옵니다.

GraphQL에서는 Query가 GET 요처럼 동작합니다. 서로 다른 데이터 세트를 가져오기 위해 서로 다른 엔드포인트를 호출하는 대신, 해당 데이터를 가져오기 위해 서로 다른 쿼리를 작성합니다.

![이미지](/assets/img/2024-07-09-FlutterwithGraphQLPart1IntroductiontoGraphQL_6.png)

3. Mutations
변형(Mutations)은 서버에서 데이터를 수정하는 방법입니다. 쿼리와 유사하지만 데이터를 생성, 업데이트 또는 삭제하는 데 사용됩니다.

<div class="content-ad"></div>

REST에서는 데이터를 수정하기 위해 POST, PUT 및 DELETE 요청을 사용합니다. 예를 들어, 새 책을 만들려면 POST api/book을 사용하고, 책을 업데이트하려면 PUT api/book?id=1을 사용하고, 책을 삭제하려면 DELETE api/book?id=1을 사용합니다.

GraphQL에서는 위 작업을 모두 다른 mutation을 작성하여 수행할 수 있습니다.

![이미지](/assets/img/2024-07-09-FlutterwithGraphQLPart1IntroductiontoGraphQL_7.png)

이제 schema, query 및 mutation이 어떻게 함께 작동하는지 이해해 보겠습니다.

<div class="content-ad"></div>

다시 책 예제를 살펴보겠습니다. 스키마에서 Book과 Author라는 객체 유형이 있습니다. 이들은 프로젝트에서 다루게 될 데이터 모델이며, 이들 구조는 스키마에서 정의되어 있습니다. 추가로, Query와 Mutation 유형이 있어 이 객체를 가져오고 수정하는 방법을 정의합니다.

```js
type Book { 
  id: ID! 
  title: String! 
  author: Author! 
} 
```

```js
type Author { 
  id: ID! 
  name: String! 
}
```

이제 여러분은 유형이 무엇인지 알았으니, Query를 통해 객체를 가져오는 시간입니다.
스키마에서의 쿼리는 이러한 객체를 가져올 수 있도록 해줍니다.

<div class="content-ad"></div>

쿼리의 스키마 정의:

- 이것은 GraphQL 스키마 정의의 일부입니다.
- 이는 GraphQL 서버가 수락할 수 있는 쿼리의 구조와 유형을 정의합니다.
- 이는 서버에게 클라이언트가 만들 수 있는 쿼리의 종류와 해당 쿼리가 반환할 데이터 유형을 알려줍니다.

```js
type Query { 
  book(id: ID!): Book 
  books: [Book] 
  author(id: ID!): Author 
}
```

- book 쿼리를 사용하면 ID에 따라 단일 도서를 가져올 수 있습니다.
- books 쿼리를 사용하면 도서 목록을 가져올 수 있습니다.
- author 쿼리를 사용하면 ID에 따라 단일 저자를 가져올 수 있습니다.

<div class="content-ad"></div>

쿼리 실행:

- 이것은 클라이언트(프론트엔드 개발자)가 작성한 실제 쿼리입니다.
- 이는 스키마에서 정의된 구조를 사용하여 서버에서 가져올 데이터를 지정합니다.
- id: 1과 같은 특정 매개변수와 요청한 데이터의 필드를 포함합니다.

```js
query {
 book(id: 1) {
   id
   title
   author {
     name
   }
 }
}
```

- 이 쿼리는 데이터를 가져오기 위해 서버에 보내집니다. 서버는 스키마를 사용하여 이 쿼리를 검증하고 실행합니다.
- 서버는 이 쿼리를 읽고 스키마에 따라 처리하여 요청된 데이터를 지정된 형식으로 반환합니다. 아래는 이 쿼리의 응답일 수 있습니다.

<div class="content-ad"></div>

```json
{
  "data": { 
      "book": { 
        "id": "1", 
        "title": "GraphQL for Beginners", 
        "author": { 
          "name": "John Doe"
         }  
       }
    }   
}
```

이전 예제를 보면 GraphQL을 사용하는 이점이 명백합니다.

변이의 스키마 정의:

```json
type Mutation {
 addBook(title: String!, authorId: ID!): Book
 updateBook(id: ID!, title: String, authorId: ID): Book
 deleteBook(id: ID!): Book
}
```

<div class="content-ad"></div>

- 쿼리와 마찬가지로 변이의 스키마 정의는 GraphQL 서버가 수용할 수 있는 변이의 구조와 유형을 정의합니다.
- 이는 클라이언트가 수행할 수 있는 데이터 수정의 종류를 서버에 전달하고 해당 변이가 반환할 데이터 유형을 알려줍니다.
- addBook 변이는 제목과 작성자 ID로 새 책을 추가할 수 있게 합니다.
- updateBook 변이는 기존 책의 제목과 작성자를 업데이트할 수 있도록 합니다.
- deleteBook 변이를 사용하면 ID를 사용하여 책을 삭제할 수 있습니다.

Mutation 실행:

```js
mutation {
 addBook(title: "New Book", authorId: "1") {
 id
 title
 author {
     name
   } 
  }
}
```

- 이것은 클라이언트(프론트엔드 개발자)가 작성한 실제 변이입니다.
- 이는 서버에서 수정하고자 하는 데이터를 클라이언트가 어떻게 지정하는지를 나타내며 스키마에서 정의된 구조를 사용합니다.
- 이는 특정 매개변수(예: title: "New Book" 및 authorId: "1")와 변이 후 반환되기를 요청한 데이터 필드를 포함합니다.

<div class="content-ad"></div>

## GraphQL 사용의 장점:

- 유연한 쿼리: 정확히 필요한 것만 요청하여 네트워크에서 전송되는 데이터 양을 줄일 수 있습니다.
- 빠른 개발: GraphQL을 사용하면 프론트엔드 개발자가 빠르게 반복하고 백엔드 변경을 기다리지 않고 개발할 수 있습니다.
- 강력한 타입 지정 스키마: GraphQL의 타입 시스템은 쿼리가 정확하며 서버에서 유효성을 검사할 수 있도록 보장합니다. 이로 인해 런타임 오류가 줄어듭니다.
- 더 나은 성능: 응답의 구조를 클라이언트가 지정할 수 있기 때문에 GraphQL은 데이터의 과다 또는 미달된 요청을 줄여 네트워크 사용을 더 효율적으로 합니다.
- 더 쉬운 진화: GraphQL을 사용하면 API에 새로운 필드와 타입을 추가해도 기존 쿼리를 변경할 필요가 없습니다. 이는 기존 클라이언트를 망가뜨리지 않고 API를 발전시키는 것을 간편하게 합니다.

휴우!!! 어려운 부분은 끝났어요! 이제 커피를 사들어 드신 후에 자기 백을 한 번 해 주세요. 여러분은 지금 GraphQL의 수수께끼를 푼 것이니까요!
그리고 만약 그렇게 하고 싶다면, 저에게 커피 한 잔 사주세요!! ☕️

## GraphQL 쿼리 및 뮤테이션을 시도해보세요.

<div class="content-ad"></div>

GraphQL 모험가 여러분! 새로운 기술을 테스트해볼 준비가 되셨나요?
저는 여러분을 위해 GraphQL API를 설정했어요. 이 API를 탐험하는 가장 멋진 방법은 Facebook에서 만든 툴인 GraphiQL (발음은 "그래피컬"입니다)을 사용하는 거에요. 이 툴을 통해 GraphQL 쿼리를 테스트하는 것이 정말 쉬워요.

GraphiQL에 접속하기
시작하려면 GraphiQL Online으로 이동해주세요. 이 도구는 여러분의 GraphQL 엔드포인트에 연결하여 자동으로 스키마를 가져와 쿼리를 작성하고 실행할 수 있는 사용자 친화적 인터페이스를 제공해줘요.

- GraphiQL Online 열기: GraphiQL Online으로 이동하기

2. 엔드포인트 설정: GraphQL 엔드포인트의 URL을 입력해주세요: https://graphql-book-server-debo-a1bbf9f7b9f0.herokuapp.com/graphql

<div class="content-ad"></div>

3. 쿼리 시작하기:

- 왼쪽 창에 쿼리나 뮤테이션을 작성해주세요.
- 실행하려면 “실행” 버튼(▶️ 아이콘)을 클릭하세요.
- 결과는 오른쪽에서 확인할 수 있습니다.

예시 쿼리: 데이터 가져오기!

- ID로 책 가져오기:

<div class="content-ad"></div>

```js
query {
 book(id: 1) {
 id
 title
 author {
     name
   }
  }
}
```

2. 모든 책 가져오기

```js
query {
  books {
    id
    title
    author {
      name
    }
  }
}
```

변이 예시: 이제 데이터를 변경해보겠습니다.

<div class="content-ad"></div>

1. 새 책 추가

```js
mutation {
 addBook(title: “New Book”, authorId: “1”) {
 id
 title
 author {
     name
   }
 }
}
```

2. 책 업데이트

```js
mutation {
  updateBook(id: "1", title: "Updated Book Title", authorId: "2") {
    id
    title
    author {
      name
    }
  }
}
```

<div class="content-ad"></div>

3. 책 삭제

```js
mutation {
  deleteBook(id: "1") {
    id
    title
  }
}
```

모험이 끌리나요? 변수를 사용하여 동적 인수를 시도해보세요.
쿼리를 매개변수화:

```js
query ($limit: Int!) {
  books(limit: $limit) {
    id
    title
  }
}
```

<div class="content-ad"></div>

GraphiQL에서 페이지 하단으로 스크롤하여 쿼리 변수 창을 찾아보세요. 거기에 변수를 JSON 객체로 추가해주세요.

```js
{
 "limit": 5
}
```

# 요약

- GraphQL 기본 지식으로 코딩 스킬을 향상시켰습니다. REST에 비해 GraphQL의 장점을 이해했습니다.
- 스키마를 정의하고 쿼리를 작성하며 뮤테이션을 수행하는 방법을 알고 있습니다.
- GraphQL 사용의 이점을 알게 되었습니다.
- GraphiQL을 사용하여 실제 쿼리와 뮤테이션을 시도해 보았습니다.

<div class="content-ad"></div>


![image1](/assets/img/2024-07-09-FlutterwithGraphQLPart1IntroductiontoGraphQL_8.png)

![image2](/assets/img/2024-07-09-FlutterwithGraphQLPart1IntroductiontoGraphQL_9.png)
