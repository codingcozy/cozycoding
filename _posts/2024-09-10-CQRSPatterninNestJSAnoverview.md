---
title: "NestJS에서의 CQRS 패턴 개념 및 활용법"
description: ""
coverImage: "/assets/img/2024-09-10-CQRSPatterninNestJSAnoverview_0.png"
date: 2024-09-10 18:42
ogImage: 
  url: /assets/img/2024-09-10-CQRSPatterninNestJSAnoverview_0.png
tag: Tech
originalTitle: "CQRS Pattern in NestJS An overview"
link: "https://medium.com/javascript-in-plain-english/cqrs-pattern-in-nestjs-an-overview-c3bc09a4192c"
isUpdated: false
---


소프트웨어 개발에서 올바른 디자인 패턴을 구현하면 확장성, 유지 보수성 및 성능을 크게 향상시킬 수 있습니다. Command Query Responsibility Segregation (CQRS) 패턴이 그 중 하나입니다. 이 글에서는 NestJS 애플리케이션에서 CQRS 패턴을 어떻게 구현하는지 탐구해볼 것입니다. 하지만 먼저 CQRS가 무엇이며 어째서 다가오는 기능에 적합한 아키텍처 선택일 수 있는지 살펴보겠습니다.

![이미지](/assets/img/2024-09-10-CQRSPatterninNestJSAnoverview_0.png)

# CQRS 소개

## CQRS란 무엇인가요?

<div class="content-ad"></div>

Command Query Responsibility Segregation (CQRS)은 소프트웨어 시스템에서 명령(command)과 조회(query) 간의 책임 분리를 권장하는 디자인 패턴입니다.

- 명령(Command): 시스템의 상태를 수정하는 작업들을 의미합니다. 예를 들어, 사용자 생성, 업데이트 또는 삭제와 같은 작업은 모두 명령에 속합니다. 왜냐하면 이러한 작업들은 애플리케이션의 데이터를 변경하기 때문입니다.
- 조회(Query): 상태를 변경하지 않고 데이터를 검색하는 작업을 의미합니다. 예를 들어, 사용자 세부 정보를 가져오거나 제품을 나열하는 것은 조회에 속합니다. 이러한 작업들은 데이터베이스에서만 읽기 때문에 상태를 변경하지 않습니다.

전통적인 아키텍처에서는 명령(쓰기 작업)과 조회(읽기 작업) 양쪽에 같은 모델이 종종 사용됩니다. 이는 성능 및 확장성을 최적화하는 데 어려움을 줄 수 있는 복잡한 단일 모델로 이어질 수 있습니다. CQRS는 시스템을 두 가지 구별된 부분, 명령 서비스와 조회 서비스로 나눔으로써 이를 개선합니다:

- 명령 서비스: 일관성과 비즈니스 규칙에 최적화되어 있습니다. 일반적으로 쓰기 최적화된 데이터베이스 모델을 사용하여 명령을 처리할 때 시스템의 상태가 일관되게 유지되도록 보장합니다.
- 조회 서비스: 이 서비스는 빠르고 효율적인 데이터 검색에 최적화되어 있습니다. 일반적으로 다른 데이터베이스 스키마나 캐시 계층을 도입하여 읽기 최적화된 모델을 사용합니다. 이를 통해 조회가 가능한 빠르게 처리되도록 보장합니다.

<div class="content-ad"></div>

# CQRS를 선택해야 하는 경우

CQRS는 다음과 같은 특성을 가진 시스템에 적합합니다:

- 고 읽기/쓰기 불균형: 시스템이 소셜 네트워크나 콘텐츠 중심 애플리케이션과 같이 읽기 대 쓰기 비율이 극명하게 다른 경우, CQRS를 사용하면 해당 불균형을 더 효율적으로 처리할 수 있습니다.
- 복잡한 도메인 로직: 응용 프로그램에 복잡한 비즈니스 규칙이나 데이터 작성 시에만 해당되는 복잡한 워크플로우가 있는 경우, CQRS를 통해 해당 복잡성을 격리하고 관리할 수 있습니다.
- 확장 요구: 서로 다른 구성 요소가 독립적으로 확장해야 하는 대규모 분산 시스템의 경우, CQRS는 강력한 솔루션이 될 수 있습니다.

하지만 CQRS는 그 대가를 치뤄야 합니다. 두 개의 별도 모델을 유지할 때 발생하는 복잡성과 추가 비용이 동반될 수 있습니다. 따라서 이 수준의 분리가 실제로 프로젝트에서 필요한지 평가하는 것이 중요합니다.

<div class="content-ad"></div>

# NestJS에서 CQRS 시작하기

