---
title: "create-react-app 사용 중지하기 CRA 대안 및 이유"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-26 20:02
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Stop using create-react-app"
link: "https://dev.to/eslachance/stop-using-create-react-app-7in"
isUpdated: false
---


도와줄 부탁이에요. 제발요. 무릎을 꿇고 울면서 부탁하는데...

## CREATE-REACT-APP을 사용 중지해주세요

만약 전문을 전체 읽기 싫다면 간단히 설명할게요 (TL;DR):

- create-react-app은 2년 전부터 업데이트가 중단되었습니다.
- CRA가 의존하는 react-scripts가 2년 전부터 업데이트가 중단되었습니다.
- CRA는 현대적인 대안들에 비해 느립니다.
- webpack은 현대적인 대안들에 비해 매우 느립니다.

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

문제의 해결책은 무엇인가요? 한 마디로 말하면: Vite.

### 더 이상 업데이트되지 않음

마지막으로 create-react-app 패키지가 업데이트된 시기는 2022년 4월 12일 오후 1시 33분 EDT였습니다. 이 시점에서 작성 기준으로 이는 2년이 넘게 지난 것입니다. 몇 가지는 몇 년 동안 업데이트되지 않을 수 있지만, CRA 자체가 업데이트되지 않은 종속성을 많이 가지고 있고 취약점의 영향을 받았을 수 있다는 것을 이해하는 것이 중요합니다. npx create-react-app을 실행하면 다음과 같은 결과가 나옵니다:

8 가지 취약점 (2개 중간, 6개 높음)

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

Facebook/React 팀은 현재 의도적으로 어떤 종속성도 수정할 계획이 없습니다. 실제로 create-react-app이 더 이상 업데이트되지 않고 다른 도구를 권장하고 있음을 명확히 알 수 있습니다.

### React-Scripts

react-scripts는 CRA가 개발 서버를 실행하거나 npm run build로 빌드할 때 사용되는 실제 종속성입니다. CRA와 마찬가지로 마지막 업데이트는 2022년에 이루어졌으며 실제로 CRA의 마지막 업데이트 커밋과 동일한 것입니다. react-scripts는 여러 업데이트 및 패치가 이루어진 종속성을 가지고 있지만 물론 react-scripts 자체가 업데이트되지 않습니다.

### Webpack

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

저는 webpack을 비난하려는 것은 아니지만, 한 가지 말씀드리겠습니다: webpack은 Vite에 비해 느리다고 생각해요. 대체 제품인 Vite만큼 설정 가능하지 않습니다. 플러그인 생태계가 풍부해 보이더라도 이미 Vite이 제공하는 것에 그림자를 드리고 있다고 생각해요.

## 해결책: Vite

Vite(프랑스어로 "빠른"이란 뜻인 vit과 veet 사이로 발음합니다)는 create-react-app의 기능을 완전히 대체한다고 볼 수 있습니다. 핫 리로드 서버와 다양한 플러그인을 제공하여 완벽하게 설정 가능한 개발 환경을 제공합니다.

Vite은 설정할 수 있을 뿐만 아니라 빠르고 대부분의 프런트엔드 프레임워크를 지원합니다. 즉, React, Svelte, Solid, Vue, Lit, Quik, 그리고 Angular용 Vite 프로젝트를 생성할 수 있습니다. 더불어 tailwind, sass 등 빌더 종속성을 가진 vanillajs 애플리케이션도 만들 수 있습니다.

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

개인적인 일화를 하나 해볼까요? 이전 직장에서 Vite에 대해 처음 알게 되었을 때, react-scripts를 사용하는 프로젝트를 Vite로 전환하는 데 반이나를 할애했어요. 이 프로젝트는 상당히 거대했기 때문에 반나절이 걸렸죠. 저는 주니어 개발자였고, 이것에 대해 많이 모르고 있었거든요. 그래도 그게 가치 있을 거라고 믿었고 맞았어요! 파일을 저장하고 브라우저에서 핫 리로드를 기다리는 시간이 6분에서 1초 미만으로 급격히 줄었답니다. 네, 맞아요, "분"과 "초" 사이에 오타가 없어요. 초기 빌드 시간도 여전히 상당히 거대한 애플리케이션이지만 약 20초로 줄었어요.

그래서 "초당빛나게 빠른"이라고 말할 때 그 말이 실제로 맞는 거예요. 허풍이나 과장이 아니에요. 제가 반복해서 말할테니까 아직도 react-scripts를 사용하는 모든 프로젝트에서 다시 한 번 Vite로 전환할 거예요.

그럼, 설득되셨나요? 지금 바로 Vite로 시작해보세요.

## 어떻게 변환하는지

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

그래서 당신은 아마 코드에서 할 변경 사항이 많을지 궁금해 할 것 같아요, 그죠? 음... 사실은 그렇지 않아요. 사실 많은 변경 사항은 앱의 설정 및 루트 수준에서 발생해요.

CRA 앱을 Vite로 변환하는 것은 다음과 같아요:

- 새 폴더(기존 앱이 아닌)에서 npm create vite@latest 실행
- React를 프레임워크로 선택
- TS를 사용하는 경우 Javascript + SWC 또는 Typescript + SWC 선택 (TS를 사용하는 경우 SWC를 이용하면 더 빠르게 빌드할 수 있어요)
- 스카폴드 프로젝트 생성
- App.js와 index.js를 제외한 모든 컴포넌트 및 하위 폴더 복사
- main.jsx 및 App.jsx 수정하여 리액트-라우터, 컨텍스트 등의 구성을 다시 추가
- vite.config.js에 필요한 플러그인 추가
- 오류/이슈에 대한 조정

