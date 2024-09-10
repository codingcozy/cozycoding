---
title: "Golang과 HTMX를 결합한 임베디드 리눅스용 도구 개발 방법"
description: ""
coverImage: "/assets/img/2024-09-10-GolangandHTMXcombinationforembeddedLinux_0.png"
date: 2024-09-10 17:44
ogImage: 
  url: /assets/img/2024-09-10-GolangandHTMXcombinationforembeddedLinux_0.png
tag: Tech
originalTitle: "Golang and HTMX combination for embedded Linux"
link: "https://medium.com/itnext/golang-and-htmx-combination-for-embedded-linux-8169887104e5"
isUpdated: false
---


지난 6년 동안 저는 Pantacor에서 일했습니다. 이 회사는 컨테이너 기반의 임베디드 리눅스를 위한 오픈 소스 프레임워크를 개발하는 회사입니다. 저희는 컨테이너 기반 구성 요소를 사용하여 임베디드 리눭스 솔루션을 만드는 데 중점을 두고 있습니다. 웹 개발자로, 저의 경험은 주로 웹 응용 프로그램 및 API 아키텍처 구축에 있었습니다. 대학에서부터 임베디드 시스템은 제 도구 상자의 일부가 아니었습니다. 그러나 임베디드 리눅스에 대한 웹 구성 요소를 사용한 작은 응용 프로그램을 만들어야 하는 필요성은 기존의 Rust + React 응용 프로그램 또는 셸 API와 React 조합 대신 대안으로 Go와 HTMX를 탐색하게 했습니다.

# 임베디드 리눅스 개발의 도전

임베디드 리눅스 시스템은 사용자 인터페이스와 웹 기반 응용 프로그램을 개발할 때 특별한 도전을 제공합니다. 이러한 시스템은 종종 제한된 자원 - 제한된 메모리와 처리 능력 -을 가지고 있어 효율적이고 가벼운 기술을 선택하는 것이 중요합니다.

# Golang과 HTMX 탐색하기

<div class="content-ad"></div>

Golang 또는 Go는 단순성과 효율성으로 유명한 정적 타입의 컴파일된 언어입니다. HTMX는 최신 브라우저 기능에 직접적으로 접근할 수 있게 해주는 가벼운 JavaScript 라이브러리로, 복잡한 JavaScript 프레임워크의 필요성을 줄이고 응용 프로그램을 구축하기 위한 전체 변환 및 컴파일 단계를 제거합니다.

장착된 리눅스 환경에서는 오프라인 상태에서 제공해야 하는 모든 에셋을 포함한 독립적인 응용 프로그램이 필요합니다. 즉, 장치의 핫스팟이나 이더넷을 통해 직접 연결되었을 때 서비스할 수 있는 응용 프로그램을 만들어야 합니다.

Golang과 HTMX의 조합은 장착된 리눅스 응용 프로그램에 대해 여러 가지 이점을 제공합니다:

- 자원의 효율성: Go와 HTMX는 작은 자원 소모량을 가지고 있어 자원이 제한된 임베디드 시스템에 적합합니다. 대부분의 경우, 전체 패키지 크기가 5MB를 초과하지 않는 컨테이너를 생성해야 합니다.
- 접근하기 쉬운 학습 곡선: 백엔드 및 CLI 응용 프로그램에서의 Go 경험을 고려하면, 학습 곡선은 주로 해결해야 하는 구체적인 문제에 중점을 두면 되며 언어 자체로 인한 걱정은 없습니다.
- 성능: Go의 컴파일된 특성과 HTMX의 최소한의 JavaScript 접근 방식은 효율적인 실행과 Overhead를 줄여줘서 자원이 제한된 환경에 중요합니다.
- 오프라인 기능: 응용 프로그램에 필요한 모든 에셋을 패키징하는 기능은 장착된 시스템에서의 오프라인 기능 요구와 완벽하게 일치합니다.
- 단순화된 개발 프로세스: 복잡한 빌드 프로세스를 제거하고 필요한 JavaScript의 양을 줄이면 개발 및 배포 워크플로우를 간소화할 수 있습니다.

