---
title: "웹 개발자를 위한 DNS 관리 기본 지식"
description: ""
coverImage: "/assets/img/2024-07-13-DNSManagementBasicsforWebDevelopers_0.png"
date: 2024-07-13 21:54
ogImage: 
  url: /assets/img/2024-07-13-DNSManagementBasicsforWebDevelopers_0.png
tag: Tech
originalTitle: "DNS Management Basics for Web Developers"
link: "https://medium.com/tech-learnings/dns-management-basics-for-web-developers-d07fe205c722"
isUpdated: true
---




대부분의 경우, 웹 개발자들은 DNS 관리의 기본을 이해하는 데 어려움을 겪어 일상적인 작업에서 혼란을 가져올 수 있습니다. 이 게시물에서는 웹 개발자가 DNS 관리 프로세스를 이해하고 일상적인 작업에서 도움을 받을 수 있는 DNS 관리 기본 사항을 논의하고자 합니다. 여기에 게시된 스크린샷과 일부 단계는 Cloudflare DNS 관리자를 기반으로 합니다.

![DNS 관리 기본 사항](/assets/img/2024-07-13-DNSManagementBasicsforWebDevelopers_0.png)

## DNS란 무엇인가요?

DNS는 Domain Name System의 약자입니다. 도메인 네임 시스템(DNS)은 인터넷 도메인 이름이 위치하고 인터넷 프로토콜(IP) 주소로 변환되는 네이밍 데이터베이스입니다. 도메인 네임 시스템은 사람들이 웹사이트를 찾는 데 사용하는 이름을 컴퓨터가 해당 웹사이트를 찾는 데 사용하는 IP 주소로 매핑합니다.

<div class="content-ad"></div>

예를 들어, DNS는 “www.albinsblog.com"과 같은 웹 주소를 해당 사이트를 호스팅하는 컴퓨터의 물리적 IP 주소인 “10.20.100.20"으로 번역합니다 (이 경우 www.albinsblog.com 홈페이지입니다).

여러 도메인 유형이 있습니다(예: 일반 최상위 도메인(gTLD), 국가 코드 최상위 도메인(ccTLD) 등이 있지만 여기서 설명은 생략합니다). 하지만 웹 개발자로서, 루트/네이키드 도메인과 서브도메인에 대해 인식해야 합니다. 도메인 이름을 등록할 때 루트 도메인(네이키드 도메인으로도 불림)으로 알려지는 것을 등록합니다. 예를 들어 albinsblog.com은 루트 도메인이며, 이 루트 도메인 아래에 여러 서브도메인을 등록할 수 있습니다. 예를 들어 www.albinsblog.com의 경우 여기서 www는 albinsblog.com 루트 도메인의 서브도메인입니다.

## 도메인 등록기:

도메인 등록기는 도메인 이름을 판매하고 등록하는 비즈니스이며, 예를 들어 GoDaddy, Google 등이 있습니다. 모든 도메인 이름 등록기는 ICANN(Internet Corporation for Assigned Names and Numbers)에 인가되어 있습니다. ICANN은 도메인 이름을 관리하는 비영리 기구입니다.

<div class="content-ad"></div>

도메인 등록기관은 데이터의 정확성을 보장하기 위해 WHOIS(WHOIS는 사용자가 도메인 및 IP 소유자 정보를 조회하고 도메인 이름에 대한 수십 가지 다른 통계를 확인할 수 있는 정보 데이터베이스입니다) 데이터를 신속하게 업데이트하고 채웁니다.

## DNS 관리:

DNS 관리는 특정 도메인이나 도메인 집합에 대한 도메인 이름 시스템(DNS)을 관리하고 관리하는 것입니다. DNS 레코드를 만들고 편집하거나, DNS 존을 관리하거나, 네임서버를 추가하거나 삭제하는 등의 작업이 포함될 수 있습니다.

## 네임서버란 무엇인가요?

<div class="content-ad"></div>

네임서버는 DNS 관리에서 도메인 이름을 IP 주소로 변환하는 서버입니다. 네임서버는 각각의 도메인과 하나 이상의 IP 주소를 매핑하는 DNS 레코드를 저장하고 구성합니다.

