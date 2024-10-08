---
title: "플러터를 인정하지 않는 구글"
description: ""
coverImage: "/assets/img/2024-06-19-GoogleDoesntAppreciateFlutter_0.png"
date: 2024-06-19 00:06
ogImage: 
  url: /assets/img/2024-06-19-GoogleDoesntAppreciateFlutter_0.png
tag: Tech
originalTitle: "Google Doesn’t Appreciate Flutter"
link: "https://medium.com/@impure/google-doesnt-appreciate-flutter-as-much-as-they-should-have-885dd1d2bd2a"
isUpdated: true
---





또 하루, 또 하나의 "Google이 Flutter를 제거할까?" 글이 올라왔어요. 이젠 거의 유쾌하다고 해야하나요?

이전에 Flutter에 대한 생각을 표현한 적이 있습니다. 그것은 단순히 실패하기에는 너무 크다고 생각해요. 하지만 구글이 정말 Flutter를 제대로 인정해주는지는 장담하기 어렵네요.

요게 좀 어이없는 글인데요, 제가 거의 홍보글 같다고 생각할 정도거든요. 왜냐하면 Google IO는 여러 개의 키노트로 이루어져 있으며 Flutter만큼 Google이 진지하게 다뤄야 할 프로젝트는 아니기 때문이죠.

하지만 눈에 띈 것은 이 부분이었어요: "Gemini Flash 1.5 통합 예제를 본 결과, JavaScript, Python, Kotlin, Swift 예제는 있었지만 Dart나 심지어 Go는 없었어요"

<div class="content-ad"></div>

얼마 전에 Firebase가 너무 비싸다고 말한 글을 썼었죠. 대안을 찾고 있었어요. Firebase 글에서는 Supabase에 대해 이야기했어요. 그리고, 네, Supabase는 정말 흥미로운 대안이에요. 하지만 저는 PocketBase도 살펴보고 있어요.

PocketBase를 사용하는 사람들이 정말 좋아하는 것 같아요. 그리고 이 프로젝트는 정말 작고 효율적이에요. PocketBase의 전체 실행 파일은 13MB밖에 안 됩니다. 13MB보다 큰 글꼴도 많이 있죠. 이렇게 작은 크기에 전체 백엔드를 얻을 수 있다는 게 정말 놀랍네요. 그리고 다른 백엔드에 비해 아주 적은 메모리를 차지하고 있어요.

어쨌든, 내 요구 사항을 충족하는 백엔드를 찾는 평가 과정 중에 이 백엔드가 무엇을 할 수 있는지 확인해봐야 해요. 제가 주로 필요로 하는 것은 다음과 같아요: 인증, 클라우드 함수, 실시간 리스너, 그리고 오프라인 기능입니다. 실시간 리스너와 오프라인 기능을 모두 지원하는 데이터베이스를 찾는 건 의외로 힘들어요. 아마 제가 오프라인을 직접 구현해야 할 것 같아요. 그러면 인증, 실시간 리스너, 그리고 클라우드 함수만 있으면 충분할 것 같아요.

아, 그리고 충돌 보고와 푸시 알림도 있지만, 무료로 사용할 수 있는 Crashlytics와 Firebase Messaging을 계속 사용해야 할 것 같아요.

<div class="content-ad"></div>

저는 Supabase와 PocketBase 둘 다 살펴봤어요. 둘 다 제가 원하는 기능을 갖추고 있더라구요. 오프라인을 지원하지 않는다는 게 좀 아쉽긴 한데, 아마 제가 직접 구현해야할 것 같아요. 근데 문서를 살펴보니 흥미로운 점이 있더라구요. Supabase는 이렇고요:

![Supabase image](/assets/img/2024-06-19-GoogleDoesntAppreciateFlutter_0.png)

그리고 PocketBase는 이렇습니다:

![PocketBase image](/assets/img/2024-06-19-GoogleDoesntAppreciateFlutter_1.png)

<div class="content-ad"></div>

코드 예시에 Dart가 포함되어 있다는 건 놀라운 일이에요. 왜냐하면 제가 예전에는 Flutter 개발자만이 중요하게 여기는 작은 언어로만 알고 있었거든요. Flutter 개발자 수가 많지 않다고 생각했거든요. 사실, Flutter를 배우기 전까지는 Dart라는 언어를 들어본 적이 없었어요.

다른 회사들도 자사의 문서에서 Dart를 언급한다는 소문을 들었어요. 대기업인 RevenueCat와 Google 등이 말이에요. 두 개나 세 개의 코드 예시만 나열된 페이지에서 Dart가 포함되어 있다는 건 예상 밖인 일이었어요.

아니면, Dart를 사용하는 프레임워크가 생각보다 많았거나 Flutter가 놀랍도록 인기가 많은 것일지도 몰라요.

저는 Flutter가 절대 멸망하지 않을 거라고 의심스러웠거든요... 제트팩 컴포즈가 인기를 얻기 전까진요. 왜냐하면 제트팩 컴포즈는 Flutter에서 많이 영감을 받았거든요. 사실, 코틀린을 사용하는 Flutter 정도라고 할 수 있어요. 근데 문서엔 코틀린이나 자바라는 언어가 언급되지 않는 거죠. 양쪽 모두 없다니까요. 겨루기 보단 함께 성장하는 게 좋죠. 좀은 경쟁이 되겠네요. 😉

