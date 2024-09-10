---
title: "iOS 애플리케이션 보안을 위한 완벽한 가이드 iOS 키패인을 활용해 민감한 데이터 보호하기"
description: ""
coverImage: "/assets/img/2024-09-10-TheCompleteGuidetoSecuringiOSApplicationsAgainstHackersProtectingSensitiveDatawithiOSKeychain_0.png"
date: 2024-09-10 19:33
ogImage: 
  url: /assets/img/2024-09-10-TheCompleteGuidetoSecuringiOSApplicationsAgainstHackersProtectingSensitiveDatawithiOSKeychain_0.png
tag: Tech
originalTitle: "The Complete Guide to Securing iOS Applications Against Hackers Protecting Sensitive Data with iOS Keychain"
link: "https://medium.com/@lioz.balki1/the-complete-guide-to-securing-ios-applications-against-hackers-protecting-sensitive-data-with-ios-1fb1a73bfdd5"
isUpdated: false
---


<img src="/assets/img/2024-09-10-TheCompleteGuidetoSecuringiOSApplicationsAgainstHackersProtectingSensitiveDatawithiOSKeychain_0.png" />

요즘의 디지털 환경에서는 모바일 애플리케이션이 해커들의 대상이 되어 민감한 정보를 빼내고자 하는 경우가 빈번히 발생합니다. iOS 애플리케이션도 예외는 아니며, Apple 플랫폼이 강력한 보안 기능을 제공하더라도, 개발자들이 추가적인 보호 조치를 취하여 앱을 안전하게 보호하는 것이 중요합니다.

iOS 애플리케이션에서 민감한 데이터를 보호하는 데 가장 효과적인 도구 중 하나는 iOS 키체인입니다. 키체인은 암호화된 안전한 저장소를 제공하여 비밀번호, API 키, 인증 토큰 및 기타 민감한 데이터와 같은 중요 정보를 저장할 수 있습니다. 이러한 정보가 노출되면 심각한 보안 위협으로 이어질 수 있으므로, 해커들은 특히 역공학에 능한 사람들은 이러한 민감한 데이터를 추출하기 위해 보안이 잘 되어 있지 않은 앱을 타겟팅하는 경우가 많습니다. 만약 앱이 안전한 저장 방법을 사용하지 않는다면, 악용될 가능성이 있습니다.

iOS 키체인을 올바르게 활용함으로써, 개발자들은 민감한 데이터가 Apple의 암호화 표준에 의해 보호되는 안전한 환경에 저장되도록 보장할 수 있습니다. 이 안내서는 iOS 키체인을 활용하여 앱을 효과적으로 보호하는 방법에 대해 안내하며, 데이터를 해커로부터 보호하고 공격을 예방하는 최상의 방법을 제공합니다.

<div class="content-ad"></div>

# iOS 키체인 개요

- 핵심 암호화: iOS 키체인은 AES-256 암호화에 기반을 두고 있어, 가장 안전한 암호화 기준 중 하나입니다. 애플은 암호화 프로세스를 내부적으로 처리하여, 키체인에 저장된 데이터가 암호화되어 무단 사용자나 애플리케이션에서 접근할 수 없도록 보장합니다.
- 장치별 보안: 키체인에 저장된 데이터는 해당 장치에 특정한 키로 암호화됩니다. 이는 해커가 키체인 데이터의 암호화된 백업을 획득하더라도 다른 장치에서는 해독할 수 없다는 것을 의미합니다. 장치별 암호화 키는 Secure Enclave라는 안전한 하드웨어 구성 요소에 저장되어, 장치가 침해당해도 민감한 데이터가 안전하게 보호됩니다.
- 자동 데이터 보호: 앱이 종료되거나 장치가 잠겨 있을 때에도 키체인 데이터는 보호됩니다. 이로써 앱이 실행 중이 아닐 때에도 데이터에 무단 접근을 방지하여 악용 가능성을 줄입니다.
- 생체 인증과 통합: 키체인은 생체 인증 (터치 ID 또는 페이스 ID)과 결합하여 추가적인 보호층을 제공할 수 있습니다. 이를 통해 장치에 누군가 접근하더라도 생체 인증을 통과하지 않으면 키체인에서 민감한 정보를 검색할 수 없습니다.
- 앱 간 공유: 키체인을 사용하면 동일한 앱 그룹 내의 앱 간에 안전하게 데이터를 공유할 수 있습니다. 이는 동일한 개발자의 여러 앱 간에 자격 증명이나 토큰을 공유할 때 다른 앱에 데이터를 노출하지 않고 사용할 수 있습니다.
- 접근 제어: 키체인의 가장 강력한 기능 중 하나는 세밀한 접근 제어입니다. 개발자는 키체인 항목이 액세스 가능한 시점을 명시할 수 있습니다:

    - 장치가 잠금 해제된 경우에만 (kSecAttrAccessibleWhenUnlocked).
    - 장치가 잠금 해제된 상태에서 장치 내에 데이터가 남아 있는 경우에만 (kSecAttrAccessibleWhenUnlockedThisDeviceOnly).
    - 장치 재부팅 후에도 사용 가능한 상태 (kSecAttrAccessibleAfterFirstUnlock). 이러한 액세스 제어를 통해 민감한 정보가 어떻게 사용되고 장치가 언제 사용되는지에 따라 데이터가 보호됩니다.

