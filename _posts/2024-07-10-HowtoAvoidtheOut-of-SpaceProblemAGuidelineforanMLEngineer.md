---
title: "ML 엔지니어를 위한 공간 부족 문제 해결 가이드 실용적인 방법"
description: ""
coverImage: "/blocktong.github.io/assets/no-image.jpg"
date: 2024-07-10 03:24
ogImage: 
  url: /blocktong.github.io/assets/no-image.jpg
tag: Tech
originalTitle: "How to Avoid the Out-of-Space Problem: A Guideline for an ML Engineer"
link: "https://medium.com/codenlp/free-up-your-disk-space-regularly-guideline-for-an-ml-engineer-c1a9eb94439b"
isUpdated: true
---




체크포인트, 도커 이미지, Python 환경, HF 모델 및 pip 캐시는 시간이 지남에 따라 증가하고 여러분이 완전히 인식하지 못한 채로 더 많은 디스크 공간을 차지할 수 있습니다. 용량 부족 문제에 빠지지 않도록 주의하세요.

# 소개

하드 드라이브 크기가 얼마나 크든, 언젠가는 용량 부족 문제에 직면할 것입니다. 머신러닝 엔지니어나 데이터 과학자로서, 홈 폴더, 시스템 구조 또는 작업 공간의 특정 위치는 시간이 지남에 따라 증가합니다. 이들은 주로 작업을 가속화하는 캐시이거나 특정 데이터에 대한 전역 위치입니다. 이야기에서는 디스크에서 일부 공간을 확보하기 위해 확인하고 검사해야 할 몇 가지 일반적인 위치를 정리했습니다.

아래는 큰 그림을 보여주기 위해 여러 달 동안 별다른 주기적인 정리 없이 작업한 후에 내 공간 사용량을 분석한 것입니다.

<div class="content-ad"></div>


600G - model checkpoints
290G - Docker images, container, and cache
231G - ~/miniconda3/envs
87G - ~/.cache/huggingface
41G - ~/miniconda3/pkgs
15G - ~/.cache/pip


다음은 동일한 내용을 파이 차트로 표시한 것입니다:
