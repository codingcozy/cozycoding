---
title: "스위프트로 API 호출하는 방법 초보자용 가이드"
description: ""
coverImage: "/assets/img/2024-08-26-HowtoCallAPIsinSwift_0.png"
date: 2024-08-26 19:31
ogImage: 
  url: /assets/img/2024-08-26-HowtoCallAPIsinSwift_0.png
tag: Tech
originalTitle: "How to Call APIs in Swift"
link: "https://medium.com/@sachinsiwal/how-to-call-apis-in-swift-1885fa48402d"
isUpdated: false
---


여기서 자세한 기사를 읽으실 수 있습니다.

맨 처음부터 시작해보죠. 여기서 무슨 이야기를 하는지 명확하게 이해하실 수 있도록 말이에요. Swift에서 네트워킹은 iOS 애플리케이션과 원격 서버 또는 인터넷을 통해 다른 디바이스 간에 데이터를 송수신하는 프로세스를 말합니다.

![이미지](/assets/img/2024-08-26-HowtoCallAPIsinSwift_0.png)

# 앱 개발에서 네트워킹이 중요한 이유는 무엇인가요?

<div class="content-ad"></div>

휴대 애플리케이션에서 왜 네트워킹이 중요한가요? 간단히 말하면 동적 콘텐츠를 제공하여 사용자가 웹 API를 사용하여 원격 데이터베이스에 저장된 활동을 볼 수 있도록 하기 때문입니다. 이를 통해 사용자가 다른 기기에서 데이터를 가져와 보거나 볼 수 있게 됩니다.

다음 글들을 읽어보시면 도움이 될지도 모릅니다:

뒤단으로 Swift 및 뒤단에 Swift 사용의 최상의 방법

Swift iOS에서 머신러닝 - 단계별 안내

<div class="content-ad"></div>

네트워킹은 다음과 같은 다양한 기능을 지원합니다:

- 실시간 데이터: 네트워킹을 통해 모바일 앱이 외부 웹 서비스, API 또는 클라우드 플랫폼과 상호 작용하여 필요한 데이터를 실시간으로 송수신할 수 있습니다.
- 보안: 네트워킹은 사용자 인증을 지원하여 민감한 데이터가 서버 측 권한 확인을 통해 안전하게 유지되도록 합니다.
- 통신과 메시징: WebSocket 및 기타 실시간 프로토콜은 인스턴트 데이턴 업데이트를 제공하여 인스턴트 메시징 및 채팅과 같은 통신 기능을 가능하게 합니다.
- 버그 수정: 네트워킹을 통해 앱과 서버의 구성을 변경하여 동적 콘텐츠 관련 버그를 신규 빌드 배포 없이 해결할 수 있습니다.
- 크로스 플랫폼 통합: 네트워킹은 서로 다른 플랫폼 및 기기 간의 통신을 용이하게 하여 더욱 원활한 사용자 경험을 제공합니다.
- 데이터 분석: 네트워킹은 서버로 데이터 분석을 송신하는 데 중요한 역할을 합니다.

좋아요! 이제 바로 네트워킹 솔루션을 Swift로 구현하는 방법에 대해 알아봅시다. URLSession부터 시작해봅시다.

# URLSession이란 무엇인가요?

<div class="content-ad"></div>

URLSession은 네트워킹을 지원하는 Swift 프레임워크로, 네트워크 세션을 구성하고 관리하기 위한 클래스와 메서드 세트를 제공합니다.

이를 통해 앱은 서버로부터 데이터를 가져오고 미디어 및 콘텐츠 파일을 업로드하고 다운로드하며 WebSocket 통신을 처리할 수 있습니다.

# 구성 요소 및 구현

URLSession에는 고려해야 할 세 가지 구성 요소가 있습니다:

<div class="content-ad"></div>

- URLSessionDataTask — 기본 HTTP GET 요청을 수행하고 데이터를 가져오는 데 사용합니다.
- URLSessionUploadTask — 데이터를 서버에 업로드하는 데 사용합니다.
- URLSessionDownloadTask — 파일 및 데이터를 백그라운드에서 다운로드하는 데 사용합니다.

요약하면 이렇습니다. 이제 이러한 구성 요소를 사용하여 앱에 기능을 구축하는 방법을 살펴보겠습니다.

# URLSessionDataTask를 사용하여 데이터 가져오기

