---
title: "안드로이드 브로드캐스트 리시버 개념 정리"
description: ""
coverImage: "/assets/img/2024-08-26-AndroidBroadcastReceiversExplainedEasyStepstoImplement_0.png"
date: 2024-08-26 19:22
ogImage: 
  url: /assets/img/2024-08-26-AndroidBroadcastReceiversExplainedEasyStepstoImplement_0.png
tag: Tech
originalTitle: "Android Broadcast Receivers Explained Easy Steps to Implement"
link: "https://medium.com/@nileshg994/android-broadcast-receivers-explained-easy-steps-to-implement-6c7ee360bb06"
isUpdated: true
updatedAt: 1724744036089
---



![image](/assets/img/2024-08-26-AndroidBroadcastReceiversExplainedEasyStepstoImplement_0.png)

Andoroid 앱들은 종종 서로 통신하거나 시스템 전체적인 이벤트에 반응해야 할 때가 있습니다. 그때 브로드캐스트 수신기가 필요합니다. 브로드캐스트 수신기는 중개인 역할을 하며, 여러 앱이나 시스템 전체에서 발생하는 이벤트에 대해 앱이 듣고 반응할 수 있도록 해줍니다.

# 브로드캐스트 수신이란?

브로드캐스트 수신기는 Android의 구성 요소로, 여러분의 앱이 Android 시스템이나 다른 앱으로부터 브로드캐스트 메시지를 수신할 수 있게 해줍니다. 이 메시지들은 여러분의 앱에게 다음과 같은 사항을 알려줄 수 있습니다:


<div class="content-ad"></div>

- 장치의 배터리 레벨이 낮습니다.
- 장치가 Wi-Fi 네트워크에 연결되었습니다.
- 어떤 앱이 다운로드를 완료했습니다.
- 화면이 꺼졌습니다.

이벤트가 발생하면 방송 메시지가 전송되며, Broadcast Receiver를 사용하여 앱이 이를 잡을 수 있습니다.

## Broadcast Receiver는 어떻게 작동하나요?

당신의 앱을 라디오 방송국으로 상상해보세요. Broadcast Receiver는 특정 신호(방송)에 튜닝된 라디오 안테나와 같습니다. 신호(방송 메시지)가 감지되면 앱은 그에 따라 반응할 수 있습니다.

<div class="content-ad"></div>

앱이 인터넷에 연결되거나 연결이 끊겼을 때를 감지하고 싶다고 가정해 봅시다. 이를 하는 방법은 다음과 같습니다:

## 1. 방송 수신기 정의:

여기서는 앱이 방송 메시지를 수신할 때 수행해야 할 작업을 지정합니다.

```js
class NetworkChangeReceiver : BroadcastReceiver() {
    override fun onReceive(context: Context?, intent: Intent?) {
        // 메시지 처리
    }
}
```

<div class="content-ad"></div>

## 2. 수신기 등록:

Android에서 등록할 수 있는 두 가지 유형의 Broadcast Receivers가 있습니다. 당신의 요구에 맞는 것을 선택하세요: `Static receivers`, AndroidManifest.xml에 선언되는 정적 수신기, 또는 `Dynamic receivers`, 코드에서 동적으로 등록되는 수신기.

A) 정적 등록: 앱의 AndroidManifest.xml 파일에 선언됩니다. 이 방법은 시스템 전역 방송에 사용되며 앱이 실행 중이 아닌 경우에도 수신할 수 있습니다.

```js
<application>
        <!-- 다른 앱 컴포넌트들(activities 등) -->

        <!-- Broadcast Receiver 선언 -->
        <receiver android:name=".NetworkChangeReceiver">
            <intent-filter>
                <action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
            </intent-filter>
        </receiver>
        
</application>
```

<div class="content-ad"></div>

B) 동적 등록: 이 방법은 일반적으로 액티비티나 서비스에서 코드로 수행됩니다. 앱이 실행 중일 때만 방송을 수신해야 할 때 사용됩니다.

```kotlin
override fun onResume() {
    super.onResume()
    val filter = IntentFilter(ConnectivityManager.CONNECTIVITY_ACTION)
    registerReceiver(NetworkChangeReceiver(), filter)
}

override fun onPause() {
    super.onPause()
    unregisterReceiver(NetworkChangeReceiver())
}
```

## 3. 방송 수신:

이벤트가 발생하면 방송 수신기가 메시지를 받아와 작성한 코드를 실행합니다.

<div class="content-ad"></div>

```js
class NetworkChangeReceiver : BroadcastReceiver() {
    override fun onReceive(context: Context?, intent: Intent?) {
        val isConnected = intent?.getBooleanExtra(ConnectivityManager.EXTRA_NO_CONNECTIVITY, false) == false
        if (isConnected) {
            // 연결된 상황일 때 처리할 내용
        } else {
            // 연결이 끊긴 상황일 때 처리할 내용
        }
    }
}
```

## 브로드캐스트 수신기를 사용하는 시기?

브로드캐스트 수신기는 시스템 이벤트나 앱 전체적인 공지에 응답해야 할 때 유용합니다. 그러나 현명하게 사용해야 합니다. 성능상의 이유로, 자주 업데이트가 필요하거나 무거운 처리 작업을 요구하는 작업에는 브로드캐스트 수신기를 사용하지 않는 것이 좋습니다.

## 결론


<div class="content-ad"></div>

브로드캐스트 수신자는 안드로이드 개발에서 강력한 도구입니다. 네 앱이 네트워크 상태 변경, 수신 메시지, 또는 다른 중요한 이벤트와 같은 주변 세계에 대응하고 정보를 유지할 수 있도록 도와줍니다. 브로드캐스트 수신자를 생성하고 등록하는 방법을 이해하면, 앱을 더 상호작용적이고 동적으로 만들 수 있습니다.

다른 블로그도 확인해보세요:

- 안드로이드 알람 관리자: 마법같은 기상 알림
- 안드로이드 WorkManager의 힘 발휘: 포괄적인 안내