---
title: "개발자를 위한 SEO 친화적인 페이지네이션 가이드 팁, 트릭, 그리고 함정들"
description: ""
coverImage: "/assets/img/2024-07-12-DevelopersGuidetoSEO-FriendlyPaginationTipsTricksandTraps_0.png"
date: 2024-07-12 22:03
ogImage: 
  url: /assets/img/2024-07-12-DevelopersGuidetoSEO-FriendlyPaginationTipsTricksandTraps_0.png
tag: Tech
originalTitle: "Developer’s Guide to SEO-Friendly Pagination: Tips, Tricks, and Traps"
link: "https://medium.com/code-like-a-girl/developers-guide-to-seo-friendly-pagination-tips-tricks-and-traps-619d61b8afbe"
isUpdated: true
---




안녕하세요, 내 신뢰 있는 개발자 친구들! 오늘은 페이지네이션의 매혹적인 세계로 뛰어들어 보려고 해요.

개발 영역에서 왜 이 주제가 중요한지 궁금할 수 있어요. 그래서 함께 알아보고 이야기해 볼게요:

- 페이지네이션이란 무엇인지,
- SEO에 미치는 영향, 그리고
- 왜 올바르게 구현하는 것이 중요한지

# 그렇다면, 페이지네이션은 정확히 무엇일까요?

<div class="content-ad"></div>

웹사이트의 콘텐츠를 여러 페이지로 나누는 것을 말해요. 뉴스 사이트, 블로그, 포럼, 거의 모든 전자 상거래 세계에서 볼 수 있어요.

![pagination image](/assets/img/2024-07-12-DevelopersGuidetoSEO-FriendlyPaginationTipsTricksandTraps_0.png)

페이지네이션은 웹사이트 콘텐츠 배포의 중요한 부분인데, 그래도 모든 게 환호와 무지개만 있는 것은 아니에요.

페이지네이션을 제대로 설정하는 것이 SEO에 중요해요.

<div class="content-ad"></div>

여기 정보가 있어요: 페이지네이션 설정의 품질은 검색 엔진이 페이지와 그 안에 포함된 콘텐츠 - 링크, 기사, 제품 등 - 을 어떻게 색인하는지 결정할 수 있는 중요한 요소입니다.

페이지네이션의 예시를 살펴보겠습니다. 이 예시에서:

- 시리즈 이름은 "멋진 레시피 시리즈"이며, URL과 관련 이미지가 있습니다.
- "페이지 1"과 "페이지 2"라는 두 페이지가 포함되어 있으며, 각각의 URL과 이름이 있습니다.
- 메타 태그에서 포지션 및 콘텐츠 속성을 복제하여 구조를 증가시키고 추가 페이지를 추가할 수 있습니다.
- 페이지네이션 메타데이터는 10페이지가 있고, 페이지 1부터 시작하여 페이지 10에서 끝나며, 현재 페이지는 1페이지임을 명시합니다.

```js
<div itemscope itemtype="http://schema.org/Series">
<span itemprop="name">멋진 레시피 시리즈</span>
<link itemprop="url" href="https://example.com/awesome-recipes">
<link itemprop="image" href="https://example.com/awesome-recipes-thumbnail.jpg">
<! - 페이지 1 →
<div itemprop="hasPart" itemscope itemtype="http://schema.org/WebPage">
<a itemprop="url" href="https://example.com/awesome-recipes/page/1">
<span itemprop="name">멋진 레시피 - 페이지 1</span>
</a>
<meta itemprop="position" content="1">
</div>
<! - 페이지 2 →
<div itemprop="hasPart" itemscope itemtype="http://schema.org/WebPage">
<a itemprop="url" href="https://example.com/awesome-recipes/page/2">
<span itemprop="name">멋진 레시피 - 페이지 2</span>
</a>
<meta itemprop="position" content="2">
</div>
<! - 여기에 더 많은 페이지 추가 →
<! - 페이지네이션 메타데이터 →
<meta itemprop="numberOfPages" content="10">
<meta itemprop="pageStart" content="1">
<meta itemprop="pageEnd" content="10">
<meta itemprop="currentPage" content="1">
</div>
```

