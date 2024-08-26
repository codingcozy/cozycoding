---
title: "초보자를 위한 NestJS로 구현하는 셰넌겐 여행 일정"
description: ""
coverImage: "/assets/img/2024-08-26-ASchengenItineraryforaNoviceNestJSBeginner_0.png"
date: 2024-08-26 17:35
ogImage: 
  url: /assets/img/2024-08-26-ASchengenItineraryforaNoviceNestJSBeginner_0.png
tag: Tech
originalTitle: "A Schengen Itinerary for a Novice NestJS Beginner"
link: "https://medium.com/@muhammedaslamc/a-schengen-itinerary-for-nestjs-e22351a38b29"
isUpdated: false
---


## NestJS 용어와 내부 작동 방식을 평이한 영어로 설명합니다.

![이미지](/assets/img/2024-08-26-ASchengenItineraryforaNoviceNestJSBeginner_0.png)

과도한 이야기는 스킵하고, 만일 당신이 구석구석을 파헤치는 것을 좋아하는 사람이라면 공식 문서를 참조하세요.

NestJS는 Progressive NodeJS 프레임워크입니다. 그러나 다른 프로그래밍 언어나 다른 NodeJS 프레임워크를 사용해 온 경우, 그 문서를 살펴보면 겁이 나곤 합니다. 타입스크립트를 강조하며 해당 기능을 많이 활용합니다. 모듈성을 강조하는 NestJS는 단어를 나누어 사용할 수 있습니다.
인터셉터, 가드, 미들웨어, 파이프입니다.

<div class="content-ad"></div>

ℹ️ 이 튜토리얼을 실습하기 위해서는 Nest CLI를 설치하는 것이 좋습니다.

```js
$ npm install -g @nestjs/cli
```

앱
Nest CLI를 사용하여 앱을 만드는 방법입니다. [더 많은 기능이 필요한 경우 옵션 목록을 여기서 확인하세요]

```js
$ nest new <name> [options]  /*<name> : 앱 이름 */
        OR
$ nest n <name> [options]
```

<div class="content-ad"></div>

요기까지, 새로운 Nest 앱이 생겼어요. 좋아하는 코드 편집기로 코드를 열어보세요. Nest는 관련 코드를 더 잘 관리하고 분리하기 위해 모든 것을 모듈로 간주합니다. 기본적으로 앱이라는 모듈이 있고, 이 모듈은 우리 애플리케이션의 다른 모든 모듈을 부트스트랩합니다.

NestJS는 REST API(제한 없음)가 요청되었을 때 다음과 같이 실행됩니다. (앱 모듈 기준으로, 요구 사항에 따라 더 나눌 수 있습니다)

![이미지](/assets/img/2024-08-26-ASchengenItineraryforaNoviceNestJSBeginner_1.png)

컨트롤러를 생성하세요

<div class="content-ad"></div>

```js
$ nest g controller <컨트롤러_이름>
```

App 모듈을 위한 기본 컨트롤러이며, @Get 데코레이터가 우리의 라우트를 자동으로 등록합니다. '/' 경로와 HTTP Get 메서드를 사용합니다. 이를 확인하려면 다음을 실행하여 응용 프로그램을 시작하세요.

```js
$ npm run start:dev
```

<img src="/assets/img/2024-08-26-ASchengenItineraryforaNoviceNestJSBeginner_2.png" />

<div class="content-ad"></div>

GET / HTTP 요청에 대해 `hello world`를 반환할 것입니다.

모듈 시스템
모듈 전체에서 의존성을 해결하기 위해 각 모듈의 #.module.ts 파일에 모두 지정하려고 합니다.
# = 저희 모듈, 현재는 앱이에요

```js
// 'Owners'라는 모듈에 대한 예제 모듈 선언
// 혼란을 피하기 위해 가능한 간단하게 유지했어요

import { Module } from '@nestjs/common';
import { OwnersService } from './owners.service';

@Module({       // 다음 항목은 모듈로 취급되어야 함을 알려줌
  providers: [ OwnersService],   // 모듈 내에서 서비스를 사용할 거에요
})
export class OwnersModule {}
```

