---
title: "M1 맥북 16GB에서 로컬 LLM 파인튜닝하는 방법"
description: ""
coverImage: "/assets/img/2024-08-03-LocalLLMFine-TuningonMacM116GB_0.png"
date: 2024-08-03 19:52
ogImage: 
  url: /assets/img/2024-08-03-LocalLLMFine-TuningonMacM116GB_0.png
tag: Tech
originalTitle: "Local LLM Fine-Tuning on Mac M1 16GB"
link: "https://medium.com/towards-data-science/local-llm-fine-tuning-on-mac-m1-16gb-f59f4f598be7"
isUpdated: true
updatedAt: 1724246346024
---


이 글은 실제로 대형 언어 모델(LLM)을 활용하는 시리즈의 일부입니다. 이전 게시물에서 한 가지 무료 GPU를 사용하여 Google Colab에서 LLM을 세부 조정하는 방법을 보여드렸습니다. 그 예제는 Nvidia 하드웨어에서 쉽게 실행되지만, M-시리즈 Mac에 쉽게 적응되지는 않습니다. 이 글에서는 Mac에서 로컬로 LLM을 세부 조정하는 쉬운 방법을 안내하겠습니다.

![Mac에서 로컬 LLM 세부 조정하기](/assets/img/2024-08-03-LocalLLMFine-TuningonMacM116GB_0.png)

오픈 소스 대형 언어 모델(LLM)과 효율적인 세부 조정 방법이 등장함에 따라 사용자 정의 머신러닝 솔루션을 만드는 것이 이제는 이전보다 쉬워졌습니다. 이제 단일 GPU만 있으면 누구나 로컬 기기에서 LLM을 세부 조정할 수 있습니다.

그러나 Mac 사용자들은 Apple의 M-시리즈 칩으로 인해이 추세에서 상당히 배제되어 왔습니다. 이러한 칩은 통합 메모리 프레임워크를 사용하므로 GPU가 필요하지 않습니다. 따라서 GPU 중심의 오픈 소스 도구에서 실행 및 LLM 훈련에 대한 많은 도구가 (현대 Mac 컴퓨팅 파워를 완전히 활용하지 못하거나 호환되지 않는) Mac과 호환되지 않습니다.

<div class="content-ad"></div>

제가 꿈꿔온 지역 LLM 훈련을 거의 포기했었는데, MLX Python 라이브러리를 발견하게 되어서 다행이에요.

## MLX

MLX는 애플의 머신 러닝 연구팀이 개발한 Python 라이브러리로, Apple 실리콘에서 행렬 연산을 효율적으로 실행할 수 있습니다. 이는 신경망의 주요 계산인 행렬 연산에 중요합니다.

MLX의 주요 장점은 M 시리즈 칩의 통합 메모리 패러다임을 완전히 활용한다는 점인데, 이는 제같이 선박하고 있는 시스템(M1 16GB)에서도 대형 모델(Mistral 7b Instruct 등)에 대한 세밀한 조정 작업을 실행할 수 있게 해줍니다.

<div class="content-ad"></div>

도서관에는 Hugging Face와 같은 모델 학습을 위한 고수준 추상화가 없지만, 다른 사용 사례에 쉽게 적용하고 수정할 수 있는 LoRA의 예제 구현이 있습니다.

아래 예제에서 바로 그렇게 하게 됩니다.

# 예시 코드: Mistral 7b Instruct 파인튜닝

이 예제는 이전 글에서 본 것과 비슷합니다. 그러나 Hugging Face의 Transformers 라이브러리와 Google Colab 대신 MLX 라이브러리와 내 로컬 머신(2020 맥 미니 M1 16GB)을 사용합니다.

<div class="content-ad"></div>

이전 예제와 비슷하게, Mistral-7b-Instruct의 양자화된 버전을 세밀하게 조정하여 YouTube 댓글에 내 비평을 답변할 것입니다. 저는 QLoRA 매개변수 효율적 세부 조정 방법을 사용합니다. QLoRA에 익숙하지 않다면, 이 방법에 대한 개요가 여기에 있습니다.

🔗 GitHub 저장소 | 훈련 데이터 | 사전 훈련된 모델

## 1) 환경 설정

