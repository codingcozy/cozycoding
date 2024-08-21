---
title: "Spring Boot를 사용하여 코드를 더 깔끔하게 만드는 사용자 정의 어노테이션 생성 방법"
description: ""
coverImage: "/assets/img/2024-08-21-HowtoCreateCustomAnnotationsinSpringBootforCleanerCode_0.png"
date: 2024-08-21 17:45
ogImage: 
  url: /assets/img/2024-08-21-HowtoCreateCustomAnnotationsinSpringBootforCleanerCode_0.png
tag: Tech
originalTitle: "How to Create Custom Annotations in Spring Boot for Cleaner Code"
link: "https://medium.com/@mahesh.babu11/how-to-create-custom-annotations-in-spring-boot-for-cleaner-code-5067f969d5ba"
isUpdated: false
---


<img src="/assets/img/2024-08-21-HowtoCreateCustomAnnotationsinSpringBootforCleanerCode_0.png" />

안녕하세요! 이미 여러분 중에는 같은 코드 줄을 반복해서 작성하는 데 갇힌 적이 있으며, 이 프로세스를 간소화할 방법이 있었으면 하는 바람을 품은 적이 있을 것입니다. Spring Boot을 사용하고 계시다면, 주석이 코드를 더 깨끗하고 효율적으로 만드는 데 얼마나 강력한지 잘 아실 겁니다. 그러나 기존 주석이 당신의 요구에 완전히 부합하지 않을 때는 어떻게 해야 할까요?

그곳에서 사용자 정의 주석이 등장합니다. 반복적인 코드 패턴을 캡슐화하는 자신만의 주석을 만들 수 있다면 어떨까요? 이렇게 하면 당신의 코드는 더 깨끗할 뿐 아니라 유지하는 것도 더 쉬워집니다. 이 기사에서는 Spring Boot에서 사용자 정의 주석을 어떻게 만들어 개발 과정을 간소화하고 중복 코드의 머리아픔을 피할 수 있는지 살펴볼 것입니다. 최상의 관행을 시행하고 사용자 지정 유효성 검사를 추가하거나 로직을 단순화하려면 사용자 정의 주석이 필요할 수 있습니다. 이제 코드가 더 똑똑하게 동작하도록 만들기 위해 뛰어들어 시작해 봅시다!

예를 하나 공유하겠습니다: 제가 애플리케이션에서 발생하는 모든 오류를 오류 로그 테이블에 기록해야 하는 상황에 직면했습니다. 필요한 모든 메서드에 같은 로깅 코드를 반복적으로 추가하는 것이 지치기 시작했습니다. 사용자 정의 주석을 사용하여 이 문제를 어떻게 해결했는지 보여드리겠습니다.

<div class="content-ad"></div>

이 사용자 정의 주석의 아이디어는 메소드에서 반복적인 오류 처리 코드가 필요 없도록 하는 것입니다. 각 메소드를 try-catch 블록으로 수동으로 감싸서 예외를 로그로 기록하는 대신, 사용자 정의 주석을 간단히 추가할 수 있습니다. 이 주석은 메소드 내에서 발생하는 모든 오류를 자동으로 로그로 기록해야 한다는 신호 역할을 합니다. 내부적으로, 이 주석은 Spring의 관점 지향 프로그래밍 (AOP) 능력을 활용하여 메소드 호출을 가로채서 예외를 처리하고 미리 정의된 형식에 따라 로그를 남기게 됩니다.

이 접근 방식은 보일러플레이트 코드를 줄이는 것뿐만 아니라 전체 애플리케이션에서 오류 처리와 로깅이 어떻게 다루어지는지에 일관성을 보장합니다. 주석 내부에 오류 처리 로직을 집중시킴으로써 개별 메소드를 건드리지 않고 로깅 전략을 쉽게 관리하고 업데이트할 수 있습니다. 코드베이스를 깔끔하고 유지 보수 가능하며 중복적인 오류 처리 코드 없이 유지할 수 있는 강력한 방법입니다.

시작해보죠!

기존 프로젝트에 통합하고 있다고 가정합니다. 그렇지 않은 경우 spring.io로 이동하여 새로운 Spring Boot 프로젝트를 만들 수 있습니다.

<div class="content-ad"></div>

# 단계 1: 프로젝트에 Spring AOP 종속성 추가하기

최신 종속성을 찾으려면 Maven 저장소를 방문하거나 아래 제공된 것을 사용할 수 있습니다:

```js
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-aop</artifactId>
    <version>6.1.12</version>
</dependency>
```

# 단계 2: 사용자 정의 주석 만들기

<div class="content-ad"></div>

사용자 정의 어노테이션을 정의하여 에러 처리를 위해 메서드를 표시하는 데 사용됩니다. 다음과 같이 ErrorHandler 어노테이션을 작성할 수 있습니다:

```java
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface ErrorHandler {
}
```

사용자 정의 어노테이션을 만드는 첫 번째 단계는 @interface 키워드를 사용하여 선언하는 것입니다. 이 경우 ErrorHandler로 선언합니다. 다음 단계는 코드에 대한 메타데이터를 제공하는 것인데, @Retention과 @Target은 이러한 어노테이션이 어떻게 사용되고 어디에 사용될 수 있는지를 정의하는 두 가지 중요한 요소입니다.

# @Retention