그런 다음 이 모듈을 AppModule에 가져와서 코드를 우리 애플리케이션으로 통합시킬 거에요.

<div class="content-ad"></div>


<img src="/assets/img/2024-08-26-ASchengenItineraryforaNoviceNestJSBeginner_3.png" />

데코레이터
Nest는 TypeScript를 많이 사용하기 때문에 데코레이터를 모든 곳에서 볼 수 있습니다. 데코레이터는 클래스/함수 선언 전에 배치됩니다. 이렇게 하면 코드 기능이 변경됩니다. 다음과 같이 `@Injectable` 데코레이터를 사용하면 OwnerService 클래스가 주입 가능한 서비스임을 나타냅니다.

<img src="/assets/img/2024-08-26-ASchengenItineraryforaNoviceNestJSBeginner_4.png" />

핸들러 / 컨트롤러
API 경로가 호출될 때 실행되는 코드로, 일반적으로 관련 리소스를 그룹화하기 위해 컨트롤러에이 부분을 작성합니다. 전자 상거래 애플리케이션에서 제품을 생성하고 업데이트하는 것은 관리 용이성을 위해 동일한 위치에 있어야 합니다. 컨트롤러 내부에 비즈니스 로직을 작성하지 마세요. 요청을 받고 처리하고(유효성 검사 등) 서비스에 전달하세요. 나머지 기능은 서비스에서 수행될 것입니다.


<div class="content-ad"></div>

## 서비스
NestJS 서비스는 비즈니스 로직을 처리하는 NestJS 애플리케이션의 필수 구성 요소입니다. 일반적으로 @Injectable() 데코레이터로 처리되며 응용 프로그램/모듈 전체에서 의존성 주입에 사용됩니다.

## 엔터티
엔터티는 데이터베이스 테이블을 나타내는 클래스로, 클래스의 각 인스턴스는 테이블의 한 행에 해당하며 해당 테이블의 열과 매핑됩니다. 일반적으로 TypeORM 또는 다른 ORM 라이브러리와 함께 사용됩니다. @Entity 데코레이터는 User 클래스를 엔터티로 표시합니다. 이는 데이터베이스에서 사용자 테이블을 조작합니다. 여기서는 권장하는 TypeOrm ORM을 사용할 것입니다. 주요 관계형 데이터베이스와 호환됩니다 (Postgres, MySQL 등).

![이미지](/assets/img/2024-08-26-ASchengenItineraryforaNoviceNestJSBeginner_5.png)

## 레포지토리 (레포지토리 패턴 사용 시)
레포지토리는 서비스를 통해 응용 프로그램에 데이터를 제공합니다. 이러한 방식으로 서비스를 더 읽기 쉽고 가볍게 만듭니다.

<div class="content-ad"></div>

<img src="/assets/img/2024-08-26-ASchengenItineraryforaNoviceNestJSBeginner_6.png" />

미들웨어
미들웨어는 라우트 핸들러가 호출되기 전에 실행되는 함수입니다. ExpressJS와 같은 프레임워크에서 오신 경우 더 쉽게 사용할 수 있습니다. 기본적으로 라우트 핸들러가 호출되기 전에 개입하며 (API 라우트를 호출할 때 일어나는 코드), 요청과 응답 객체에 모두 액세스할 수 있습니다. 예상치 못한 일이 발생할 때 작업을 중지할 수 있습니다. 모든 조건이 충족되기 전에 핸들러에 도달하기 전에 여러 미들웨어를 연결할 수 있습니다. 다음 명령어를 사용하여 미들웨어를 생성하세요.

```js
$ nest g mi users/isAuthenticated
```

미들웨어 코드는 다음과 같이 보입니다.

<div class="content-ad"></div>

