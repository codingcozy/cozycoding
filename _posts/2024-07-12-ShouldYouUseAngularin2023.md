---
title: "2023년에 Angular를 사용해야 할까"
description: ""
coverImage: "/assets/img/2024-07-12-ShouldYouUseAngularin2023_0.png"
date: 2024-07-12 22:27
ogImage: 
  url: /assets/img/2024-07-12-ShouldYouUseAngularin2023_0.png
tag: Tech
originalTitle: "Should You Use Angular in 2023?"
link: "https://medium.com/better-programming/should-you-use-angular-in-2023-e85221071712"
---


![이미지](/assets/img/2024-07-12-ShouldYouUseAngularin2023_0.png)

얼마 전에 고객 전화 중에 Angular에 대한 이야기가 나왔을 때 그가 얼굴을 찡그리고 비웃었다.

"지금은 누가 Angular을 사용하나?" 라고 했다.

나는 준비가 되어 있지 않았다. 그러다가 그가 Vue를 좋아하는 사람이라는 것을 알게 되었는데, 괜찮아요 - 사람들은 익숙한 것을 좋아합니다. 하지만 그러다가 나는 생각하게 되었어요, Angular를 선택하는 사람들은 누구일까? 왜 사람들은 React와 같은 더 쉬운 것보다 Angular를 선택할까요?

<div class="content-ad"></div>

정말 솔직해요, Angular은 배우기 쉽지 않아요. 결국 프레임워크니까, 작동을 위해 지켜야 할 것들이 있어요.

하지만 Angular에는 장점이 있어요.

# Angular의 장점

처음 시작하는 사람들에게는 Angular이 번거로워 보일 수 있어요. 프레임워크를 시작하기 전에 알아야 할 지식이 많아서요.

<div class="content-ad"></div>

하지만 사실을 이야기하자면, 일반적으로 그렇습니다. 새로운 것을 배우는 것이 필요할 때, 우리 중 일부는 두닝-크루거 효과를 가장 특히 느끼는 뇌 속의 자동 저항체를 갖고 있습니다.

새로운 것을 배우는 것은 특히 편안한 영역에 적응한 후에는 시간이 지남에 따라 더욱 괴롭습니다, 특히 우리가 전문가라고 생각될 때에는 더 심하게 그렇습니다.

하지만 우리 모두 어느 정도는 초보자이며, 프레임워크, 라이브러리 및 아이디어 간에 전반적인 컨셉 지식을 전달할수록 새로운 것을 배우는 학습 곡선을 오르는 동안 그 감정을 극복하기가 더 쉬워집니다.

![](/assets/img/2024-07-12-ShouldYouUseAngularin2023_1.png)

<div class="content-ad"></div>

회자되었네요. 그러면 사람들이 Angular을 사용하는 이유는 무엇일까요?

Angular은 코드를 바이트 크기의 블록으로 구성하는 컴포넌트 기반 구조를 사용하여 관심사를 명확히 분리하여 코드를 구성하기 쉽게 합니다. 모듈성은 Angular 프레임워크의 기반입니다.

React와 Vue에 대해서도 비슷한 말을 할 수 있지만, 어떻게 구현되는지가 Angular을 다른 프레임워크에서 차별화시킵니다.

Angular은 강력한 의견을 가지고 있습니다. 이는 무언가를 하고 싶을 때 특정한 방법으로 해야 한다는 것을 의미합니다. 협상의 여지가 없습니다. 한편 React는 라이브러리이므로 구현 방식에 유연성이 있지만, 이는 다양성을 초래할 수도 있습니다.

<div class="content-ad"></div>

작은 프로젝트에 대해서는 React가 좋습니다. 원하는 대로 원하는 대로 작업할 수 있기 때문이죠. 그러나 프로젝트가 커지고 팀에서 다양한 의견이 충돌할 때, 특정한 패턴과 구현이 어떻게 보이길 원하는지에 대한 갈등이 생길 수 있습니다.

한 사람의 최선의 사례 버전이 다른 주장하는 팀원과 일치하지 않을 수 있습니다. 우리는 인간이니까요. 일어나는 일이죠. React와 Vue는 대규모 프로젝트에 사용되기 때문에, 세상이 끝나는 것은 아니며 단지 더 많은 협상이 필요하고 팀에서의 표준화가 필요합니다.

Angular은 어떻게 일을 수행해야 하는지에 대한 청사진을 제공함으로써 이를 해결합니다.

## 하지만 단지 강제 모듈화 이상의 것이 있을 것입니다.

<div class="content-ad"></div>

네, 당연히 있어요. 

의존성 주입은 Angular를 특별하게 만드는 또 다른 기능입니다. 최근 몇 년 동안 React가 따라 잡았지만, Angular가 의존성 주입을 다뤄오는 방식이 의존성을 다루는 과정을 매끄럽게 만듭니다.

의존성 주입이 무엇인가요?

의존성 주입(Dependency Injection, DI)은 소프트웨어 개발에서 사용되는 디자인 패턴으로, 컴포넌트와 해당 의존성을 분리하는 데 도움을 주는 것입니다. 간단히 말해, DI는 컴포넌트나 클래스에 기능을 제공하기 위해 해당 의존성을 생성하거나 인스턴스화하는 대신 필요한 의존성을 제공하는 방법입니다.

