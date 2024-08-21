---
title: "리액트 서버 컴포넌트 (RSC) 정리"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-21 18:56
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "React Server Components (RSC) The Future of Rendering in React "
link: "https://dev.to/margish288/react-server-components-rsc-the-future-of-rendering-in-react-1hd"
isUpdated: true
updatedAt: 1724245489175
---


리액트를 처음 발견했던 때를 기억하시나요? 커피가 떨어지지 않는 것을 발견한 것 같았죠 – 그저 삶을 더 좋게 만들어줬어요.

하지만 리액트가 더 멋진 줄 알았더니 새롭고 마음을 뺴는 것이 나타나서 놀랐죠: 리액트 서버 컴포넌트(RSC)입니다. 리액트 앱이 더 빠르고 똑똑해지면서 여러분이 노력을 들이지 않고도, 마법 같은 일이 일어날 것 같죠? 그렇다면, 함께 RSC의 마법을 탐험해봐요.

## 리액트 서버 컴포넌트란? 🎩🪄

이런 상황을 상상해보세요: 여러분의 리액트 컴포넌트는 록 밴드처럼 비교될 수 있어요. 리드 싱어(클라이언트 측 컴포넌트)는 관객과 상호 작용하면서 무대 앞에 서 있고, 드러머와 베이스 기타 연주자(서버 측 컴포넌트)는 모두를 뒷받침하고 있어요. 그들 없이는 전체 공연이 붕괴될 거예요.

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


![Server Component](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fpecokrjeygiloc6wlhvn.gif)

React Server Components (RSC)는 배경 음악을 이끌어내는 주역들이에요. 모든 것을 클라이언트(사용자의 브라우저)에서 실행하는 대신, RSC를 사용하면 일부 중요한 작업을 서버에서 처리할 수 있어요. 이는 당신의 기림이 과중해지지 않도록 리드 싱어가 전체 공연을 독점할 필요가 없다는 것을 의미해요. 결과는 무엇일까요? 여전히 멋지게 속도가 빠르고 효율적인 앱이에요.

## 이 마법은 어떻게 작동할까요? ✨

RSC는 서버와 클라이언트 사이의 팀워크에 관한 것이에요.


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

서버 측: 상호 작용이 필요하지 않은 구성 요소(예: 데이터 가져오기 또는 정적 콘텐츠 렌더링)는 서버에서 처리됩니다. 마치 미리 포장된 식사를 받는 것과 같아요. 모든 것이 사용 준비되어 있어 브라우저가 처음부터 요리를 할 필요가 없어요.

클라이언트 측: 버튼이나 양식과 같이 상호 작용해야 하는 구성 요소는 여전히 클라이언트에서 실행됩니다. 여기가 마법입니다. 서버에서 렌더링된 구성 요소와 클라이언트에서 렌더링된 구성 요소가 완벽하게 함께 이어져 마치 완벽하게 조정된 댄스처럼 보여요.

데이터 가져오기 혼돈 해결: RSC로 하면 "시간 내에 로드될까?" 라는 게임을 할 필요가 없어요. 서버가 필요한 모든 데이터를 한 번에 가져와 최종 제품을 전송하고 이제 데이터가 한 방울씩 도착할 때 기다릴 필요가 없어요.

## 왜 관심을 가져야 할까요? 🚀

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

- 친구야, 솔직하게 얘기하자면: 우리 모두가 앱이 빠르고 효율적이며 부드러워야 한다고 생각해. 여기서 RSC가 새로운 절친이 되는 이유가 있어:
- 꽈당 빠른 로딩 시간: 서버가 주요 작업을 처리해주면 당신의 앱은 고양이가 레이저 포인터를 찾는 것보다 더 빨리 로딩돼.
더 적은 JavaScript, 더 빠른 속도: 브라우저가 처리해야 하는 JavaScript가 적을수록 앱이 더 빨라져. RSC는 불필요한 것을 제거해서 당신에게 가늠돌림 없이 아름다운 렌더링 기계를 남겨줘.
- SEO 향상: 서버가 HTML을 렌더링하니까 검색 엔진이 쉽게 페이지를 검색할 수 있어서 SEO에 작은 도움이 돼. 피자 배달하는 사람이 무료 쿠폰까지 준다는 걸 알게 된 것 같은 느낌이라고 할까?
- 간소화된 데이터 관리: 서버가 데이터를 가져오고 관리하는 걱정은 맡기고 당신은 앱이 멋지게 보이도록 신경 쓰자. 클라이언트쪽에서 API 호출을 계속 하는 건 이제 끝났어. 일련의 처리가 프로처럼 처리돼.

## 언제 RSC를 사용할 수 있는가? ⏳

React Server Components는 아직 준비 중이지만 빨리 오픈될 예정이야. Suspense나 Concurrent Rendering과 완벽하게 호환되도록 디자인됐으니, 완전히 마련되면 문제 없이 앱에 통합할 수 있을 거야.

## 예시: 블로그 만들기! 📝

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

블로그를 만들고 있다고 상상해봐요. 뒷 부분, 즉 포스트를 가져오고 표시하는 부분은 서버에서 처리할 수 있어요. 한편, 사용자들이 상호작용하는 댓글 섹션은 클라이언트에서 처리돼요.

여기 sneak peek를 가져와 봤어요:

```js
// ServerComponent.js
export default function BlogPost({ id }) {
  const post = fetchPostFromDatabase(id); // 서버 측에서 데이터 가져오기
  return <div dangerouslySetInnerHTML={ __html: post.content } />;
}

// ClientComponent.js
export default function CommentSection({ postId }) {
  const [comments, setComments] = useState([]);

  useEffect(() => {
    fetch(`/api/comments?postId=${postId}`)
      .then((res) => res.json())
      .then((data) => setComments(data));
  }, [postId]);

  return (
    <div>
      {comments.map((comment) => (
        <p key={comment.id}>{comment.text}</p>
      ))}
    </div>
  );
}
```

이 시나리오에서 BlogPost는 서버에서 렌더링되기 때문에 사용자가 도착하는 즉시 콘텐츠가 준비돼요. 한편, 댓글 섹션은 사용자들이 지연 없이 채팅을 시작할 수 있게 클라이언트에서 대화형으로 렌더링돼요.

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

미래가 올 것이고, 그것은 빠릅니다 🌟
React Server Components가 게임을 바꾸려고 합니다. 우리 앱을 더 빠르고 가볍고 똑똑하게 만들어 줄 겁니다. 아직 실험단계인 RSC이지만, React 개발의 미래가 여기에 있음은 분명합니다. 그러니 앱을 다음 수준으로 끌어올릴 준비를 해보세요!

이 버전이 여러분의 흥미를 자극했으면 좋겠네요! 더 탐험해보고 싶은 것이 있다면 알려주세요. 🚀