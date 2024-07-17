---
title: "Flutter 앱에 비밀번호 관리자 지원 추가하는 방법"
description: ""
coverImage: "/assets/img/2024-07-12-HowToAddPasswordManagerSupportToYourFlutterApps_0.png"
date: 2024-07-12 21:57
ogImage: 
  url: /assets/img/2024-07-12-HowToAddPasswordManagerSupportToYourFlutterApps_0.png
tag: Tech
originalTitle: "How To Add Password Manager Support To Your Flutter Apps"
link: "https://medium.com/gitconnected/how-to-add-password-manager-support-to-your-flutter-apps-2412685145da"
---


![링크](/assets/img/2024-07-12-HowToAddPasswordManagerSupportToYourFlutterApps_0.png)

안녕하세요! 패스워드 매니저는 시간을 절약하고 더욱 강력하고 다양한 비밀번호를 선택하는 데 도움을 줍니다. 모든 사이트의 수백 개의 자격 증명을 기억하는 대신, 마스터 비밀번호 하나로 충분합니다. 이 기사에서는 플러터 앱에 패스워드 매니저 지원을 추가하는 방법을 보여 드릴 거에요. 또한, 이를 구성하여 이메일, 사용자 이름, 생일, 주소 데이터 또는 신용 카드 정보를 자동으로 작성하는 것도 가능합니다!

# 로그인 예제

간단한 로그인 페이지가 있다고 상상해보세요. 사용자 이름과 비밀번호를 입력하는 두 개의 TextField 위젯이 있는 페이지입니다.

<div class="content-ad"></div>

패스워드 매니저 및 자동완성 기능을 지원하려면 입력 필드를 AutofillGroup 위젯으로 래핑하면 됩니다. 그런 다음 필드별로 AutofillHints를 지정합니다. 선택할 수 있는 다양한 옵션이 있고 배열로 정의할 수 있습니다.

다음은 코드 예제입니다:

```js
class LoginScreen extends StatelessWidget {
  final _usernameController = TextEditingController();
  final _passwordController = TextEditingController();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Login')),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: AutofillGroup(
          child: Column(
            children: [
              TextField(
                controller: _usernameController,
                decoration: InputDecoration(labelText: 'Username'),
                autofillHints: [AutofillHints.username]
              ),
              SizedBox(height: 16.0),
              TextField(
                controller: _passwordController,
                decoration: InputDecoration(labelText: 'Password'),
                obscureText: true,
                autofillHints: [AutofillHints.password]
              ),
              SizedBox(height: 16.0),
              ElevatedButton(
                onPressed: () {
                  // Handle login logic here
                  print('Username: ${_usernameController.text}');
                  print('Password: ${_passwordController.text}');
                },
                child: Text('Login')
              )
            ]
          )
        )
      )
    );
  }
}
```

사용자 이름의 경우, 이메일을 사용하는 경우 AutofillHints.username 및 AutofillHints.email을 사용하는 것이 합리적일 수 있습니다.

<div class="content-ad"></div>

생일, 주소 데이터, 신용카드 정보 또는 다른 내용을 사용할 수도 있어요. 단지 autofillHints 속성을 알맞게 변경해주세요. 추가했지만 autofill이 작동하지 않을 때는 페이지 하단의 문제 해결 힌트를 확인해보세요.

AutofillHints 클래스에 정의된 모든 상수의 전체 목록과 설명이 여기 있어요:

```js
addressCity → 입력 필드는 주소 지역 (도시/마을)를 예상합니다.
addressCityAndState → 입력 필드는 도시명과 주 이름을 결합한 것을 예상합니다.
addressState → 입력 필드는 지역/주를 예상합니다.
birthday → 입력 필드는 사람의 전체 생년월일을 예상합니다.
birthdayDay → 입력 필드는 사람의 생년월일 일(월)을 예상합니다.
birthdayMonth → 입력 필드는 사람의 생년월일 월을 예상합니다.
birthdayYear → 입력 필드는 사람의 생년월일 년을 예상합니다.
(중략)
username → 입력 필드는 사용자 이름 또는 계정 이름을 예상합니다.
```

아래에서는 Flutter 웹 앱에서 Bitwarden 비밀번호 관리자를 사용할 때 통합이 어떻게 보이는지 예시를 확인할 수 있어요.

<div class="content-ad"></div>


![이미지](/assets/img/2024-07-12-HowToAddPasswordManagerSupportToYourFlutterApps_1.png)

# 결론

이 글에서는 플러터 앱에 비밀번호 관리자 지원하는 방법을 배웠습니다. 사용자들의 삶을 더 쉽게 만드세요. 사용자명, 비밀번호, 거리 주소 또는 신용 카드 데이터를 입력하는 바로 가기를 추가해보세요.

💡 기다려주세요! 제 뉴스레터에 가입하여 간단한 플러터 팁을 받아보세요! 콤팩트한 이북으로 Firebase 또는 플러터를 배우세요. 그리고 디지털 제품, 도구 및 무료 자료가 있는 내 상점도 방문해보세요!
