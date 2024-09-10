---
title: " ChatGPT, Leonardo.ai 활용하여 하루 144개의 완전 자동화된 YouTube 비디오 만드는 방법 (전체 소스 코드 공개)"
description: ""
coverImage: "/assets/img/2024-09-10-HowICreated144FullyAutomatedYouTubeVideosPerDayUsingChatGPTLeonardoaiFullSourceCode_0.png"
date: 2024-09-10 19:25
ogImage: 
  url: /assets/img/2024-09-10-HowICreated144FullyAutomatedYouTubeVideosPerDayUsingChatGPTLeonardoaiFullSourceCode_0.png
tag: Tech
originalTitle: " How I Created 144 Fully Automated YouTube Videos Per Day Using ChatGPT , Leonardo.ai  (Full Source Code)"
link: "https://medium.com/@zerocodingstartup/how-i-created-144-fully-automated-youtube-videos-per-day-using-chatgpt-leonardo-ai-475deb4ab63e"
isUpdated: false
---


자동 콘텐츠 생성 도구인 Dubdup이나 Canva와 같은 도구의 등장으로 YouTube Shorts를 수천 개 만들 수 있는 방법에 대한 관심이 높아지고 있어요. 모두 “AI 부업으로 일일 $XXX” 라고 말했죠.하지만

<img src="/assets/img/2024-09-10-HowICreated144FullyAutomatedYouTubeVideosPerDayUsingChatGPTLeonardoaiFullSourceCode_0.png" />

이 같은 주장에 푹 빠져, 저는 ChatGPT와 Leonardo.ai와 같은 혁신적인 AI LLM 기술을 활용하여 완전 자동화된 YouTube 비디오를 만들어 보기로 결심했어요. 제 목표는 이 예시와 같이, 매일 144개의 5~10 분 짜리 비디오를 규모에 맞게 제작하는 것이었어요. 제 3의 도구에 의존하지 않고 이를 달성하는 방법에 대한 상세한 안내입니다. 발생한 인사이트와 직면한 과제를 포함해 보여드릴게요.

# 프로세스 개요

<div class="content-ad"></div>

이 게시물에서는 YouTube 비디오 제작을 자동화하는 스크립트를 만드는 과정을 소개하겠습니다. 비디오 주제 생성부터 썸네일 제작까지, 이 end-to-end 솔루션은 콘텐츠 제작에서 AI의 능력과 잠재력을 보여줍니다. 아래는 주요 기능과 목적에 대한 요약입니다:

- 주제 생성: ChatGPT를 사용하여 자기 계발 YouTube 채널에 대한 매력적이고 관련성 높은 주제를 권장합니다.
- YouTube 스크립트 생성: 동영상을 위한 자세하고 매력적인 스크립트를 작성하여 소개, 주요 포인트, 개인 이야기, 그리고 동기부여적 결론을 포함하도록 합니다. 불필요한 문자를 제거하여 텍스트 음성 출력의 품질을 향상시키기도 합니다.
- Leonardo Prompts 생성: Leonardo.ai에 이미지를 생성하기 위한 프롬프트를 개발합니다. 이러한 이미지는 해당 비디오 콘텐트를 반영하는 이미지로 생성되며, 해당 문서에 명시된 대로 Leonardo.ai의 모션 생성 API를 사용하여 업스케일링 및 애니메이션화됩니다.
- 텍스트 음성 변환: YouTube 스크립트를 고품질의 오디오 내레이션으로 변환하기 위해 Google Cloud의 텍스트 음성 API를 사용합니다. 이 설정은 다큐멘터리 스타일 내레이션을 위한 목소리를 구성하는 것도 포함합니다.
- 음악 검색 및 다운로드: 처음에는 Pixabay API를 사용하여 음악을 검색하려 했지만, 제한사항으로 인해 로열티 프리 음악을 로컬로 다운로드하기로 결정했습니다. 또는 YouTube Studio에서 음악을 다운로드할 수도 있습니다.
- 비디오와 음악 및 TTS 결합: 생성된 비디오 클립, 배경 음악, 내레이션을 한데 모아 일관된 비디오로 만듭니다. 이는 비디오 클립 간에 크로스 디졸브 전환을 추가하고 자막을 오버레이하는 것을 포함합니다.
- YouTube 썸네일 생성: 비디오의 한 장면을 사용하여 시각적으로 매력적인 썸네일을 생성하고 중앙에 비디오 제목 텍스트를 추가합니다.
- 콘텐츠 저장 및 조직화: 생성된 모든 콘텐츠, 비디오, 썸네일, 스크립트를 구조화된 폴더 시스템에 저장합니다.
- 스케줄링으로 자동화: 스케줄 라이브러리를 사용하여 스크립트를 하루에 여러 번 실행하여 지속적인 비디오 제작을 가능하게 합니다.

