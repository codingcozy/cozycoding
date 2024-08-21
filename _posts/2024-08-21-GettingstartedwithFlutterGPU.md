---
title: "Flutter GPU를 활용한 초보자 가이드"
description: ""
coverImage: "/assets/img/2024-08-21-GettingstartedwithFlutterGPU_0.png"
date: 2024-08-21 18:37
ogImage: 
  url: /assets/img/2024-08-21-GettingstartedwithFlutterGPU_0.png
tag: Tech
originalTitle: "Getting started with Flutter GPU"
link: "https://medium.com/flutter/getting-started-with-flutter-gpu-f33d497b7c11"
isUpdated: false
---


## 플러터에서 사용자 정의 렌더러를 작성하고 3D 씬을 렌더링하세요.

플러터 3.24 버전에서는 Flutter GPU라는 새로운 저수준 그래픽 API가 소개되었습니다. Flutter GPU로 지원되는 3D 렌더링 라이브러리인 Flutter Scene (패키지: flutter_scene)도 있습니다. Flutter GPU와 Flutter Scene은 현재 미리보기 상태이며 실험적 기능에 의존하기 때문에 플러터의 본 채널에서만 사용할 수 있습니다. 이를 사용하려면 Impeller를 활성화해야 하며 가끔 중단을 유발할 수도 있습니다.

이 기사에는 이러한 패키지에 대한 두 가지 "시작하기" 가이드가 포함되어 있습니다:

- 🔺 고급: Flutter GPU로 시작하기
만일 경험 많은 그래픽 프로그래머이거나 플러터에서 처음부터 렌더러를 작성하고 싶은 경우, 이 가이드를 통해 Flutter GPU를 활용해 작업할 수 있습니다. 여기에서는 플러터에서 처음으로 삼각형을 그려볼 수 있습니다!
- 💚 중급: Flutter Scene으로 3D 렌더링하기
만일 플러터 개발자이고 앱에 3D 기능을 추가하거나 Dart와 Flutter를 사용하여 3D 게임을 만들고 싶다면 이 가이드가 도움이 될 것입니다! 여기에서는 플러터에서 3D 자산을 가져와 렌더링하는 프로젝트를 설정할 수 있습니다.

<div class="content-ad"></div>

# Flutter GPU를 시작하자!

⚠️ 주의! ⚠️ Flutter GPU는 궁극적으로 낮은 수준의 API입니다. Flutter GPU의 존재로 혜택을 받을 가능성이 매우 높은 대부분의 Flutter 개발자들은 Flutter Scene 렌더링 패키지와 같이 pub.dev에 게시된 높은 수준의 렌더링 라이브러리를 사용함으로써 혜택을 받을 것입니다. 만약 Flutter GPU API 자체에 관심이 없고 3D 렌더링에만 관심이 있다면, Flutter Scene과 함께하는 3D 렌더링으로 건너뛰어주세요.

<img src="/assets/img/2024-08-21-GettingstartedwithFlutterGPU_0.png" />

# Flutter GPU를 시작하자!

<div class="content-ad"></div>

플러터 GPU는 플러터의 내장 저수준 그래픽 API입니다. 다트 코드와 GLSL 셰이더를 작성하여 플러터에서 사용자 정의 렌더러를 빌드하고 통합할 수 있습니다. 네이티브 플랫폼 코드는 필요하지 않습니다.

현재, 플러터 GPU는 초기 미리 보기 단계에 있으며 기본 래스터화 API를 제공하지만 안정화될 때까지 추가 기능이 계속해서 추가되고 개선될 것입니다.

플러터 GPU를 사용하려면 Impeller가 활성화되어 있어야 합니다. 따라서 Impeller에서 지원되는 플랫폼을 타겟팅할 때만 사용할 수 있습니다. 현재 Impeller는 다음을 지원합니다:

- iOS (기본적으로 활성화)
- macOS (옵트인 미리 보기)
- Android (옵트인 미리 보기)

<div class="content-ad"></div>

Flutter GPU의 목표는 결국 Flutter의 모든 플랫폼 대상을 지원하는 것입니다. 궁극적인 목표는 패키지 저자들에게 유지 관리하기 쉽고 사용자들에게 쉽게 설치할 수 있는 Flutter의 크로스 플랫폼 렌더링 솔루션 생태계를 육성하는 것입니다.

3D 렌더링은 단지 가능한 사용 사례 중 하나일 뿐입니다. Flutter GPU는 특수한 2D 렌더러를 구축하거나, 4차원 공간의 3D 슬라이스를 렌더링하거나 비유클리드 공간을 투영하는 등 더 비정상적인 용도로도 사용될 수 있습니다.

Flutter GPU를 이용한 사용자 정의 2D 렌더러의 좋은 예시 중 하나는 스켈레톤 메시 변형을 활용하는 2D 캐릭터 애니메이션 형식입니다. Spine 2D가 그 예시 중 하나입니다. 이러한 스켈레톤 메시 솔루션은 일반적으로 각 뼈에 대한 번역, 회전 및 스케일 속성을 조작하는 애니메이션 클립을 보유하며, 각 정점은 해당 뼈를 얼마나 영향을 미쳐야 하는지 및 얼마나 영향을 미쳐야 하는지를 결정하는 몇 가지 관련 "뼈 가중치"를 갖습니다.

