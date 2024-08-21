---
title: "Ahrefs가 90 할인으로 수십억 달러 인프라를 구축한 비결"
description: ""
coverImage: "/assets/img/2024-07-01-HowAhrefsGetsaBillionDollar-WorthInfrastructureWitha90Discount_0.png"
date: 2024-07-01 21:32
ogImage: 
  url: /assets/img/2024-07-01-HowAhrefsGetsaBillionDollar-WorthInfrastructureWitha90Discount_0.png
tag: Tech
originalTitle: "How Ahrefs Gets a Billion Dollar-Worth Infrastructure With a 90% Discount"
link: "https://medium.com/ahrefs/how-ahrefs-gets-a-billion-dollar-worth-infrastructure-with-a-90-discount-5edd473b2399"
isUpdated: true
---





싱가포르 데이터 센터에서 3년 동안 4억 달러 이상을 절감한 우리 글이 화제였습니다. 이 글에서는 850대의 동일한 서버가 AWS 대체품에 어떻게 비교되어 한 달간의 지출을 기준으로 설명되었습니다. 이제 우리는 시야를 넓혀서 Ahrefs가 콜로케이션 여정 시작부터 온프레미스 인프라 전체를 클라우드로 전환했다면 총 비용이 어땠을지 고려해보겠습니다.

역사적으로 Ahrefs는 콜로케이션에서 시작하여 OVH, SoftLayer, Hetzner 등의 호스팅 제공업체를 이용하게 되었습니다. 2017년에 회사는 콜로케이션으로 돌아가 그 이후 이 방식을 확대해왔습니다. 본 리뷰는 2017년 7월 1일부터 2023년 6월 30일까지 6년간의 실제 콜로케이션 관련 비용을 다루며, 데이터 센터에서 발생한 모든 지출을 월별로 그룹화하고 역사적인 월간 지출 그래프를 작성했습니다.

그래프의 하늘색 바에는 월별 일반적인 비용인 약정된 전력, 전력 소비, 인터넷 서비스 요금 및 랙, 전력 분배 시스템, 생체 인식 접근 등과 같은 일회성 비용을 포함한 콜로케이션 비용이 나타납니다. 어두운 파란색 바는 네트워크 및 서버 하드웨어 구매, 그에 따른 라이선스, 지원 및 설치 및 케이블링을 위한 전문 서비스를 나타냅니다.

2023년 초에 우리는 인프라에 대해 최대 단일월 투자를 약 $19백만에 하였습니다. 아래에서 해당 시간에 주문된 일부 서버를 살펴볼 수 있습니다.

<div class="content-ad"></div>

![image](/assets/img/2024-07-01-HowAhrefsGetsaBillionDollar-WorthInfrastructureWitha90Discount_0.png)

자, 이제 확대해서 2017년 이후 당사의 온프레미스 지출을 살펴보겠습니다. 매월 발생하는 지출 막대의 급격한 증가는 누적 지출 그래프에서 뚜렷한 증가와 일치합니다.

총적으로, Ahrefs는 2017년 이후 자사의 온프레미스 인프라를 지원하기 위해 1억 2,200만 달러를 지출했습니다. 이는 회사에게 주요 비용 요소가 되었습니다.

이제 이것을 월별로 사용한 서버 및 저장소에 대한 AWS 교체 비용과 비교해보겠습니다. AWS는 두 가지 주요 유형의 EC2 인스턴스를 제공합니다:

<div class="content-ad"></div>

- AWS 온디맨드: 이 옵션은 기본적이고 할인되지 않은 시간당 요금을 가장 유연하게 제공합니다.
- AWS 3년간 예약 인스턴스 선불: 이것은 3년의 약정과 선불 결제가 필요한 가장 비용 효율적인 선택지입니다.

우리가 서버용으로 EC2 인스턴스, 저장용으로 EBS, 그리고 기본 가정들을 선택한 방법에 대한 더 자세한 정보는 글의 끝에 있는 부록을 참조해주세요.

비교를 완전히 설명하기 위해 동등한 AWS 지출을 표시하기 위해 10배 확대해야 합니다.

놀랍게도, 두 AWS 옵션 모두 10억 달러 미만의 가격표를 얻을 수 없으며, Ahrefs는 비용을 약 10배 낮게 유지할 수 있었습니다. 클라우드 비용에 빠져들지 않고 남은 10억 달러로 어마어마한 가능성을 상상해보세요.

<div class="content-ad"></div>

