---
title: "자바스크립트 의존성 주입이 망가졌다 wroud di가 고친 방법"
description: ""
coverImage: "/assets/img/2024-08-26-JavaScriptDependencyInjectionisBrokenHeresHowwrouddiisFixingIt_0.png"
date: 2024-08-26 17:33
ogImage: 
  url: /assets/img/2024-08-26-JavaScriptDependencyInjectionisBrokenHeresHowwrouddiisFixingIt_0.png
tag: Tech
originalTitle: "JavaScript Dependency Injection is Broken Heres How wroud di is Fixing It"
link: "https://medium.com/@wroud/javascript-dependency-injection-is-broken-heres-how-wroud-di-is-fixing-it-929d3dbd1bb7"
isUpdated: false
---



![Dependency Injection](/assets/img/2024-08-26-JavaScriptDependencyInjectionisBrokenHeresHowwrouddiisFixingIt_0.png)

의존성 주입(Dependency Injection, DI)은 현대 JavaScript 및 TypeScript 개발에서 중요한 패턴으로, 개발자가 구성 요소를 분리하고 의존성을 더 효율적으로 관리할 수 있게 해줍니다. 그러나 JavaScript에서 인기 있는 DI 라이브러리를 사용해 본 적이 있다면, 오래된 TypeScript 데코레이터부터 혼란스러운 API까지 골치아픈 문제에 직면했을 것입니다. 이러한 라이브러리들은 종종 문제를 해결하기보다는 더 많은 문제를 발생시킵니다.

이 기사에서는 JavaScript 의존성 주입 라이브러리의 일반적인 함정들을 살펴보고(명확하지 않은 문서, 안전하지 않은 관행, 확장성 부족 등), 이러한 도전 과제를 극복하기 위해 고안된 현대적인 해결책인 @wroud/di를 소개하겠습니다. 충분하지 않은 도구와 싸우는 느린 개발자든, 간단하고 확장 가능한 DI 솔루션을 찾는 초심자든, @wroud/di는 개발 프로세스를 간소화하는 데 필요한 명확성과 신뢰성을 제공합니다.

# 인기 있는 JavaScript 및 TypeScript DI 라이브러리의 일반적인 함정


<div class="content-ad"></div>

의존성 주입은 확장 가능하고 유지보수 가능한 애플리케이션을 구축하는 강력한 패턴이지만, 일부 개발자들은 인기 있는 JavaScript 및 TypeScript DI 라이브러리를 구현할 때 심각한 어려움을 겪습니다. 다음에서 가장 일반적인 이슈들을 살펴보겠습니다:

- 구식 방식
많은 기존 DI 라이브러리는 TypeScript의 사용되지 않는 기능이나 이전 버전에 의존합니다. 특히 데코레이터(decorators)와 관련된 기능입니다. TC39 제안인 decorators는 이제 3단계에 이르렀으며, 현대적인 라이브러리에 적합한 옵션이 되었습니다. 하지만 중요한 이슈가 있습니다: 현재 데코레이터의 버전은 이전 구현과 호환되지 않습니다. 이러한 비호환성은 새 라이브러리를 기존의 구식 데코레이터를 사용하는 코드베이스에 통합하려고 할 때 문제를 일으킬 수 있습니다. 또한 3단계 데코레이터는 라이브러리에서 사용할 수 있지만, 전환 기간과 하위 호환성 부재로 인해 유지보수 또는 레거시 코드 리팩토링이 필요한 개발자에게 마찰을 초래할 수 있습니다.
- 모호하고 복잡한 API
인기 있는 DI 라이브러리를 사용하는 개발자들 사이에서 가파른 학습 곡선은 흔히 제기되는 불만입니다. 종종 API는 직관적이지 않으며, 난해한 문서와 예제를 통해 많은 시간을 들여 읽어야 합니다. 이 복잡성은 개발을 늦추며 생산성을 감소시키며 특히 신입 개발자들 사이에서 도입을 방해할 수 있습니다.
- 안전하지 않은 방법과 해결책
이러한 라이브러리의 제한 사항이나 버그를 해결하기 위해 개발자들은 때로 해킹이나 위험한 방식에 의존할 수 있습니다. 이러한 해결책은 코드 품질을 저해하고 감지하기 어려운 버그를 도입하며, 코드베이스를 깨지기 쉽게 만들 수 있습니다. 이러한 방식은 DI가 제공하려는 이점을 약화시키므로, 나중에 더 많은 문제를 야기시킬 수 있습니다.
- 확장 가능성 솔루션의 부재
애플리케이션이 성장함에 따라 의존성 관리가 더욱 복잡해집니다. 많은 DI 라이브러리는 아래와 같은 대규모 애플리케이션에 대한 솔루션을 제공하지 못합니다:
- 서비스 관리: 다수의 서비스와 그 등록을 처리하는 것이 번거로워질 수 있습니다.
- 순환 의존성 탐지: 순환 의존성을 식별하고 해결하는 것은 종종 간단하지 않습니다.
- 코드 분할 지원: 성능 최적화를 위해 지연 로딩과 코드 분할을 용이하게 하는 것은 충분히 다루지 못할 수 있습니다.
이러한 측면에 대한 내장 지원이 없으면, 애플리케이션의 확장이 복잡하고 오류가 발생할 수 있습니다.
- 암호화된 오류 메시지
문제가 발생할 때, 많은 DI 라이브러리가 제공하는 오류 메시지는 모호하거나 정보가 부족할 수 있습니다. 이러한 분명하지 않은 점은 긴 디버깅 세션, 좌절 및 개발시간 낭비로 이어질 수 있습니다. 명확하고 실질적인 오류 메시지는 효율적인 문제 해결과 개발자의 생산성을 유지하는 데 중요합니다.

