---
title: "라즈베리 파이 5에서 Piper TTS를 활용한 텍스트 음성 변환 방법"
description: ""
coverImage: "/assets/img/2024-08-26-EasyGuidetoText-to-SpeechonRaspberryPi5UsingPiperTTS_0.png"
date: 2024-08-26 17:29
ogImage: 
  url: /assets/img/2024-08-26-EasyGuidetoText-to-SpeechonRaspberryPi5UsingPiperTTS_0.png
tag: Tech
originalTitle: "Easy Guide to Text-to-Speech on Raspberry Pi 5 Using Piper TTS"
link: "https://medium.com/@vadikus/easy-guide-to-text-to-speech-on-raspberry-pi-5-using-piper-tts-cc5ed537a7f6"
isUpdated: true
updatedAt: 1724743605615
---


![Piper TTS](/assets/img/2024-08-26-EasyGuidetoText-to-SpeechonRaspberryPi5UsingPiperTTS_0.png)

파이퍼 TTS는 라즈베리 파이 4용으로 최적화된 특수 로컬 신경망 텍스트 음성 변환 시스템으로, 라즈베리 파이 5에도 적용할 수 있습니다. 빠르고 안정적인 음성 출력을 제공하며 오프라인, 집 내 네트워크 또는 온라인 상태에서 작동할 수 있습니다. 이러한 다양성으로 라즈베리 파이 프로젝트에 이중상태 인터넷 접속 불가 또는 홈 기반 IoT 시스템의 일부인 경우에 이상적입니다. 제가 학습 목적으로 만든 것처럼 특수 경량 텍스트 음성 처리 서버에 특화된 프로젝트에 특히 유용합니다.

파이퍼 TTS를 설정에 통합하면 이를 통해 기술 생태계의 상호작용하는 구성 요소로 변환하여 기능을 향상시키고 응용 범위를 확대할 수 있습니다. 기본적으로 라즈베리 파이에 목소리를 부여하여 기술 설정의 중추 요소로 만들어줍니다.

이 곳의 진정한 가치는 라즈베리 파이가 파이썬 스크립트를 실행한다는 것입니다. 이는 거의 상상할 수 있는 모든 방법으로 프로그래밍할 수 있게 해줍니다. 이를 통해 간단한 자동화 작업부터 복잡한 IoT 시스템까지 프로젝트를 광범위하게 사용자 정의하고 적응시킬 수 있습니다.

<div class="content-ad"></div>

# IoT 프로젝트에 라즈베리 파이를 선택하는 이유는 무엇인가요?

일반적인 노트북, PC 또는 태블릿과 같은 기기 대신 라즈베리 파이나 유사한 제품을 선택하는 이유는 종종 해당 기기가 영구적인 IoT 중심 설치에 적합하다는 점입니다. 라즈베리 파이는 비용 효율성과 유연성, 그리고 충분한 처리 능력을 결합하여, 디지털 어시스턴트부터 가정용 미디어 시스템 그리고 환경 모니터링 스테이션과 같은 다양한 응용 프로그램을 수용할 수 있습니다. 이러한 기기들은 지속적으로 작동해야 하므로 장기간의 데이터 집중적인 작업에 이상적입니다.

# 기술 분야에서 약간의 재미 요소

다양한 리뷰에 따르면, 라즈베리 파이 5는 연산 능력 측면에서 자신의 전임자보다 약 2~3배 더 빠르다고 선언되었습니다. 최고 모델에서는 더 빠른 I/O 속도와 더 많은 메모리를 자랑합니다. 각각의 구체적인 작업은 정확한 숫자를 얻기 위해 별도의 벤치마킹이 필요하지만, 이러한 추정치는 일반적인 비교에 도움이 됩니다.

<div class="content-ad"></div>

이러한 작업에는 Docker를 사용하는 것이 좋습니다. 도커는 확장 가능하고 격리된 솔루션을 제공하며 편리한 API로 래핑되어 쉽게 문서화할 수 있습니다. 이 기사에서는 ARM64용 Ubuntu를 기본 시스템으로 사용하여 동일한 방법을 채택할 것입니다. 이 지침은 라즈베리 파이 OS를 사용하는 설정에도 적용될 수 있으며, 주요 요구 사항은 올바르게 설정된 Docker 환경입니다.

Piper TTS에 대한 자세한 내용은 해당 GitHub 페이지에서 확인할 수 있습니다. 소스로부터 빌드하거나 미리 컴파일된 이진 패키지를 활용하는 방법에 대한 단계별 지침은 Piper TTS 설치 가이드를 참조하시기 바랍니다. 라즈베리 파이 5를 위한 미리 만들어진 레시피는 없지만 piper_linux_aarch64.tar.gz에서 제공되는 AArch64용 빌드된 이진 파일은 잘 작동합니다. 그러나 우리는 프로세스의 자세한 설명을 제공하기 위해 소스로 패키지를 빌드하는 방식을 계속할 것입니다.

