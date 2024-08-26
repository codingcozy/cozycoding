---
title: "서버란 무엇인가 초보자를 위한 간단한 서버 만들기"
description: ""
coverImage: "/assets/img/2024-08-26-WhatisaserverLetsmakeatinyserverfromscratch_0.png"
date: 2024-08-26 18:37
ogImage: 
  url: /assets/img/2024-08-26-WhatisaserverLetsmakeatinyserverfromscratch_0.png
tag: Tech
originalTitle: "What is a server Lets make a tiny server from scratch"
link: "https://medium.com/mossy-code/what-is-a-server-lets-make-a-tiny-server-from-scratch-d037cb68b5de"
isUpdated: false
---


![이미지](/assets/img/2024-08-26-WhatisaserverLetsmakeatinyserverfromscratch_0.png)

인터넷이 정말 어떻게 작동하는지 궁금했던 적이 있나요? 분명 서버와 그들의 클라이언트에 의해 구동되지만, 그게 정말 무슨 의미일까요? 어떤 기능이 어떤 기계에서 실행되는 프로그램을 서버로 만드는 걸까요?

시작하기 전에 한 가지 명심해야 할 것은, 수염을 치켜들어도 좋다는 거에요 (최대 50번 😱)

이 글에서는 Node.js를 사용하여 처음부터 최소한의 서버를 만들면서 서버가 정확히 어떻게 작동하는지 기초 개념을 살펴보겠습니다. 그 과정에서 서버가 클라이언트와 상호작용하는 핵심 개념과 인터넷이 그것을 어떻게 가능하게 하는지 분해해볼 거예요. 당신이 경험 많은 코더이든 가장 좋아하는 웹사이트 뒤의 기술에 궁금증을 품은 초보자이든, 이 안내서는 서버의 마법을 풀어내어 여러분이 직접 만들 수 있도록 안내해줄 거에요.

<div class="content-ad"></div>

만약 node.js를 시작하는 방법을 모르시면 여기서 시작해보세요!

먼저 우리는 인터넷이 어떻게 작동하는지 알아보고, 네트워크 상의 컴퓨터들이 어떻게 통신하는지 살펴본 뒤, 서버가 무엇이며 클라이언트와 상호작용하는 방법에 대해 알아보고 나서, 우리만의 작은 서버를 작성해볼 것입니다.

# 인터넷 소개

웹 서버가 무엇인지 제대로 이해하기 위해서는 먼저 인터넷이 무엇인지에 대해 알아야 합니다. 네트워크가 무엇이며 네트워크 상의 컴퓨터들이 어떻게 통신하는지에 대한 기본적인 개념을 정의해야 합니다.

<div class="content-ad"></div>

2006년 유명한 밈이 말하는 대로 "인터넷은 튜브들의 연속이야! 🎶"

그래요, 어떤 면에선. 인터넷은 네트워크 케이블을 통해 상호 연결된 컴퓨터로 구성되어 있어요. 처음에는 전화선, 그 다음은 이더넷, 마지막으로 광섬유까지 있죠. 그리고 다양한 무선 주파수 기술도 잊지 말아야 해요. 라디오가 오디오를 장치로 방송하는 방식처럼, 컴퓨터도 유사한 기술을 사용해 무선으로 통신하며, 비트를 공중을 통해 전송해요(WiFi, 4G 등을 생각해보세요).

이것이 "컴퓨터 네트워크"를 형성하는 것입니다: 컴퓨터들, 그들을 연결하는 케이블들, 그리고 일부는 무선 주파수를 통해 통신하기도 해요 📻 이것은 종종 물리층(physical layer)이라고 불려요.

이 물리층을 이루는 구성 요소들은 네트워크 프로토콜을 사용해서 통신해요. 인터넷은 특히 TCP/IP 프로토콜에 의존하며, 더 자세히 알아보자면 TCP - Transmission Control Protocol에만 초점을 맞출 거예요.

<div class="content-ad"></div>

이제 "서버란 무엇인가?"에 초점을 맞출 수 있습니다. 네트워크 상의 모든 통신은 클라이언트-서버 통신으로 정의할 수 있습니다. 클라이언트는 무언가를 요청하고, 서버는 그 요청에 응답합니다. 이것의 가장 간단한 예는 지금 당신이 브라우저(클라이언트)를 통해 이 웹사이트를 읽기 위해 요청하는 것입니다. 당신의 요청은 일련의 튜브를 통해 특정 서버(컴퓨터)를 찾아 이 게시물을 포함한 웹사이트로 응답을 받았습니다.

인터넷 작동 방식에 대한 이 기본적인 이해를 가지고 있으면, 이제 프로그램을 서버로 만드는 데 필요한 기초가 갖추어졌습니다.

