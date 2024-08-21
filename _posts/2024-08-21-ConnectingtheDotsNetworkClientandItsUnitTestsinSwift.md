---
title: "네트워크 클라이언트와 Swift에서의 유닛 테스트 방법 정리"
description: ""
coverImage: "/assets/img/2024-08-21-ConnectingtheDotsNetworkClientandItsUnitTestsinSwift_0.png"
date: 2024-08-21 18:22
ogImage: 
  url: /assets/img/2024-08-21-ConnectingtheDotsNetworkClientandItsUnitTestsinSwift_0.png
tag: Tech
originalTitle: "Connecting the Dots Network Client and Its Unit Tests in Swift"
link: "https://medium.com/@yossansr/connecting-the-dots-network-client-and-its-unit-tests-in-swift-6474c73ab61a"
isUpdated: true
updatedAt: 1724245994612
---


<img src="/assets/img/2024-08-21-ConnectingtheDotsNetworkClientandItsUnitTestsinSwift_0.png" />

어플리케이션이 원격 소스에서 데이터를 필요로 할 때, 네트워킹은 중요해집니다. 다행히도 URLSession은 네트워크 구현을 우리에게 훨씬 쉽게 만들어줍니다. 하지만 더 간단하게 만들고 더 구조화된 방식으로 가기 위해, NetworkClient 클래스를 생성할 것입니다.

# 개요

이 글에서는 async/await 메커니즘을 활용하는 URLSession.data(for:)를 사용할 것입니다. 이 함수에는 기본 캐싱 및 취소 메커니즘이 포함되어 있습니다. 이를 염두에 두고, 우리의 네트워크 클라이언트는 다음에 초점을 맞출 것입니다:

<div class="content-ad"></div>

- URLComponents 및 URLRequest을 생성합니다.
- 재시도 메커니즘을 사용하여 요청을 처리합니다.
- HTTP 응답 상태 코드를 관리합니다.
- 검색한 데이터를 우리 모델로 디코딩합니다.

마지막으로 캐싱 메커니즘을 포함한 모든 기능에 대한 단위 테스트를 작성할 것입니다. 단위 테스트를 위해 코드를 쉽게 모의할 수 있도록 프로토콜 지향 프로그래밍을 사용할 것입니다.

## 프로토콜 및 열거형 정의

네트워크 클라이언트를 구축하기 전에, 네트워크 클라이언트 클래스에서 사용될 프로토콜과 열거형을 정의해 봅시다.

<div class="content-ad"></div>

## NetworkClientProtocol

우리의 프로토콜을 NetworkClientProtocol로 정의해봅시다. 이 프로토콜은 request 함수를 가지고 있습니다. 이 함수는 Decodable 프로토콜을 준수하는 일반적인 타입을 가지고 Endpoint 매개변수를 취합니다. 우리는 전통적인 완료 패턴 대신에 async/await를 사용할 것입니다.

```js
protocol NetworkClientProtocol {
    func request<T: Decodable>(target: Endpoint) async throws -> T
}
```

## Endpoint

<div class="content-ad"></div>

Endpoint은 URLComponents와 URLRequest를 생성하는 데 필요한 속성을 정의하는 프로토콜입니다. 또한 구현자가 기본값을 할당할 수 있는 속성을 설정하여 구현자가 편리하게 사용할 수 있도록 해줍니다. 아래는 기본값이 지정된 Endpoint입니다:

```js
protocol Endpoint {
    // For URLComponents
    var scheme: String { get }
    var host: String { get }
    var path: String { get }
    var queryItems: [URLQueryItem]? { get }
    
    // For URLRequest
    var cachePolicy: URLRequest.CachePolicy { get }
    var timeoutInterval: TimeInterval { get }
    var method: HTTPMethod { get }
    var header: [String: String]? { get }
    
    // For retry mechanism
    // Indicates the maximum number of retries allowed
    var retries: Int { get }
}

extension Endpoint {
    var scheme: String {
        "https"
    }
    var queryItems: [URLQueryItem]? {
        nil
    }
    var cachePolicy: URLRequest.CachePolicy {
        .useProtocolCachePolicy
    }
    var timeoutInterval: TimeInterval {
        10
    }
    var header: [String: String]? {
        ["Content-Type": "application/json"]
    }
    
    var retries: Int {
        3
    }
}

enum HTTPMethod: String {
    case get = "GET"
    case post = "POST"
    case put = "PUT"
    case patch = "PATCH"
    case delete = "DELETE"
}
```