# 주요 포인트

<div class="content-ad"></div>

- 민감한 데이터 안전하게 저장하기: 비밀번호, API 토큰 또는 인증 자격 증명과 같은 민감한 정보는 항상 키체인에 저장해야 합니다. UserDefaults나 일반 텍스트 파일과 같이 보안이 취약한 위치에는 저장하지 않도록 합니다.
- 올바른 키체인 액세스 제어 사용하기: 다양한 유형의 민감한 데이터에 대해 올바른 액세스 제어 옵션을 선택합니다. 높은 보안 요건이 필요한 데이터에 대해서는 kSecAttrAccessibleWhenUnlockedThisDeviceOnly와 같이 엄격한 제어를 사용하여 해당 기기가 잠겨 있을 때만 액세스 가능하고 다른 기기로 전송되지 않도록 합니다.
- 데이터 저장 전 암호화하기: 민감한 데이터를 키체인에 저장하기 전 보안을 추가하기 위해 암호화합니다. 내장 암호화를 제공하는 키체인을 사용하더라도 이를 통해 추가적인 보호층을 부여하여 키체인이 침해당할 경우에 대비합니다.
- 민감한 정보 하드코딩 피하기: API 키, 암호화 키 또는 자격 증명과 같은 민감한 데이터를 앱 코드에 직접 하드코딩하지 않습니다. 대신에 키체인에 안전하게 저장하고 필요할 때 동적으로 검색하도록 합니다.
- 키체인 액세스에 생체 인증 구현하기: SecAccessControl을 통해 생체 인증을 구현하여 주요한 민감한 데이터에 대한 Face ID 또는 Touch ID를 사용합니다. 이를 통해 인증된 사용자만이 키체인 데이터에 액세스할 수 있도록 보장합니다.
- 주기적으로 키와 토큰 교체하기: 키체인에 저장된 암호화 키나 인증 토큰을 주기적으로 교체하여 민감한 정보가 침해당한 경우의 위험을 최소화합니다. 백엔드와 협력하여 토큰 만료 및 키 교체 전략을 구현합니다.

# iOS 키체인 사용에 대한 모범 사례

## 1. 민감한 데이터 안전하게 저장하기

개발자들이 가장 흔히 하는 실수 중 하나는 민감한 정보(예: API 토큰, 비밀번호)를 UserDefaults, 일반 텍스트 파일 또는 앱 코드에 직접 저장하는 것입니다. 이러한 저장 위치는 암호화되지 않았으며 앱이 역공학으로 분석될 경우 해커들이 쉽게 접근할 수 있습니다. 이러한 데이터를 안전하게 저장하기 위해 iOS 키체인을 사용해야 하며 이는 기기의 하드웨어에 의해 보호되는 암호화된 안전한 저장 공간을 제공합니다.

<div class="content-ad"></div>

테이블 태그를 Markdown 형식으로 변경하십시오:

```js
import Security

// 토큰 문자열을 Data 형식으로 변환
let tokenData = "API_TOKEN_123".data(using: .utf8)!

// 토큰을 저장하는 Keychain 쿼리 생성
let query: [String: Any] = [
    kSecClass as String: kSecClassGenericPassword,  // 아이템 타입: 일반 패스워드
    kSecAttrService as String: "com.example.app",   // 앱의 서비스 식별자
    kSecAttrAccount as String: "userToken",         // 계정 식별자 (예: "userToken")
    kSecValueData as String: tokenData,             // 저장할 민감한 데이터
    kSecAttrAccessible as String: kSecAttrAccessibleWhenUnlockedThisDeviceOnly // 장치 잠금 해제 시에만 액세스 가능
]

// Keychain에 항목 추가
let status = SecItemAdd(query as CFDictionary, nil)

if status == errSecSuccess {
    print("토큰이 Keychain에 성공적으로 저장되었습니다")
} else {
    print("Keychain에 토큰 저장 실패: \(status)")
}
```