# Piper 보이스

Piper를 위한 사용 준비가 된 보이스 패키지는 Hugging Face와 Piper GitHub 페이지의 VOICE.md 파일에서 제공됩니다. 각 패키지에는 ONNX 형식의 모델 자체와 JSON 구성 파일을 포함합니다. 또한, MODEL_CARD는 각 모델 및 사용 방법에 대한 라이선싱 정보 및 자세한 설명을 제공합니다. 예제 모델 aru medium은 다음 Docker 패키지에 포함되어 있지만, 사용자 기호에 따라 더 많은 보이스를 다운로드할 수 있습니다.

<div class="content-ad"></div>

# 우분투에 Docker Engine 설치하기

이 단계를 따라 우분투 시스템에 Docker Engine이 올바르게 설치되었는지 확인하세요. 이미 시스템에 Docker가 설치되어 있는 경우 이 섹션을 건너뛰고 Dockerfile 장으로 진행할 수 있습니다. 더 포괄적인 가이드가 필요하다면 우분투용 공식 Docker 설치 가이드를 참고하세요.

## 충돌 가능성이 있는 패키지 제거

```js
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done
```

<div class="content-ad"></div>

## 도커의 공식 GPG 키 추가

```js
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
```

## 저장소 추가

```js
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

<div class="content-ad"></div>

## 패키지 업데이트

```js
sudo apt-get update
```

## 최신 패키지 설치

```js
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

<div class="content-ad"></div>

## 설치 확인

```js
sudo docker run hello-world
```

# Dockerfile

작업 디렉토리에 다음 내용을 가진 Dockerfile을 생성하세요:

<div class="content-ad"></div>

```js
# 특정하고 안정적인 Ubuntu 버전 사용
FROM ubuntu:24.04 AS main

ARG DEBIAN_FRONTEND=noninteractive

# 기본 쉘을 bash로 교체
RUN rm /bin/sh && ln -s /bin/bash /bin/sh

# 코드 디렉토리 경로 설정
ENV CODE_PATH=/opt

# 필요한 패키지 업데이트 및 설치, 정리
RUN apt-get update && DEBIAN_FRONTEND=noninteractive TZ=Etc/UTC apt-get -y install tzdata \
	build-essential apt-utils vim bzip2 tar gzip git curl wget python3 python3-pip cmake ca-certificates \
	&& rm -rf /var/lib/apt/lists/*

# 파이썬 pip 설정
RUN echo $'[global]\nbreak-system-packages = true' > /etc/pip.conf

# pip 업그레이드
RUN pip install --quiet --upgrade pip --ignore-installed

# Piper TTS 레포지토리 복제
RUN git clone https://github.com/rhasspy/piper.git $CODE_PATH/piper
WORKDIR $CODE_PATH/piper
# 테스트된 Piper 버전 사용
RUN git checkout tags/2023.11.14-2

# Piper Phonemize 설치 및 설정
WORKDIR $CODE_PATH/piper/lib
RUN mkdir -p Linux-arm64/piper_phonemize
WORKDIR Linux-arm64/piper_phonemize
# 테스트된 Piper Phonemize 버전 사용
RUN wget https://github.com/rhasspy/piper-phonemize/releases/download/v1.1.0/libpiper_phonemize-arm64.tar.gz \
	&& tar -xf libpiper_phonemize-arm64.tar.gz \
	&& rm libpiper_phonemize-arm64.tar.gz \
	&& cp -r $CODE_PATH/piper/lib/Linux-arm64/piper_phonemize/lib/espeak-ng-data /usr/share/espeak-ng-data

# Piper 빌드 및 테스트
WORKDIR $CODE_PATH/piper
RUN cmake -Bbuild -DCMAKE_INSTALL_PREFIX=install
RUN cmake --build build --config Release
RUN cmake --install build
WORKDIR build
RUN ctest --config Release

# Piper를 위한 짧은 별칭 생성
RUN echo "alias piper='$CODE_PATH/piper/build/piper'" >> ~/.bashrc \
	&& source ~/.bashrc

# 모델 디렉토리 생성 및 샘플 음성 추가
RUN mkdir -p $CODE_PATH/piper/voices/aru/medium

# 각 URL을 새 줄에 포함한 목록 파일 생성
RUN echo -e "https://huggingface.co/rhasspy/piper-voices/resolve/main/en/en_GB/aru/medium/en_GB-aru-medium.onnx\n \
https://huggingface.co/rhasspy/piper-voices/resolve/main/en/en_GB/aru/medium/en_GB-aru-medium.onnx.json\n \
https://huggingface.co/rhasspy/piper-voices/resolve/main/en/en_GB/aru/medium/MODEL_CARD" > aru_medium_model_file_urls.txt

# 파일에 나열된 모든 URL 다운로드
RUN wget -i aru_medium_model_file_urls.txt -P $CODE_PATH/piper/voices/aru/medium

# 코드 경로로 이동
WORKDIR $CODE_PATH
```