drawVertices와 같은 Canvas 솔루션을 사용할 경우, 각 정점에 대해 뼈 가중치 변환을 CPU에서 적용해야 합니다. 그러나 Flutter GPU를 사용하면 뼈 변형을 정점 쉐이더에 균일 배열 형태나 심지어 텍스처 샘플러 형태로 제공하여 각 정점의 최종 위치를 GPU에서 계산할 수 있게 되며, GPU는 뼈 구조와 정점당 뼈 가중치 상태에 기반하여 각 정점의 최종 위치를 병렬로 계산할 수 있습니다.

<div class="content-ad"></div>

수고하셨습니다! 이제 부드러운 소개를 통해 플러터 GPU로 첫 번째 삼각형을 그려보겠습니다!

![이미지](/assets/img/2024-08-21-GettingstartedwithFlutterGPU_1.png)

## 프로젝트에 Flutter GPU 추가하기

우선, Flutter GPU가 현재 초기 미리보기 상태에 있다는 점을 유의해주세요. API 변경이 발생할 수 있습니다. 현재 API로 이미 다양한 기능을 구현할 수 있지만, 경험 많은 그래픽 엔지니어들은 몇 가지 일반적인 기능이 미싱되어 있을 수 있습니다. 앞으로 몇 달 동안 Flutter GPU에 대한 많은 계획이 있습니다.

<div class="content-ad"></div>

이러한 이유로, 현재 Flutter GPU에 대한 패키지를 개발할 때 주요 채널 끝에 대해 작업하는 것이 강력히 권장됩니다. 예기치 못한 동작, 버그 또는 기능 요청이 발생하면 GitHub의 표준 Flutter 이슈 템플릿을 사용하여 문제를 제기해주세요. Flutter GPU와 관련된 모든 추적된 이슈는 flutter-gpu 레이블이 지정됩니다.

따라서 Flutter GPU를 실험하기 전에, 다음 명령을 실행하여 Flutter를 주요 채널로 전환해주세요.

```js
flutter channel main
flutter upgrade
```

그런 다음 새로운 Flutter 프로젝트를 생성해보세요.

<div class="content-ad"></div>

```js
flutter create my_cool_renderer
cd my_cool_renderer
```

다음으로, flutter_gpu SDK 패키지를 pubspec에 추가해주세요.

```js
flutter pub add flutter_gpu --sdk=flutter
```

# Shader 번들을 빌드하고 가져오기.

<div class="content-ad"></div>

플러터 GPU로 무언가를 렌더링하려면 GLSL 쉐이더를 작성해야 합니다. 플러터 GPU의 쉐이더는 플러터의 fragment shader 기능에서 사용되는 것과는 달리 균일 바인딩에 대해 다른 의미를 갖습니다. 또한 fragment shader와 함께 작동할 수 있는 vertex shader를 정의해야 합니다.

가능한 가장 간단한 쉐이더를 정의하는 것부터 시작하세요. 프로젝트의 어디에서든 쉐이더를 배치할 수 있지만, 이 예시에서는 shaders 디렉토리를 만들어 그 안에 두 가지 쉐이더를 넣어두세요: simple.vert와 simple.frag .

```js
// shaders/simple.vert 파일에 복사하세요

in vec2 position;

void main() {
  gl_Position = vec4(position, 0.0, 1.0);
}
```

삼각형을 그릴 때, 각 정점을 정의하는 데이터 목록이 있어야 합니다. 이 경우 2D 위치만 나열하고 있습니다. 각각의 정점에 대해 간단한 vertex shader는 이러한 2D 위치를 클립 스페이스 출력 intrinsic인 gl_Position에 할당합니다.

<div class="content-ad"></div>

```js
// shaders/simple.frag 파일로 복사

out vec4 frag_color;

void main() {
  frag_color = vec4(0, 1, 0, 1);
}
```

프래그먼트 쉐이더는 훨씬 더 간단합니다. (0, 0, 0, 0)부터 (1, 1, 1, 1)의 범위 내에서 RGBA 색상을 출력합니다. 그래서 모든 것이 녹색으로 셰이딩될 것입니다.

이제 쉐이더를 가지고 있으니, Flutter의 고정 시간(AOT) 쉐이더 컴파일러를 사용하여 컴파일하세요. 쉐이더 번들을 위한 자동화된 빌드를 설정하려면 flutter_gpu_shaders 패키지를 사용하는 것을 권장합니다.

프로젝트에서 flutter_gpu_shaders를 일반적인 종속성으로 추가하려면 pub을 사용하세요.

<div class="content-ad"></div>

```js
flutter pub add flutter_gpu_shaders
```

