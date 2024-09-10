---
title: "SpringBoot 3.3 새롭게 강화된 기능. 꼭 알아야 할 사항"
description: ""
coverImage: "/assets/img/2024-09-10-SpringBoot33NewenhancedfeatureMust-Know_0.png"
date: 2024-09-10 18:49
ogImage: 
  url: /assets/img/2024-09-10-SpringBoot33NewenhancedfeatureMust-Know_0.png
tag: Tech
originalTitle: "SpringBoot 3.3 New enhanced feature. Must-Know"
link: "https://medium.com/gitconnected/springboot-3-3-new-enhanced-feature-must-know-3188305c3fd0"
isUpdated: false
---


동시성 및 I/O 바운드 작업의 성능 향상

SpringBoot 3.3에는 많은 향상된 기능들이 있습니다. 이 글에서는 Virtual Threads 및 3.3에서의 향상에 대해 이야기하려고 합니다.

![SpringBoot 3.3 New Enhanced Feature](/assets/img/2024-09-10-SpringBoot33NewenhancedfeatureMust-Know\_0.png)

가상 스레드란 무엇인가요?

<div class="content-ad"></div>

가상 스레드는 전통적인 플랫폼 스레드에 대한 가벼운 대안을 제공합니다. 운영 체제가 아닌 JVM에 의해 관리되며, 가상 스레드는 전통적인 스레드 관리의 부담 없이 방대한 수의 동시 작업을 처리하기 위해 설계되었습니다. 실행 중에 플랫폼 스레드에 바운드되지만 생성 및 스케줄링시 상당히 적은 오버헤드를 가집니다. 이 설계는 고도의 동시성 환경에서 스레드 생성 및 컨텍스트 전환이 연관된 성능 제한을 완화하기 위한 것입니다.

가상 스레드의 사용 사례

1. 고 동시성 요청 처리: 높은 수준의 사용자 요청을 처리하는 웹 애플리케이션은 가상 스레드에서 혜택을 받을 수 있습니다. 전통적인 스레드 풀은 고갈될 수 있어 성능 병목 현상을 초래할 수 있습니다. 가상 스레드를 사용하면 수천 개 또는 수백만 개의 동시 요청으로 확장할 수 있고 성능 저하가 크지 않습니다.

2. I/O-바운드 작업: 데이터베이스 쿼리 또는 네트워크 호출과 같이 방대한 I/O 작업이 포함된 시나리오에서 가상 스레드가 뛰어난 성능을 발휘합니다. 기다리는 동안 자원을 차지하고 차단 시키는 전통적인 스레드와 달리, 가상 스레드는 이러한 차단 작업을 효율적으로 처리하여 자원 활용을 향상시킵니다.

3. 긴 실행 시간 작업: 데이터 처리 또는 로그 분석과 같이 오랜 기간 실행되는 작업은 가상 스레드의 낮은 오버헤드에서 혜택을 받을 수 있습니다. 가상 스레드는 상당한 시스템 자원을 소비하지 않고 긴 실행 작업을 관리할 수 있어 성능과 확장성을 유지할 수 있습니다.

# Spring Boot에서 가상 스레드 지원

<div class="content-ad"></div>

가상 스레드는 프로젝트 룸(Project Loom)의 미리보기 기능으로 Java 19에서 소개된 기능입니다. Java 애플리케이션의 동시성을 보다 효율적으로 관리할 수 있습니다. 가상 스레드는 Java의 일부이지만, Spring Boot는 버전 3.2부터 지원을 시작했습니다.

Spring Boot 3.3은 이 지원을 더욱 다듬어 Java 19와의 통합을 최적화하여 가상 스레드를 효과적으로 활용합니다. 이 업데이트는 가상 스레드의 장점을 동시 요청 처리, I/O 작업, 그리고 오랜 시간 소요되는 작업을 포함한 다양한 애플리케이션 개발 측면에 확장합니다.

주요 이정표

- Java 19: 미리보기 기능으로 가상 스레드를 도입했습니다.
- Spring Boot 3.2: Spring의 동시성 메커니즘과의 통합을 포함하여 가상 스레드 지원 추가.
- Spring Boot 3.3: 성능 최적화 및 사용 사례 확대를 위해 가상 스레드 지원을 강화했습니다.

<div class="content-ad"></div>

# Spring Boot에서 더 강화된 지원

1. Core Spring Framework 적응

Spring Boot 3.3은 가상 스레드를 보다 잘 지원하기 위해 다양한 핵심 Spring 기능을 적응시킵니다. 트랜잭션 관리, 비동기 작업 실행 및 이벤트 처리와 같은 메커니즘이 가상 스레드와 매끄럽게 작동하도록 보장합니다.

