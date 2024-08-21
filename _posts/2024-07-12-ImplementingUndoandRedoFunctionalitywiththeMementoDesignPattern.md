---
title: "메멘토 디자인 패턴으로 Undo와 Redo 기능 구현하는 방법"
description: ""
coverImage: "/assets/img/2024-07-12-ImplementingUndoandRedoFunctionalitywiththeMementoDesignPattern_0.png"
date: 2024-07-12 22:13
ogImage: 
  url: /assets/img/2024-07-12-ImplementingUndoandRedoFunctionalitywiththeMementoDesignPattern_0.png
tag: Tech
originalTitle: "Implementing Undo and Redo Functionality with the Memento Design Pattern"
link: "https://medium.com/dev-genius/implementing-undo-and-redo-functionality-with-the-memento-design-pattern-bca64a5281ea"
isUpdated: true
---





![image](/assets/img/2024-07-12-ImplementingUndoandRedoFunctionalitywiththeMementoDesignPattern_0.png)

제가 일하는 애플리케이션에서는 백엔드 팀이 실행 취소 및 다시 실행 기능을 구현하고 있어요.

처음에는 모든 변경 사항을 추적할 생각이었지만, 문제는 변경 사항이 앱 내에서 무엇이든 될 수 있다는 것이었어요. 현재 상태의 스냅샷을 찍어 두는 방식으로 돌아가거나 앞으로 이동할 수 있는 기능이 구현돼 있다는 걸 깨달았어요.

그 후에 Memento 디자인 패턴에 대해 알게 되었는데, "백엔드 팀이 구현하려는 게 뭔가 비슷하게 느껴진다"고 생각했어요.


<div class="content-ad"></div>

## 예시

은행 계좌의 변화를 추적해 봅시다. 우리는 은행 계좌에 돈을 추가하길 원하지만, 상태를 되돌리거나 다시 되돌릴 수 있어야 합니다.

## Memento

이 클래스는 계좌 잔액의 현재 상태를 저장할 것입니다. 이것이 우리의 스냅샷입니다.

<div class="content-ad"></div>

```js
class Memento {
    private _balance: number;

    constructor(balance: number) {
        this._balance = balance;
    }

    get balance(): number {
        return this._balance;
    }
}
```

## 은행 계좌

이것은 BankAccount 클래스입니다. 많은 기능이 포함되어 있는 것을 볼 수 있습니다.

한 번 자세히 살펴보죠.

<div class="content-ad"></div>

```js
class BankAccount {
    private _balance: number; // 은행 계좌의 현재 잔액
    private _changes: Memento[] = []; // 잔액 상태의 이력을 저장하는 목록
    private _current: number = 0; // 변경 목록에서 현재 상태를 추적하는 인덱스

    constructor(balance: number) {
        this._balance = balance;
        this._changes.push(new Memento(balance)); // 초기 잔액으로 초기화
    }

    // 계좌에 금액을 입금하고 새로운 상태를 저장하는 메서드
    deposit(amount: number): Memento {
        this._balance += amount;
        const memento = new Memento(this._balance);
        this._changes.push(memento); // 새 상태를 이력에 추가
        this._current++; // 현재 인덱스를 최신 상태로 이동

        return memento;
    }

...
```

## 현재를 왜 필요로 하는가?

_current는 _changes 목록에서 현재 위치를 추적합니다. 현재 어떤 상태(memento)에 있는지 알아내어 undo 및 redo 기능을 올바르게 구현할 수 있도록 합니다.

## Changes가 왜 필요한가?

<div class="content-ad"></div>

BankAccount 클래스의 _changes 목록은 잔액 상태(메멘토)의 이력을 저장하는 데 사용됩니다. 각 메멘토는 특정 시점의 잔액을 캡처하여 클래스가 상태 목록을 통해 전/후 행동 취소 기능을 구현할 수 있게 합니다.

## 이제 행동할 시간입니다

이제 1000으로 변경 사항을 효과적으로 되돌린 다음 한 걸음 앞으로 1500으로 이동할 수 있는 방법을 살펴보겠습니다.

```js
const bankAccount = new BankAccount(1000);
bankAccount.deposit(500);
bankAccount.deposit(250);
console.log(bankAccount.toString()); // 1750

bankAccount.undo();
console.log(bankAccount.toString()); // 1500

bankAccount.undo();
console.log(bankAccount.toString()); // 1000

bankAccount.redo();
console.log(bankAccount.toString()); // 1500
```

<div class="content-ad"></div>


![image](https://miro.medium.com/v2/resize:fit:400/0*mEYooH6f_e8XhEQz.gif)