# 처음부터 서버 만들기!

인터넷에 연결되는 모든 컴퓨터는 드라이버와 프로토콜에 의존하여 들어오는 요청을 해석하고 네트워크의 다른 컴퓨터로 메시지를 보냅니다.

<div class="content-ad"></div>

대부분의 프로그래밍 언어는 네트워크 통신용 내장 도구를 제공합니다. 이러한 도구들은 버퍼 관리, 데이터 스트림 처리, 포트 처리와 같은 기본적인 복잡성을 처리합니다. 네트워크 통신이 작동하는 중요한 측면이기는 하지만, 이 기사에서는 이러한 세부 사항에 대해서는 다루지 않겠습니다. 대신 최소한 서버를 구축하는 데 필요한 기본 사항에 초점을 맞추겠습니다.

# 단계 1: 기본 TCP 서버 만들기

Node.js에는 net이라는 내장 저수준 네트워킹 모듈이 함께 제공됩니다. 이는 TCP 연결을 설정하고 관리하기 위한 편리한 추상화입니다.

가장 간단한 구현부터 시작해 보겠습니다. createServer() 함수를 사용하여 두 개의 콜백인 "listening" 및 "connection"을 추가해 보겠습니다.

<div class="content-ad"></div>

TCP 서버가 초기화를 완료하고 들어오는 TCP 연결을 활발히 수신할 때 듣기 기능이 활성화됩니다.

다음을 server.js 파일에 작성하세요.

```js
import net from 'net';

const PORT = process.env.PORT || 3000;
const server = net.createServer();

server.on("connection", (socket) => {
  console.log("client connected");
});

server.listen(PORT);

server.on("listening", () => {
  console.log(`server listening on port ${PORT}`);
});
```

그런 다음 터미널에서 node server.js를 실행하세요.

<div class="content-ad"></div>

터미널에서 확인해보세요! 너무 멋져요 💅

![이미지](/assets/img/2024-08-26-WhatisaserverLetsmakeatinyserverfromscratch_1.png)

이제 브라우저를 열고 요청을 보내면 어떻게 되는지 확인해봅시다. 주소창에 localhost:3000을 입력하세요.

![이미지](/assets/img/2024-08-26-WhatisaserverLetsmakeatinyserverfromscratch_2.png)

<div class="content-ad"></div>

이게 뭐에요! 방금 웹 서버를 작성했는데요? 😤

문제는 우리가 맨땅에 허리한 TCP 서버만 만들었다는 점이에요. 반면 브라우저는 HTTP 프로토콜을 기본으로 사용하죠. 우리 서버가 HTTP로 응답하지 않기 때문에 요청이 실패한 거예요. 하지만 걱정하지 마세요—HTTP는 TCP 기반으로 만들어졌기 때문에 멀지 않아요.

이제 터미널에서 확인해보면, 우리 서버가 방금 연결을 기록했으니 뭔가 진행 중인 것 같아요...👀

![이미지](/assets/img/2024-08-26-WhatisaserverLetsmakeatinyserverfromscratch_3.png)

<div class="content-ad"></div>

요런, 작고 귀여운 클라이언트-서버 통신이 나왔네요. 이 초기 연결은 서버와 클라이언트 사이에 이뤄지는 이른바 핸드셰이크입니다. 핸드셰이크는 통신이 어떻게 이루어질지에 대한 클라이언트와 서버 간의 합의입니다. 예를 들어 HTTP 클라이언트-서버 전송 프로토콜을 초기화할 때 이루어집니다.

저희는 여기서 핸드셰이크에 대해 다루지 않겠지만, 그에 대해 더 읽고 싶다면 언제든지 말씀해주세요!

# 단계 2: 서버용 클라이언트

브라우저를 서버의 클라이언트로 사용하는 대신, 동일한 net 패키지를 사용하여 클라이언트를 직접 작성할 수 있습니다. 이 패키지는 createConnection이라는 함수를 노출합니다. 그래서, 새 파일을 만들어 client.js라고 이름 짓도록 하죠.

<div class="content-ad"></div>

그런 다음 이렇게 쓰시면 됩니다:

```js
import net from "net";

let socket = net.createConnection({ port: 3000 }, () => {
    console.log("서버에 연결되었습니다!");
});
```

지금까지 작성한 코드에 대한 링크는 아래와 같습니다.

그런 다음 새로운 터미널 창에서 `node client.js`를 실행합니다.

<div class="content-ad"></div>


![image](/assets/img/2024-08-26-WhatisaserverLetsmakeatinyserverfromscratch_4.png)

여기까지 왔어요... 기본 클라이언트와 서버가 TCP 네트워크 프로토콜을 통해 통신하고 있습니다. 이제 "와이어"를 통해 일부 데이터를 전송해보겠습니다. 작은 핑=퐁.