# @wroud/di 소개: 의존성 주입에 대한 현대적인 접근 방식

이러한 어려움을 인식하여, @wroud/di는 강력하고 최신의 JavaScript 및 TypeScript 개발자를 위한 DI 라이브러리로 등장합니다. @wroud/di가 각각의 언급된 이슈를 해결하는 방법은 다음과 같습니다:

<div class="content-ad"></div>

- 현대적이고 안정적인 구현
@wroud/di는 현대 및 레거시 환경을 모두 고려하여 설계되었습니다. 그 중 하나는 기존 레거시 데코레이터(stage 2) 및 더 새로운 stage 3 데코레이터와 호환성을 갖춘 것입니다. 이러한 유연성은 기존의 레거시 데코레이터를 사용하면서 @wroud/di를 시작하고 코드를 변경하지 않고도 단순히 stage 3 데코레이터로 원활하게 전환할 수 있다는 것을 의미합니다. 이 접근 방식은 애플리케이션을 미래를 준비시켜주는 것뿐만 아니라 최신 표준을 직접적인 위험 없이 자신의 속도로 채택할 수 있도록 해줍니다.

- 명확하고 직관적인 API
@wroud/di는 개발자 경험을 염두에 두고 설계된 API를 제공합니다. 그 간단함으로 경험이 풍부한 개발자들뿐만 아니라 새로운 도입자들도 불필요한 복잡성 없이 DI를 쉽게 구현할 수 있습니다. 명확한 관례와 포괄적인 문서화는 더 빠른 적응 및 효율적인 사용을 도와줍니다.

- 안전하고 신뢰할 수 있는 관행
@wroud/di는 의존성 관리에 대한 최선의 관행을 장려하며, 안전하지 않은 해킹이나 회피책이 필요하지 않습니다. 의존성을 우아하게 처리하는 아키텍처는 코드베이스가 깨끗하고 유지보수 가능하며 숨겨진 버그가 없도록 보장합니다.

- 확장성 중심의 기능
성장하는 애플리케이션을 지원하기 위해 @wroud/di에는 다음과 같은 기능이 포함되어 있습니다:
- 효율적인 서비스 관리: 많은 서비스를 등록하고 관리하는 간소화된 메커니즘.
- 순환 의존성 검출: 순환 의존성을 식별하고 해결하는 데 도움이 되는 내장 도구로 런타임 문제를 방지합니다.
- 코드 분할 지원: 지연 로딩 및 모듈화를 용이하게 하는 기능이며, 애플리케이션의 성능 및 유지보수성을 향상시킵니다.
이러한 기능들은 전통적인 DI와 관련된 일반적인 문제를 겪지 않고 애플리케이션의 확장성을 높이는 명확한 방안을 제공합니다.

- 구체적이고 도움이 되는 오류 메시지
@wroud/di는 개발자와의 명확한 커뮤니케이션을 우선시합니다. 오류가 발생할 때 문제 해결을 안내하는 자세하고 실행 가능한 오류 메시지를 제공하여 문제를 신속히 해결할 수 있습니다. 이 투명성은 디버깅 효율성을 높이고 예상하지 못한 문제로 인한 다운타임을 줄여줍니다.

# 다음 프로젝트에 @wroud/di를 선택하는 이유

적절한 DI 라이브러리를 선택하는 것은 개발 워크플로우, 코드 품질 및 애플리케이션의 확장성에 큰 영향을 미칠 수 있습니다. 다음은 @wroud/di가 돋보이는 이유입니다:

- 성능: 가벼우며 성능을 최적화하여 최소한의 오버헤드를 보장합니다.
- 유연성: 다양한 프로젝트 아키텍처와 호환되며 기존 코드베이스에 쉽게 통합할 수 있습니다.
- 커뮤니티 및 지원: 활발한 개발과 지원을 통해 @wroud/di가 최신 산업 동향과 개발자 요구를 반영하도록 보장합니다.
- 포괄적인 문서화: 상세한 가이드, 예제 및 튜토리얼을 통해 쉽게 시작하고 고급 기능을 마스터할 수 있습니다.

<div class="content-ad"></div>

# @wroud/di in Action: 코드 예시

@wroud/di의 단순함과 강력함을 보여주기 위해, 실용적인 사용 예시를 통해 살펴보겠습니다. 기존 데코레이터나 새로운 stage 3 데코레이터를 사용하더라도, @wroud/di를 통해 의존성 주입을 간편하고 유연하게 처리할 수 있습니다.

## 1. 기본 서비스 등록 및 해결

먼저, 기초적인 내용부터 시작해보겠습니다: 서비스 등록 및 해결.

<div class="content-ad"></div>

```js
import { ServiceContainerBuilder, injectable } from '@wroud/di';

// 간단한 서비스를 정의합니다.
@injectable()
class Logger {
  log(message: string) {
    console.log(`Logger: ${message}`);
  }
}

// 컨테이너에 서비스를 등록합니다.
const builder = new ServiceContainerBuilder();
builder.addSingleton(Logger);

const serviceProvider = builder.build();

// 서비스를 해결합니다.
const logger = serviceProvider.getService(Logger);
logger.log('안녕, 의존성 주입!');
```

이 예제에서는 Logger 서비스를 정의하고, ServiceContainerBuilder를 사용하여 싱글톤으로 등록한 다음 ServiceProvider를 통해 해결합니다. 이 프로세스는 명확하고 직관적이며, 애플리케이션 로직에 집중할 수 있습니다.

## 2. Legacy 및 Stage 3 데코레이터와 함께 작업하기

@wroud/di는 레거시 데코레이터(Stage 2)와 Stage 3 데코레이터를 모두 지원하여 매끄러운 전환 경로를 제공합니다. 아래에서는 코드 변경 없이 두 버전 중 어느 것을 사용할 수 있는지 설명합니다.

<div class="content-ad"></div>

Legacy Decorators (Stage 2):

```js
// TypeScript 구성에서 레거시 데코레이터가 활성화된 것으로 가정

@injectable()
class Database {
  connect() {
    console.log('데이터베이스에 연결되었습니다');
  }
}

builder.addSingleton(Database);
const database = serviceProvider.getService(Database);
database.connect();
```

Stage 3 Decorators로 마이그레이션:

```js
// TypeScript 구성에서 stage 3 데코레이터가 활성화된 것으로 가정

@injectable()
class Database {
  connect() {
    console.log('데이터베이스에 연결되었습니다');
  }
}

// 코드 구조에 대한 변경이 필요하지 않습니다
builder.addSingleton(Database);
const database = serviceProvider.getService(Database);
database.connect();
```

<div class="content-ad"></div>

@wroud/di를 사용하면, 레거시에서 stage 3 데코레이터로 전환하는 경우에도 서비스 정의나 사용 패턴을 변경할 필요가 없습니다. 이 유연성을 통해 코드가 새로운 표준을 채택하면서도 일관되고 유지보수 가능하게 유지됩니다.

## 3. 고급 기능: 지연 로딩 및 코드 분할

@wroud/di는 지연 로딩과 같이 성능을 향상시킬 수 있는 고급 기능도 지원합니다. 이 기능은 필요한 시점에만 서비스를 로딩함으로써 성능을 개선할 수 있습니다.

```js
// IHeavyService.ts
import { createService } from "@wroud/di";

export interface IHeavyService {
  load(): void;
}
export const IHeavyService = createService<IHeavyService>("IHeavyService");
```

<div class="content-ad"></div>

```js
// HeavyService.ts
import { injectable } from "@wroud/di";
import { IHeavyService } from "./IHeavyService.ts";

@injectable()
export class HeavyService implements IHeavyService {
  load() {
    console.log("Heavy service loaded");
  }
}
```

```js
import { ServiceContainerBuilder, lazy } from "@wroud/di";
import { IHeavyService } from "./IHeavyService.ts";

const builder = new ServiceContainerBuilder();
builder.addSingleton(
  IHeavyService,
  lazy(() => import("./HeavyService.ts").then((m) => m.HeavyService)),
);

const serviceProvider = builder.build();

// Lazy load the service
serviceProvider.getServiceAsync(IHeavyService).then((heavyService) => {
  heavyService.load();
});
```

이 예시에서는 HeavyService가 명시적으로 요청될 때만 로드되며, 특히 시작 시간을 최적화하고자 하는 대규모 응용 프로그램에 유용합니다.

## 4. 의존성이 있는 여러 서비스 관리하기