## NetworkError

저희의 요청 함수는 throwable이며, 오류를 분류하기 위해 사용자 정의 오류 유형이 필요합니다.

<div class="content-ad"></div>

```js
열거형 NetworkError: Error {
    case badURL
    case invalidResponse
    case notFound
    case internalServerError
    case unsupportedContentType
    case requestFailed(Error)
    case decodingFailed(Error)
    case unknownError(statusCode: Int)
}
```

# NetworkClient Class 만들기

NetworkClientProtocol을 준수하는 NetworkClient 클래스를 만들겠습니다. 이 클래스는 URLSession 속성을 가지고 있는데, URLSessionConfiguration으로 초기화하여 구성할 수 있습니다.

```js
최종 클래스 NetworkClient: NetworkClientProtocol {
    private let urlSession: URLSession
    
    초기화(urlSessionConfiguration: URLSessionConfiguration = URLSessionConfiguration.default) {
        self.urlSession = URLSession(configuration: urlSessionConfiguration)
    }

    func request<T: Decodable>(target: Endpoint) async throws -> T {}
}
```

<div class="content-ad"></div>

## URLComponents 및 URLRequest 생성

먼저, 요청 함수를 만들기 위해 URLComponents 및 URLRequest의 생성 함수를 작성하는 것으로 시작하겠습니다.

```js
private func createUrlRequest(target: Endpoint) throws -> URLRequest {
    var components = URLComponents()
    components.scheme = target.scheme
    components.host = target.host
    components.path = target.path
    components.queryItems = target.queryItems
    
    guard let url = components.url else {
        throw NetworkError.badURL
    }
    
    var urlRequest = URLRequest(
        url: url,
        cachePolicy: target.cachePolicy,
        timeoutInterval: target.timeoutInterval
    )
    urlRequest.httpMethod = target.method.rawValue
    urlRequest.allHTTPHeaderFields = target.header
    return urlRequest
}
```

Endpoint의 속성을 사용하여 URLComponents 및 URLRequest의 속성을 할당할 것입니다.

<div class="content-ad"></div>

## 요청 처리

URLRequest를 획득한 후, 요청을 실행할 수 있습니다. 이 handle request 함수에서는 URLSession을 사용하여 재시도 메커니즘을 통해 요청을 수행합니다.

```swift
private func handleRequest(urlRequest: URLRequest, retries: Int = 3) async throws -> (Data, URLResponse) {
    var attempts = 0
    while attempts < retries {
        do {
            return try await urlSession.data(for: urlRequest)
        } catch {
            attempts += 1
            if attempts >= retries {
                throw NetworkError.requestFailed(error)
            }
            
            try await Task.sleep(nanoseconds: 2_000_000_000)
        }
    }
    
    // 루프 때문에 여기에 도달하지는 않겠지만, 컴파일러에서 필요합니다.
    throw NetworkError.requestFailed(NSError(domain: "", code: 0))
}
```

재시도 간에 2초씩 대기하는 것을 주목하세요. 다음 시도를 하기 전에 잠시 기다리는 것은 일반적인 관행입니다.

<div class="content-ad"></div>

## HTTP 프로토콜의 상태 코드 다루기

HTTP 상태 코드는 요청의 상태를 나타내는데, 성공을 나타내는 200–299, 클라이언트 요청이 올바르지 않은 경우를 나타내는 400–499, 서버 측 문제를 나타내는 500–599 등이 있습니다. 이러한 상태 코드를 기반으로 오류를 throw하여 디버깅이 쉽거나 사용자에게 적절한 오류 메시지와 복구 제안을 제공할 수 있습니다. 이 예제에서는 이러한 상태 코드를 처리해 보겠습니다.

