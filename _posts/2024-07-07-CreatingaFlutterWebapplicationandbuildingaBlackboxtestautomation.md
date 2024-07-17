---
title: "Flutter 웹 애플리케이션 만들기 및 블랙박스 테스트 자동화 구축 방법"
description: ""
coverImage: "/assets/img/2024-07-07-CreatingaFlutterWebapplicationandbuildingaBlackboxtestautomation_0.png"
date: 2024-07-07 19:37
ogImage: 
  url: /assets/img/2024-07-07-CreatingaFlutterWebapplicationandbuildingaBlackboxtestautomation_0.png
tag: Tech
originalTitle: "Creating a Flutter Web application and building a Blackbox test automation"
link: "https://medium.com/@pradappandiyan/creating-a-flutter-web-application-and-building-a-blackbox-test-automation-26c704b9a9b2"
---


<img src="/assets/img/2024-07-07-CreatingaFlutterWebapplicationandbuildingaBlackboxtestautomation_0.png" />

플러터(Flutter)는 하나의 코드베이스로 네이티브 컴파일된 모바일, 웹 및 데스크톱 애플리케이션을 구축할 수 있는 강력한 UI 툴킷입니다. 이 기사에서는 카운터를 위한 증가 및 감소 기능을 구현하여 기본 상태 관리를 보여주는 간단한 플러터 앱을 만들 것입니다.

# 단계별 안내

단계 1: 플러터 환경 설정

<div class="content-ad"></div>

시작하기 전에 귀하의 컴퓨터에 Flutter가 설치되어 있는지 확인해주세요. 만약 설치되어 있지 않다면 [공식 Flutter 설치 안내서](https://flutter.dev/docs/get-started/install)를 참고하실 수 있습니다.

단계 2: 새로운 Flutter 프로젝트 생성하기

터미널을 열고 다음 명령어를 실행하여 새로운 Flutter 프로젝트를 생성해주세요:

```bash
flutter create counter_app
```

<div class="content-ad"></div>

프로젝트 디렉토리로 이동해주세요:

```js
cd counter_app
```

단계 3: Flutter 코드 작성

`lib/main.dart` 파일의 내용을 다음 코드로 교체해주세요

<div class="content-ad"></div>

```js
import 'package:flutter/material.dart';
import 'package:flutter/rendering.dart';

void main() {
  runApp(const MyApp());
  RendererBinding.instance.ensureSemantics();
}

class MyApp extends StatelessWidget {
  const…
```