URLSessionDataTask는 주로 URL에서 텍스트 데이터(예: 게시물 제목 및 설명)를 가져오기 위해 간단한 GET 요청을 수행하는 데 사용되는 URLSession 프레임워크입니다.

<div class="content-ad"></div>

한 예제로 어떻게 작동하는지 확인해 봅시다:

```js
import Foundation
```

```js
// 요청할 URL을 정의합니다
let apiUrlStr = "<https://bugfender.request.url>"
// 문자열에서 URL 개체 생성
if let apiUrl = URL(string: apiUrlStr) {
    
    // URLSession 인스턴스 생성
    let session = URLSession.shared
    
    // URLSessionDataTask를 사용하여 데이터 작업 생성
    let dataTask = session.dataTask(with: apiUrl) { (data, response, error) in
        // 응답 처리
        
        // 오류 확인
        if let error = error {
            print("오류: \(error)")
            return
        }
        
        // 데이터 확인
        guard let responseData = data else {
            print("받은 데이터 없음")
            return
        }
        
        // 받은 데이터 처리
        do {
            if let json = try JSONSerialization.jsonObject(with: responseData, options: []) as? [String: Any] {
                print("응답 JSON: \(json)")
                if let title = json["title"] as? String {
                    print("제목: \(title)")
                }
            }
        } catch {
            print("JSON 구문 분석 오류: \(error)")
        }
    }
    
    dataTask.resume()
} else {
    print("유효하지 않은 URL입니다!")
}
```

예제가 하는 일을 살펴보겠습니다:

<div class="content-ad"></div>

- URLSessionDataTask의 인스턴스는 URL을 제공하여 초기화되며, 그런 다음 기능의 완료를 실행하기 위한 클로저가 있습니다.
- 이제 URLSessionDataTask가 있으므로 resume() 메서드를 사용하여 네트워크 요청을 시작할 수 있습니다.
- 완료 핸들러는 데이터 작업이 실행을 완료했을 때 호출되며, 수신된 데이터, URL 응답 및 서버로부터 요청 중 발생할 수 있는 모든 오류가 포함됩니다.

예제에서 보듯이, 우리는 URLSession.shared 싱글톤을 사용하고 있습니다. 이 공유 세션은 특별한 구성이 필요하지 않는 간단한 작업에 이상적입니다.

# URLSessionUploadTask를 사용하여 데이터 업로드하기

따라서 데이터를 가져올 수 있으므로 이제 URLSessionUploadTask를 사용하여 지정된 URL로 데이터를 업로드하는 방법을 살펴봅시다.

<div class="content-ad"></div>

```swift
import Foundation
```

```swift
// 서버 엔드포인트의 URL 정의
let apiUrlString = "<https://bugfender.request.url>"
// String에서 URL 객체 생성
if let apiUrl = URL(string: apiUrlString) {
    
    // URLSession 인스턴스 생성
    let session = URLSession.shared
    
    // 업로드할 데이터 정의
    let jsonPayload = ["key": "value"]
    
    // JSON payload를 Data로 변환
    do {
        let jsonData = try JSONSerialization.data(withJSONObject: jsonPayload, options: [])
        
        // URL을 사용하여 URLRequest 생성 및 HTTP 메서드를 POST로 설정
        var request = URLRequest(url: apiUrl)
        request.httpMethod = "POST"
        request.httpBody = jsonData
        
        // URLSessionUploadTask를 사용하여 업로드 작업 생성
        let uploadTask = session.uploadTask(with: request, from: jsonData) { (data, response, error) in
            // 여기서 응답 처리
            
            // 서버로부터 받은 오류 확인
            if let error = error {
                print("Error: \(error)")
                return
            }
            
            // 데이터 확인
            if let responseData = data {
                // 필요한 대로 응답 데이터 처리
                let responseString = String(data: responseData, encoding: .utf8)
                print("Response: \(responseString ?? "No response data")")
            }
        }
        
        // 요청을 시작하려면 업로드 작업을 재개
        uploadTask.resume()
        
    } catch {
        print("JSON 데이터 오류: \(error)")
    }
    
} else {
    print("유효하지 않은 URL입니다")
}
```

이 예제에서는 간단한 JSON payload를 원격 서버에 업로드합니다.

서버가 특정 엔드포인트에서 JSON payload를 처리할 수 있어야 함을 유의해야 합니다.


