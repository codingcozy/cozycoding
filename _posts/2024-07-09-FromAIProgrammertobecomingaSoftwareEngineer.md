---
title: "AI 프로그래머에서 소프트웨어 엔지니어로 성장하는 방법"
description: ""
coverImage: "/assets/img/2024-07-09-FromAIProgrammertobecomingaSoftwareEngineer_0.png"
date: 2024-07-09 11:16
ogImage: 
  url: /assets/img/2024-07-09-FromAIProgrammertobecomingaSoftwareEngineer_0.png
tag: Tech
originalTitle: "From AI Programmer to becoming a “Software Engineer”"
link: "https://medium.com/@keerthigowdad/from-ai-programmer-to-becoming-a-software-engineer-dcba8aae801a"
isUpdated: true
---




![Image](/assets/img/2024-07-09-FromAIProgrammertobecomingaSoftwareEngineer_0.png)

When you ask a senior developer about their experience with AI, you'll likely hear, "It's been great! I can get things done quickly, and simple tasks take much less time."

Now, pose the same question to a junior developer, and the response won't differ much. They might mention receiving a lot of feedback from seniors on improving their code.

In my coding journey over the past 3-4 years, I've transitioned from writing what I used to consider "useless, crappy code" to refining my skills at my first job. The learning curve has been steep, about 31 times more than what I learned during my undergraduate days. Initially, I rarely relied on tools like ChatGPT or other AI-based solutions, preferring to write most of the code myself.

<div class="content-ad"></div>

하지만 그런 툴들에 너무 의존하게 되는 가장 최악의 악몽이 찾아오죠. 이것이 바로 현명한 방법은 아니지만, 막 시작한 사람에게는 바로 옳은 방법이 아닙니다. 내가 한 일 중, 플러터에서 가장 간단한 버튼 변경이나 SQLAlchemy에서 단순한 모델 변경조차 나타나자마자 해결하기 위해 GPT를 사용하게 되었습니다. 심지어 직접 작성할 수 있는 코드임에도 불구하고 아래 코드 조각을 생성하기 위해 GPT를 사용했죠.

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def read_root():
    return {"Hello": "World"}
```

참 새로운 프로개발자로서 간단한 것들은 괜찮을 것이지만, 이런 상황에서 빠지게 되면 문제가 생길 수 있죠. 저는 요즘 내 코드 완성의 80%를 GPT를 사용해서 하는 정도에 이르렀습니다. 하지만, 어떤 가능한 변경도 없이 정확히 이해하고 잘사용하면 괜찮습니다. 처음에는 이 코드가 무엇을 하는지, 왜 그렇게 했는지, 이렇게 하지 않은 이유를 물었었고, 마치 시니어 프로그래머/엔지니어가 모든 것을 설명해주는 것 같았습니다. 그러나 모든 것이 변해버렸죠.

일주일 전, 배포를 위해 코드를 검토하기 시작했습니다. 가능한 취약점이나 이슈, 코드를 깔끔하게 유지하기 위한 가능한 리팩토링을 찾기 위해서요. 코드베이스는 제가 기대한 것이 아니었고, 백만 년 동안 쓰지 않았을 것 같은 것이었습니다. 시니어 개발자라면 누구나 한 가지 질문을 할 것입니다. "프로그래밍을 언제 시작한 건가요?".

<div class="content-ad"></div>

저는 동일한 일을 하는 동료 개발자를 찾기 시작했고 그들의 경험을 알아보기 시작했습니다. 많은 블로그와 기사들을 읽어보고 몇 개의 비디오도 시청했습니다. 그런 다음 피드에 이 비디오가 나타나서 정말 보고 싶었던 것이었습니다.

여기서 모든 것이 시작되었습니다. "GPT 의존성"에서 벗어나기로 결심했습니다. 자신의 능력으로 일을 처리하고 싶었습니다. GPT는 저를 더 나은 프로그래머로 만들어 준 적이 없었고 오히려 최악의 결과를 가져왔습니다. 이전에도 과로 경험은 있었지만 AI로 인해 일이 더 쉬워졌다는 것이 결코 이유가 되지 않았습니다.

항상 사람들이 이야기하는 10배 더 잘하는 프로그래머가 되고 싶었고, 문제가 발생했을 때 기본적으로 해결사가 되고 싶어 했습니다. 항상 자신의 코드에 문제가 있거나 벵갈루루 교통처럼 최적이 아닌 것을 작성하는 사람이 아닌 사람이 되고 싶습니다. 그래서 무엇을 이미 알고 있고 잘하고 싶은 영역에 집중하는 시간이 필요하다는 생각이 듭니다.

매주 Medium과 Dev.to에 블로그를 작성하기로 계획 중입니다. 일하는 동안 다양한 주제와 경험을 다룬 세 개의 블로그나 네 개의 블로그를 작성할 계획을 세우고 있습니다. 이것이 제 첫 번째 블로그이기 때문에 내용이 좀 흩어져 있고 어떻게 개선할 수 있는지와 더 나아지도록 하는 방법에 대한 리뷰를 받고 싶습니다.

<div class="content-ad"></div>

### 행복한 코딩!

![이미지 출처](https://betterprogramming.pub/how-to-outperform-a-10x-developer-fa1132807934)