이제 CQRS에 대한 개요를 알았으니, NestJS 애플리케이션에서 이를 어떻게 구현하는지 살펴보겠습니다. NestJS는 확장 가능한 서버 측 애플리케이션을 구축하기 위한 혁신적인 Node.js 프레임워크입니다. NestJS를 사용하면 @nestjs/cqrs 패키지를 통해 CQRS 패턴을 구현하는 것이 간단해집니다.

## 단계 1: 필요한 패키지 설치

먼저, @nestjs/cqrs 패키지를 설치해야 합니다:

<div class="content-ad"></div>

```js
npm install @nestjs/cqrs
```

## Step 2: 애플리케이션 구조 설정

CQRS 기반 애플리케이션에서는 명령(command)과 쿼리(query)를 별도의 디렉토리로 구성해야 합니다. 아래는 예시 디렉토리 구조입니다:

```js
src/
│
├── app/
│   ├── commands/
│   ├── queries/
│   ├── handlers/
│   ├── services/
│   ├── schemas/
│   └── controllers/
```

<div class="content-ad"></div>

- 명령어: 응용 프로그램 상태를 수정하는 작업을 정의합니다.
- 쿼리: 데이터 검색 작업을 처리합니다.
- 핸들러: 명령어와 쿼리를 처리하는 역할을 합니다.
- 서비스: 핵심 비즈니스 로직을 구현합니다.
- 스키마: 데이터베이스 모델과 스키마를 정의합니다.

## 단계 3: 간단한 CQRS 예제 구현하기

- 명령어 생성: 먼저 사용자를 생성하는 간단한 명령어를 만들어 봅시다.

```js
// src/app/commands/create-user.command.ts

export class CreateUserCommand {
  constructor(
    public readonly name: string,
    public readonly email: string,
  ) {}
}
```

<div class="content-ad"></div>

2. Command Handler 생성: 이제이 명령을 처리하는 핸들러를 만들어야합니다.

```js
// src/app/handlers/create-user.handler.ts

import { CommandHandler, ICommandHandler } from '@nestjs/cqrs';
import { CreateUserCommand } from '../commands/create-user.command';
import { UserService } from '../services/user.service';

@CommandHandler(CreateUserCommand)
export class CreateUserHandler implements ICommandHandler<CreateUserCommand> {
  constructor(private readonly userService: UserService) {}

  async execute(command: CreateUserCommand) {
    const { name, email } = command;
    return this.userService.createUser(name, email);
  }
}
```

3. Query 생성: 다음으로, 이메일로 사용자를 가져오는 쿼리를 만들어 보겠습니다.

```js
// src/app/queries/get-user.query.ts

export class GetUserQuery {
  constructor(public readonly email: string) {}
}
```

<div class="content-ad"></div>

4. 쿼리 핸들러 생성하기: 명령어와 마찬가지로 쿼리도 핸들러가 필요합니다. 실제 데이터 가져오기를 수행하는 핸들러를 만들어보겠습니다:

```js
// src/app/handlers/get-user.handler.ts

import { IQueryHandler, QueryHandler } from '@nestjs/cqrs';
import { GetUserQuery } from '../queries/get-user.query';
import { UserService } from '../services/user.service';

@QueryHandler(GetUserQuery)
export class GetUserHandler implements IQueryHandler<GetUserQuery> {
  constructor(private readonly userService: UserService) {}

  async execute(query: GetUserQuery) {
    return this.userService.findByEmail(query.email);
  }
}
```

이 글에서는 서비스 및 해당 데이터베이스 모델을 만드는 데 중점을 두지 않습니다.

## 단계 4: 사용자 서비스 생성하기

<div class="content-ad"></div>

서비스 레이어에는 비즈니스 로직이 포함되어 있고 데이터베이스와 상호 작용합니다.

```js
// src/app/services/user.service.ts

import { Injectable } from '@nestjs/common';
import { InjectModel } from '@nestjs/mongoose';
import { Model } from 'mongoose';
import { User } from '../schemas/user.schema';

@Injectable()
export class UserService {
  constructor(@InjectModel(User.name) private userModel: Model<User>) {}

  async createUser(name: string, email: string): Promise<User> {
    const user = new this.userModel({ name, email });
    return user.save();
  }

  async findByEmail(email: string): Promise<User | null> {
    return this.userModel.findOne({ email }).exec();
  }
}
```

- createUser: 이름과 이메일을 받아 새 사용자 레코드를 생성하고 데이터베이스에 저장합니다.
- findByEmail: 제공된 이메일에 기반하여 사용자를 검색합니다.

## 단계 5: 사용자 스키마 생성하기

<div class="content-ad"></div>

