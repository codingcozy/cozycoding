---
title: "ESP32 IoT 실시간 데이터 모니터링 시스템 구축 방법"
description: ""
coverImage: "/assets/img/2024-08-21-ESP32IoT-BasedReal-TimeDataMonitoringSystemfromScratch_0.png"
date: 2024-08-21 17:42
ogImage: 
  url: /assets/img/2024-08-21-ESP32IoT-BasedReal-TimeDataMonitoringSystemfromScratch_0.png
tag: Tech
originalTitle: "ESP32 IoT-Based Real-Time Data Monitoring System from Scratch"
link: "https://medium.com/@sudipto3331/esp32-iot-based-real-time-data-monitoring-system-from-scratch-c3e2b2bedf0a"
isUpdated: true
updatedAt: 1724245960423
---



![이미지](/assets/img/2024-08-21-ESP32IoT-BasedReal-TimeDataMonitoringSystemfromScratch_0.png)

실시간 인터페이스: https://sudiptomondal.me/iot/

이 ESP32 프로젝트에서는 ESP32를 사용하여 데이터 모니터링 시스템을 직접 만드는 방법을 안내해 드릴 거에요. ESP32로 IoT 시스템의 기본을 배우고 이 지식을 당신의 IoT 기반 프로젝트에 적용할 수 있을 거예요.

우리의 목표는 ESP32를 사용하여 클라우드 원격 서버로 데이터를 보내고 받아서 그 데이터를 데이터베이스에 저장하여 나중에 사용하는 것이에요. 더불어, 우리는 중앙 웹 인터페이스 대시보드를 구축할 거에요. 여기서 실시간 데이터와 데이터베이스 콘텐츠를 시각화하고 또한 액추에이터 제어를 위해 ESP32에 명령을 보낼 수 있을 거에요.


<div class="content-ad"></div>

# 1. 사용할 기술:

- ESP32와 Arduino 소프트웨어
- MQTT 프로토콜
- Node.js 또는 PHP/Apache 서버
- MySQL 데이터베이스
- GET API 인터페이싱
- Javascript, CSS, HTML

대시보드 인터페이스:

![대시보드 이미지](/assets/img/2024-08-21-ESP32IoT-BasedReal-TimeDataMonitoringSystemfromScratch_1.png)

<div class="content-ad"></div>

# 2. 프로젝트 시각화:

IoT 기반 실시간 데이터 모니터링 시스템은 방 내부에 설치된 센서에서 데이터를 수집, 처리 및 표시하는 종합 솔루션입니다. 사용자는 대시보드에서 실시간 데이터와 일일, 주간, 월간 요약 통계 데이터를 확인할 수 있습니다. 기술 분류:

- 센서 데이터 획득을 위한 ESP32 마이크로컨트롤러.
- 장치 간 효율적인 통신을 위한 MQTT 서버.
- MQTT 메시지를 처리하는 서버 생성을 위한 Node.js.
- 실시간 및 이력 데이터 저장을 위한 MySQL 데이터베이스.
- 데이터베이스와 상호작용하기 위한 API 구축을 위한 PHP.
- 프론트엔드 인터페이스 개발을 위한 HTML/CSS/JavaScript.

# 3. 지시사항:

<div class="content-ad"></div>

- GitHub 프로젝트를 다운로드하세요 (웹 || GitHub).
- 설치 섹션으로 이동하여 필요한 모든 라이브러리를 설치하세요.
- 코드를 ESP32에 업로드하세요.
- MQTT Explorer를 사용하여 데이터 흐름을 확인하세요.
- 적절한 테이블과 자격 증명으로 데이터베이스를 설정하세요 (.env 파일).
- mqtt-listener.js를 실행하세요.
- 데이터 스트림을 확인하기 위해 콘솔 및 MySQL 데이터베이스를 확인하세요.
- index.html 페이지를 방문하여 즐기세요..!