<div class="content-ad"></div>

# URLSessionDownloadTask을 사용하여 파일 다운로드하기

파일을 업로드한 후 다운로드하는 일이 왔습니다. URLSessionDownloadTask를 사용하여 지정된 URL에서 파일을 다운로드할 수 있습니다. 아래와 같이 코드를 작성해보세요.

```swift
import Foundation
```

```swift
// 다운로드할 파일의 URL을 정의합니다
let fileUrlString = "<https://bugfender.request.url>"
// 문자열에서 URL 객체를 생성합니다
if let fileUrl = URL(string: fileUrlString) {
    
    // URLSession 인스턴스를 생성합니다
    let session = URLSession.shared
    
    // URLSessionDownloadTask를 사용하여 다운로드 작업을 만듭니다
    let downloadTask = session.downloadTask(with: fileUrl) { (temporaryUrl, response, error) in
        // 응답을 처리합니다
        
        // 오류를 확인합니다
        if let error = error {
            print("오류: \\(error)")
            return
        }
        
        // 임시 파일 URL이 있는지 확인합니다
        if let tempUrl = temporaryUrl {
            do {
                // 다운로드된 파일을 저장할 대상 URL을 생성합니다
                let documentsDirectory = FileManager.default.urls(for: .documentDirectory, in: .userDomainMask).first!
                let destinationUrl = documentsDirectory.appendingPathComponent("downloadedFile.pdf")
                
                // 임시 파일을 대상 URL로 이동합니다
                try FileManager.default.moveItem(at: tempUrl, to: destinationUrl)
                
                print("파일이 성공적으로 다운로드되었습니다. 저장 위치: \\(destinationUrl)")
            } catch {
                print("파일 이동 오류: \\(error)")
            }
        }
    }
    
    // 요청을 시작하려면 다운로드 작업을 재개합니다
    downloadTask.resume()
    
} else {
    print("유효하지 않은 URL")
}
```

<div class="content-ad"></div>

이 예제에서는 URLSessionDownloadTask를 사용하여 파일을 다운로드하고 로컬 디렉토리에 저장하는 방법을 보여드렸어요.

지금까지 잘 따라 왔나요? 좋아요!

이제 조금 더 복잡한 내용을 살펴봅시다.

# 델리게이트 및 클로저를 사용해 URLSession 작업을 처리하는 방법 설정하기

<div class="content-ad"></div>

URLSession을 사용하여 네트워킹에서 비동기 요청과 응답을 관리하는 두 가지 방법 중 하나인 delegate와 closure를 사용할 수 있습니다.

각각은 다른 방식으로 설정되어 있습니다. 먼저 delegate 접근 방식을 살펴보겠습니다. 이를 위해 다음을 수행해야 합니다:

- URLSessionDelegate 프로토콜을 준수하기
- 필수 함수 구현
- delegate를 사용하여 세션 생성
- 해당 세션에 작업 연결

```swift
import Foundation
```

<div class="content-ad"></div>

```swift
class NetworkDelegateClass: NSObject, URLSessionDelegate, URLSessionDataDelegate {
    
    // URLSessionDataDelegate method to handle response data
    func urlSession(_ session: URLSession, dataTask: URLSessionDataTask, didReceive data: Data) {
        // Process the received data
        print("Received data: \(data)")
    }
    
    // URLSessionDataDelegate method to handle completion
    func urlSession(_ session: URLSession, task: URLSessionTask, didCompleteWithError error: Error?) {
        if let error = error {
            // Handle error
            print("Task completed with error: \(error)")
        } else {
            // Task completed successfully
            print("Task completed successfully")
        }
    }
}
// Example of using delegates
let delegateClass = NetworkDelegateClass()
let delegateSession = URLSession(configuration: .default, delegate: delegateClass, delegateQueue: nil)
if let url = URL(string: "<https://bugfender.sample.request/url>") {
    let delegateTask = delegateSession.dataTask(with: url)
    delegateTask.resume()
}
```

반대로, 응답을 처리하는 데 클로저를 사용하는 것이 더 간편합니다.

이를 위해 아래처럼 작업을 생성할 때 클로저를 직접 제공해야합니다:

```swift
import Foundation
```

<div class="content-ad"></div>

