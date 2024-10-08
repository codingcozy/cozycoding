---
title: "Retrofit과 Flutter로 프로처럼 개발하는 방법"
description: ""
coverImage: "/assets/img/2024-06-22-RetrofitandFlutterlikeaPRO_0.png"
date: 2024-06-22 04:08
ogImage: 
  url: /assets/img/2024-06-22-RetrofitandFlutterlikeaPRO_0.png
tag: Tech
originalTitle: "Retrofit and Flutter like a PRO"
link: "https://medium.com/@ayalon.idan/retrofit-and-flutter-like-a-pro-e80b654545c1"
isUpdated: true
---




![이미지](/assets/img/2024-06-22-RetrofitandFlutterlikeaPRO_0.png)

데이터 처리 및 API 통합은 저희 모바일 개발자들에게 친숙한 주제입니다.

Flutter로 이를 처리하는 몇 가지 방법이 있습니다. 이 기사에서는 REST API를 처리하는 데 가장 효과적이라고 생각하는 방법을 보여드립니다. 이 멋진 기사를 보신 후에는 다른 것을 사용하지 않게 될 것입니다!

우리는 훌륭한 Retrofit 패키지에 대해 알아볼 것입니다. 이 패키지는 Dio 패키지의 유형 변환을 수행하며 (JSON을 Dart 객체로 변환하는 고통스러운 작업을 source_gen을 사용해 코드를 생성함으로써) 번거로움을 줄여줍니다. 시작해 봅시다.

<div class="content-ad"></div>

- 개요
- 설치
- Retrofit 구성 방법
- 사용 방법
- 모든 호출에 토큰 추가하는 방법
- freezed와 Retrofit 사용 방법
- 응답 캐시하는 방법
- 트래픽 로깅하는 방법
- 편리한 팁

# 개요

4년 전에 Flutter를 개발하기 시작했을 때 처음 검색했던 것은 REST API를 올바르게 처리하는 방법이었습니다.

안드로이드 개발자로서, Square의 Retrofit이 인터페이스와 주석을 사용하여 HTTP 요청을 설명하는 방법을 좋아했습니다. 다행히 누군가가 이 아이디어를 가져와 Flutter 세계에 구현했습니다.

<div class="content-ad"></div>

플러터 버전에서도 REST 작업을 설명하는 인터페이스(추상 클래스)를 사용하며 대화 형식을 자동으로 만들어줍니다.

안드로이드 버전은 기본 호출자로 OkHttp를 사용했습니다. 플러터의 경우 Dio를 사용하며, 이는 Dio를 사용하기 매우 쉽고 훌륭한 기능이 많이 포함되어 있어 기쁜 소식입니다. 또한 Retrofit과 함께 사용할 수 있는 애드온 몇 가지가 있어 REST에 추가적인 기능을 더해줄 수 있습니다. 예를 들어:

- dio_cookie_manager - Dio용 쿠키 매니저
- dio_http2_adapter - HTTP/2.0을 지원하는 Dio HttpClientAdapter

<div class="content-ad"></div>

# dio_smart_retry

Dio를 위한 유연한 재시도 라이브러리

# dio_cache_interceptor

여러 저장소를 존줍하여 HTTP 지시문을 준수하는 (또는 그렇지 않은) Dio HTTP 캐시 인터셉터

# dio_http_cache

안드로이드의 Rxcache와 같은 Dio를 위한 간단한 캐시 라이브러리

# pretty_dio_logger

네트워크 호출을 읽기 쉬운 형식으로 로그하는 Dio 인터셉터, 예쁜 Dio 로거

<div class="content-ad"></div>

# 설치

pubspec.yaml 파일에 아래 내용을 dependencies 아래에 추가해주세요:

```js
retrofit: check_latest_ver (^3.0.1+1 현재 버전)
```

그리고 dev_dependencies에는 아래 세 가지를 추가해주세요:

<div class="content-ad"></div>

```js
retrofit_generator: 최신 버전 확인
build_runner: 최신 버전 확인
json_serializable: 최신 버전 확인
```

이제 flutter pub을 실행하고 명령어를 받은 후, Flutter 신에게 기도합니다. 종종 종속성으로 인해 약간 골을 주는 경우가 있습니다. 일시적인 해결책은 버전을 아무 값으로 설정하는 것입니다.

다음으로, 주요 추상 클래스, Dio 설정을 만들고, get_it을 사용하여 나중에 리포지토리에서 액세스할 수 있는 레이지 싱글톤을 만들겠습니다.

# 추상 클래스 만들기

<div class="content-ad"></div>