<div class="content-ad"></div>

# 무한 스크롤은 무엇인가요?

이것은 페이지 네비게이션의 정반대입니다. 콘텐츠를 별도의 페이지로 나누는 대신 무한 스크롤은 모든 내용을 한 페이지에 촘촘히 담아 사용자가 끝없이 스크롤할 수 있도록 합니다.

![이미지](/assets/img/2024-07-12-DevelopersGuidetoSEO-FriendlyPaginationTipsTricksandTraps_1.png)

가끔 "더 보기" 버튼을 클릭하여 다음 일괄 콘텐츠를 공개할 수도 있지만 기술적으로는 여전히 한 페이지 안에 모든 내용이 들어있습니다.

<div class="content-ad"></div>

검색 엔진은 페이지네이션과 무한 스크롤을 단일 페이지로 간주합니다.

무한 스크롤은 세련된 모습과 모바일 친화성 같은 이점이 있지만, SEO 측면에서는 마법같이 좋은 방법은 아닙니다.

요점은 다음과 같습니다: 구글봇은 스크롤 동작을 모방할 수 없으며, 확실히 "더 보기"를 클릭할 수도 없습니다. 따라서 검색 엔진은 무한 스크롤 페이지의 모든 콘텐츠를 효과적으로 평가하고 색인화하기 어렵습니다.

페이지네이션은 이러한 문제가 없습니다. 왜냐하면 크롤러가 각 페이지를 독립적인 엔티티로 처리하기 때문입니다.

<div class="content-ad"></div>

# 개발자를 위한 페이징 최상의 실천 방법 안내

이제 구글의 지침대로 페이징의 최상의 실천 방법에 대해 이야기해 봅시다.

1. 페이지 링크 순서

- 매 페이지마다 `a href` 태그를 사용하여 다음 페이지로 연결되는 링크를 항상 포함하세요.
- 또한, 모든 페이지에서 페이징 내에 첫 번째 페이지로 이어지는 링크를 제공하여 검색 엔진이 SERP에서 원하는 결과를 선호하도록 해주세요.

<div class="content-ad"></div>

2. 올바른 URL 생성하기

- URL에 현재 페이지 번호를 보여주기 위해 파라미터 ?page=n을 사용하세요.
- 첫 번째 페이지를 근원적이라고 참조하지 마세요; 각 페이지에 독립적인 링크를 가진 캐노니컬 태그를 사용하세요.

3. 데이터 구조

- 사이트에서 페이지네이션을 사용한다는 것을 검색 엔진에 알리기 위해 마이크로 마크업을 활용하세요. Schema.org가 여기서 도움이 될 것입니다.

<div class="content-ad"></div>

4. 중복 텍스트 피하기

- 페이지네이션 중 카테고리에 소개 텍스트가 있다면, 첫 페이지에 넣어주세요.

5. URL 단편 식별자에 거부 의사 표명하기