- kSecClass: 저장되는 데이터의 유형을 정의합니다 (중요한 데이터인 경우 kSecClassGenericPassword 사용).
- kSecAttrService: 데이터를 저장하는 앱 또는 서비스의 고유 식별자입니다 (예: 앱 번들 식별자).
- kSecAttrAccount: 데이터의 고유 계정 식별자입니다 (예: userToken).
- kSecValueData: Data 형식으로 변환된 실제 저장되는 데이터입니다.
- kSecAttrAccessible: 데이터 액세스 가능 시점을 지정합니다. 이 예제에서는 장치가 잠금 해제될 때만 액세스 가능합니다 (kSecAttrAccessibleWhenUnlockedThisDeviceOnly).

이 코드는 API 토큰을 Keychain에 안전하게 저장하여 장치가 잠겨 있거나 다른 장치로 전송되는 경우에도 토큰에 액세스할 수 없게 합니다.

<div class="content-ad"></div>

## 2. 올바른 키체인 액세스 컨트롤 사용하기:

키체인 액세스 컨트롤은 민감한 데이터에 대한 액세스 방법과 타이밍을 결정하는 데 중요합니다. iOS 키체인은 다양한 수준의 액세스 컨트롤을 제공하여 개발자가 데이터에 언제 액세스할 수 있는지를 지정할 수 있게 합니다. 장치가 잠긴 상태인지, 재부팅 이후인지, 또는 잠금 해제된 상태인지에 따라 데이터에 액세스할 수 있는지를 설정할 수 있습니다. 보관 중인 데이터의 민감도에 따라 올바른 액세스 컨트롤을 선택하는 것이 중요합니다. 비밀번호나 API 토큰과 같이 매우 민감한 데이터의 경우, kSecAttrAccessibleWhenUnlockedThisDeviceOnly와 같은 엄격한 액세스 컨트롤을 사용하세요. 이를 통해 데이터는 장치가 잠금 해제된 상태일 때에만 액세스 가능하도록하고 다른 장치로의 데이터 이전을 방지합니다.

일반적인 액세스 컨트롤 옵션은 다음과 같습니다:

- kSecAttrAccessibleWhenUnlocked: 데이터는 장치가 잠금 해제된 동안에만 액세스 가능합니다.
- kSecAttrAccessibleWhenUnlockedThisDeviceOnly: 데이터는 장치가 잠금 해제된 동안에만 액세스 가능하며 다른 장치로 전송되지 않습니다.
- kSecAttrAccessibleAfterFirstUnlock: 데이터는 부팅 후 처음 장치가 잠금 해제된 이후에 액세스 가능합니다.
- kSecAttrAccessibleAfterFirstUnlockThisDeviceOnly: 위와 유사하지만 데이터를 다른 장치로 전송하는 것을 방지합니다.

<div class="content-ad"></div>

코드 예시: 민감한 데이터 보관 시 접근 제어 사용하기

```js
import Security

// 토큰 문자열을 Data 형식으로 변환
let tokenData = "API_TOKEN_123".data(using: .utf8)!

// 장치 잠금 해제 시에만 데이터 접근 가능하도록 엄격한 접근 제어 정의
let query: [String: Any] = [
    kSecClass as String: kSecClassGenericPassword,  // 항목 유형: 일반 비밀번호
    kSecAttrService as String: "com.example.app",   // 앱 서비스 식별자
    kSecAttrAccount as String: "userToken",         // 계정 식별자
    kSecValueData as String: tokenData,             // 보관할 민감한 데이터
    kSecAttrAccessible as String: kSecAttrAccessibleWhenUnlockedThisDeviceOnly // 엄격한 접근 제어
]

// 정의된 접근 제어로 Keychain에 항목 추가
let status = SecItemAdd(query as CFDictionary, nil)

if status == errSecSuccess {
    print("안전한 접근 제어로 토큰이 성공적으로 저장되었습니다")
} else {
    print("접근 제어를 사용하여 토큰을 저장하지 못했습니다: \(status)")
}
```

- kSecAttrAccessibleWhenUnlockedThisDeviceOnly: 이는 데이터가 장치가 잠겨 있을 때에만 접근 가능하고 원래 장치에 유지되도록 함을 보장합니다. 데이터는 백업이나 이주를 통해 다른 장치로 전송될 수 없습니다.
- 이 접근 제어는 API 토큰, 비밀번호, 사용자 자격 증명과 같이 장치가 잠겨 있을 때에도 접근되거나 전송되어서는 안 되는 매우 민감한 정보에 이상적입니다.

기타 접근 제어 옵션: 앱의 요구에 따라 접근 제어를 수정할 수 있습니다. 예를 들어:

<div class="content-ad"></div>

