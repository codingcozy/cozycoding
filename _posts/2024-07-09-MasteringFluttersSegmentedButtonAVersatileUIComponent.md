---
title: "Flutter의 SegmentedButton 완전 정복 다재다능한 UI 컴포넌트 사용법"
description: ""
coverImage: "/uidev-css.github.io/assets/no-image.jpg"
date: 2024-07-09 09:48
ogImage: 
  url: /uidev-css.github.io/assets/no-image.jpg
tag: Tech
originalTitle: "Mastering Flutter’s SegmentedButton: A Versatile UI Component"
link: "https://medium.com/@gimhantharuke856/mastering-flutters-segmentedbutton-a-versatile-ui-component-dd7af757954f"
---


Flutter의 SegmentedButton은 개발자가 직관적이고 선택 가능한 버튼 그룹을 만들 수 있는 강력하고 유연한 위젯입니다. 날씨 앱, 설정 페이지 또는 사용자 선택이 필요한 인터페이스를 구축할 때 SegmentedButton은 깔끔하고 효율적인 해결책을 제공합니다. 이제 SegmentedButton의 기능과 구현 방법을 알아보겠습니다.

# SegmentedButton 이해하기

Flutter의 SegmentedButton 위젯은 각각이 다른 옵션을 나타내는 선택 가능한 세그먼트 행을 제공합니다. 특히 다음과 같은 경우에 유용합니다:

- 단일 선택 시나리오(예: 과일 유형 선택)
- 다중 선택 사례(예: 여러 야채 선택)
- 아이콘만 선택(예: 날씨 상황)

<div class="content-ad"></div>

# 주요 기능:

- 사용자 정의 가능한 외관
- 단일 및 다중 선택 지원
- 안전한 값 처리를 위한 enum과의 쉬운 통합

# 구현 예시

날씨 선택 인터페이스에 SegmentedButton을 사용하는 실용적 예시를 살펴봅시다:

<div class="content-ad"></div>

```js
열거형 Weather { 흐림, 비, 맑음 }

SegmentedButton<Weather>(
  selected: const <Weather>{Weather.rainy},
  onSelectionChanged: (Set<Weather> newSelection) {
    // 선택 변경 처리
  },
  segments: const <ButtonSegment<Weather>>[
    ButtonSegment(
      icon: Icon(Icons.cloud_outlined),
      value: Weather.cloudy,
    ),
    ButtonSegment(
      icon: Icon(Icons.beach_access_sharp),
      value: Weather.rainy,
    ),
    ButtonSegment(
      icon: Icon(Icons.brightness_5_sharp),
      value: Weather.sunny,
    ),
  ],
)
```

코드 설명:

- 우리의 선택지를 나타내기 위해 열거형 Weather를 정의합니다.
- selected 파라미터는 초기 선택을 설정합니다 (이 경우 비).
- onSelectionChanged는 새로운 선택을 다루는 콜백입니다.
- segments는 ButtonSegment 위젯의 목록이며, 각각 아이콘과 연관된 열거형 값으로 날씨 옵션을 나타냅니다.

## 사용자 정의 옵션

<div class="content-ad"></div>

SegmentedButton은 다양한 사용자 정의 기능을 제공합니다:

- 아이콘 옆에 레이블 추가
- 색상, 여백 및 기타 스타일 속성 조정
- 다중 선택 기능 활성화 또는 비활성화

## 권장 사항

- 옵션을 정의할 때 타입 안전을 위해 열거형 사용
- 선택된 상태에 대한 명확한 시각적 피드백 제공
- 아이콘만 있는 버튼에 대해 레이블 또는 툴팁 텍스트를 포함하여 접근성 고려

<div class="content-ad"></div>

## 결론

SegmentedButton 위젯은 Flutter의 UI 도구 중 다재다능한 도구입니다. 깔끔한 디자인과 유연성으로 다양한 선택 인터페이스에 적합합니다. 이 구성 요소를 숙달하면 Flutter 애플리케이션에서 더 직관적이고 상호작용적인 사용자 경험을 만들 수 있습니다.

특정 사용 사례에 가장 적합한 구성을 찾기 위해 다양한 설정을 실험하는 것을 잊지 마세요. 즐거운 코딩 되세요!