대부분의 도메인 등록 기관은 도메인 이름을 등록할 때 기본 네임서버를 제공할 것입니다. 그러나 사용자 정의 네임서버를 사용하여 DNS 레코드를 관리할 수도 있습니다. 사용자 정의 네임서버를 사용하면 DNS 설정을 더욱 세밀히 조정할 수 있고 성능이나 보안을 향상시킬 수 있습니다.

사용자 정의 네임서버를 사용하려면 도메인 등록기관의 제어판을 통해 도메인의 "네임서버"를 원하는 DNS 서버로 설정해야 합니다. 도메인 등록기관의 기본 네임서버를 사용하여 DNS 레코드를 관리하거나 외부에서 DNS 레코드를 관리할 수 있는 사용자 정의 네임서버로 포인팅합니다.

![이미지](/assets/img/2024-07-13-DNSManagementBasicsforWebDevelopers_1.png)

<div class="content-ad"></div>

DNS 관리자는 기본 네임 서버를 가리킵니다. 예를 들어, Cloudflare는 기본 Cloudflare 네임 서버를 가리킵니다. 필요한 경우, 도메인은 사용자 정의 네임 서버를 가리킬 수 있습니다.

기존 DNS를 사용자 정의 네임 서버로 가리키는 동안 현재 DNS 레코드는 Zone 파일로 내보낼 수 있으며, 동일한 파일을 사용자 정의 네임 서버로 가져올 수 있습니다.

Zone 파일은 도메인에 대한 모든 DNS 레코드가 저장된 기본 위치입니다. DNS 쿼리를 수행할 때 서버는 존 파일에서 정보를 추출하여 응답합니다.

![이미지](/assets/img/2024-07-13-DNSManagementBasicsforWebDevelopers_2.png)

<div class="content-ad"></div>

내보낸 Zone 파일은 모든 구성된 DNS 레코드 세부 정보가 포함된 간단한 텍스트 파일입니다.

example.com.txt

```js
;;
;; Domain:     example.com.
;; Exported:   2022-09-28 23:47:34
;;
;; 이 파일은 정보 제공 및 기록용으로만 사용되도록 되어 있으며
;; 실제 운영 DNS 서버에서 사용하기 전에 반드시 편집해야 합니다.
;; 특히 다음 사항을 업데이트해야 합니다:
;;   -- 올바른 권위 있는 이름 서버로 SOA 레코드 업데이트
;;   -- 연락처 이메일 주소 정보로 SOA 레코드 업데이트
;;   -- 이 도메인을 위한 권위 있는 이름 서버로 NS 레코드 업데이트
;;
;; 자세한 내용은 BIND 설명서를 참조하십시오
;; 다음 웹 사이트에서 찾을 수 있습니다:
;;
;; http://www.isc.org/
;;
;; 그리고 RFC 1035:
;;
;; http://www.ietf.org/rfc/rfc1035.txt
;;
;; 사용 시 주의하십시오.
;; SOA 레코드
example.com 3600 IN SOA example.com root.example.com xxxxxxx xxxx xxxx xxxxx xxxx
;; A 레코드
example.com. 1 IN A 10.20.100.20
;; CNAME 레코드
www.example.com. 1 IN CNAME test.cdn.com.
;; MX 레코드
example.com. 3600 IN MX 5 gmr-smtp-in.l.google.com.
;; TXT 레코드
example.com. 1 IN TXT "v=spf1 include:_spf.google.com ~all"
www.example.com. 1 IN TXT "yandex-verification: xxxxxxxxxxxxx"
```

## DNS 레코드:

<div class="content-ad"></div>

DNS 레코드는 DNS 서버의 Zone 파일에 생성 및 저장된 지침입니다. 이러한 레코드는 도메인 및 호스트 이름에 대한 중요하고 관련성 있는 세부 정보를 제공합니다. 또한 DNS 서버가 DNS 요청을 처리하는 방법에 대한 다양한 명령을 포함하고 있습니다.

DNS 관리자는 도메인의 DNS 레코드를 관리하기 위한 포털을 제공합니다.

자주 사용되는 DNS 레코드 유형:

A 레코드 — FQDN(전체 자격 이름 도메인)을 IPv4 주소로 매핑하는 데 가장 일반적으로 사용되며 도메인 이름(예: www.albinsblog.com)을 IP 주소(예: 10.20.100.20)로 변환하여 번역기 역할을 합니다.

<div class="content-ad"></div>

루트 도메인(예: albinsblog.com — @로 표시)이나 다른 서브도메인(e.g., www.albinsblog.com, test.albinsblog.com)에 대해 매핑을 활성화할 수 있습니다.

![이미지](/assets/img/2024-07-13-DNSManagementBasicsforWebDevelopers_3.png)

![이미지](/assets/img/2024-07-13-DNSManagementBasicsforWebDevelopers_4.png)

AAAA 레코드는 IPv6 주소에 해당하는 도메인 이름을 해석합니다.

<div class="content-ad"></div>


![DNS Management Basics for Web Developers](/assets/img/2024-07-13-DNSManagementBasicsforWebDevelopers_5.png)

CNAME — CNAME 레코드는 한 이름을 다른 이름으로 별칭 지정하는 데 사용할 수 있습니다. CNAME은 Canonical Name의 약자입니다.

예를 들어, albinsblog.com 및 www.albinsblog.com은 동일한 응용 프로그램을 가리키고 동일한 서버에 호스팅되어 있습니다. 두 개의 다른 A 레코드 레코드를 유지하는 것을 피하기 위해 다음과 같이 생성하는 것이 일반적입니다:

- albinsblog.com을 서버 IP 주소를 가리키는 A 레코드
- www.albinsblog.com을 albinsblog.com을 가리키는 CNAME 레코드


<div class="content-ad"></div>

다른 경우는 있습니다; 하위 도메인이 동적 IP 또는 다중 IP가 활성화된 서버를 가리켜야 할 경우, 예를 들어, CDNs의 경우; 이 경우에는 필요한 IP를 지시하는 도메인을 가리키는 CNAME 레코드를 활성화하는 것이 합리적입니다.

![image](/assets/img/2024-07-13-DNSManagementBasicsforWebDevelopers_6.png)

같은 이름에 대한 다른 레코드와 CNAME 레코드가 공존할 수 없습니다. www.albinsblog.com에 대해 CNAME 및 TXT 레코드를 동시에 가질 수 없습니다.

기본 동작은 최상위 루트 도메인인 albinsblog.com을 CNAME 레코드가 아닌 A 레코드에만 가리킬 수 있다는 것입니다.

<div class="content-ad"></div>

외부 CDN을 통해 웹 사이트를 호스팅하는 동안 CNAME 포인팅을 필요로 하는 경우가 있습니다. 메인 웹 사이트는 서브도메인에 호스팅될 것입니다. 예를 들어 www.albinsblog.com(www가 CDN 도메인을 가리키도록 CNAME 포인팅)이고, 최상위 도메인은 A-Record(IP 주소)을 통해 서버/서비스를 가리켜야 합니다. 서버/서비스는 최상위 도메인을 서브도메인으로 리디렉션할 수 있습니다(albinsblog.com에서 www.albinsblog.com으로); 대부분의 경우 DNS 관리자는 루트 또는 특정 서브도메인을 특정 대상 URL로 리디렉션하기 위한 포워딩 서비스를 제공합니다.

최근의 CNAME 플랫닝 개념은 일부 DNS 관리자에서 지원됩니다. Cloudflare와 같은 업체는 최상위 또는 루트 도메인을위한 CNAME 레코드를 활성화할 수 있도록 합니다.

또한 ANAME(별칭 또는 가상 레코드 유형이라고도 불리는)과 같은 비표준 레코드 유형은 GoDaddy, Google Domain 등 일부 DNS 제공업체에서 지원됩니다. 이 레코드 유형은 최상위(루트) 도메인을 DNS에 가리키도록 하는 CNAME과 유사한 동작을 제공합니다.

TXT 레코드- TXT 레코드는 도메인 외부 소스를 위한 텍스트 정보를 담고 있는 유형의 DNS 레코드입니다. TXT 레코드는 SSL 인증서 생성을 위한 도메인 소유권 확인, Google Analytics를 위한 도메인 소유권 확인, SEO 도구를 위한 도메인 소유권 확인 등 다양한 목적으로 사용될 수 있습니다.

<div class="content-ad"></div>


![DNS Management Basics for Web Developers](/assets/img/2024-07-13-DNSManagementBasicsforWebDevelopers_7.png)

SPF(Sender Policy Framework) 레코드는 이메일 인증을 위해 일반적으로 사용되는 DNS TXT 레코드 유형입니다. SPF 레코드에는 해당 도메인에서 이메일을 보낼 수 있는 IP 주소 및 도메인 목록이 포함됩니다.

SOA — DNS 'start of authority (SOA)' 레코드에는 도메인 또는 존에 대한 중요한 정보가 저장되어 있습니다. 관리자의 이메일 주소, 도메인이 마지막으로 업데이트된 시기, 서버가 새로 고침 사이에 대기해야 하는 시간 등이 있습니다. 모든 DNS 존은 IETF 표준을 준수하기 위해 SOA 레코드가 필요합니다. DNS 관리자는 기본적으로 도메인에 대해 SOA 레코드를 활성화합니다.

MX — DNS 'mail exchange' (MX) 레코드는 이메일을 메일 서버로 안내합니다. MX 레코드는 어떻게 간단한 메일 전송 프로토콜이 이메일 메시지를 라우팅해야 하는지를 나타냅니다. CNAME 레코드와 마찬가지로 MX 레코드는 항상 다른 도메인을 가리켜야 합니다. 도메인에 대해 여러 개의 MX 레코드를 정의할 수 있으며, 우선 순위 번호가 선호도를 나타냅니다.


<div class="content-ad"></div>



![DNS Propagation:](/assets/img/2024-07-13-DNSManagementBasicsforWebDevelopers_8.png)

![DNS Propagation:](/assets/img/2024-07-13-DNSManagementBasicsforWebDevelopers_9.png)

## DNS 전파:

DNS 전파는 DNS 레코드의 업데이트가 웹의 모든 서버에 완전히 적용되는 데 걸리는 시간을 의미합니다. 변경 사항은 즉시 발생하지 않습니다. 이는 네임서버가 도메인 레코드 정보를 캐시에 저장한 후 새로 고칠 때까지 일정 시간 동안 웹 전체에 효과를 내지 않기 때문입니다.



<div class="content-ad"></div>

일반적으로 DNS는 몇 시간 이내에 전파되지만 최대 72시간이 걸릴 수도 있습니다. 전파 시간은 여러 요인에 따라 달라지며, 인터넷 서비스 제공업체(ISP), 도메인 레지스트리, DNS 레코드의 TTL(Time to Live) 값 등이 영향을 줍니다.

일반적으로 레코드의 TTL 값이 높은 경우는 거의 변하지 않는 레코드에 사용됩니다. 예를 들어 MX 또는 TXT 같은 레코드들이 그에 해당합니다. 높은 TTL 값은 정보를 로컬에 저장한 후 다시 검색함으로써 보다 정적인 자원에 대해 더 빠른 응답을 제공합니다.

자주 업데이트해야 하는 레코드는 TTL 값이 낮아야 합니다. TTL이 낮은 것은 주요 레코드에 권장되며 자주 변경됩니다. 적절한 범위는 30에서 300초(5분) 사이입니다.

![이미지](/assets/img/2024-07-13-DNSManagementBasicsforWebDevelopers_10.png)

<div class="content-ad"></div>

중요한 DNS 레코드를 수정하는 동안, 엔드 사용자에게 다운타임을 피하기 위해 적절한 계획이 필요할 수 있습니다. 레코드가 이미 높은 TTL로 활성화되어 있는 경우에는 실제 변경 전에 TTL을 낮은 값으로 변경하는 것이 좋습니다. 이렇게 하면 서버가 전파될 때까지 일부 사용자는 여전히 사이트의 캐시된 버전을 제공 받을 수 있습니다.

DNS 전송을 새로운 등록기관으로:

가끔 우리는 도메인을 다른 등록기관으로 전송해야 할 수도 있습니다. 도메인 전송은 도메인 이름을 한 등록기관에서 다른 등록기관으로 변경하는 것을 의미합니다. 전송을 받으려면 ICANN에서 60일 변경 등록자 잠금을 시행하므로 현재 등록기관과 최소 60일 동안 있어야 합니다.

<div class="content-ad"></div>

한 번 이체를 확정지으면 현재 등록기관에서 도메인을 잠금 해제하여 이체를 시작할 수 있습니다. DNS 관리자는 포탈을 통해 이 구성을 지원합니다. 또한 현재 등록기관에서 이체 AUTH 토큰을 생성하세요. 새로운 등록기관은 이 AUTH 토큰을 통해 이체를 시작할 수 있습니다.

![DNSManagementBasicsforWebDevelopers](/assets/img/2024-07-13-DNSManagementBasicsforWebDevelopers_11.png)

기존 등록기관에서의 DNS 레코드는 새로운 등록기관으로 이전됩니다. 혹시 도메인이 현재 등록기관에서 사용 중인 기본 이름 서버를 사용 중일 경우, 이 이름 서버 포인트는 새로운 등록기관의 기본 이름 서버로 이전되어야 합니다. 또한 새로운 등록기관이 현재 등록기관이 지원하는 특별 서비스(예: 도메인 포워딩)를 지원하는지 확인하세요.

## DNS 보안:

<div class="content-ad"></div>

필요한 DNS 보안 기능을 활성화하면 DNS를 보호하여 DNS 스푸핑/캐시 변조, DNS 하이재킹과 같은 보안 공격으로부터 보호할 수 있습니다.

DNS 오버 HTTPS(TLS) — DNS 쿼리는 평문으로 전송되어 누구나 읽을 수 있는 상황입니다. DNS 오버 HTTPS 및 DNS 오버 TLS는 DNS 쿼리와 응답을 암호화하여 사용자 브라우징을 안전하고 개인 정보 보호를 유지합니다. 자세한 내용은 DNS 오버 TLS 대 DNS 오버 HTTPS | 안전한 DNS | 클라우드플레어에서 확인할 수 있습니다.

DNSSEC — DNS 시스템은 초기에 보안을 고려하여 구축되지 않았습니다. DNSSEC는 공개 키 암호화를 기반으로 디지털 서명을 사용하여 DNS에서 인증을 강화합니다. DNSSEC를 통해 DNS 쿼리 및 응답을 암호화하는 것이 아니라 데이터 소유자가 DNS 데이터 자체에 서명하는 방식을 사용합니다. DNSSEC를 통해 중요한 도메인을 보호하여 보안 공격을 방지합니다. DNSSEC에 대한 자세한 내용은 클라우드플레어의 How DNSSEC Works | Cloudflare를 참고하세요.

도메인 잠금 — 이전에 설명한 대로, 도메인 잠금은 무단 이체를 방지합니다.

<div class="content-ad"></div>

## 도구:

DNS 작업 시 도움이 되는 일부 도구들

- WHOIS Lookup — 도메인 이름의 현재 등록기관 세부 정보, 네임 서버 등을 조회하는 기능을 제공합니다; 예: WHOIS | 도메인 이름 가용성 조회 — GoDaddy

- DNS checker 도구, 예: DNS Checker — DNS 확인 전파 도구, 서로 다른 위치에서 DNS 전파 상태를 확인합니다.

<div class="content-ad"></div>

nslookup/dig은 도메인 네임 시스템 (DNS)을 쿼리하는 데 사용되는 명령줄 도구로, 도메인 이름과 IP 주소 또는 기타 DNS 레코드 간의 매핑을 얻을 수 있습니다.

예시:

```js
nslookup example.com
nslookup -q=CNAME www.example.com
nslookup -type=MX example.com
dig example.com
dig example.com MX
```

온라인 DNS Lookup은 특정 도메인의 DNS 레코드를 보기 위해 DNS Lookup — Check DNS Records (dnschecker.org)와 같은 온라인 DNS lookup 도구를 사용할 수 있습니다.

<div class="content-ad"></div>

원문: https://www.albinsblog.com 에서 게시되었습니다.