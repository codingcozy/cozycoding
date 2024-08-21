---
title: "사용자 인터페이스 원하는 대로 만드는 방법"
description: ""
coverImage: "/assets/img/2024-07-30-MakeUIDoWhatYouWant_0.png"
date: 2024-07-30 16:43
ogImage: 
  url: /assets/img/2024-07-30-MakeUIDoWhatYouWant_0.png
tag: Tech
originalTitle: "Make UI Do What You Want"
link: "https://medium.com/@iammanolov98/make-ui-do-what-you-want-1036f91e21c3"
isUpdated: true
---





![image](/assets/img/2024-07-30-MakeUIDoWhatYouWant_0.png)

여러분 안녕하세요, 다시 돌아왔습니다!

제게는 긴 한 주였고, 또 다른 기술적 기사를 가져왔습니다.

오늘은 매우 흥미로우며 중요한 주제를 살펴보겠습니다.


<div class="content-ad"></div>

UI에서 div를 마스터하고 우리가 원하는 대로 사용하도록 만들 거에요.

그렇게 말했으니, 편안한 자세로 앉아서 시작해봐요!

# 소개

특별한 소개는 필요 없어요.

<div class="content-ad"></div>

프로젝트는 제가 작업 중인 것이에요. 지금 보시다시피, clearly 우리는 문제가 있어 보이죠.

아이템들이 div를 채우지 못하고 이상하게 보여요. 이 부분을 함께 수정할 거에요!

기술 스택 - Tailwindcss & React.

# 디버깅 🐛

<div class="content-ad"></div>

일반적으로 해결책을 찾기 전에 많이 고민하는 편이에요. 인터넷에서 div를 가운데 정렬하는 방법을 계속 찾느라 피곤한 거죠!!

그래서 저는 디버깅 과정과 아이디어 전체를 작성해서 UI 요소를 원하는 위치에 정확히 배치할 때 도움이 되도록 해놨어요!

무슨 문제가 있나요?

![이미지](/assets/img/2024-07-30-MakeUIDoWhatYouWant_1.png)

<div class="content-ad"></div>

아래는 표 형식으로 나타내드리겠습니다.


| 이름       | 성별   | 나이 |
|------------|--------|------|
| Jane Doe  | 여성   | 30   |
| John Smith | 남성   | 35   |


<div class="content-ad"></div>

보시다시피, 전체 공간을 차지하고 있는 몇 가지 테일윈드 요소가 있는 div가 있습니다.

이 div를 찾는 가장 쉬운 방법은 검색에 해당 문자열을 입력하는 것입니다.

거기에 도착하면 다른 내용이 포함되어 있는 것을 볼 수 있습니다.

```js
 {/*** 작은 워 그래프 ***/}
 <div className="bg-green-300 p-0.5 rounded-lg text-center hover:bg-green-200">
     <div className="md:flex md:flex-row p-0.5 gap-2 justify-center">
        <div className="mt-2 md:mt-0 flex-1">
             <InfoGraphs
                title="Players Ratio"
                subTitle=""
                imgPath="/clanArrowed.png"
                graphData={}
                tinyGraphTitle=""
                info={""} />
         </div>
.....
.....
.....  
```

<div class="content-ad"></div>

여기서 우리는 2개의 div를 가지고 있음을 볼 수 있습니다. 첫 번째와 두 번째 그리고 그런 다음 자체적으로 일부 div를 보유하는 InfoGraphs가 있습니다. 이것은 큰 프로젝트이기 때문에 완전히 분석하는 데 시간이 걸릴 것입니다!

그러나 F12를 사용하여 흥미로운 것을 발견했습니다!

![이미지](/assets/img/2024-07-30-MakeUIDoWhatYouWant_3.png)

![이미지](/assets/img/2024-07-30-MakeUIDoWhatYouWant_4.png)

<div class="content-ad"></div>

실제로 메인 아래 두 번째 div이 문제를 일으키고 있는 것 같아요!

왜 아래까지 완전히 나오지 않는 걸까요?

여기에 수정 방법이 있어요!

```js
 <div className="md:flex md:flex-row p-0.5 gap-2 justify-center md:h-full">
  <div className="mt-2 md:mt-0 flex-1">                                                                        
```

<div class="content-ad"></div>

마크다운 형식으로 변경하는 것을 추가해서 정말 잘 작동했네요!

![이미지](/assets/img/2024-07-30-MakeUIDoWhatYouWant_5.png)

이제 원하는 만큼만 가져가요!

# 결론

<div class="content-ad"></div>

<div 계층 구조를 살펴보면 어떤 div가 문제를 일으키는 지 알 수 있고 그에 몇 가지 값을 추가할 수 있어요.

Tailwindcss를 사용하면 많은 문서를 활용할 수 있어서 작업이 훨씬 더 좋고 쉬워요!

이 기술적인 글이 유용했기를 바라며, 다음 글에서 뵙겠습니다!

즐거운 코딩, 엔지니어분들!

<div class="content-ad"></div>

안전하고 건강하게 지내세요! 🚓