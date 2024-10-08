---
title: "12로 만드는 셀프 호스팅 디지털 아이덴티티 구축 방법"
description: ""
coverImage: "/assets/img/2024-06-22-ICreatedAComprehensiveSelf-HostedDigitalIdentityfor12month_0.png"
date: 2024-06-22 15:56
ogImage: 
  url: /assets/img/2024-06-22-ICreatedAComprehensiveSelf-HostedDigitalIdentityfor12month_0.png
tag: Tech
originalTitle: "I Created A Comprehensive Self-Hosted Digital Identity for $12 month"
link: "https://medium.com/gitconnected/i-created-a-comprehensive-self-hosted-digital-identity-for-12-month-59bc3fe3d69f"
isUpdated: true
---





## 스마트 검색, 블로그, 뉴스레터, 분석, 제휴 마케팅, RSS 피드 — 모두 자체 호스팅되고 월 12달러 미만!

![이미지](/assets/img/2024-06-22-ICreatedAComprehensiveSelf-HostedDigitalIdentityfor12month_0.png)

지난 2개월 동안 나는 블로그 웹사이트, 뉴스레터 시스템, 웹 트래픽 분석, 제휴 마케팅, 스마트 AI 검색, 그리고 예전 방식의 RSS 피드를 모두 포함한 포괄적인 디지털 아이덴티티를 만들었습니다.

이들을 사용하던 다양한 프로프라이터리 도구는 월 100달러 이상 지출이었지만, 이제는 모든 것을 디지턈 오션 인스턴스에서 자체 호스팅하면 월 12달러로 가능합니다.

<div class="content-ad"></div>

지난 8주 동안 시스템을 구축해 왔는데, 다른 블로그 포스트에서 제가 배운 내용을 공유해 왔어요. 그런데 이번 블로그 포스트에서는 그 모든 것을 한데 모아서 큰 그림을 이해하는 데 도움이 되도록 할 거에요.

관심 있으시다면, 시작해볼까요?

# 웹사이트 스택

모든 것은 제 웹사이트 https://irtizahafiz.com에서 시작돼요.

<div class="content-ad"></div>

많은 다양한 기술이 이것을 만드는 데 사용되었습니다. 따라서 기술 스택을 나열하기 전에 카테고리화해 보겠습니다.

- 프론트엔드 — NextJS, ChakraUI
- 백엔드 — FastAPI, Postgres, Nginx
- 보안 — Nginx Rate Limits, Let’s Encrypt SSL Certificates
- 배포 — Nginx Reverse Proxy, Digital Ocean Droplet

곧 듣게 될 다른 모든 기술들과 함께, 저의 웹사이트는 Digital Ocean 인스턴스 안에서 자체 호스팅되어 있습니다.

NextJS 서버, 백엔드 API 엔드포인트, Postgres 데이터베이스를 포함해 모든 것이 동일한 머신에 있습니다.

<div class="content-ad"></div>

웹사이트는 서버 측에서 렌더링되어 SEO 이점이 매우 크며, 모든 콘텐츠는 정적으로 생성되므로 반응성이 뛰어납니다.

# 블로그 및 유튜브 인프라

대부분의 개인 웹사이트와 마찬가지로, 내 웹사이트의 주요 목적은 내가 게시하는 모든 콘텐츠의 홈으로 기능하는 것입니다.

구체적으로는 내 블로그 글과 유튜브 동영상입니다.

<div class="content-ad"></div>

나의 YouTube 동영상에 관한 경우, 제 모든 메타데이터 - 제목, 설명, URL -을 Postgres 데이터베이스에 저장합니다. 내 웹사이트에서는 동영상을 호스팅하는 대신 YouTube 동영상에 링크를 제공합니다. 그것이 더 합리적이라고 생각했거든요.

하지만, 블로그 글은 내가 직접 호스팅합니다.

내 NextJS 앱에서 모든 블로그 글은 Markdown 파일로 저장됩니다. 특별한 형식인 mdx를 사용하는데, 이것은 Markdown 파일을 업그레이드한 것으로 생각할 수 있습니다. 이 형식을 사용하면 코드 블록을 렌더링하거나 Markdown 콘텐트 내에 "좋아요" 카운터, 날짜 선택기 등과 같은 동적 React 컴포넌트를 넣을 수 있습니다.

빌드 시간에 NextJS 앱은 모든 블로그 콘텐츠를 정적으로 생성합니다.

<div class="content-ad"></div>

저는 이 설정이 매우 직관적이라고 생각해요. 일반적인 마크다운 파일에 작성하는 것은 매우 간단해요. NextJS는 내 마크다운 파일을 읽기 쉬운 웹페이지로 바꾸는 모든 번거로운 작업을 처리하기 때문에 게시하는 것은 아주 쉬워요.

Squarespace나 Ghost 같은 플랫폼이 제공하는 콘텐츠 관리 시스템(CMS)이 없으면, 이 정도면 충분하다고 생각해요.

# 뉴스레터 및 메일링 리스트 관리자

자체 호스팅 여정을 시작할 때, 무료 뉴스레터나 메일링 리스트 관리자를 만드는 것이 가장 복잡할 것이라는 것을 알고 있었어요.

<div class="content-ad"></div>

좋은 예로는 Ghost, Squarespace, MailChimp 등의 플랫폼이 있습니다. 구독/해지 양식, 규정 준수 알림, 거부 링크, 템플릿, 캠페인 일정 등을 처리해줍니다.

