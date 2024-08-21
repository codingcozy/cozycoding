---
title: "부트스트랩 기초  브레이크포인트 이해하기 7편"
description: ""
coverImage: "/assets/img/2024-07-20-BootstrapFundamentalsBreakpoints7_0.png"
date: 2024-07-20 11:21
ogImage: 
  url: /assets/img/2024-07-20-BootstrapFundamentalsBreakpoints7_0.png
tag: Tech
originalTitle: "Bootstrap Fundamentals Breakpoints 7"
link: "https://medium.com/@tomas-svojanovsky/bootstrap-fundamentals-breakpoints-7-44df7644a4ed"
isUpdated: true
---





![Image](/assets/img/2024-07-20-BootstrapFundamentalsBreakpoints7_0.png)

때로는 수동으로 브레이크포인트를 사용하고 뷰포트 폭에 따라 레이아웃/스타일을 조정해야 할 때가 있습니다.

일관성을 유지하기 위해 Bootstrap 브레이크포인트를 따라야 합니다.

```js
<style>
    /*  X-Small devices (portrait phones, less than 576px) */
    body {
        background: white;
    }

    /* `sm` applies to small devices (portrait phones, more than 575px) */
    @media (min-width: 576px) {
        body {
          background: red;
        }
    }

    /* `md` applies to medium devices (tablets, more than 767px) */
    @media (min-width: 768px) {
        body {
          background: blue;
        }
    }

    /* `lg` applies to large devices (desktops, more than 991px) */
    @media (min-width: 992px) {
        body {
          background: green;
        }
    }

    /* `xl` applies to extra large devices (large desktops, more than 1199px) */
    @media (min-width: 1200px) {
        body {
          background: yellow;
        }
    }

    /* `xxl` applies to extra extra large devices (larger desktops, more than 1399px) */
    @media (min-width: 1400px) {
        body {
          background: purple;
        }
    }
</style>
```

<div class="content-ad"></div>

뷰포트를 작게 만들어 보세요. 그러면 색깔이 어떻게 바뀌는지 확인할 수 있어요.

![image1](https://miro.medium.com/v2/resize:fit:1400/1*BkVAopGCecwZQRO_Q2Jp9g.gif)

![image2](https://miro.medium.com/v2/resize:fit:400/0*jYd4F4QxuNXGzOR4.gif)