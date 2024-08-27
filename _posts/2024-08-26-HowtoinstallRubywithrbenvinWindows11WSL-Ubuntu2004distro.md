---
title: "Windows 11의 WSL-Ubuntu 20.04 배포판에서 rbenv를 사용해 Ruby를 설치하는 방법"
description: ""
coverImage: "/assets/img/2024-08-26-HowtoinstallRubywithrbenvinWindows11WSL-Ubuntu2004distro_0.png"
date: 2024-08-26 19:13
ogImage: 
  url: /assets/img/2024-08-26-HowtoinstallRubywithrbenvinWindows11WSL-Ubuntu2004distro_0.png
tag: Tech
originalTitle: "How to install Ruby with rbenv in Windows 11 WSL-Ubuntu 20.04 distro"
link: "https://medium.com/@satriajanaka09/how-to-install-ruby-with-rbenv-in-windows-11-wsl-ubuntu-20-04-distro-3a38c389cf89"
isUpdated: true
updatedAt: 1724743465123
---


<img src="/assets/img/2024-08-26-HowtoinstallRubywithrbenvinWindows11WSL-Ubuntu2004distro_0.png" />

윈도우 11 WSL-Ubuntu 20.04 배포본에 완전한 Ruby를 설치하는 데 사용한 단계입니다.

- 필수 패키지 설치

```js
$ sudo apt-get update
```

<div class="content-ad"></div>

2. rbenv 복제하기

```js
$ git clone https://github.com/rbenv/rbenv.git ~/.rbenv
```

3. rbenv 플러그인 복제하기

```js
$ git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build
$ git clone https://github.com/rkh/rbenv-whatis.git ~/.rbenv/plugins/rbenv-whatis
$ git clone https://github.com/rkh/rbenv-use.git ~/.rbenv/plugins/rbenv-use
```

<div class="content-ad"></div>

참고:

만약 이 메시지를 받았다면:

```js
fatal: unable to access 'https://github.com/rbenv/ruby-build.git/': gnutls_handshake() failed: The TLS connection was non-properly terminated.
```

터미널에서 이 스크립트를 먼저 실행해야 할 수도 있습니다:

<div class="content-ad"></div>

```js
$ sudo apt-get install -y libcurl4-openssl-dev
```

4. rbenv 유틸리티 컴파일

```js
$ cd ~/.rbenv && src/configure && make -C src
```

참고:

<div class="content-ad"></div>

만약 위의 명령을 실행한 후 이 메시지를 보게 된다면, 먼저 이 스크립트를 실행해야 할 수도 있습니다:

```bash
$ sudo apt-get install build-essential
```

5. PATH에 rbenv 추가하기

```bash
$ echo 'export PATH=$HOME/.rbenv/bin:$PATH' >> ~/.bash_profile
```

<div class="content-ad"></div>

6. ~/.bash_profile 파일의 끝에 이 줄을 추가해주세요.

```js
eval "$(rbenv init -)"
```

7. rbenv를 설치한 사용자로 로그아웃 후 다시 로그인하여 모든 것이 제대로 작동하는지 확인해주세요.

```js
$ exit
$ su -l <your_username>
```

<div class="content-ad"></div>

8. 모든 사용 가능한 Ruby 버전을 나열하여 rbenv를 확인해보세요.

```js
$ rbenv install -l
```

9. Ruby의 버전을 설치하세요.

```js
$ rbenv install 3.1.1
```

<div class="content-ad"></div>

에러 메시지를 받은 경우 다음과 같은 스크립트를 먼저 실행해야 할 수 있습니다:

```js
$ apt-get install -y libssl-dev zlib1g-dev
```

<div class="content-ad"></div>

10. 이 스크립트를 실행하세요

```js
$ rbenv rehash
```

11. 설치된 루비 버전들을 나열합니다

```js
$ rbenv versions
  3.1.1
```

<div class="content-ad"></div>

12. 루비의 로컬 버전을 설정하세요

```js
$ rbenv local 3.1.1
```

13. 또는, 루비의 글로벌 버전을 설정하세요

```js
$ rbenv global 3.1.1
```

<div class="content-ad"></div>

14. 루비 버전 표시

```js
$ ruby -v
```

15. 젬 버전 표시

```js
$ gem -v
```