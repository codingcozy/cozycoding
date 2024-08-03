---
title: "NextJs 애플리케이션에서 TinyMCE와 Drawio 및 MathType 통합하는 방법"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-03 19:29
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Integrate Drawio and MathType with TinyMCE in NextJs Application"
link: "https://dev.to/sagar_gurung_7/integrate-drawio-and-mathtype-with-tinymce-in-nextjs-application-1lbd"
---


이 기사에서는 TinyMCE 리치 텍스트 편집기와 Draw.io 및 MathType를 통합하여 React 애플리케이션 내에서 다이어그램 생성, 수학 표현 작성하는 방법을 살펴보겠습니다. 편리하게 mantine 컴포넌트 라이브러리를 사용했습니다. 해당 내용은 mantine.dev에서 확인할 수 있습니다. 이 통합을 통해 사용자는 신속한 작업 흐름을 통해 콘텐츠에 다이어그램을 직접 만들고 삽입할 수 있습니다.

## 프로젝트 설정하기

먼저 React 프로젝트가 설정되어 있는지 확인하세요. 설정되어 있지 않다면 다음 명령어를 사용하여 React 앱을 만드세요:

```js
npx create-react-app tinymce-drawio-integration
cd tinymce-drawio-integration
```

<div class="content-ad"></div>

## 종속 항목 설치

다음으로 TinyMCE, Draw.io 및 기타 필수 패키지를 설치해야 합니다:

```js
npm install @tinymce/tinymce-react react-drawio @mantine/hooks @mantine/core @wiris/mathtype-tinymce6
```

다음으로 통합을 위한 새 컴포넌트를 만들어야 합니다. Editor.tsx라는 이름을 붙여보죠.

<div class="content-ad"></div>

## 에디터 컴포넌트 만들기

Editor.tsx 파일 안에는 TinyMCE 에디터를 구현하고 모달 내에서 Draw.io 다이어그램 에디터를 통합할 것입니다. 아래는 전체 코드입니다:

```js
import React, { FC, useRef, useState } from "react";
import { Editor as TinyMCEEditor } from "@tinymce/tinymce-react";
import { DrawIoEmbed, DrawIoEmbedRef, EventExport } from "react-drawio";
import { useDisclosure } from "@mantine/hooks";
import { Button, Modal } from "@mantine/core";

const Editor = () => {
  const [opened, { open, close }] = useDisclosure(false);
  const [editorContent, setEditorContent] = useState("");
  const drawioRef = useRef<DrawIoEmbedRef>(null);

  const exportData = () => {
    if (drawioRef.current) {
      drawioRef.current.exportDiagram({
        format: "xmlsvg",
      });
    }
  };

  const handleEditorChange = (content: string) => {
    setEditorContent(content);
  };

  const handleSaveDrawio = (data: EventExport) => {
    setEditorContent(
      (prevState) =>
        prevState + `<img src="${data.data}" alt="Drawio Diagram"/>`
    );
    close();
  };

  return (
    <div>
      <TinyMCEEditor
        id={id}
        apiKey="YOUR_TINYMCE_API_KEY"
        value={editorContent}
        init={
          height: 500,
          draggable_modal: true,
          extended_valid_elements: "*[.*]",
          plugins: [
            "advlist",
            "autolink",
            "lists",
            "link",
            "image",
            "charmap",
            "preview",
            "anchor",
            "searchreplace",
            "visualblocks",
            "code",
            "fullscreen",
            "insertdatetime",
            "media",
            "table",
            "help",
            "wordcount",
            "tiny_mce_wiris",
          ],
          toolbar:
            "undo redo | formatselect | bold italic backcolor | \
            alignleft aligncenter alignright alignjustify | \
            bullist numlist outdent indent | removeformat | help | image | table | drawio | tiny_mce_wiris_formulaEditor | tiny_mce_wiris_formulaEditorChemistry",
          setup: (editor) => {
            editor.ui.registry.addButton("drawio", {
              text: "Draw",
              onAction: () => {
                open();
              },
            });
          },
          external_plugins: {
            tiny_mce_wiris: `http://localhost:3000/tinymce_plugins/mathtype/mathtype-tinymce6/plugin.min.js`,
          },
        }
        onEditorChange={handleEditorChange}
      />
      <Modal
        opened={opened}
        onClose={close}
        size="80vw"
        withCloseButton={false}
      >
        <div className="h-[80vh] relative">
          <Button
            className="bg-accent absolute right-14 top-4"
            onClick={exportData}
            size="compact-xs"
          >
            Export
          </Button>
          <DrawIoEmbed
            ref={drawioRef}
            onExport={handleSaveDrawio}
            urlParameters={
              ui: "kennedy",
              spin: true,
              noSaveBtn: true,
              libraries: true,
              saveAndExit: false,
              noExitBtn: true,
            }
          />
        </div>
      </Modal>
    </div>
  );
};

export default Editor;
```

## 설명

<div class="content-ad"></div>

1. TinyMCE 편집기: 우리는 다양한 플러그인과 사용자 지정 툴바를 사용하여 TinyMCE 편집기를 구성합니다. 설정 함수는 Draw.io 모달을 열 수 있는 사용자 지정 버튼을 추가합니다.

2. Draw.io 통합: Draw.io를 임베드하기 위해 react-drawio 패키지를 사용합니다. DrawIoEmbed 컴포넌트는 @mantine/core의 Modal로 래핑됩니다.

3. 상태 관리: 컴포넌트는 편집기 콘텐츠를 editorContent 상태에 유지합니다. @mantine/hooks의 useDisclosure 훅은 모달의 열기 및 닫기 상태를 관리합니다.

4. 다이어그램 내보내기와 임베드: 사용자가 다이어그램을 내보낼 때, exportData 함수가 호출되어 DrawIoEmbedRef의 exportDiagram 메서드를 호출합니다. 그 결과로 나온 SVG 데이터는 TinyMCE 편집기 콘텐츠에 임베드됩니다.

<div class="content-ad"></div>

5. 수식 편집기 통합: node_modules에 설치된 MathType 플러그인을 복사하여 public 폴더에 붙여넣고, 편집기 external_plugins 섹션에 플러그인 경로를 제공해주세요.

```js
external_plugins: {
  tiny_mce_wiris: `http://localhost:3000/tinymce_plugins/mathtype/mathtype-tinymce6/plugin.min.js`,
},
``` 

통합한 부분 사용자 정의하기 
1. API 키: "YOUR_TINYMCE_API_KEY"를 귀하의 TinyMCE API 키로 교체해주세요.
2. UI 및 기능: DrawIoEmbed 컴포넌트의 urlParameters를 조정하여 Draw.io 편집기 인터페이스를 사용자 정의할 수 있습니다.

## 애플리케이션 실행하기

<div class="content-ad"></div>

컴포넌트를 설정한 후에는 이제 애플리케이션 내에서 사용할 수 있습니다. 메인 애플리케이션 파일에서 MyComponent를 import하여 사용하세요:

```js
import React from "react";
import ReactDOM from "react-dom";
import Editor from "./Editor";

const App = () => {
  return (
    <div>
      <h1>React TinyMCE와 Draw.io 통합</h1>
      <Editor />
    </div>
  );
};

ReactDOM.render(<App />, document.getElementById("root"));
```

이제 애플리케이션을 시작하세요:

```js
npm start
```

<div class="content-ad"></div>

TineMCE에 사용자 정의 "그리기" 버튼이 있는 에디터가 표시됩니다. 이 버튼을 클릭하면 모달에서 Draw.io 편집기가 열리며 다이어그램을 손쉽게 생성하고 삽입할 수 있습니다.

## 결론

React 애플리케이션에서 TinyMCE와 Draw.io를 통합하면 풍부한 텍스트 편집 기능과 강력한 다이어그램 생성 기능을 결합하여 사용자 경험을 향상시킬 수 있습니다. 이 통합은 문서 작성 도구, 교육 플랫폼, 협업 공간과 같이 상세한 콘텐츠 작성이 필요한 애플리케이션에 특히 유용합니다.