- Google은 URL의 단편식별자(#)을 무시합니다. 링크가 크롤링될 수 있도록 해주세요.

<div class="content-ad"></div>

6. Rel="next" 및 Rel="prev" 태그

- Google은 2011년에 소개된 이 태그를 더 이상 요구하지 않습니다. 마이크로 마크업이 충분합니다.

보너스 팁

페이지네이션을 더욱 강력하게 만들고 싶나요? 다음 추가 기능을 고려해보세요:

<div class="content-ad"></div>

- preload, reconnect, 또는 prefetch 태그를 사용하여 사용자의 페이지 전환 속도를 높이세요.
- "다음/이전" 및 "페이지 번호"와 같은 두 가지 시각적 페이지네이션 유형이 있습니다. 내용 양에 가장 적합한 것을 선택하세요.

# 무한 스크롤 처리하는 개발자 안내서

무한 스크롤에 관심이 있다면, 그것은 주로 AJAX 기술로 구동됨을 기억하세요.

검색 엔진은 사용자가 스크롤하거나 클릭하여 더 많은 내용을로드 할 때까지 숨겨져 있기 때문에 일부 콘텐츠를 놓칠 수 있습니다.

<div class="content-ad"></div>

여기 JavaScript 조각을 보여드릴게요:

```js
window.onscroll = function() {
    // 사용자가 페이지 아래로 스크롤했는지 확인합니다.
    if (window.innerHeight + window.scrollY >= document.body.offsetHeight) {
        // 페이지의 맨 아래에 도달했습니다. 여기서 추가 콘텐츠를 로드하세요.
        // 여기서 AJAX 요청을 보내서 추가 콘텐츠를 가져올 수 있습니다.
        // 새로운 콘텐츠를 로드한 후 페이지의 기존 콘텐츠에 추가할 수 있습니다.
        // 필요하다면 오류 처리 및 로딩 지표를 구현하는 것을 잊지 마세요.
    }
};
```

이 코드에서:

- window.onscroll은 사용자가 스크롤할 때 함수를 트리거하는 이벤트 리스너입니다.
- window.innerHeight는 브라우저 뷰포트의 높이를 나타냅니다.
- window.scrollY는 페이지가 세로로 얼마나 스크롤되었는지를 나타냅니다.
- document.body.offsetHeight는 문서의 총 높이를 제공합니다.

<div class="content-ad"></div>

window.innerHeight와 window.scrollY의 합이 document.body.offsetHeight와 같거나 초과하면 사용자가 페이지 하단으로 스크롤했다는 의미이며, 추가 콘텐츠를 로딩할 수 있습니다.

// Load more content here 주석을 새 콘텐츠를 로드하고 웹페이지에 추가하는 실제 코드로 대체해 주세요.

또한 스크롤 이벤트에 지나치게 호출되는 것을 방지하고 성능을 향상시키기 위해 디바운스 또는 스로틀 메커니즘을 추가하는 것이 좋습니다.

또한, 콘텐츠를 관리 가능한 섹션으로 나누는 것에 대해 이야기해야 합니다. JavaScript가 비활성화되어 있어도 접근성을 보장하고 URL 무결성을 유지할 수 있습니다. 여기에 대해 자세히 알아보겠습니다:

<div class="content-ad"></div>

1. 웹 접근성을 위해 컨텐츠를 나눠주세요

무한 스크롤을 구현할 때, 컨텐츠를 작고 접근 가능한 섹션으로 나누는 것이 중요합니다. 이렇게 하면 JavaScript가 비활성화된 사용자도 내용에 원활하게 접근하고 탐색할 수 있습니다. 이러한 섹션을 중복해서 만들지 마세요. 다음 예를 고려해보세요:

예시 1: site.com/stickers&page=1

- 이 URL 구조는 "스티커" 카테고리 내 첫 번째 컨텐츠 페이지를 나타냅니다.

<div class="content-ad"></div>

예시 2: site.com/stickers?lastid=123

- "lastid"와 같은 고유 식별자를 사용하면 URL의 고유성을 유지하여 사용자가 특정 섹션에 직접 액세스할 수 있습니다.

예시: site.com/stickers#1

- URL 조각 식별자(#)를 사용하는 것은 SEO에 이상적이지는 않지만 약간의 접근성을 제공합니다. 그러나 가능한 경우 쿼리 매개변수나 깔끔한 URL을 사용하는 것이 더 나은 방법입니다.

<div class="content-ad"></div>

2. URL 고유성 및 기능성 보장

귀하의 URL은 고유하고 기능적이어야 합니다. 사용자 쿠키나 브라우징 기록에 독립적으로 작동해야 합니다. 이를 통해 원활한 사용자 경험과 검색 엔진에 의한 적절한 색인이 보장됩니다.

여기서 중요한 것은 예시입니다:

고유하고 기능적인 URL: site.com/stickers&page=2

<div class="content-ad"></div>

- "stickers" 카테고리의 두 번째 페이지를 참조하는 이 URL은 독특하면서 기능적입니다.

URL 프래그먼트 식별자 피하기:

- site.com/stickers#1과 같은 프래그먼트 식별자에만 의존하지 말고, 기능 및 색인화에 있어 신뢰성이 떨어질 수 있습니다.

3. replaceState 및 pushState로 사용자 경험 향상하기

<div class="content-ad"></div>

웹사이트 사용자의 행동에 따라 사용자 경험을 더 향상시키려면 replaceState 및 pushState를 사용해 보세요:

- replaceState: 사용자의 클릭 또는 스크롤과 유사한 작업이 있을 때 이 방법을 사용합니다. 이 방법은 새 항목을 만들지 않고 브라우저 기록의 현재 URL을 교체합니다.
- pushState: 사용자가 이전에 스크롤한 콘텐츠로 쉽게 돌아갈 수 있기를 원할 때 이 방법을 사용합니다. 이는 브라우저 기록에 새 항목을 추가합니다.

다음은 간단한 예시입니다:

```js
// pushState 사용
history.pushState({ page: 'scroll' }, '스크롤된 콘텐츠', '/teams/giraffes?page=2');
// replaceState 사용
history.replaceState({ page: 'scroll' }, '스크롤된 콘텐츠', 'teams/giraffes?page=3');
```

<div class="content-ad"></div>

이러한 방법을 전략적으로 활용하여 무한 스크롤을 구현할 때 더 원활하고 사용자 친화적인 경험을 제공할 수 있습니다.

요약하면, 무한 스크롤을 최적화할 때 접근성, URL 구조 및 상태 조작을 통한 사용자 경험 개선을 고려해야 합니다.

이 접근 방식은 JavaScript를 비활성화한 사용자를 포함하여 모든 사용자가 내용을 효과적으로 탐색할 수 있도록 보장하면서 SEO 및 사용자 참여를 향상시킵니다.

# 피해야 할 흔한 페이징 오류

<div class="content-ad"></div>

마지막으로, 이 흔한 페이지네이션 함정을 피하세요:

- “noindex” 태그 대신에 canonical 태그를 사용하면 검색 엔진이 페이지를 올바르게 색인하는 것을 방지할 수 있습니다.
- 페이지네이션을 위한 정적 URL은 피해야 합니다. URL 내 동적 매개변수를 사용해야 합니다.
- 현재 카테고리에 속하지 않는 페이지는 404 오류를 반환해야 합니다.

# 요약하자면

적절하지 않은 페이지네이션의 주요 문제는 콘텐츠에 대한 접근이 제한된 것입니다. 이 함정을 피하기 위해 이 황금 규칙을 기억하세요:

<div class="content-ad"></div>

- 크롤러가 액세스할 수 있는 사용자 친화적인 href 링크를 사용하여 페이징 테이블을 변경하세요.
- 순서 중 첫 번째 페이지만 최적화하고 페이징 URL에서 SEO에 관련된 내용을 삭제하세요.
- Googlebot은 스크롤하거나 클릭할 수 없으므로 JavaScript 없이도 모든 콘텐츠에 액세스할 수 있도록 해야 합니다.
- Google 검색 콘솔에서 페이지 URL의 접근성을 두 번 확인하세요.

지혜롭게 페이지를 분할하세요. 당신의 웹사이트의 SEO가 기뻐할 겁니다!

# 이메일로 최신 소식을 받아보세요

![image](/assets/img/2024-07-12-DevelopersGuidetoSEO-FriendlyPaginationTipsTricksandTraps_2.png)

<div class="content-ad"></div>

최신 꿀팁과 디지털 마케팅 산업 트렌드를 받아보고 싶다면, 제 뉴스레터에 가입해보세요!