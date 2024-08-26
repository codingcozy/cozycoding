---
title: "Node.js로 간단한 CLI 도구 만들기 초보자를 위한 가이드"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-26 18:38
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Building a Simple CLI Tool with Node.js"
link: "https://medium.com/@tarundhiman1309/building-a-simple-cli-tool-with-node-js-6e9783e3647e"
isUpdated: false
---


이 글에서는 Node.js를 사용하여 간단한 Command-Line Interface (CLI) 도구를 만드는 방법을 안내하려고 해요. 이 도구는 단어 수 세기, 파일 읽기, 파일 내 줄 수 세기와 같은 기본 파일 작업을 수행하도록 설계되었어요. 개발 도구에 가벼운 유틸리티를 추가하려면 이 프로젝트가 시작하는 방법을 안내할 거에요.

CLI 도구를 만들 이유는?

CLI 도구는 반복적인 작업을 자동화하고 특정 작업을 명령줄에서 직접 처리하는 데 매우 유용해요. 시간을 절약하고 파일, 데이터베이스 또는 기타 시스템과 빠르게 상호작용하는 빠른 방법을 제공할 수 있어요.

이 프로젝트에서는 Counter라는 CLI 도구를 만들기로 결정했어요:

<div class="content-ad"></div>

- 파일의 단어 수를 세어보세요.
- 파일의 내용을 콘솔에 출력하세요.
- 파일의 줄 수를 세어보세요.

이제 이것을 어떻게 만들었는지 알아봅시다.

시작하기

먼저, 컴퓨터에 Node.js가 설치되어 있어야 합니다. 아직 설치하지 않았다면 여기서 다운로드할 수 있습니다.

<div class="content-ad"></div>

다음으로, 프로젝트를 위한 새 디렉토리를 만들고 해당 디렉토리로 이동해주세요:

```js
mkdir myCounter-cli
cd myCounter-cli
```

새로운 Node.js 프로젝트를 초기화하세요:

```js
npm init -y
```

<div class="content-ad"></div>

지금 우리가 CLI를 만드는 데 사용할 commander 패키지를 설치해 보세요:

```js
npm install commander
```

코드 작성하기

프로젝트 루트 디렉토리에 counter.js라는 파일을 만들어주세요. 이 파일에는 우리의 CLI 도구 코드가 포함될 것입니다. 아래에 설명과 함께 완전한 코드가 있습니다:

<div class="content-ad"></div>

```js
// 'commander' 패키지에서 Command 클래스를 가져옵니다.
const { Command } = require("commander");

// 파일 시스템을 다루기 위해 'fs' (파일 시스템) 모듈을 가져옵니다.
const fs = require("fs");

// Command 클래스의 새 인스턴스를 생성합니다.
const program = new Command();

// CLI 도구의 이름, 설명 및 버전을 설정합니다.
program
  .name("counter") // CLI 도구의 이름을 'counter'로 설정
  .description("일부 JavaScript 유틸리티에 대한 CLI") // CLI 도구에 대한 설명 추가
  .version("0.8.0"); // CLI 도구의 버전을 설정

// 'count' 명령어를 정의합니다.
program
  .command("count") // 명령어의 이름을 정의
  .description("파일의 단어 수를 계산합니다") // 명령어의 동작을 설명
  .argument("<file>", "단어 수를 계산할 파일") // 명령어가 파일을 필요로 한다는 것을 명시
  .action((file) => {
    // 명령어 호출 시 수행할 동작 정의
    // 파일을 UTF-8 인코딩으로 비동기적으로 읽기
    fs.readFile(file, "utf8", (err, data) => {
      if (err) {
        // 오류 발생 시, 콘솔에 로그를 출력하고 함수 종료
        console.error(err);
        return;
      }
      // 파일 내용을 공백으로 분할하여 단어 배열로 만듭니다.
      const words = data.split(" ");
      // 콘솔에 단어 수를 출력합니다.
      console.log(words.length);
    });
  });

// 'read_file' 명령어를 정의합니다.
program
  .command("read_file") // 명령어의 이름을 정의
  .description("파일을 읽고 내용을 콘솔에 출력합니다") // 명령어의 동작을 설명
  .argument("<file>", "읽을 파일") // 명령어가 파일을 필요로 한다는 것을 명시
  .action((file) => {
    // 명령어 호출 시 수행할 동작 정의
    // 파일을 UTF-8 인코딩으로 비동기적으로 읽기
    fs.readFile(file, "utf8", (err, data) => {
      if (err) {
        // 오류 발생 시, 콘솔에 로그를 출력하고 함수 종료
        console.error(err);
        return;
      }
      // 파일 내용을 콘솔에 출력합니다.
      console.log(data);
    });
  });

// 'read_Line' 명령어를 정의합니다.
program
  .command("read_Line") // 명령어의 이름을 정의
  .description("파일의 줄 수를 계산합니다") // 명령어의 동작을 설명
  .argument("<file>", "읽을 파일") // 명령어가 파일을 필요로 한다는 것을 명시
  .action((file) => {
    // 명령어 호출 시 수행할 동작 정의
    // 파일을 UTF-8 인코딩으로 비동기적으로 읽기
    fs.readFile(file, "utf8", (err, data) => {
      if (err) {
        // 오류 발생 시, 콘솔에 로그를 출력하고 함수 종료
        console.error(err);
        return;
      }
      // 파일 내용을 개행 문자로 분할하여 줄 배열로 만듭니다.
      const lines = data.split("\n");
      // 콘솔에 줄 수를 출력합니다.
      console.log(lines.length);
    });
  });

// 명령줄 인수를 구문 분석하고 해당하는 명령어를 실행합니다.
program.parse();
``` 

# 설명

- Commander.js: 이것은 Node.js 패키지로서 CLI 도구를 만드는 과정을 단순화시켜줍니다. CLI 도구를 위한 명령어, 인수 및 옵션을 정의하는 간단한 API를 제공합니다.
- 파일 시스템(fs) 모듈: 내장된 Node.js fs 모듈을 사용하여 파일 시스템과 상호 작용할 수 있게 되어 파일을 읽고 내용에 대한 작업을 수행할 수 있습니다.
- 명령어:
— count: 파일 내용을 공백을 기준으로 분할하여 파일의 단어 수를 계산합니다.
— read_file: 파일 전체 내용을 콘솔에 출력합니다.
— read_Line: 파일 내용을 개행 문자를 기준으로 분할하여 파일의 줄 수를 계산합니다.

도구 실행하기   

<div class="content-ad"></div>

위 코드를 작성한 후 파일을 저장하고 터미널로 돌아가세요. 이제 다음 명령어를 실행할 수 있어요:

- 파일 안의 단어 수를 세는 방법:

```js
node counter.js count <file_path>
```

- 파일 내용을 출력하는 방법:  

<div class="content-ad"></div>

```js
node counter.js read_file <file_path>
```

3. 파일에서 라인 수를 세려면:

```js
node counter.js read_Line <file_path>
```

# 결론

<div class="content-ad"></div>

Node.js로 CLI 도구를 만드는 것은 간단하면서도 강력합니다. 이 프로젝트는 기본 파일 작업을 처리하는 유틸리티를 빠르게 만드는 방법을 보여줍니다. 이 원칙들은 더 복잡한 작업에 적용될 수 있습니다. 프로세스 자동화하거나 실험해보는 중이든, CLI 도구는 개발자로서 생산성을 크게 향상시켜줄 수 있습니다.

GitHub에서 소스 코드를 확인하고 한 번 실행해보세요! 즐거운 코딩 되세요!