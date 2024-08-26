---
title: "IoT 대시보드 만들기 step by step 튜토리얼"
description: ""
coverImage: "/assets/img/2024-08-26-IoTDashboardTutorial_0.png"
date: 2024-08-26 18:09
ogImage: 
  url: /assets/img/2024-08-26-IoTDashboardTutorial_0.png
tag: Tech
originalTitle: "IoT Dashboard  Tutorial"
link: "https://medium.com/@adityanashikkar94/iot-dashboard-tutorial-b278754ea8d7"
isUpdated: false
---


안녕하세요! 실시간 데이터 시각화와 함께 IoT 대시보드를 만드는 튜토리얼에 오신 것을 환영합니다! 이 튜토리얼에서는 HTML5, CSS3, Bootstrap 4 및 JavaScript를 사용하여 실시간 온도 값을 표시하는 동적 대시보드를 만들어보겠습니다. 노드.js를 사용하여 센서 데이터를 모방하고 라이브 환경을 시뮬레이션할 것입니다. 프로젝트에 IoT를 통합하고자 하는 개발자이거나 실시간 데이터 시각화에 대해 궁금한 분들을 위해 이 튜토리얼이 한 단계씩 가이드해 드릴 것입니다. 완료하면 새로운 데이터가 들어올 때마다 업데이트되는 완전히 기능적인 온도 차트를 갖게 될 것입니다. 이제 시작하여 여러분의 IoT 프로젝트를 현실로 만들어봅시다! 

뒤에 나온 Markdown 표식을 이해하고 계시나요?

Backend code to mock sensor data


<div class="content-ad"></div>

한 줄 씩 코드를 이해해 볼까요?

1. WebSocket 모듈을 가져오기

```js
const WebSocket = require('ws');
```

이 코드는 ws 모듈을 가져오는 역할을 합니다. ws는 Node.js용 인기 있는 WebSocket 라이브러리입니다. WebSocket을 사용하면 서버와 클라이언트 간에 실시간 양방향 통신이 가능하며 단일, 오래 지속되는 연결을 통해 이루어집니다.

<div class="content-ad"></div>

2. WebSocket 서버 만들기

```js
const server = new WebSocket.Server({ port: 8080 });
```

여기서는 포트 8080에서 수신 대기하는 새로운 WebSocket 서버 인스턴스가 생성됩니다. 이 서버는 클라이언트로부터 들어오는 WebSocket 연결을 수락할 것입니다.

3. 클라이언트 연결 처리하기

<div class="content-ad"></div>

```js
server.on('connection', (socket) => {
    console.log('Client connected');
```

이 부분은 웹소켓 서버에 클라이언트가 연결될 때 트리거되는 connection 이벤트를 위한 이벤트 리스너를 설정합니다. 연결이 성립되면 서버는 "Client connected"를 콘솔에 로깅합니다.

4. 실시간 온도 데이터 전송

```js
    const sendTemperatureData = () => {
        const temperature = (Math.random() * 10 + 20).toFixed(2); // 20에서 30 사이의 더미 온도
        const timeStamp = (new Date()).getTime();
        const data = JSON.stringify({ temperature, timeStamp });
```

<div class="content-ad"></div>

- 이 함수인 sendTemperatureData는 연결된 클라이언트에 온도 데이터를 생성하고 전송하는 역할을 합니다.
- Math.random()을 사용하여 20도부터 30도 사이의 임의의 온도가 생성되며, .toFixed(2)를 사용하여 결과가 소수점 둘째 자리까지 포맷됩니다.
- 현재 시간을 밀리초로 나타내는 new Date().getTime()을 사용하여 타임스탬프가 생성됩니다 (Unix epoch인 1970년 1월 1일부터의 시간).
- 그런 다음 온도와 타임스탬프는 JSON.stringify를 사용하여 JSON 문자열로 패키징됩니다.

```js
        socket.send(data);
```

온도와 타임스탬프가 포함된 JSON 문자열은 socket.send를 사용하여 클라이언트로 전송됩니다.

```js
        setTimeout(sendTemperatureData, 2000);
    };
```

<div class="content-ad"></div>

`setTimeout`은 실시간 데이터 스트림을 시뮬레이션하기 위해 sendTemperatureData 함수를 매 2초마다 호출하는 데 사용됩니다.

5. 초기 데이터 전송

```js
sendTemperatureData();
```

이 줄은 클라이언트가 연결된 후 즉시 sendTemperatureData를 처음 호출합니다. setTimeout 루프로 인해 이 함수는 매 2초마다 계속 호출됩니다.

<div class="content-ad"></div>

6. 클라이언트 연결 해제 처리

```js
socket.on('close', () => {
    console.log('클라이언트가 연결을 해제했습니다');
});
```