마지막 3단계가 당신의 대부분의 작업이 진행될 곳이에요. 기존 애플리케이션의 복잡성에 따라 올바른 플러그인과 구성을 찾는 데는 시간이 걸릴 수도 있고, 만날 수 있는 오류에 대해 조정하는 데 시간이 걸릴 수도 있어요. 그러나 최종적으로 다시 한 번 말씀드리지만 노력이 절대적으로 가치 있을 거에요.

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

## 도움이 필요하다면 어떻게 해야 할까요?

만약 이 글을 읽고 Vite로 전환하기로 결심했지만 문제나 오류를 다루어야 할까 봐 걱정이 된다면, Discord의 The Programmer's Hangout 채널인 #javascript에 함께 참여하도록 초대합니다. 저는 거기서 eslachance(보통은 Alterion으로 닉네임을 사용합니다)으로 활동하고 있으니 도움이 필요하면 언제든지 나를 불러주시고 이 dev.to 게시물을 언급해 주세요. 더 공식적인 Reactiflux 서버도 여러분의 지원을 기다리고 있습니다.

제가 없어도 TPH와 Reactiflux에는 여러분을 도와줄 수 있는 사람들이 많이 있습니다. 실제로 제가 직접 Vite로 전환하는 데 도움을 준 사람들도 많아요!

## 음모론적인 이론

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

드라마, 음모 이론, 그리고 불만에 관심이 없다면 여기서 멈추세요. 저는 토끼 굴 속으로 깊이 파고들 것이고, 여러분은 따라오라는 강요는 전혀 없어요 - Vite은 이미 멋지니까요, 이것이 여러분의 답이기 때문에 나머지에 대한 관심을 잃어도 괜찮아요 ;)

자주 스스로에게 묻는 질문은: React 팀이 Vite을 자세히 다룬 "심층 분석" 블록에 Vite 언급을 재발하게 했는지에 대해서에요. 왜 Vite을 언급하는 것이 무시된 느낌을 주는 걸까요? 다른 전체 스택 프레임워크와 함께 언급되는 것이 아닌, 대다수의 사람들이 읽지 않을 벽 글 끝에 중요하지 않은 주석으로만 느껴지나요?

내가 생각하는 것은 Vite과 Parcel을 언급하는 거에 의심스러운 '막대한 숨겨진 벽' 대신, 페이지 상단의 문단에 있었어야 했어. 
페이지는 create-react-app이 폐기되었음을 언급하고 (CRA에 대해 언급이 전혀 없다!) 프론트엔드 전용 프로젝트에 대한 직접 제안으로 Vite 사용을 알아야 했어야 했지.

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

하지만 문서에서 하는 일은 "Production-grade React framework"를 사용하라고 제안하며, 주요 및 처음으로 NextJS를 소개하는 것입니다.

여기가 음모가 시작되는 곳입니다. NextJS는 Vercel에서 만들었으며, NextJS와 특히 호환되는 호스팅을 제공합니다. NextJS는 호스팅할 수 없는데도 서비스에서만 사용 가능합니다. 문서에서 "지원을 위한 활성 커뮤니티가 있는 오픈 소스 모든 framework를 권장하며, 자체 서버 또는 호스팅 공급자에 배포할 수 있다"고 하지만, 호스팅 또는 서버가 이 작업을 수행하려면 nodejs가 실행되어야 한다는 것을 언급하지 않습니다.

그리고 Vercel은 대기업이며, 스스로를 React 생태계에 침투시키고 있습니다. Reddit 게시물 하나에서 언급한 것처럼:

- "React 팀은 현재 Vercel과 주로 협력하여 'React Server Components'와 같은 bleeding-edge React 기능을 '연구, 개발, 통합, 테스트'하고 있습니다. React 문서 웹사이트는 NextJS로 구축되어 있습니다."
- "Vercel은 React 코어 팀 멤버를 더 적극적으로 고용하고 후원하고 있습니다."

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

이제... 이것은 사실을 근거로 하지 않기 때문에 소금 조금 넣고 들으면 좋겠어요. (내가 직접 이에 대한 소스를 심각하게 찾지 못했기 때문에 이것이 사실을 단언하는 것이 아니라 음모론이라고 하는 것이죠). 하지만 Vercel이 React 팀에 얼마나 많은 돈을 투입했는지, 그 팀이 얼마나 많은 멤버를 빼앗아 갔는지, 그리고 React 문서와 팀 자체에 그것이 어떻게 영향을 미쳤는지에 대해 고민하게 만들어요. 직접적으로든, "내 동료가 여기서 일하다가 이제 Vercel에서 일하고 있는데 그들이 Next를 대단하다고 자꾸 말해"와 같은 접근으로든요.

이 주제에 대한 실제 정보가 있으면 개인적으로 연락 주시면 감사하겠어요. 조사 기자들처럼 약간의 조사를 하고 관련된 진짜 기사나 비디오를 작성하고 싶어요. 하지만 그 전까지는 이것이 공식적인 Facebook/React 팀 멤버가 Vite가 문서의 최상단에 나타나지 않은 이유를 설명해야 하는 것이 아닌 한 실제 사실이 아닌 음모론으로 남아있을 거에요.

그게 다예요, 친구들!