예제 코드를 실행하기 전에 Python 환경을 설정해야 합니다. 첫 번째 단계는 GitHub 저장소에서 코드를 다운로드하는 것입니다.

<div class="content-ad"></div>

```js
git clone https://github.com/ShawhinT/YouTube-Blog.git
```

이 예제의 코드는 LLMs/qlora-mlx 하위 디렉토리에 있습니다. 이 폴더로 이동하여 새 Python 환경을 만들 수 있습니다 (여기서는 mlx-env로 부릅니다).

```js
# 디렉토리 변경
cd LLMs/qlora-mlx

# py venv 생성
python -m venv mlx-env
```

다음으로 환경을 활성화하고 requirements.txt 파일에서 필요한 패키지를 설치합니다. 알림: mlx는 M 시리즈 칩, Python `= 3.8 및 macOS `= 13.5가 있는 시스템을 요구합니다.

<div class="content-ad"></div>


# 가상 환경 활성화
source mlx-env/bin/activate

# 요구 사항 설치
pip install -r requirements.txt


## 2) 사전 훈련되지 않은 모델을 사용한 추론

mlx와 기타 종속 항목을 설치했으므로 파이썬 코드를 실행해 봅시다! 먼저 유용한 라이브러리를 가져와서 시작합시다.


# 모듈 가져오기 (이제 이것은 Python 코드입니다)
import subprocess
from mlx_lm import load, generate


<div class="content-ad"></div>

subprocess 모듈을 사용하여 Python을 통해 터미널 명령을 실행하고 mlx-lm 라이브러리를 사용하여 사전 훈련된 모델에서 추론을 실행할 것입니다.

mlx-lm은 mlx 위에 구축되었으며 Hugging Face 허브의 모델을 실행하는 데 특히 만들어졌습니다. 기존 모델에서 텍스트를 생성하는 방법은 다음과 같습니다.

```js
# 입력 정의
model_path = "mlx-community/Mistral-7B-Instruct-v0.2-4bit"
prompt = prompt_builder("Great content, thank you!")
max_tokens = 140

# 모델 로드
model, tokenizer = load(model_path)

# 응답 생성
response = generate(model, tokenizer, prompt=prompt, 
                                      max_tokens=max_tokens, 
                                      verbose=True)
```

참고: Hugging Face mlx-community 페이지의 수백 개 모델 중 어느 것이든 쉽게 추론에 사용할 수 있습니다. 사용할 수 없는 모델을 사용하려면 (그럴듯한 경우), 호환 가능한 형식으로 변환하기 위해 scripts/convert.py 스크립트를 사용할 수 있습니다.

<div class="content-ad"></div>

prompt_builder() 함수는 YouTube 댓글을 가져와 아래에 나와 있는 프롬프트 템플릿에 통합합니다.

```js
# 프롬프트 형식
intstructions_string = f"""ShawGPT가 YouTube에서 가상 데이터 과학 컨설턴트로서 활동하며, 명확하고 접근 가능한 언어로 소통하며, 요청 시 기술적인 내용으로 발전합니다. \
적절하게 피드백에 반응하고 응답을 '–ShawGPT'로 마무리합니다. ShawGPT는 시청자의 댓글 길이에 맞게 응답을 맞춤화하여 간결한 칭찬이나 피드백에 간략한 인정을 제공하여 상호 작용을 자연스럽고 매혹적으로 유지합니다.

다음 댓글에 답변해 주세요.
"""

# 람다 함수 정의
prompt_builder = lambda comment: f'''<s>[INST] {intstructions_string} \n{comment} \n[/INST]\n'''
```

모델이 "Great content, thank you!" 라는 댓글에 대해 파인 튜닝 없이 응답하는 방식은 다음과 같습니다.

```js
–ShawGPT: 친절한 말씀 감사드립니다! 콘텐츠가 도움이 되고 즐거우셨다니 기쁩니다.
더 자세한 질문이나 다루고 싶은 주제가 있으면 자유롭게 물어봐 주세요!
```

<div class="content-ad"></div>

응답은 일관성이 있지만, 2가지 주요 문제가 있습니다. 1) 서명 "-ShawGPT"이 응답의 맨 앞에 위치해야 하는 대신 뒤에 위치하고 있습니다(지시사항대로), 그리고 2) 응답이 실제로 이런 댓글에 대답할 때보다 훨씬 길어요.

