---
title: "Flutter를 사용한 크로스 플랫폼 개발 방법"
description: ""
coverImage: "/assets/img/2024-06-21-CrossPlatformdevelopmentwithFlutterHowGoogleClassroomgetsteachersandstudentsonthesamepage_0.png"
date: 2024-06-21 23:54
ogImage: 
  url: /assets/img/2024-06-21-CrossPlatformdevelopmentwithFlutterHowGoogleClassroomgetsteachersandstudentsonthesamepage_0.png
tag: Tech
originalTitle: "Cross Platform development with Flutter — How Google Classroom gets teachers and students on the same page"
link: "https://medium.com/flutter/cross-platform-development-with-flutter-how-google-classroom-gets-teachers-and-students-on-the-597d4f3b450c"
isUpdated: true
---




Google의 Classroom 앱은 원래 2014년에 시작되어 전 세계의 1억 5천만 교육자와 학생들이 수업 내에서 숙제, 성적 및 커뮤니케이션을 조직화하는 데 사용됩니다. Android 및 iOS에서 사용할 수 있으며, 개발은 처음 시작된 해부터 이어져 왔으며, 이동 플랫폼의 변화 시대를 겪었습니다. 그 다양한 변화를 관리하는 것은 어려운 일이었습니다.

동기화를 위한 노력에도 불구하고, 2021년에는 7년 뒤에도 Classroom 앱의 독특한 Android 및 iOS 코드베이스가 특징, UI 및 구현에서 점진적으로 멀어졌습니다. 동일한 UI에 대해 다른 접근 방식을 취한 화면과 같은 가장 명백한 차이부터 인증 및 앱 시작 로직의 차이와 같이 덜 명백한 차이까지; Classroom은 유지 및 향상하기 어려운 앱으로 변모했고, 두 코드베이스가 소규모 개발자 풀에 부담을 주었습니다.

지속적인 현상 유지, 더 많은 개발자 추가, 크로스 플랫폼 프레임워크를 사용한 두 코드베이스의 완전한 재작성이 포함된 다양한 옵션이 있었습니다. 개선에 헌신하는 팀은 상태 quo 옵션을 제외하고, 이미 존재하는 두 코드베이스를 안정화하는 데 필요한 조치를 평가한 후, 단순히 더 많은 개발자를 추가하는 것으로는 부족하다고 판단했습니다. 결국 팀은 그들의 세 번째 선택인 Flutter를 사용하여 단일 소스, 크로스 플랫폼 솔루션으로 Classroom을 재상상하기로 결정했습니다.

# Flutter가 Classroom 앱을 어떻게 간단하게 만들었는지

<div class="content-ad"></div>

## 일관성 없는 UIs

Google Classroom의 가장 뚜렷한 문제 중 하나는 UI의 변화로, 선생님들이 안드로이드와 iOS UI를 깊이 알아야 했습니다. 학생들이 한 플랫폼에서 보는 숙제 화면과 지침에 관한 질문을 받았을 때, 다른 플랫폼에서 본 화면과의 비교로 답변이 상당히 혼란스러울 수 있습니다.

![UI variations](/assets/img/2024-06-21-CrossPlatformdevelopmentwithFlutterHowGoogleClassroomgetsteachersandstudentsonthesamepage_0.png)

일반적인 접근 방식은 시간이 지남에 따라 차이가 벌어지는 별도의 팀이 개발한 별도의 클라이언트 앱입니다. 이를 방지하기 위해서는 매우 일관된 작업을 통해 모든 기능을 동기화해야 합니다. 그것과 대조적으로, 플러터의 본질은 기본 설정으로 UI가 동일하며 [1], 사용자의 이점을 위해 적응성을 위한 노력(자주 발생)이 있을 때까지만 다른 방향으로 빗겨나갑니다.

<div class="content-ad"></div>

## 혼동된 비즈니스 로직

클래스룸의 안드로이드 및 iOS 클라이언트가 맞지 않았던 유일한 곳은 사용자 인터페이스뿐만이 아니었습니다. 일부 복잡한 비즈니스 로직을 클라이언트로 오프로드하는 서버 측 솔루션에 의해 형성된 과거의 클래스룸 앱은 또한 핵심 구현에서의 차이를 처리했습니다. 가끔씩 플랫폼별 버그가 발생하기도 했으며 (엔지니어가 재현하려는 노력을 좌절시키는 경우도 있었습니다!), 이는 양쪽 구현의 정확성을 평가하려는 사람에게 상당한 정신적 부담을 줬습니다.