- 파일을 생성하세요. 이름을 rest_client.dart로 지정할게요.
- Retrofit은 코드를 생성하기 때문에, 파일 상단에 .g 파일을 part로 지정해야 합니다. 우리의 경우에는 rest.client.g.dart가 될 거예요.
- 추상 클래스를 생성하고 @RestApi() 어노테이션을 사용하세요. 이렇게 하면 제너레이터가 retrofit 인터페이스(추상 클래스)임을 알 수 있어요.
- Dio와 옵션으로 베이스 URL을 받는 팩토리 생성자를 만드세요.

![이미지](/assets/img/2024-06-22-RetrofitandFlutterlikeaPRO_1.png)

# Dio 설정

이전에 언급했듯이, Retrofit은 Dio를 완전히 의존하므로, 이를 생성해보겠습니다.

<div class="content-ad"></div>

- dio_client.dart이라는 파일을 만드세요.
- baseUrl 문자열을 인수로 받아 Dio 클라이언트를 반환하는 함수를 생성하세요.
- baseUrl과 Interceptor(이에 대해서 나중에 논의할 것입니다.)와 같은 최소한의 \*BaseOptions로 Dio 인스턴스를 만드세요.

BaseOptions: Dio 인스턴스의 표준 구성입니다. 이 기사에서 다루지는 않겠지만, 연결 제한 시간, 수신 제한 시간, 전송 제한 시간, 쿼리 매개변수, 헤더 등과 같은 다양한 구성 요소가 포함되어 있습니다.

![img](/assets/img/2024-06-22-RetrofitandFlutterlikeaPRO_2.png)

# get_it으로 Dio 및 Retrofit 클래스 연결하기

<div class="content-ad"></div>

의존성 주입을 선택할 때 항상 get_it을 사용했었어요. 이것은 사용하기 쉽고 컨텍스트가 필요하지 않아요. 마지막 단계는 Dio와 Retrofit을 연결하는 것이에요. 이건 한 줄의 코드만 필요해요. get_it에 관한 글이 있으니, 더 깊이 파고들고 싶다면 꼭 읽어보세요. 로케이터 설정에 해당 종속성을 추가하세요:

![이미지](/assets/img/2024-06-22-RetrofitandFlutterlikeaPRO_3.png)

이 두 줄의 코드는 게으른 싱글톤(처음 호출될 때만 만들어지는)을 생성할 거에요. RetrofitClient 인스턴스에서는 Dio 클라이언트와 함께 원격 구성을 전달했어요.

주의 - 원격 구성은 변수로 변경하거나 Firebase와 같은 원격 서비스로 변경하여 env 기본 URL을 구성할 수 있어요.

<div class="content-ad"></div>

드디어 Retrofit을 사용할 준비가 되었습니다! 사용 방법을 살펴봅시다.

## 사용 방법

먼저, 지원되는 HTTP 메소드와 그 추가 방법에 대해 이해해야 합니다. Retrofit은 다음을 지원합니다:

- @GET() — GET 요청을 사용하여 리소스 표현/정보만 검색하고 수정하지 않습니다. 이는 리소스의 상태를 변경하지 않기 때문에 안전한 메소드로 알려져 있습니다.

<div class="content-ad"></div>

@ PATCH() - PUT 요청을 보면 리소스 엔티티를 수정합니다. 좀 더 정확히 말하면 PATCH 방법은 기존 리소스를 부분적으로 업데이트하는 올바른 선택입니다. 전체 리소스를 교체하는 경우에만 PUT을 사용해야 합니다.

@ PUT() - 기본적으로 PUT API를 사용하여 기존 리소스를 업데이트합니다 (리소스가 존재하지 않는 경우 API가 새 리소스를 생성할지 여부를 결정할 수 있음).

@ DELETE() - 이름 그대로 DELETE API는 요청 URI로 식별된 리소스를 삭제합니다.

실제 세계의 예시를 살펴보겠습니다. JSONPlaceholder 웹사이트를 사용하여 예시용 JSON을 생성하겠습니다. 간단한 GET 요청부터 시작해봅시다.

<div class="content-ad"></div>

해당 엔드포인트를 사용할 거예요.

https://jsonplaceholder.typicode.com/posts

우선 Retrofit을 위한 기본 URL을 정의해야 해요. 이 부분이죠: https://jsonplaceholder.typicode.com/

- 기본 URL 정의하기 — 나에게는 모든 원격 주소를 담은 (remote_config) dart 파일이 있는게 의미가 있어요. 그래서 Dio 클라이언트를 빌드하는 get_it 파일에 정의해요 (마지막 사진에서 어떻게 할 지 보여 줄 거예요).
- 웹이나 Swager에서 JSON 응답을 받거나 백엔드 팀에게 요청해요. Retrofit이 변환할 수 있는 Freezed 파일을 만들어보죠. 가장 쉬운 방법은 JSON 요청을 붙여넣고 타깃 언어를 Dart로 선택해서 excellent web tool인 quicktype을 사용하는 거예요. 그리고 패널에서 다음을 켜주세요:

<div class="content-ad"></div>

클래스에 인코더 및 디코더를 추가하고 @freezed 호환성을 갖도록 클래스 정의를 생성해보세요.

<img src="/assets/img/2024-06-22-RetrofitandFlutterlikeaPRO_4.png" />

그 후, freezed 결과를 복사하여 새로운 모델 파일, 예를 들어 post_model.dart를 만들고 오른쪽 코드를 붙여넣은 다음 다음 명령을 실행해보세요. (NOTICE: quickly에서 필드들을 선택적으로하거나 필수로하지 않았는데, freezed는 그것을 허용하지 않으므로 각 부분에 ?를 추가하여 선택적으로 만듭니다.)

flutter pub run build_runner build --delete-conflicting-outputs

<div class="content-ad"></div>

이 는 대화 내용이 포함된 .g 파일과 copyWith, toString 및 해시와 같은 다른 유용한 정보가 들어 있는 .freezed 파일을 생성합니다 (Freezed에 대해 자세히 알고 싶다면 이 문서를 확인해보세요)

다음에 get 호출을 추가하기 전에 API에서 수신한 데이터를 변환하기 위해 retrofit이 필요로하는 응답 클래스를 만드는 부분이 있습니다.

이제 Retrofit을 위한 GET 호출을 추가해 보겠습니다.

![그림](/assets/img/2024-06-22-RetrofitandFlutterlikeaPRO_5.png)

<div class="content-ad"></div>

build_runner를 다시 실행하여 Retrofit이 중요한 작업을 대신 처리하도록 하세요. 이제 API를 사용할 준비가 되었어요. 멋져요. (클린 아키텍처를 사용하는 경우, 모델 레이어의 리포지토리에서 Retrofit 추상 클래스를 호출하고 나중에 FutureBuilder와 함께 위젯에서 호출하지 않고 Usecase로 전달하는 것이 좋습니다.)

좋아요, 가장 간단한 호출을 다루었어요. 조금 흥미롭게 만들어 볼까요? 이제 특정 카테고리, 스포츠라는 카테고리의 게시물을 가져와야 한다고 가정해봅시다. 따라서 String 유형의 쿼리를 전달해야합니다. 문제없어요:

![image](/assets/img/2024-06-22-RetrofitandFlutterlikeaPRO_6.png)

동일한 카테고리 키('category')로 몇 가지 카테고리 ID를 전달해야하는 경우는 어떨까요? String 대신 String 목록으로 교체하면 됩니다. 이렇게 하세요.

<div class="content-ad"></div>

![이미지](/assets/img/2024-06-22-RetrofitandFlutterlikeaPRO_7.png)

현재 백엔드 개발자가 호출 경로에서 카테고리 ID를 지정해야 한다고 결정했습니다. 따라서 다음과 같이 될 것입니다: post/'id'.

![이미지](/assets/img/2024-06-22-RetrofitandFlutterlikeaPRO_8.png)

좋아요, 그럼 데이터와 함께 새로운 포스트를 보내는 것은 어떤가요? 예를 들어, 제목과 내용이 있는 POST라고 할 때요. 쉽죠, 새로운 요청을 위한 새로운 freezed 파일을 만들어보세요.

<div class="content-ad"></div>

```js
@freeze
abstract class PostModelRequest with _$PostModelRequest {
  const factory PostModelRequest({
    String? title,
    String? content,
  }) = _PostModelRequest;
  factory PostModelRequest.fromJson(Map<String, dynamic> json) =>
      _$PostModelRequestFromJson(json);
}
```

그리고 PostModelRequest를 body에 보내는 Post 메서드를 추가해주세요.

![image](/assets/img/2024-06-22-RetrofitandFlutterlikeaPRO_9.png)

같은 방법으로 retrofit 클라이언트에 @PUT, @PATCH, @DELETE 메서드를 추가할 수 있습니다.

<div class="content-ad"></div>

Markdown 형식으로 변경해 보겠습니다.

Retrofit은 인코딩된 폼 데이터를 사용하여 업로드도 지원합니다.

![image1](/assets/img/2024-06-22-RetrofitandFlutterlikeaPRO_10.png)

예를 들어, 무거운 파일을 업로드하다 중단하고 싶을 때는 다음과 같이 할 수 있습니다.

![image2](/assets/img/2024-06-22-RetrofitandFlutterlikeaPRO_11.png)

<div class="content-ad"></div>

