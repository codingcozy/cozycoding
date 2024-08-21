---
title: "프론트엔드 성능 향상 러브 스토리"
description: ""
coverImage: "/assets/img/2024-08-21-FrontendPerformanceLoveStory_0.png"
date: 2024-08-21 17:17
ogImage: 
  url: /assets/img/2024-08-21-FrontendPerformanceLoveStory_0.png
tag: Tech
originalTitle: "Frontend Performance Love Story"
link: "https://medium.com/itnext/frontend-performance-love-story-ce92302fea5f"
isUpdated: false
---


## "빈 body로 시작하는 웹사이트는 좋은 Lighthouse 점수를 얻을 수 없어요!"

저는 새로운 앱을 소개해 드릴 수 있어서 정말 기쁩니다. 이 앱은 초기 렌더링 뿐만 아니라 주변을 이동하는 것에 있어서 더욱 뛰어난 성능을 자랑합니다. 프론트엔드 엔지니어링에 진지하게 관심이 있다면, 이 기사를 주의깊게 따라가는 것을 강력히 추천드립니다.

# 내용

- 소개
- 이 앱의 특징은 무엇인가요?
- 높은 상호작용성을 갖춘 Multi-Window 뷰
- 온라인 데모 링크
- 하나의 페이지에서 개발 및 dist/프로덕션 버전을 함께 실행하는 방법은 무엇인가요?
- 네비게이션 성능을 측정하는 데에 Lighthouse보다 나은 대안이 있나요?
- 이 앱의 성능을 더 향상시킬 수 있을까요?

<div class="content-ad"></div>

# 1. 소개

지난 게시물 이후에 런타임 업데이트가 좋지만 Lighthouse 점수가 받아들일 수 없다는 것을 말씀해주셨습니다. 따라서 Lighthouse 점수를 수정했습니다. 아래에서 설명하겠습니다. 또한 웹 앱의 종류 및 SSR 및 수분 공급과 같은 문제와 관련된 컨텍스트를 추가해야 한다는 요청도 받았습니다.

그러니까, 매우 흔한 두 가지 다른 유형의 앱에 대해 생각해 봅시다.

- 웹 사이트 및 온라인 상점
캐시를 삭제하고 첫 번째 콘텐츠를 인터랙티브해지기 전에 먼저 보이도록 초기 로딩 경험 최적화
- 기업 앱 및 소셜 네트워크
빠른 페이지 탐색, 런타임 성능을 포함한 페이지 빠른 재로드 최적화.

<div class="content-ad"></div>

Neo를 개발하여 풍부한 인터넷 애플리케이션(RIA)을 만들 수 있었어요. 이것은 두 번째 범주를 위해 최적화된 두꺼운/능숙한 브라우저 측 클라이언트를 사용합니다. 상호작용형 대시보드 애플리케이션이 높은 성능의 RIA/두꺼운 클라이언트를 위한 대본 역할을 하고 있답니다.

빠른 업데이트 대 Lighthouse 점수에 대해, Neo에서의 접근 방식은 다음과 같아요:

- 초기 페이지 로드는 전혀 HTML 콘텐츠 없이 수행해요. 빈 body 태그로 시작하고 직접 포함된 스타일시트가 없어요.
- 클라이언트에 미세한 JavaScript HTML 생성기를 즉시 로드해요. 브라우저가 이를 캐시하므로 다시 가져올 필요가 없어요.
- JSON 구성 데이터와 생성기가 사용하는 JSON 또는 기타 콘텐츠 데이터를 전송하여 이후 DOM을 채우고 업데이트해요... 그 body 태그의 내용을 채워요.

Neo 클라이언트인 조립된 JavaScript 생성기는 혼자서 복잡하고 동적인 UI를 만들 수 있는 모든 도구를 갖췄어요. 서버에서 HTML을 스트리밍할 필요가 없으며(SSR), 수화의 복잡함에 대처할 필요가 없어요.

<div class="content-ad"></div>


![Frontend Performance Love Story 0](/assets/img/2024-08-21-FrontendPerformanceLoveStory_0.png)

