---
title: "Node.js에서 SAML 및 Okta, Auth0와 같은 주요 IdP와 함께 사용자 인증을 쉽게 만드는 SSO 로그인 포괄적인 사용자 인증 가이드"
description: ""
coverImage: "/assets/img/2024-08-21-SSOLogininNodejsmadeeasyAComprehensiveGuidetoUserAuthenticationwithSAMLandLeadingIdPslikeOktaAuth0andMore_0.png"
date: 2024-08-21 17:40
ogImage: 
  url: /assets/img/2024-08-21-SSOLogininNodejsmadeeasyAComprehensiveGuidetoUserAuthenticationwithSAMLandLeadingIdPslikeOktaAuth0andMore_0.png
tag: Tech
originalTitle: "SSO Login in Node.js made easy A Comprehensive Guide to User Authentication with SAML and Leading IdPs like Okta, Auth0, and More"
link: "https://medium.com/@mujhassan786/sso-login-in-node-js-d071b2e4f47d"
isUpdated: false
---


![image](/assets/img/2024-08-21-SSOLogininNodejsmadeeasyAComprehensiveGuidetoUserAuthenticationwithSAMLandLeadingIdPslikeOktaAuth0andMore_0.png)

이 기사에서는 Node.js 애플리케이션과 Identity Provider (IdP)를 통합하는 방법을 배우게 될 거에요. 특정 IdP에 중점을 둔 많은 안내서와는 달리, 우리는 모든 IdP와 작동할 수 있도록하는 일반적인 방법론을 개발할 거에요. 이 안내서는 여러 조직 또는 클라이언트를 봉사하도록 설계된 다중 테넌트 애플리케이션에서 특히 유용합니다. 각각 다른 Identity Provider (IdP)를 사용할 수 있습니다. 예시로 Okta를 사용하겠지만, 동일한 원칙을 적용하여 다른 IdP를 신속하게 통합할 수 있습니다.

Node.js, Single Sign-On (SSO) 및 SAML에 어느 정도 익숙하다고 가정할게요. 간단히 요약하자면, SSO는 사용자가 한 세트의 자격 증명으로 여러 애플리케이션이나 서비스에 액세스할 수 있는 인증 프로세스이며, SAML은 안전하게 인증 정보를 교환하는 표준 프로토콜로 사용됩니다. 사용자가 인증되면 다시 로그인할 필요 없이 서비스 간을 원활하게 이동할 수 있습니다. 아래는 Node.js 앱에서 Okta 통합을 설정하는 간단한 단계입니다.

## Okta 계정 및 응용 프로그램 설정하기

<div class="content-ad"></div>

https://developer.okta.com/으로 이동해서 개발자 계정을 등록해주세요.

![image1](/assets/img/2024-08-21-SSOLogininNodejsmadeeasyAComprehensiveGuidetoUserAuthenticationwithSAMLandLeadingIdPslikeOktaAuth0andMore_1.png)

계정을 등록한 후, 애플리케이션으로 이동하여 App Integration을 생성해주세요.

![image2](/assets/img/2024-08-21-SSOLogininNodejsmadeeasyAComprehensiveGuidetoUserAuthenticationwithSAMLandLeadingIdPslikeOktaAuth0andMore_2.png)

<div class="content-ad"></div>

SAML 2.0을 로그인 방법으로 선택하세요.

![이미지](/assets/img/2024-08-21-SSOLogininNodejsmadeeasyAComprehensiveGuidetoUserAuthenticationwithSAMLandLeadingIdPslikeOktaAuth0andMore_3.png)

## 애플리케이션 구성

학습 목적으로 진행하는 이 프로세스에서 우리는 로컬 Node.js 앱 URL인 http://localhost:4000을 사용할 것입니다. Single Sign-On URL은 http://localhost:4000/auth/sso-callback?domainName=test_org로 설정되어 있습니다. 이 URL은 Okta가 로그인 후 SAML 어설션을 보내는 곳이며, 도메인 이름은 콜백 시 사용할 SSO 구성이 어떤 조직에 속하는지 식별하는 데 사용됩니다. Audience URI (SP 엔티티 ID)도 http://localhost:4000으로 설정되어 있습니다. 이는 SAML 응답이 귀하의 애플리케이션을 위한 것임을 보장하기 위한 고유 식별자 역할을 합니다. Default RelayState는 비워두어 사용자가 로그인 후 기본 랜딩 페이지로 리디렉션되도록 합니다. 이러한 설정을 통해 SAML 어설션이 안전하게 전달되고 귀하의 애플리케이션에 올바르게 처리되도록 보장할 수 있습니다.

