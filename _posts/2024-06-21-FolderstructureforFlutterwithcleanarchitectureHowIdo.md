---
title: "클린 아키텍처 기반의 Flutter 폴더 구조 작성 방법"
description: ""
coverImage: "/assets/img/2024-06-21-FolderstructureforFlutterwithcleanarchitectureHowIdo_0.png"
date: 2024-06-21 21:54
ogImage: 
  url: /assets/img/2024-06-21-FolderstructureforFlutterwithcleanarchitectureHowIdo_0.png
tag: Tech
originalTitle: "Folder structure for Flutter with clean architecture. How I do."
link: "https://medium.com/@felipeemidio/folder-structure-for-flutter-with-clean-architecture-how-i-do-bbe29225774f"
isUpdated: true
---





## 폴더 구조

![Folder Structure](/assets/img/2024-06-21-FolderstructureforFlutterwithcleanarchitectureHowIdo_0.png)

폴더와 파일을 정리하는 일은 고통스럽습니다. 특히 수천 개의 파일을 관리해야 하는 대규모 프로젝트에서는 더욱 그렇습니다.

파일은 수백 줄을 포함해서는 안 됩니다. 정신 건강을 위해 높은 결합도를 가질 필요가 없고 단일 책임 원칙을 준수하려고 노력해야 합니다.

<div class="content-ad"></div>

고품질의 코드를 작성하면 파일 크기가 작아지고 파일 수가 늘어납니다. 여기서 문제가 발생합니다. 이 모든 파일을 효율적으로 어떻게 정리해야 할까요?

# Clean Architecture으로 작업하기

"Clean architecture"의 개념은 넓고 모호하지만, "저는 이 프로젝트에 Clean Arch를 사용하고 있어요"라고 말할 때, 나는 명확히 Flutterando의 아키텍처 제안을 말하는 것입니다!

## 제안서 간단히 살펴보기

<div class="content-ad"></div>

코드를 4개의 계층으로 분리할 것입니다: Presenter, Domain, Infra, External.

![이미지](/assets/img/2024-06-21-FolderstructureforFlutterwithcleanarchitectureHowIdo_1.png)

- Presenter — UI 구성 요소입니다. 실제로 위젯이거나 위젯의 컨트롤러인 모든 것이 여기에 속합니다.
- Domain — 앱의 핵심입니다. 모든 엔티티 및 비즈니스 로직이 이 곳에 보관됩니다.
- Infra — 외부 레이어에서 오는 데이터를 모델, 저장소 및 서비스를 통해 도메인 레이어를 지원합니다.
- External — 타사 라이브러리, 센서, SO, 저장소 및 앱의 다른 외부 종속성의 기능을 래핑하는 클래스입니다.

강건한 해결책은 없지만, 저는 대부분의 상황에서 잘 작동하는 깔끔한 아키텍처를 선호합니다. 플러터 앱에서 깔끔한 아키텍처를 개선하기 위해 기술하는 몇 가지 관행을 추가한 이 글을 참고해 주세요.

<div class="content-ad"></div>

메모:

이 아키텍처 제안에 익숙하지 않은 분들에게는 "entity", "controller" 등과 같은 구체적인 용어의 의미를 알기 어려울 수 있습니다.

이 글 전체를 통해 더 자세한 설명을 볼 수 있지만, 제안 내용을 설명하는 것이 목표가 아닙니다. 공식 문서를 읽을 필요는 없습니다.

## Clean arch + Modular arch = 행복한 개발자 😀

<div class="content-ad"></div>

모듈화 아키텍처는 관련 콘텐츠를 "모듈"이라고 불리는 한 곳에 모으는 것을 목표로 합니다.

각 모듈은 시스템의 중요한 책임을 나타내며 모든 모듈은 다른 모듈과 통신하는 규제를 갖습니다.

이는 결합도를 줄이고 다른 팀과 전문가들이 동일한 소스 코드로 작업하는 데 도움이 되는 시스템으로 볼 수 있습니다.

![image](/assets/img/2024-06-21-FolderstructureforFlutterwithcleanarchitectureHowIdo_2.png)

<div class="content-ad"></div>

플러터에서 모듈 구조를 만들려면 Angular 프레임워크의 모듈 시스템을 기반으로 한 flutter_modular 패키지를 사용할 수 있어요.