## 3) 훈련 데이터 준비

Fine-tuning 작업을 실행하기 전에, 훈련, 테스트, 및 검증 데이터셋을 준비해야 합니다. 여기에서는 50개의 실제 댓글과 응답을 훈련에, 10개의 댓글/응답을 검증과 테스트에 사용합니다 (총 70개의 예제).

아래에 훈련 예제가 제공됩니다. 이는 JSON 형식으로 되어 있으며, 즉 키-값 쌍으로 표현되는데, 여기서 키는 "text"이고 값은 prompt, 댓글, 응답이 합쳐진 것입니다.

<div class="content-ad"></div>

```json
{"text": "<s>[INST] ShawGPT은 YouTube에서 가상 데이터 과학 컨설턴트로 작동하며 명쾌하고 접근 가능한 언어로 소통하며 요청 시 기술적인 심도로 진행됩니다. 그것은 적절하게 피드백에 반응하고 응답을 '\u2013ShawGPT'로 끝냅니다. ShawGPT는 시청자의 댓글과 일치하도록 응답의 길이를 맞추며 간결한 감사 또는 피드백 표현에 간결한 인사를 제공하여 상호작용을 자연스럽고 매력적으로 유지합니다.\n\n다음 댓글에 대답해주세요.\n \nLLM에 대한 매우 철저한 소개였고 제가 가지고 있던 많은 질문에 답변했습니다. 감사합니다. \n[/INST]\n들어보니 좋다는 소식 참 반가워요 :) -ShawGPT</s>"}
```

.train, .test 및 .val 데이터셋을 .csv 파일에서 생성하는 코드는 GitHub에서 이용 가능합니다.

## 4) 모델 세부 튜닝

준비된 훈련 데이터로 모델을 세부 튜닝할 수 있습니다. 여기서는 mlx 팀이 작성한 lora.py 예제 스크립트를 사용합니다.

<div class="content-ad"></div>

저희가 복제한 리포지토리의 scripts 폴더에 이 스크립트가 저장되어 있고, train/test/val 데이터는 data 폴더에 저장되어 있습니다. 파인튜닝 작업을 실행하려면 다음 터미널 명령어를 실행할 수 있어요.

```js
python scripts/lora.py --model mlx-community/Mistral-7B-Instruct-v0.2-4bit \
                       --train \
                       --iters 100 \
                       --steps-per-eval 10 \
                       --val-batches -1 \
                       --learning-rate 1e-5 \
                       --lora-layers 16 \
                       --test

# --train = LoRA 훈련 실행
# --iters = 훈련 단계 수
# --steps-per-eval = val 손실을 계산하기 전에 수행할 단계 수
# --val-batches = val 손실에 사용할 val 데이터 집합 예제 수 (-1 = 전부)
# --learning-rate (기본값과 동일)
# --lora-layers (기본값과 동일)
# --test = 훈련 종료 시 테스트 손실 계산
```

훈련을 가능한 한 빠르게 진행하기 위해 나머지 모든 프로세스를 종료하여 파인튜닝 프로세스에 최대한 많은 메모리를 할당했어요. 16GB 메모리를 가진 M1에서 이 작업은 실행하는 데 약 15~20분이 소요되었고 메모리 사용량은 약 13~14GB로 증가했습니다.

참고: 오버피팅을 피하기 위해 lora.py 스크립트의 340~341번 줄에서 LoRA 어댑터의 랭크를 r=8에서 r=4로 변경했어야 했어요.

<div class="content-ad"></div>

## 5) 미세 조정된 모델을 사용하여 추론하기

학습이 완료되면 작업 디렉토리에 adapters.npz라는 파일이 생성됩니다. 이 파일에는 LoRA 어댑터 가중치가 포함되어 있습니다.

이를 사용하여 추론을 실행하려면 lora.py를 다시 사용할 수 있습니다. 그러나 이번에는 스크립트를 터미널에서 직접 실행하는 대신 subprocess 모듈을 사용하여 Python에서 스크립트를 실행했습니다. 이를 통해 이전에 정의한 prompt_builder() 함수를 사용할 수 있었습니다.

