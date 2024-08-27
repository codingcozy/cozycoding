---
title: "Babel을 사용한 국경 없는 코딩"
description: ""
coverImage: "/assets/img/2024-08-26-BabelCodeWithoutBorders_0.png"
date: 2024-08-26 18:20
ogImage: 
  url: /assets/img/2024-08-26-BabelCodeWithoutBorders_0.png
tag: Tech
originalTitle: "Babel Code Without Borders"
link: "https://medium.com/towardsdev/babel-code-without-borders-22806a84d061"
isUpdated: true
updatedAt: 1724743985266
---


당신의 JS 번역기

안녕하세요 여러분,
제 블로그로 다시 오신 것을 환영합니다 🙏.
모두가 안전하고 건강한 상태에 있다면 좋겠네요 ️🫶.

![이미지](/assets/img/2024-08-26-BabelCodeWithoutBorders_0.png)

목차
- 소개
- 설치
- 구현
- 결론
- 참고

<div class="content-ad"></div>

# 소개

안녕하세요,
위 문장을 이해하는 사람이 모두가 아닐 것 같아요, 맞죠?
이해하지 못한다면, 누군가는 즉시 번역기를 사용하여 문장을 익숙한 언어로 변환합니다.
번역기와 마찬가지로 Babel은 현대의 JavaScript 코드를 더 오래된 JavaScript 버전으로 번역하여 다양한 브라우저와의 호환성을 확보하고 개발자들이 JavaScript의 최신 기능을 사용할 수 있도록 합니다.

그런데, 안녕하세요를 텔루구어로 향합니다 🙏.

# 설치

<div class="content-ad"></div>

Babel은 애플리케이션을 개발하거나 빌드할 때 사용하는 dev-dependency입니다.

```js
yarn add --dev @babel/core
            (또는)
npm install @babel/core --save-dev
```

@babel/core는 Babel의 핵심입니다. 이는 현대 JavaScript를 더 많은 곳에서 작동하는 더 오래된 버전으로 변환하는 주요 작업을 수행합니다. 여러분의 코드를 읽고 변경하고 업데이트된 코드를 작성하는 엔진과 같습니다.

```js
yarn add --dev @babel/cli
            (또는)
npm install @babel/cli --save-dev
```

<div class="content-ad"></div>

@babel/cli은 터미널에서 Babel을 사용할 수 있게 해줍니다. 거기서 직접 JavaScript 파일을 변환하는 명령을 입력할 수 있어요. 프로젝트를 빌드하거나 Babel을 수동으로 실행할 때 유용해요.

```js
yarn add --dev @babel/preset-env
               (또는)
npm install @babel/preset-env --save-dev
```

@babel/preset-env는 새로운 JavaScript 기능을 구형 버전으로 변환하는 Babel 플러그인의 모음입니다. 이러한 변환은 오래된 브라우저와 같은 각기 다른 환경에서 작동합니다. 지원하려는 브라우저에 따라 자동으로 조정됩니다.

```js
yarn add --dev @babel/core @babel/cli @babel/preset-env
                           (또는)
npm install --save-dev @babel/core @babel/cli @babel/preset-env
```

<div class="content-ad"></div>

자바스크립트 코드를 Babel이 어떻게 번역하는지 간단한 작동 예제로 이해해봅시다.

# 구현

이제, Babel을 응용 프로그램에 설치하는 방법을 알았습니다. 하지만

## 어떻게 그리고 어디에서 구성해야 하나요?

<div class="content-ad"></div>

Babel을 구성하는 몇 가지 방법이 있어요,

- babel.config.json (권장됨)
- .babelrc.json

```js
{
  "presets": [...],
  "plugins": [...]
}
```

- package.json에서도 구성할 수 있어요

<div class="content-ad"></div>

```json
{
  "name": "my-package",
  "version": "1.0.0",
  "babel": {
    "presets": [ ... ],
    "plugins": [ ... ]
  }
}
```

- 조건부 구성이 있는 경우 .js 파일을 사용하여 구성할 수도 있습니다. 예: babel.config.js, .babelrc.js

```js
module.exports = function(api) {
  const isTest = api.env('test');

  return {
    presets: [
      ["@babel/preset-env", {
        targets: isTest ? { node: 'current' } : { browsers: ["> 1%", "last 2 versions"] }
      }]
    ]
  };
}
```

이제 구성을 넣을 수 있는 위치를 알았어요.
코드가 어떻게 트랜스파일되는지 살펴보겠습니다.

<div class="content-ad"></div>

```js
class Person {
  name = 'Default Name';

  constructor(name) {
    if (name) {
      this.name = name;
    }
  }

  greet() {
    return `Hello, my name is ${this.name}!`;
  }
}

const person = new Person('Alice');
console.log(person.greet());

const defaultPerson = new Person();
console.log(defaultPerson.greet());
```

간단한 Person 클래스와 일부 문자열을 반환하는 greet 메서드가 있습니다.

![이미지](/assets/img/2024-08-26-BabelCodeWithoutBorders_1.png)

ES6 클래스는 대부분의 브라우저에서 널리 지원되지만,
Opera Mini 브라우저 사용자는 오류 및 호환성 문제에 직면할 수 있습니다.
여기서 Babel이 이러한 상황을 처리하는 데 도움이 됩니다.


<div class="content-ad"></div>

내 바벨 구성