<div class="content-ad"></div>

@Retention 어노테이션은 어노테이션이 유지되는 기간을 지정합니다:

- RetentionPolicy.SOURCE: 어노테이션은 소스 코드에서만 유지되며 컴파일러가 컴파일하는 동안 폐기됩니다. 컴파일된 .class 파일에 포함되지 않으며 런타임에서 사용할 수 없습니다.
- RetentionPolicy.CLASS: 어노테이션은 컴파일된 .class 파일에 유지되지만 런타임에서 사용할 수 없습니다. @Retention을 지정하지 않을 경우 기본 동작입니다.
- RetentionPolicy.RUNTIME: 어노테이션은 컴파일된 .class 파일에 유지되고 리플렉션을 통해 런타임에서 사용할 수 있습니다. 이로써 런타임에서 프레임워크와 라이브러리가 어노테이션을 사용할 수 있습니다.

우리의 ErrorHandler 어노테이션에서 @Retention(RetentionPolicy.RUNTIME)를 사용합니다. 이는 어노테이션이 런타임에 사용 가능하며, 애플리케이션 실행 중에 어노테이션을 검사해야 하는 프레임워크나 사용자 정의 로직에 필요합니다.

# @Target

<div class="content-ad"></div>

@Target 어노테이션은 어노테이션이 적용될 수 있는 요소의 종류를 지정합니다:

- ElementType.TYPE: 어노테이션을 클래스, 인터페이스 또는 enum에 적용할 수 있습니다.
- ElementType.FIELD: 어노테이션을 필드(변수)에 적용할 수 있습니다.
- ElementType.METHOD: 어노테이션을 메서드에 적용할 수 있습니다.
- ElementType.PARAMETER: 어노테이션을 메서드 매개변수에 적용할 수 있습니다.
- ElementType.CONSTRUCTOR: 어노테이션을 생성자에 적용할 수 있습니다.
- ElementType.LOCAL_VARIABLE: 어노테이션을 지역 변수에 적용할 수 있습니다.
- ElementType.ANNOTATION_TYPE: 어노테이션을 다른 어노테이션에 적용할 수 있습니다.
- ElementType.PACKAGE: 어노테이션을 패키지 선언에 적용할 수 있습니다.

ErrorHandler 어노테이션의 경우, @Target(ElementType.METHOD)는 어노테이션이 메서드에만 적용될 수 있다고 지정합니다. 이는 @ErrorHandler를 사용하여 사용자 정의 오류 처리 로직을 적용할 메서드를 지정할 수 있지만, 클래스, 필드 또는 다른 요소에는 적용할 수 없다는 것을 의미합니다.

이제 ErrorHandler 애스펙트를 정의해봅시다.

<div class="content-ad"></div>

```java
package com.maheshbabu11.spring_custom_annotations.service;

import com.maheshbabu11.spring_custom_annotations.annotations.ErrorHandler;
import org.springframework.stereotype.Service;

@Service
public class MyService {

    @ErrorHandler
    public void myMethod() {
        // Method logic that may throw an exception
        throw new RuntimeException("An error occurred!");
    }
}
``` 

이제 이 주석을 필요한 메서드에 추가하고 어떻게 작동하는지 확인해 봅시다.

# 단계 3: 필요한 메서드에 사용자 지정 주석 추가하기.

<div class="content-ad"></div>

우리의 메소드에 사용자 정의 주석을 추가하고 해당 메소드에서 예외가 발생할 때 어떤 일이 발생하는지 확인해 봅시다.

```js
   @ErrorHandler
    public void testExceptionLogging() {

        //예외 발생 시뮬레이션
        if (true) {
            throw new RuntimeException("예외가 발생했습니다");
        }
    }
```

메소드가 호출되자마자 새로운 런타임 예외가 발생하여 ErrorHandlerAspect의 @AfterThrowing 어드바이스가 트리거됩니다. 이로써 콘솔에 예외 메시지가 출력되고 오류 로그 테이블에 오류가 로깅됩니다.

<img src="/assets/img/2024-08-21-HowtoCreateCustomAnnotationsinSpringBootforCleanerCode_1.png" />

<div class="content-ad"></div>

해당 기사에서 사용된 완전한 소스 코드는 다음에서 확인할 수 있습니다:

끝으로, Spring Boot에서 사용하는 사용자 정의 어노테이션인 @ErrorHandler 어노테이션과 같은 어노테이션은 반복적인 작업을 자동화하고 코드 유지 관리를 향상시키는 강력한 메커니즘을 제공합니다. Spring AOP를 활용하여 오류 처리와 로깅을 최적화함으로써, 예외가 일관되게 기록되고 관리되도록 보장하며 비즈니스 로직을 반복적인 try-catch 블록으로 난잡해지지 않도록 합니다. 이 접근 방식은 코드를 단순화뿐만 아니라 오류 처리를 중앙 집중화하여 업데이트 및 유지 관리를 보다 쉽게 할 수 있도록합니다. 사용자 정의 어노테이션을 사용하면, 응용 프로그램의 핵심 기능에 집중할 수 있으며 오류 로깅과 같은 교차 관심 사항을 처리하기 위해 견고하고 자동화된 매커니즘에 의존할 수 있습니다.

코딩 즐기기 😊!!! 읽어 주셔서 즐거우셨다면 👏을 남겨주세요.