```js
# 입력값 정의
adapter_path = "adapters.npz" # 기본값과 동일
max_tokens_str = "140" # 문자열로 지정해야 함

# 명령어 정의
command = ['python', 'scripts/lora.py', '--model', model_path, 
                                        '--adapter-file', adapter_path, 
                                        '--max-tokens', max_tokens_str, 
                                        '--prompt', prompt]

# 명령어 실행하고 결과를 지속적으로 출력
run_command_with_live_output(command)
```

<div class="content-ad"></div>

run_command_with_live_output()은 ChatGPT의 친절한 도움 함수로, 터미널 명령의 프로세스 출력을 지속적으로 출력합니다. 이를 통해 추론이 완료될 때까지 기다릴 필요 없이 출력을 볼 수 있습니다.

```js
def run_command_with_live_output(command: list[str]) -> None:
    """
    ChatGPT의 친절함:
    명령을 실행하고 실행 중에 한 줄씩 출력합니다.

    Args:
        command (List[str]): 실행할 명령과 그 인수들입니다.

    Returns:
        None
    """
    process = subprocess.Popen(command, stdout=subprocess.PIPE, stderr=subprocess.PIPE, text=True)

    # 출력을 한 줄씩 출력
    while True:
        output = process.stdout.readline()
        if output == '' and process.poll() is not None:
            break
        if output:
            print(output.strip())
        
    # 에러 출력이 있는 경우 출력
    err_output = process.stderr.read()
    if err_output:
        print(err_output)
```

동일한 코멘트(Great content, thank you!)에 대한 모델의 응답을 미세 조정 후 보여줍니다.

```js
즐겁게 즐겨주셔서 기뻐요! -ShawGPT
```

<div class="content-ad"></div>

이 회신은 이전의 세부 조정보다 훨씬 나아졌어요. "-ShawGPT" 서명이 올바른 위치에 있고, 제가 실제로 할 말인 것처럼 들립니다.

하지만, 이 코멘트보다 좀 더 도전적인 것을 봅시다. 아래와 같은 내용을 봐요.

```js
코멘트:
어제 당신의 채널을 발견했고 정말 중독되었어요. 수고하셨어요. HF를 사용해서 ShawGPT를 세부 조정하는 비디오를 보면 좋을 것 같아요. Mistal-7b를 사용해 Colab에서 실행하는 비디오를 본 적이 있어요. 노트북(Mac)이나 HF Spaces를 사용한 비디오를 올리실 계획이 있나요?

회신:
감사합니다, 즐겁게 보셨다니 다행이에요! 제 노트북에서 세부 조정 비디오를 기대하고 있어요. HF API의 최신 버전을 실행하는 M1 Mac Mini가 있어요. -ShawGPT
```

<div class="content-ad"></div>

첫 번째 눈에 들었을 때, 이 응답은 정말 좋습니다. 모델은 적절하게 응답하고 적절한 마무리를 합니다. 또한, 나에게 M1 맥 미니가 있다고 맞히기에도 성공하네요 😉

하지만, 이에 두 가지 문제가 있습니다. 첫째, 맥 미니는 노트북이 아니라 데스크탑입니다. 둘째, 이 예제는 HF API를 직접 사용하지 않습니다.

# 다음 단계는?

여기서 M-시리즈 맥을 위한 간단한 로컬 파인튜닝 예제를 공유했습니다. 이 예제의 데이터와 코드는 GitHub 레포지토리에서 자유롭게 이용할 수 있습니다.

<div class="content-ad"></div>

제가 도움이 되었으면 좋겠어요. 이 내용이 여러분의 사용 사례에 도움이 되길 바랍니다. 이 시리즈의 미래 콘텐츠에 대한 제안이 있으면 댓글로 알려주세요 :)

LLM에 대해 더 알아보기 👇

## 자료

- 연락하기: 제 웹사이트 | 통화 예약

<div class="content-ad"></div>

소셜 미디어: YouTube 🎥 | LinkedIn | Instagram

지원하기: 커피 사주기 ☕️

[1] MLX

[2] 원본 코드

<div class="content-ad"></div>

[3] LoRA 논문