<div class="content-ad"></div>

# 현실 세계 응용

고와 HTMX를 임베디드 리눅스 환경에서 사용하는 실용적인 이점을 보여주기 위해, Pantacor에서의 작업에서 가져온 두 가지 예시를 살펴보겠습니다:

# 1. WiFi 연결 앱

개발한 응용 프로그램 중 하나는 리눅스 장치용 WiFi 연결 도구입니다. 이 앱은 포털을 생성하여 사용자가 임베디드 리눅스 장치에 손쉽게 WiFi를 설정할 수 있게 합니다. 작동 방식은 다음과 같습니다:

<div class="content-ad"></div>

- 장치는 액세스 포인트를 만듭니다.
- 사용자가 휴대전화나 노트북을 사용하여이 액세스 포인트에 연결합니다.
- 웹 인터페이스를 통해 사용자는 집 네트워크의 WiFi 자격 증명을 제공할 수 있습니다.
- 장치는 그런 다음이 자격 증명을 사용하여 사용자의 집 네트워크에 연결합니다.

## 2. 장치 관리 앱

팬타바이저를 위한 장치 관리 애플리케이션의 또 다른 예는 다음과 같습니다. 이 웹 기반 유틸리티는 사용자가 브라우저 인터페이스를 통해 Linux 장치를 관리할 수 있도록 합니다. 기능은 다음과 같습니다:

- tarball 파일을 드래그 앤 드롭하여 새 컨테이너 설치
- 클릭 몇 번으로 컨테이너 제거
- 컨테이너 로그 읽기
- 컨테이너 구성 설정

<div class="content-ad"></div>

# Go와 HTMX 구현: 실용적인 예제

Go와 HTMX를 활용하여 간단한 WiFi 네트워크 관리 애플리케이션을 만드는 실용적인 예제를 살펴봅시다. 이 예제는 사용 가능한 네트워크 목록을 보여주고 선택한 네트워크에 연결하는 방법을 보여줍니다.

전체 예제의 소스 코드는 다음에서 확인할 수 있습니다:

https://github.com/highercomve/gohtmx

<div class="content-ad"></div>

# 서버 설정하기

먼저, Go로 작은 서버를 만들어 세 가지 엔드포인트를 갖도록 설정할 거에요:

- GET /: 애플리케이션의 index.html을 반환합니다.
- GET /networks: HTML로 네트워크 목록을 반환합니다.
- POST /networks: 선택한 네트워크를 저장하고 연결합니다.

여기에 인덱스 엔드포인트를 구현하는 방법이 있어요:

<div class="content-ad"></div>

```go
func getIndex(w http.ResponseWriter, r *http.Request, opts *servermodels.ServerOptions) {
    data := servermodels.Response{
        Code:  http.StatusOK,
        Data:  nil,
        Error: nil,
    }
    render(w, r, opts, "index.html", &data)
}
```

# Index HTML

index.html 파일은 HTMX를 사용하여 상호작용하는 인터페이스를 만듭니다:

```js
{template "layout_start"}
{define "title"}Go HTMX Example{end}
<section class="flex justify-content-center pb-2">
    <header class="col-6 text-center">
        <h1>Manage wifi networks with Golang + HTMX + nmcli</h1>
    </header>
</section>
<section class="flex justify-content-center mb-2">
    <header class="col-12 flex align-items-center justify-content-center mb-2">
        <button
            class="btn btn-primary"
            hx-get="/networks"
            hx-target="#connections-list"
            hx-swap="morph:outerHTML"
        >
            Scan Networks
            <div class="htmx-indicator spinner-grow" role="status"></div>
        </button>
    </header>
    {if notNil .Data} { block "network_list" . }{end} {else}
    <section id="connections-list">
        <div
            hx-get="/networks"
            hx-target="#connections-list"
            hx-swap="morph:outerHTML"
            hx-trigger="load"
            class="flex flex-direction-column justify-content-center align-items-center"
        >
            <div class="htmx-indicator spinner-grow" role="status"></div>
            <div class="pt-1 color-grey-300">Loading networks...</div>
        </div>
    </section>
    {end}
</section>
{template "layout_end"}
```