```swift
// 클로저 사용 예제
if let url = URL(string: "https://bugfender.sample.request/url") {
    let closureSession = URLSession.shared
    
    let closureTask = closureSession.dataTask(with: url) { (data, response, error) in
        // 응답 처리
        
        // 에러 확인
        if let error = error {
            print("에러: \(error)")
            return
        }
        
        // 데이터가 있는지 확인
        guard let responseData = data else {
            print("수신된 데이터 없음")
            return
        }
        
        // 수신된 데이터 처리
        print("수신된 데이터: \(responseData)")
    }
    
    // 요청 시작을 위해 테스크 재개
    closureTask.resume()
}
```

URLSession이라는 것에 대해 알아봤으니 이제 URLRequest에 대해 알아볼 차례입니다.

## URLRequest란?

URLRequest는 Swift에서 네트워크 요청을 생성하는 데 사용되며 요청의 세부 정보(URL, HTTP 메소드 [GET, POST 등], 요청 헤더 및 기타 매개변수 등)를 포함하므로 서버에 실행되기 전에 네트워크 요청을 설정하고 사용자 정의할 수 있습니다. 


<div class="content-ad"></div>

URLRequest는 Swift에서 네트워킹 작업을 위해 URLSession과 함께 작동하며, 요청을 특정 요구 사항에 맞게 구성할 수 있고 공유 URLSession보다 더 복잡한 시나리오를 지원합니다.

그럼 이제 예제를 통해 URL, HTTP 메서드, 헤더 및 캐시 정책을 설정하는 방법을 살펴보겠습니다.

# URL 설정

URL은 네트워크 요청의 핵심 구성 요소입니다. 요청을 보내는 위치로서 기능합니다.

<div class="content-ad"></div>

아래 예제에서는 URLRequest 객체를 생성할 때 URL을 설정하는 방법을 보여줍니다:

```js
import Foundation
```

```js
// URLRequest에 URL 설정 예제
if let url = URL(string: "https://api.bugfender.com/data") {
    var request = URLRequest(url: url)
    // 이 요청에 대해 추가 구성 및 매개변수를 설정할 수 있습니다
}
```

# HTTP 메소드 설정

<div class="content-ad"></div>

동등하게 중요한 것은 HTTP 메서드가 특정 네트워크 요청에 의해 수행될 작업 유형을 정의한다는 것입니다.

흔한 메서드로는 GET, POST, PUT 및 DELETE가 있으며 URLRequest를 만들 때 지정해야 합니다. 예를 들어:

```js
// URLRequest에서 HTTP 메서드를 지정하는 예시
request.httpMethod = "POST"
```

# 헤더 추가

<div class="content-ad"></div>

헤더는 서버에 대한 요청에 대한 추가 정보(예: 인증 토큰, 콘텐츠 유형 및 기타 사용자 지정 메타데이터)를 제공하는 데 사용될 수 있습니다.

URLRequest에 헤더를 추가하는 방법은 addValue(_:forHTTPHeaderField:)를 사용하는 것입니다.

```js
// URLRequest에 헤더 추가 예시
request.addValue("application/json", forHTTPHeaderField: "Content-Type")
request.addValue("Bearer YourAccessToken", forHTTPHeaderField: "Authorization")
```

# 캐시 정책 구성

<div class="content-ad"></div>

캐시 정책은 요청이 캐시된 데이터를 사용해야 하는지 여부와 캐싱을 처리하는 방법을 설정합니다.

URLRequest 객체를 만들 때 캐시 정책을 구성할 수 있습니다. 예를 들어, 다음과 같이 할 수 있습니다:

```js
// URLRequest에서 캐시 정책을 구성하는 예제
request.cachePolicy = .reloadIgnoringLocalCacheData
```

# 매개변수 인코딩

<div class="content-ad"></div>

POST 요청을 위한 쿼리 매개변수 및 데이터를 URLRequest에 인코딩하여 추가할 수 있습니다. 다음과 같이 수행됩니다:

```js
// URLRequest에서 매개변수 인코딩 예시
let parameters: [String: Any] = ["key1": "value1", "key2": "value2"]
if let jsonData = try? JSONSerialization.data(withJSONObject: parameters) {
    request.httpBody = jsonData
}
```

이러한 매개변수는 JSON으로 인코딩되어 요청의 HTTP 본문으로 설정되며, 매개변수 인코딩은 API의 특정 요구 사항에 따라 다양한 방법(URL 또는 JSON 등)으로 사용할 수 있습니다.