Flutter를 사용하여 클래스룸을 다시 작성함으로써, Flutter가 네이티브 플랫폼 상호작용을 처리하는 방식 때문에, 이전에 보고되지 않았던 수많은 버그들이 간단히 해결되었습니다.

원래 코드에서는 수년에 걸친 지속적인 개발로 UI, 비즈니스 및 플랫폼별 로직 간의 경계가 가끔씩 흐려졌습니다. 이는 사용자의 버그 보고가 거의 항상 전체 호출 스택이 잠재적으로 죄가 있는 지 절대적으로 구분해야하는 큰 노력임을 의미했습니다. 요청한 파일이 로드되지 않는 이유가 파일 시스템을 잘못 읽어서, 비즈니스 로직에서 미스통신 때문에, 아니면 UI가 파일을 수신하지만 그 후로 파일을 잃어버린 것일까요? 그것을 알아내는 유일한 방법은 모든 것을 조사하는 것뿐이었습니다.

<div class="content-ad"></div>

물론, 플러터 개발자들도 누구나 할 수 있는 것처럼 이 경계를 흐려지게 만들고 로직을 섞을 수 있지만, 수업실 엔지니어링 팀은 프레임워크 모베르 베스트 프랙티스를 따르는 것이 이를 쉽게 드러내게 만드는 것으로 발견했습니다. 플러터의 선언적 UI 시스템은 UI 위젯 내에서 실수로 비지니스 로직을 배치하는 것을 강력히 권고하며, 새로운 MVVM 아키텍처는 실수 없이 정리된 책임 층을 강제할 수 있도록 도와주었습니다. 이는 플러터 위젯 뒤에 있는 방대한 코드베이스 내에서 분명한 레이어를 유지할 수 있도록 해주는 데 도움이 되었습니다.

플러터 앱은 여전히 주기적으로 기본 플랫폼과 통신해야 합니다 — 어쨌든, 숙제를 업로드하고 볼 수 있는 사용자 경로는 파일 시스템을 사용하지 않으면 이뤄질 수 없습니다. 하지만 여기서도 플러터는 플랫폼별 로직을 전용 플러그인으로 격리하는 패턴을 따라 일상적인 디스크 I/O와 같은 것이 소속되지 말아야 하는 곳으로 슬금슬금 스니킹되는 것을 방지했습니다. 아래 예제는 플러터 앱이 전체 호출 스택을 혼란스럽지 않게 하면서 파일 시스템에 액세스하는 현실적인 방법을 보여줍니다.

```js
import "dart:io";
import "package:path/path.dart" as path;
import "package:path_provider/path_provider.dart" as path_provider;

// 특정 과제에 대한 학생의 저장된 숙제를 불러오는 함수
// 반환된 값의 exists() 함수는 학생이 숙제를 먹은 경우 False를 반환합니다.
Future<File> getHomework(Assignment assignment) async {
  // `path_provider` 패키지를 사용하여 플랫폼별 파일 시스템 특성을 추상화합니다
  final Directory homeworkDirectory =
    await path_provider.getApplicationSupportDirectory();

  // 학생이 업로드한 숙제를 추출합니다
  return File(
    path.join([homeworkDirectory.absolute.path, assignment.name]),
  );
}
```

이 예는 간단합니다. 수업실 엔지니어링 팀은 궁극적으로 플랫폼과 더 복잡한 상호작용을 포함하는 자체 플러그인을 개발했습니다. 흥미로운 점은, 이렇게 함으로써 그들의 네이티브 코드가 원래의 네이티브 앱보다 디버깅하기 쉬워졌습니다. 이게 가능했던 이유는 무엇일까요? Flutter 플러그인에서 반복하지 않기(Don’t Repeat Yourself, DRY) 원칙을 따르면 가능한 한 많은 비지니스 로직을 다트 코드로 올려놓고, 네이티브 상호작용을 위한 가장 간단한 메서드 호출만 남겨두기 때문입니다. 이는 도메인 로직과 플랫폼 로직 사이에 확고한 분리를 강요하여, 수업실의 안드로이드나 iOS 코드의 오류가 단일 책임 함수에 격리되어 쉽게 이해할 수 있는 것이었습니다.

