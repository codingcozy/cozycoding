---
title: "Swift  Facade 패턴 정리"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-21 18:34
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Facade pattern  Swift  Simply Explained"
link: "https://medium.com/@algorhythm2411/facade-pattern-ios-simple-expl-9ea3b854104e"
isUpdated: true
updatedAt: 1724245926532
---


# 퍼사드 패턴

퍼사드 패턴은 복잡한 하위 시스템에 대한 간소화된 인터페이스를 제공하는 디자인 패턴입니다. 이는 하위 시스템 내의 인터페이스 집합에 대한 "전면" 또는 "게이트웨이" 역할을 하며 사용하기 쉽게 만들어줍니다.

## 작동 방식

- 복잡한 시스템: 여러 클래스와 메서드로 구성된 복잡한 시스템이 있다고 상상해보세요.
- 퍼사드: 이 복잡한 시스템과 상호 작용하는 간단하고 통합된 인터페이스를 제공하는 퍼사드 클래스를 생성합니다.
- 간소화: 하위 시스템의 사용자는 퍼사드와만 상호 작용하여 여러 클래스와 메서드를 직접 다루는 복잡성을 피할 수 있습니다.

<div class="content-ad"></div>

## 예시

조명, 에어컨 및 보안 시스템이 있는 간단한 홈 자동화 시스템을 상상해보세요.

복잡한 하위 시스템 클래스:

```js
class Lights {
    func turnOn() {
        print("조명이 켜졌습니다")
    }
    func turnOff() {
        print("조명이 꺼졌습니다")
    }
}
class AirConditioner {
    func turnOn() {
        print("에어컨이 켜졌습니다")
    }
    func turnOff() {
        print("에어컨이 꺼졌습니다")
    }
}
class SecuritySystem {
    func activate() {
        print("보안 시스템이 활성화되었습니다")
    }
    func deactivate() {
        print("보안 시스템이 비활성화되었습니다")
    }
}
```

<div class="content-ad"></div>

퍼사드 클래스

```js
class HomeAutomationFacade {
    private let lights: Lights
    private let airConditioner: AirConditioner
    private let securitySystem: SecuritySystem
    init(lights: Lights, airConditioner: AirConditioner, securitySystem: SecuritySystem) {
        self.lights = lights
        self.airConditioner = airConditioner
        self.securitySystem = securitySystem
    }
    func activateHome() {
        lights.turnOn()
        airConditioner.turnOn()
        securitySystem.activate()
        print("Home is activated")
    }
    func deactivateHome() {
        lights.turnOff()
        airConditioner.turnOff()
        securitySystem.deactivate()
        print("Home is deactivated")
    }
}
```

퍼사드 사용하기

```js
let lights = Lights()
let airConditioner = AirConditioner()
let securitySystem = SecuritySystem()
let homeAutomation = HomeAutomationFacade(lights: lights, airConditioner: airConditioner, securitySystem: securitySystem)
homeAutomation.activateHome()
// 출력:
// Lights are on
// Air conditioner is on
// Security system activated
// Home is activated
homeAutomation.deactivateHome()
// 출력:
// Lights are off
// Air conditioner is off
// Security system deactivated
// Home is deactivated
```

<div class="content-ad"></div>

## 요약

- Facade는 복잡한 시스템에 간단한 인터페이스를 제공합니다.
- 목적: 서브시스템과의 상호작용을 더 쉽고 직관적으로 만드는 것입니다.