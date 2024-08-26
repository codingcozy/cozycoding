---
title: "스파르타 코딩 클럽 1주차 배운 것들과 과제 소개 "
description: ""
coverImage: "/assets/img/2024-08-26-SpartaCodingClubWeek-1TILAssignment_0.png"
date: 2024-08-26 17:40
ogImage: 
  url: /assets/img/2024-08-26-SpartaCodingClubWeek-1TILAssignment_0.png
tag: Tech
originalTitle: "Sparta Coding Club Week-1 TIL , Assignment "
link: "https://medium.com/@sumitchakrabortyin/sparta-coding-club-week-1-til-assignment-595ba16640fe"
isUpdated: false
---


![표](/assets/img/2024-08-26-SpartaCodingClubWeek-1TILAssignment_0.png)

스파르타 코딩 클럽에서의 여정이 시작되었는데 정말 흥미롭고 도전적이에요. 웹 개발에 항상 관심이 많았던 저로서는 첫 주는 새로운 개념을 배우고 웹을 구동하는 도구들을 실제로 다뤄보는 과정이었어요. 이번 주에는 HTML, CSS 및 부트스트랩의 기초에 초점을 맞춰 흥미진진한 코딩 모험을 위한 기반을 다졌어요.

# 건축의 기본 요소 이해하기: HTML, CSS 및 부트스트랩:

배운 내용을 살펴보기 전에, 이 세 가지 주요 구성 요소를 살펴보죠:

<div class="content-ad"></div>

Markdown 문법:

# HTML (HyperText Markup Language):

CSS (Cascading Style Sheets):

# 나의 첫 번째 프로젝트: 로그인 페이지 만들기

HTML과 CSS에 대한 기본적인 이해를 바탕으로 나는 내 첫 프로젝트로 간단한 로그인 페이지를 만들어보았어요. 배운 내용을 적용하는 완벽한 방법이었죠. 페이지 구조는 HTML을 사용하여 만들고, CSS를 사용하여 스타일을 입혔답니다.

<div class="content-ad"></div>

마크다운 형식의 표:

| 내용                  | 설명                                                                                                               |
|-----------------------|--------------------------------------------------------------------------------------------------------------------|
| HTML 구조            | 기본 HTML 구조를 만들었습니다. 이에는 사용자 이름과 암호를 입력할 수 있는 간단한 양식과 제출 버튼이 포함되어 있습니다.  |
| CSS 스타일링         | 로그인 페이지를 시각적으로 매력적으로 만들기 위해 CSS 스타일을 적용했습니다. 색상, 글꼴, 간격을 조정하여 양식이 깔끔하고 사용자 친화적으로 보이도록 했습니다. |

<div class="content-ad"></div>

# 프로젝트 쇼케이스:

여기는 로그인 페이지 프로젝트의 최종 결과물을 보여주기 위한 스크린샷을 포함할 공간입니다.

# 코드 스니펫:

아래는 저의 로그인 페이지를 만들기 위해 사용한 HTML과 CSS 코드 일부입니다:

<div class="content-ad"></div>

```js
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Login Page!</title>

    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link
        href="https://fonts.googleapis.com/css2?family=New+Amsterdam&family=Roboto+Mono:ital,wght@0,100..700;1,100..700&display=swap"
        rel="stylesheet">
    <link rel="stylesheet" type="text/css" href="loginstyle.css">
</head>

<body>

    <div class="wrap">
        <div class="mycontainer">

            <h1>Login Page</h1>
            <h4>Type your ID and PW</h4>

        </div>

        <div class="mycontainer1">

            <p>ID: <input type="text" /></p>
            <p>PW: <input type="text" /></p>

        </div>

        <div class="mybtn">

            <h4>Login</h4>

        </div>

    </div>

</body>
</html>
```

```js
* {
    font-family: "Roboto Mono", monospace;
    font-optical-sizing: auto;

}

.mycontainer {

    color: white;
    width: 300px;
    height: 200px;

    background-image: url('https://www.ancient-origins.net/sites/default/files/field/image/Agesilaus-II-cover.jpg');

    background-position: center;
    background-size: cover;

    border-radius: 10px;

    text-align: center;

    padding-top: 40px;


}

.wrap {

    margin: auto;
    width: 300px;
}

.mycontainer1 {
    text-align: center;
}

.mybtn {
    color: white;
    width: 300px;
    height: 50px;
    background-color: brown;
    border-radius: 10px;
    text-align: center;
    padding-top: 1px;
}
```

#부트스트랩 탐색: 웹 페이지 반응형 만들기


<div class="content-ad"></div>

부트스트랩에 대해 배운 내용

부트스트랩은 반응형 및 모바일 최적화 웹사이트를 빠르고 효율적으로 만들 수 있는 툴킷입니다. 그리드, 내비게이션 바, 버튼 등 CSS 및 JavaScript 구성 요소의 컬렉션을 제공하며, 이 모든 것은 쉽게 사용하고 맞춤화할 수 있도록 설계되어 있습니다. 부트스트랩의 진정한 매력은 그리드 시스템과 반응형 유틸리티에 있으며, 이를 통해 웹 페이지가 스마트폰, 태블릿 또는 데스크톱과 같은 모든 기기에서 멋지게 표시되도록 도와줍니다.

# 내 프로젝트: 인기 영화 목록과 추가 기능

부트스트랩 스킬을 테스트해보기 위해 가장 많이 시청한 영화를 나열할 수 있는 프로젝트를 만들었습니다. 이 프로젝트에는 영화 제목뿐만 아니라 URL, 평점 및 의견과 같은 내용까지 추가할 수 있는 게시물 상자 기능이 포함되어 있습니다. 이 기능은 완전히 반응형으로 설계되어 어떤 기기에서도 원활한 사용자 경험을 제공합니다.

<div class="content-ad"></div>

프로젝트 쇼케이스

프로젝트의 스크린샷을 여기에 포함하였습니다. 화면 크기에 따라 어떻게 폼과 영화 항목이 보이는지 강조하고 사용자가 쉽게 즐겨찾는 영화를 추가할 수 있는 방법을 보여줍니다.

![이미지1](/assets/img/2024-08-26-SpartaCodingClubWeek-1TILAssignment_1.png)

![이미지2](/assets/img/2024-08-26-SpartaCodingClubWeek-1TILAssignment_2.png)

<div class="content-ad"></div>

코드 스니펫

아래는 프로젝트를 만드는 데 사용한 HTML과 Bootstrap 코드의 스니펫입니다:

```js
<!doctype html>
<html lang="en">

<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/css/bootstrap.min.css" rel="stylesheet"
        integrity="sha384-EVSTQN3/azprG1Anm3QDgpJLIm9Nao0Yz1ztcQTwFspd3yD65VohhpuuCOmLASjC" crossorigin="anonymous">
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/js/bootstrap.bundle.min.js"
        integrity="sha384-MrcW6ZMFYlzcLA8Nl+NtUVF0sA7MsXsP1UyJoMp4YLEuNSfAP+JcXn/tWtIaxVXM"
        crossorigin="anonymous"></script>

    <title>최다 시청 영화</title>

    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Moderustic:wght@300..800&display=swap" rel="stylesheet">

    <style>
        * {
            font-family: "Moderustic", sans-serif;
            font-optical-sizing: auto;
            font-weight: 600;
            font-style: normal;
        }

        .mytitle {
            width: 100%;
            height: 250px;
            background-image: linear-gradient(0deg, rgba(0, 0, 0, 0.5), rgba(0, 0, 0, 0.5)), url('https://movie-phinf.pstatic.net/20210715_95/1626338192428gTnJl_JPEG/movie_image.jpg');
            background-position: center;
            background-size: cover;
            color: white;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
        }

        .mytitle>button {
            width: 200px;
            height: 50px;
            background-color: transparent;
            color: white;
            border-radius: 50px;
            border: 1px solid white;
            margin-top: 10px;
        }

        .mytitle>button:hover {
            border: 2px solid white;
        }

        .mycards {
            margin: 20px auto 0px auto;
            width: 95%;
            max-width: 1200px;
        }

        .mycomment {
            color: gray;
        }

        .mycontainer {
            box-shadow: 0px 0px 3px 0px gray;
            padding: 20px;
            margin: auto;
            width: 95%;
            max-width: 500px;
        }

        .mybutton {
            text-align: center;
        }

        .mybox {
            padding-top: 20px;
        }
    </style>
</head>

<body>

    <div class="mytitle">
        <h1>영화 컬렉션</h1>
        <button>새 영화 저장</button>
    </div>

    <div class="mybox">
        <div class="mycontainer">
            <div class="mb-3">
                <!-- <label for="exampleFormControlInput1" class="form-label">Email address</label> -->
                <input type="url" class="form-control" id="exampleFormControlInput1" placeholder="영화 URL">
            </div>
            <div class="input-group input-group-sm mb-3">
                <span class="input-group-text" id="inputGroup-sizing-sm">별점</span>
                <select class="form-select" aria-label="Default select example">
                    <option selected>-- 평점 선택 --</option>
                    <option value="1">🤩</option>
                    <option value="2">🤩🤩</option>
                    <option value="3">🤩🤩🤩</option>
                    <option value="4">🤩🤩🤩🤩</option>
                    <option value="5">🤩🤩🤩🤩🤩</option>
                </select>
            </div>
            <div class="mb-3">
                <!-- <label for="exampleFormControlTextarea1" class="form-label">Example textarea</label> -->
                <textarea class="form-control" id="exampleFormControlTextarea1" rows="3"
                    placeholder="코멘트"></textarea>
            </div>
            <div class="mybutton">
                <button type="button" class="btn btn-outline-dark">저장</button>
                <button type="button" class="btn btn-outline-dark">닫기</button>
            </div>
        </div>
    </div>

    <div class="mycards">
        <div class="row row-cols-1 row-cols-md-4 g-4">
            <div class="col">
                <div class="card h-100">
                    <img src="https://m.media-amazon.com/images/M/MV5BNDYxNjQyMjAtNTdiOS00NGYwLWFmNTAtNThmYjU5ZGI2YTI1XkEyXkFqcGdeQXVyMTMxODk2OTU@._V1_.jpg"
                        class="card-img-top" alt="...">
                    <div class="card-body">
                        <h5 class="card-title">어벤져스</h5>
                        <p class="card-text">지구 최강의 영웅들이 함께 모여 팀으로 싸워야만 사악한 로키와 그의 외계군단이 인류를 노예로 만들지 못할 것이다.</p>
                        <p>🤩🤩🤩</p>
                        <p class="mycomment">결국 캠프하고 공식적인 행동에도 불구하고, 어벤져스는 극도로 즐거운 시간을 선사합니다.</p>
                    </div>
                </div>
            </div>
            (중략)
        </div>
    </div>

</body>

</html>
```

<div class="content-ad"></div>

스파르타 코딩 클럽에서의 첫 주는 웹 개발에 대한 훌륭한 소개였어요. HTML로 웹페이지를 구조화하고 CSS로 스타일을 입히고 Bootstrap으로 기능을 향상시키는 것을 배워서 튼튼한 기반을 쌓을 수 있었어요. 이러한 기술이 어떻게 발전하는지 계속해서 코딩 여정을 이어갈 때 어떤 모습으로 변모하는지 기대돼요!

웹 개발 세계를 더 깊이 파헤칠 때 더 많은 업데이트를 기대해 주세요.