<div class="content-ad"></div>

Angular에서는 의존성 주입(Dependency Injection)이 응용 프로그램의 의존성을 관리하는 프로세스를 간소화하는 핵심 기능입니다. Angular의 DI 시스템은 인젝터(Injectors) 및 프로바이더(Providers) 개념을 중심으로 구성되어 있습니다. 인젝터는 의존성의 인스턴스를 생성하고 관리하는 역할을 하며, 프로바이더는 해당 의존성이 어떻게 생성되어야 하는지 구성하는 데 사용됩니다.

## 여기서 의존성 주입이 중요한 이유

Decoupling: DI는 컴포넌트와 그 의존성 간의 느슨한 결합을 촉진하여 코드를 수정, 테스트 및 유지 관리하기 쉽게 만듭니다. 의존성을 주입함으로써, 컴포넌트가 특정 구현에 강하게 결합되지 않으므로 변경 사항을 가할 때 더 큰 유연성을 제공합니다.

재사용성: DI를 사용하면 서비스나 다른 의존성의 인스턴스를 쉽게 재사용하고 여러 부분에서 공유할 수 있습니다. 이를 통해 코드 중복이 줄어들고 더 깔끔하고 조직적인 코드베이스가 유도됩니다.

<div class="content-ad"></div>

테스트 용이성: DI를 사용하면 컴포넌트나 클래스에 대한 단위 테스트 작성이 쉬워지며, 실제 종속성을 모의 객체로 쉽게 대체할 수 있습니다. 이를 통해 테스트되는 컴포넌트나 클래스를 격리시키고 해당 기능에 집중할 수 있습니다. 종속성의 동작에 대해 걱정하지 않고 기능에 집중할 수 있습니다.

다음은 Angular에서 Dependency Injection을 설명하는 간단한 예시입니다:

```js
import { Injectable } from '@angular/core';

@Injectable()
export class UserService {
  getUsers() {
    // API나 기타 데이터 소스에서 사용자를 가져오는 로직
  }
}
```

이 예시에서는 getUsers 메서드를 갖는 UserService 클래스를 정의합니다. @Injectable 데코레이터는 Angular에게 이 클래스가 종속성으로 사용될 수 있고 DI 시스템에서 관리되어야 함을 알려줍니다.

<div class="content-ad"></div>

이제 UserService를 사용하여 사용자 데이터를 가져와야하는 컴포넌트를 만들어봅시다:

```js
import { Component, OnInit } from '@angular/core';
import { UserService } from './user.service';

@Component({
  selector: 'app-users',
  template: `
    <ul>
      <li *ngFor="let user of users">{ user.name }</li>
    </ul>
  `,
})
export class UsersComponent implements OnInit {
  users = [];

  constructor(private userService: UserService) {}

  ngOnInit() {
    this.users = this.userService.getUsers();
  }
}
```

UsersComponent에서는 UserService를 생성자에 매개변수로 추가하여 주입합니다. Angular의 DI 시스템은 자동으로 UserService의 인스턴스를 생성하고 컴포넌트에 제공합니다. 이렇게하면 UserService를 수동으로 인스턴스화하거나 관리하는 것을 걱정할 필요없이 컴포넌트가 초기화될 때 userService 인스턴스를 사용하여 사용자를 가져올 수 있습니다.

Angular에서 의존성 주입을 사용하면 더 깨끗하고 유지보수 가능하며 테스트 가능한 코드를 작성할 수 있어 결국 더 견고하고 확장 가능한 애플리케이션을 만들 수 있습니다.

<div class="content-ad"></div>

React와 Vue는 Angular와는 달리 의존성 주입을 다르게 다룹니다. React와 Vue는 더 가볍고 덜 주장적이어서 Angular처럼 내장된 의존성 주입 시스템을 제공하지 않습니다. 그러나 개발자들은 여전히 React 및 Vue 애플리케이션에서 다양한 기술과 라이브러리를 사용하여 의존성 주입을 구현할 수 있습니다.

React는 데이터와 의존성을 컴포넌트 트리 아래로 전달하기 위해 "props"를 사용합니다. 이는 간단한 애플리케이션에 대해서는 작동할 수 있지만, 애플리케이션이 커짐에 따라 관리하기 어려워집니다. 더 큰 React 애플리케이션에서 의존성 주입을 다루기 위해 개발자는 InversifyJS와 같은 서드파티 라이브러리를 사용하거나 React의 context API를 활용할 수 있습니다. 이를 통해 프롭스를 통해 값을 전달하지 않고 컴포넌트 트리 전체에서 값을 공유할 수 있습니다.

```js
// createContext
import React, { createContext, useContext } from 'react';
import UserService from './UserService';

const UserServiceContext = createContext(new UserService());

function Users() {
  const userService = useContext(UserServiceContext);
  // userService를 사용하여 사용자 데이터를 가져올 수 있음
}
```

