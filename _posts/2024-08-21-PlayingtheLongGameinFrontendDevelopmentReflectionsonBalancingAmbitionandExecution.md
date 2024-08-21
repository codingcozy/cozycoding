---
title: "프론트엔드 개발자가 되려고 한다면"
description: ""
coverImage: "/assets/img/2024-08-21-PlayingtheLongGameinFrontendDevelopmentReflectionsonBalancingAmbitionandExecution_0.png"
date: 2024-08-21 18:55
ogImage: 
  url: /assets/img/2024-08-21-PlayingtheLongGameinFrontendDevelopmentReflectionsonBalancingAmbitionandExecution_0.png
tag: Tech
originalTitle: "Playing the Long Game in Frontend Development Reflections on Balancing Ambition and Execution"
link: "https://dev.to/eransakal/playing-the-long-game-in-frontend-development-reflections-on-balancing-ambition-and-execution-1d7d"
isUpdated: true
updatedAt: 1724245552768
---


소프트웨어 개발의 빠른 세계에서는 고객의 즉각적인 요구를 충족하는 동시에 장기적인 목표를 위해 건설하는 지속적인 과제가 있습니다. 하루하루에 빠져들기 쉽지만, 최종 목표를 항상 염두에 두는 것이 중요합니다.

### 시작하기 전에 - 독자들에게

이 글은 저(인간!)가 쓰고 AI 도구의 도움을 받아 깔끔하게 다듬은 것입니다. 제 소망은 이 글이 주로 동료인간들에게 읽혀지고 AI 에이전트만이 아니라는 것입니다 😆 AI의 도움은 가치 있지만, 진정한 마법은 인간의 연결에서 온다는 점을 잊지 말아야 합니다.

## 두 프로젝트, 하나의 여정

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

이번 달은 나에겐 흥미로운 이정표인데요: 내가 이끌었던 두 가지 주요 프로젝트가 완료되었습니다. 이 기사의 초안 동기는 이 업적을 디지턀로 축하하는 것이었어요. 그래서 이 기사는 이 두 프로젝트 뒤에 숨은 이야기에 초점을 맞추었습니다.

하지만 내 현명한 동료 스타스가 떠올려 준 것처럼, 실행 가능한 통찰력이 없는 기사는 불완전합니다. 그래서 나는 이 프로젝트로부터 얻은 주요 교훈들을 별도의 기사로 추출했는데, 여러분도 읽고 싶어할 것 같아요!

프로젝트 #1: 현대 프론트엔드로의 3년 여정

3년 전에 Kaltura는 이미 상당한 일일 사용자 트래픽을 다루고 있던 React 기반 애플리케이션을 보유한 회사를 인수했죠. 그리고 그것을 리드하여 프론트엔드 개발을 하게 된 것이죠.

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

저희의 즉각적인 목표는 명확했습니다: 안정성 향상, 성능 향상, 그리고 팀의 기술을 높이는 것이었습니다. 그러나 장기적인 비전은 야심찼습니다: 최신 기술을 기반으로 한 현대적이고 견고한 응용 프로그램으로 레거시 시스템을 완전히 대체하는 것이었습니다. 전형적인 MVP(Minimum Viable Product) 방식과 달리, 우리는 점진적인 반복을 할 여유가 없었습니다. 새로운 응용 프로그램은 신속하게 완전하게 기능하도록 제작되어야 했습니다.

우리는 멀티이어 저니를 시작했습니다. 두 개의 응용 프로그램을 병행하여 개발했습니다. 하나는 수백만 명의 일일 사용자들을 위한 좋은 경험을 보장하기 위해 집중적인 유지 보수와 개발이 필요한 취득한 응용 프로그램이고, 다른 하나는 처음부터 새롭게 구축된 응용 프로그램이었습니다. 우리는 이 새로운 응용 프로그램을 신속하게 출시하여, 레거시 시스템과 완전한 기능 동등성을 제공하도록 했습니다.

2021년부터 2024년까지의 프로젝트 기간 동안 레거시 시스템에 대한 의존도가 점진적으로 줄어드는 것을 보여주는 그래프입니다. 완전히 통합된 레거시 시스템에서 시작하여, 전략적인 이정표와 반복적 진전을 통해 우리는 새로운 응용 프로그램으로 천천히 이행했습니다. 스마트한 "다리" 생성, 코드베이스 분리, 새로운 기능 구현과 같은 주요 이벤트들이 이 감소에 기여하며, 2024년 8월까지 레거시 의존성이 완전히 제거되는 것으로 끝납니다. 이 시각적인 여정은 원활한 전환을 이루기 위해 필요한 신중한 계획과 실행을 반영합니다.

![이미지](/assets/img/2024-08-21-PlayingtheLongGameinFrontendDevelopmentReflectionsonBalancingAmbitionandExecution_0.png)

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

그래프를 보면, 며칠 전 팀인 스타스, 토르니케, 안나, 그리고 토비가 새 어플리케이션 코드베이스에서 획득한 어플리케이션의 3000개 파일을 삭제하면서 이 전환 작업을 완료했다는 주요한 성과를 이뤘습니다.

이 성공은 제품, 디자이너, 사용자 경험 디자이너, 품질 보증, 서버, 그리고 RTC 팀 등 여러 팀의 협력 노력이 요인이었습니다.

### 프로젝트 번호2: 팬데믹으로부터 탄생한 플랫폼

두 번째 프로젝트는 아직 초기 단계이지만, 실시간 경험을 제공하는 방식에 대한 주요한 변화를 대표합니다. 이 프로젝트 아이디어는 COVID-19 팬데믹의 도전기간 동안에 생겨났습니다. 세계가 온라인으로 전환됨에 따라, 우리 회사는 고객이 실제 대면 행사와 동등한 매력적인 가상 이벤트를 개최할 수 있도록 적응하고 고객에게 도움을 줘야 한다는 필요성을 인지했습니다.

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

2020년에는 이러한 온라인 협업을 위해 특별히 새로운 인프라를 구축하여 대응했습니다. 이를 통해 우리 제품에는 일반적으로 현장 행사에서만 볼 수 있던 기능들이 포함되었습니다 - 스폰서 부스, 병렬 트랙, 네트워킹 라운지, 실시간 반응, 인터랙티브 퀴즈, 조절된 이벤트 등 다양한 기능이 포함되었습니다.

초기 솔루션이 효과적이었지만, 기능을 계속 추가함에 따라 우리가 비전을 확장해야 할 필요성이 더욱 분명해졌습니다. 고객, 최종 사용자, 제품 팀, 개발자, 마케팅 및 전문 서비스를 고려하는 포괄적인 프레임워크를 만들어야 했습니다. 두 해 전에 제 매니저가 저에게 빠르고 매끄럽게 성장할 수 있는 그러한 프레임워크를 설계하고 실행하라는 도전을 했습니다.

이 프로젝트는 흥미진진한 도전이었는데, 극복해야 할 다양한 구조적 난관과 중요한 결정들이 있었습니다. 핵심 인프라를 재설계하고 개발자들이 사용하기 쉽도록 보장하는 것까지, 모든 단계가 배우고 혁신할 기회였습니다.

이 두 프로젝트가 앞으로 몇 년 동안 우리 회사의 제품과 성장에 어떤 영향을 미칠지 진심으로 기대됩니다. 이번 여정에서 얻은 교훈과 채택한 전략에 대해 궁금하다면, 저의 후속 기사를 확인해 주세요.そEn 함께 이번 여정에서 얻은 주요 요점들을 살펴보겠습니다.

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

그 다음에 또 만나요,
에란