![Frontend Performance Love Story 1](/assets/img/2024-08-21-FrontendPerformanceLoveStory_1.png)

# 2. 앱의 특징은 무엇인가요?

지난 블로그 포스트에서 앱의 개발 모드를 소개했어요. 이 버전은 브라우저에서 직접 최신 ECMAScript 코드를 실행합니다. 컴파일이나 변환 작업이 필요하지 않아요. 물론 이 코드는 최소화되지 않았죠.


<div class="content-ad"></div>

앱을 배포하려면 Webpack, ESBuild 또는 Vite와 같은 도구로 최소화되고 번들링된 dist/production 버전을 배포하고 싶어합니다.

문제는: 사용자가 런타임에서 자체 코드를 생성할 수 있도록 앱에 허용하려는 경우, 앱 JavaScript 코드 베이스의 일부를 번들링하는 것이 불가능해집니다.

Neo.mjs 학습 섹션 및 랜딩 페이지 내에서는 VSCode 내에서도 사용되는 Monaco Editor를 사용하고자 합니다. 이 "비스트"는 파일 크기가 3MB 이상이어서 좋은 Lighthouse 점수를 얻기가 쉽지 않습니다.

![이미지](/assets/img/2024-08-21-FrontendPerformanceLoveStory_2.png)

<div class="content-ad"></div>

이 코드 편집기 안에서는 Neo.mjs 클래스(모듈)를 가져올 수 있어요.

![이미지](/assets/img/2024-08-21-FrontendPerformanceLoveStory_3.png)

생성된 코드가 iframes에 들어가지 않고, DOM에 직접 탑재돼요. 그래서 가져온 JavaScript 모듈들은 메인 앱과 동일한 영역에 살아있죠.

특정한 경우이지만, 이는 번들링 도구로 단순히 처리할 수 없어요. 모든 가능한 모듈 조합에 대해 분할 청크를 만들어야 한다면, 부담이 상당할 거예요.

<div class="content-ad"></div>

그래서, 요약하면: 주 앱은 minified dist/production 모드 내에서 실행되도록 하고, 코드.LivePreview의 생성된 모듈은 컴파일되지 않은 개발 모드 내에서 실행되어야 합니다. 또한 두 버전을 동일한 워커 내에서 실행하고 직접 통신할 수 있도록 하려고 합니다. 이에 대해 곧 자세히 알아볼 것입니다.

# 3. 매우 상호작용적인 다중 창 뷰

랜딩 페이지에서 데모를 여러 브라우저 창으로 쉽게 분리할 수 있습니다. 이들은 상태 및 데이터를 공유할 뿐만 아니라 연결된 앱의 모든 구성 요소가 동일한 Application Worker 내에서 실행되며 직접 통신할 수 있습니다.

![이미지](/assets/img/2024-08-21-FrontendPerformanceLoveStory_4.png)

<div class="content-ad"></div>

아래에 표가 있는 이미지입니다.

아래에는 새 위젯 인스턴스를 생성하지 않고도 브라우저 창 간에 동일한 JavaScript 인스턴스의 DOM 출력을 이동할 수 있습니다. 이를 통해 크로스 창 델타 CSS 업데이트도 트리거됩니다.

# 4. 온라인 데모 링크



<div class="content-ad"></div>

먼저 몇 가지 빠른 정보를 알려드리겠습니다:

- 다중 창 데모를 최대한 즐기려면 데스크톱 기반 브라우저 내에서 앱을 열어보세요.
- 페이지의 팝업을 허용해주세요 (광고는 없어요, 약속합니다.)
- Chrome을 사용하여 Android 기기에서도 다중 창이 작동합니다.
- iOS에서는 다중 창 기능이 없이도 앱이 실행됩니다.

dist/production 버전 (성능을 테스트하려면 이 버전을 사용해주세요):

https://neomjs.com/dist/production/apps/portal/#/home

<div class="content-ad"></div>