- 페이스북이 인스타그램을 10억 달러에 인수한 것을 따라할 수 있는 회사가 있을까요?
- 혹시 10억 7000만 달러로 가치가 평가된 Air New Zealand을 인수하여 항공 산업에 진출할 수 있을까요?
- 혹시 15억 달러로 버즈 할리파처럼 기록을 세울만한 건축 기적을 건설할 수 있을까요?
- 아니면 더 실용적으로, 아마존과 비슷한 10억 달러에 1GW 원자력 데이터 센터를 구입하고 클라우드 제공 업체들에 임대하거나 재판매하여 이익을 올릴 수도 있지 않을까요?

재정 격차는 경고를 넘어서 깨달음의 기회입니다.

2017년 에아프스가 콜로케이션을 추구하는 대신 트렌디한 클라우드 솔루션을 택했다면, 오늘날 회사는 존재하지 않았을지도 모릅니다. 이익 창출은 물론이고 매번 클라우드 비용을 줄이기 위해 애쓰고, 클라우드에 특화된 최적화와 전략에 상당한 시간과 노력을 들일 것입니다. 그러나 위의 그래프에서 보여준 대로, Ahrefs의 인프라에서는 요금이 가장 비싼 온디맨드 서비스부터 "할인율 최대 60%에 달하는" 3년 선납 예약 인스턴스까지의 두 가지 최적화 방법이 장기적으로 큰 차이를 보이지 않는 것으로 나타납니다. 아마도 성능을 유지하기 위해 서버를 정기적으로 추가하고 수백 페타바이트의 빠른 SSD 스토리지를 활용하기 때문에 사전 예약 인스턴스의 할인 혜택이 드러나지 않을 수도 있나요?

하지만 알아봅시다. 만약 인프라를 현재 상태로 유지하고 미래를 전망해보자면 어떨까요? 우리가 이전과 똑같은 구성 요소를 추가하거나 제거하지 않고, 2023년 6월 30일 현재의 월별 비용을 계속 지불한다고 가정해봅시다. 전체 범위를 보기 위해 좀 더 확대해보겠습니다.

<div class="content-ad"></div>

서버 네이티브 방식으로 최적화하지 않은 거야!

어떤 사람들은 "사용한 만큼 지불하기" 원칙을 광범위하게 홍보하는 서버리스 아키텍처로의 전환을 지지해왔습니다. 이 방식은 자주 사용되지 않는 자원이나 가벼운 부하를 갖는 자원에 효과적일 수 있습니다. 그러나 아마존 프라임 비디오 팀은 AWS 서버리스에서 EC2 및 ECS 플랫폼으로의 전환이 자체적 으로 무거운 부하를 갖는 애플리케이션의 비용을 90% 절감할 수 있었다는 것을 입증했습니다.

<div class="content-ad"></div>

Ahrefs에서는 서버리스로 전환하는 아이디어가 설득력이 없다고 생각합니다. 이전에 논의한 850대의 서버는 매달 데이터 홀에서 이용 가능한 총 전력의 86-92%를 사용하여 높은 이용률을 보여주고 있습니다. 이러한 서버는 AWS EC2+EBS와 비교했을 때 비용이 약 1/10 정도 되며, Prime Video의 급격한 비용 절감을 활용함으로써 Ahrefs의 인프라 비용이 서버리스 환경에서 100배 이상 급증할 수 있음이 명백합니다.

또 다른 최적화 제안은 스팟 인스턴스의 사용이었습니다. 그러나 저희는 서버 부하가 계속 높으므로 일관된 안정적인 인스턴스가 필요하며 간헐적인 것이 아닙니다.

마지막 제안은 클라우드 공급업체들과의 큰 규모 할인 협상을 하는 것이었습니다. 이 할인 혜택이 재정적으로 타당하려면 최소 90%의 할인율이 필요하며 비용을 원래의 지출의 10%로 줄여야 합니다. 이 글이 도움이 되어 더 나은 설명을 하는 데 도움이 되기를 바랍니다.

# 그런데 사람들은 어떻게 될까요?!

<div class="content-ad"></div>

이전 글에는 다음과 같이 요약할 수 있는 많은 댓글이 달렸습니다:

"사람 비용은 당신의 계산에 포함되지 않았습니다! 실패하는 하드웨어를 지원하기 위해 다양한 기술을 가진 많은 사람이 필요합니다! 그들 없이 당신의 수학과 결론은 완전히 틀렸습니다!"