이 패키지는 앱을 모듈로 분할하며, 각각의 모듈은 페이지와 종속성을 가지고 있어요. 사용자가 모듈을 빠져나오면, 해당 모듈의 모든 종속성이 폐기됩니다.

또한, 이 패키지는 의존성 주입 및 시스템 내비게이션을 위한 도구도 함께 제공돼요.

# 루트 구조

<div class="content-ad"></div>


<img src="/assets/img/2024-06-21-FolderstructureforFlutterwithcleanarchitectureHowIdo_3.png" />

pubspec.yaml 파일과 함께 작업해야 할 디렉토리는 lib, assets, test 이렇게 3개가 있습니다.

여기에 새로운 내용은 없습니다! 아마 이미 사용하고 있을 것입니다. lib와 test는 플러터 프로젝트를 생성할 때 자동으로 생성되는 폴더이며, asset 폴더 사용이 플러터 팀에 의해 권장됩니다.

하지만 새로 온 사람들을 위해 그들의 사용법을 정의해야 할 것입니다. 그 이후에는 우리의 관심을 lib 폴더에만 집중할 것입니다.


<div class="content-ad"></div>

## 에셋 폴더

이미지, 글꼴, 아이콘, 비디오 등과 같은 모든 코드가 아닌 파일을 보관하는 곳입니다. 앱에서 사용되는 모든 자원이 여기에 포함될 수 있습니다. 여기에는 에셋의 공식적인 정의가 있습니다:

## 라이브러리 폴더

여기에는 당신의 Dart 파일이 위치합니다! 코드를 여기에 넣어주세요. =)

<div class="content-ad"></div>

## 테스트 폴더

여러분의 테스트 파일은 여기에 있습니다. 이 구조는 lib 폴더와 동일합니다.

예를 들어, lib/modules/register/presenters/widgets에 있는 위젯 user_register_form.dart에 대한 테스트를 예로 들어보겠습니다.

<div class="content-ad"></div>

아래와 같이 만들어야 합니다.

test/modules/register/presenters/widgets/user_register_form_test.dart.

# 라이브러리 구조

![라이브러리 구조](/assets/img/2024-06-21-FolderstructureforFlutterwithcleanarchitectureHowIdo_4.png)

<div class="content-ad"></div>

## 메인 폴더

![이미지](/assets/img/2024-06-21-FolderstructureforFlutterwithcleanarchitectureHowIdo_5.png)

이 폴더에는 각 Flavor의 주요 기능을 실행하는 파일이 포함되어 있습니다.

또한 모든 Flavor의 일반 명령을 실행하는 common_main.dart 파일도 있습니다.

<div class="content-ad"></div>

플레이버를 사용하지 않는 경우, 이 폴더와 해당 내용을 닥스트 파일만 사용할 수 있도록 전환할 수 있습니다.

## i18n 폴더

![이미지](/assets/img/2024-06-21-FolderstructureforFlutterwithcleanarchitectureHowIdo_6.png)

잠깐! JSON 파일인가요? assets 폴더에 있어야 하는 게 아닌가요? 네! 그렇습니다! 이것이 제 죄약입니다. JSON 파일을 넣어야 하는 올바른 위치는 assets 폴더입니다. 하지만 localization 패키지는 lib 파일에 유지해야 합니다.

<div class="content-ad"></div>

물론이에요, 이건 패키지에서 강요하는 대로 하는 거예요. 그러나 의존성을 변경하고 easy_localization 패키지 등을 사용한다면 lib의 i18n 폴더를 제거해야 해요.

그렇다고 해도, 난 여전히 localization 라이브러리를 선호해요. 사용하기 쉽고 열린 이슈도 적거든요.

easy_localization 패키지 매니저는 더 이상 패키지를 활발하게 유지하지 않겠다고 이미 말했어요. 그래서 사용에 대해 경고하는 신호를 보냈어요.

## Core 폴더

<div class="content-ad"></div>


![Folder structure for Flutter with clean architecture](/assets/img/2024-06-21-FolderstructureforFlutterwithcleanarchitectureHowIdo_7.png)

다음은 Clean Architecture 레이어로 표현할 수 없는 모든 공유 로직을 배치하는 폴더입니다. 예를 들어, 정규식 문자열, 믹스인 및 유틸리티 클래스의 클래스입니다.

