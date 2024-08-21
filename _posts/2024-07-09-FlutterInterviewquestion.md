---
title: "Flutter 면접 질문 모음 2024년 최신 가이드"
description: ""
coverImage: "/assets/img/2024-07-09-FlutterInterviewquestion_0.png"
date: 2024-07-09 09:47
ogImage: 
  url: /assets/img/2024-07-09-FlutterInterviewquestion_0.png
tag: Tech
originalTitle: "Flutter Interview question:"
link: "https://medium.com/@arjunkurup/-cfd4817284a7"
isUpdated: true
---





![이미지](/assets/img/2024-07-09-FlutterInterviewquestion_0.png)

정렬된 두 정수 배열 nums1과 nums2, 그리고 nums1과 nums2 각각의 요소 수인 m과 n이 주어집니다.

nums1과 nums2를 비내림차순으로 정렬된 하나의 배열로 병합하세요.

```js
Input: nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
Output: [1,2,2,3,5,6]
```

<div class="content-ad"></div>

## 코드:

<img src="/assets/img/2024-07-09-FlutterInterviewquestion_1.png" />

설명:

1. 메소드 정의:

<div class="content-ad"></div>

<img src="/assets/img/2024-07-09-FlutterInterviewquestion_2.png" />

- List`int` nums1: 첫 번째 정수 List인 nums1을 선언합니다.
- int m: nums1의 요소 수(0이 아닌 값의 수)입니다.
- List`int` nums2: 두 번째 정수 List인 nums2를 선언합니다.
- int n: nums2의 요소 수(0이 아닌 값의 수)입니다.

nums1에서 범위 설정:

<img src="/assets/img/2024-07-09-FlutterInterviewquestion_3.png" />

<div class="content-ad"></div>

setRange(m, m + n, nums2): 이 함수는 nums2의 요소들을 nums1에 m 인덱스부터 복사합니다.

- m: nums1에서 nums2의 요소를 놓을 시작 인덱스(병합 시작 위치)입니다.
- m + n: nums1에서 nums2의 요소를 놓을 끝 인덱스입니다.

우리의 예시에서:

- m = 3, 이는 nums1에서 인덱스 3부터 nums2의 요소를 놓기 시작함을 의미합니다. ||모든 목록에서 인덱스는 0부터 시작합니다.
- m + n = 3 + 3 = 6, 이는 nums1에서 인덱스 6 이전까지 nums2의 요소를 놓기를 중단함을 의미합니다.

<div class="content-ad"></div>

그러니까, nums1.setRange(3, 6, nums2)는 nums2의 요소들을 nums1에 인덱스 3부터 6까지(6번째 요소는 제외) 복사합니다. 즉, nums1의 인덱스 3부터 6까지의 값을 nums2의 값으로 대체하는 것입니다.

실제적인 응용:

예를 들어, 위 예시에서 nums1.setRange(3, 6, nums2);를 실행하면, 다음과 같이 nums1이 수정됩니다:

![이미지](/assets/img/2024-07-09-FlutterInterviewquestion_4.png)

<div class="content-ad"></div>

오늘은 여기까지입니다.

만약 플러터에 대해 더 배우고 싶다면, 내일 다시 확인해 주세요. 우리는 그것의 기능에 대해 더 깊이 파헤치고 이 강력한 프레임워크의 더 흥미로운 측면을 탐구할 것입니다. 기대해 주세요!

<div class="content-ad"></div>

아르준 쿠럽 🙇🏻‍♂️