만약 특정 호출을 위해 사용자 정의 헤더가 필요하다면, 이 훌륭한 도구를 통해 가능합니다. (보통 이러한 헤더는 전역적이며 레트로핏 클라이언트를 생성할 때 Dio 클라이언트에서 정의됩니다)

![이미지](/assets/img/2024-06-22-RetrofitandFlutterlikeaPRO_12.png)

사용자 정의 헤더는 다음과 같이 동적으로 전달될 수도 있습니다.

![이미지](/assets/img/2024-06-22-RetrofitandFlutterlikeaPRO_13.png)

<div class="content-ad"></div>

개발자님 안녕하세요. 업로드된 진행 상황을 얻어 업로드하는 것도 요청하셨지만, 이겪는 것은 선택 사항입니다. 네, 물론, 단지:

<img src="/assets/img/2024-06-22-RetrofitandFlutterlikeaPRO_14.png" />

# 모든 호출에 토큰 추가하는 방법

회사에서 엄격한 곳이어서 호출을 액세스 토큰으로 안전하게 보호하길 원합니다. 물론, 당신이 원하는 마지막 것은 각 호출에 수동으로 이러한 토큰을 추가하는 것입니다. 절대 안돼요! 우리에게는 인터셉터가 있어서 다행입니다. 인터셉터는 당신이 만든 모든 호출을 듣는 훌륭한 도구입니다. 이들에는 onRequest, onError 및 onResponse와 같은 세 가지 콜백이 있습니다. 우리의 작은 예에서 백엔드는 우리의 토큰을 파이어베이스에서 원합니다. 그래서 token_interceptor.dart라는 새 파일을 만들고 나중에 Dio 클라이언트에 추가합시다.

<div class="content-ad"></div>

![이미지](/assets/img/2024-06-22-RetrofitandFlutterlikeaPRO_15.png)

우리는 모든 요청을 듣고, IdToken이 있으면 헤더로 추가합니다.

또한 오류가 발생하면 간단히 처리하는 방법이 있습니다. 실제 세계에서는 토큰이 만료되어 새로 고칠 필요가 있는 상황을 처리할 수도 있습니다. 요청에 무언가를 하고 싶다면 언제든지 onResponse를 재정의할 수 있습니다.

# 응답을 캐시하는 방법

<div class="content-ad"></div>

Dio에는 더 많은 기능을 추가할 수있는 애드온이 많이 있어요. 그 중 하나는 Hive나 objectbox와 같은 데이터베이스를 구현하지 않고도 응답을 캐시하는 데 도움을 줍니다. pub.dev에 가서 dio_http_cache를 검색해보세요.

설치는 쉬워요:

첫 번째 단계:

```js
dio.interceptors.add(DioCacheManager(CacheConfig(baseUrl: RemoteConfig.baseUrl)).interceptor);
```

<div class="content-ad"></div>

두 번째 단계에서는 요청이 저장될 최대 연령을 설정할 수 있습니다:

```js
Dio().get(
  RemoteConfig.baseUrl,
  options: buildCacheOptions(Duration(days: 7)),
);
```

# 트래픽 로깅하는 방법

트래픽을 로깅하지 않으면 우리의 API 문제를 이해하고 디버깅하는 데 너무 많은 시간이 걸려요. 그래서 좋은 로거가 항상 필요하죠. 다시 pub.dev에 가서 pretty_dio_logger을 검색하세요. 이를 인터셉터 목록에 추가하면 트래픽을 마음껏 볼 수 있습니다. (네트워크 도구를 사용할 수 있지만 때때로 그거 제대로 작동 안 해요. 그리고 이 방법은 AK-47처럼 신뢰할 수 있어요.)

<div class="content-ad"></div>

```js
dio.interceptors.add(PrettyDioLogger(
        requestHeader: true,
        requestBody: true,
        responseBody: true,
        responseHeader: false,
        error: true,
        compact: true,
        maxWidth: 90));
```

애드온 목록이 굉장히 방대하고, 이 기사는 길어지고 있습니다. Dio에 있는 다른 애드온을 확인하시기를 권장합니다 (위에 나열되어 있음).

다음 기사는 오류를 전문가처럼 처리하는 방법에 대해 다룰 예정입니다. 이 내용이 도움이 되었기를 바랍니다. Flutter 서비스가 필요하신 경우 언제든지 연락해 주세요. Flutter 개발 전문 회사인 BlueBirdCoders에서는 4년 이상의 경험을 가지고 있으며, 아래 링크에서 우리 멋진 커뮤니티에 참여하실 수 있습니다:

https://www.facebook.com/groups/flutteril/

<div class="content-ad"></div>

만나서 반가워요! 함께 즐거운 시간 보내요! 🤘🏽