```js
private func handleStatusCode(response: URLResponse) throws {
    guard let httpResponse = response as? HTTPURLResponse else {
        throw NetworkError.invalidResponse
    }

    switch httpResponse.statusCode {
    case 200...299:
        return
    case 404:
        throw NetworkError.notFound
    case 500:
        throw NetworkError.internalServerError
    default:
        throw NetworkError.unknownError(statusCode: httpResponse.statusCode)
    }
}
``` 

## 검색된 데이터 디코딩하기

<div class="content-ad"></div>

데이터를 받은 후에는 Swift에서 쉽게 작업할 수 있도록 데이터 모델로 디코딩해야 합니다. 이 예제에서는 응답 콘텐츠 유형이 JSON이라고 가정합니다.

```swift
private func decode<T: Decodable>(data: Data, contentType: String?) throws -> T {
    guard let safeContentType = contentType, safeContentType.contains("application/json") else {
        throw NetworkError.unsupportedContentType
    }
    
    do {
        let decodedData = try JSONDecoder().decode(T.self, from: data)
        return decodedData
    } catch let decodingError {
        throw NetworkError.decodingFailed(decodingError)
    }
}
```

## 요청 함수 완성

요청 함수에 필요한 함수들은 이미 만들어졌으므로 간단히 그 안에서 호출하면 됩니다.

<div class="content-ad"></div>

```swift
final class NetworkClient: NetworkClientProtocol {
    
    // 코드의 나머지 부분

    func request<T: Decodable>(target: Endpoint) async throws -> T {
        let urlRequest = try createUrlRequest(target: target)
        let (data, urlResponse) = try await handleRequest(urlRequest: urlRequest, retries: target.retries)
        try handleStatusCode(response: urlResponse)
        let decodedData: T = try decode(data: data, contentType: urlResponse.mimeType)
        
        return decodedData
    }

    // 코드의 나머지 부분
}
```

## URLCache 설정

URLSessionConfiguration.urlCache는 기본적으로 URLCache.shared를 사용합니다. AppDelegate.swift에서 캐시의 디스크와 메모리 할당량을 정의하기 위해 캐시 구성을 설정할 수 있습니다.

```swift
class AppDelegate: UIResponder, UIApplicationDelegate {
    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        // 애플리케이션 실행 후 사용자 정의 지점
        let memoryCapacity = 4 * 1024 * 1024 // 4 MB
        let diskCapacity = 20 * 1024 * 1024 // 20 MB
        let cache = URLCache(memoryCapacity: memoryCapacity, diskCapacity: diskCapacity)
        URLCache.shared = cache
        
        return true
    }
}
```

<div class="content-ad"></div>

## NetworkClient 사용 예시

간단한 예시에서는 서비스와 해당 엔드포인트를 만들어 NetworkClient를 구현하는 한 가지 방법을 보여드릴게요.

```js
// Endpoint
enum ExampleEndpoint: Endpoint {
    case getCommentsWith(postId: String)
    
    var host: String {
        "jsonplaceholder.typicode.com"
    }
    
    var path: String {
        switch self {
        case .getCommentsWith: "/comments"
        }
    }
    
    var method: HTTPMethod {
        switch self {
        case .getCommentsWith: .get
        }
    }

    var queryItems: [URLQueryItem]? {
        switch self {
        case .getCommentsWith(let postId):
            [
                URLQueryItem(name: "postId", value: postId)
            ]
        }
    }
}

// Service
protocol ExampleServiceProtocol {
    // `Comment` 및 `ExampleError`의 정의는 기사 하단에 첨부된 저장소에서 볼 수 있어요
    func getComments(with postId: String) async -> Result<[Comment], ExampleError>
}

final class ExampleService: ExampleServiceProtocol {
    private let networkClient: NetworkClientProtocol
    
    init(networkClient: NetworkClientProtocol = NetworkClient(urlSessionConfiguration: .default)) {
        self.networkClient = networkClient
    }
    
    func getComments(with postId: String) async -> Result<[Comment], ExampleError> {
        do {
            let comments: [Comment] = try await networkClient.request(target: ExampleEndpoint.getCommentsWith(postId: postId))
            
            return !comments.isEmpty ? .success(comments) : .failure(.empty)
        } catch {
            return .failure(.network)
        }
    }
}

// 서비스 사용
let service: ExampleServiceProtocol = ExampleService()
let postId = "2"
Task {
    let comments: Comment = await service.getComments(with: postId)
    print(comments)
}
```

