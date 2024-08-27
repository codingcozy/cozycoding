---
title: "플러터 vs 리액트 네이티브 성능 벤치마크 비교 정리"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-26 19:48
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Flutter Vs React Native  Performance Benchmarks you cant miss  "
link: "https://medium.com/@nateshmbhat/flutter-vs-react-native-performance-benchmarks-you-cant-miss-%EF%B8%8F-2e31905df9b4"
isUpdated: true
updatedAt: 1724743600413
---


모바일 앱 개발에서 Flutter와 React Native 사이를 선택하는 것은 주로 성능 고려사항에 달려있습니다. 두 프레임워크 모두 지지자들이 있지만, 실제 벤치마킹 결과가 많은 사람들에게 중요합니다.

Flutter와 React Native에 대한 벤치마킹 비교 자료가 많이 없어 놀랐습니다. 심지어 그 중에서도 꽤 오래된 자료들이었습니다. 그래서 Flutter 및 React Native의 최신 버전을 사용하여 앱 크기, 메모리 및 CPU 사용량 면에서 철저히 벤치마킹 비교를 진행했습니다. 결과는 흥미로우며 놀랍다고 해도 과언이 아닙니다.

# 벤치마킹된 앱

세 가지 유형의 앱이 벤치마킹되었습니다:

<div class="content-ad"></div>

- 1001개 항목을 포함한 대규모 Listview: 각 항목에는 정적 이미지와 무한 회전 이미지가 포함됩니다.
- 대규모 이미지 애니메이션: 200개의 이미지가 동시에 회전, 투명도 조절, 크기 조정됩니다.
- 대규모 Lottie 애니메이션: 단일 화면에 36개의 Lottie 애니메이션이 표시됩니다.

# 벤치마킹 방법론

- 각 벤치마크는 M1 Mac에서 수행되었습니다.
- OnePlus 7 및 Pixel 기기에서 Android 운영체제를 실행 중인 앱을 테스트했습니다. (곧 ios 벤치마크 결과를 게시할 예정입니다)
- 메모리 및 CPU 사용량은 Flutter 및 React Native 모두에 대해 릴리스 apk에서 Android Profiler를 사용하여 측정되었습니다.
- FPS는 Flutter 앱의 "프로필 모드"에서 측정되었고 React Native는 JS 최소화 및 개발 모드 비활성화 빌드에서 측정되었지만 메트로 번들러 연결이 필요합니다. (React Native 성능 통계는 메트로 번들러에 연결된 경우에만 지원됩니다)
- 벤치마크 소스 코드는 다음과 같습니다 : https://github.com/nateshmbhat/flutter-rn-performance-benchmarks
- 애니메이션에는 Flutter의 내장 Animation API 및 React Native의 산업 표준 Reanimated v3이 사용되었습니다.

## 사용된 프레임워크 버전:

<div class="content-ad"></div>

React Native: 0.74.1
Flutter: 3.19.5
Dart: 3.3.3

# Listview 성능

이 앱은 플러터에서 내장 ListView 및 Animated API를 사용합니다. React Native에서는 최신 Reanimated v3 라이브러리를 사용하고 내장 FlatList를 사용합니다. 두 목록 모두 명시적 최적화나 고정 높이 범위 없이 사용 중입니다.

플러터:

<div class="content-ad"></div>

- FPS: 60 (튕기지 않음)
- Dart 힙 메모리 사용량: 7–8 MB
- APK 크기: 16.8 MB (빌드하는 데 7.6초 소요)
- 프로세스 메모리: 120–130 MB (스크롤할 때 동일 유지)
- CPU: 5–8% (스크롤할 때 동일 유지)
- 빠른 스크롤링 시에도 튕김이나 프레임 드랍이 없음

React Native:

- FPS: 50–55 (느껴지는 튕김)
- APK 크기: 21.9 MB (빌드하는 데 23초 소요)
- 메모리: 160 MB (스크롤하지 않을 때), 180–190 MB (스크롤할 때)
- CPU: 11–13% (스크롤하지 않을 때), 25–30%까지 급등 (스크롤할 때)
- 가끔 프레임 드랍 및 스크롤 중 공백 항목 발생

Flutter는 스크롤할 때 메모리나 CPU 급증이 없고 부드럽게 작동하는 반면, React Native는 프레임 드랍이 발생하고 CPU 및 메모리 급증이 있었습니다.

<div class="content-ad"></div>

# 대량 이미지 애니메이션

플러터:

- FPS: 58–60
- Dart 힙: 13.4 MB
- APK 크기: 11.6 MB (빌드 시간 19.6 초)
- 메모리: 128–135 MB
- CPU: 8%

리액트 네이티브:

<div class="content-ad"></div>

- FPS: 58-60 (가끔 떨어짐)
- APK 크기: 21 MB (빌드 시간 20초)
- 메모리: 380-396 MB
- CPU: 12-16%

두 프레임워크는 유사한 FPS를 유지했지만 React Native은 애니메이션 재시작 시 FPS가 떨어지고 메모리 사용량이 매우 높았습니다.

# 대량 Lottie 애니메이션

Flutter:

<div class="content-ad"></div>

- FPS: 36
- APK size: 7.6 MB (20.8 초 소요)
- 메모리: 220 MB
- CPU: 11%

React Native:

- FPS: 43
- APK size: 18.5 MB (23 초 소요)
- 메모리: 240 MB
- CPU: 22%

이 테스트에서 흥미로운 점은 React Native이 FPS에서 Flutter를 능가했지만 CPU 및 메모리 사용량이 더 높다는 것입니다.

<div class="content-ad"></div>

# 스타터 프로젝트 비교

React Native나 Flutter에서 프로젝트를 처음 만들 때, 예상치 못한 결과로 React Native의 Apk 크기가 스타터 프로젝트에서 Flutter 앱보다 현저히 크다는 것을 발견하였습니다.

이 특정한 경우가 공정한 비교가 아닐 수 있다는 것을 염두에 두어야 하지만(두 스타터 앱이 다르기 때문), 성능 기준 결과로서 제외될 수는 있지만, 몇 가지 흥미로운 관찰을 제시합니다.

새 프로젝트를 설정할 때 React Native에서 전체 react-native 라이브러리가 다운로드되기 때문에 앱 생성 시간이 더 오래 걸리지만, 이미 플러터 SDK가 다운로드되어 있기 때문에 Flutter에서는 이미 준비되어 있습니다.

<div class="content-ad"></div>

# 최종 판결

## 다음 단계 :

- iOS 성능 벤치마킹 실행
- React Native의 새 아키텍처를 활성화하여 성능 벤치마킹 실행

# 다가오는 벤치마킹 포스트

<div class="content-ad"></div>

더 자세한 벤치마크 비교와 메모리 및 CPU 사용량에 대한 스냅샷 데이터를 곧 게시할 예정입니다.
아직은 iOS에서의 성능 비교를 게시하지 않았어요.
그 부분을 기대해주세요 !

![image](https://miro.medium.com/v2/resize:fit:1000/0*dYbJK5NYUGUlm5Xk.gif)