<div class="content-ad"></div>

음, 그리고 'Appwrite'라는 세 번째 백엔드도 있더라구요. 이상한 가격 정책 때문에 사용할 계획은 없고 많이 언급되지 않는 것 같아요. 하지만 그 문서에서 플러터(Flutter)와 코틀린(Kotlin)도 언급한다고 해요.

![이미지](/assets/img/2024-06-19-GoogleDoesntAppreciateFlutter_2.png)

그런데 플러터에 모든 주목을 받는 것에도 불구하고, 구글이 그들이 손에 쥔 것을 정말로 가치 있게 여기지 않는 것 같아요. 개발자 기조를 보는데, 그들은 플러터에 대해 이야기하기 시작한 건 45분 정도가 지난 후였어요. 그리고 이야기를 시작하자마자 2분도 안 되어 씩 간단히 소개하고 그 이후에 새로운 파이어베이스 로고가 얼마나 멋지다고 말하기 시작했어요.

![이미지](/assets/img/2024-06-19-GoogleDoesntAppreciateFlutter_3.png)

<div class="content-ad"></div>

플러터가 계속해서 코틀린 멀티플랫폼에 대해 이야기할 때 플러터가 충분한 시간을 얻지 못한 것 같아요. 계속해서 그 내용을 살펴보고 있었는데, “코틀린 멀티플랫폼에 대해 아직도 이야기를 다 끝내지 않았나요?”라고 생각했어요. 그 내용을 다루는데 더 많은 주목을 받은 것은 Gemini 뿐이에요.

그래, Gemini는 멋지긴 한데, 이에 대해 많은 시간을 할애해야 한다고는 생각하지 않아요. 특히 Gemini가 하는 대부분의 것들이 그다지 인상적이지 않다는 점에서요.

Gemini가 Firebase Cloud Functions 로그를 설명하도록 하는 버튼이 있는 건 알고 계시나요? 그리 유용하지 않아요. 제가 경고를 설명하도록 요청했는데, 경고에 대한 정보를 알려줄 줄 알았는데 아니었어요. 대신 그 경고를 발생시킨 IP 주소만 알려주더라구요.

그리고 Gemini는 아직도 주요 환각 문제를 가지고 있어요. 심지어 AI 환각으로 인한 취약점까지 생겼다고 하더라구요.

<div class="content-ad"></div>

아마 그렇게 나쁘지는 않았습니다. 지난 해에 플러터에 더 많은 시간을 할애했고, 플러터 3.22의 변경 사항은 그다지 놀라운 것이 아닙니다. 대부분 웹 어셈블리와 성능 개선 뿐이에요. 하지만 그 성능 개선은 상당히 인상적입니다. iOS의 GPU 사용량과 전력 소비가 50% 감소했다고요? 그렇죠.

그리고 웹 어셈블리? 저는 웹 어셈블리에 대해 진지한 우려가 있어서 여기서 논의했습니다. 또한 플러터 문서를 찾다가 이것을 보았어요:

![이미지](/assets/img/2024-06-19-GoogleDoesntAppreciateFlutter_4.png)

iOS에서 무슨 문제인지 잘 모르겠네요. 모바일 Safari는 웹 어셈블리를 지원해야할 텐데 말이죠. 아마도 웹 GC(가비지 컬렉션) 관련 문제일까요? 어쨌든, iOS에서 지원하지 않는다면 큰 문제가 될 겁니다.

<div class="content-ad"></div>

그래도 Flutter는 더 많은 관심을 받아야 한다고 생각해요. 특히 개발자 키노트 전체에서 가장 큰 반응을 얻었을 때에. 구글 I/O 전체에 이루어진 것 같아요, 비록 전부 다는 보지 않았지만 말이에요.

그냥 구글이 Flutter를 충분히 인정하지 않는 것 같아요.

사람들은 종종 묻곤 해요, "만약 구글이 Flutter를 포기한다면 어떻게 될까?" 제 의견은 이미 알려졌죠. 그렇게 될 것 같진 않다고 생각해요. 하지만 구글이 Flutter를 포기한다 해도 그것은 Flutter를 없애지 않을 거에요. 그만큼 Flutter는 크고 이미 많은 팬들을 가지고 있거든요. 이렇게 말이에요.

개발자 커뮤니티에 이미 너무 깊게 뿌리내린 것 같아요. 도큐먼트가 어디서든 찾아볼 수 있어요. Swift나 Kotlin, Java 도큐먼트보다 더 많은 Dart 도큐먼트가 있어요. Flutter는 이미 인정받았어요.

<div class="content-ad"></div>

Google이 Flutter를 계속 추구할 가치가 없다고 결정하더라도 다른 누군가가 계속해서 작업을 이어나갈 것이라고 확신해요. 많은 사람들에게 너무 중요한 프로젝트이기 때문이죠.