- Configs: 시스템에 필요한 초기 구성을 저장합니다. 예를 들어, firebase 초기 구성.
- Constants: 라우트 이름 및 정규식과 같은 앱 설정 문자열.
- Extensions: Dart 확장 기능.
- Mixins: Dart 믹스인.
- Utils: 통화 포매터 또는 날짜 유틸리티와 같은 유틸리티 클래스.
- Validator: 필드, 전화 번호, 문서 등을 유효성 검사하는 클래스입니다.

다시 말씀드리지만, 이것은 저의 개인 경험입니다. 일반적으로 이러한 폴더들로 충분히 필요한 모든 클래스를 보관할 수 있습니다. 프로젝트에 따라 utils의 남용을 피하기 위해 다른 폴더를 생성하기도 합니다.


<div class="content-ad"></div>

## 모듈 폴더

![모듈 폴더 구성](/assets/img/2024-06-21-FolderstructureforFlutterwithcleanarchitectureHowIdo_8.png)

앱 모든 모듈을 포함하는 폴더입니다. 각 모듈 내에서는 앱의 각 페이지에 대한 필요한 클린 코드 레이어를 생성합니다.

주 모듈과 루트 위젯도 모듈 폴더 내에서 생성됩니다.

<div class="content-ad"></div>

각 레이어에는 특정 유형의 클래스가 저장되어 있습니다. 이들은 다음과 같습니다:

- 프리젠터:
위젯 — 시각적 컴포넌트 클래스.
컨트롤러 — 위젯의 상태 관리 클래스.
- 도메인:
엔티티 — 데이터를 저장하는 클래스.
유스케이스 — 비즈니스 논리 클래스.
- 인프라:
모델 — 데이터를 변환하기 위해 엔티티를 확장하는 클래스.
데이터 소스 — 외부 API와 연결하는 클래스 (HTTP 드라이버 사용).
서비스 — 외부 API와 연결하지 않는 클래스 (HTTP 드라이버를 사용하지 않음).
- 외부:
드라이버 — 외부 라이브러리나 시스템 기능을 격리하는 클래스.

외부 레이어에는 클래스 유형이 하나뿐이므로 드라이버 폴더를 직접 만들겠습니다.

# 모듈 구조

<div class="content-ad"></div>

모듈 구조의 세부 사항을 분석하기 위한 예시를 살펴보겠습니다. 저는 권한 모듈을 선택했는데, 이는 로그인 및 사용자 등록 양식과 같이 인증되지 않은 사용자가 액세스할 수 있는 모든 화면을 나타냅니다.

![Folder Structure](/assets/img/2024-06-21-FolderstructureforFlutterwithcleanarchitectureHowIdo_9.png)

각 모듈 폴더 내에서는 해당 모듈에서 사용하는 네비게이션 경로와 의존성을 설명하는 동일한 이름의 모듈 파일이 있을 것입니다.

저는 모듈을 페이지로 분리하는 것을 좋아합니다. 이 경우 사용자는 로그인, 등록 및 비밀번호 복구 3개의 페이지에 액세스할 수 있습니다.

<div class="content-ad"></div>

<img src="/assets/img/2024-06-21-FolderstructureforFlutterwithcleanarchitectureHowIdo_10.png" />

로그인 폴더 내부에는 로그인 페이지에 필요한 클린 아키텍처 레이어가 포함되어 있습니다.

## 프리젠터

모듈은 페이지로 나뉘기 때문에, 프리젠터 폴더의 루트에는 로그인 페이지 위젯과 컨트롤러가 나타납니다.

<div class="content-ad"></div>

다른 위젯은 "위젯" 폴더와 같은 다른 폴더에 있습니다. 필요할 때 위젯을 편의에 맞게 정리해보세요.

그리고 "dialogs" 폴더가 있습니다. 또한 페이지를 구성하는 데 PageView 위젯이 필요한 경우 "views" 폴더가 있을 수 있습니다.

위젯과 컨트롤러는 자신의 폴더가 없는 유일한 클래스 유형입니다. 높은 상관 관계 때문에 항상 함께 있어야 합니다.

## 도메인

<div class="content-ad"></div>