# 4. 설치:

Arduino IDE 라이브러리

- ArduinoJson.h https://github.com/bblanchon/ArduinoJson.git
- WiFi.h https://www.arduino.cc/reference/en/libraries/wifi
- PubSubClient.h https://github.com/knolleary/pubsubclient.git
- Wire.hhttps://www.arduino.cc/reference/en/language/functions/communication/wire

<div class="content-ad"></div>

# MQTT Explorer

이 프로젝트에서는 MQTT 서버에서 데이터를 시각화하기 위해 MQTT Explorer를 활용하고 있습니다. 다운로드 링크:

## 서버 사이드

이 프로젝트에서는 배포를 위해 원격 서버를 사용하고 있습니다. 서버는 클라우드 Linux에서 실행됩니다. 서버를 위해 Linux 또는 다른 운영 체제를 사용 중이라면 다음 명령을 수정하여 사용해 주세요.

<div class="content-ad"></div>

- Node.js 설치

```js
yum install -y gcc-c++ make
```

```js
curl -sL https://rpm.nodesource.com/setup_14.x | sudo -E bash -
```

```js
yum install nodejs
```

<div class="content-ad"></div>

```js
ln -s /usr/lib/node_modules/npm/bin/npm-cli.js /usr/local/bin/npm
```

```js
cp /usr/bin/node /usr/local/bin/node
```

```js
nano /etc/cagefs/conf.d/node.cfg
```

해당 라인들을 추가하고 저장하세요:


<div class="content-ad"></div>


[node]
comment=Node
paths=/usr/local/bin/node


```bash
nano /etc/cagefs/conf.d/npm.cfg
```

아래 라인을 추가하고 저장하세요:


[npm]
comment=NPM
paths=/usr/local/bin/npm


<div class="content-ad"></div>

```js
cagefsctl --force-update
```

Node.js 설치에 대한 전체 안내

2. Node를 통한 MQTT 설치

```js
npm install mqtt
```

<div class="content-ad"></div>

3. Node를 통한 MySQL 설치

```js
npm install mysql
```

4. Composer (옵션)

```js
php composer-setup.php --install-dir=bin --filename=composer
```

<div class="content-ad"></div>

컴포저 설치를 위한 전체 안내

5. Express (옵션): Express를 통해 API를 실행하려는 사용자들을 위해

```js
npm install express
```

6. HTTPS를 위한 파일 시스템 모듈 (옵션): Express를 통해 API를 실행하려는 사용자들

<div class="content-ad"></div>

```js
npm install fs
```

7. 서버에서 실행 중인 MySQL 데이터베이스

# 5. 프로젝트 워크플로우

5.1 데이터 생성, 수집 및 전송

<div class="content-ad"></div>

ESP32 마이크로컨트롤러는 방에 설치된 센서에서 데이터를 수집하는 데 사용됩니다. 현재 ESP32에는 물리적으로 센서가 연결되어 있지 않습니다. 랜덤 함수를 사용하여 실제 센서 데이터에 해당하는 데이터를 생성합니다. 이 응용 프로그램에서는 네 개의 개별 방에 대해 네 가지 매개변수가 고려됩니다: [room1:room4] 온도, 습도, 가스 및 산소. 그런 다음, 이 데이터는 MQTT 프로토콜을 사용하여 MQTT 서버로 전송되어 안정적이고 가벼운 통신이 보장됩니다.

```js
int temperature1 = random(25, 50);
int humidity1 = random(75, 100);
int gas1 = random(40, 80);
int oxygen1 = random(85, 100);
```

```js
int temperature2  = random(25, 50);
int humidity2 = random(75, 100);
int gas2 = random(40, 80);
int oxygen2 = random(85, 100);
```

5.2 MQTT Server

<div class="content-ad"></div>