<div class="content-ad"></div>

<img src="/assets/img/2024-08-21-SSOLogininNodejsmadeeasyAComprehensiveGuidetoUserAuthenticationwithSAMLandLeadingIdPslikeOktaAuth0andMore_4.png" />

## SAML Assertions이란 무엇인가요?

SAML assertions는 사용자에 대한 인증 정보를 포함하는 XML 문서로, Identity Provider (IdP)에서 성공적인 로그인 후 발급됩니다. 이러한 assertions에는 사용자의 식별 정보와 같은 세부 정보가 포함되어 있으며, 해당 정보는 사용자가 인증되었음을 확인하기 위해 서비스 제공자 (SP)에게 보내집니다. 이 경우, SP는 이러한 assertion을 확인하여 자신을 위해 의도된 것인지 확인한 후 사용자에게 액세스 권한을 부여합니다.

<img src="/assets/img/2024-08-21-SSOLogininNodejsmadeeasyAComprehensiveGuidetoUserAuthenticationwithSAMLandLeadingIdPslikeOktaAuth0andMore_5.png" />

<div class="content-ad"></div>

피드백 단계를 마치면 'Sign On' 탭으로 이동하세요. 여기에서 통합에 필요한 설정을 찾을 수 있습니다. 저희는 솔루션을 구현하기 위해 @node-saml/node-saml 패키지를 사용할 것입니다. 필요한 주요 매개변수는 다음과 같습니다:

- 발행자(issuer): 아이덴티티 제공자(옥타)를 위한 고유 식별자로, SAML 어설션이 올바른 IdP에 의해 발행되었음을 보장합니다.
- 수신자(audience): SAML 어설션의 목표 수신자로, 애플리케이션의 SP 엔티티 ID와 일치하여 Node.js 앱을 위한 것임을 확인합니다.
- entryPoint(로그인 URL): SAML 인증 요청이 전송되는 URL로, 사용자를 IdP 로그인 페이지(옥타)로 안내하여 SSO를 시작합니다.
- callbackUrl: 로그인 후 옥타가 SAML 응답을 보내는 앱의 엔드포인트로, 어설션을 처리하고 검증합니다.
- wantAuthnResponseSigned: IdP에서 SAML 응답에 서명을 할지 여부를 지정합니다. 여기서 false로 설정되어 있으며, 서명된 응답이 필요하지 않음을 의미합니다.
- idpCert: SAML 응답의 진위성과 무결성을 확인하는 데 사용되는 옥타의 공개 인증서입니다.

```js
{
    "issuer": "http://www.okta.com/xxxxxxxxxx",
    "audience": "http://localhost:4000",
    "entryPoint": "https://dev-xxxxxxx.okta.com/app/xxxxxxx/xxxxxxxxx/sso/saml",
    "callbackUrl": "http://localhost:4000/auth/sso-callback",
    "wantAuthnResponseSigned": false,
    "idpCert": "xxxxx"
}
```

상기 필요 매개변수는 'Sign On' 탭에서 얻을 수 있습니다.

<div class="content-ad"></div>

<table>
<tr>
<th>앱에 사람 배정</th>
</tr>
</table>

노드.js 애플리케이션 설정하기

<div class="content-ad"></div>

다음으로, 기본 Node.js 애플리케이션을 설정하거나 기존 애플리케이션에 이 기능을 통합해 보세요. 두 가지 새로운 엔드포인트를 추가해야 합니다:

- /sso-login (GET): 이 엔드포인트는 SSO 프로세스를 시작합니다. 제공된 도메인 이름을 기반으로 조직의 SSO 구성을 가져오고 로그인 URL을 생성하여 클라이언트에게 전송합니다.
- /sso-callback (POST): 이 엔드포인트는 SSO 콜백을 처리합니다. 사용자가 Identity Provider (IdP)를 통해 성공적으로 로그인한 후 SAML 응답이 이 엔드포인트로 전송됩니다. 여기서 SAML 주장을 유효성 검사하고 사용자를 확인하며 액세스 토큰을 생성한 뒤 사용자를 대시보드로 리디렉션합니다.

