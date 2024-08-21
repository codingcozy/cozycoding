---
title: "데이터 전문가를 위한 쉽고 재밌는 파이썬 동시성 가이드"
description: ""
coverImage: "/assets/img/2024-07-27-PythonConcurrencyABrain-FriendlyGuideforDataProfessionals_0.png"
date: 2024-07-27 13:47
ogImage: 
  url: /assets/img/2024-07-27-PythonConcurrencyABrain-FriendlyGuideforDataProfessionals_0.png
tag: Tech
originalTitle: "Python Concurrency  A Brain-Friendly Guide for Data Professionals"
link: "https://medium.com/towards-data-science/python-concurrency-a-brain-friendly-guide-for-data-professionals-a6215a3e9e26"
isUpdated: true
---





![PythonConcurrencyABrain-FriendlyGuideforDataProfessionals_0](/assets/img/2024-07-27-PythonConcurrencyABrain-FriendlyGuideforDataProfessionals_0.png)

파이썬은 종종 가장 느린 프로그래밍 언어 중 하나로 비판을 받습니다. 이 주장은 일부 사실이 있지만, 파이썬이 처음 프로그래밍 언어를 배우는 사람들이 많기 때문에 대부분의 코드가 최적화되어 있지 않다는 점을 강조해야 합니다.

하지만 파이썬에는 몇 가지 요령이 있습니다. 동시 함수 실행을 활용하는 것은 구현하기 매우 간단하지만, 데이터 파이프라인의 실행 시간을 10배 감소시킬 수 있습니다. 몇 시간이 아니라 몇 분이 걸립니다. 무료로.

오늘은 파이썬에서 동시성이 어떻게 작동하는지 알아보고, 예외 처리, 사용자 정의 콜백 및 속도 제한 처리하는 방법도 배워보겠습니다. 자세히 살펴봅시다!


<div class="content-ad"></div>

# JSON Placeholder — 하루 데이터 원본

첫 번째 일은 데이터 원본을 설정하는 것입니다. 나는 독점적인 것이나 설치하기 오랜 시간이 걸리는 것을 피하고 싶었습니다. JSON Placeholder — 무료 REST API 서비스 — 가 완벽한 후보입니다.