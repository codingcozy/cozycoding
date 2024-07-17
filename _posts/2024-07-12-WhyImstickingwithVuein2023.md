---
title: "내가 2023년에 Vue를 여전히 사용할 이유"
description: ""
coverImage: "/assets/img/2024-07-12-WhyImstickingwithVuein2023_0.png"
date: 2024-07-12 22:37
ogImage: 
  url: /assets/img/2024-07-12-WhyImstickingwithVuein2023_0.png
tag: Tech
originalTitle: "Why I’m sticking with Vue in 2023"
link: "https://medium.com/@lindblomdev/why-im-sticking-with-vue-in-2023-d67bce7bc2f4"
---


아래는 마크다운 형식으로 표로 변환한 내용입니다.


![이미지](/assets/img/2024-07-12-WhyImstickingwithVuein2023_0.png)

저는 2018년경부터 Angular에서 전환하여 Vue를 전문적으로 사용하기 시작했습니다. 이 기간 동안 Vue가 제게 많은 도움이 되었지만, 늘 같은 작업을 하다 보면 지루해질 수 있다는 점을 알고 있습니다. 또한, 다른 여러 프레임워크가 프레임워크 벤치마킹에서 Vue를 앞지르고 있는 것을 알 수 있었습니다.

![이미지](/assets/img/2024-07-12-WhyImstickingwithVuein2023_1.png)

Svelte라는 프레임워크가 약간 더 매력적으로 다가왔습니다. 그 당시에 Svelte 컴포넌트는 Vue보다 매우 적은 보일러플레이트를 가지고 있었습니다.


<div class="content-ad"></div>

# Vue (options API):

```js
<script>
import { defineComponent } from 'vue'

export default defineComponent({
  data() {
    return {
      count: 0,
    };
  },
  methods: {
    handleClick() {
       this.count += 1;
    }
  },
  computed: {
    label() {
      return this.count === 1 ? 'time' : 'times';
    }
  }
});
</script>

<template>
  <button @click="handleClick">
    Clicked { count } { label }
  </button>
</template>
```

# Svelte:

```js
<script>
 let count = 0;

 function handleClick() {
  count += 1;
 }

 $:label = count === 1 ? 'time' : 'times';
</script>

<button on:click={handleClick}>
 Clicked {count} {label}
</button>
```

<div class="content-ad"></div>

지난 몇 년 동안, 나는 스벨트와 약간 놀아보았어요. 문서를 읽고 장난감 코드로 놀아보았죠. 그러나 진지하게 다가가지 않았어요. 작년, 내가 "완벽한 스택"을 선택하는 앱을 만들어야 하는 요청을 받게 되면서, 드디어 그때가 왔어요. 실제 사용자를 대상으로 한 Svelte + Rust 웹 앱을 만들 시간이었죠.

맡은 앱은 딸들의 트램폴린 클럽을 위한 대회 도구였어요. 팀이 스스로 등록하고 선수를 등록하며, 매년 열리는 대회를 위해 식사와 숙소를 관리할 수 있는 도구였죠.

앱을 개발하면서, Svelte 도구가 vue 도구만큼 안정적이지 않다는 것을 알게 되었어요. 오토 임포트가 잘못된 경로를 가져오거나, 새 파일을 프로젝트에 추가할 때 언어 서버가 멈추는 등의 문제가 있었어요. Svelte에서 개발하는 게 기대했던 만큼 재미있지 않았고, 애플리케이션도 깔끔하지 않게 느껴졌고, 편안한 Vue 경험을 그리워했어요.

마감일이 다가올수록, 도구 사용에 대한 답답함으로 스벨트에 대한 의심이 들기 시작했어요. Solid라는 새로운 플레이어가 입지를 다져 미세한 리액티비티로 유명해졌기 때문이었죠. 나는 그것으로 앱을 전환하기 시작했고, 그 도구가 더 나을 것이라고 희망했어요. Solid에 대해 잘 몰랐기 때문에 이 작업은 시간이 많이 걸렸어요. Vue가 이미 미세한 리액티비티를 가지고 있기 때문에, 처음에 스위칭한 결정에 대해 의심이 들기 시작했어요.

<div class="content-ad"></div>

저는 올바른 선택이었던지 다시 확신하기 위해 벤치마크로 돌아가 봤어요. 이번에는 지난번 놓친 것을 알게 되었어요. Vue가 그다지 느리지 않았구나, 사실 꽤 빠른 것 같았어요. Vue가 가장 성능이 나쁘게 나온 테스트에서는 Solid보다 8밀리초(16배 느린 CPU에서) 느리고, Svelte를 아래에서 이겼죠.

![이미지](/assets/img/2024-07-12-WhyImstickingwithVuein2023_2.png)

그리고 C#에서 Rust로 백엔드를 전환하는 것과 달리, 다른 프론트엔드 프레임워크로 전환하면 새로운 변화가 없었어요. 저의 개발자 생활은 그들로 전환함으로써 더 좋아지지 않았어요, 그보다는 반대로 그랬어요. Vue는 스크립트 설정과 구성 API로 더 깔끔하고(주관적인 의견이죠), 도구 설정이 더 나아요. 성능도 비슷해요.

```javascript
<script setup>
import { ref, computed } from 'vue'

const count = ref(0);
const label = computed(() => count.value === 1 ? '번' : '번')
function handleClick() {
  count.value += 1;
}
</script>

<template>
  <button @click="handleClick">
    { count } { label } 클릭
  </button>
</template>
```

<div class="content-ad"></div>

다른 프레임워크가 Vue를 이기는 한 가지 예는 번들 크기입니다. 특히 초기 번들 크기에서는 다른 프레임워크가 더 좋을 수 있습니다. 전체 번들 크기에 있어서는 어느 정도 규모의 앱에 대해서는 꽤 유사한 결과가 나올 것으로 생각됩니다. 실제로 Vue는 더 작을 수도 있습니다. 그러나 오늘날에는 총 번들 크기가 큰 문제가 되지 않습니다. 필요에 따라 번들을 작은 청크로 분리하여 필요할 때 로드하기 때문입니다. Vue의 경우 초기 번들에 필요한 프레임워크 조각들을 포함해야 하며, 이는 Svelte와 같은 것보다 큽니다. 큰 초기 번들은 초기 시작 시간이 느려질 수 있습니다. 이는 처음 사이트에 접속했을 때 발생하는 것이며, 이후 페이지 로딩은 빠릅니다. 이에 대해 고민하신다면 resumable web에 대한 qwik의 아이디어를 살펴보시는 것이 좋습니다.

요약하면, 2023년에도 Vue를 계속 사용하겠다는 제 이유 목록입니다:

- Script setup + composition API를 활용하여 깔끔한 코드 생성
- 성능이 우수
- 사용해 본 최고의 툴링

2023년에도 Vue를 계속 사용하기로 한 결정에 대해 작은 기사를 읽어 주셔서 감사합니다. 더 많은 웹 개발 관련 컨텐츠를 원하신다면 저를 팔로우해주세요. 👏