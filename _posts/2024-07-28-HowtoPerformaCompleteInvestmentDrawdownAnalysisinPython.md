---
title: "파이썬으로 투자 손실 분석을 완벽하게 수행하는 방법"
description: ""
coverImage: "/assets/img/2024-07-28-HowtoPerformaCompleteInvestmentDrawdownAnalysisinPython_0.png"
date: 2024-07-28 13:46
ogImage: 
  url: /assets/img/2024-07-28-HowtoPerformaCompleteInvestmentDrawdownAnalysisinPython_0.png
tag: Tech
originalTitle: "How to Perform a Complete Investment Drawdown Analysis in Python"
link: "https://medium.com/ai-advances/how-to-perform-a-complete-investment-drawdown-analysis-in-python-75d8fa92670c"
---


![이미지](/assets/img/2024-07-28-HowtoPerformaCompleteInvestmentDrawdownAnalysisinPython_0.png)

안녕하세요! 코르델 탠니(Cordell Tanny)는 24년 이상의 금융 서비스 경험이 있으며 양적 금융에 특화된 전문가입니다. 코르델는 이전에 캐나다 주요 기관에서 양적 분석가 및 포트폴리오 매니저로 일하며 20억 달러의 다자산 소매 투자 프로그램을 책임지기도 했습니다.

현재 코르델는 양적 금융 및 인공지능 솔루션 회사인 Trend Prophets의 대표이자 공동 창립자로 활동하고 있습니다. 또한 DigitalHub Insights의 이사로서 투자 관리에 AI를 소개하는 교육 자료를 제공하고 있습니다.

코르델는 맥길 대학에서 생물학 학사 학위를 받았으며 CFA 회원자 자격증, 금융 리스크 관리자 자격증, 그리고 금융 데이터 전문가 자격증을 보유하고 있습니다.

<div class="content-ad"></div>

trendprophets.com을 방문하여 더 많은 정보를 얻을 수 있어요.

이 기사에 대한 모든 Python 코드는 코드 템플릿에 등록하면 여기에서 찾을 수 있어요 (페이지 하단의 코드 템플릿 등록란에 있어요).

## Drawdown 분석

투자 성과를 평가하는 데 사용되는 매우 흔한 리스크 지표 중 하나인 최대 손실액을 계산할 수 있어요. 최대 손실액은 최고점과 최저점 사이에서 경험한 가장 나쁜 손실을 나타냅니다. 이는 투자가 가치를 회복하기 시작하고 결국 이전 최고점을 재차 달성하기까지의 과정을 의미해요. 간단한 용어로 말하면...