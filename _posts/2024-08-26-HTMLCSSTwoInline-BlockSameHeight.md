---
title: "HTML CSS 두 개의 인라인 블록을 동일한 높이로 맞추는 방법"
description: ""
coverImage: "/assets/img/2024-08-26-HTMLCSSTwoInline-BlockSameHeight_0.png"
date: 2024-08-26 17:43
ogImage: 
  url: /assets/img/2024-08-26-HTMLCSSTwoInline-BlockSameHeight_0.png
tag: Tech
originalTitle: "HTML CSS Two Inline-Block Same Height"
link: "https://medium.com/gitconnected/html-css-two-inline-block-same-height-59dee2e50e2f"
isUpdated: true
updatedAt: 1724743549748
---


<img src="/assets/img/2024-08-26-HTMLCSSTwoInline-BlockSameHeight_0.png" />

제목이 상당히 명확한 것 같습니다만, 명확하게 설명하기 위해 여기서 이 기사에서 할 것은 display:inline-block 유형의 두(또는 그 이상) 요소의 높이를 동일하게 만드는 것입니다.

아마도 우리는 간단히 height:100%를 설정하면 된다고 생각할 수 있지만, 불행하게도 그렇게 하는 것은 동작하지 않을 것입니다. 아래는 이것을 보여주는 간단한 스니펫입니다.

```js
/* css */
.container {
  display: block;
}

.col {
  display: inline-block;
  background: red;
  padding: 4px;
  height: 100%;
  vertical-align: middle;
}


<!-- HTML -->
<h3> Height: 100% </h3>
<div class="container">
  <div class="col">
    some contents<br>
    some contents<br>
    some contents<br>
    some contents
  </div>

  <div class="col">
    one line
  </div>
</div>
```

<div class="content-ad"></div>

그리고 여기가 결과입니다.

![결과 이미지](/assets/img/2024-08-26-HTMLCSSTwoInline-BlockSameHeight_1.png)

온라인 솔루션 중에는 flex나 float를 사용하는 것을 제안하는 것이 많지만, 불행하게도 저는 내 것을 inline-block으로 유지해야 합니다.

자바스크립트를 사용하지 않고 여러 inline-blocks의 높이를 동일하게 맞출 수 있는 방법을 살펴보겠습니다!

<div class="content-ad"></div>

이 기사에서 사용하는 모든 코드 스니펫을 내 Codepen에서 찾을 수 있어요.

# 기본 접근 방법

세 가지 단계. 간단한 해결책.

- 거의 무한한 패딩을 추가하세요.
- 마이너스 마진을 추가하세요 (패딩을 균형을 맞추기 위해).
- 오버플로우를 숨기세요.

<div class="content-ad"></div>

세 가지 다른 사용 사례를 살펴볼 것입니다: 맨 위, 중간, 그리고 맨 아래 정렬.

# 맨 위 정렬

```js
/* css */
.container {
  display: block;
  overflow: hidden;
}

.colAlignTop {
  display: inline-block;
  background: red;
  padding: 4px;
  padding-bottom: 1000%;
  margin-bottom: -1000%;
  vertical-align: top;
}

<!-- HTML -->
<h3> 높이 일치: 맨 위 정렬 </h3>
<div class="container">
  <div class="colAlignTop">
    내용<br>
    내용<br>
    내용<br>
    내용
  </div>

  <div class="colAlignTop">
    한 줄
  </div>
</div>
```

여기서 패딩과 마진에 1000% 및 -1000%를 사용했지만, 사용 사례에 따라 더 작은 값으로 충분할 수도 있고, 더 큰 값이 필요할 수도 있습니다.

<div class="content-ad"></div>

아래는 결과입니다.

![이미지](/assets/img/2024-08-26-HTMLCSSTwoInline-BlockSameHeight_2.png)

# 가운데 정렬

똑같은 아이디어에요.

<div class="content-ad"></div>

```js
/* css */
.container {
  display: block;
  overflow: hidden;
}

.colAlignMiddle {
  display: inline-block;
  background: red;
  padding: 4px;
  padding-bottom: 1000%;
  margin-bottom: -1000%;
  padding-top: 1000%;
  margin-top: -1000%;
  vertical-align: middle;
}


# HTML #
```

![HTML and CSS](/assets/img/2024-08-26-HTMLCSSTwoInline-BlockSameHeight_3.png)

# Align Bottom

```js
/* css */
.container {
  display: block;
  overflow: hidden;
}

.colAlignBottom {
  display: inline-block;
  background: red;
  padding: 4px;
  padding-top: 1000%;
  margin-top: -1000%;
  vertical-align: bottom;
}


# HTML #
```

<div class="content-ad"></div>


![image](/assets/img/2024-08-26-HTMLCSSTwoInline-BlockSameHeight_4.png)

# A Little Bonus

만약 여러 개의 inline-block이 한 줄에 유지되도록 강제하고 싶다면, 다음과 같이 컨테이너에 text-wrap: nowrap를 추가하면 됩니다.

```js
.container {
  display: block;
  overflow: hidden;
  text-wrap: nowrap;
}
```

<div class="content-ad"></div>

읽어 주셔서 감사합니다!

오늘은 여기까지!

즐거운 정렬하세요!