그러나 YouTube API를 통해 비디오를 업로드하려면 추가적인 검증이 필요하므로 제가 직접 자동으로 생성된 비디오를 업로드하기로 결정했습니다. ChatGPT의 도움으로 이 프로젝트의 코드를 단 2시간 만에 작성할 수 있었습니다. 매일 144개의 비디오를 만들어 매 10분마다 한 개의 비디오를 제작하는 방법에 대한 상세 안내는 여기에서 확인할 수 있습니다.

<div class="content-ad"></div>

# 요청 및 조정 요약

- 초기 요청: 성공한 사람들의 마인드셋과 동기에 초점을 맞춘 YouTube 채널을 만들기로 했습니다.
- 스크립트 생성: 어색한 구절을 최소화하면서 매력적인 콘텐츠를 제작할 수 있는 스크립트 생성기가 필요했습니다. "주최자:"와 같은 불필요한 문자를 제거하여 부드러운 텍스트 음성 변환을 보장하기 위한 작업을 포함합니다.
- 이미지 및 비디오 생성: Leonardo.ai를 사용하여 이미지를 생성하고 확대한 후 비디오로 변환했습니다. 이 프로세스는 Leonardo.ai 문서를 참조하여 진행되었습니다.
- 텍스트 음성 변환: Google Cloud Text-to-Speech를 사용하여 서사형 나레이션을 구성했습니다. 이 서비스는 무료 할당량을 제공하며, 신규 사용자는 $300의 무료 크레딧을 받습니다. 이 서비스를 사용하려면 Google Cloud 계정을 생성하고 API를 활성화해야 합니다.
- 자막 생성: OpenAI의 Whisper API를 사용하여 자막을 생성했습니다.
- 배경 음악과 전환: 배경 음악으로 Pixabay API를 사용해보려고 했으나, 음악 검색을 위한 API를 제공하지 않아 로열티 프리 음악을 로컬이나 YouTube Studio에서 다운로드하기로 결정했습니다.
- 썸네일 제작: 썸네일은 Python Imaging Library (PIL)을 사용하여 생성했습니다.
- 콘텐츠 저장 및 업로드: 모든 생성된 콘텐츠를 구조화된 폴더에 저장하고 YouTube에 업로드했습니다.
- YouTube에 비디오 업로드: Youtube Data API를 사용하여 제목, 설명 및 썸네일이 생성된 모든 비디오를 업로드했습니다.
- 스크립트 예약: Python의 schedule 라이브러리를 사용하여 스크립트를 하루에 여러 번 실행했습니다.

# ChatGPT를 통한 아이디어 구상

ChatGPT와의 전체 프로세스는 간단하면서도 강력했습니다. 처음에는 ChatGPT에게 YouTube 채널을 위한 셀프 개선 및 동기 부여 콘텐츠 생성을 위한 아이디어 생성 기능을 맡겼습니다. 그리고 ChatGPT의 API를 활용하여 비디오 콘텐츠를 위한 상세한 스크립트를 생성하는 함수를 요청했습니다. 생성된 스크립트를 손에 넣은 후, Leonardo.ai를 사용하여 이미지와 비디오를 만들기 위해 적합한 5-10가지 프롬프트를 생성할 수 있는 함수를 ChatGPT에 요청했습니다.

<div class="content-ad"></div>

다음으로, Python API를 통해 텍스트 음성 변환(TTS) 및 비디오 편집을 처리하는 기능을 찾았어요. ChatGPT이 처음에는 내장 TTS 라이브러리 gtts를 추천했지만, 기존의 Google Cloud 계정이 있어 Google Cloud Text-to-Speech API를 사용하기로 결정했어요. 이 API는 더 자연스럽고 다양한 음성 합성 기능을 제공합니다.