MQTT 서버로 전송된 데이터는 특정 주제에 기반하여 수신됩니다. MQTT Explorer를 사용하여 전송된 임의의 데이터를 시각화합니다. 네 개의 개별 주제가 개별 방에 대해 선택되었습니다. 개별 방에 대한 데이터는 'sensorReadDelay1' 및 'sensorReadDelay2'로 정의된 일정 시간 간격 후 MQTT 서버로 전송됩니다.

```js
int sensorReadDelay1 = 1000;
int sensorReadDelay2 = 3000;
```

이 프로젝트에서 사용되는 MQTT 서버 주제는 다음과 같습니다:

```js
const char* publishTopic1 = "/SATL/room1";
const char* publishTopic2 = "/SATL/room2";
const char* publishTopic3 = "/SATL/room3";
const char* publishTopic4 = "/SATL/room4";
```

<div class="content-ad"></div>

이 프로젝트에 대한 브로커 서버 URL:

```js
const char* mqtt_server = "broker.emqx.io";
```

이 목록에서 다양한 오픈 소스 MQTT 브로커를 사용할 수 있습니다.

5.3 MQTT Explorer

<div class="content-ad"></div>

MQTT Explorer에서 MQTT Broker 서버에 연결하여 ESP32에서 보낸 데이터를 시각화할 수 있어요. 이전에 제공해드린 특정 주제를 구독하면 됩니다.

![이미지](/assets/img/2024-08-21-ESP32IoT-BasedReal-TimeDataMonitoringSystemfromScratch_2.png)

5.4 서버 측 개발

Node.js 서버가 MQTT 서버로부터 수신된 메시지를 수신하기 위해 설정되었어요. 데이터를 수신하면 서버가 해당 데이터를 구문 분석하여 데이터베이스에 저장합니다. 이를 통해 효율적인 데이터 관리 및 추출이 가능해지며 추가 처리를 위해 데이터가 저장된 특정 테이블을 통해 대시보드로 데이터가 제공됩니다. 현재 네 개의 개별 방에 대한 현재 무작위 데이터는 그래픽 형식으로 데이터를 시각화하기 위해 15초 간격으로 별도의 데이터 테이블에 저장됩니다. 평균 시간 별 데이터, 일별 데이터 및 주간 데이터는 역사적 분석을 위해 별도의 데이터 테이블에 저장됩니다.

<div class="content-ad"></div>

```js
mqtt-listener.js를 실행하여 데이터 스트림을 시작해야 합니다.
```

```js
node mqtt-listener.js
```

데이터베이스 관리 5.5

이 응용 프로그램에서 데이터 저장소로 MySQL 데이터베이스가 사용됩니다. MQTT 서버에서 수신한 데이터를 저장하기 위해 여러 데이터 테이블이 설계되었습니다. 두 테이블은 데이터를 자주 MQTT 서버에서 업데이트하는 즉시 데이터 업데이트 테이블로 지정됩니다. 최소, 최대 및 평균 데이터는 시간당 별도의 테이블에 저장됩니다. 또한 시간 간격에 따라 매일 평균 데이터가 다른 테이블에도 저장되어 있어서, 역사적 분석에 사용됩니다.


<div class="content-ad"></div>

다음은 제공되는 데이터베이스 테이블입니다:

![Database Tables](/assets/img/2024-08-21-ESP32IoT-BasedReal-TimeDataMonitoringSystemfromScratch_3.png)

5.6 API 생성: PHP

PHP를 사용하여 데이터베이스와 상호 작용하는 API를 개발합니다. 이러한 API는 프런트엔드가 데이터베이스에 저장된 데이터를 액세스하고 검색할 수 있는 엔드포인트로 작용하여 프런트엔드와 백엔드 구성 요소 간의 원활한 통합을 가능하게 합니다. 다음은 API 및 샘플 출력 데이터 형식입니다.

<div class="content-ad"></div>

## API 1

End Point:

```js
https://trackingdevice.info/SATL/live-data.php?room=room1
```