지금까지 잘 진행되었습니다. 이제 URLSession 및 URLRequest를 구성했으므로, 두 가지를 함께 사용할 시간입니다.

<div class="content-ad"></div>

# URLRequest를 사용하여 요청 실행하기

URLRequest를 URLSession과 함께 사용하여 네트워크 요청을 실행하려면 이 예시에 표시된 대로 해당 요청을 데이터 작업 메서드에 전달해야 합니다:

```js
import Foundation
```

```js
// URLRequest에 URL 설정하는 예시
if let url = URL(string: "https://api.bugfender.com/data") {
    var request = URLRequest(url: url)
    
    // 요청의 HTTP 메소드를 GET으로 설정합니다. 이는 실제로 설정되지 않으면 기본값입니다. 여기서는 명확하게 포함했습니다.
    request.httpMethod = "GET"
    
    // 여기에 헤더를 추가하세요. 예를 들어:
    request.setValue("application/json", forHTTPHeaderField: "Content-Type")
    // API에 대한 실제 액세스 토큰으로 `YourAccessToken`을 교체합니다.
    request.setValue("Bearer YourAccessToken", forHTTPHeaderField: "Authorization")
    
    // URLSession 구성
    let config = URLSessionConfiguration.default
    let session = URLSession(configuration: config)
    
    // 데이터를 가져오기 위한 작업 생성
    let task = session.dataTask(with: request) { (data, response, error) in
      // 데이터 처리
      //...
    }
    
    // 작업 시작
    task.resume()
}
```

<div class="content-ad"></div>

초기 설정만큼 중요한 것은 에러 처리입니다. 다음으로 에러 처리를 살펴보겠습니다.

# 네트워킹에서의 에러 처리

그래서 네트워킹에서 가장 흔한 에러는 무엇일까요?

- 인터넷 연결 없음: 장치가 인터넷에 연결되지 않은 경우 서버 요청을 할 수 없습니다.
- 시간 초과: 서버가 요청에 너무 오랜 시간이 걸려 설정된 시간 초과 간격을 초과할 때 발생합니다.
- 서버 측 에러: 요청된 리소스를 찾을 수 없거나 서버가 과부하로 인해 다운된 경우와 같은 문제는 HTTP 응답 코드와 함께 제공됩니다.
- 잘못된 URL: 제공된 URL이 잘못되었거나 유효한 URL 객체로 변환할 수 없는 경우.
- SSL 인증서 문제: SSL 핸드셰이크 문제나 인증서가 유효하지 않은 경우 발생합니다.
- 데이터 구문 분석 에러: 데이터가 형식 불일치로 인해 올바르게 구문 분석되지 않을 때 발생합니다.

<div class="content-ad"></div>

# URLSessionDelegate를 사용하여 에러 처리하기

이러한 에러를 처리하기 위해 URLSessionDelegate 메소드와 URLSessionTaskDelegate 프로토콜을 사용할 수 있습니다. 아래와 같이 사용할 수 있어요:

```js
class MyDelegateClass: NSObject, URLSessionDelegate, URLSessionTaskDelegate {
    // URLSessionTaskDelegate 메소드를 사용하여 에러를 처리합니다
    func urlSession(_ session: URLSession, task: URLSessionTask, didCompleteWithError error: Error?) {
        if let error = error {
            // 에러 처리
            print("에러가 발생하여 작업이 완료되었습니다: \\(error)")
        } else {
            // 작업이 성공적으로 완료됨
            print("작업이 성공적으로 완료되었습니다")
        }
    }
}
```

# Result 유형을 사용한 에러 처리

<div class="content-ad"></div>

스위프트 5부터는 오류 처리를 더 효과적으로 다루는 방법으로 Result 타입을 사용할 수 있습니다. 이 방법은 특히 클로저와 함께 작업할 때 매우 유용합니다. 아래에서 볼 수 있듯이:

```js
import Foundation
```