- kSecAttrAccessibleAfterFirstUnlock: 재부팅 후에도 지속되어야 하지만 여전히 보호되어야 하는 데이터에 사용합니다(예: 로그인 세션 토큰).
- kSecAttrAccessibleWhenUnlocked: 사용자가 기기를 활성 상태로 사용할 때만 액세스할 수 있는 데이터에 적합한 옵션입니다. 그러나 데이터는 기기 간에 이동할 수 있습니다.

## 3. 데이터 저장 전에 데이터 암호화:

키체인은 내장 암호화를 제공하지만, 민감한 데이터를 저장하기 전에 추가적인 암호화 층을 추가하는 것은 취약성에 대한 더 나은 보호를 제공할 수 있습니다. 예를 들어, 데이터를 직접 암호화하면 해커가 키체인에 액세스하더라도 암호화 키 없이는 데이터가 해독되지 않음을 보장할 수 있습니다. Apple의 암호화 프레임워크인 CryptoKit을 사용하면 데이터를 키체인에 저장하기 전에 안전하게 암호화할 수 있습니다. 특히 매우 민감한 정보에 대한 최고 수준의 보안이 필요할 때 유용합니다.

단계:

<div class="content-ad"></div>

- 대칭 암호화: 데이터를 저장하기 전에 AES-GCM 또는 ChaChaPoly로 대칭 암호화를 사용하여 데이터를 암호화합니다.
- 키 관리: 암호화에 안전하게 생성된 키를 사용하고 Keychain에 안전하게 저장하거나 동적으로 검색하십시오.

코드 예시: Keychain에 저장하기 전 데이터 암호화

```js
import Security
import CryptoKit

// 단계 1: 대칭 키 생성 (이 키는 재사용을 위해 Keychain에 안전하게 저장될 수 있음)
let symmetricKey = SymmetricKey(size: .bits256)

// 단계 2: 대칭 키를 사용하여 민감한 데이터 암호화
let tokenData = "API_TOKEN_123".data(using: .utf8)!
let sealedBox = try! ChaChaPoly.seal(tokenData, using: symmetricKey)

// 단계 3: 암호화된 데이터 (결합된 형식)을 Keychain에 저장
let encryptedData = sealedBox.combined

// 단계 4: 접근 제어와 함께 Keychain 쿼리 생성
let query: [String: Any] = [
    kSecClass as String: kSecClassGenericPassword,
    kSecAttrService as String: "com.example.app",
    kSecAttrAccount as String: "userToken",
    kSecValueData as String: encryptedData, // 암호화된 데이터 저장
    kSecAttrAccessible as String: kSecAttrAccessibleWhenUnlockedThisDeviceOnly // 엄격한 접근 제어
]

// Keychain에 아이템 추가
let status = SecItemAdd(query as CFDictionary, nil)

if status == errSecSuccess {
    print("Keychain에 성공적으로 암호화된 토큰 저장됨")
} else {
    print("Keychain에 암호화된 토큰 저장 실패: \(status)")
}
```

- 대칭 키 생성: CryptoKit을 사용하여 256비트 대칭 키를 생성합니다. 이 키는 민감한 데이터(여기서는 API 토큰)를 암호화하는 데 사용될 것입니다. 키 자체는 나중에 사용을 위해 Keychain에 안전하게 저장될 수 있습니다.
- 데이터 암호화: CryptoKit에서 지원하는 고성능 암호화 알고리즘인 ChaChaPoly를 사용하여 데이터(tokenData)를 암호화합니다. 결과는 암호문, 논스, 인증 태그가 포함된 밀봉 상자입니다.
- Keychain에 암호화된 데이터 저장: 암호화된 데이터(sealedBox.combined)가 Keychain에 저장됩니다. Keychain은 암호화된 데이터 자체를 보호하며, 추가 암호화는 데이터가 읽을 수 없는 상태로 남아있도록 보장합니다.

<div class="content-ad"></div>

## 4. 민감한 정보 하드코딩 피하기:

하드코딩을 피해야 하는 이유:

- 역공학: 해커들은 앱을 디컴파일하거나 역공학하여 코드를 검사할 수 있습니다. 민감한 데이터가 하드코딩되어 있다면 주요한 공격 대상이 될 수 있습니다.
- 정적 분석 도구: 공격자들은 종종 정적 분석 도구를 사용하여 API 키나 자격 증명과 같은 비밀 정보를 찾아내기 위해 앱의 바이너리를 검사합니다.
- 유연성 부족: 하드코딩된 데이터는 쉽게 변경하거나 교체할 수 없어서 보안 침해에 대응하거나 키를 업데이트하는 것이 더 어려울 수 있습니다.

해결책:

<div class="content-ad"></div>

하드코딩하는 대신 API 키, 토큰 및 비밀 정보와 같은 민감한 정보를 Keychain에 저장하세요. Keychain은 암호화와 접근 제어로 보호되어 해커가 이 데이터를 추출하는 것이 거의 불가능하게 만듭니다. 심지어 앱을 역공학으로 분석해도 데이터를 추출하기 어렵습니다.

코드 예시: Keychain에서 API 키를 안전하게 저장하고 검색하는 방법

```js
import Security

// Keychain에 API 키를 안전하게 저장하는 함수
func storeAPIKeyInKeychain(apiKey: String) -> OSStatus {
    // API 키를 데이터 형식으로 변환
    let apiKeyData = apiKey.data(using: .utf8)!

    // API 키를 저장하기 위한 Keychain 쿼리 생성
    let query: [String: Any] = [
        kSecClass as String: kSecClassGenericPassword,
        kSecAttrService as String: "com.example.app",
        kSecAttrAccount as String: "apiKey",           // API 키의 계정 식별자
        kSecValueData as String: apiKeyData,           // API 키 데이터 형식
        kSecAttrAccessible as String: kSecAttrAccessibleWhenUnlockedThisDeviceOnly // 접근 제어 설정
    ]

    // Keychain에 항목 추가
    let status = SecItemAdd(query as CFDictionary, nil)

    return status
}

// Keychain에서 API 키를 안전하게 검색하는 함수
func retrieveAPIKeyFromKeychain() -> String? {
    // API 키를 검색하기 위한 Keychain 쿼리 생성
    let query: [String: Any] = [
        kSecClass as String: kSecClassGenericPassword,
        kSecAttrService as String: "com.example.app",
        kSecAttrAccount as String: "apiKey",           // 계정 식별자
        kSecReturnData as String: kCFBooleanTrue!,     // API 키 데이터 반환 요청
        kSecMatchLimit as String: kSecMatchLimitOne    // 결과를 하나로 제한
    ]

    // Keychain에서 항목 검색
    var item: CFTypeRef?
    let status = SecItemCopyMatching(query as CFDictionary, &item)

    if status == errSecSuccess {
        // 반환된 데이터를 문자열로 변환
        if let apiKeyData = item as? Data,
           let apiKey = String(data: apiKeyData, encoding: .utf8) {
            return apiKey
        }
    }

    return nil
}

// 예시 사용법
let apiKeyStatus = storeAPIKeyInKeychain(apiKey: "YOUR_API_KEY_HERE")
if apiKeyStatus == errSecSuccess {
    print("API 키가 Keychain에 성공적으로 저장되었습니다.")
} else {
    print("API 키를 Keychain에 저장하는 데 실패했습니다: \(apiKeyStatus)")
}

if let retrievedAPIKey = retrieveAPIKeyFromKeychain() {
    print("Keychain에서 검색한 API 키: \(retrievedAPIKey)")
} else {
    print("Keychain에서 API 키를 검색하는 데 실패했습니다.")
}
```

- API 키 저장: API 키는 데이터 형식으로 변환되어 Keychain에 SecItemAdd를 사용하여 저장됩니다. kSecAttrAccessibleWhenUnlockedThisDeviceOnly 속성으로 안전하게 보호되어 기기가 잠금 해제된 상태에서만 액세스할 수 있고 다른 기기로 전송할 수 없도록 보장됩니다.
- API 키 검색: API 키는 SecItemCopyMatching을 사용하여 Keychain에서 안전하게 검색되어 민감한 데이터가 하드코딩되는 대신 필요할 때 동적으로 가져오도록 보장됩니다.

<div class="content-ad"></div>

Keychain에 민감한 정보를 저장함으로써, 이러한 데이터가 역공학이나 정적 분석을 통해 노출될 위험을 제거할 수 있어요.

## 5. Keychain 접근을 위한 생체 인증 구현하기:

API 키, 인증 토큰 또는 개인 사용자 정보와 같이 매우 민감한 데이터에 대해, Keychain의 암호화 위에 추가 보안 계층을 추가하는 것이 중요해요. 생체 인증(얼굴 ID 또는 터치 ID)을 사용하여, 누군가가 기기에 접근하더라도 사용자의 생체 항목 확인 없이 민감한 데이터를 가져갈 수 없도록 할 수 있어요.

iOS는 SecAccessControlCreateWithFlags를 사용하여 Keychain 액세스에 생체 인증을 통합할 수 있는 기능을 제공해요. 이를 통해 개발자는 Keychain 항목이 성공적인 생체 인증 이후에만 액세스할 수 있도록 지정할 수 있어요.

<div class="content-ad"></div>

코드 예시: Face ID / Touch ID 인증을 사용하여 키체인에 데이터 저장하기