또한, Leonardo.ai를 사용하여 이미지 생성 및 확대 기능, 이러한 이미지를 비디오로 변환하고 음성 오버와 일치시키기 위해 크로스 디졸브 효과를 적용하는 기능을 요청했어요. 게다가 로열티 프리 배경 음악과 비디오 내에 자막을 통합하고 싶었어요. 결과물을 간단하게 만들기 위해, 생성된 모든 콘텐츠를 타임스탬프가 찍힌 폴더에 저장하는 기능을 요청했고요. 마지막으로, 이 프로세스를 자동화하기 위해 Python의 Schedule 라이브러리를 사용하여 정기적으로 스크립트를 실행하기로 결정했어요.

## 도전 극복과 세부 조정

개발 과정 중에는 몇 가지 세부 조정 단계가 필요했어요. ChatGPT이 요청을 이해하고 필수 기능을 제공했지만, 처음에는 몇 가지 오류가 있었어요. 예를 들어, 초기에 제공된 코드는 최신 OpenAI Python 라이브러리 버전과 일치하지 않았죠. 그러나 OpenAI의 제안을 따라 openai migrate 명령을 사용하여 이 문제를 해결했어요.

<div class="content-ad"></div>

또한, ChatGPT는 처음에 Leonardo.ai의 사용 방법을 잘못 이해했지만, Leonardo.ai의 API 문서와 레시피를 참조하여 적절한 코드를 생성할 수 있도록 안내했습니다. ChatGPT와 협력하여이 작업을 완료하는 데 소요되는 시간을 크게 단축 시킬 수 있었는데, 예상 시간으로는 6시간이 걸릴 것을 약 2시간으로 단축했습니다.

# 배경 음악 실험

테스트 단계에서 Pixabay API가 배경 음악 다운로드를 지원하지 않는 것을 발견했습니다. 그 후 ChatGPT에게 직접 다운로드하기 위해 Selenium을 사용하도록 요청했지만, 이 방법은 ChromeDriver와 헤드리스 브라우저가 필요하여 번거로웠습니다. 따라서 YouTube에서 Pixabay 콘텐츠에 대한 잠재적인 저작권 문제를 고려하여 최상위 40개의 동기부여 음악을 수동으로 다운로드했습니다.

# 상세한 설명

<div class="content-ad"></div>

# 1. 주제 생성하기

첫 번째 단계는 YouTube 비디오를 위해 매력적인 주제를 생성하는 것입니다. OpenAI의 ChatGPT를 사용하여 개인 성장과 동기부여를 원하는 관객을 대상으로 매력적이고 영감을 주는 주제를 만듭니다. 이를 통해 우리 콘텐츠가 항상 신선하고 시청자들에게 매력적인 것을 보장합니다.

```js
def generate_topic():
    prompt = "자기 동기 부여용 YouTube 채널에 대한 매력적인 주제를 추천해 주세요. 주제는 매력적이고 영감을 주며, 자기 성장과 동기 부여를 원하는 관객에게 관련이 있어야 합니다."

    response = client.chat.completions.create(
        model="gpt-3.5-turbo",
        messages=[
            {"role": "system", "content": "도움이 되는 보조 툴입니다."},
            {"role": "user", "content": prompt},
        ],
    )

    topic = response.choices[0].message.content.strip()
    return topic
```

# 2. YouTube 대본 생성하기

<div class="content-ad"></div>

한 번 주제를 정하면 YouTube 스크립트를 자세히 작성해요. 이 스크립트에는 매력적인 소개, 주요 요점, 개인 이야기, 그리고 동기 부여적인 결론이 포함돼요. 스크립트에 불필요한 문자가 없어야 하며, 이는 텍스트 음성 출력의 품질을 향상시킵니다. 불필요한 문자를 제거하면 "주최자:" 와 같은 어색한 TTS 발음을 피할 수 있어요.