```js
enum NetworkError: Error {
    case noInternet
    case timeout
    case serverError
    // ... 다른 특정 오류 케이스
}
func fetchData(completion: @escaping (Result<Data, NetworkError>) -> Void) {
    guard hasInternetConnection() else {
        completion(.failure(.noInternet))
        return
    }
    // URLSession 작업 수행
    let task = URLSession.shared.dataTask(with: url) { (data, response, error) in
        if let error = error {
            completion(.failure(.serverError))
            return
        }
        guard let responseData = data else {
            completion(.failure(.serverError))
            return
        }
        completion(.success(responseData))
    }
    task.resume()
}
// 사용법
fetchData { result in
    switch result {
    case .success(let data):
        // 성공 처리
        print("받은 데이터: \(data)")
    case .failure(let error):
       // 실패 처리
        print("오류: \(error)")
    }
}
```

이제 우리는 어떻게 오류를 처리하는지 알았으니, 좀 더 복잡한 작업을 살펴보겠습니다.

<div class="content-ad"></div>

# 비동기 URLSession 작업

비동기 프로그래밍은 작업들이 동시에 실행되어 시스템이 작업이 완료될 때까지 대기하는 동안 다른 작업을 수행할 수 있도록 하는 것입니다.

데이터 작업, 다운로드 작업, 업로드 작업과 같은 URLSession 작업은 기본적으로 비동기적으로 동작하며, 네트워크 요청이 주 스레드를 차단하지 않고 데이터를 가져오거나 업로드/다운로드 작업 중일 때 UI가 멈추지 않도록 합니다.

# 클로저 기반 완료 핸들러 사용하기

<div class="content-ad"></div>

URLSession 작업은 비동기 응답을 관리하기 위해 클로저 기반의 완료 핸들러를 사용합니다.

클로저는 주로 작업이 완료될 때 실행되며, 데이터, 응답 및 오류에 액세스할 수 있도록 제공되며 다음과 같이 설정합니다:

```swift
let url = URL(string: "https://api.bugfender.com/data")!
```

```swift
let task = URLSession.shared.dataTask(with: url) { (data, response, error) in
    // 비동기 응답 처리
    if let error = error {
        print("Error: \(error)")
        return
    }
    if let responseData = data {
        // 수신된 데이터 처리
        print("Received data: \(responseData)")
    }
}
// 요청을 시작하려면 작업을 재개하십시오
task.resume()
```

<div class="content-ad"></div>

# 배경 작업 관리를 위해 디스패치 큐 사용하기

디스패치 큐를 사용하면 사용자는 병렬 작업을 관리하고 다른 스레드에서 코드의 전체 실행을 제어할 수 있습니다.

시간이 오래 걸리는 작업을 처리할 때 메인 스레드에 영향을 미치지 않고 백그라운드 큐를 사용하는 것이 중요합니다.

아래에서는 백그라운드 큐에서 네트워크 요청을 실행하고, 메인 큐에서 UI를 업데이트했습니다:

<div class="content-ad"></div>

```swift
let url = URL(string: "https://api.bugfender.com/data")!
```

```swift
DispatchQueue.global(qos: .background).async {
    // 네트워크 요청을 백그라운드 큐에서 수행합니다.
    let data = try? Data(contentsOf: url)
    DispatchQueue.main.async {
        // 메인 큐에서 UI를 업데이트합니다.
        if let responseData = data {
            print("받은 데이터: \(responseData)")
        }
    }
}
```

여기까지입니다!

# URLSession 사용 최적 사례 방법

<div class="content-ad"></div>

마지막으로, 개발자가 URLSession을 구성하고 사용할 때 주의해야 할 모베스트 프랙티스 예제를 살펴보겠습니다.

# 세션 및 작업 관리

- 세션 구성: 요구 사항에 맞는 URLSessionConfiguration을 선택하는 것이 중요합니다 (예: 백그라운드 구성을 사용하여 백그라운드에서 계속 실행되는 작업에 대한 구성).
- 세션 재사용: 동일 유형의 작업에 대해 URLSession 개체를 재사용하여 연결을 재사용하여 리소스 사용을 최적화하십시오.
- 작업 우선순위: 작업 우선순위(높음, 기본, 낮음)를 활용하여 세션에서 작업 실행 순서를 관리합니다.

```js
let highPriorityTask = session.dataTask(with: highPriorityRequest)
highPriorityTask.priority = URLSessionTask.highPriority
highPriorityTask.resume()
```

<div class="content-ad"></div>

- 작업 취소 및 무효화: 더 이상 필요하지 않은 작업이 있다면 자원을 확보하기 위해 작업을 취소하고, 더 이상 필요하지 않은 경우 세션을 무효화하십시오.