- 비동기 작업 실행: @Async 어노테이션에 대한 개선된 지원으로, 가상 스레드 기반의 실행자를 사용하여 동시성 성능을 향상시킵니다.

<div class="content-ad"></div>

```java
@Configuration
public class AsyncConfig implements AsyncConfigurer {

    @Override
    public Executor getAsyncExecutor() {
        return Executors.newVirtualThreadPerTaskExecutor();
    }
}
```

- 이벤트 주도 아키텍처: 가상 스레드는 이벤트 주도 아키텍처에서 효율적으로 이벤트를 처리하여 전통적인 스레드 리소스에 제한받지 않고 빠르게 이벤트를 처리할 수 있습니다.

```java
@Configuration
public class AsyncConfig implements AsyncConfigurer {

    @Override
    public Executor getAsyncExecutor() {
        return Executors.newVirtualThreadPerTaskExecutor();
    }
}
```

2. Spring MVC 및 WebFlux 통합

<div class="content-ad"></div>

Spring Boot 3.3에서는 Spring MVC 및 WebFlux의 가상 스레드 지원이 강화되어, 특히 고 동시성 환경에서 HTTP 요청 처리의 효율성이 향상되었습니다.

- Spring MVC: 가상 스레드를 사용하면 Spring MVC가 스레드 풀 고갈을 피하고 고부하 시나리오에서 요청을 더 효율적으로 처리할 수 있습니다.

```java
@RestController
@RequestMapping("/api")
public class ExampleController {

    @GetMapping("/data")
    public CompletableFuture<String> getData() {
        return CompletableFuture.supplyAsync(() -> {
            
            // 여기에서 로직 처리
            return "데이터 처리 완료";
        }, Executors.newVirtualThreadPerTaskExecutor());
    }
}
```

- WebFlux: WebFlux의 반응형 모델은 가상 스레드를 통해 블로킹 작업을 처리하면서 비블로킹 특성을 유지하는 이점을 얻습니다.

<div class="content-ad"></div>

3. Spring Data와 트랜잭션 관리

가상 스레드는 데이터베이스 접근 및 트랜잭션 관리를 향상시켜 블로킹 시간을 줄이고 올바른 트랜잭션 전파를 보장합니다.

```java
@Service
public class UserService {

    @Autowired
    private UserRepository userRepository;

    @Transactional
    public void processUser(Long userId) {
        Executors.newVirtualThreadPerTaskExecutor().submit(() -> {
            User user = userRepository.findById(userId).orElseThrow();
            // 여기에서 로직 처리
        }).join(); // 트랜잭션이 올바르게 종료되도록 확인
    }
}
```

4. 향상된 오류 처리 및 디버깅 지원

<div class="content-ad"></div>

Spring Boot 3.3은 가상 스레드에 대한 디버깅 및 오류 처리를 개선하여, 예외를 캡처하고 관리하기 쉽게 하며 명확한 스택 추적 및 스레드 정보를 제공합니다.

- 향상된 예외 처리: 가상 스레드 예외를 캡처하고 균일하게 처리하여 디버깅을 위한 자세한 스택 추적과 스레드 컨텍스트를 제공합니다.

```java
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.RestControllerAdvice;

@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(Exception.class)
    public ResponseEntity<String> handleException(Exception e) {
        // 디버깅을 위해 스택 추적을 출력합니다
        StringBuilder stackTrace = new StringBuilder();
        for (StackTraceElement element : e.getStackTrace()) {
            stackTrace.append(element.toString()).append("\n");
        }
        
        // 또한 현재 스레드 정보도 출력합니다
        String threadInfo = "Thread: " + Thread.currentThread().getName();
        
        System.err.println("예외 발생 위치: " + threadInfo);
        System.err.println(stackTrace.toString());

        return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR)
                .body("오류 발생: " + e.getMessage() + "\n" + stackTrace.toString() + "\n" + threadInfo);
    }
}
```

오류 로그는 다음과 같이 보입니다:

<div class="content-ad"></div>

```js
Thread: VirtualThread-17 (Virtual Thread: true)에서 예외가 발생했습니다.
java.lang.RuntimeException: ID가 123인 사용자를 찾을 수 없습니다.
 at com.example.UserService.lambda$processUser$0(UserService.java:15)
 at java.base/java.util.concurrent.FutureTask.run(FutureTask.java:264)
 at java.base/java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1144)
 at java.base/java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:641)
 at java.base/java.lang.VirtualThread.run(VirtualThread.java:494)

StackTrace:
com.example.UserService.lambda$processUser$0(UserService.java:15)
java.util.concurrent.FutureTask.run(FutureTask.java:264)
java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1144)
java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:641)
java.lang.VirtualThread.run(VirtualThread.java:494)
Thread: VirtualThread-17 (Virtual Thread: true)
```