```js
def generate_youtube_script(topic):
    prompt = (
        f"'{topic}' 주제의 자기계발 채널을 위한 YouTube 스크립트를 작성해주세요. "
        "스크립트에는 매력적인 소개, 주요 요점이 포함된 본문 섹션, "
        "개인 이야기나 예시, 그리고 동기 부여적인 메시지가 담긴 결론이 포함돼야 해요. "
        "스크립트의 길이는 일반적인 발화로 10~15분 정도여야 해요. "
        "소주제는 적어도 10개 이상이어야 해요. "
        "이 텍스트 안에 있는 [INTRO]와 번호 매기기 같은 괄호안 텍스트를 지우고, 'Host:'도 삭제해주세요. "
        "스크립트를 더 매력적으로 다듬고 채널 이름 'Motivation Marvel'을 소개해주세요. "
        "스크립트 끝에 사용자들에게 채널과 이 영상 구독 및 좋아요 클릭을 요청해주세요."
    )

    response = client.chat.completions.create(
        model="gpt-3.5-turbo",
        messages=[
            {"role": "system", "content": "도움이 되는 조수입니다."},
            {"role": "user", "content": prompt},
        ],
    )

    script = response.choices[0].message.content.strip()
    # 괄호 안 텍스트 제거
    script = re.sub(r"\[.*?\]", "", script)
    # 번호 제거
    script = re.sub(r"^\d+\.\s*", "", script, flags=re.MULTILINE)
    # 'Host:' 제거
    script = re.sub(r"\bHost:\s*", "", script)
    # 알파벳, 숫자, 공백, 지정된 구두점을 제외한 모든 문자 제거
    script = re.sub(r"[^a-zA-Z0-9\s,?.!]", "", script)
    return script
```

# 3. 레오나르도 프롬프트 생성

시각적 콘텐츠에는 Leonardo.ai의 API를 활용해요. 이 단계에서는 고품질 이미지를 생성하고 확대해 동적인 영상을 만들어요. 이 프로세스는 정적 이미지를 동적 시각 콘텐츠로 변환하여 비디오를 더 매력적으로 만들어 줘요. Leonardo.ai의 API 레시피 문서를 참고하여 이미지 생성, 확대, 그리고 모션 비디오를 만드는 과정을 이해해요.

<div class="content-ad"></div>

```js
def generate_leonardo_prompts(youtube_script):
    prompt = (
        f"다음 YouTube 스크립트를 기반으로 Leonardo.ai를 위한 10가지 텍스트 프롬프트를 생성해보세요. "
        f"프롬프트는 성공한 부자들의 동기부여를 다루어야 합니다. 각 프롬프트는 한 명의 사람을 묘사하고 사진에 얼굴이 나오지 않아야 합니다. "
        f"사진에 텍스트는 들어가지 않아야 합니다. 또한, 사진 속 얼굴이 화면 앞을 보고 있으면 안 됩니다. "
        f"프롬프트에 번호를 넣으면 안 되며, 비어있는 프롬프트는 제외해주세요."
        f"다음은 YouTube 스크립트입니다:\n\n{youtube_script}"
    )

    response = client.chat.completions.create(
        model="gpt-3.5-turbo",
        messages=[
            {"role": "system", "content": "도움이 필요한 어시스턴트입니다."},
            {"role": "user", "content": prompt},
        ],
    )

    prompts = response.choices[0].message.content.strip().split("\n")
    prompts = [
        prompt for prompt in prompts if prompt.strip()
    ]  # 비어있는 프롬프트 제거

    # 각 프롬프트에 추가 요구 사항 추가
    prompts = [
        f"{prompt}. 추가로, 생성된 이미지에 텍스트를 포함하지 마시고, 사진 속 인물의 얼굴은 전면을 보면 안 됩니다."
        for prompt in prompts
    ]

    return prompts
```

## 4. 텍스트 음성 변환

그런 다음, 스크립트는 Google Cloud의 Text-to-Speech API를 사용하여 음성으로 변환됩니다. 우리는 음성을 다큐멘터리 스타일 톤으로 설정하여 전문적이고 매력적으로 들리도록 합니다. Google Cloud는 이 서비스에 대한 무료 할당량을 제공하여 비용 효율적입니다. 이 서비스를 사용하려면 Google Cloud 계정을 생성하고 Text-to-Speech API를 활성화해야 합니다. 이 서비스에는 무료 크레딧 $300이 제공됩니다.

```js
def convert_text_to_speech(text, filename):
    # Google Cloud Text-to-Speech 클라이언트 초기화
    client = texttospeech.TextToSpeechClient.from_service_account_json(
        "path/to/your/service_account.json"
    )

    # 합성할 텍스트 입력 설정
    synthesis_input = texttospeech.SynthesisInput(text=text)

    # 음성 요청 빌드
    voice = texttospeech.VoiceSelectionParams(
        language_code="en-US",
        name="en-US-Wavenet-B",  # WaveNet 남성 음성 예시
        ssml_gender=texttospeech.SsmlVoiceGender.MALE,
    )

    # 오디오 구성 설정
    audio_config = texttospeech.AudioConfig(
        audio_encoding=texttospeech.AudioEncoding.MP3,
        pitch=-2.0,  # 진지한 톤을 위해 약간 더 낮은 음높이
        speaking_rate=0.9,  # 차분함을 위해 약간 느린 천천히 말하는 속도
        volume_gain_db=0.0,  # 기본 볼륨
    )

    # 텍스트 음성 변환 요청
    response = client.synthesize_speech(
        input=synthesis_input, voice=voice, audio_config=audio_config
    )

    # 응답을 오디오 파일로 저장
    with open("temp.mp3", "wb") as out:
        out.write(response.audio_content)
        print(f'오디오 콘텐츠가 "temp.mp3" 파일에 작성되었습니다.')

    # 생성된 음성 불러오기
    audio = AudioSegment.from_file("temp.mp3", format="mp3")

    # 침묵을 기준으로 분할하고 제거
    chunks = split_on_silence(
        audio, silence_thresh=-50, min_silence_len=500, keep_silence=250
    )

    # 최종 오디오를 형성할 조각들 연결
    trimmed_audio = AudioSegment.empty()
    for chunk in chunks:
        trimmed_audio += chunk

    # 침묵 없는 최종 오디오 내보내기
    trimmed_audio.export(filename, format="mp3")
    print(f"{filename}에 TTS 오디오를 저장했습니다.")

    # 임시 파일 제거
    os.remove("temp.mp3")
```

<div class="content-ad"></div>

# 5. 음악 검색 및 다운로드

접근성을 향상시키고 시청자 참여를 증진시키기 위해 비디오에 자막을 생성합니다. OpenAI의 Whisper API를 사용하여 오디오를 전사하고 SRT 파일을 생성합니다. 이 파일은 그 후 비디오 위에 자막을 오버레이하는 데 사용됩니다.

# 6. 음악 및 보이스 오버가 포함된 비디오 연결

처음에는 동기부여를 주는 음악을 검색하기 위해 Pixabay의 API를 사용해보았습니다. 그러나 음악 검색을 위한 API를 제공하지 않아 로열티 프리 음악 트랙을 로컬 저장소에 다운로드했습니다. 이러한 트랙은 비디오에 추가되어 그 감정적인 영향을 향상시킵니다. 또는 YouTube 스튜디오에서 로열티 프리 음악을 다운로드할 수도 있습니다.

<div class="content-ad"></div>

```js
def concatenate_video_with_music_and_tts(
    video_path,
    music_folder,
    tts_path,
    srt_path,
    output_path,
    target_resolution=(720, 1280),
):
    video_clip = VideoFileClip(video_path)
    tts_clip = AudioFileClip(tts_path)

    # 비디오 클립을 대상 해상도로 조정합니다.
    video_clip = video_clip.resize(width=1920)

    # 지정된 폴더에서 음악 파일을 로드합니다.
    # 지정된 폴더에서 무작위로 음악 파일을 로드합니다.
    music_files = [
        os.path.join(music_folder, file)
        for file in os.listdir(music_folder)
        if file.endswith(".mp3")
    ]
    num_music_files = len(music_files)
    num_clips_to_select = min(
        num_music_files, 40
    )  # 필요에 따라 선택할 음악 클립 수를 조정합니다.
    music_paths = random.sample(music_files, num_clips_to_select)
    music_clips = [
        AudioFileClip(music_path).fx(afx.volumex, 0.1) for music_path in music_paths
    ]  # 볼륨을 50%로 줄입니다.

    # TTS 클립의 길이에 맞게 비디오를 반복합니다.
    video_clip = vfx.loop(video_clip, duration=tts_clip.duration)

    # 음악 클립을 순차적으로 반복해 TTS 클립의 길이에 맞춥니다.
    looped_music_clip = loop_audio_clips_sequentially(music_clips, tts_clip.duration)

    # TTS 및 음악을 동시에 시작하는 합성 오디오 클립을 생성합니다.
    composite_audio = CompositeAudioClip([tts_clip, looped_music_clip]).set_duration(
        tts_clip.duration
    )

    # SRT 파일에서 자막 클립을 생성합니다.
    subtitles = parse_srt(srt_path)
    font_path = "NotoSans-Bold.ttf"
    subtitle_clips = [
        TextClip(
            text,
            fontsize=60,
            color="white",
            font=font_path,
            stroke_color="black",
            stroke_width=1,
            method="caption",
            size=video_clip.size,
            align="center",
            # bg_color="rgba(0, 0, 0, 0.5)",  # 반투명 배경
        )
        .set_position(("center", "center"))
        .set_start(start)
        .set_end(end)
        for (start, end, text) in subtitles
    ]

    # 비디오 클립에 자막을 오버레이합니다.
    video_with_subtitles = CompositeVideoClip([video_clip] + subtitle_clips)

    # 합성 오디오를 자막이 있는 비디오 클립에 설정합니다.
    final_video = video_with_subtitles.set_audio(composite_audio)

    # 페이드 인 및 페이드 아웃 효과 추가
    final_video = final_video.fadein(2).fadeout(2)

    # 최종 비디오를 파일로 저장합니다.
    final_video.write_videofile(
        output_path, codec="libx264", audio_codec="aac", fps=30, preset="ultrafast"
    )
    print(f"{output_path}에 최종 비디오 저장됨")
```

# 7. YouTube 섬네일 생성

시청자를 유혹하기 위해 맞춤형 섬네일은 필수입니다. 스크립트는 비디오에서 프레임을 캡처하고 텍스트를 오버레이하여 시각적으로 매력적인 섬네일을 만들어냅니다. 이 단계는 YouTube 검색 결과와 추천에서 비디오를 빛나게 만들어 주어 중요합니다. create_youtube_thumbnail 함수는 비디오에서 프레임을 캡처하고 1280x720로 크기를 조정한 후 중앙에 전문적인 글꼴과 스타일로 텍스트를 오버레이하여 고품질 섬네일을 생성합니다.

```js
def create_youtube_thumbnail(video_path, text, output_path):
    # 'Title: ' 제거
    text = re.sub(r"\bTitle:\s*", "", text)
    # 비디오 클립 로드
    video_clip = VideoFileClip(video_path)

    # 비디오 중간의 프레임 가져오기
    frame = video_clip.get_frame(video_clip.duration / 8)

    # 프레임을 이미지로 변환
    image = Image.fromarray(frame)

    # 이미지에 텍스트 그리기
    draw = ImageDraw.Draw(image)
    font_path = "NotoSans-Bold.ttf"  # 글꼴 파일 경로를 제공하세요

    # 글꼴 파일이 있는지 확인
    if not os.path.exists(font_path):
        raise FileNotFoundError(f"글꼴 파일을 찾을 수 없음: {font_path}")

    font = ImageFont.truetype(font_path, 50)

    # 텍스트 감싸는 함수
    def draw_text(draw, text, font, max_width):
        lines = []
        words = text.split(" ")
        line = []
        for word in words:
            test_line = " ".join(line + [word])
            bbox = draw.textbbox((0, 0), test_line, font=font)
            width = bbox[2] - bbox[0]
            if width <= max_width:
                line.append(word)
            else:
                lines.append(" ".join(line))
                line = [word]
        lines.append(" ".join(line))
        return lines

    max_width = image.width - 100  # 텍스트의 최대 너비
    lines = draw_text(draw, text, font, max_width)

    # 텍스트 블록의 총 높이 계산
    total_height = (
        sum(
            draw.textbbox((0, 0), line, font=font)[3]
            - draw.textbbox((0, 0), line, font=font)[1]
            for line in lines
        )
        + (len(lines) - 1) * 10
    )
    y_text = (image.height - total_height) // 2  # 중앙부터 그리기 시작

    for line in lines:
        bbox = draw.textbbox((0, 0), line, font=font)
        width = bbox[2] - bbox[0]
        height = bbox[3] - bbox[1]
        draw.text(
            ((image.width - width) / 2, y_text),
            line,
            font=font,
            fill="white",
            stroke_width=1,
            stroke_fill="black",
        )
        y_text += height + 10

    # 이미지를 전체 대상 크기로 크기 조정 및 자르기
    target_width, target_height = 1280, 720
    original_aspect = image.width / image.height
    target_aspect = target_width / target_height

    if original_aspect > target_aspect:
        # 너비 자르기
        new_height = target_height
        new_width = int(target_height * original_aspect)
        image = image.resize((new_width, new_height), Image.Resampling.LANCZOS)
        crop_x = (new_width - target_width) // 2
        image = image.crop((crop_x, 0, crop_x + target_width, new_height))
    else:
        # 높이 자르기
        new_width = target_width
        new_height = int(target_width / original_aspect)
        image = image.resize((new_width, new_height), Image.Resampling.LANCZOS)
        crop_y = (new_height - target_height) // 2
        image = image.crop((0, crop_y, new_width, crop_y + target_height))

    # 섬네일로 이미지 저장
    image.save(output_path)
    print(f"{output_path}에 YouTube 섬네일 저장됨")
```

<div class="content-ad"></div>

# 8. 콘텐츠 저장 및 정리

최종 단계에서는 비디오를 YouTube에 업로드합니다. YouTube Data API를 사용하여 자동 업로드를 위한 추가적인 확인이 필요하지만, 필요한 경우 비디오를 수동으로 업로드하는 구조화된 접근 방식을 제공합니다. 이 함수는 YouTube Data API를 사용하여 비디오를 업로드하고 제목 및 설명을 설정하며 사용자 지정 섬네일을 추가합니다.

```js
def save_to_folder(
    topic,
    youtube_script,
    title,
    description,
    tts_filename,
    video_filename,
    srt_filename,
    thumbnail_filename,
):
    # YYYYMMDD_HHMMSS 형식으로 현재 날짜와 시간 생성
    current_time = datetime.now().strftime("%Y%m%d_%H%M%S")
    # 폴더 생성
    folder_name = f"{current_time}"
    os.makedirs(folder_name, exist_ok=True)

    # 텍스트 콘텐츠를 파일로 저장
    text_filename = os.path.join(folder_name, f"{current_time}_content.txt")
    with open(text_filename, "w") as file:
        file.write(f"주제: {topic}\n\n")
        file.write(f"YouTube 스크립트:\n{youtube_script}\n\n")
        file.write(f"제목: {title}\n\n")
        file.write(f"설명:\n{description}\n\n")

    # 생성된 파일을 폴더로 이동
    os.rename(tts_filename, os.path.join(folder_name, tts_filename))
    os.rename(video_filename, os.path.join(folder_name, video_filename))
    os.rename(srt_filename, os.path.join(folder_name, srt_filename))
    os.rename(thumbnail_filename, os.path.join(folder_name, thumbnail_filename))

    print(f"모든 파일을 폴더 {folder_name}에 저장했습니다")
```

# 9. YouTube에 비디오 업로드

<div class="content-ad"></div>

최종 단계는 YouTube에 비디오를 업로드하는 것입니다. YouTube Data API를 사용하여 자동 업로드에는 추가적인 확인이 필요하지만, 필요한 경우 비디오를 수동으로 업로드하는 구조적인 접근 방법을 제공하는 스크립트가 있습니다. 이 함수는 YouTube Data API를 사용하여 비디오를 업로드하고 제목과 설명을 설정하며 사용자 정의 섬네일을 추가합니다.

```js
# YouTube API 자격 증명 설정
CLIENT_SECRETS_FILE = "path/to/your/client_secret.json"
SCOPES = ["https://www.googleapis.com/auth/youtube.upload"]

def get_authenticated_service():
    credentials = None
    if os.path.exists("token.pickle"):
        with open("token.pickle", "rb") as token:
            credentials = pickle.load(token)
    if not credentials or not credentials.valid:
        if credentials and credentials.expired and credentials.refresh_token:
            credentials.refresh(Request())
        else:
            flow = InstalledAppFlow.from_client_secrets_file(CLIENT_SECRETS_FILE, SCOPES)
            credentials = flow.run_local_server(port=0)
        with open("token.pickle", "wb") as token:
            pickle.dump(credentials, token)
    return build("youtube", "v3", credentials=credentials)

def upload_video_to_youtube(youtube, video_file, title, description, thumbnail_file):
    body = {
        "snippet": {
            "title": title,
            "description": description,
            "tags": ["motivation", "self-improvement", "success"],
            "categoryId": "22"  # 카테고리: 사람들 & 블로그
        },
        "status": {
            "privacyStatus": "public",
            "selfDeclaredMadeForKids": False
        }
    }

    # API의 videos.insert 메서드를 호출하여 비디오를 만들고 업로드합니다.
    media = MediaFileUpload(video_file, chunksize=-1, resumable=True)
    request = youtube.videos().insert(
        part="snippet,status",
        body=body,
        media_body=media
    )

    response = request.execute()
    print("아이디가 있는 비디오 업로드: " + response["id"])

    # 업로드된 비디오에 썸네일 설정
    youtube.thumbnails().set(
        videoId=response["id"],
        media_body=thumbnail_file
    ).execute()
```