<div class="content-ad"></div>

## 성능 저하

사용자의 여정이 실패할 때, 모두가 동의하는 반드시 해결해야 할 명확한 버그가 제출됩니다. 하지만 앱 시작 시간이 점점 악화되어온 지어 올 해 말로 얼마전 앱이 출시된 후 몇 년 동안 변해온 문제와 같은 더 부드러운 문제에 대해 어떻게 해야 할까요? 여러 클라이언트를 동기화하는 것에 대한 우려를 더하면, 앱의 느린 시작 흐름을 해결하는 작업은 절망적인 일처럼 느껴집니다.

여기서 Flutter이 도움을 주어 문제를 악화시키지 않으면서 충분히 빠른 속도로 도와주었고, 더 중요한 것은 깨끗한 출발의 기회를 제공했습니다. 개발 몇 년을 끌어온 기존 문제를 해결하는 대신 새로운 것을 구축하고 있다는 사실로, Classroom 팀은 중복되는 API 호출을 제거하고 다른 독립적인 API 호출을 병렬화하며, 모든 것이 해결될 때까지 shimmer 효과와 다른 UI 미리보기를 표시함으로써 권한 부여 및 시작 흐름을 명확히 했습니다. 결과는 앱 시작 시간의 놀라운 80% 감소였습니다!

## 주석 기능

<div class="content-ad"></div>

대부분의 Classroom 기능은 과제와 업로드된 숙제와 같은 내용을 공유하는 사용자들을 모아주는 비교적 일상적인 앱으로 볼 수 있습니다. 그러나 한 가지 기능이 분명히 복잡하게 빛을 발합니다. Classroom의 주요 기능 중 하나는 파일 공유입니다. 여기서 교사와 학생 모두가 파일을 만들고 보고 편집할 수 있으며, 자유형 주석을 추가할 수 있습니다. 마치 펜이나 마커로 직접 종이에 그림을 그리는 것처럼 자유롭게 주석을 추가할 수 있습니다. 이 주석 공유 기능은 이미 Classroom의 내장 Android 및 iOS 클라이언트에 존재했기 때문에 Flutter로 이식하는 것이 어려웠던 문제였습니다.

Classroom 팀은 이 주석 기능을 플랫폼별 구현을 별도의 라이브러리로 위임하는 플러그인으로 재패키지할 수 있었습니다. 파일 주석에 대해, 해당 기능은 이미 Google One, Google Keep 및 이전 Classroom 앱에서 사용되는 기존 네이티브 라이브러리들을 감싼 얇은 래퍼로 되어 있었습니다. 내부적으로 Android와 iOS는 파일 공유 주변에 서로 다른 구현 요구 사항이 있습니다. iOS에서는 Classroom 앱이 네이티브 뷰를 통해 파일에 액세스하지만, Android에서는 Google Keep 앱을 직접 엽니다. 그러나 좋은 플러그인 디자인 원칙을 통해 이러한 구현 세부 사항을 격리시키고 앱의 나머지 부분에서 깨끗하고 일관된 Dart API를 노출할 수 있었습니다. 결과적으로 Classroom의 "가장 복잡한" 기능 중 하나가 Flutter로 성공적으로 이식되었습니다.

아래는 Android에서 Classroom 주석 기능의 시각화입니다. 네이티브 및 Flutter UI 구성 요소의 혼합이 나와 있습니다.

![Classroom Annotation Feature on Android](/assets/img/2024-06-21-CrossPlatformdevelopmentwithFlutterHowGoogleClassroomgetsteachersandstudentsonthesamepage_1.png)

<div class="content-ad"></div>

보다 넓게 바라보면, 플러터에서의 전형적인 플러그인 디자인은 다음과 같이 구성되어 있습니다. 단일하고 간결한 인터페이스가 플랫폼별 라이브러리를 로드하며, 이 라이브러리들은 다시 FFI 또는 JNI를 사용하여 기본 플랫폼과 통신합니다. 이를 통해 플러터 앱은 빌드 대상의 모든 플랫폼별 네이티브 API와 상호 작용할 수 있으며, 이러한 고려 사항을 Dart 코드로 유출시키지 않습니다.

![이미지](/assets/img/2024-06-21-CrossPlatformdevelopmentwithFlutterHowGoogleClassroomgetsteachersandstudentsonthesamepage_2.png)