한편 Vue는 의존성 주입을 다루기 위한 “provide/inject”이라는 내장된 메커니즘을 갖고 있습니다. Angular의 DI 시스템만큼 강력하지는 않지만, 이를 통해 데이터와 의존성을 프롭스를 통해 전달하지 않고 컴포넌트 간에 공유할 수 있습니다.

<div class="content-ad"></div>

```js
// 부모 구성 요소 또는 Vue 인스턴스에서
import UserService from './UserService';

export default {
  provide: {
    userService: new UserService(),
  },
};

// 자식 구성 요소에서
export default {
  inject: ['userService'],
  // userService를 사용하여 사용자 데이터 가져오기
};
```

앵귤러의 의존성 주입 시스템은 React와 Vue보다 더 심화되고 의견이 분분합니다. 이것은 프레임워크에 내장되어 있어 응용 프로그램 전반에 걸쳐 의존성을 일관되게 관리하기 쉽습니다. 앵귤러의 의존성 주입 시스템은 데코레이터, 계층적 주입기 및 프로바이더를 사용하여 의존성을 만들고 관리하는 데 보다 유연성과 제어를 제공합니다.

# 핵심으로 돌아가면: 아직 앵귤러를 사용하는 사람들은 누구인가요?

2022년 Stack Overflow 개발자 설문조사에 따르면, 앵귤러는 웹 프레임워크 및 기술에서 20.39%를 차지합니다. React가 42.62% 에 앉아 있다면, 앵귤러는 "코드 학습 중인 사람보다 전문 개발자들이 더 많이 사용하는" 것으로 Stack Overflow에서 지적합니다.


<div class="content-ad"></div>

React가 인기가 많은 반면 그 인기는 웹용 싱글 페이지 앱을 구축하기 위한 게이트웨이로 작용하고 있다고 말하고 있습니다. 한편으로는 Angular의 전임자인 Vue(*아헴* Angular.js, 오늘날의 Angular와는 완전히 다릅니다)가 인기 투표에서 확실한 18.82%를 차지하고 있습니다.

State of JS 2021에 따르면, 섞이고 맞춰진 프레임워크와 라이브러리의 수는 시간이 지남에 따라 분화되었습니다.

![이미지](/assets/img/2024-07-12-ShouldYouUseAngularin2023_2.png)

수치의 합이 100이 되지 않는 이유는 영원히 그렇지 않기 때문입니다. JS의 가벼움과 모듈성의 표준화는 일반적으로 프레임워크와 라이브러리가 고립적으로 사용되지 않는다는 것을 의미합니다.

<div class="content-ad"></div>

Google Trends에 따르면 React가 명확한 우승자인데, 라이브러리나 프레임워크를 사용하는 다양한 유형과 수준의 개발자를 고려하지 않습니다.

![image](/assets/img/2024-07-12-ShouldYouUseAngularin2023_3.png)

내 연구에서는 Angular이 스타트업보다 기업에서 더 많이 사용되고 있다는 점이 강조되고 있습니다.

그래서 급여 차이에 대해 궁금해졌어요.

<div class="content-ad"></div>

Arc에 따르면, 원격 Angular 개발자들은 연간 평균 $71,326을 벌고 있습니다. 반면에, 원격 React 개발자들은 연간 평균 $72,342를 벌고 있습니다.

![image 1](/assets/img/2024-07-12-ShouldYouUseAngularin2023_4.png)

![image 2](/assets/img/2024-07-12-ShouldYouUseAngularin2023_5.png)

하지만, Glassdoor의 통계는 다른 모습을 보여주고 있습니다.

<div class="content-ad"></div>

![이미지1](/assets/img/2024-07-12-ShouldYouUseAngularin2023_6.png)

![이미지2](/assets/img/2024-07-12-ShouldYouUseAngularin2023_7.png)

# Angular을 아직 배워야 할 가치가 있는가요?

결국 어떤 프레임워크나 라이브러리를 전문으로 하는지는 별로 중요하지 않아요. 프로젝트나 고객의 요구 사항과 기대를 충족시킬 수 있는 능력이 가장 중요한 거죠.

<div class="content-ad"></div>

모든 것이 끝나면, 비 기술자들은 모듈식이거나 깨끗한 코드인지에 대해 디테일을 신경 쓰지 않습니다. 중요한 것은 구축해야 하는 디지털 부동산이 작동하고, 고트래픽 시그널이 발생하더라도 곧바로 고장 나지 않는 것입니다. 이는 프레임워크나 라이브러리 보다는 인프라 확장에 더 가까운 부분입니다.

Angular에서 강조된 아이디어는 좋고, 깔끔하고 정돈된 방식으로 일을 처리하는 데 도움이 되는 훌륭한 습관을 가르칠 수 있습니다. 한 가지에만 치우치지 말고, 모든 것에 대해 일반적으로 알고 있는 것이 좋습니다.

어떤 분야의 전문가가 되는 것은 자신을 홍보하는 데 도움이 되지만, 코딩의 실제 작업에 오면, 이전 프로젝트에서 알고 있던 조각들을 서로 엮어 일을 처리하는 즉흥적인 공간입니다.