플러터 GPU 셰이더는 .shaderbundle 파일로 묶여 있습니다. 이 파일들은 프로젝트의 에셋 번들에 일반 에셋으로 추가할 수 있습니다. 셰이더 번들은 플랫폼 타겟을 위한 컴파일된 셰이더 소스를 포함합니다.

다음으로, 셰이더 번들 내용을 설명하는 셰이더 번들 매니페스트 파일을 생성하세요. 프로젝트의 루트 디렉토리에 있는 my_renderer.shaderbundle.json 파일에 다음 내용을 추가하세요.

```js
{
    "SimpleVertex": {
        "type": "vertex",
        "file": "shaders/simple.vert"
    },
    "SimpleFragment": {
        "type": "fragment",
        "file": "shaders/simple.frag"
    }
}
```

<div class="content-ad"></div>

셰이더 번들의 각 항목은 임의의 이름을 가질 수 있습니다. 이 경우에는 "SimpleVertex"와 "SimpleFragment"라는 이름을 사용했습니다. 이러한 이름은 앱에서 셰이더를 찾을 때 사용됩니다.

그런 다음, flutter_gpu_shaders 패키지를 사용하여 shaderbundle을 빌드하세요. 실험적인 "native assets" 기능을 활성화하여 자동으로 빌드를 트리거하는 후크를 추가할 수 있습니다. 다음 명령을 사용하여 native assets를 활성화하고 native_assets_cli 패키지를 설치하세요.

```js
flutter config --enable-native-assets
flutter pub add native_assets_cli
```

native assets 기능을 활성화한 후, 후크 디렉토리 아래에 빌드를 자동으로 트리거하는 build.dart 스크립트를 추가하세요.

<div class="content-ad"></div>

```js
// 다음 코드를 hook/build.dart 파일에 복사합니다

import 'package:native_assets_cli/native_assets_cli.dart';
import 'package:flutter_gpu_shaders/build.dart';

void main(List<String> args) async {
  await build(args, (config, output) async {
    await buildShaderBundleJson(
        buildConfig: config,
        buildOutput: output,
        manifestFileName: 'my_renderer.shaderbundle.json');
  });
}
```

위 변경 후, Flutter 도구가 프로젝트를 빌드할 때 buildShaderBundleJson이 셰이더 번들을 구성하고 결과를 패키지 루트 아래 build/shaderbundles/my_renderer.shaderbundle에 출력합니다.

셰이더 번들 형식 자체는 사용 중인 Flutter 버전과 밀접하게 관련되어 있으며 시간이 지남에 따라 변경될 수 있습니다. 셰이더 번들을 빌드하는 패키지를 제작 중이라면 생성된 .shaderbundle 파일을 소스 트리에 체크인하지 마십시오. 대신 빌드 프로세스를 자동화하기 위해 빌드 후크를 사용하십시오 (이전에 설명한대로).

이렇게 하면 라이브러리를 사용하는 개발자들이 항상 올바른 형식으로 최신의 셰이더 번들을 빌드할 수 있습니다!


<div class="content-ad"></div>

이제 shader 번들을 자동으로 빌드하고 있으니, 일반 에셋처럼 가져와보세요. 프로젝트의 pubspec.yaml에 에셋 항목을 추가해주세요:

```yaml
flutter:
  assets:
    - build/shaderbundles/
```

앞으로 네이티브 에셋 기능이 빌드 훅에 데이터 에셋을 번들에 추가할 수 있을 것입니다. 이 기능이 구현되면, 빌드 훅과 함께 에셋 가져오기 규칙을 추가할 필요가 없어질 것입니다.

그런 다음, 런타임에 셰이더들을 로딩하기 위한 코드를 추가해보세요. lib/shaders.dart를 생성하고 아래 코드를 추가해주세요.

<div class="content-ad"></div>

```js
// lib/shaders.dart에 복사하세요

import 'package:flutter_gpu/gpu.dart' as gpu;

const String _kShaderBundlePath =
    'build/shaderbundles/my_renderer.shaderbundle';
// 주의: 라이브러리를 빌드하는 경우, 경로는 패키지 이름으로 접두어를 붙여야 합니다. 예를 들어:
//      'packages/my_cool_renderer/build/shaderbundles/my_renderer.shaderbundle'

gpu.ShaderLibrary? _shaderLibrary;
gpu.ShaderLibrary get shaderLibrary {
  if (_shaderLibrary != null) {
    return _shaderLibrary!;
  }
  _shaderLibrary = gpu.ShaderLibrary.fromAsset(_kShaderBundlePath);
  if (_shaderLibrary != null) {
    return _shaderLibrary!;
  }

  throw Exception("Failed to load shader bundle! ($_kShaderBundlePath)");
}
```

이 코드는 Flutter GPU 셰이더 런타임 라이브러리에 대한 싱글턴 getter를 생성합니다. shaderLibrary에 처음 액세스할 때, gpu.ShaderLibrary.fromAsset(shader_bundle_path)를 사용하여 런타임 셰이더 라이브러리를 초기화합니다.