로그인 기능을 위해 사용자 이름과 비밀번호를 저장하는 AuthEntity 클래스가 있습니다.

Usecase에서는 모든 비즈니스 로직을 실행합니다. 그렇다면 로그인 기능을 위한 가능한 비즈니스 로직은 무엇일까요?

만약 없다면, 단지 데이터 소스를 호출하기 위해 도메인이 필요합니다. 비즈니스 로직이 없다고 해서 아키텍처 규칙을 어기는 것은 변명이 되지 않습니다.

하지만... 사용자가 앱을 닫은 후에도 인증된 상태를 유지하나요? 그렇다면, 로컬에 인증 토큰을 저장해야 하는데, 이것도 비즈니스 로직입니다.

<div class="content-ad"></div>

사용자가 로그인할 때 분석 데이터를 보내야 하나요? 이것도 비즈니스 로직입니다.

## 인프라

auth_datasource.dart 및 auth_datasource_impl.dart 파일을 메모해 두었나요?

Datasources, Services, 및 Drivers를 위한 인터페이스가 작성되었습니다. 코드가 있는 위치는 `name`_`layer`_impl.dart 파일입니다. 인터페이스 사용은 결합도를 줄이고 Mock 클래스로 테스트를 구축하는 데 도움이 됩니다.

<div class="content-ad"></div>

만약 파일의 유형이 데이터 소스인 경우 이미 API를 쿼리한다는 것을 알고 계실 겁니다.

도메인은 데이터 소스에 AuthEntity를 제공하며, 이는 API 형식을 위해 값을 변환하기 위해 AuthModel을 사용합니다.

## Driver (External)

우리가 API를 호출하면서 HTTP 클라이언트를 위한 드라이버 폴더가 없는 이유는 무엇일까요? 그것이 공유 모듈에 있기 때문이죠!

<div class="content-ad"></div>

## 공유 모듈

![이미지](/assets/img/2024-06-21-FolderstructureforFlutterwithcleanarchitectureHowIdo_11.png)

페이지를 보관하는 모듈 외에도 공유 모듈을 가지고 있습니다.

이 모듈에는 모듈 간에 공유되는 모든 유형의 클래스를 넣을 것입니다. 가장 좋은 예는 HTTP 클라이언트 드라이버입니다.

<div class="content-ad"></div>

이것은 버튼, 텍스트 필드, 스위치 등과 같은 테마 위젯을 공유하는 모듈입니다.

# 내 폴더 구조 만족스러운가요?

![image](/assets/img/2024-06-21-FolderstructureforFlutterwithcleanarchitectureHowIdo_12.png)

아니요! 왜 그래야 하나요?

<div class="content-ad"></div>

그 죄악스러운 i18n 폴더, 언젠가는 로컬라이제이션 패키지가 고쳐줄 거에요!

제가 작업 중인 현재 모듈이나 공유 모듈에 원하는 드라이버가 있는지 항상 확신할 수 없네요.

utils 폴더를 봤나요? 유틸리티 클래스의 정확한 정의가 뭔지 아시나요? 제가 프로그래밍한 이유는 제게 유틸리티가 있기 때문이죠. 그저 제대로된 이름을 생각해내지 못해서 그것뿐인 거예요.

이론적으로, 필드 유효성 검증은 비즈니스 로직에 의해 정의되지만, TextField 위젯에 설정해야 합니다. 아마도 입력 유효성 검증은 공유 모듈 내부의 도메인에 있어야 할 것 같아요. 하지만, 그것들은 일반 유틸리티 함수이기 때문에 코어 폴더에 있어요.

<div class="content-ad"></div>

머리 아프다! 그래도 과거의 나보다는 나아진 것 같아. 앞으로도 계속 나아졌으면 좋겠다.

그럼, 내가 도울 수 있을까 생각해봐. 나는 같은 문제를 겪는 개발자들을 지원하고 피드백을 얻기 위해 이 지식과 생각을 공유했어.

Flutter 앱을 위한 파일 구조에 대한 더 나은 또는 알려지지 않은 솔루션이 있는지 알고 있니?

내 생각과 프로그래밍 팁을 더 읽고 싶다면, 내 글을 확인하고 앞으로 올 컨텐츠를 팔로우하거나 구독해줘.