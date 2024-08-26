---
title: "비밀을 나누는 방법 Shamirs Secret Sharing Scheme으로 알아보기"
description: ""
coverImage: "/assets/img/2024-08-26-HowtoSplitaSecret_0.png"
date: 2024-08-26 17:30
ogImage: 
  url: /assets/img/2024-08-26-HowtoSplitaSecret_0.png
tag: Tech
originalTitle: "How to Split a Secret"
link: "https://medium.com/@rohitg5249/how-to-split-a-secret-d92c0a57b62e"
isUpdated: false
---


상상해보세요. 당신이 부자 억만장자라고 상상해봅시다. 10명의 가족 구성원 사이에 분배할 큰 금액의 돈이 있습니다. 이 돈은 비밀번호로 보호된 계정에 보관되어 있습니다. 모든 10명에게 비밀번호를 주는 대신 조건이 있습니다: 적어도 10명 중 6명 이상이 합의하고 모일 때에만 돈에 액세스할 수 있도록 하고 싶습니다. 10명의 가족 구성원에게 비밀번호를 어떻게 전달해야 하나요? 6명 이상이 모이지 않는 이상 원래 비밀번호와 돈이 있는 계정에 액세스할 수 없도록 할까요?

Adi Shamir은 Shamir의 비밀 분할(Secret Sharing) 기법으로 이를 가능하게 했습니다. 이 간단하지만 매우 강력한 기법이 다양한 상황에서 유용하게 활용됩니다. 실제로 Shamir의 시크릿 셰어링이 실제 세계에서 사용되는 몇 가지 사례를 살펴보겠습니다.

- Hashicorp Vault은 고도의 보안을 위해 비밀 키를 여러 부분으로 나누어 여러 기계에 분산하는 데 SSS를 사용합니다.
- DNSSEC 루트 키는 7명의 수호자에 의해 보유되어 루트 키를 복구하기 위해 5명 이상이 모여 결정하는 경우에만 검색될 수 있습니다(Shamir의 시크릿 셰어링을 사용하는 것으로 알고 있습니다).
- 실제로 데이터 저장 장치의 오류 검사(예: 저장 디스크에서)를 위해 사용되는 리드-솔로몬 오류 수정 코드도 동일한 기본 수학 기법을 사용합니다.

이 게시물에서는 이 기술 뒤에 있는 간단하지만 흥미로운 수학을 이해하고 구현하는 방법을 알아보겠습니다.

<div class="content-ad"></div>

# Shamir's Secret Sharing이란 무엇인가요?

2개의 점 (x1, y1) 및 (x2, y2)가 주어진다고 상상해보세요. 이 두 점을 연결하는 독특한 선의 방정식을 복구하는 것은 정말 쉽습니다.

이제 만약 하나의 점 (x1, y1)만 주어진다면 어떨까요? 딱 한 점만 주어졌을 때는 위의 선을 복구할 방법이 없습니다.

<div class="content-ad"></div>


![이미지](/assets/img/2024-08-26-HowtoSplitaSecret_1.png)

이제 동일한 개념을 차수 n의 임의의 다항식으로 확장합니다. 이 다항식을 복구하려면 최소 n + 1 점이 필요합니다. 이것이 Shamir의 비밀 공유 기술의 기초입니다.

Shamir는 비밀 값 'k'가 주어지면 상수 계수를 가진 다항식 F(x)로 인코딩할 수 있음을 언급했습니다. 't'는 상수항을 제외한 랜덤으로 선택된 계수입니다. 이는 차수 't'의 다항식을 제공합니다.

이제 이 다항식에서 'n'개의 점을 생성합니다.


<div class="content-ad"></div>

그것을 'n' 참가자들 사이에 분배합니다. 'n' 참가자 중 t + 1이 만나면이 점수를 사용하여 다항식 방정식을 복구하고 그 비밀을 검색할 수 있습니다(즉, 상수 항의 계수).

이것은 간단해 보이죠? 잘 알려진 이 n개의 점에 대해 임의의 'n'에 대한 원래 다항식 방정식을 실제로 어떻게 재구성하는 것인가요?

계속 읽어보세요.....

# 랑그랑지 보간법

<div class="content-ad"></div>

Lagrange는 곡선 상 적어도 t + 1개의 점이 주어지면 해당 't' 차 다항식을 구성하는 방법을 제공합니다. 실제로 이전 블로그 글에서 설명한 대로, 우리는 Hack The Box (HTB 2024) 챌린지를 해결하는 데 Lagrange의 보간법을 사용했습니다. 시간을 절약하기 위해 sage를 사용했지만, 여기에서는 실제로 수학을 이해하고 구현할 (sage를 사용하지 않는) 코드를 작성할 것입니다.

기본 아이디어는 제공된 각 점에 대해 라그랑지 기저 다항식을 생성하여 기저 다항식이 주어진 점에서 1로 평가되지만 다른 모든 점에서 0으로 평가되도록 하는 것입니다.

라그랑지 기저 다항식을 어떻게 생성할까요?

P0(x0) = 1임을 쉽게 확인할 수 있지만 다른 값 x(x1, x2, ... xn)에 대해 P0(x) = 0입니다. 마찬가지로 P1(x1) = 1이지만 다른 값 x(x0, x2, ... xn)에 대해 P1(x) = 0입니다.

<div class="content-ad"></div>

요기에 있는 표 태그를 Markdown 형식으로 변경해보세요.

<div class="content-ad"></div>

# 얼마나 안전한가요?

샤미르의 비밀 공유는 't' 중 'n' 비밀 공유가 공개될 때까지 정보 이론적 보안을 제공한다고 알려져 있습니다. 이러한 방식들은 무한한 컴퓨팅 자원과 시간을 갖춘 상대에 대해 안전합니다. 이것은 최근 NIST가 Post-Quantum 암호를 표준화하기로 발표한 것과 흥미로운 연관이 있습니다. 하지만 이에 대한 논의는 다음에... 기대해 주세요!

# 참고 자료:

- 비밀을 공유하는 방법: [링크](https://web.mit.edu/6.857/OldStuff/Fall03/ref/Shamir-HowToShareASecret.pdf)
- 라그랑주의 다항 보간: [링크](https://pages.cs.wisc.edu/~sifakis/courses/cs412-s13/lecture_notes/CS412_12_Feb_2013.pdf)
- 샤미르의 비밀 공유: [링크](https://en.wikipedia.org/wiki/Shamir%27s_secret_sharing)
- 리드-솔로몬 코드: [링크](https://en.wikipedia.org/wiki/Reed%E2%80%93Solomon_error_correction)
- 샤미르의 비밀 공유 그래프 참고: [링크](https://distributeddatastore.blogspot.com/2016/03/a-way-to-understand-shamirs-secret.html)
- 갈루아 필드: [링크](https://en.wikipedia.org/wiki/Finite_field)
- NIST 후언 양자 암호 및 서명 알고리즘: [링크](https://www.nist.gov/news-events/news/2024/08/nist-releases-first-3-finalized-post-quantum-encryption-standards)
- [링크](https://research.ibm.com/blog/nist-pqc-standards#-fn-5)
- [링크](https://www.schneier.com/blog/archives/2010/07/dnssec_root_key.html)