```swift
import Security

// 생체 인증을 사용하여 키체인에 민감한 데이터 저장하는 함수
func storeDataWithBiometricAuth(data: String) -> OSStatus {
    // 데이터를 Data 형식으로 변환
    let dataToStore = data.data(using: .utf8)!

    // 아이템에 접근하는 데 Face ID 또는 Touch ID가 필요한 액세스 컨트롤 생성
    var error: Unmanaged<CFError>?
    let accessControl = SecAccessControlCreateWithFlags(
        nil,
        kSecAttrAccessibleWhenUnlockedThisDeviceOnly, // 엄격한 액세스 제어
        .userPresence,                                // 사용자 인증 필요 (Face ID / Touch ID)
        &error
    )

    if error != nil {
        print("액세스 컨트롤 생성 중 오류 발생: \(String(describing: error))")
        return errSecParam
    }

    // 생체 인증을 포함한 키체인 쿼리 생성
    let query: [String: Any] = [
        kSecClass as String: kSecClassGenericPassword,
        kSecAttrService as String: "com.example.app",
        kSecAttrAccount as String: "biometricData",     // 계정 식별자
        kSecValueData as String: dataToStore,           // 저장할 민감한 데이터
        kSecAttrAccessControl as String: accessControl! // 생체 인증 액세스 컨트롤 추가
    ]

    // 아이템을 키체인에 추가
    let status = SecItemAdd(query as CFDictionary, nil)

    return status
}
```

- SecAccessControlCreateWithFlags: SecAccessControlCreateWithFlags를 사용하여 액세스 컨트롤 객체를 생성하며, 이것은 사용자 인증(Face ID 또는 Touch ID) 후에만 키체인 아이템에 접근할 수 있도록 지정합니다.
- SecAccessControlCreateWithFlags: kSecAttrAccessibleWhenUnlockedThisDeviceOnly는 데이터가 잠금 해제된 상태에서만 접근 가능하도록 하며, 다른 기기로 전송되지 않도록 보장합니다.
- 키체인 저장: SecItemAdd 함수를 사용하여 데이터를 키체인에 저장하며, 이를 검색하기 위해서는 생체 인증이 필요합니다.

코드 예시: 생체 인증을 사용하여 키체인에서 데이터 검색하기

<div class="content-ad"></div>

```swift
import Security

// 생체 인증을 사용하여 Keychain에서 데이터를 검색하는 함수
func retrieveDataWithBiometricAuth() -> String? {
    // Keychain 항목을 검색하기 위한 쿼리 생성
    let query: [String: Any] = [
        kSecClass as String: kSecClassGenericPassword,
        kSecAttrService as String: "com.example.app",
        kSecAttrAccount as String: "biometricData",     // 계정 식별자
        kSecReturnData as String: kCFBooleanTrue!,      // 데이터를 반환 요청
        kSecUseOperationPrompt as String: "자료에 액세스하려면 인증하세요" // 생체 인증 프롬프트
    ]

    // Keychain에서 항목 검색
    var item: CFTypeRef?
    let status = SecItemCopyMatching(query as CFDictionary, &item)

    if status == errSecSuccess {
        // 반환된 데이터를 String으로 변환
        if let retrievedData = item as? Data,
           let dataString = String(data: retrievedData, encoding: .utf8) {
            return dataString
        }
    } else {
        print("Keychain에서 데이터를 검색하는 데 실패했습니다: \(status)")
    }

    return nil
}

// 예시 사용법
let status = storeDataWithBiometricAuth(data: "중요 정보")
if status == errSecSuccess {
    print("생체 인증으로 데이터를 성공적으로 저장했습니다.")
} else {
    print("생체 인증으로 데이터를 저장하는 데 실패했습니다: \(status)")
}

if let retrievedData = retrieveDataWithBiometricAuth() {
    print("Keychain에서 검색된 데이터: \(retrievedData)")
} else {
    print("데이터를 검색하는 데 실패했습니다.")
}
```

검색 프로세스:

- kSecUseOperationPrompt 키는 저장된 데이터에 액세스하기 전에 생체 인증 (Face ID 또는 Touch ID)를 요청하는 프롬프트를 표시하는 데 사용됩니다.
- 사용자가 데이터를 검색하려고 할 때 “자료에 액세스하려면 인증하세요”와 같은 메시지가 표시됩니다.

데이터 검색:

<div class="content-ad"></div>

- 생체 인증이 성공하면 데이터가 키체인에서 검색됩니다. 인증에 실패하면 데이터에 액세스할 수 없습니다.

보안 혜택:

- 이 방법을 통해 장치에 누군가가 접근하더라도 민감한 데이터는 생체 인증 이후에만 검색할 수 있습니다.
- 이것은 키체인의 암호화와 액세스 제어 위에 추가적인 보안 계층을 더합니다.