```js
{
  "presets": [],
  "plugins": ["@babel/plugin-transform-class-properties"]
}
```

`@babel/plugin-transform-class-properties`는 자바스크립트 코드에서 클래스 속성을 사용할 수 있게 해주는 바벨 플러그인입니다.
클래스 속성은 자바스크립트의 구문 특징으로, 클래스 외부에서 직접 클래스에 속성을 정의할 수 있도록 해줍니다.

이 구성을 사용한 후에, 다음 명령어를 사용하면

```js
babel src --out-dir dist

// 이 명령은 src 폴더 파일을 변환하여 dist 폴더에 저장합니다.
```

<div class="content-ad"></div>

그래서, 코드를 트랜스 파일한 뒤에 출력된 내용이에요

```js
"use strict";

function _typeof(o) { "@babel/helpers - typeof"; return _typeof = "function" == typeof Symbol && "symbol" == typeof Symbol.iterator ? function (o) { return typeof o; } : function (o) { return o && "function" == typeof Symbol && o.constructor === Symbol && o !== Symbol.prototype ? "symbol" : typeof o; }, _typeof(o); }
function _classCallCheck(a, n) { if (!(a instanceof n)) throw new TypeError("Cannot call a class as a function"); }
function _defineProperties(e, r) { for (var t = 0; t < r.length; t++) { var o = r[t]; o.enumerable = o.enumerable || !1, o.configurable = !0, "value" in o && (o.writable = !0), Object.defineProperty(e, _toPropertyKey(o.key), o); } }
function _createClass(e, r, t) { return r && _defineProperties(e.prototype, r), t && _defineProperties(e, t), Object.defineProperty(e, "prototype", { writable: !1 }), e; }
function _defineProperty(e, r, t) { return (r = _toPropertyKey(r)) in e ? Object.defineProperty(e, r, { value: t, enumerable: !0, configurable: !0, writable: !0 }) : e[r] = t, e; }
function _toPropertyKey(t) { var i = _toPrimitive(t, "string"); return "symbol" == _typeof(i) ? i : i + ""; }
function _toPrimitive(t, r) { if ("object" != _typeof(t) || !t) return t; var e = t[Symbol.toPrimitive]; if (void 0 !== e) { var i = e.call(t, r || "default"); if ("object" != _typeof(i)) return i; throw new TypeError("@@toPrimitive must return a primitive value."); } return ("string" === r ? String : Number)(t); }
var Person = /*#__PURE__*/function () {
  function Person(name) {
    _classCallCheck(this, Person);
    _defineProperty(this, "name", 'Default Name');
    if (name) {
      this.name = name;
    }
  }
  return _createClass(Person, [{
    key: "greet",
    value: function greet() {
      return "Hello, my name is ".concat(this.name, "!");
    }
  }]);
}();
var person = new Person('Alice');
console.log(person.greet());
var defaultPerson = new Person();
console.log(defaultPerson.greet());
```

지금 이 코드는 오페라 미니를 포함한 모든 브라우저에서 지원돼요.

잠깐만 기다려주세요!
프리셋을 사용하여 이 문제를 해결할 수도 있어요

<div class="content-ad"></div>

Yeonsogdo gidoeneunnal год글arago mareul holro byeongyeonghayeo Jameunikeul d l아urt나세요.

geureohdago 하얍요??

<div class="content-ad"></div>

@babel/preset-env은 자주 사용되는 플러그인 모음이에요. 이 preset에는 @babel/plugin-transform-class-properties도 포함되어 있어요.

```js
// @babel/preset-env 7.21.5에는 아래 플러그인들이 포함되어 있어요


"@babel/compat-data": "^7.21.5",
"@babel/helper-compilation-targets": "^7.21.5",
"@babel/helper-plugin-utils": "^7.21.5",
"@babel/helper-validator-option": "^7.21.0",
"@babel/plugin-bugfix-safari-id-destructuring-collision-in-function-expression": "^7.18.6",
...
```

위 목록은 업그레이드마다 다를 수 있어요.
참고: 새로운 플러그인을 추가할 때는 node_modules에서 이 목록을 확인하고 preset-env에 포함되어 있지 않다면 설치하고 Babel 구성의 플러그인 배열에 추가하세요.

# 결론

<div class="content-ad"></div>

Babel은 호환성 문제를 걱정하지 않고 현대적인 JavaScript 코드를 작성하는 데 도움을 주는 도구입니다. 코드를 변환하여 다양한 환경과 브라우저에서 작동하도록 합니다. Babel을 사용하면 JavaScript의 최신 기능을 활용하고 최신 상태를 유지할 수 있습니다. 이는 청결하고 미래지향적인 코드를 작성하고자 하는 모든 개발자에게 필수 도구입니다.

제 다음 블로그에서 SWC (Super Web Compiler)에 대해 알아봅시다. 이 도구는 Babel보다 빠르다고 합니다.

# 참고

여기까지 입니다. 이번 짧은 블로그는 여기까지입니다.
모두가 이 블로그를 좋아했으면 좋겠습니다.
그러면 박수를 👏 하고, 친구와 공유해보세요.

<div class="content-ad"></div>

프론트엔드에 대한 더 흥미로운 콘텐츠를 보려면 저를 팔로우해주세요 🎯.

여러분 모두, 정말 감사합니다 🙏.
다음에 또 만나요,
즐거운 학습 되세요 📖✍️.