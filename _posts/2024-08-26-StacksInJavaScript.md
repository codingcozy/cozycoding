---
title: "자바스크립트에서 사용되는 스택 자료구조와 메서드들"
description: ""
coverImage: "/assets/img/2024-08-26-StacksInJavaScript_0.png"
date: 2024-08-26 18:19
ogImage: 
  url: /assets/img/2024-08-26-StacksInJavaScript_0.png
tag: Tech
originalTitle: "Stacks In JavaScript"
link: "https://medium.com/@talha_nuh/stacks-in-javascript-d0c2658af32b"
isUpdated: false
---


<img src="/assets/img/2024-08-26-StacksInJavaScript_0.png" />

```js
class Stack (){  
    constructor(){
    this.items = [];
    }
}
```

- 먼저 'Stack'이라는 새 클래스를 정의했습니다.
- this.items
- 여기에서 스택 데이터 구조를 만들었습니다.
- 주된 규칙은 "나중에 추가한 것이 먼저 빠져나간다(LAST IN, FIRST OUT)"입니다.
- 이것은 스택에 추가된 마지막 항목이 가장 먼저 제거된다는 것을 의미합니다.

# push 메소드

<div class="content-ad"></div>

```js
class Stack{
   ...
   
   push(element) {
    this.items.push(element);
   }
}
```

'push' 작업은 스택의 맨 위에 새 항목을 추가합니다. 
괄호 안에 element, value 등을 작성하는 것을 기억하세요.

## isEmpty 메서드

```js
class Stack{
   ...
   
   isEmpty() {
       return this.items.length === 0;
    }
}
```

<div class="content-ad"></div>

isEmpty 액션은 스택에 항목이 있는지 확인합니다.

# pop 메서드

```js
class Stack{
   ...
   
   pop() {
    if (this.isEmpty()) {
         return "스택이 비어 있습니다";
       }
       return this.items.pop();
   }
}
```

pop 액션은 스택의 맨 위에 있는 항목을 제거합니다. pop 메서드를 수행할 때 스택이 비어 있는지 여부를 확인해야 합니다. 왜냐하면 스택이 비어 있을 수도 있기 때문입니다.

<div class="content-ad"></div>

# peek 메소드

```js
class Stack{
   ...
   
   peek() {
    if (this.isEmpty()) {
         return "스택이 비어 있습니다";
       }
       return this.items[this.items.length-1];
   }
}
```

peek 작업은 스택의 맨 위 항목을 제거하지 않고도 볼 수 있게 합니다.

# size 메소드

<div class="content-ad"></div>

```js
class Stack{
   ...
   
   size() {
    return this.items.length;
   }
}
```

스택에 있는 항목의 수를 반환합니다.

# clear 메소드

```js
class Stack{
   ...
   
   clear() {
    this.items = [];
   }
}
```

<div class="content-ad"></div>

스택에서 모든 요소를 제거하는 데 도움이 됩니다.

모든 코드를 함께 사용하실 수 있습니다.

```js
class Stack {
  constructor() {
    this.items = [];
  }

  push(element) {
    this.items.push(element);
  }

  isEmpty() {
    return this.items.length === 0;
  }

  pop() {
    if (this.isEmpty()) {
      return "스택이 비어있습니다";
    }
    return this.items.pop();
  }

  peek() {
    if (this.isEmpty()) {
      return "스택이 비어있습니다";
    } 
    return this.items[this.items.length - 1];
  }

  size() {
    return this.items.length;
  }
};

const stack = new Stack();    

stack.push(1);
stack.push(2);
stack.push(9);

console.log(stack.size());   // 3
console.log(stack.peek());   // 9
```

참고:

<div class="content-ad"></div>

스택의 내용을 확인하고 싶다면 확인할 메서드를 추가할 수 있어요.

```js
viewStack() {
  return this.items;
}

console.log(stack.viewStack());     //Output: [1, 2, 9]
```

읽어주셔서 감사합니다.