아래는 이러한 엔드포인트의 코드 예시입니다:

```js
const ssoLogin = async (req, res)=>{
    let { domainName } = req.query;
    const organization = await organizations.findOne({domainName})

    if(!organization) return res.status(404).json({ message: "No organization found" });
    const { ssoConfig } = organization;
    const loginUrl = await getLoginUrl(ssoConfig);
    res.json({ ssoUrl: loginUrl });
  }

async function ssoCallback(req, res) {
    try {
      let { domainName } = req.query;
      const organization = await organizations.findOne({domainName})

      if(!organization) throw boom.notFound('Organization Not found');
      const assertion = await validateLogin(req.body, organization.ssoConfig);
      const email = assertion.nameID
      if (!email) throw boom.unauthorized('Email not found in SAML assertion.');
      const ssoUser = await user.findOne({ email });

      if (!ssoUser) { throw boom.unauthorized('SSO login was attempt was failed.') }
      const userAccessToken = generateAccessToken(ssoUser._id.toString());
      //redirect to dashboard with the token in the url.
      res.redirect(`${process.env.DASHBOARD_URL}?token=${userAccessToken}`);
    } catch (error) {
      res.status(500).json({ message: error.message });
    }
  }
```

<div class="content-ad"></div>

위 코드에서 SAML 클래스는 Single Sign-On (SSO)에서 SAML 요청 및 응답을 처리하는 도구로 작용합니다. samlOptions를 전달하여이 도구를 Identity Provider (IdP) 인 Okta와 상호 작용하는 데 필요한 설정으로 구성할 수 있습니다. getAuthorizeUrlAsync 메서드는 사용자를 IdP의 로그인 페이지로 안내하는 안전한 로그인 URL을 생성합니다. 사용자가 로그인 한 후 validatePostResponseAsync 메서드는 IdP로부터 받은 SAML 응답 (samlResponse)을 확인하고 응답이 유효하며 사용자 정보를 추출하는 도구를 사용합니다.

## Workflow Overview

<div class="content-ad"></div>

http://localhost:4000/auth/sso-login?domainName=test_org에 액세스하면, 애플리케이션이 제공된 도메인 이름을 기반으로 조직에 해당하는 SSO 구성을 검색합니다. 그런 다음 서버가 SAML 인증 요청을 생성하고 ssoUrl을 반환합니다. 프론트엔드는 이 URL을 사용하여 사용자를 Okta와 같은 조직의 ID 공급자(IdP)로 리디렉션하며 거기서 자격 증명을 입력합니다.

![이미지](/assets/img/2024-08-21-SSOLogininNodejsmadeeasyAComprehensiveGuidetoUserAuthenticationwithSAMLandLeadingIdPslikeOktaAuth0andMore_8.png)

![이미지](/assets/img/2024-08-21-SSOLogininNodejsmadeeasyAComprehensiveGuidetoUserAuthenticationwithSAMLandLeadingIdPslikeOktaAuth0andMore_9.png)

성공적인 로그인 후에 Okta는 사용자를 구성한 단일 로그인 URL로 리디렉션하며, 이 경우에는 http://localhost:4000/auth/sso-callback?domainName=test_org입니다. 애플리케이션은 그런 다음 SAML 응답을 유효성 검사하고 JWT 토큰을 생성하여 사용자를 대시보드 URL로 리디렉션하며, 이 경우에는 http://localhost:3000이며 URL에 토큰이 포함됩니다. 이제 프론트엔드는 이 JWT 토큰을 사용하여 사용자를 인증하고 세션을 유지할 수 있습니다.

<div class="content-ad"></div>

![이미지](/assets/img/2024-08-21-SSOLogininNodejsmadeeasyAComprehensiveGuidetoUserAuthenticationwithSAMLandLeadingIdPslikeOktaAuth0andMore_10.png)

## Git 저장소:

만약 이 글이 도움이 되었다면 👏, 코드도 도움이 되었다면 ⭐️을 주시면 감사하겠습니다.