저희는 인력 비용을 계산에 포함시키지 않았는데, 그들이 전체적인 재정 결과에 큰 영향을 미치지 않기 때문입니다. Ahrefs의 모든 서버는 다양한 콜로케이션 데이터 센터, 호스팅 제공업체, AWS 클라우드에 걸쳐 효과적으로 11명의 소규모 팀에 의해 관리됩니다. 이 그룹은 인프라 및 SRE 엔지니어, 데이터 센터 기술자, 그리고 이 글의 저자로 구성되어 있습니다. 이러한 숙련된 전문가들은 다양한 환경에서 여러 해 동안 서버, 네트워크, 운영 체제 및 다중 소프트웨어 시스템을 관리, 유지, 업그레이드 및 문제 해결해 왔습니다.

2023년 6월에는 검토 기간 중 최대 서버 수를 가졌습니다. 그 달, 저희 팀은 3300대 서버에서 94건의 하드웨어 문제에 대응하여 매일 평균 약 네 건의 문제를 처리하는 능력을 보여주었습니다.

<div class="content-ad"></div>

![How Ahrefs Gets a Billion-Dollar-Worth Infrastructure With a 90% Discount](/assets/img/2024-07-01-HowAhrefsGetsaBillionDollar-WorthInfrastructureWitha90Discount_1.png)

But hey, let's consider the idea that Ahrefs may not be managing things optimally. Picture a scenario where we have to significantly increase our workforce to maintain our 3300 physical servers, which are not cloud-based and are prone to issues. The theory is that by expanding our team to manage colocation servers, cloud solutions could become more beneficial compared to our current setup.

Imagine putting together a comprehensive team for server hardware support. This team would consist of DevOps specialists, hardware engineers, data center technicians, solution architects, NOC and SOC staff for network and security operations, along with procurement, project, capacity, asset, delivery, sustainability, and diversity managers. Add in team leads, vice presidents, senior vice presidents, recruiters, and HR business partners to oversee. Let’s say we overlook some essential roles and round up the hiring to 120 professionals solely focused on server support. Considering our company had around 120 employees as of June 2023, this hiring spree would double the company's size.

Now, let’s be generous and say these new hires earn an average annual salary of $500,000 each. So, the total annual cost for this "dream team" would amount to $60 million, reaching $360 million over six years. Interestingly, this figure is triple what we've spent on our entire infrastructure so far. Well, that's how the math adds up.

<div class="content-ad"></div>

이제 우리가 $122백만의 인프라 비용과 이 정말 비싼 팀을 합치면 합계가 $482백만이 됩니다. 이 광적으로 비싼 그리고 과도하게 자격이 높은 가상의 팀은 갖춰진 모든 기술을 가지고 있지만, 우리의 하드웨어를 소유하는 비용은 가장 저렴한 클라우드 옵션의 절반 비용에 불과합니다. 물론, 이에는 인력 비용이 포함되어 있지 않습니다. 클라우드 서비스도 인간의 감독을 필요로 합니다만, 우리는 우리의 계산에서 제외했습니다.

우리가 코로케이션 설정을 지원하기 위해 스태프를 강화하는 것이 재정적으로 어렵고 클라우드가 더 유리하다는 우리의 가설은 틀렸음을 확인했습니다.

우리의 클라우드, 호스팅 및 코로케이션을 처리하는 실제 팀은 훨씬 작아요. 여기에 덧붙여 확장을 위해 채용을 진행 중이니 곧 더 많은 서버를 처리할 수 있을 거에요.

# 결론

<div class="content-ad"></div>

인프라에 대한 콜로케이션 선택은 Ahrefs에게 옳은 결정이었습니다. 지난 6년을 되돌아볼 때, 우리의 자사 서버와 네트워크를 갖춘 데이터 센터는 1억 2천만 달러가 들었는데, 이 금액은 가장 저렴한 AWS 클라우드 옵션을 선택했다면 견자흔한 110억 달러에 이르렀을 것입니다. 강력한 온프렘 서버 덕분에 우리 개발자들은 제품을 향상시키는 데 집중할 수 있었고, 클라우드 비용을 최적화하느라 어렵게 지치지 않았습니다.

마음을 잘못 이해하지 마세요. 특히 AWS 같은 클라우드 기술은 빠른 배포에 뛰어나므로 프로토타입 생성, 신속한 테스트, 또는 글로벌 엣지 인프라 배포와 같은 긴급한 프로젝트를 위한 이상적인 선택입니다. 또 다른 중요한 장점은 시간과 위험을 최소화할 수 있는 능력으로, 즉시 서버 접속을 제공하며 물리적 설치의 관리절차와 지연을 피할 수 있습니다. 새 서버나 데이터 센터를 기다릴 수 없다면 클라우드는 여전히 유효한 옵션일지도 모릅니다.