<div class="content-ad"></div>

이제 세 가지 서비스가 서로에게 의존하는 더 복잡한 시나리오를 살펴보겠습니다. 이 예제는 레거시 데코레이터, stage 3 데코레이터 또는 lazy loading을 사용하든지에 관계없이 API가 일관되게 유지됨을 보여줍니다.

```js
import { ServiceContainerBuilder, injectable } from '@wroud/di';

// Logger 서비스 정의
@injectable()
class Logger {
  log(message: string) {
    console.log(`Logger: ${message}`);
  }
}

// Logger에 의존하는 Database 서비스 정의
@injectable(() => [Logger])
class Database {
  constructor(private logger: Logger) {}

  connect() {
    this.logger.log('데이터베이스에 연결됨');
  }
}

// Database에 의존하는 UserService 정의
@injectable(() => [Database])
class UserService {
  constructor(private database: Database) {}

  getUser(id: string) {
    this.database.connect();
    console.log(`아이디가 ${id}인 사용자 가져오는 중`);
  }
}

// 컨테이너에 서비스 등록
const builder = new ServiceContainerBuilder();
builder.addSingleton(Logger);
builder.addSingleton(Database);
builder.addSingleton(UserService);

const serviceProvider = builder.build();

// UserService 해결 및 사용
const userService = serviceProvider.getService(UserService);
userService.getUser('12345');
```

이 예제에서:

- Logger 서비스는 독립적입니다.
- Database 서비스는 Logger 서비스에 의존하며, @injectable(() => [Logger]) 데코레이터를 사용하여 지정됩니다.
- UserService는 Database 서비스에 의존하며, 비슷하게 @injectable(() => [Database]) 데코레이터로 지정됩니다.

<div class="content-ad"></div>

이 설정은 UserService를 요청할 때 컨테이너가 종속성을 명확히 정의하고 자동으로 해결하도록 보장합니다.

주요 포인트: @wroud/di의 장점은 사용하는 데코레이터 버전이 무엇이든 상관없이 또는 게으르게 로딩과 같은 고급 기능을 구현하는지 여부와 관계없이 이 API가 일관적으로 유지된다는 것입니다. 이 일관성은 애플리케이션을 손상시키는 변경 사항을 걱정하지 않고 스케일을 쉽게 조절하고 리팩토링할 수 있음을 보장합니다.

이 예제들은 @wroud/di가 JavaScript 또는 TypeScript 애플리케이션에서 DI를 구현하는 것이 얼마나 쉬운지, 사용하는 데코레이터 버전이 무엇이든지나 프로젝트의 복잡성에 상관없이 보여줍니다. 이제 얼마나 직관적으로 작업할 수 있는지 보셨으니, Dependency Injection을 위한 최상의 선택으로 @wroud/di를 선택해야 하는 이유에 대해 종결하겠습니다.

# 결론

<div class="content-ad"></div>

의존성 주입은 JavaScript 및 TypeScript 애플리케이션의 모듈성, 테스트 용이성 및 유지 보수성을 크게 향상시킬 수 있는 강력한 패턴입니다. 그러나 많은 기존 DI 라이브러리의 단점은 좌절, 비효율성 및 기술 부채로 이어질 수 있습니다.

@wroud/di는 이러한 공통적인 고통 점을 해결하는 현대적이고 직관적이며 확장 가능한 솔루션을 제공합니다. 오래된 관행, 복잡한 API 또는 지연 로딩 및 코드 분할과 같은 고급 기능이 필요한 경우, @wroud/di는 당신의 요구에 적응하는 일관된 신뢰할 수 있는 API를 제공합니다.

이 기사에서 제공된 예제는 @wroud/di가 능숙한지 엿볼 수 있는 간략한 소개일 뿐입니다. 오래된 및 현대적인 데코레이터에 대한 포괄적인 지원, 명확한 서비스 관리 및 견고한 오류 처리를 통해 @wroud/di는 개발자가 쉽게 확장 가능하고 유지보수 가능한 애플리케이션을 구축할 수 있도록 돕습니다.

그러나 이것은 시작에 불과합니다. @wroud/di에는 개발 프로세스를 더욱 간편화하는 더 많은 기능과 능력이 패키지되어 있습니다. 강력한 도구를 모두 탐험하고 @wroud/di를 최대한 활용하는 방법을 배우려면 wroud.dev에서 전체 문서 및 추가 예제를 확인해보세요.

<div class="content-ad"></div>

누구든지 경험 많은 개발자이든 디펜던시 인젝션을 막 시작한 초보자이든, @wroud/di가 여러분의 개발 여정을 더 원할하고 효율적으로 만들어 드립니다. 함께 더 즐거운 개발을 할 수 있게 도와드릴게요.