# 라즈베리파이 5에서 Piper TTS 도커 컨테이너 빌드 및 관리

## 도커 이미지 빌드

시스템에 NVMe 저장소가 장착되어 있으면 빌드 프로세스에 약 4-5분 정도 소요됩니다. 빌드 시간은 라즈베리파이 5의 저장장치 성능과 RAM 구성에 따라 다를 수 있습니다. SD 카드 성능이 충분하지 않을 수 있어 빌드 시간이 늘어날 수 있으므로 NVMe 저장소 사용을 권장합니다. 그러나 SD 카드도 호환됩니다.

<div class="content-ad"></div>

라즈베리 파이 5에서 piper-tts-rpi5 도커 이미지를 빌드하려면 Dockerfile이 있는 작업 디렉토리에서 다음 명령을 실행하세요:

```js
sudo docker build -t piper-tts-rpi5 .
```

## Docker 컨테이너 실행

도커 컨테이너를 처음 실행하고 이름을 piper-tts로 설정하려면 다음 명령을 사용하세요:

<div class="content-ad"></div>

```js
sudo docker run --name piper-tts -it piper-tts-rpi5
```

## 컨테이너 내부

Piper Alias 확인: 컨테이너 내부에서 다음 명령어를 실행하여 piper 별칭이 올바르게 설정되었는지 확인하세요:

```js
piper -h
```

<div class="content-ad"></div>

추론 실행: 텍스트를 음성으로 변환하려면 다음 명령을 사용하세요:

```js
echo "To address the elephant in the room: using text-to-speech technology isn’t just practical, it’s a lot of fun too!" | piper --model /opt/piper/voices/aru/medium/en_GB-aru-medium.onnx --output_file /opt/welcome.wav
```

컨테이너에서 나오기:

```js
exit
```

<div class="content-ad"></div>

## 호스트 머신으로 결과 파일 복사하기

도커 컨테이너에서 생성된 오디오 파일을 호스트 머신으로 복사하려면 다음을 실행하세요:

```js
docker cp piper-tts:/opt/welcome.wav ./
```

## 컨테이너 관리

<div class="content-ad"></div>

초기 설정 후에는 필요할 때 컨테이너를 이름으로 시작하고 중지할 수 있어요:

```bash
sudo docker start piper-tts
sudo docker stop piper-tts
```

## 컨테이너 외부

호스트 머신에서 추론 실행: 컨테이너에 들어가지 않고 텍스트를 음성으로 변환하는 추론을 수행하려면 다음 명령을 사용하세요:

<div class="content-ad"></div>

```js
sudo docker exec -ti piper-tts sh -c 'echo "방 안 코끼리에 대한 문제를 다루자면: 텍스트 음성 변환 기술을 사용하는 것은 실용적일 뿐만 아니라 재미도 많이 있어요!" | /opt/piper/build/piper --model /opt/piper/voices/aru/medium/en_GB-aru-medium.onnx --output_file /opt/welcome.wav'
```

결과 데이터 복사: 결과는 다음과 같이 호스트로 복사할 수 있습니다:

```js
docker cp piper-tts:/opt/welcome.wav ./
```

## 생성된 .wav 파일 다운로드하기


<div class="content-ad"></div>

라즈베리 파이에서 생성된 .wav 파일을 다운로드하는 방법은 몇 가지가 있습니다. 그 중 가장 편리한 방법 중 하나는 SSH 복사 명령을 사용하는 것입니다. 다음 명령을 실행하여 라즈베리 파이에서 파일을 안전하게 로컬 기기로 전송할 수 있습니다. 이때 라즈베리 파이 5 호스트 이름과 경로를 실제 정보로 업데이트해야 합니다:

```js
scp rpi5-hostname:/path/to/file.wav ./local/pc/path/
```

이 명령은 SCP (Secure Copy Protocol)를 사용하여 SSH를 통해 파일을 안전하게 전송하므로 파일을 검색하는 믿을 만한 선택지입니다.

# 결론

<div class="content-ad"></div>

이 기본 지침은 라즈베리 파이 5 호스팅 파이퍼 TTS 프로토 타입을 시작하는 데 필요한 필수 요소를 제공합니다. 단계는 직관적인 방식으로 설명되어 있어 추가 자동화를 통해 조정하고 개선하거나 필요에 따라 Ubuntu 사용자를 만들고 파일 경로를 조정할 수 있습니다. 이 안내서가 유용하다고 생각되거나 여러분의 프로젝트에 몇 가지 아이디어를 준다면 좋아요 버튼을 눌러 다른 사람들과 공유하기를 망설이지 마세요.

더 많은 통찰력과 토론을 위해 저와 연결하세요:

- LinkedIn
- Twitter

읽어 주셔서 감사합니다!