비슷한 기능 세트를 갖춘 것을 0부터 만드는 것은 고려할 수 없었습니다.

여기 Listmonk이 나왔습니다.

Listmonk은 자체 호스트할 수 있는 무료 오픈 소스 뉴스레터 및 메일링리스트 관리자입니다.

<div class="content-ad"></div>

요즘 여기에 관해 글을 썼어. 관심이 있으면 참고해봐. 간단히 말하자면, 1만 명의 구독자 이메일 뉴스레터 시스템을 호스팅하는 데 필요한 기능의 90%를 제공해줘. Digital Ocean 인스턴스에서 호스팅 중이고, 따라서 비용은 $0이야.

# 웹 분석

웹사이트를 만드는 것도 중요하지만, 성능을 측정하고 사용자 행동을 최적화하는 것도 중요해.

좋은 분석 도구가 없으면 올바른 컨텐츠가 올바른 사람들에게 제공되는지 판단하기 어려워. 이런 사용자 통찰력을 제공하는 가장 일반적인 도구는 Google Analytics야.

<div class="content-ad"></div>

하지만 여러 가지 이유로 인해 - 이곳과 이곳에서 썼던 것들에 대해 썼었지만 - 저는 대신에 비공개, 무료, 그리고 오픈 소스 대안을 선택했습니다.

우마미(Umami)를 소개합니다.

Listmonk과 유사하게, 우마미도 전통적으로 필요한 기능의 거의 90%를 제공하며 완전한 개인 정보 보호를 유지할 수 있고, 비용은 $0입니다.

이것을 통해 웹사이트 성능과 고객 상호 작용을 추적할 수 있으며, 전환 퍼널을 측정하는 데 사용하는 사용자 정의 이벤트를 추적할 수 있습니다.

<div class="content-ad"></div>

모든 고객 데이터는 내 디지털 오션 서버에 호스팅된 포스트그레스 데이터베이스에 로컬로 저장되어 있습니다. 따라서 개인 정보 보호와 안전이 모두 보장됩니다.

# 스마트 검색

지금까지 관심 가져 주셔서 감사합니다. 이제 여러분과 공유할 가장 흥미로운 기능이 있어요.

사용자가 나의 모든 블로그와 유튜브 비디오를 검색할 수 있는 Open AI 기반의 스마트 검색입니다.

<div class="content-ad"></div>

저는 이 주제에 대해 몇 가지 깊이 있는 조사를 했어요: '실시간 스마트 검색 파이프라인 구축하기'와 'OpenAI 임베딩 및 Postgres 벡터를 사용한 AI 검색 기능 구축하기'에 대해 찾아보고 싶다면요.

위에서 설명한 모든 것과 비슷하게, 이 기능을 지원하는 모든 인프라는 무료로 동일한 Digital Ocean 클러스터에 호스팅되어 있어요.

다만 하나의 비용 구성 요소가 있습니다 — OpenAI의 임베딩 API를 사용하는 데 필요한 $$$요.

보통 몇 센트밖에 안 돼요. 이를 더 줄이기 위해, 파이프라인의 각 단계에 캐시를 구현했어요. 이 캐시 또한 제 서버에 로컬로 호스팅되어요.

<div class="content-ad"></div>

모든 기능 중에서도 "스마트 검색" 기능이 가장 기쁨을 주었어요. 그래서 여러분이 이것을 시도해 보길 원해요. https://irtizahafiz.com 에서 확인해보세요.

# RSS 피드

그럼 이것으로 하나의 간단하지만 강력한 기능인 RSS 피드를 마무리하겠습니다.

저는 RSS 피드의 열렬한 팬이에요.

<div class="content-ad"></div>

알고리즘에 따르기보다는, 내가 존경하는 사람들의 콘텐츠를 명시적으로 따르는 것을 선호해요. 그리고 그들의 블로그를 내 RSS 피드에 추가해요.

읽을 시간이 생기면, 알고리즘에서 주는 것이 아니라 내가 의도적으로 RSS 피드에서 가져와요.

그래서 내 블로그 글 전체를 포함한 RSS 피드를 공개하는 것이 중요했어요.

python-feedgen을 사용하여 RSS 피드를 정의하는 XML 파일을 구성했어요. 이 피드는 내 로컬에 호스팅된 모든 Markdown 파일을 위해 만들어졌어요.

<div class="content-ad"></div>

매번 새 블로그 글을 게시할 때마다, 제 서버에서 스크립트가 실행되어 RSS 피드를 업데이트하고 파일을 https://irtizahafiz.com/feed.xml에 게시합니다.

원하시는 RSS 피드 리더에 피드를 가져올 수 있습니다.

# 마무리

이것까지 읽어주셨다면, 유익하게 받아들여 주셨으면 좋겠습니다.

<div class="content-ad"></div>

월 12달러 미만으로 이러한 포괄적인 디지털 존재를 만들어 나가는 것이 정말 즐겁습니다.

제품군을 전부 만드는 것이 흥미로웠을 뿐만 아니라 블로그, 뉴스레터, 분석, 스마트 검색, RSS 피드까지 자체 호스팅하고 개인 정보 보호에 중점을 둔 기술을 사용할 수 있다는 것도 멋졌습니다.

저를 따라와 주실 수 있는 몇 가지 방법이 있습니다: Medium에서 나를 팔로우하거나, 웹사이트를 구독하거나, YouTube에서 나를 팔로우해주세요.