클라이언트에서는 소켓의 write 메서드를 사용하여 서버에 "Hello server 🎈" 메시지를 보내고 서버로부터 전송된 메시지를 기록합니다. 여기에 클라이언트.js 파일에 이미 있는 내용 이후에 추가할 내용입니다:

```js
socket.write("Hello server! 🎈", "utf8", (err) => {
  if (err) {
    console.error(err);
  }
});

socket.addListener("data", (msg) => {
  console.log(msg.toString());
});
```

<div class="content-ad"></div>

그리고 이제 server.js 파일에 "connection" 콜백 내에 리스너를 추가하여 모든 메시지를 로그로 남기세요. 그런 다음 "Hello client! 🎉"로 응답하세요.

```js
socket.addListener("data", (msg) => 
    console.log(msg.toString());
    socket.write("Hello client! 🎉", "utf8");
});
```

이 단계에서 수행된 코드 변경 사항에 대한 링크

그런 다음 다시 실행해보세요:

<div class="content-ad"></div>


![이미지](/assets/img/2024-08-26-WhatisaserverLetsmakeatinyserverfromscratch_5.png)

와우, 우리가 지금까지 이룬 것에 대해 더 행복할 수 없습니다.

우리는 기본적인 TCP 클라이언트-서버 통신을 작성하는 데 성공했습니다. 당연히 다음 단계는 HTTP 마을입니다. 몇 줄의 코드로 가능해야합니다. 하이퍼텍스트 전송 프로토콜은 메시지가 구성되는 방식이 필요합니다. 이것이 브라우저를 열고 프로토콜을 "완수"하는 열쇠입니다. 그래서 우리의 작은 서버를 진정한 웹 서버로 변신시켜 브라우저와 HTTP 프로토콜을 사용하여 클라이언트와 상호 작용할 수 있는 능력을 갖추도록 합시다!

이를 달성하기 위해 우리는 server.js 파일을 조금 수정할 것입니다. 서버를 수정하여 수신된 HTTP 요청을 처리하고 간단한 HTTP 응답을 보내도록 변경할 것입니다.


<div class="content-ad"></div>

# 단계 3: HTTP 타운

서버.js 파일의 내용을 다음 코드로 교체하세요:

```js
import net from "net";

const PORT = process.env.PORT || 3000;

const server = net.createServer((socket) => {
  console.log("클라이언트가 연결되었습니다"); // 수신된 데이터 처리 (HTTP 요청 처리)
  socket.on("data", (data) => {
    // 기본 HTTP 응답 보내기
    const httpResponse =
      "HTTP/1.1 200 OK\r\n" +
      "Content-Type: text/plain\r\n" +
      "Content-Length: 13\r\n" +
      "\r\n" +
      "안녕, 세상!";

    socket.write(httpResponse, "utf8", (err) => {
      console.error(err);
    });
    socket.end(); // 응답을 보낸 후 연결 종료
  });

  socket.on("end", () => {
    console.log("클라이언트가 연결을 해제했습니다");
  });
});

server.listen(PORT, () => {
  console.log(`프릭킹 HTTP 서버가 포트 ${PORT}에서 수신 대기 중입니다`);
});
```

server.js에 변경사항을 저장한 후, 다음 명령어로 서버를 다시 실행하세요:

<div class="content-ad"></div>

```js
node server.js
```

브라우저에서 localhost:3000로 이동하면 이제 "Hello, world! 🎉" 텍스트가 표시됩니다.

이것은 당신의 서버가 연결을 수락하는 것뿐만 아니라 적절한 HTTP 메시지로 응답하는 것을 확인합니다. 이 주제에는 더 많은 내용이 있지만, 이것은 시작에 불과합니다. 우리 작은 서버에서 다룰 수 있는 몇 가지 다음 단계는 다음과 같습니다: 다중 연결 처리, HTTP 요청 구문 분석 및 다양한 방법 지원 (시도해 볼 수 있는 것). 계속해서 더 있습니다. 다음으로 무엇을 다룰지 알려주세요!

server.js에 적용된 최종 변경 사항 링크: [링크](링크)


<div class="content-ad"></div>

# 계속해서 발전해 나가세요!

축하해요! 👏 이제 TCP 서버를 생성하고 HTTP 서버로 업그레이드하여 기술을 탐구하고 웹의 근본을 깊게 파고들었어요.

이를 이해하는 것은 웹과 서버 측 기술을 이해하는 좋은 기반을 제공해줘요. 이 내용이 도움이 되었기를 바라며, 추가 탐구를 위한 질문이나 아이디어가 있다면 망설이지 마시고 댓글을 남기시거나 연락해주세요!