응답:

<div class="content-ad"></div>

```json
{"temperature":"29","humidity":"75","gas":"51","oxygen":"92"}
```

## API 2

Endpoint:

```json
https://trackingdevice.info/SATL/all-data.php?room=room1&type=gas
```

<div class="content-ad"></div>

응답:

```js
[{"id":"1","gas":"76","timestamp":"2024-03-28 15:03:47"},
{"id":"2","gas":"62","timestamp":"2024-03-28 15:04:01"},
{"id":"3","gas":"61","timestamp":"2024-03-28 15:04:16"},
{"id":"4","gas":"58","timestamp":"2024-03-28 15:04:31"},
{"id":"5","gas":"60","timestamp":"2024-03-28 15:04:46"},
{"id":"6","gas":"55","timestamp":"2024-03-28 15:05:01"},
{"id":"7","gas":"49","timestamp":"2024-03-28 15:05:16"},
{"id":"8","gas":"55","timestamp":"2024-03-28 15:05:31"},
{"id":"9","gas":"51","timestamp":"2024-03-28 15:05:46"}]
```

## API 3

엔드포인트:

<div class="content-ad"></div>

```js
https://trackingdevice.info/SATL/hour-data.php?room_no=room1&timestamp=2024-03-28
```

응답:

```js
[{"id":1,"room_no":"room1","t_min":"27","t_max":"30","h_min":"75","h_max":"80","g_min":"40","g_max":"42","o_min":"85","o_max":"92","last_updated":"2024-03-28 08:00:00"},
{"id":5,"room_no":"room1","t_min":"27","t_max":"35","h_min":"77","h_max":"85","g_min":"47","g_max":"79","o_min":"86","o_max":"96","last_updated":"2024-03-28 09:00:00"},
{"id":17,"room_no":"room1","t_min":"38","t_max":"30","h_min":"88","h_max":"97","g_min":"78","g_max":"42","o_min":"91","o_max":"99","last_updated":"2024-03-28 10:00:00"},
{"id":21,"room_no":"room1","t_min":"33","t_max":"35","h_min":"78","h_max":"86","g_min":"76","g_max":"47","o_min":"87","o_max":"87","last_updated":"2024-03-28 11:00:00"},
{"id":25,"room_no":"room1","t_min":"27","t_max":"37","h_min":"78","h_max":"82","g_min":"41","g_max":"49","o_min":"87","o_max":"85","last_updated":"2024-03-28 12:00:00"},
{"id":29,"room_no":"room1","t_min":"38","t_max":"30","h_min":"88","h_max":"75","g_min":"57","g_max":"46","o_min":"92","o_max":"85","last_updated":"2024-03-28 13:00:00"},
{"id":41,"room_no":"room1","t_min":"25","t_max":"26","h_min":"79","h_max":"96","g_min":"65","g_max":"67","o_min":"93","o_max":"95","last_updated":"2024-03-28 14:00:00"},
{"id":45,"room_no":"room1","t_min":"27","t_max":"48","h_min":"84","h_max":"78","g_min":"60","g_max":"45","o_min":"87","o_max":"93","last_updated":"2024-03-28 15:00:00"},
{"id":49,"room_no":"room1","t_min":"25","t_max":"31","h_min":"79","h_max":"80","g_min":"60","g_max":"74","o_min":"97","o_max":"92","last_updated":"2024-03-28 16:00:00"},
{"id":53,"room_no":"room1","t_min":"26","t_max":"26","h_min":"75","h_max":"97","g_min":"56","g_max":"55","o_min":"96","o_max":"93","last_updated":"2024-03-28 17:00:00"}]
```

## API 4


<div class="content-ad"></div>

엔드포인트:

```js
https://trackingdevice.info/SATL/daily-data.php?room_no=room1&timestamp=2024-03-28
```

응답:

```js
[
  {
    "id": 12,
    "room_no": "room1",
    "avg_t_min": "30",
    "avg_t_max": "26",
    "avg_h_min": "92",
    "avg_h_max": "80",
    "avg_g_min": "41",
    "avg_g_max": "59",
    "avg_o_min": "90",
    "avg_o_max": "91",
    "last_updated": "2024-03-28 12:30:00"
  }
]
```

<div class="content-ad"></div>

5.7 API 생성: Express (선택 사항)

서버에서 API를 실행하려면 Node를 통해 api.js를 실행할 수 있습니다. API에는 HTTPS 사용을 권장합니다. 코드가 유연하게 설정되어 있어 SSL을 사용할 수 있습니다. HTTPS를 사용하려면 변수 useHTTPS를 true로 설정해야 합니다.

```js
const useHTTPS = true;
```

또한 SSL 서버 인증서를 server.crt 파일에 업로드하고 서버 키를 server.hey 파일에 넣어야 합니다. HTTPS 인증서는 cPanel이나 이 사이트에서 생성할 수 있습니다. server.crt

<div class="content-ad"></div>

```js
-----BEGIN CERTIFICATE-----
인증서 삽입
-----END CERTIFICATE-----
```

서버 키

```js
-----BEGIN RSA PRIVATE KEY-----
개인 키 삽입
-----END RSA PRIVATE KEY-----
```

5.8 프론트엔드 개발

<div class="content-ad"></div>

프런트엔드 인터페이스는 HTML, CSS 및 JavaScript를 사용하여 제작되었으며, 사용자들에게 모니터링된 데이터를 시각화하고 상호 작용할 수 있는 직관적인 플랫폼을 제공합니다. 다이내믹 렌더링과 실시간 업데이트를 통해 사용자들은 간편하게 감시 방조건과 트렌드를 모니터링할 수 있습니다. 프런트엔드는 PHP API로의 AJAX 요청을 통해 최신 센서 읽기를 표시하도록 동적으로 업데이트됩니다. 실시간 데이터는 90밀리초마다 업데이트되며, 그래프 데이터는 500밀리초마다 업데이트됩니다. 이러한 매개 변수는 dataFetch.js 파일에서 변경할 수 있습니다.

# 6. 결론

IoT 기반의 실시간 모니터링 시스템은 각종 기술을 성공적으로 통합하여 사용자들에게 감시 방조건을 모니터링하는 종합적인 솔루션을 제공합니다. ESP32, MQTT, Node.js, PHP 및 HTML/CSS/JavaScript를 활용하여, 시스템은 효율적인 데이터 수집, 처리 및 시각화를 보장하며, 사용자 경험을 향상시키고 체계적인 의사 결정을 내릴 수 있도록 지원합니다.

# 7. 신속한 팁과 디버깅

<div class="content-ad"></div>

- 모든 필수 패키지와 라이브러리가 설치되어 있는지 확인하세요.
- Express API를 사용하려면 서버의 방화벽이 차단하지 않는지 확인하세요. API 통신을 위해 지정된 포트를 열어야 합니다. 이 경우 포트는 4115입니다.
- 데이터베이스 자격 증명을 db.js 파일에 직접 입력하거나 이 시나리오에서 사용한 환경 변수를 사용할 수 있습니다.
- start.js 파일을 실행하여 API와 리스너를 동시에 실행할 수 있습니다.
- 더 많은 업데이트가 금방 이루어질 예정입니다...!

어떤 질문이든 자유롭게 연락하세요:

웹사이트: [https://sudiptomondal.me/](https://sudiptomondal.me/)

이메일: sudipto3331@gmail.com; sudipto.mondal@ieee.org;

<div class="content-ad"></div>

휴대폰: +8801735493331

가이드 라인: [여기를 클릭하세요](https://sudiptomondal.me/projects?name=ESP32%20IoT-Based%20Real-Time%20Data%20Monitoring%20System)