위 예시의 전체 코드는 기사 하단에 링크된 저장소에서 확인할 수 있어요.

<div class="content-ad"></div>

# NetworkClient의 단위 테스트

NetworkClient가 잘 작동하고 앞으로도 잘 작동할 수 있도록 해당 단위 테스트를 작성할 수 있습니다.

## URLProtocol, Endpoint, 및 Model의 Mock

URLSessionConfiguration에서 프로토콜 클래스를 설정할 수 있습니다. 이 클래스들은 URLSession 기능을 모의(Mock)하는 우리가 정의한 사용자 정의 URLProtocol 객체와 해당 객체들이 대응됩니다. 이제 사용자 정의 URLProtocol을 정의해 봅시다.

<div class="content-ad"></div>

```swift
class MockURLProtocol: URLProtocol {
    static var requestHandler: ((URLRequest) throws -> (Data, URLResponse))?
    
    override class func canInit(with request: URLRequest) -> Bool {
        return true
    }
    
    override class func canonicalRequest(for request: URLRequest) -> URLRequest {
        return request
    }
    
    override func startLoading() {
        guard let handler = MockURLProtocol.requestHandler else {
            fatalError("handler not implemented")
        }
        
        do {
            let (data, response) = try handler(request)
            
            client?.urlProtocol(self, didReceive: response, cacheStoragePolicy: .allowedInMemoryOnly)
            client?.urlProtocol(self, didLoad: data)
            client?.urlProtocolDidFinishLoading(self)
        } catch {
            client?.urlProtocol(self, didFailWithError: error)
        }
    }
    
    override func stopLoading() {}
}
```

테스트 케이스에서 request 함수를 호출하고 싶을 때 구현해야 하는 requestHandler 속성을 주목해주세요. 이 속성을 사용하면 우리는 모의 데이터와 응답을 할당할 수 있게 됩니다.

또한 모의 엔드포인트와 디코더블 모델을 정의하고자 합니다. 간단히 우리 모델에서 키 속성을 사용하겠습니다.

```swift
struct MockValidEndpoint: Endpoint {
    var host: String = "example.com"
    var path: String = "/"
    var method: HTTPMethod = .get
}

struct MockDecodable: Decodable {
    let key: String
}
```

<div class="content-ad"></div>

## 요청 성공 및 요청 실패 테스트하기

먼저 XCTestCase 클래스를 정의할 것입니다.

```js
final class NetworkClientTests: XCTestCase {
    private var networkClient: NetworkClient!
    
    override func setUp() {
        super.setUp()

        // .ephemeral로 설정하면 세션 객체가 디스크가 아닌 RAM/메모리에 저장됩니다
        let config = URLSessionConfiguration.ephemeral 
        config.protocolClasses = [MockURLProtocol.self]
        
        networkClient = NetworkClient(urlSessionConfiguration: config)
    }
    
    override func tearDown() {
        networkClient = nil
        super.tearDown()
    }
}
```

그런 다음 "주어진 경우, 이때, 그러면"의 흐름을 사용하여 테스트 케이스를 작성할 수 있습니다. 아래는 요청 성공 테스트 케이스입니다.

<div class="content-ad"></div>

```js
func test_request_success() async throws {
     // 주어졌을 때
    let expectedData = "{\"key\":\"value\"}".data(using: .utf8)
  
    MockURLProtocol.requestHandler = { request in
        let response = HTTPURLResponse(url: request.url!, statusCode: 200, httpVersion: nil, headerFields: request.allHTTPHeaderFields)
        
        return (expectedData!, response!)
    }
    
    // 실행했을 때
    let result: MockDecodable = try await networkClient.request(target: MockValidEndpoint())
    
    // 그러면
    XCTAssertEqual(result.key, "value")
}
```

실패 테스트의 경우, 성공 테스트와 유사한 프로세스를 따릅니다. 다만, requestHandler에서 에러를 throw합니다.