<div class="content-ad"></div>

# HTMX 속성 이해하기

저희 WiFi 관리 애플리케이션에서 사용하는 HTMX 속성들을 좀 더 살펴보겠습니다. 이러한 속성들은 복잡한 JavaScript 없이도 동적이고 앱과 같은 느낌을 제공하는 데 기여합니다:

- hx-get="/networks": 이 속성은 요소가 트리거될 때 HTMX에게 "/networks" 엔드포인트로 GET 요청을 보내라고 알려줍니다. "네트워크 스캔" 버튼에서는 이를 클릭하면 사용 가능한 네트워크 목록을 가져오는 것을 의미합니다.
- hx-target="#connections-list": 이것은 서버로부터의 응답이 삽입되어야하는 위치를 지정합니다. 여기서는 네트워크 목록이 "connections-list"라는 ID를 가진 요소에 배치될 것입니다.
- hx-swap="morph:outerHTML": 이것은 새 콘텐츠를 어떻게 교체할지를 결정합니다. "morph" 전략은 DOM을 지능적으로 업데이트하여 부드러운 전환을 위해 변경을 최소화합니다. "outerHTML"은 대상 요소 전체를(요소 자체 포함) 바꿈을 의미합니다.
- hx-trigger="load": 이 속성은 HTMX 요청이 언제 트리거될지를 정의합니다. "load"로 설정하면 요청은 요소가 DOM으로 로드될 때 즉시 실행됩니다. 첫 번째 네트워크 목록 div에서 사용하여 페이지가 로드될 때 네트워크를 자동으로 로드합니다.
- `div class="htmx-indicator spinner-grow" role="status"``/div`: hx-* 속성은 아니지만, 이 div는 HTMX의 로딩 인디케이터를 표시하는데 사용됩니다. "htmx-indicator" 클래스는 요청이 진행 중일 때 이 요소를 표시하고 그 외에는 숨기도록 HTMX에게 지시합니다.

이러한 HTMX 속성을 통해 우리는 최소한의 JavaScript로 반응적이고 동적인 사용자 인터페이스를 생성할 수 있습니다. 서버는 이러한 요청에 대해 HTML을 보내고, HTMX는 이를 페이지에 매끄럽게 통합합니다. 이 접근 방식은 임베디드 리눅스 환경에서 특히 유용하며, 클라이언트 측 처리를 최소화하면서도 부드러운 사용자 경험을 제공하고자 하는 경우에 특히 이점이 있습니다.

<div class="content-ad"></div>

HTMX 기능을 활용하면 임베디드 리눅스 시스템과 작업할 때 중요한 요소인 우리 애플리케이션을 가벼우며 효율적으로 유지하면서도 반응이 빠르고 현대적인 WiFi 관리 인터페이스를 만들 수 있습니다.

# 사용 가능한 네트워크 목록

GET /networks 엔드포인트는 사용 가능한 WiFi 네트워크 목록을 검색하여 표시합니다.

```js
func getNetworksList(w http.ResponseWriter, r *http.Request, opts *servermodels.ServerOptions) {
  networkmanager := nm.Init()
  conns, err := networkmanager.List()
  if err != nil {
    opts.Logger.Println(err)
    data := servermodels.Response{
      Code:  http.StatusInternalServerError,
      Data:  nil,
      Error: err,
    }
    render(w, r, opts, "error.html", &data)
    return
  }
  data := servermodels.Response{
    Code:  http.StatusOK,
    Data:  conns,
    Error: nil,
  }
  render(w, r, opts, "network_list", &data)
}
```

<div class="content-ad"></div>

네트워크 관리자(List() 함수)는 사용 가능한 WiFi 네트워크 목록을 검색하기 위해 nmcli 명령을 사용합니다:

```js
func (nm *NetworkManager) List() (conns []nmmodules.WifiConn, err error) {
  cmd := exec.Command(
    "nmcli",
    "-f",
    "ssid,mode,freq,signal,active,security",
    "device",
    "wifi",
    "list",
 )
 stdout, err := cmd.Output()
 if err != nil {
   fmt.Println("nmcli 명령 실행 중 오류 발생:", err)
   return
 }
 
 // ... (출력을 구문 분석하고 WifiConn 구조체 생성하는 코드)
 return conns, err
}
```

# 네트워크 목록 HTML

```js
{ define "network_list" } { $numOfConns := len .Data }
<section
 id="connections-list"
 class="connections flex justify-content-center col-12"
>
 <form
  class="col-6"
  hx-post="/networks"
  hx-target="body"
  hx-confirm="이 변경 사항을 적용하면 장치가 새 네트워크에 연결되고 AP가 사라집니다."
 >
  {if notNil .Error}
  <div class="alert alert-danger">{.Error}</div>
  {end}
  <div
   class="input-group mb-2 flex align-items-center justify-content-center"
  >
   <details class="custom-select">
    <summary class="radios">
     { if (eq $numOfConns 0) }
     <input type="radio" title="사용 가능한 네트워크 없음" checked />
     { else } {range $conn := .Data}
     <input
      type="radio"
      name="ssid"
      id="{$conn.ID}"
      title="{$conn.SSID} {if $conn.Active}*{end}"
      value="{$conn.SSID}"
      {if
      $conn.Active}checked{end}
     />
     { end } { end }
    </summary>
    <ul class="list">
     { if eq $numOfConns 0 }
     <li>
      <label>No networks</label>
     </li>
     { else } {range $conn := .Data}
     <li>
      <label for="{$conn.ID}">
       {block "signal" $conn.Strength}{end}
       {$conn.SSID}
       <span class="ml-05"> {$conn.Frequency} </span>
       { if $conn.Active } * { end }
      </label>
     </li>
     {end} { end }
    </ul>
   </details>
  </div>
  <div class="input-group mb-1 flex">
   <label class="input-group-text" for="password">비밀번호</label>
   <input
    name="password"
    id="password"
    type="password"
    aria-label="Password"
    class="form-control"
   />
  </div>
  <div class="input-group mb-2 flex">
   <input
    type="checkbox"
    id="show-password"
    class=""
    onclick="showPassword('password')"
   />
   <label class="input-group-text" for="show-password"
    >비밀번호 표시</label
   >
  </div>
  <script>
   function showPassword(elementID) {
    var x = document.getElementById(elementID);
    if (x.type === "password") {
     x.type = "text";
    } else {
     x.type = "password";
    }
   }
  </script>
  <div class="input-group mb-2 flex justify-content-center">
   <button class="btn btn-primary" type="submit">
    이 네트워크에 연결
    <div class="htmx-indicator spinner-grow" role="status"></div>
   </button>
  </div>
 </form>
</section>
{ end }
```

<div class="content-ad"></div>

이 HTML은 다음과 같은 인터페이스를 생성합니다:

![Interface](/assets/img/2024-09-10-GolangandHTMXcombinationforembeddedLinux_0.png)

# 네트워크에 연결하기

이 섹션에서는 nmcli를 사용하여 WiFi 네트워크에 연결하는 방법을 안내합니다. nmcli는 NetworkManager를 제어하는 명령줄 도구로, 이 프로세스는 Golang 및 HTMX 애플리케이션에 통합되어 끊김 없는 연결 경험을 제공할 것입니다.

<div class="content-ad"></div>

## POST /networks Endpoint

사용자가 목록에서 네트워크를 선택하고 암호를 제출하면 POST /networks 엔드포인트가 디바이스를 선택한 WiFi 네트워크에 연결하는 책임을 집니다. 여기에 플로우를 상세히 설명합니다:

- 사용자가 네트워크 및 암호를 제출: 사용자 인터페이스를 통해 사용자가 네트워크를 선택하고 암호를 입력할 수 있습니다.
- 폼 제출: 폼 데이터(네트워크 SSID 및 암호)가 POST를 통해 /networks 엔드포인트로 제출됩니다.
- NetworkManager CLI (nmcli) 통합: Go 애플리케이션은 선택한 WiFi 네트워크에 연결을 처리하기 위해 nmcli를 호출합니다.

네트워크 생성 방법

<div class="content-ad"></div>

```js
func handleNetworkConnection(w http.ResponseWriter, r *http.Request, opts *servermodels.ServerOptions) {
    ssid := r.FormValue("ssid")
    password := r.FormValue("password")
    networkmanager := nm.Init()

    // nmcli를 사용하여 연결하는 함수 호출
    err := networkmanager.Save(ssid, password)
    if err != nil {
        opts.Logger.Println(err)
        data := servermodels.Response{
            Code:  http.StatusOK,
            Data:  nil,
            Error: err,
        }
        render(w, r, opts, "error.html", &data)
        return
    }

    data := servermodels.Response{
        Code:  http.StatusOK,
        Data:  nil,
        Error: nil,
    }
    opts.Logger.Printf("%s에 성공적으로 연결되었습니다.\n", ssid)
    render(w, r, opts, "", &data)
}
```

저장된 연결 방법은 다음과 같습니다.

```js
func (nm *NetworkManager) Save(ssid, password string) error {
    // 네트워크에 연결하기 위해 nmcli 사용
    cmd := exec.Command("nmcli", "device", "wifi", "connect", ssid, "password", password)

    // 출력과 오류를 각각 캡처
    var stderr bytes.Buffer
    var stdout bytes.Buffer
    cmd.Stdout = &stdout
    cmd.Stderr = &stderr

    // 명령 실행
    err := cmd.Run()
    if err != nil {
        // 오류 발생 시 nmcli의 stderr를 포함한 Go 오류로 반환
        return fmt.Errorf("%s에 연결 실패: %s, nmcli 오류: %s", ssid, err.Error(), stderr.String())
    }

    // 선택적으로 stdout에서 성공 메시지 로깅
    fmt.Println("nmcli 출력:", stdout.String())

    return nil
}
```

# Docker로 응용 프로그램 묶는 방법


<div class="content-ad"></div>

앱이 작동한 후에는 도커를 사용하여 응용 프로그램을 번들로 만들 수 있고 약 3mb 크기의 컨테이너가 생기게 됩니다.

컨테이너를 생성하는 방법에 대해 자세히 설명하지는 않겠습니다. 이미 다른 글에서 그 내용을 다뤘기 때문이죠.

https://itnext.io/optimize-your-docker-containers-6255545dfa54

# 결론

<div class="content-ad"></div>

Pantacor에서는 내장형 리눅스 응용 프로그램을 개발하는 데 Go와 HTMX를 사용함으로써 개발 프로세스를 획기적으로 효율화했습니다. 이를 통해 우리는 임베디드 시스템의 독특한 도전에 부합하는 효율적이고 자체 포함형 응용 프로그램을 만들 수 있게 되었습니다. 이 접근 방식이 모든 상황에 적합하지는 않을 수 있지만, 특히 제한된 자원을 필요로 하는 장치들에 대해 사용자 친화적인 인터페이스를 제공하기 위해 우리의 도구 상자에서 강력한 도구로 입증되었습니다.

임베디드 시스템이 계속 발전하고 보급되면서, 효율적이고 가벼운 웹 기술의 필요성이 더욱 증가할 것입니다. Go와 HTMX의 결합은 임베디드 리눅스 장치를 위한 현대적인 웹 인터페이스를 구축하려는 개발자들에게 유망한 방향을 제시합니다.

Go의 효율성과 HTMX의 간단함을 활용함으로써, 우리는 임베디드 시스템의 제약 사항을 존중하면서도 반응성이 뛰어나며 기능이 풍부한 응용 프로그램을 만들 수 있습니다. 이 접근 방식은 개발을 간소화할 뿐만 아니라 응용 프로그램이 미래 요구 사항에 대응하고 유지보수하기 쉬운 것을 보장합니다.