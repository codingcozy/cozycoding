---
title: "앵귤러에서 RxJS 코드 최적화하기"
description: ""
coverImage: "/assets/img/2024-08-21-Optimization-RxJScodeinAngular_0.png"
date: 2024-08-21 17:30
ogImage: 
  url: /assets/img/2024-08-21-Optimization-RxJScodeinAngular_0.png
tag: Tech
originalTitle: "Optimization-RxJS code in Angular"
link: "https://medium.com/gitconnected/optimization-rxjs-code-in-angular-c454d6cf8333"
isUpdated: false
---


<img src="/assets/img/2024-08-21-Optimization-RxJScodeinAngular_0.png" />

요즘 코드 리뷰를 하면서 RxJS가 얼마나 적게 사용되고 있는지 놀랐어요.

한 서비스에서 API로부터 데이터를 호출하고 두 컴포넌트가 서비스가 반환한 데이터를 사용하고 있네요.

두 컴포넌트 모두 if 블록이 반복되고 컴포넌트 B에서는 추가적인 로직이 수행되고 있는 것을 볼 수 있어요.

<div class="content-ad"></div>

위 코드의 문제점은 무엇인가요?

맞아요! 맞추셨네요. 코드가 반복되고 있습니다. DRY는 자신을 반복하지 말라고 말합니다. 코드 중복은 미묘한 버그를 야기할 수 있는 요점입니다. 만약 if 블록의 로직을 수정해야 한다면 두 컴포넌트에서 수정해야 합니다. 그리고 만약 놓치게 된다면 얻어맞을 수도 있겠죠 😝.

그리고 사람들은 이를 처리하기 위해 다양한 방법을 발명하지만 그 결과로 코드 스멜만 더해지는 경우가 많습니다. 이에 대해 다음 파트에서 보여드릴게요.

해결책은 간단합니다. 중복된 로직을 서비스로 이동시키고 map RxJS 연산자를 사용하여 코드를 리팩토링하는 것입니다.

<div class="content-ad"></div>

이제 서비스에서 map 연산자를 사용하여 데이터를 변환했다는 것을 알 수 있습니다. 중복 로직은 서비스로 위임되고, 컴포넌트의 코드는 서비스에서 받은 처리된 데이터에 중점을 둡니다.

유용하게 활용하시기 바랍니다.

저는 이와 같은 문제와 해결책을 다루는 많은 실용적인 내용을 포함한 Udemy의 Angular 코스를 만들었습니다. 이는 귀하의 전문적인 Angular 여정의 발판이 될 수 있습니다. 한 번 살펴봐 주세요.

![2024-08-21-Optimization-RxJScodeinAngular_1.png](/assets/img/2024-08-21-Optimization-RxJScodeinAngular_1.png)

<div class="content-ad"></div>

제 무료 YouTube 채널도 구독하거나 시청하실 수 있어요.

구독/팔로우/좋아요/박수 부탁드려요.