```js
// `/user` 경로에 생성된 미들웨어를 적용합니다.
export class AppModule {
  configure(consumer: MiddlewareConsumer) {
    consumer
      .apply(IsAuthenticatedMiddleware)
      .forRoutes({ path: '/user', method: RequestMethod.GET });
  }
}
```

경로에 미들웨어 적용하기

가드
가드는 미들웨어의 뒤에 오는 것입니다. 여기서 우리는 사후 조치를 취할 수 있습니다. 미들웨어에서는 사용자가 인증되었는지 여부를 확인할 것입니다. 여기서 가드에서는 요청을 ACL에 대해 확인합니다. 인증된 사용자가 작업을 수행할 권한이 충분한지 여부를 확인할 것입니다.


<div class="content-ad"></div>

```javascript
import { Injectable, CanActivate, ExecutionContext } from '@nestjs/common';
import { Observable } from 'rxjs';

@Injectable()
export class AuthGuard implements CanActivate {
  canActivate(
    context: ExecutionContext,
  ): boolean | Promise<boolean> | Observable<boolean> {
    const request = context.switchToHttp().getRequest();
    return validateRequest(request);
  }
}
```

파이프
1. 변환: 입력 데이터를 원하는 형식으로 변환합니다(예: 문자열에서 정수로 변환)
2. 유효성 검사: 입력 데이터를 평가하고 유효한 경우에는 그대로 전달하며, 그렇지 않은 경우 예외를 발생시킵니다.
ValidationPipe, ParseIntPipe, ParseFloatPipe, ParseBoolPipe, ParseArrayPipe, ParseUUIDPipe, ParseEnumPipe, DefaultValuePipe, ParseFilePipe와 같은 내장 파이프들이 있지만 우리만의 파이프를 정의할 수도 있습니다.

![이미지](/assets/img/2024-08-26-ASchengenItineraryforaNoviceNestJSBeginner_8.png)

Nest는 데이터 객체에 대한 유효성 검사를 수행하기 위해 파이프를 사용합니다. 이는 다가오는 유효성 검사 섹션에서 다룰 예정입니다.


<div class="content-ad"></div>

검증
현대 애플리케이션에서 검증은 핵심 역할을 합니다. 제출된 데이터가 순수하고 오류가 존재하지 않다는 보장이 없습니다. 예를 들어 특정 필드에 이메일 주소가 예상되는데 일반 문자열이 입력되는 경우가 있습니다. 이는 처리 API나 제3자 API 내에서 혼돈을 일으킬 수 있습니다. 따라서 검증이 필요하며 불행한 일을 방지하기 위해 필수적입니다.
Nest는 DTO(자세히 다룰 예정)를 사용하여 검증을 수행합니다. 클래스 변환기와 클래스 유효성 검사 라이브러리를 사용합니다. 따라서 Nest 앱 상단에 이를 먼저 설치해야 합니다.

```js
$ npm i --save class-validator class-transformer
```

App 모듈에서 검증을 전역으로 활성화하려면 기본 코드를 다음과 같이 편집하여 main.ts 파일을 수정하세요. 전역으로 설정하기 때문에 해당 파일을 수정해야 합니다.

![이미지](/assets/img/2024-08-26-ASchengenItineraryforaNoviceNestJSBeginner_9.png)

<div class="content-ad"></div>

지금 Nest는 들어오는 모든 요청을 내부적으로 조사할 수 있어요. 그런데 각 API 경로에 대한 규칙을 어떻게 알 수 있을까요? 여기에서 DTO가 필요합니다. 우리는 각 API에 대한 규칙을 DTO 파일에 작성해요. 대부분의 유효성 검사 규칙은 class-validator로 처리돼요. 참고용으로 해당 규칙 목록을 확인해주세요.

users/dto 폴더 안에 createuser.dto.ts 파일을 생성해주세요.

```ts
import { IsEmail, IsString } from 'class-validator';

class BaseContent {
  @IsEmail()
  email: string;

  @IsString()
  password: string;
}
```