프로젝트는 이제 Flutter GPU 셰이더를 사용할 수 있도록 설정되었습니다. 이제 그 삼각형을 렌더링할 시간입니다!

# 처음으로 삼각형 그리기

<div class="content-ad"></div>

이 가이드에서는 RGBA Flutter GPU Texture 및 Texture를 색상 출력으로 첨부하는 RenderPass를 생성하고, Canvas.drawImage를 사용하여 위젯에서 Texture를 렌더링합니다.

간결함을 위해 매 프레임마다 모든 리소스를 다시 작성하는 것으로 충분합니다.

할당할 때 Texture를 "shader readable"로 표시하면, 이를 dart:ui.Image로 변환할 수 있습니다. 위젯 트리에서 렌더링된 결과를 표시하려면, 이를 dart:ui.Canvas에 그려야 합니다!

사용자 정의 페인터로 위젯 트리를 래핑하여 Canvas에 액세스할 수 있습니다. lib/main.dart의 내용을 다음과 같이 교체하십시오:

<div class="content-ad"></div>

```js
import 'dart:typed_data';

import 'package:flutter/material.dart';
import 'package:flutter_gpu/gpu.dart' as gpu;

// 참고: Shader 번들 import 설정 중에 이전에 만들었습니다!
import 'shaders.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter GPU Triangle Example',
      home: CustomPaint(
        painter: TrianglePainter(),
      ),
    );
  }
}

class TrianglePainter extends CustomPainter {
  @override
  void paint(Canvas canvas, Size size) {
    // `gpu.gpuContext`에 액세스 시도.
    // Flutter GPU가 지원되지 않을 경우 예외가 발생합니다.
    print('기본 색상 포맷: ' +
        gpu.gpuContext.defaultColorFormat.toString());
  }

  @override
  bool shouldRepaint(covariant CustomPainter oldDelegate) => true;
}
```

앱을 실행하세요. 알림으로, 현재 Flutter GPU는 Impeller가 활성화되어야 합니다. Impeller를 지원하는 플랫폼을 사용해야 합니다. 이 가이드에서는 macOS를 대상으로 할 것입니다.

```bash
flutter run -d macos --enable-impeller
```

![이미지](/assets/img/2024-08-21-GettingstartedwithFlutterGPU_2.png)


<div class="content-ad"></div>

만약 플러터 GPU가 작동 중이라면, 콘솔에 기본 색상 형식이 출력됩니다.

```js
flutter: Default color format: PixelFormat.b8g8r8a8UNormInt
```

임펠러가 활성화되지 않았다면, gpu.gpuContext에 액세스하려고 할 때 예외가 발생합니다.

```js
Exception: Flutter GPU requires the Impeller rendering backend to be enabled.

The relevant error-causing widget was:
  CustomPaint
```

<div class="content-ad"></div>

간편함을 위해, 여기서부터는 paint 메소드만 수정하겠습니다.

우선, Flutter GPU 텍스처를 생성하고, 초기화한 후에 Canvas로 그려서 표시하세요.

Canvas 크기와 같은 크기의 텍스처를 만드세요. StorageMode를 선택해야 합니다. 이 경우 텍스처를 devicePrivate로 표시하겠습니다. 왜냐하면 디바이스(GPU)에서 텍스처 메모리에 접근하는 명령만 사용하기 때문입니다.

```js
final texture = gpu.gpuContext.createTexture(gpu.StorageMode.devicePrivate,
    size.width.toInt(), size.height.toInt())!;
```

<div class="content-ad"></div>

만약 호스트(CPU)에서 업로드하여 텍스처 데이터를 덮어씌우려면 StorageMode.hostVisible을 사용하십시오.

세 번째로 사용할 수 있는 옵션은 StorageMode.deviceTransient이며, 렌더 패스(RenderPass)의 수명을 초과할 필요가 없는 첨부 파일에 유용합니다 (따라서 타일 메모리에만 존재하고 VRAM 할당이 필요하지 않음). 종종, 깊이/스텐실 텍스처가 이 기준에 맞습니다.

다음으로 RenderTarget을 정의합니다. 렌더 타겟에는 렌더 패스의 시작과 끝에서 프래그먼트 당 메모리 레이아웃을 설명하고 설정/해제 동작을 나타내는 "첨부 파일(attachments)"의 컬렉션이 포함되어 있습니다.

기본적으로 RenderTarget는 렌더 패스에 대한 재사용 가능한 서술자(descriptor)입니다.

<div class="content-ad"></div>

지금은 단일 색상 첨부 파일만 있는 매우 간단한 RenderTarget을 정의하십시오.

```js
최종 renderTarget = gpu.RenderTarget.singleColor(
gpu.ColorAttachment(texture: texture, clearValue: Colors.lightBlue));
```

이 코드는 clearValue를 연해색으로 설정한다는 것을 주목해주세요. 각 첨부에는 로드 작업(LoadAction)과 저장 작업(StoreAction)이 있습니다. 이 작업은 패스의 시작과 끝에서 첨부의 일시적인 타일 메모리에 무엇이 발생해야 하는지 결정합니다.