```js
func test_request_requestFailed() async throws {
    // 주어졌을 때
    let expectedError = NSError(domain: "", code: 0)
    MockURLProtocol.requestHandler = { _ in
        throw expectedError
    }
    
    do {
        // 실행했을 때
        let _: MockDecodable = try await networkClient.request(target: MockValidEndpoint())
        XCTFail("에러가 발생하지 않았습니다")
    } catch {
        // 그러면
        XCTAssertEqual(error as? NetworkError, NetworkError.requestFailed(expectedError))
    }
}
```

## 캐싱 메커니즘 테스트하기

<div class="content-ad"></div>

우리는 URLSession이 URLCache를 캐싱에 사용한다는 것을 알고 있습니다. 그래서 URLSessionConfiguration.urlCache에 할당 될 사용자 정의 URLCache를 만들 것입니다.

```js
final class MockURLCache: URLCache {
    var storedResponses: [URLRequest: CachedURLResponse] = [:]
    var isCacheUsed: Bool = false
    
    override func cachedResponse(for request: URLRequest) -> CachedURLResponse? {
        if let storedResponse = storedResponses[request] {
            isCacheUsed = true
            return storedResponse
        }
        
        return nil
    }
    
    override func storeCachedResponse(_ cachedResponse: CachedURLResponse, for request: URLRequest) {
        storedResponses[request] = cachedResponse
    }
}
```

우리 MockURLCache의 전략은 boolean인 isCacheUsed를 설정하고 나중에 테스트 케이스에서 확인하는 것입니다. cachedResponse 함수가 호출되었을 때 storedResponse가 nil이 아닌 경우에 캐시가 사용되었다는 것을 알 수 있습니다.

이제 URLCache의 모의 객체를 설정하고 해당 테스트 케이스를 테스트 클래스에 설정해 봅시다.

<div class="content-ad"></div>

```swift
final class NetworkClientTests: XCTestCase {
    private var mockCache: MockURLCache!
    private var networkClient: NetworkClient!
    
    override func setUp() {
        super.setUp()
        
        let config = URLSessionConfiguration.ephemeral
        config.protocolClasses = [MockURLProtocol.self]

        // 우리의 사용자 정의 URLCache 구현체로 변경합니다.
        mockCache = MockURLCache()
        config.urlCache = mockCache
        
        networkClient = NetworkClient(urlSessionConfiguration: config)
    }
    
    override func tearDown() {
        mockCache = nil
        networkClient = nil
        super.tearDown()
    }

    func test_request_usesCache() async throws {
        // 주어졌을 때
        let expectedData = "{\"key\":\"cachedValue\"}".data(using: .utf8)
        
        MockURLProtocol.requestHandler = { request in
            let response = HTTPURLResponse(url: request.url!, statusCode: 200, httpVersion: nil, headerFields: request.allHTTPHeaderFields)
            
            return (expectedData!, response!)
        }
        
        // 실행했을 때
        // 첫 번째 요청은 캐시를 사용하지 않음
        let result: MockDecodable = try await networkClient.request(target: MockValidEndpoint())
        
        // 그런 다음
        XCTAssertEqual(result.key, "cachedValue")
        XCTAssertEqual(self.mockCache.storedResponses.count, 1)
        XCTAssertFalse(mockCache.isCacheUsed)
        
        // 실행했을 때
        // 두 번째 요청은 캐시를 사용해야 함
        let cachedResult: MockDecodable = try await networkClient.request(target: MockValidEndpoint())
        
        // 그런 다음
        XCTAssertEqual(cachedResult.key, "cachedValue")
        XCTAssertTrue(mockCache.isCacheUsed)
    }
}
```

## 기타 테스트 케이스

100% 커버리지를 달성하기 위해 우리의 테스트 케이스에서 발생하는 모든 에러를 커버해야 합니다. 나머지 테스트 케이스는 아래 첨부한 저장소에서 확인할 수 있습니다.

<img src="/assets/img/2024-08-21-ConnectingtheDotsNetworkClientandItsUnitTestsinSwift_1.png" />