## 6. 주기적으로 키와 토큰 회전하기

<div class="content-ad"></div>

안녕하세요! 앱의 장기 보안을 유지하기 위한 중요한 실천 방법으로 암호화 키 또는 인증 토큰을 주기적으로 교체하는 것이 중요합니다. API 키, 토큰 또는 암호화 키가 긴 기간 동안 변경되지 않은 채로 유지되면 악의적인 사용자에 의해 compromise되고 악용될 가능성이 높아집니다. 이러한 키나 토큰을 정기적으로 교체함으로써 해커들이 compromise된 데이터를 이용할 수 있는 기회를 줄일 수 있습니다.

안전한 시스템에서는 다음과 같은 조치가 필요합니다:

- API 키: compromise된 키의 노출을 제한하기 위해 정기적으로 교체되어야 합니다.
- 암호화 키: 오래된 키가 compromise되더라도 새로 암호화된 데이터가 안전하게 유지되도록 주기적으로 교체되어야 합니다.
- 토큰: 세션 기반 인증을 위한 토큰은 주기적으로 갱신되어야 하며 이를 통해 사용되지 않은 토큰을 악용할 수 없도록 보호됩니다.

조치 방법:

<div class="content-ad"></div>

- 새 키나 토큰을 주기적으로 생성하거나 사용 횟수 후에 생성하세요.
- 새 키를 안전하게 Keychain에 저장하여 이전 키를 대체하세요.
- 노출되었을 경우 영향을 최소화하기 위해 이전 키나 토큰을 만료시키거나 무효화하세요.
- 만료된 또는 무효화된 토큰을 처리하기 위해 백엔드와 키 회전을 조정하세요.

코드 예시: API 토큰 회전하기: 새로 생성된 토큰을 업데이트하고 저장하고, 이전 토큰을 안전하게 Keychain에서 제거하는 방법입니다.

```js
import Security

// 새 API 토큰을 저장하고 이전 토큰을 회전하는 함수
func rotateAPIToken(newToken: String) -> OSStatus {
    // 새 토큰을 데이터 형식으로 변환
    let newTokenData = newToken.data(using: .utf8)!

    // 먼저, Keychain에서 이전 토큰 삭제
    let deleteQuery: [String: Any] = [
        kSecClass as String: kSecClassGenericPassword,
        kSecAttrService as String: "com.example.app",
        kSecAttrAccount as String: "apiToken"
    ]

    let deleteStatus = SecItemDelete(deleteQuery as CFDictionary)
    
    if deleteStatus == errSecSuccess || deleteStatus == errSecItemNotFound {
        // 이전 토큰이 성공적으로 삭제되었거나 존재하지 않음
        
        // 새 토큰을 저장할 Keychain 쿼리 생성
        let query: [String: Any] = [
            kSecClass as String: kSecClassGenericPassword,
            kSecAttrService as String: "com.example.app",
            kSecAttrAccount as String: "apiToken",          // API 토큰을 위한 계정 식별자
            kSecValueData as String: newTokenData,           // 데이터 형식의 새 API 토큰
            kSecAttrAccessible as String: kSecAttrAccessibleWhenUnlockedThisDeviceOnly // 접근 제어
        ]
        
        // 새 토큰을 Keychain에 추가
        let addStatus = SecItemAdd(query as CFDictionary, nil)
        
        return addStatus
    } else {
        print("Keychain에서 이전 토큰 삭제 실패: \(deleteStatus)")
        return deleteStatus
    }
}
```

- 이전 토큰 삭제: 새 토큰을 저장하기 전에 SecItemDelete 함수를 사용하여 Keychain에서 기존 토큰을 제거하므로, 새로운 토큰을 저장한 후에도 낡거나 손상된 토큰이 사용되지 않도록 합니다.
- 새 토큰 저장: 이전 토큰을 삭제한 후, 새 API 토큰은 해당 장치에서만 액세스할 수 있도록 적절한 액세스 제어와 함께 Keychain에 안전하게 저장됩니다(kSecAttrAccessibleWhenUnlockedThisDeviceOnly를 사용한 경우).

<div class="content-ad"></div>

암호화 키 회전 코드 예제: 암호화 키를 주기적으로 회전시키면, 이전 키가 노출되더라도 새로 암호화된 데이터가 안전하게 유지됩니다.