기본적으로, 색상 첨부는 LoadAction.clear(지정된 색상으로 타일 메모리를 초기화)와 StoreAction.store(결과를 첨부된 텍스처의 VRAM 할당에 저장)로 설정됩니다.

<div class="content-ad"></div>

이제 CommandBuffer를 만들어서 앞서 언급한 RenderTarget을 사용하여 RenderPass를 생성하고, 해당 CommandBuffer를 즉시 제출하세요.

```js
const commandBuffer = gpu.gpuContext.createCommandBuffer();
const renderPass = commandBuffer.createRenderPass(renderTarget);
// ... 여기에 draw 호출을 추가하세요!
commandBuffer.submit();
```

이제 초기화된 텍스처를 Canvas에 그릴 차례입니다!

```js
const image = texture.asImage();
canvas.drawImage(image, Offset.zero, Paint());
```

<div class="content-ad"></div>

![Getting started with Flutter GPU](/assets/img/2024-08-21-GettingstartedwithFlutterGPU_3.png)

이제 RenderPass가 화면에 결과를 표시하기 위해 연결되었으므로 삼각형을 그리기 시작할 준비가 되었습니다. 이를 위해 다음을 설정하세요:

- 셰이더에서 만든 RenderPipeline 및
- 여기에 세 개의 정점 위치가 포함된 GPU 액세스 가능한 버퍼.

RenderPipeline을 만드는 것은 쉽습니다. 라이브러리에서 정점 및 프래그먼트 셰이더를 결합하기만 하면 됩니다.

<div class="content-ad"></div>

```js
const vert = shaderLibrary['SimpleVertex']!;
const frag = shaderLibrary['SimpleFragment']!;
const pipeline = gpu.gpuContext.createRenderPipeline(vert, frag);
```

이제 geometry 부분입니다. "SimpleVertex" 쉐이더는 in vec2 position이라는 하나의 입력만 있다는 것을 기억하세요. 그래서 세 점을 그리려면 두 개의 부동 소수점 숫자 세트가 필요합니다.

```js
const vertices = Float32List.fromList([
  -0.5, -0.5, // 첫 번째 점
   0.5, -0.5, // 두 번째 점
   0.0,  0.5, // 세 번째 점
]);
const verticesDeviceBuffer = gpu.gpuContext
    .createDeviceBufferWithCopy(ByteData.sublistView(vertices))!;
```

이제 남은 일은 새 자원을 바인딩하고 renderPass.draw()를 호출하여 그리기 호출을 기록하는 것 뿐입니다.

<div class="content-ad"></div>

```js
renderPass.bindPipeline(pipeline);

final verticesView = gpu.BufferView(
  verticesDeviceBuffer,
  offsetInBytes: 0,
  lengthInBytes: verticesDeviceBuffer.sizeInBytes,
);
renderPass.bindVertexBuffer(verticesView, 3);

renderPass.draw();
```

이제 앱을 실행하면 초록색 삼각형이 나타날 것입니다!

<img src="/assets/img/2024-08-21-GettingstartedwithFlutterGPU_4.png" />

우와, Flutter, Dart 및 약간의 GLSL을 사용하여 처음부터 렌더러를 만들었네요!

<div class="content-ad"></div>

이 삼각형을 렌더링하는 것이 처음이든 경험이 풍부한 그래픽 전문가든, Flutter GPU와 Flutter Scene과 같은 저희가 작업 중인 패키지들을 계속해서 살펴봐 주면 좋을 것 같아요.

앞으로는 Flutter GPU의 기본 동작 및 모범 사례에 대해 깊이 파고드는 초보자 친화적인 코딜랩을 공개할 예정이에요. 아직까지 정점 속성 레이아웃, 텍스처 바인딩, 유니폼 및 정렬 요구사항, 파이프라인 블렌딩, 깊이 및 스텐실 첨부물, 원근 보정 등에 대해 이야기하지 않았지만, 많은 내용이 더 있어요!

그 전까지 Flutter GPU를 사용하는 방법을 보다 포괄적으로 보여주는 Flutter Scene을 살펴보는 것을 추천해요.

# Flutter Scene을 활용한 3D 렌더링

<div class="content-ad"></div>

플러터 씬(Flutter Scene) (패키지 flutter_scene)은 플러터 GPU를 이용한 새로운 3D 씬 그래프 패키지로, 플러터 개발자들이 애니메이션된 glTF 모델을 가져와 실시간 3D 씬을 렌더링할 수 있게 도와줍니다.

이 패키지의 목표는 플러터에서 상호작용하는 3D 앱과 게임을 쉽게 구축할 수 있도록 하는 것입니다.

![이미지](/assets/img/2024-08-21-GettingstartedwithFlutterGPU_5.png)

이 패키지는 기존에는 C++로 작성된 3D 렌더러를 dart:ui 확장으로 사용하고 있었으나, 더 유연한 인터페이스를 갖춘 Flutter GPU에 재작성되었습니다.