```js
task.cancel()
session.invalidateAndCancel()
```

# 백그라운드 작업 처리

- 백그라운드 구성: 앱이 비활성 상태여도 계속 실행되어야 하는 작업에 대해 백그라운드 세션 구성을 사용하십시오.

<div class="content-ad"></div>

```swift
let backgroundConfig = URLSessionConfiguration.background(withIdentifier: "com.bugfender.backgroundSession")
let backgroundSession = URLSession(configuration: backgroundConfig, delegate: delegate, delegateQueue: nil)
```

- 백그라운드 완료 핸들러: 백그라운드에서 발생하는 이벤트를 처리하는 백그라운드 완료 핸들러를 구현하는 것을 잊지 마세요.

```swift
func application(_ application: UIApplication, handleEventsForBackgroundURLSession identifier: String, completionHandler: @escaping () -> Void) {
    // 나중에 사용하기 위해 완료 핸들러 저장
    backgroundCompletionHandler = completionHandler
}
```

- 백그라운드 세션 이벤트 처리: 백그라운드 세션 이벤트를 처리하기 위해 적절한 URLSessionDelegate 메서드를 구현하는 것을 기억해 주세요.


<div class="content-ad"></div>

```swift
func urlSessionDidFinishEvents(forBackgroundURLSession session: URLSession) {
    if let completionHandler = backgroundCompletionHandler {
        backgroundCompletionHandler = nil
        completionHandler()
    }
}
```

# 네트워크 활동 모니터링

- 활동 인디케이터: 네트워크 활동 인디케이터를 사용하여 사용자에게 계속되는 네트워크 요청에 대해 알리고, 애플리케이션이 서버로부터의 응답을 기다리고 있다는 것을 알 수 있도록 합니다.

```swift
UIApplication.shared.isNetworkActivityIndicatorVisible = true // 인디케이터 표시
// 네트워크 요청 수행
UIApplication.shared.isNetworkActivityIndicatorVisible = false // 인디케이터 숨김
```

<div class="content-ad"></div>

- 여러 요청을 결합: 가능한 경우 여러 네트워크 요청을 하나의 논리적 작업으로 결합하여 네트워크 활동 인디케이터의 반복을 줄입니다.
- 완료 핸들러: 완료 핸들러에서 데이터를 받으면 네트워크 활동 인디케이터를 끄는 것이 좋은 아이디어입니다.

```js
UIApplication.shared.isNetworkActivityIndicatorVisible = true // 인디케이터 표시
session.dataTask(with: request) { (data, response, error) in
    // 응답 처리
```

```js
    DispatchQueue.main.async {
        UIApplication.shared.isNetworkActivityIndicatorVisible = false // 인디케이터 숨김
    }
}.resume()
```

지금은 여기까지입니다.

<div class="content-ad"></div>

# 요약하자면

- URLSession을 사용하여 네트워킹 작업을 수행하는 것은 주요 개념에 대한 좋은 이해를 요구하며, 최적의 관행을 채택하면 네트워킹에 대해 신뢰할 수 있고 유지보수 가능한 코드를 보장할 수 있습니다.
- URLRequest는 URL 요청, HTTP 메서드, 헤더 및 캐시 정책을 나타내는 Swift 구조체입니다.
- URLRequest의 구성은 올바른 URL 설정, HTTP 메서드 추가, 헤더 추가 및 네트워크 요청의 캐시 정책 구성이 포함됩니다.
- URLSession은 네트워크 요청을 만들기 위한 Swift API입니다.
- 세션과 작업 관리를 위한 최상의 방법에는 세션 재사용, 작업 우선순위 설정, 필요하지 않은 작업 취소가 포함됩니다.
- URLSession 작업은 기본적으로 비동기이며, iOS 앱에서 반응성을 보장합니다.
- 비동기 작업 처리를 위해 클로저를 사용하며, Swift 5(이상)의 Result 타입은 오류 처리를 강화합니다.

최상의 관행을 준수하고 핵심 기본 개념을 이해하면 개발자들은 URLSession을 사용하여 견고하고 확장 가능한 네트워킹 코드를 작성할 수 있습니다. 무엇이든 그렇게 하듯, 이 가이드를 활용하여 실험하고 더 잘 이해할 수 있도록 도움받으세요.