close 이벤트를 처리하기 위한 이벤트 리스너가 설정되어 있습니다. 이 이벤트는 클라이언트가 웹소켓 서버와의 연결을 해제할 때 발생합니다. 이때 콘솔에 "클라이언트가 연결을 해제했습니다"가 로그로 출력됩니다.

7. 웹소켓 서버 시작

<div class="content-ad"></div>

```js
console.log('웹소켓 서버가 ws://localhost:8080 주소에서 실행 중입니다.');
```

마지막으로, 이 라인은 웹소켓 서버가 작동 중이며 클라이언트가 연결할 수 있는 URL(ws://localhost:8080)을 특정하는 메시지를 기록합니다.

이 코드는 간단한 웹소켓 서버를 설정하여, 매 2초마다 연결된 클라이언트에게 랜덤 온도 데이터와 타임스탬프를 스트리밍합니다. 또한 연결 및 연결 해제 이벤트를 로깅합니다.

실시간 데이터 시각화를 위한 프론트엔드 코드입니다.

<div class="content-ad"></div>

```js
<!-- index.html -->
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>IoT 대시보드</title>
    <!-- Highcharts 라이브러리 포함 -->
    <script src="https://code.highcharts.com/highcharts.js"></script>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.6.2/dist/css/bootstrap.min.css">
    <script src="https://cdn.jsdelivr.net/npm/jquery@3.7.1/dist/jquery.slim.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.1/dist/umd/popper.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@4.6.2/dist/js/bootstrap.bundle.min.js"></script>
    <script src="script.js" defer></script>
</head>

<body>
    <nav class="navbar navbar-expand-sm bg-dark navbar-dark">
        <ul class="navbar-nav">
            <li class="nav-item active">
                <a class="nav-link" href="#">IoT 대시보드</a>
            </li>
        </ul>
    </nav>
    <div class="row mt-4 p-4">
        <div class="col">
            <div id="accordion">

                <div class="card rounded-0">
                    <div class="card-header bg-info card rounded-0">
                        <a class="card-link text-white" data-toggle="collapse" href="#collapseOne">
                                센서 1의 온도
                            </a>
                    </div>
                    <div id="collapseOne" class="collapse show" data-parent="#accordion">
                        <div class="card-body">
                            <div id="temperatureChart"></div>
                        </div>
                    </div>
                </div>

            </div>
        </div>
    </div>
    <script>
    </script>
</body>

</html>
```

```js
// script.js
// Highcharts 차트 초기화
const chart = Highcharts.chart('temperatureChart', {
    chart: {
        type: 'line',
        events: {
            load: function() {
                this.series[0].setData([]);
            }
        }
    },
    title: {
        text: ''
    },
    xAxis: {
        type: 'datetime'
    },
    yAxis: {
        title: {
            text: '온도 (°C)'
        },
        plotLines: [{
            value: 0,
            width: 1,
            color: '#808080'
        }]
    },
    series: [{
        name: '온도',
        data: []
    }]
});

// 로컬 WebSocket 서버에 연결
const socket = new WebSocket('ws://localhost:8080');

socket.onopen = function(event) {
    console.log('WebSocket 서버에 연결되었습니다');
};

socket.onmessage = function(event) {
    const sensorData = JSON.parse(event.data);
    const series = chart.series[0];
    const x = sensorData.timeStamp;
    const y = parseFloat(sensorData.temperature);
    series.addPoint([x, y]);
};

socket.onerror = function(error) {
    console.error('WebSocket 오류 발생:', error);
};

socket.onclose = function(event) {
    console.log('WebSocket 연결이 닫혔습니다');
};
```

![IoT 대시보드](/assets/img/2024-08-26-IoTDashboardTutorial_1.png)

<div class="content-ad"></div>

1. Highcharts 차트 초기화하기

```js
const chart = Highcharts.chart('temperatureChart', {
    chart: {
        type: 'line',
        events: {
            load: function() {
                this.series[0].setData([]);
            }
        }
    },
    title: {
        text: ''
    },
    xAxis: {
        type: 'datetime'
    },
    yAxis: {
        title: {
            text: 'Temperature (°C)'
        },
        plotLines: [{
            value: 0,
            width: 1,
            color: '#808080'
        }]
    },
    series: [{
        name: 'Temperature',
        data: []
    }]
});
```

- const chart = Highcharts.chart(`temperatureChart`, ' ... '): 이 줄은 새로운 Highcharts 차트를 초기화하고 그것을 chart 변수에 할당합니다. 차트는 temperatureChart라는 ID를 가진 HTML 요소에 렌더링됩니다.
- chart: ' type: `line` ': 차트 유형을 선 그래프로 지정하며, 시간에 따른 지속적인 데이터를 표시하는 데 적합합니다. (ex: 온도 측정값)
- events: ' load: function() ' ... ' ': 로드 이벤트에 대한 이벤트 리스너를 정의하고, 차트가 완전히 로드되면 트리거됩니다. 해당 코드는 이 경우 첫 번째 시리즈의 초기 데이터를 지우도록 설정되어 있습니다: this.series[0].setData([]);.
- title: ' text: `` ': 여기서 차트 제목을 정의하는 부분이지만, 이 예시에서는 비워두었습니다.
- xAxis: ' type: `datetime` ': x-축이 시간 기반 데이터를 표시하도록 구성되어 있어, 시간에 따른 온도 측정값을 그래프로 표시할 때 중요합니다.
- yAxis: ' title: ' text: `Temperature (°C)` ', plotLines: [' value: 0, width: 1, color: `#808080` '] ':
- y-축은 "Temperature (°C)"로 레이블이 지정됩니다.
- 너비가 1픽셀이고 회색(#808080)인 y = 0에 플롯 라인이 추가됩니다. 일반적으로 중요한 값이나 임계값을 나타내는 데 사용되지만, 여기에서는 기준선으로만 사용됩니다.
- series: [' name: `Temperature`, data: [] ']: 차트의 데이터 시리즈를 정의합니다. 시리즈는 "Temperature"로 이름이 지정되며 빈 데이터 배열로 시작됩니다. 데이터가 수신되면 이 시리즈에 추가될 것입니다.

2. WebSocket 서버에 연결하기

<div class="content-ad"></div>

```js
const socket = new WebSocket('ws://localhost:8080');
```

이 코드는 ws://localhost:8080으로 로컬에서 실행 중인 서버에 WebSocket 연결을 설정합니다. 이 연결을 통해 서버에서 클라이언트로 실시간 데이터를 스트리밍할 수 있습니다.

3. WebSocket 이벤트 처리

연결이 열리면


<div class="content-ad"></div>

```js
socket.onopen = function(event) {
    console.log('웹소켓 서버에 연결되었습니다');
};
```

socket.onopen: 이 이벤트 핸들러는 WebSocket 연결이 성공적으로 열릴 때 트리거됩니다. 콘솔에 "웹소켓 서버에 연결되었습니다"를 기록합니다.

메시지 수신

```js
socket.onmessage = function(event) {
    const sensorData = JSON.parse(event.data);
    const series = chart.series[0];
    const x = sensorData.timeStamp;
    const y = parseFloat(sensorData.temperature);
    series.addPoint([x, y]);
};
```

<div class="content-ad"></div>

socket.onmessage: 이벤트 핸들러는 WebSocket 서버로부터 메시지를 수신했을 때 트리거됩니다.

- const sensorData = JSON.parse(event.data);: 수신된 JSON 데이터를 JavaScript 객체(sensorData)로 파싱하여 온도 데이터와 타임스탬프가 포함되어 있습니다.
- const series = chart.series[0];: 차트 내 첫 번째(유일한) 데이터 시리즈를 참조합니다.
- const x = sensorData.timeStamp;: 수신된 데이터에서 타임스탬프를 추출하여 x-좌표로 사용합니다.
- const y = parseFloat(sensorData.temperature);: 온도 값을 부동 소수점 숫자로 변환하여 y-좌표로 사용합니다.
- series.addPoint([x, y]);: 새로운 데이터 포인트 [x, y]를 차트에 추가하여 실시간으로 라인 그래프를 업데이트합니다.

오류 처리

```js
socket.onerror = function(error) {
    console.error('WebSocket error:', error);
};
```  

<div class="content-ad"></div>

socket.onerror: WebSocket 연결 중 오류가 발생하면이 이벤트 핸들러가 트리거됩니다. 오류는 콘솔에 "WebSocket error:" 메시지와 함께 기록됩니다.

연결 닫힘

```js
socket.onclose = function(event) {
    console.log('WebSocket connection closed');
};
```

socket.onclose: WebSocket 연결이 클라이언트 또는 서버에 의해 닫힐 때 트리거되는 이벤트 핸들러입니다. 콘솔에 "WebSocket connection closed"를 기록합니다.

<div class="content-ad"></div>

이 코드는 Highcharts와 웹 소켓을 사용하여 실시간 온도 모니터링 시스템을 설정합니다. 차트는 시간이 지남에 따른 온도 데이터를 표시하도록 초기화되며, 웹 소켓 연결이 설정되어 실시간 업데이트를 받습니다. 새로운 온도 측정값이 수신되면 차트에 추가되어 계속해서 업데이트되는 시각화를 제공합니다. 코드에는 기본 오류 처리가 포함되어 있으며 연결 상태를 추적하기 위해 콘솔에 로그 메시지가 기록됩니다.