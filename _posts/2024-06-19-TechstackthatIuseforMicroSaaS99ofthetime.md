---
title: "나의 Micro SaaS를 위해 99 시간동안 사용하는 기술 스택"
description: ""
coverImage: "/assets/img/2024-06-19-TechstackthatIuseforMicroSaaS99ofthetime_0.png"
date: 2024-06-19 00:23
ogImage: 
  url: /assets/img/2024-06-19-TechstackthatIuseforMicroSaaS99ofthetime_0.png
tag: Tech
originalTitle: "Tech stack that I use for Micro SaaS 99% of the time!"
link: "https://medium.com/@shivanshudev/tech-stack-that-i-use-for-micro-saas-99-of-the-time-2aebc9cb6922"
isUpdated: true
---





현재 많은 기술이 사용 가능하며, 매달 새로운 JS 프레임워크가 출시됩니다. 때로는 처음부터 올바른 기술 스택을 선택하지 않으면 나중에 확장하기 어려울 수 있습니다. 그래서 오늘의 글에서는 제가 Micro SaaS를 개발하는 데 사용하는 기술과 스택을 공유하겠습니다.

하지만 시작하기 전에 제가 쓴 "SaaS 건설을 위한 치트 코드"라는 책을 공유하고 싶습니다. 이 책은 성공적인 SaaS 기초를 구축하고 운영하기 위해 필요한 모든 요구 사항과 단계를 강조합니다.

저는 프론트엔드 개발, 백엔드 개발, 데이터베이스, 결제, 클라우드, 보안으로 모든 것을 세분화하여 이해하기 쉽도록 했습니다. 모두 한 가지씩 확인해보겠습니다.

<div class="content-ad"></div>

# 프론트엔드 개발

프론트엔드 개발에서는 대부분 Vite와 React JS를 사용하는 것을 선호합니다. 때로는 Next JS를 사용하여 프로젝트를 구성하기도 합니다. 그러나 대부분의 프로젝트에서 window, location 등과 같은 네이티브 JS 라이브러리가 필요합니다.

또한 간편함을 위해 MUI에서 미리 만들어진 템플릿을 사용하여 많은 시간을 절약했습니다. 저는 프론트엔드에 능숙하지 않으며 SaaS의 MVP 버전을 만들고 아이디어를 검증하고 싶어합니다. 모든 것이 순조롭고 SaaS가 잠재력을 가지고 있다고 느낄 때 스타일링을 더 발전시킬 수 있습니다.

![image](/assets/img/2024-06-19-TechstackthatIuseforMicroSaaS99ofthetime_1.png)

<div class="content-ad"></div>

대부분의 경우에는 개발 중에 ESLint의 체크를 비활성화합니다. 원하는 경우 체크를 유지하여 깨끗하고 좋은 코드를 작성할 수 있습니다.

# Backend Development

이제 백엔드 개발에서, 많은 기능을 갖춘 복잡한 SaaS 프로젝트를 구축할 때 Nest JS를 사용하게 될 것입니다. Nest JS는 Express JS의 심화 버전입니다.

![이미지](/assets/img/2024-06-19-TechstackthatIuseforMicroSaaS99ofthetime_2.png)

<div class="content-ad"></div>

프레임워크는 Express JS를 사용하는 것보다 신뢰성이 높고 확장성이 더 좋아요. 하지만 기능이 적은 SaaS를 개발 중이거나 Micro SaaS를 개발 중이라면 Node JS - Express JS 프레임워크를 사용해 볼 수 있어요. 

저는 Micro SaaS에서 코드를 작성할 때 JS를 선호하고, 기능이 많고 Micro SaaS보다 큰 프로젝트에는 TypeScript를 사용하곤 해요. 

가끔 CRUD 작업을 빠르게 처리하고 시간을 절약하기 위해 Strapi와 같은 헤드리스 CMS도 사용해요.

![TechstackthatIuseforMicroSaaS99ofthetime_3.png](/assets/img/2024-06-19-TechstackthatIuseforMicroSaaS99ofthetime_3.png)

<div class="content-ad"></div>

또한 Stripe와 Mailchimp와 같은 다른 써드 파티와의 통합을 제공하여 작업과 시간을 절약할 수 있습니다.

# 데이터베이스

데이터베이스로는 대부분의 프로젝트에 MongoDB를 사용하고, 사용자 활동을 수집해야 하는 경우에는 PostgreSQL을 선호합니다.

캐싱이 필요한 경우 Redis를 사용하며, PlanetScale, Redis.com, MongoDB.com과 같은 공급업체에서 데이터베이스를 구하고, Kafka와 같은 큐 서비스가 필요한 경우에는 Upstash를 선호합니다.

<div class="content-ad"></div>

# 결제

구독과 결제에 대해서는 항상 Stripe를 사용합니다. 더 복잡한 세금 문서 작성이 필요하다면, Lemon Squeezy를 사용해보실 수도 있어요. 저는 지난 프로젝트에서 사용해보았는데 도움이 되었어요.

# 호스팅 및 배포

저는 좋은 DevOps 엔지니어가 아니기 때문에, 배포에는 Heroku, Render, 그리고 Firebase와 같은 플랫폼을 주로 사용하는 편입니다.

<div class="content-ad"></div>

저는 클라이언트의 대규모 프로젝트를 처리할 때 Azure와 GCP를 사용해 왔습니다. 이 프로젝트들에는 그들의 크레딧도 함께 사용했습니다.

하지만 여러분의 SaaS 제품이 MVP 단계이고 복잡한 아키텍처가 필요하지 않다면, 프론트엔드 배포에는 Vercel이나 Netlify, API/백엔드 배포에는 Heroku나 Render, 클라우드 스토리지에는 Wasabi, 그리고 서버리스에는 Firebase Cloud Function이나 Vercel Edge Functions를 사용해 볼 수 있습니다. Azure나 AWS보다 덜 복잡하며 모든 것을 한 곳에서 관리하고 싶다면 Digital Ocean도 좋은 선택입니다.

AI 모델 및 배포에 대해서는 Runpod를 선호합니다.

<div class="content-ad"></div>

이외에 오픈 AI API를 사용하고 싶다면, Azure에서 크레딧을 사용하여 사용할 수도 있습니다. 또는 다른 재구축 모델을 사용하려면 Together.ai를 사용합니다.

# 보안

내 SaaS의 보안을 위해서는 먼저 데이터베이스가 공개되지 않도록 해야 합니다. 그리고 주로 보안을 위해 Cloudflare를 사용하여 SaaS를 다양한 공격과 침투로부터 보호합니다.

이외에도 로그인 및 모니터링을 위해 NewRelic 및 Site 24x7을 사용하여 로그와 알림을 유지합니다. 또한 SaaS의 로그 및 알림을 관리하고자 한다면, 제 블로그의 글을 자유롭게 확인해주세요.

<div class="content-ad"></div>

이것은 여러분이 따를 수 있는 기술 스택에 대한 좋은 아이디어를 줄 수 있지만, SaaS에 대한 기술 요구 사항을 잘 조사한 후에 어떤 결정을 내리기 전에 꼭 확인하세요.

그리고, 만약 마케팅에 관해 깊이 파고들고 SaaS에 장기적인 유료 사용자를 확보하는 방법을 알고 싶다면, "SaaS 성장을 위한 마케팅 전략"이라는 제 책을 읽어보세요. 이 책은 SaaS를 처음부터 시작하여 실제 유료 사용자들을 확보하는 데 도움이 되는 필요한 모든 단계와 전략을 깊이 있게 다룰 것입니다.