이후에 이 dto를 컨트롤러 본문으로 전달하면, 남은 작업은 Nest에게 맡겨도 돼요. Nest가 요청 본문을 탐색하고 필요한 작업을 처리할 거예요.

<div class="content-ad"></div>

```js
// 컨트롤러 핸들러 함수에서 사용자 DTO를 생성합니다. DTO를 가져와 사용하세요.

@Post()
createUser(@Body() createUserDto: CreateUserDto): string {

  // 핸들러 코드
}
```

데이터베이스 액세스
Nest는 SQL 및 NoSQL 데이터베이스를 다양한 방법으로 지원합니다. 우리는 단지 typeorm(전문가를 위한 타입스크립트 중심 ORM👌)을 사용하여 postgres 데이터베이스에 연결하는 방법을 보여주고 있습니다. 앱 모듈에 이 섹션을 추가하면 postgres 데이터베이스에 연결됩니다. 멋져요!

```js
// 데이터베이스에 연결하기 전에 해당 postgres 라이브러리를 설치하려면 다음을 실행하세요.
$ npm i pg
```

```js
@Module({
  imports:[TypeOrmModule.forRoot({
    type: 'postgres',
    host: 'localhost', // 데이터베이스 호스트
    port: 5432, // 데이터베이스 포트
    username: 'postgres', // 데이터베이스 사용자명
    password: '비밀번호', // 데이터베이스 암호
    database: 'app_db', // 데이터베이스 이름
    entities: ['dist/**/*.entity{.ts,.js}'], // 모델/엔티티가 저장된 위치
    logging: true, // 콘솔에 DB 쿼리 로깅

    synchronize: true,  // 모델 변경 사항이 자동으로 데이터베이스 테이블에 기록됨
      // 실제 운영 환경에서는 이 옵션을 사용하지 마십시오.
  }]),
```

<div class="content-ad"></div>

Cors
Cors는 응답을 받을 출처를 결정합니다. 우리는 앱에게 https://abc.com에서 오는 요청에만 응답하도록 요청할 수 있습니다. https://xyz.com에서 오는 요청에는 응답하지 않습니다. 단순히 main.ts에 다음을 추가하면 어디서든 CORS가 활성화됩니다.

![image](/assets/img/2024-08-26-ASchengenItineraryforaNoviceNestJSBeginner_10.png)

특정 도메인에 대해 제한하거나 더 많은 제어를 원한다면 다음을 추가하세요.

```js
app.enableCors({
  origin: ['http://localhost:4200'],  // 이 주소 요청에만 응답
  methods: ['GET', 'POST'], 
  credentials: true,
 });
```

<div class="content-ad"></div>

API의 무담 장치로서 증폭 공격을 방어하기 위해 요청 속도 제한 메커니즘을 적용할 수 있습니다. 동일 출처에서 앱으로의 플러딩 요청이 넘치는 경우 특정 시간 동안 요청을 제한합니다. 먼저 쓸어나가 라이브러리를 설치하고 main.ts 파일을 조정하세요.

```js
$ npm i --save @nestjs/throttler
```

main.ts 파일에 속도 제한기 추가

```js
@Module({
  imports: [
    ThrottlerModule.forRoot([{
      ttl: 60000,   // 윈도우 내에서 유지할 시간
      limit: 10,   // 이 수의 제한
    }]),
  ],
})
```

<div class="content-ad"></div>

## 마무리하겠습니다!

Nest는 단일 응용 프로그램(monolith applications)에만 사용되는 것이 아니라, 마이크로서비스에 대한 내장 지원을 갖추고 있습니다. 그러므로 어떤 방식을 선택할지는 개발자의 선택과 응용 프로그램 요구 사항에 달려 있습니다.

## 기타 긴급 링크

- Nest에서 헬멧을 쓰세요
- 구성 저장
- Nest의 CSRF 보호
- 마이크로서비스 시대의 Nest