<div class="content-ad"></div>

Flutter GPU API 자체와 마찬가지로, Flutter Scene도 현재 초기 미리보기 상태이며 Impeller를 활성화해야 합니다. Flutter Scene은 일반적으로 Flutter GPU API의 주요 변경 사항을 따라가며, Flutter Scene을 실험할 때는 주로 메인 채널을 사용하는 것이 강력히 추천됩니다.

다음으로 Flutter Scene을 사용하여 앱을 만들어 보세요!

# Flutter Scene 프로젝트 설정하기

Flutter Scene을 주로 메인 채널과 함께 사용하는 것이 강력히 권장되므로, 먼저 해당 채널로 전환해 보세요.

<div class="content-ad"></div>

```js
flutter channel main
flutter upgrade
```

다음으로, 새로운 Flutter 프로젝트를 생성하세요.

```js
flutter create my_3d_app
cd my_3d_app
```

Flutter Scene은 새 실험적인 "native assets" 기능에 의존하여 자동으로 셰이더를 빌드합니다. 곧 사용하게 될 native assets를 설정하여 Flutter Scene에서 3D 모델을 자동으로 가져오도록 설정하겠습니다.

<div class="content-ad"></div>

다음 명령어를 사용하여 네이티브 자산을 활성화하세요.

```js
flutter config --enable-native-assets
```

마지막으로 Flutter Scene을 프로젝트 종속성으로 추가해주세요.

또한 Flutter Scene의 API와 상호작용하는 동안 여러 vector_math 구성 요소를 사용해야 하므로 vector_math 패키지도 추가해야 합니다.

<div class="content-ad"></div>

```js
flutter pub add flutter_scene vector_math
```

다음은 3D 모델을 가져오세요!

# 3D 모델 가져오기

먼저 렌더링할 3D 모델이 필요합니다. 이 안내서에서는 일반적인 glTF 샘플 자산인 DamagedHelmet.glb를 사용할 것입니다. 이게 그 모습입니다.

<div class="content-ad"></div>

```yaml
![이미지](/assets/img/2024-08-21-GettingstartedwithFlutterGPU_6.png)

glTF 샘플 에셋 저장소에서 다운로드할 수 있어요. DamagedHelmet.glb 파일을 프로젝트 루트에 넣어주세요.

curl -O https://raw.githubusercontent.com/KhronosGroup/glTF-Sample-Models/main/2.0/DamagedHelmet/glTF-Binary/DamagedHelmet.glb

대부분의 실시간 3D 렌더러와 마찬가지로, Flutter Scene은 전용 3D 모델 형식을 사용해요. 일반 glTF 바이너리(.glb 파일)를 이 형식으로 변환하려면 Flutter Scene의 오프라인 임포터 툴을 사용하시면 돼요.
```

<div class="content-ad"></div>

프로젝트에 flutter_scene_importer 패키지를 일반 의존성으로 추가해보세요.

```js
flutter pub add flutter_scene_importer
```

이 패키지를 추가하면 dart run을 사용하여 수동으로 임포터를 호출할 수 있습니다.

```js
dart --enable-experiment=native-assets \
     run flutter_scene_importer:import \
     --input "path/to/my/source_model.glb" \
     --output "path/to/my/imported_model.model"
```

<div class="content-ad"></div>

네이티브 자산 빌드 후크를 사용하여 가져오기 작업을 자동으로 실행할 수 있습니다. 이를 위해 먼저 일반 프로젝트 종속성으로 native_assets_cli를 설치하세요.

```js
flutter pub add native_assets_cli
```

이제 빌드 후크를 작성할 수 있습니다. 다음 내용으로 hook/build.dart 파일을 생성하세요.

```js
import 'package:native_assets_cli/native_assets_cli.dart';
import 'package:flutter_scene_importer/build_hooks.dart';

void main(List<String> args) {
  build(args, (config, output) async {
    buildModels(buildConfig: config, inputFilePaths: [
      'DamagedHelmet.glb',
    ]);
  });
}
```

<div class="content-ad"></div>

flutter_scene_importer의 buildModels 유틸리티를 사용하여 빌드할 모델 목록을 제공하세요. 이 경로는 프로젝트의 빌드 루트를 기준으로 상대적입니다.

플러터 도구가 프로젝트를 빌드할 때, buildModels는 이제 셰이더 번들을 빌드하고 결과를 패키지 루트에서 build/models/DamagedModel.model 경로로 출력합니다.

가져온 모델 형식 자체는 사용 중인 Flutter Scene의 특정 버전에 연결되어 있으며 시간이 지남에 따라 변경될 수 있습니다. Flutter Scene을 사용하는 앱이나 라이브러리를 작성할 때, 생성된 .model 파일을 소스 트리에 체크인하지 마세요. 대신, 이전에 설명한 대로 빌드 후크를 사용하여 소스 모델에서 이를 생성하세요.

이렇게 하면 Flutter Scene을 업그레이드할 때 항상 올바른 형식의 새로운 .model 파일을 빌드할 수 있습니다!