스키마는 데이터베이스에서 사용자 데이터가 구조화될 방법을 정의합니다.

```js
// src/app/schemas/user.schema.ts

import { Prop, Schema, SchemaFactory } from '@nestjs/mongoose';
import { Document } from 'mongoose';

@Schema()
export class User extends Document {
  @Prop({ required: true })
  name: string;

  @Prop({ required: true, unique: true })
  email: string;
}

export const UserSchema = SchemaFactory.createForClass(User);
```

이 스키마는 이름과 이메일 두 가지 속성을 가진 사용자 문서를 정의합니다.

## 단계 6: 모듈에서 핸들러, 서비스 및 스키마 등록하기

<div class="content-ad"></div>

모든 것을 연결하려면 NestJS 모듈에서 핸들러, 서비스 및 스키마를 등록해야 합니다.

```js
// src/app/app.module.ts

import { Module } from '@nestjs/common';
import { MongooseModule } from '@nestjs/mongoose';
import { CqrsModule } from '@nestjs/cqrs';
import { CreateUserHandler } from './handlers/create-user.handler';
import { GetUserHandler } from './handlers/get-user.handler';
import { UserService } from './services/user.service';
import { User, UserSchema } from './schemas/user.schema';

@Module({
  imports: [
    CqrsModule,
    MongooseModule.forRoot('mongodb://localhost/nest'),
    MongooseModule.forFeature([{ name: User.name, schema: UserSchema }])
  ],
  providers: [
    UserService,
    CreateUserHandler,
    GetUserHandler,
  ],
})
export class AppModule {}
```

여기서:
- CQRS를 처리하기 위해 CqrsModule을 가져옵니다.
- MongooseModule.forRoot()를 사용하여 MongoDB를 설정합니다.
- MongooseModule.forFeature()를 사용하여 UserSchema를 등록합니다.

<div class="content-ad"></div>


## 단계 7: 컨트롤러 생성

컨트롤러는 CommandBus와 QueryBus를 사용하여 명령(command)과 쿼리(query)를 보냅니다.

```js
// src/app/controllers/user.controller.ts

import { Controller, Post, Body, Get, Query } from '@nestjs/common';
import { CommandBus, QueryBus } from '@nestjs/cqrs';
import { CreateUserCommand } from '../commands/create-user.command';
import { GetUserQuery } from '../queries/get-user.query';

@Controller('user')
export class UserController {
  constructor(
    private readonly commandBus: CommandBus,
    private readonly queryBus: QueryBus,
  ) {}

  @Post()
  async createUser(@Body('name') name: string, @Body('email') email: string) {
    const command = new CreateUserCommand(name, email);
    return this.commandBus.execute(command);
  }

  @Get()
  async getUser(@Query('email') email: string) {
    const query = new GetUserQuery(email);
    return this.queryBus.execute(query);
  }
}
```

UserController는 CommandBus와 QueryBus를 통해 명령과 쿼리를 보내고, 해당 핸들러와 상호 작용합니다.

<div class="content-ad"></div>

## 단계 8: 애플리케이션 실행하기

모든 준비가 끝났으니 다음 명령어를 사용하여 애플리케이션을 실행할 수 있습니다:

```js
npm run start
```

이 설정을 통해 NestJS에서 사용자 데이터의 쓰기 및 읽기를 처리하는 완전히 작동하는 CQRS 시스템을 마칠 수 있게 되었습니다. 필요한 데이터베이스 스키마와 서비스 로직도 마련되어 있습니다.

<div class="content-ad"></div>

## 결론

CQRS는 읽기 및 쓰기 작업을 독립적으로 확장해야 하는 시스템, 비즈니스 로직 분리 및 성능 향상이 필요한 시스템에 적합한 강력한 디자인 패턴입니다. 복잡성을 추가하지만, 올바른 맥락에서 사용될 때 응용 프로그램의 확장성과 유지 관리성을 크게 향상시킬 수 있습니다. 이 안내서는 NestJS에서 CQRS를 기본적으로 구현하는 방법을 제공합니다.

이 글이 마음에 드시면 박수와 댓글을 자유롭게 남겨주세요. 더 많은 콘텐츠를 원하시면 팔로우해주세요.

# 간단히 설명하자면 🚀

<div class="content-ad"></div>

"In Plain English" 커뮤니티의 일원이 되어 주셔서 감사합니다! 떠나시기 전에:

- 작가를 추천하고 팔로우해보세요 ️👏️️
- 팔로우하기: X | LinkedIn | YouTube | Discord | 뉴스레터
- 다른 플랫폼 방문하기: CoFeed | Differ
- 더 많은 콘텐츠: PlainEnglish.io