---
title: "Flutter에서 REST API 데이터로 드롭다운 메뉴 만들기 방법 총정리"
description: ""
coverImage: "/assets/img/2024-07-09-LoadingDatafromRESTAPIintoDropdowninFlutter_0.png"
date: 2024-07-09 22:36
ogImage: 
  url: /assets/img/2024-07-09-LoadingDatafromRESTAPIintoDropdowninFlutter_0.png
tag: Tech
originalTitle: "Loading Data from REST API into Dropdown in Flutter"
link: "https://medium.com/@axiftaj/loading-data-from-rest-api-into-dropdown-in-flutter-9e788ee927d3"
---



<img src="/assets/img/2024-07-09-LoadingDatafromRESTAPIintoDropdowninFlutter_0.png" />

REST API에서 데이터를 드롭다운 메뉴로 로드하는 것은 플러터 앱 개발에서 흔한 작업입니다. 특히 동적 데이터 선택이 필요할 때 유용합니다. 이 프로세스에는 원격 서버에서 데이터를 가져와 구문 분석하여 검색한 데이터로 드롭다운 위젯을 채우는 과정이 포함됩니다. 또한 예외를 신중하게 처리하여 부드러운 사용자 경험을 제공하는 것이 중요합니다.

이 안내서에서는 예외를 처리하는 동안 플러터 어플리케이션에서 REST API로부터 데이터를 로드하여 드롭다운 메뉴에 표시하는 단계를 안내하겠습니다.

# 소스 코드:


<div class="content-ad"></div>

# 단계 1: 의존성 추가

먼저 Flutter 프로젝트를 생성하고 yaml 파일에 http 패키지를 추가하세요

```js
dependencies:
  flutter:
    sdk: flutter
  http: ^0.13.3
```

# 단계 2: 포스트 모델

<div class="content-ad"></div>

표 태그를 Markdown 형식으로 변경해 보겠습니다.

```js
class PostsModel {
  int? userId;
  int? id;
  String? title;
  String? body;

  PostsModel({this.userId, this.id, this.title, this.body});

  PostsModel.fromJson(Map<String, dynamic> json) {
    userId = json['userId'];
    id = json['id'];
    title = json['title'];
    body = json['body'];
  }

  Map<String, dynamic> toJson() {
    final Map<String, dynamic> data = new Map<String, dynamic>();
    data['userId'] = this.userId;
    data['id'] = this.id;
    data['title'] = this.title;
    data['body'] = this.body;
    return data;
  }
}
```

## 단계 3: API 호출

getPostApi()라는 Future 메서드를 정의하여 서버로 HTTP GET 요청을 보냅니다. 이 메서드는 데이터를 반환하고 해당 데이터를 위젯에 표시할 포스트 목록으로 반환할 것입니다.

<div class="content-ad"></div>

```js
Future<List<PostsModel>> getPostApi() async {

    try {
      final response = await http.get(Uri.parse('https://jsonplaceholder.typicode.com/posts'));
      final body = json.decode(response.body) as List;
      if (response.statusCode == 200) {

        return body.map((dynamic json) {
          final map = json as Map<String, dynamic>;
          return PostsModel(
            id: map['id'] as int,
            title: map['title'] as String,
            body: map['body'] as String,
          );
        }).toList();
      }
    } on SocketException {
      await Future.delayed(const Duration(milliseconds: 1800));
      throw Exception('인터넷 연결 없음');
    } on TimeoutException {
      throw Exception('');
    }
    throw Exception('데이터를 가져오는 중 에러 발생');
}
```

# 단계 4: 드롭다운 위젯

`newData`라는 변수는 드롭다운에서 선택한 값을 저장합니다. ID를 보여주고 있지만 원하는 값으로 변경해도 괜찮습니다.

`getPostApi()` 메소드는 Future를 반환하므로 FutureBuilder를 사용합니다.


<div class="content-ad"></div>

```js
FutureBuilder<List<PostsModel>>(
              future: getPostApi(),
              builder: (context, snapshot) {
                if (snapshot.hasData) {
                  return DropdownButton(
                    // 초기 값
                    value: newData,
                    hint: Text('값을 선택하세요'),
                    isExpanded: true,
                    // 아래 화살표 아이콘
                    icon: const Icon(Icons.keyboard_arrow_down),

                    // 아이템 목록 배열
                    items: snapshot.data!.map((item) {
                      return DropdownMenuItem(
                        value: item.id,
                        child: Text(item.id.toString()),
                      );
                    }).toList(),
                    onChanged: (value) {
                      newData = value ;
                      setState(() {

                      });
                    },

                  );
                } else {
                  return Center(child: const CircularProgressIndicator());
                }
              },
```

# 완성된 소스 코드:

```js
import 'dart:async';
import 'dart:convert';
import 'dart:io';

import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;
import '../Models/posts_model.dart';

class DropDownApi extends StatefulWidget {
  const DropDownApi({Key? key}) : super(key: key);

  @override
  State<DropDownApi> createState() => _DropDownApiState();
}

class _DropDownApiState extends State<DropDownApi> {




  Future<List<PostsModel>> getPostApi ()async{

    try {
      final response = await http.get(Uri.parse('https://jsonplaceholder.typicode.com/posts')) ;
      final body = json.decode(response.body) as List;
      if(response.statusCode == 200) {

        return  body.map((dynamic json) {
          final map = json as Map<String, dynamic>;
          return  PostsModel(
            id: map['id'] as int,
            title: map['title'] as String,
            body: map['body'] as String,
          );
        }).toList();
      }
    } on SocketException {
      await Future.delayed(const Duration(milliseconds: 1800));
      throw Exception('인터넷 연결 없음');
    } on TimeoutException {
      throw Exception('');
    }
    throw Exception('데이터 불러오기 오류');

  }

  var newData ;
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Dropdown Api'),
      ),
      body: Padding(
        padding: const EdgeInsets.symmetric(horizontal: 20),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            FutureBuilder<List<PostsModel>>(
              future: getPostApi(),
              builder: (context, snapshot) {
                if (snapshot.hasData) {
                  return DropdownButton(
                    // 초기 값
                    value: newData,
                    hint: Text('값을 선택하세요'),
                    isExpanded: true,
                    // 아래 화살표 아이콘
                    icon: const Icon(Icons.keyboard_arrow_down),

                    // 아이템 목록 배열
                    items: snapshot.data!.map((item) {
                      return DropdownMenuItem(
                        value: item.id,
                        child: Text(item.id.toString()),
                      );
                    }).toList(),
                    onChanged: (value) {
                      newData = value ;
                      setState(() {

                      });
                    },

                  );
                } else {
                  return Center(child: const CircularProgressIndicator());
                }
              },
            )
          ],
        ),
      ),
    );
  }
}
```