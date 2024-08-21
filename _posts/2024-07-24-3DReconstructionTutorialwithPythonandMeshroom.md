---
title: "Python과 Meshroom을 사용한 3D 재구성 튜토리얼"
description: ""
coverImage: "/assets/img/2024-07-24-3DReconstructionTutorialwithPythonandMeshroom_0.png"
date: 2024-07-24 11:50
ogImage: 
  url: /assets/img/2024-07-24-3DReconstructionTutorialwithPythonandMeshroom_0.png
tag: Tech
originalTitle: "3D Reconstruction Tutorial with Python and Meshroom"
link: "https://medium.com/towards-data-science/3d-reconstruction-tutorial-with-python-and-meshroom-2aa37805ab4a"
isUpdated: true
---




## 3D 재구성

과거에 제가 쓴 기사에서 이미지 기반 3D 재구성에 대해 들어보신 적이 있을 수 있습니다. 이것은 나중에 사용할 수 있도록 깊이 우선 예제입니다:

오늘은 여러분이 사진을 찍어 3D 포인트 클라우드 또는 3D 메시를 생성할 수 있는 파이썬 스크립트를 개발하는 쿨한 경험을 만들 수 있도록 "간결한" 내용에 집중하고 싶습니다.

그런 내용이 마음에 든다면, 아래의 질문에 빠르게 답변해 보겠습니다:

<div class="content-ad"></div>

![img](/assets/img/2024-07-24-3DReconstructionTutorialwithPythonandMeshroom_0.png)

언제든지 준비되면, 재미있는 부분으로 넘어갈까요?

📦 플로랑의 자료들: 이 튜토리얼에 필요한 자료를 여기서 찾을 수 있습니다—먼저, 문서와 함께 Meshroom 소프트웨어가 있습니다. 그리고, 일부 라이브러리를 사용하기 위해 Python 3.9+를 갖춘 아나콘다 배포가 있습니다.

튜토리얼을 7단계로 나누어 설명하겠습니다:

<div class="content-ad"></div>


![Image](/assets/img/2024-07-24-3DReconstructionTutorialwithPythonandMeshroom_1.png)

Time to move onto Stage 1: a quick (and dirty) introduction to photogrammetry.

# 1. Photogrammetry: introduction

So, photogrammetry is a powerful technique that allows the creation of 3D models from a series of 2D images.


<div class="content-ad"></div>

오늘은 Python과 Meshroom을 사용하여 3D 포인트 클라우드를 생성하는 방법을 안내할 것입니다. 이 과정은 이미지를 촬영하고 Meshroom에서 처리하여 3D 모델을 생성한 다음 Python을 사용하여 결과 포인트 클라우드를 조작하고 시각화하는 것을 포함합니다. 아래 이미지에 설명되어 있습니다.

![3D Reconstruction Tutorial with Python and Meshroom](/assets/img/2024-07-24-3DReconstructionTutorialwithPythonandMeshroom_2.png)

우리가 높은 수준에서 무엇을 할 것인지 이해했다면, 이제 2단계로 넘어갑시다.

# 2. 준비물

<div class="content-ad"></div>

시작하기 전에 다음 사항을 확인하세요:

![image](/assets/img/2024-07-24-3DReconstructionTutorialwithPythonandMeshroom_3.png)

준비물:

- 카메라 (스마트폰 카메라로도 충분합니다)
- Meshroom이 설치된 컴퓨터
- 컴퓨터에 Python이 설치되어 있어야 합니다
- 라이브러리: open3d, numpy, matplotlib

<div class="content-ad"></div>

파이썬 라이브러리를 설치하려면 pip을 사용할 수 있어요:

```js
pip install open3d numpy matplotlib
```

🦚 플로랑의 메모: 이 부분이 낯설다면, 3D 데이터 처리를 위한 최소한의 파이썬 설정에 대한 이 튜토리얼을 참고할 수 있어요:

좋아요, 모든 것이 원활해졌어요! 이제 다음 부분들이 준비되어 있어요:

<div class="content-ad"></div>


![image](/assets/img/2024-07-24-3DReconstructionTutorialwithPythonandMeshroom_4.png)

Let's take some pictures.

## 3. Capturing Images

The quality of your 3D model largely depends on the quality and quantity of the images you take. Follow these guidelines for the best results:


<div class="content-ad"></div>


