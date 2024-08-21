---
title: "웹 개발에서 로컬 스토리지 완벽 가이드"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-07-27 13:43
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Mastering Local Storage in Web Development"
link: "https://dev.to/code_passion/mastering-local-storage-in-web-development-fl5"
isUpdated: true
---




로컬 스토리지는 개발자가 사용자의 브라우저 내에 데이터를 저장할 수 있는 유용한 웹 개발 도구입니다. 이 글에서는 로컬 스토리지의 다양한 기능을 살펴보고, 초보자를 위한 예제부터 더 복잡한 기술까지 다룰 것입니다. 이 가이드를 마칠 때쯤에는 웹 애플리케이션에서 로컬 스토리지를 성공적으로 활용하는 방법에 대한 기본적인 이해를 갖게 될 것입니다.

로컬 스토리지 초보자 레벨 예제

1. 사용자 환경 설정 저장하기:

```js
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>User Preferences</title>
</head>

<body>
    <label for="theme">테마 선택:</label>
    <select id="theme">
        <option value="light">라이트</option>
        <option value="dark">다크</option>
        <option value="medium">중간</option>
    </select>
    <button onclick="savePreference()">설정 저장</button>

    <script>
        function savePreference() {
            const selectedTheme = document.getElementById('theme').value;
            localStorage.setItem('theme', selectedTheme);
            console.log('설정 저장됨:', selectedTheme);
            alert('설정이 저장되었습니다! ' + " " + selectedTheme);
        }
    </script>
</body>

</html>
```

<div class="content-ad"></div>

사용자가 테마를 선택하고 "설정 저장" 버튼을 클릭하면, 이 코드는 콘솔에 테마를 기록합니다. 이 로그를 읽으려면 브라우저의 개발자 도구를 열어야 합니다 (일반적으로 F12를 누르거나 페이지를 마우스 오른쪽 버튼으로 클릭한 다음 "검사"를 선택합니다) 그리고 콘솔 탭으로 이동하세요.(더 읽기)

2. 사용자 입력 기억:

```js
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>사용자 입력 기억</title>
</head>

<body>
    <input type="text" id="userInput">
    <button onclick="saveInput()">입력 저장</button>

    <script>
        function saveInput() {
            const userInput = document.getElementById('userInput').value;
            localStorage.setItem('savedInput', userInput);
            console.log(userInput + " " + '저장되었습니다!');
            alert('입력이 저장되었습니다!');
        }
    </script>
</body>

</html>
```

이 HTML 예제는 사용자가 텍스트를 입력란에 입력하고 이를 브라우저의 로컬 저장소에 저장할 수 있는 간단한 웹 페이지를 생성합니다.

<div class="content-ad"></div>

사용자가 입력 상자에 텍스트를 입력하고 "저장" 버튼을 클릭하면 해당 텍스트가 브라우저의 로컬 저장소에 저장됩니다. 이는 사용자가 웹사이트를 새로 고치거나 브라우저를 닫고 다시 열어도 저장된 입력이 유지된다는 것을 의미합니다. 콘솔 로그와 경고창은 사용자에게 입력이 성공적으로 저장되었음을 알립니다. (계속 읽기)

3. 쇼핑 카트 지속성:

```js
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>쇼핑 카트</title>
</head>

<body>
    <h1>쇼핑 카트</h1>
    <ul id="cartItems"></ul>

    <script>
        document.addEventListener('DOMContentLoaded', () => {
            const savedCart = JSON.parse(localStorage.getItem('cart')) || [];
            const cartItemsElement = document.getElementById('cartItems');

            savedCart.forEach(item => {
                const li = document.createElement('li');
                li.textContent = item;
                cartItemsElement.appendChild(li);
            });
            console.log('저장된 카트 아이템:', savedCart);
        });

        function addToCart(item) {
            const savedCart = JSON.parse(localStorage.getItem('cart')) || [];
            savedCart.push(item);
            localStorage.setItem('cart', JSON.stringify(savedCart));
            console.log(item, '를 카트에 추가했습니다');
            location.reload(); // 변경 사항을 반영하기 위해 페이지 새로고침
        }
    </script>

    <button onclick="addToCart('상품 1')">상품 1을 카트에 추가</button>
    <button onclick="addToCart('상품 2')">상품 2를 카트에 추가</button>
    <button onclick="addToCart('상품 3')">상품 3을 카트에 추가</button>
</body>

</html>
```

이 예제는 로컬 저장소를 사용하여 쇼핑 카트를 저장하는 방법을 보여줍니다. 카트에 추가된 항목은 'cart' 키 아래에 배열로 로컬 저장소에 저장됩니다. 페이지가 로드될 때, 저장된 카트 아이템이 로컬 저장소에서 가져와서 표시됩니다.

<div class="content-ad"></div>

"“Add Item X to Cart” 버튼 중 하나를 클릭하면 해당 아이템이 장바구니에 추가되며, 업데이트된 장바구니 내용이 콘솔에 표시됩니다. 이 로그를 확인하려면 브라우저의 개발자 도구를 열어야 합니다 (일반적으로 F12를 누르거나 페이지를 마우스 오른쪽 버튼으로 클릭하고 “Inspect”를 선택하면 됩니다) 그리고 콘솔 탭으로 이동하세요.

브라우저의 개발자 도구를 통해 로컬 저장소를 직접 볼 수도 있습니다. 대부분의 브라우저에서는 페이지를 마우스 오른쪽 버튼으로 클릭하고 “Inspect”를 선택하여 개발자 도구를 가져온 다음 “Application” 또는 “Storage” 탭으로 이동합니다. 그리고 거기서 “Local Storage” 섹션을 확장하여 웹사이트의 key-value 쌍을 확인할 수 있습니다. 이 예시에서는 “cart” 키에 저장된 카트 아이템을 나타내는 JSON 문자열이 포함되어 있습니다.
전체 기사 읽기 - 웹 개발에서 로컬 저장소 마스터하기: 초보자에서 전문가까지의 8가지 실용적 예제!

Json 배우기 - 여기를 클릭해주세요"