# 되돌아보며

## 개발 속도

<div class="content-ad"></div>

클래스룸 팀은 앱을 다시 작성하는 데 2년이 걸렸습니다. 초기 프로토타입 단계에서 1명의 엔지니어로 시작하여 개발의 절정 단계에서는 10명의 정규 엔지니어로 성장한 팀과 함께 작업했습니다. 이것은 작은 투자가 아니지만, 개발 및 유지 보수가 영원히 빨라진다는 약속을 바탕으로 하였습니다. 클래스룸은 2023년 6월에 iOS에서 플러터 재작업을 시작하고, 2024년 1월에 Android에서 발표하여 프로젝트를 완료했습니다. 그 이후로 새로운 기능에 소요된 평균 엔지니어링 시간이 3분의 2 감소했고, 개발자 속도가 3배 증가했습니다! 2년간 새로운 기능을 기다린 이후 스테이크홀더들은 기다렸던 ✨빠른 기능 개발✨의 도래로 기뻐하고 있습니다.

클래스룸 팀이 다시 작성을 결정한 부분 중 하나는, 프로젝트가 결코 "끝나지 않을" 것이라는 것을 인지하였기 때문입니다. 새로운 기능이 미래에도 계속 추가될 것으로 예상되었습니다. 이는 다소 비용이 많이 들더라도 언젠가는 그 대가를 치룰 것이라는 설득력 있는 이유가 되었습니다. 클래스룸 팀이 다시 작성에서 투자 회수 포인트에 도달할 때까지의 공식은 다음과 같습니다:

![수식](/assets/img/2024-06-21-CrossPlatformdevelopmentwithFlutterHowGoogleClassroomgetsteachersandstudentsonthesamepage_3.png)

iOS를 출시한 이후 9개월 동안, 클래스룸은 플러터가 제공하는 개발자 속도 3배 증가로 초기 투자의 40%를 이미 회수했다고 추정하고 있습니다.

<div class="content-ad"></div>

## 개발자 경험

개발 속도가 증가하여 가장 행복해하는 사람들은 이해 관계자보다 개발자들입니다. 교실 팀의 경우, 개발 속도가 3배 증가한 것은 각 기능을 한 번만 작성하거나(또는 무거운 네이티브 구성 요소가 있는 경우에는 최대 1.5배 작성), 종종 몇 달씩 격리되어 있는 두 팀 간의 조정 비용을 제거하고, 물론 핫 리로드가 결합된 결과입니다. 핫 리로드만으로도 약 99%의 재구축 시간 감소를 이뤘으며, 이것은 교실 팀의 사기를 높여 두 네이티브 클라이언트로부터 얻었던 것보다 더욱 크게 향상시켰습니다. 교실 팀은 Flutter로 전환한 후 엔지니어를 쉽게 유치하고 유지할 수 있었습니다.

게다가, 교실 팀은 새로운 기능을 구현하는 데 평균적으로 최소 50% 더 적은 코드 라인이 필요하다는 사실을 발견했습니다. 실제로는, 재작업 중에 구축한 모든 기능이 두 네이티브 클라이언트에서 실제로 구현되어 있지 않았기 때문에 감소율은 상당히 높을 수 있습니다. 다시 말해, 이전 모든 기능뿐만 아니라(예: iOS의 오프라인 지원 포함) 큰 기능 차이를 완벽하게 수행하는데 필요한 코드의 50% 정도만으로 충분합니다.

# 결론

<div class="content-ad"></div>

교실 팀은 리라이트를 시작한 지 약 2년 후에 Android 및 iOS로 그들의 앱을 출시했으며 추가적인 기능을 추가하여 초기 투자의 40%를 상환했습니다. 새로운 앱은 이전 것보다 거의 5배 빠르게 실행되어 개발자와 최종 사용자가 시간과 귀찮음을 절약할 수 있었습니다. 미래를 전망해 보면, 새로운 기능은 이전 상태보다 개발 비용이 1/3이며 두 플랫폼에서 동시에 릴리스되고 문제 해결 및 유지보수가 더 쉽습니다. 교실이 플러터로 전환한 후 사용자, 개발자 및 이해당사자들의 사기는 그들의 미래를 투자하는 것으로 바뀐 후에 이전보다 더 높아졌습니다.