# 10. 일정화로 자동화하기

전체 프로세스를 자동화하기 위해 schedule Python 라이브러리를 사용합니다. 이를 통해 스크립트가 하루에 여러 차례 실행되어 콘텐츠가 꾸준히 생산되고 업로드할 준비가 되도록 할 수 있습니다.

<div class="content-ad"></div>

```python
import schedule
import time
import os

def job():
    print("일정이 실행 중입니다...")
    os.system("python3 /경로/스크립트.py")

print("일정이 시작됩니다...")
schedule.every(10).minutes.do(job) # 일정을 매 10분마다 실행

while True:
    schedule.run_pending()
    time.sleep(1)
```

# 챗지피티(CheatGPT)와 코딩에 대한 생각

<img src="/assets/img/2024-09-10-HowICreated144FullyAutomatedYouTubeVideosPerDayUsingChatGPTLeonardoaiFullSourceCode_2.png" />

이 프로젝트는 챗지피티와 레오나르도.ai의 인상적인 기능을 콘텐츠 생성 워크플로우 자동화에 성공적으로 보여주었습니다. 그러나 유의해야 할 점은 YouTube가 AI로 생성된 비디오에 대한 수익화를 허용하지 않는다는 것입니다. 이 과정은 기술적으로 실행 가능하지만 상당한 API 비용이 발생할 수 있습니다. 또한, Canva와 DubDup 같은 도구들도 투자를 요구합니다. 자동화된 YouTube 동영상을 부업으로 광고하는 많은 동영상들은 제휴 마케팅을 동기로 하고 있을 수 있습니다. 따라서, 더 비싼 도구로 투자하기 전에 챗지피티를 사용하여 비용이 적거나 전혀 들지 않는 자동화 아이디어를 검증하는 것이 좋습니다.

<div class="content-ad"></div>

# 결론

YouTube 비디오 생성을 AI 도구인 ChatGPT와 Leonardo.ai를 사용하여 자동화하는 프로세스는 매력적이고 기술적으로 인상적이지만, 보다 광범위한 영향을 고려하는 것이 중요합니다. YouTube의 수익화 정책은 AI로 생성된 콘텐츠를 선호하지 않으며, 이러한 비디오를 생성하는 데는 API 비용과 시간적 리소스가 많이 필요할 수 있습니다.

게다가, 자동 콘텐츠의 매력은 종종 제휴 마케팅 전략에서 비롯되어 사용자들이 도구와 서비스를 구매하도록 유도합니다. 그 대신 무료 체험판과 저렴한 솔루션을 활용하여 자동화 아이디어를 탐색하는 것을 권장합니다. 궁극적으로 목표는 수익화 기회를 따라가는 것이 아닌, 철저히 관객들과 유익하게 관계를 형성하며, 낮은 노력과 높은 양의 비디오 생산에 기여하는 것이 아닌 가치 있는 콘텐츠를 만드는 것이어야 합니다.

비디오 생성 프로세스를 성공적으로 자동화하였더라도, 후속적으로 이를 더 이상 추구하지 않기로 결정했습니다. 낭비되는 콘텐츠를 생성하는 것을 피하고 시청자들의 소중한 시간을 존중하기 위해서입니다. 이러한 강력한 도구를 어떻게 활용하여 수익화 기회를 추구하는 것이 아닌 의미 있는 기여를 할 수 있는지 신중히 고려해보세요.

<div class="content-ad"></div>

전체 소스 코드: youtube_video_auto_creator.py scheduler.py