<div class="content-ad"></div>

다음으로, 일반 자산처럼 모델을 가져오세요. 프로젝트의 pubspec.yaml에 자산 항목을 추가하세요.

```js
flutter:
  assets:
    - build/models/
```

나중에 네이티브 자산 기능이 빌드 후크를 사용하여 데이터 자산을 번들에 추가할 수 있게 될 것입니다. 그렇게 되면 빌드 후크와 함께 자산 가져오기 규칙을 추가할 필요가 없게 될 것입니다.

# 3D 장면 렌더링

<div class="content-ad"></div>

앱의 코드에 대해 이제 설명하겠습니다.

먼저, Scene을 프레임 간에 유지하기 위한 Stateful 위젯을 만듭니다.

시간을 기반으로 애니메이션을 적용할 것이므로 SingleTickerProviderStateMixin을 상태에 추가하고 elapsedSeconds 멤버를 추가합니다.

```js
import 'dart:math';

import 'package:flutter/material.dart';
import 'package:flutter/scheduler.dart';
import 'package:flutter_scene/camera.dart';
import 'package:flutter_scene/node.dart';
import 'package:flutter_scene/scene.dart';
import 'package:vector_math/vector_math.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatefulWidget{
  const MyApp({Key key}) : super(key: key);

  @override
  MyAppState createState() => MyAppState();
}

class MyAppState extends State<MyApp> with SingleTickerProviderStateMixin {
  double elapsedSeconds = 0;
  Scene scene = Scene();

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'My 3D app',
      home: Placeholder(),
    );
  }
}
```

<div class="content-ad"></div>

앱을 스모크 테스트로 실행해서 오류가 없는지 확인해주세요. 그리고 Impeller를 활성화해야 합니다!

```js
flutter run -d macos --enable-impeller
```

<img src="/assets/img/2024-08-21-GettingstartedwithFlutterGPU_7.png" />

계속하기 전에 애니메이션을 위한 티커를 설정해주세요. MyAppState에서 initState를 오버라이드하여 createTicker를 호출하세요.

<div class="content-ad"></div>

```js
  @override
  void initState() {
    createTicker((elapsed) {
      setState(() {
        elapsedSeconds = elapsed.inMilliseconds.toDouble() / 1000;
      });
    }).start();

    super.initState();
  }
```

위젯이 화면에 표시되는 한, 매 프레임마다 티커 콜백이 호출됩니다. setState를 호출하면 이 위젯이 매 프레임마다 다시 빌드되도록 합니다.

다음으로, 이전에 프로젝트에 추가한 3D 모델을 로드하고 Scene에 추가합니다.

Node.fromAsset를 사용하여 에셋 번들에서 모델을 로드하세요. 아래 코드를 initState에 넣어주세요.


<div class="content-ad"></div>

```js
    Node.fromAsset('build/models/DamagedHelmet.model').then((model) {
      model.name = '헬멧';
      scene.add(model);
    });
``` 

Node.fromAsset은 모델을 자산 번들에서 비동기적으로 역직렬화하고 씬에 추가할 준비가 되면 반환된 Future`Node`를 해결합니다.

MyAppState.initState은 이제 다음과 같이 보여야 합니다:

```js
  @override
  void initState() {
    createTicker((elapsed) {
      setState(() {
        elapsedSeconds = elapsed.inMilliseconds.toDouble() / 1000;
      });
    }).start();

    Node.fromAsset('build/models/DamagedHelmet.model').then((model) {
      model.name = '헬멧';
      scene.add(model);
    });

    super.initState();
  }
```

<div class="content-ad"></div>

그러나 아직 3D 씬을 실제로 렌더링하지는 않았어요! Scene.render를 사용해야 해요. 이 함수는 UI Canvas, Flutter Scene Camera 및 크기를 필요로 해요.

캔버스에 액세스하는 한 가지 방법은 CustomPainter를 생성하는 것입니다:

```js
class ScenePainter extends CustomPainter {
  ScenePainter({required this.scene, required this.camera});
  Scene scene;
  Camera camera;

  @override
  void paint(Canvas canvas, Size size) {
    scene.render(camera, canvas, viewport: Offset.zero & size);
  }

  @override
  bool shouldRepaint(covariant CustomPainter oldDelegate) => true;
}
```

커스텀 페인터를 다시 그려야 할 때는 항상 true를 반환하도록 shouldRepaint 오버라이드를 설정하는 것을 잊지 마세요.

<div class="content-ad"></div>

마지막으로, 소스 트리에 CustomPainter를 추가하세요.

```js
  @override
  Widget build(BuildContext context) {
    final painter = ScenePainter(
      scene: scene,
      camera: PerspectiveCamera(
        position: Vector3(sin(elapsedSeconds) * 3, 2, cos(elapsedSeconds) * 3),
        target: Vector3(0, 0, 0),
      ),
    );

    return MaterialApp(
      title: 'My 3D app',
      home: CustomPaint(painter: painter),
    );
  }
```

