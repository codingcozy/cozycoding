---
title: "React 컴포넌트와 TinyMCE, Firebase Firestore를 사용하여 포스트 HTML 글 작성하는 방법"
description: ""
coverImage: "/assets/img/2024-07-29-CreateaPostHTMLArticleReactComponentTinyMCEFirebaseFirestore_0.png"
date: 2024-07-29 13:45
ogImage: 
  url: /assets/img/2024-07-29-CreateaPostHTMLArticleReactComponentTinyMCEFirebaseFirestore_0.png
tag: Tech
originalTitle: "Create a Post HTML Article React Component TinyMCE  Firebase Firestore"
link: "https://medium.com/javascript-quantum/create-a-post-html-article-react-component-tinymce-firebase-firestore-e1904cde29a7"
isUpdated: true
---




알겠어요, 간단한 개념 편집기 포스트와 기사 작성 및 저장을 위한 간단한 UI를 만들었어요. 저는 www.quantumcompass.xyz 에서 무료로 배우는 엔카 enarn 백과사전를 제작 중이에요. 제 첫 번째 목표는 Firebase Firestore 데이터베이스에 3 가지 엘리멘트(개념에 대한)를 저장하는 거에요:
✅ 기사 제목
✅ 기사 HTML 설명
✅ 제목 슬러그 (고유 식별자로)

<img src="/assets/img/2024-07-29-CreateaPostHTMLArticleReactComponentTinyMCEFirebaseFirestore_0.png" />

<div class="content-ad"></div>

당신이 블로그나 CMS Firebase-Firestore 기반을 구축하고 싶다면 전체 코드를 공유하겠습니다.

먼저, 작은 클라우드 계정을 생성해주세요.

## TinyMCE란 무엇인가요?

- 풍부한 텍스트 편집기: TinyMCE는 강력한 JavaScript 기반 WYSIWYG 편집기입니다.
- 사용자 정의 가능: 다양한 플러그인과 구성 옵션을 제공합니다.
- 통합: 웹 애플리케이션 및 프레임워크와 쉽게 통합됩니다.

<div class="content-ad"></div>

## TinyMCE 사용 사례들

- 콘텐츠 관리 시스템(CMS): TinyMCE를 사용하여 풍부한 콘텐츠 편집을 수행합니다.
- 블로그 플랫폼: 블로그 게시물을 만들고 편집하기 위해 TinyMCE를 사용합니다.
- 폼 입력: 웹 폼의 텍스트 영역을 개선하여 사용자 입력을 원활하게 합니다.