![image](/assets/img/2024-07-24-3DReconstructionTutorialwithPythonandMeshroom_5.png)

If I have to describe a bit what that means:

- Capture images with good lighting and minimal shadows.
- Ensure the object is in focus in all pictures.
- Take photos from multiple angles to cover all sides of the object.
- Overlap between consecutive images should be around 60–80%.

With these four simple directions, you should be good to go and ready to move on to stage 4.


<div class="content-ad"></div>

# 4. Meshroom을 사용하여 이미지 처리하기

좋아요, 이제 컴퓨터의 로컬 드라이브 폴더에 업로드한 이미지들이 많아요. 이제 Meshroom을 사용해봅시다. 아래 그림과 같이:

![Meshroom Image](/assets/img/2024-07-24-3DReconstructionTutorialwithPythonandMeshroom_6.png)

자세히 해야 할 일은 다음과 같아요:

<div class="content-ad"></div>

- Meshroom 열기: 컴퓨터에서 Meshroom을 실행하세요.
- 새 프로젝트 생성: 새 프로젝트를 시작하고 이미지를 Meshroom 작업 공간으로 끌어다 놓아 가져옵니다.
- 파이프라인 실행: "시작" 버튼을 클릭하여 기본 사진 측량 파이프라인을 실행하세요. Meshroom은 이미지를 처리하고 3D 모델을 생성할 것입니다.
- 모델 검토: 처리가 완료되면 3D 모델을 검토하여 예상에 부합하는지 확인하세요.

이러한 단계를 완료한 후, 3D 데이터 세트를 내보내는 단계로 넘어갈 수 있습니다.

# 5. 포인트 클라우드 내보내기

Meshroom에서 3D 모델을 생성한 후, 포인트 클라우드를 내보내봅시다. 아래 나의 설명을 살펴보시면, 우리가 하는 일입니다:

<div class="content-ad"></div>

<img src="/assets/img/2024-07-24-3DReconstructionTutorialwithPythonandMeshroom_7.png" />

간단히 말해서:

- 출력 선택: Meshroom 인터페이스에서 “Texturing” 노드로 이동합니다.
- 내보내기: “Texturing” 노드에서 마우스 오른쪽 버튼을 클릭하고 “내보내기”를 선택합니다. .ply 또는 .obj와 같은 원하는 형식을 선택합니다.

좋아요, 이제 다음 단계인 여섯으로 넘어갈게요.

<div class="content-ad"></div>

🦚 플로랑의 노트: 현재 단계에서 무언가가 제대로 작동하지 않는다고 느끼신다면, 빠른 복제를 위해 여기에서는 놓친 다양한 심층적인 세부 정보를 위해 앞서 언급된 튜토리얼을 확인하는 것을 권장합니다.

# 6. 파이썬으로 포인트 클라우드 시각화하기

이제 포인트 클라우드 파일을 갖고 있다면, Python을 사용하여 시각화할 수 있습니다. 아래는 open3d를 사용한 샘플 스크립트입니다:

```python
import open3d as o3d
```

<div class="content-ad"></div>

그리고 포인트 클라우드를 시각화하기 위해 다음의 코드를 사용하세요:

```js
pcd = o3d.io.read_point_cloud("path_to_your_point_cloud.ply")
o3d.visualization.draw_geometries([pcd])
```

저는 위 코드로 35개의 사진을 생성했습니다. 

<img src="https://miro.medium.com/v2/resize:fit:1200/1*xvWCgFf_fUyX0bdgu5skGw.gif" />

<div class="content-ad"></div>

괜찮지 않아? 😉

# 7. 결론

Python과 Meshroom을 사용하여 3D 포인트 클라우드를 생성하는 것은 사진측량의 힘을 활용한 간단한 과정입니다. 이 문서에 나열된 단계를 따라가면 간단한 2D 이미지에서 자세한 3D 모델을 만들고 Python을 사용하여 시각화할 수 있습니다.

![3D Reconstruction Tutorial with Python and Meshroom](/assets/img/2024-07-24-3DReconstructionTutorialwithPythonandMeshroom_8.png)

<div class="content-ad"></div>

이 기술은 가상 현실부터 3D 프린팅까지 다양한 분야에 응용 가능성을 열어 줍니다.

이제 이 기술을 어떻게 활용할지 기대됩니다! 3D 기술 무기함에 이 기술을 추가해 보세요 🏹.