# 가상 스레드 잘못 사용시 문제점 및 이슈:

## 1. 자원 고갈

문제: 가상 스레드는 가벼우나 메모리 및 CPU 자원을 사용합니다. 적절히 관리하지 않으면 너무 많은 가상 스레드를 생성하면 자원 고갈이 발생할 수 있으며, 이로 인해 응용프로그램이 다운되거나 성능이 저하될 수 있습니다.


<div class="content-ad"></div>

예시: JVM을 과부하로 이끄는 것을 방지하려면 순차적으로 처리할 수 있는 작업에 대해 수백만 개의 가상 스레드를 생성하는 것은 필요하지 않습니다.

## 2. 적절하지 못한 블로킹 작업

문제: 가상 스레드는 블로킹 작업을 더 효율적으로 처리하기 위해 설계되었지만, 적절하지 않은 상황(예: 반복적인 루프나 핫 패스)에서 블로킹 작업에 지나치게 의존하면 가상 스레드라고 해도 성능이 저하될 수 있습니다.

예시: 크리티컬 섹션이나 루프 내에서 지속적으로 I/O 또는 동기화 원시 데이터에 대해 블로킹을 거는 것은 가상 스레드를 사용하더라도 경합과 성능 병목 현상을 유발할 수 있습니다.

<div class="content-ad"></div>

## 3. 잘못된 예외 처리

문제: 가상 스레드를 사용할 때 예외 처리가 복잡해질 수 있습니다, 특히 가상 스레드 작업 중에 예외가 발생할 경우입니다. 예외가 올바르게 전파되지 않거나 처리되지 않으면 무시될 수 있어 정상적인 실패로 이어질 수 있습니다.

예시: 가상 스레드에서 예외를 올바르게 처리하지 않으면 작업이 어떤 알림 없이 실패할 수 있어 데이터 손상이나 일관성 없는 상태로 이어질 수 있습니다.

## 4. CPU 바운드 작업에 적합하지 않은 사용법

<div class="content-ad"></div>

문제: 가상 스레드는 CPU 바운드 작업보다는 I/O 바운드 작업에 가장 유익합니다. CPU 바운드 작업에 가상 스레드를 사용하면 효율적인 CPU 활용이 어려울 수 있습니다.

예시: 계산 집약적 작업을 가상 스레드에 위임하면 성능을 향상시키는 대신 불필요한 문맥 전환과 오버헤드를 일으킬 수 있습니다.

## 5. 복잡한 디버깅 및 프로파일링

문제: 가상 스레드를 사용한 응용 프로그램의 디버깅 및 프로파일링은 전통적인 스레드보다 복잡할 수 있습니다. 도구들이 아직 가상 스레드를 완전히 지원하지 않을 수 있어 문제나 성능 문제를 진단하는 데 어려움을 겪을 수 있습니다.

<div class="content-ad"></div>

예시: 전통적인 쓰레드 덤프나 스택 트레이스는 가상 쓰레드와 관련된 문제를 명확하게 보여주지 않을 수 있어서 디버깅 중에 혼란을 야기할 수 있습니다.

# 그래서 몇 가지 권장 사항이 있습니다:

## 1. I/O 바운드 작업에 가상 쓰레드 사용하기

권장: 가상 쓰레드는 네트워크 요청 처리, 데이터베이스 작업 또는 파일 I/O와 같이 작업이 I/O 바운드인 시나리오에서 뛰어납니다. 이를 통해 기존 쓰레드 풀의 제약 사항을 고려하지 않고 동시 작업의 수를 확장할 수 있습니다.

<div class="content-ad"></div>

가상 쓰레드를 사용하여 웹 서버나 마이크로서비스 아키텍처에서 동시에 많은 수의 HTTP 요청을 처리하세요.

## 2. CPU 바운드 작업에 가상 스레드 사용하지 않기

권장 사항: CPU 바운드 작업의 경우, 플랫폼 스레드를 사용하거나 가상 스레드의 수를 신중하게 관리하는 것을 고려해보세요. 가상 스레드는 계산 집약적 작업에서 불필요한 오버헤드를 도입할 수 있습니다.

예시: 복잡한 계산을 수행하는 데이터 처리 파이프라인의 경우, 개별 작업마다 가상 스레드를 생성하는 대신 플랫폼 스레드와 함께 고정 크기의 스레드 풀을 사용하세요.