이 코드는 카메라가 지속적으로 원을 그리는 동작을 지시하지만, 항상 원점을 향해 바라보도록 설정되어 있습니다.

마지막으로, 앱을 시작하세요!

<div class="content-ad"></div>

```js
flutter run -d macos --enable-impeller
```

<img src="/assets/img/2024-08-21-GettingstartedwithFlutterGPU_8.png" />

여기에 우리가 함께 넣은 전체 소스가 있어요.

```js
import 'dart:math';

import 'package:flutter/material.dart';
import 'package:flutter_scene/camera.dart';
import 'package:flutter_scene/node.dart';
import 'package:flutter_scene/scene.dart';
import 'package:vector_math/vector_math.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatefulWidget {
  const MyApp({super.key});

  @override
  MyAppState createState() => MyAppState();
}

class MyAppState extends State<MyApp> with SingleTickerProviderStateMixin {
  double elapsedSeconds = 0;
  Scene scene = Scene();

  @override
  void initState() {
    createTicker((elapsed) {
      setState(() {
        elapsedSeconds = elapsed.inMilliseconds.toDouble() / 1000;
      });
    }).start();

    Node.fromAsset('build/models/DamagedHelmet.model').then((model) {
      model.name = 'Helmet';
      scene.add(model);
    });

    super.initState();
  }

  @override
  Widget build(BuildContext context) {
    final painter = ScenePainter(
      scene: scene,
      camera: PerspectiveCamera(
        position: Vector3(sin(elapsedSeconds) * 3, 2, cos(elapsedSeconds) * 3),
        target: Vector3(0, 0, 0),
      ),
    );

    return MaterialApp(
      title: 'My 3D app',
      home: CustomPaint(painter: painter),
    );
  }
}

class ScenePainter extends CustomPainter {
  ScenePainter({required this.scene, required this.camera});
  Scene scene;
  Camera camera;

  @override
  void paint(Canvas canvas, Size size) {
    scene.render(camera, canvas, viewport: Offset.zero & size);
  }

  @override
  bool shouldRepaint(covariant CustomPainter oldDelegate) => true;
}
```

<div class="content-ad"></div>

# 플러터의 밝은 미래

만약 이 가이드 중 하나를 성공적으로 따라하고 무언가를 구동시킬 수 있었다면: 대단해요, 축하해요!

플러터 GPU와 플러터 Scene은 아직 플랫폼 지원이 제한적인 매우 어린 프로젝트입니다. 하지만 언젠가 우리는 이런 겸손한 시작들을 회고할 것이라고 생각해요.

임펠러 프로젝트로 인해 플러터 팀은 렌더링 스택에 대한 완전한 소유권을 가져가게 되었습니다. 왜냐하면 렌더러를 플러터의 사용 사례에 특화되게 만들어야 했기 때문이죠. 그리고 이제 우린 플러터 역사에서 새로운 장을 시작하고 있어요. 그 장에서 바로 **여러분이 렌더링을 제어**해요!

<div class="content-ad"></div>

Flutter Scene는 Impeller의 C++ 구성 요소로 시작되었습니다. 2D 캔버스 렌더러와 가늘고 다트:ui 확장이 함께 했죠. 제가 이것을 구축할 때에는 이미 Flutter Engine이 최종 목적지가 되지는 않을 것을 알고 있었습니다.

3D 렌더러를 위한 아키텍처 결정의 바다는 광활합니다. 어떤 범용 3D 렌더러도 모든 사용 사례를 잘 해결해주지는 못합니다. "범용"과 "고성능"은 일반적으로 상반된 목표입니다.

최고로 만능을 지향하는 것이 모든 것에 적합하다는 것을 의미하지는 않습니다.

렌더링 성능의 세계에서는 상황이 더욱 안좋습니다. 하나의 사용 사례에 특화되는 것은 종종 다른 사용 사례의 성능을 저하시키는 것을 의미합니다.

<div class="content-ad"></div>

간략히 말하자면, 모든 사용 사례를 해결할 수 있는 범용 3D 렌더러를 모든 사람에게 제공하는 것은 불가능합니다. 그러나 (Flutter GPU를) 이용하여 필요한 저수준 API를 노출시키고, Flutter 커뮤니티가 쉽게 검토하고 수정할 수 있는 유용한 범용 3D 렌더러(플러터 씬)를 이용해 여러분이 고유 솔루션을 만들 수 있도록 하고 있습니다. 이를 통해 플러터 개발자들이 구식 기능을 걱정할 필요 없이 안정적인 환경을 누리고 높은 보상을 받을 수 있습니다.

![image](https://miro.medium.com/v2/resize:fit:1012/1*jfeUgpEP9AgAz94yVxVW1g.gif)

새로운 기능들로 무엇을 만들어 낼 지 기대됩니다. 플러터 씬의 향후 릴리스를 기대해 주세요. 많은 것이 준비 중에 있습니다.

한편, 저는 다시 일하러 돌아가야겠습니다.

<div class="content-ad"></div>

곧 다시 만나요. :)