```js
import CryptoKit
import Security

// Function to rotate encryption keys
func rotateEncryptionKey() -> SymmetricKey {
    // Generate a new symmetric encryption key
    let newKey = SymmetricKey(size: .bits256)

    // Convert the new key to Data format
    let newKeyData = newKey.withUnsafeBytes { Data(Array($0)) }

    // First, delete the old encryption key from the Keychain
    let deleteQuery: [String: Any] = [
        kSecClass as String: kSecClassGenericPassword,
        kSecAttrService as String: "com.example.app",
        kSecAttrAccount as String: "encryptionKey"
    ]

    let deleteStatus = SecItemDelete(deleteQuery as CFDictionary)

    if deleteStatus == errSecSuccess || deleteStatus == errSecItemNotFound {
        // Old key successfully deleted or doesn't exist
        
        // Create the Keychain query to store the new encryption key
        let query: [String: Any] = [
            kSecClass as String: kSecClassGenericPassword,
            kSecAttrService as String: "com.example.app",
            kSecAttrAccount as String: "encryptionKey",          // The account identifier for the encryption key
            kSecValueData as String: newKeyData,                  // The new encryption key in Data format
            kSecAttrAccessible as String: kSecAttrAccessibleWhenUnlockedThisDeviceOnly // Access control
        ]

        // Add the new encryption key to the Keychain
        let addStatus = SecItemAdd(query as CFDictionary, nil)

        if addStatus == errSecSuccess {
            print("New encryption key stored successfully.")
        } else {
            print("Failed to store new encryption key: \(addStatus)")
        }
    } else {
        print("Failed to delete old encryption key: \(deleteStatus)")
    }

    return newKey
}
```

- 새로운 암호화 키 생성: CryptoKit을 사용하여 새로운 256비트 대칭키가 생성됩니다.
- 이전 키 삭제: 새로운 키를 저장하기 전에 Keychain에서 이전 암호화 키를 제거하여 현재 키에만 접근할 수 있도록 합니다.
- 새로운 키 저장: 새로 생성된 암호화 키가 Keychain에 안전하게 저장되며, 적절한 접근 제어가 적용됩니다 (kSecAttrAccessibleWhenUnlockedThisDeviceOnly).

추가 고려 사항:

<div class="content-ad"></div>

- Backend Coordination: API 키 또는 토큰을 교체할 때, 백엔드에서 새로운 키 또는 토큰을 처리할 수 있고, 이전 것을 적절하게 무효화하는지 확인하세요.
- 토큰 만료: 일정 기간 또는 사용 횟수 후에 토큰이 만료되도록 설정하여 정기적으로 교체하는 시스템을 구현하세요.
- 암호화 키 관리: 장기적인 암호화를 위해 암호화된 데이터를 새 키로 다시 암호화할 수 있는 방식으로 저장하는 것이 중요합니다. 데이터 손실이나 손상 없이 다시 암호화할 수 있어야 합니다.

# 결론

iOS 애플리케이션에서 민감한 데이터를 안전하게 보호하는 것은 앱 개발의 중요한 측면입니다. 특히, 모바일 앱이 해커의 주요 표적이 되는 상황에서는 더욱 중요합니다. iOS 키체인을 활용하여 개발자는 API 키, 인증 토큰, 비밀번호와 같은 중요 정보를 역공학 및 무단 접근에 대한 저항이 강한 안전하고 암호화된 환경에 저장할 수 있습니다.

이 가이드를 통해 저희는 다음을 살펴보았습니다:

<div class="content-ad"></div>

- iOS 키체인의 강력한 기능과 암호화, 액세스 제어 및 기기별 보안과 같은 주요 기능
- 민감한 데이터를 안전하게 저장하고 검색하는 최상의 방법에 대한 모범 사례, 올바른 액세스 제어 선택의 중요성, 데이터를 저장하기 전에 암호화하는 것, 민감한 정보 하드코딩을 피하는 방법 등
- 생체 인증 (Face ID / Touch ID)을 활용하여 권한이 있는 사용자만 중요 데이터에 액세스할 수 있도록 하는 것, 주기적으로 키 및 토큰을 회전시켜 악용 가능성을 최소화하는 중요성

이러한 사례를 실행함으로써 개발자들은 데이터 유출의 위험을 크게 줄일 수 있으며, 앱이 역공학적으로 분석되더라도 민감한 정보가 보호됨을 보장합니다.

# 마지막으로:

iOS 키체인은 보안 방어용 강력한 도구이지만 그것은 보호의 한 가지 층에 불과합니다. 언제나 앱 보안에 대해 전체적으로 생각하고 안전한 통신 프로토콜, 정기적인 보안 감사, 최상의 코딩 관행을 통합하여 견고하고 안전한 애플리케이션을 만드는 것을 고려하십시오.

<div class="content-ad"></div>

앱을 보호하는 것은 사용자를 지키는 것뿐만이 아니라 여러분의 명예를 보호하고 앱이 신뢰할 수 있고 신뢰성 있는 플랫폼으로 유지되도록 하는 것과 관련이 있습니다.