클라우드 서비스 제공업체들은 클라우드를 단일로서 유일한 인프라 솔루션으로 강력하게 마케팅하면서 제품 비교를 복잡하게 하고 지각된 이점을 강조합니다. 그러나 콜로케이션 인프라와의 장기 비교를 통해 지속적인 클라우드 사용이 지나치게 비쌉니다. 이는 보편적인 해결책으로서의 적합성을 의심시킵니다.

아마 기업에게 클라우드는 사람에게 약물과 같이 고려되어야 할까요? 약물로서 클라우드는 단기적이고 좁은 적용 분야를 위해 극도로 유용할 수 있으며, 기업의 성과를 향상시키고 큰 이득을 가져올 수 있습니다. 그러나 장기적으로 너무 많은 비용을 소진할 수 있으며, 기업에 해를 끼칠 수 있고 파산으로 이어질 수도 있습니다. 어떤 약물이나 클라우드를 적당량으로 책임 있게 사용하세요.

<div class="content-ad"></div>

## 부록. 가정과 계산

우선, 우리는 콜로케이션 데이터 센터에서의 송장을 통합하고 분석했습니다. 그런 다음 비교를 위해 가능한 최대한 AWS와 가장 비슷한 서비스를 선택했습니다. EC2 및 EBS 서비스의 가격은 검토 기간의 끝부분부터 AWS 요금을 사용했습니다. 우리는 Ahrefs 서버의 위치와 가장 가까운 지역 또는 가장 가까운 지리적 대체물을 선택하여 가격을 결정했습니다. 그런 다음 매달 우리 데이터 센터의 특정 서버 수에 기초하여 AWS 대안의 금액을 계산했습니다.

저희 서버에 적합한 EC2 대안을 찾는 것은 항상 쉬운 일이 아니었습니다. 우리 서버 대부분은 3세대마다 듀얼 AMD 64코어/128스레드 CPU, 1.5TB 또는 2TB의 RAM 및 복수의 15TB NVMe SSD 드라이브로 구성되어 있습니다. AWS EC2에서 가장 가까운 옵션은 i3en.metal 인스턴스였으며, 주로 8개의 7.5TB NVMe SSD 드라이브로 선택되었습니다. 그럼에도 불구하고, 당사 서버 하나의 CPU, RAM 및 SSD 리소스와 일치시키기 위해서는 2배에서 3.5배의 i3en.metal 인스턴스가 필요했습니다.

저장 공간에 대해서는 가능한 경우 EC2 인스턴스 내부 SSD 및 HDD를 사용하고 추가 스토리지 필요 사항은 가장 비용 효율적인 AWS gp2(SSD) 및 sc1(HDD) 스토리지 옵션으로 보충했습니다. 우리는 용량을 기반으로 드라이브를 매칭하고 IOPS 비용과 AWS EBS 스토리지의 상대적으로 느린 성능을 무시했습니다. 예를 들어, AWS gp2 볼륨의 처리량은 250MB/s로 제한되어 있으며, 이는 PCIe Gen3 및 Gen4 NVMe SSD 드라이브의 일반적인 3-7GB/s 속도와 비교했을 때 상당히 느립니다.

<div class="content-ad"></div>

아래 표는 AWS에 대한 추가 고려 사항과 혜택을 설명하고 있습니다. 이러한 부분들은 정확하게 측정하기 어려워서 우리가 감안하여 계산했습니다. EC2와 EBS만을 고려한 AWS의 최소 가치를 추정했으나 실제 수치는 더 높을 것으로 예상됩니다.


![링크](/assets/img/2024-07-01-HowAhrefsGetsaBillionDollar-WorthInfrastructureWitha90Discount_2.png)


AWS 서버 비용은 서버가 전달된 시점부터 시작됩니다. Ahrefs가 서버를 처음 구입하고 수령을 기다리던 시간은 고려하지 않습니다. 예를 들어, 2023년 3월과 4월에 구매한 서버들은 2023년 8월에야 수령되었습니다. 따라서, 우리 분석에서의 동등한 AWS 지출은 2023년 8월부터 시작됩니다.


![링크](/assets/img/2024-07-01-HowAhrefsGetsaBillionDollar-WorthInfrastructureWitha90Discount_3.png)

<div class="content-ad"></div>

최근 피드백에 따르면, 3년 예약형 EC2 인스턴스에서 “모두 선결제”라는 용어가 혼란스러울 수 있다는 지적이 있었습니다. 이러한 인스턴스는 전액을 지불하지만 일반적으로 내부 저장 공간이 충분하지 않아 EBS에 대한 추가 월간 지불이 필요합니다. 따라서 위의 그래프에서 베타 각도가 알파보다 크게 나타나는 경우가 많습니다. 특히 저장 용량이 큰 서버를 포함할 때입니다.