<div class="content-ad"></div>

## 3. Executor 서비스 활용하기

권장사항: Executors.newVirtualThreadPerTaskExecutor()를 사용하여 가상 스레드를 효율적으로 관리하는 Executor 서비스를 생성합니다. 이를 통해 가상 스레드에서 수행할 작업을 수동으로 관리하지 않고 제출할 수 있습니다.

예시: Spring Boot 애플리케이션에서 @Async로 주석 처리된 비동기 작업을 처리하는 데 이 Executor 서비스를 사용합니다.

## 4. 가상 스레드 범위 제한하기

<div class="content-ad"></div>

### 모범 사례: 가상 스레드는 작업이 일시적이거나 블로킹이 예상되는 경우에 사용해야 합니다. 무기한 실행되거나 무거운 계산을 포함하는 작업에 대해 가상 스레드를 생성하지 마세요.

예시: REST API에서 요청을 처리할 때 가상 스레드를 사용하되, 장기 실행 백그라운드 작업이나 밀집된 일괄 처리에는 사용하지 마세요.

## 5. 스레드 로컬 변수를 사용할 때 주의해 주세요.

모범 사례: 가상 스레드에서 스레드 로컬 변수에 의존하지 마세요. 스레드 재사용으로 인해 원하는 대로 동작하지 않을 수 있습니다. 스레드 로컬 저장소를 사용해야 하는 경우 주의해서 관리해 주세요.

<div class="content-ad"></div>

예시: 사용자 컨텍스트나 세션 데이터에 스레드 로컬 변수를 사용하는 대신, 필요한 메서드에 명시적으로 이 정보를 전달하세요.

## 6. 적절한 예외 처리

Best Practice: 가상 스레드 내의 예외가 제대로 처리되고 눈치채지 못하게 되지 않도록 보장하세요. 적절한 예외 처리 메커니즘과 로깅을 사용하여 가상 스레드의 오류를 잡아내세요.

예시: Spring Boot 애플리케이션에서 전역 예외 처리기를 구현하여 가상 스레드에서 실행 중인 작업에서 발생한 예외를 캐치하고 로깅하세요.

<div class="content-ad"></div>

## 7. 가상 스레드 모니터링 및 프로파일링

**최상의 방법:** 가상 스레드가 효과적으로 사용되고 있는지 확인하기 위해 정기적으로 응용 프로그램을 모니터링하고 프로파일링하세요. 가상 스레드를 지원하는 도구를 사용하여 성능을 분석하고 문제를 감지하세요.

**예시:** 가상 스레드와 호환되는 Java Flight Recorder (JFR) 또는 다른 프로파일링 도구를 사용하여 스레드 사용량을 모니터링하고 잠재적인 병목 현상을 식별하세요.

## 8. 블로킹 작업 최적화

<div class="content-ad"></div>

최적 사례: 가상 스레드를 사용할 때는 I/O, 데이터베이스 액세스와 같은 블로킹 작업이 최적화되었는지 확인하세요. 가상 스레드는 블로킹을 더 우아하게 처리할 수 있지만, 과도한 블로킹은 성능 저하로 이어질 수 있습니다.

예시: 데이터베이스 액세스 레이어에서는 쿼리가 최적화되어 블로킹 시간이 줄어들도록 보장하세요. 가상 스레드를 사용할 때에도 이를 실천해주세요.

## 9. 현실적인 환경에서 테스트

최적 사례: 가상 스레드가 부하를 겪는 환경에서 응용 프로그램을 테스트하여 실제 운영과 유사한 환경에서 가상 스레드의 동작을 관찰하세요. 이를 통해 사용자에게 영향을 미치기 전에 잠재적인 문제를 식별할 수 있습니다.

<div class="content-ad"></div>

예시: 가상 스레드를 사용하여 고 동시성을 모방하고 확장성 문제를 식별하도록 애플리케이션에 부하 테스트를 수행하세요.

## 10. 점진적 적용

최상의 방법: 애플리케이션에 가상 스레드를 점진적으로 도입하세요. 중요하지 않은 경로부터 시작하여 자신감을 얻고 그 영향을 이해하는 동안 사용범위를 확장하세요.

예시: 주요 요청 처리 작업에 채택하기 전에 비동기 처리나 백그라운드 작업에서 가상 스레드를 사용하여 시작하세요.

<div class="content-ad"></div>

다른 SpringBoot 3.3 좋은 기사들:

여기까지입니다.

읽어 주셔서 감사합니다! 좋아하셨거나 도움이 되었다면 박수 버튼을 클릭해주세요.
즐거운 코딩이 되길 바라며. 다음에 또 만나요 :)