개발 버전입니다. 궁금하시다면 최적화되지 않은 코드로 빠져들어 볼 수 있습니다.

[여기](https://neomjs.com/apps/portal/#/home)에서 특히 이제 학습 섹션 내에서 네비게이션하는 것이 놀라운 성능을 보여줍니다.

App의 소스 코드는 [여기](https://neomjs.com/apps/portal/#/home)에서 찾을 수 있습니다. 완전히 MIT 라이센스로 허가되었습니다.

<div class="content-ad"></div>

향후 버전에서 큐브 레이아웃을 비활성화하여(카드 레이아웃으로 전환) 할 수도 있습니다. 실행 시 활성화할 수 있는 이스터 에그로 남겨둘 예정입니다. 어떻게 생각하시나요?

# 5. 어떻게 하면 개발 및 분배/프로덕션 버전을 한 페이지에 결합하여 실행할 수 있을까요?

지금까지 Neo.mjs 버전 6.x에서는 전혀 불가능했습니다.

동일한 페이지에서 같은 프레임워크의 여러 버전 또는 환경이 실행될 때, 문제가 발생할 수 있습니다. 동일한 DOM ID가 더 이상 고유하지 않게되는 IdGenerator의 여러 버전을 생각해보세요.

<div class="content-ad"></div>

또한 서로 다른 파일을 가리키는 SharedWorkers를 사용하는 것을 고려해보세요 → 더 이상 전혀 "공유"되지 않습니다.

변화를 발견하기 위해 매우 간단한 Neo.mjs 클래스를 살펴봅시다:

setupClass()는 프로토타입 체인을 통합한 모든 정적 구성을 적용하며 조정된 Label 클래스를 반환합니다.

새로운 버전:

<div class="content-ad"></div>

라벨 클래스 자체의 내보내기는 더 이상 없이 setupClass()의 출력물을 사용합니다. 이 메서드는 이제 주어진 className 네임스페이스 내에 이미 콘텐츠가 있는지 확인하고 있다면 "캐시된" 버전을 반환합니다.

이 변화는 매우 강력한데, 동시에 간단합니다.

다른 하나의 변경 사항은 동일한 SharedWorker 인스턴스를 계속해서 재사용할 수 있도록 하는 데 필요했습니다: 새로운 브라우저 창을 열 때, 애플리케이션 워커가 실행 중인 env를 확인해야 합니다.

즉, 웹 사이트 앱은 dist 프로덕션 환경에서 실행되지만, LivePreview 내의 Helix는 개발 모드에서 실행됩니다. 따라서 컨트롤을 분리하기 위해 창을 열 때, 해당 창은 개발 모드에서 모듈을 가져올 수 있더라도 dist/프로덕션 모드 워커의 파일/URL을 열어야 합니다.

<div class="content-ad"></div>

# 6. 내비게이션 성능 측정을 위한 Lighthouse보다 나은 대안이 있나요?

최근 Chrome에서 Long Animation Frame API를 출시했어요:

이건 정말 놀라운 소식이에요. 드디어 실제 벤치마크를 얻을 수 있겠네요.

우리는 여러 프레임워크와 라이브러리에서 동일한 데모 애플리케이션을 만들어서 내비게이션 성능을 측정할 계획이에요.

<div class="content-ad"></div>

언제든 Slack에서 연락 주시면 이 주제에 대해 도와주실 수 있습니다.

# 7. 이 앱의 성능을 더 향상시킬 수 있을까요?

그렇습니다, 가능하며 해낼 것입니다.

- 자산들이 아직 번들되지 않음 (미들웨어 처리 예정 항목)
- 중요한 렌더링 경로를 위해 자산 사전로딩 (또한 미들웨어 항목)
- 워커 간 통신을 더 개선할 수 있습니다.
- 델타 업데이트를 위한 트리 스코핑 (아마도 다음 주요 버전에서)

<div class="content-ad"></div>

어떤 기술 측면을 좀 더 깊게 파헤치고 싶다면, 댓글을 남겨주시면 환영입니다.

즐거운 코딩,
토비아스