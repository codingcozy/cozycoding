---
title: "현대 데이터 스택에서 드러나는 3가지 주요 균열"
description: ""
coverImage: "/assets/img/2024-07-13-Thebig3cracksbeginningtoshowintheModernDataStack_0.png"
date: 2024-07-13 01:59
ogImage: 
  url: /assets/img/2024-07-13-Thebig3cracksbeginningtoshowintheModernDataStack_0.png
tag: Tech
originalTitle: "The big 3 cracks beginning to show in the Modern Data Stack"
link: "https://medium.com/@hugolu87/the-big-3-cracks-beginning-to-show-in-the-modern-data-stack-d4394ff7f6aa"
---


# 소개

벤처 캐피탈 업계에서 번들링과 언번들링에 관한 관찰이 있습니다.

이는 경제 사이클이 진행됨에 따라 소프트웨어 솔루션이 번들링이 되어 단일 소프트웨어 업체가 여러 솔루션을 제공하는 패턴을 거치고 있다는 관찰입니다 ("번들링"). 시장은 이 상태와 소프트웨어 솔루션이 포인트 솔루션을 제공하는 상태 사이를 오가고 있습니다 ("언번들링").

이는 투자와 경제 성장이 증가하는 요인과 더불어 성장하는 복잡성과 기술적 발전 등요소를 반영하고 있습니다. 기술이 향상되면 복잡성 증가로 이어져 많은 기회가 갑자기 열리게 되므로, 더 많은 포인트 솔루션 제공이 필요해질 수 있습니다. 이를 위한 좋은 예시로는 인공지능과 생성적 AI가 있습니다.

<div class="content-ad"></div>

데이터 산업은 교차로에 서 있습니다. 지난 5년 동안 데이터 공간의 공급업체들이 급속하게 증가했습니다. 여러 도구를 포함한 데이터 스택이 미래로 손꼽혔지만 이제 사람들은 이에 대해 의심하기 시작했습니다.

이 기사에서는 "현대 데이터 스택"에서 드러나는 세 가지 주요 결함과 우리에게 제시할 수 있는 기업가로서의 교훈을 다룰 것입니다.

# 첫 번째 결함 - 올인원 데이터 플랫폼

가장 먼저 주목할 점은 우리가 드디어 새로운 "올인원" 데이터 플랫폼 세대가 큰 폭으로 등장하고 있다는 사실입니다. 기업들이 Talend, Informatica PowerCentre, Matillion과 같은 완전한 데이터 플랫폼을 사용해왔던 것과는 달리, 이제 기업들은 "Databricks와 GCP로 이전하고 있습니다." MDS 전체를 건너뛰고 새로운 올인원 플랫폼으로 전환하고 있습니다.

<div class="content-ad"></div>

Microsoft Fabric 및 Snowflake의 Snowpark와 Polaris 주변의 발표는 데이터 웨어하우스 / 컴퓨트 엔진이 데이터 및 AI 공간의 모든 부분을 섭렵하려는 모습을 정착시킵니다.

물론 지금까지 Microsoft, Snowflake, Databricks 등 다른 업체들과 항상 협력해 왔습니다. 특히 데이터를 가져오는 것에 초점을 맞춘 업체들과 BI 도구들에 관심을 갖는 업체들입니다.

Databricks의 여름 발표인 Lakeflow (제가 지칭한 것)과 AI/BI는 이러한 양상이 변화 중임을 보여줍니다. Databricks와 같은 기업들은 더 이상 파트너 네트워크를 통해 이익을 창출하려는 것이 아니라, 모든 것을 섭렵하고 "올인원" 데이터 플랫폼이 되려는 모습을 보입니다.

[이미지](/assets/img/2024-07-13-Thebig3cracksbeginningtoshowintheModernDataStack_0.png)

<div class="content-ad"></div>

이는 우리가 다시 묶는 중이라는 비즈니스 사이클의 변화의 증상입니다.

## 통합으로부터 기업가를 위한 교훈

데이터 파이프라인을 관리하기 위한 핵심 솔루션을 구축 중인 입장에서는 묶음이 환영받지 않을 것 같습니다.

왜냐하면 이미 Orchestration 엔진이 있을 때 Orchestration 엔진이 필요한가요? Databricks에는 워크플로, Snowflake에는 태스크가 있기 때문이죠. BigQuery에는 DataForm과 BigQuery 워크플로가 있습니다.

<div class="content-ad"></div>

건반별로 확장 가능한 Databricks 코드와 워크플로 코드에 내장된 편하거나 일반적인 부분이 엄청나다는 걸 경험을 통해 알았어요. 데이터를 관리하는 기본적인 방법으로 데이터 창고(Warehouses)가 될 수도 있지만, Orchestra는 데이터 파이프라인을 관리하기 위한 방법으로 만들어지고 있어요.

저는 이 분야에 아직 많은 기회가 있다고 생각해요. 그래서 배울 점은, 쉽게 묶일 수 있는 것을 만들지 말라는 거에요.

# 두 번째 문제 — 형편없는 지원과 버그가 많은 서비스

창업 초기 단계의 작은 기업이 CEO가 슬랙 질문에 몇 분 내에 답변하는 것으로 놀라운 지원을 제공한다는 소식을 얼마나 자주 들어요?

<div class="content-ad"></div>

대기업에서 이런 일이 벌어지는 것을 얼마나 자주 듣나요? 훨씬 적게요!

대기업은 최고의 지원을 받는 기업들을 위해 최선을 다하면서 시간이 흐름에 따라 지원이 악화되는 것 같습니다.

우리는 최근 5년 동안 항상 여정 초반에 있는 기업들과 함께 기뻤습니다. 고객 지원은 꽤 좋았고 제품 품질도 좋았어요 (적어도 대안들과 비교해서요).

그러나 이제 많은 변화가 있었습니다. 특정 업체를 지목하는 걸 좋아하지는 않지만, Fivetran은 최근 지원이 좋지 않다는 소문을 많이 듣고 있어요. 저도 몇 년간 그런 부분을 조금 본 적이 있어요. dbt Labs에서는 dbt-core이 거의 지원을 받지 않고 있어요. 이는 기업들이 어려운 시기를 맞이하고 있어서 "비핵심" 활동을 줄이고 있다는 점을 보여줍니다. 안타깝게도 지원은 보통 처음으로 타격을 받는 부분 중 하나입니다.

<div class="content-ad"></div>

서비스도 버그가 더 늘어나는 경향이 있습니다. 예를 들어, Fivetran은 이상한 일을 하곤 합니다.

다른 이야기 하나는 Snowflake입니다. 예를 들어, 지금 LLM Functions가 "일반적으로 이용 가능" 상태입니다 — 이건 "누구나 사용할 수 있다"는 말인 줄 알았는데, 실제로는 "귀하의 지역에서 지원되는 경우에만 사용 가능"을 의미합니다. 이것이 꽤 괜찮다고 생각하지만, 이런 것들이 발표되었을 때 완전히 전파되지 않는다는 것을 깨닫게 됩니다.

![이미지](/assets/img/2024-07-13-Thebig3cracksbeginningtoshowintheModernDataStack_1.png)

물론 이건 주관적인 견해일 수 있습니다. 그러나 저는 더 나빠지는 지원과 완전하지 않은 제품, 완전한 출시가 점점 줄어드는 양상을 보고 있습니다. 이는 에너지가 고갈된 시스템을 연상케 합니다.

<div class="content-ad"></div>

## 나쁜 지원 사례에서 CEO에게 교훈

MDS와 같은 기업이 이 위치에 있는 이유 중 하나는 상당히 큰 가치평가액으로 자금을 조달했다는 점입니다. Snowflake도 상장되어 있으므로 공개 회사의 모든 압박을 받고 있습니다.

이는 수익과 이익을 동시에 얻어야 한다는 압력을 의미합니다. 이는 치열한 경쟁 속에서 꼭 모서리를 깎게 만들 수 있습니다. 이런 위치에 있는 리더들에게 많은 동정을 표합니다.

우리에게 주는 교훈은 시기에 따라 자금을 조달하는 것이 무엇을 의미하는지 알고 있어야 한다는 것입니다.

<div class="content-ad"></div>

![Thebig3cracksbeginningtoshowintheModernDataStack_2.png](/assets/img/2024-07-13-Thebig3cracksbeginningtoshowintheModernDataStack_2.png)

2021년을 지내는 사람들에게는 이 돈을 잘 활용하고 거기서 상당한 수익을 올리는 것을 의미합니다. 현호를 타고 있는 분들에게는 그 현금으로 불타고 있는 즉각적인 수익을 창출하고 있지만, 오늘 같은 시기에 있는 사람들에게는 더 가볍게 운영하거나 더 많은 자금을 조달하기 위해 다른 사람들이 고생하는 사이에 제품-시장 적합성(Product-Market-Fit)을 확보하여 자본을 확보하는 것을 의미합니다.

# 세 번째이자 마지막 쇄골 - 경쟁 압력

Snowflake는 생태계 플레이어로서 말은 잘하는 편이며 특히 지난 4년간 큰 이슈였습니다.

<div class="content-ad"></div>

적당한 규모의 스노우플레이크 행사에서 Fivetran, Sigma, dbt 등의 영업 대표들이 가득차지 않는다면 드물게 어려움을 겪을 것입니다. 몇몇 파트너들은 추천으로부터 거래의 거의 60%가 유입된다고 보고했는데, 이는 엄청난 채널입니다.

그러나 경쟁 압력이 이러한 관계에 부담을 주고 있습니다.

우선, 수익이 필요한 회사들이 파트너들의 영토로 밀어넣는 것을 볼 수 있습니다. 스노우플레이크 Tasks는 dbt의 직접적인 경쟁 상대입니다. 스노우플레이크 인게스트는 Fivetran이나 Airbyte와 직접 분쟁하고 있습니다.

이러한 현상은 전 분야에 걸쳐 보이는데, Fivetran이 dbt 변환(매우 잘못된 기능)을 추가하고, BI 도구가 의미론적 레이어(예: Lightdash)로 이동하며 주로 데이터 웨어하우스에서 모든 것을 구축하고 있는 것을 볼 수 있습니다(Google Workflows, Google Catalog, Google Vertex AI, Dataplex).

<div class="content-ad"></div>

현재 하위 업체가 많아서 경쟁 압력이 점점 더 커지고 다른 공급 업체들이 동일한 서비스를 제공하기 시작했습니다. 가장 강한 자만이 살아남을 것이 분명한 상황입니다.

## 리더로서 경쟁 압력을 극복하는 방법

실제로 여기서 배울 수 있는 교훈은 그리 많지 않다고 생각합니다. 함께 뭉치는 것에는 명백한 상업적인 힘이 있었지만, 시간이 지나면 제품 로드맵이 충돌하고 있다는 점을 알게 되었습니다. 판매 팀 자체는 아니었지만요.

그래서 여기서의 교훈은, 좋은 시기가 끝나면 최고에 있어야 한다는 것입니다.

<div class="content-ad"></div>

Snowflake, Google, and other companies have little to lose by entering their competitors' fields, as long as they invest in new features and roll them out effectively.

This can be a serious threat to startups that focus on just a small part of the market.

In times of growth, it's important to aim for big moves. When faced with competition, remember it's not personal— it's part of the industry landscape. Everyone's dealing with the same challenges.

In conclusion, the data landscape is constantly evolving. 🚀

<div class="content-ad"></div>

요즘에 우리가 빠르게 변화하는 데이터 환경 속으로 다시 돌아온 것에는 놀라지 않겠죠.

시장 역학을 채우기 위해 많은 솔루션과 도구들이 빠르게 제시되었지만, 그들도 곧 사라져갈 것입니다. 아마도 NVIDIA의 주가와 관련이 있을지도 모르겠네요.

우리는 MDS의 많은 속성이 그 시대나 ZIRP 시대의 기능에 따른 것으로 보고 있습니다; 많은 포인트 솔루션, 높은 수준의 파트너 협력, 매우 높은 투자 수준, 오픈 소스에서의 마케팅 부상 등이 있습니다.

이제 더 이상 제로 퍼센트 이자율이 아니기 때문에, 경쟁 압력으로 인해 현재의 Modern Data Stack의 시장 구조에 균열이 나기 시작하고 있습니다.

<div class="content-ad"></div>

CEO 및 상업 리더들에게 훌륭한 교훈이 많이 있긴 하지만, 변화는 언제나 변함없이 계속 됩니다! 합병은 이미 시작되어 있으며, Rockset과 같은 서비스도 인수로 인해 완전히 사라지려는 시도를 하고 있습니다.

현재 데이터 분야에 있기에 흥미로운 시기입니다. 준비를 해두고 닭들을 세어봐야 할 것 같네요, 일이 꽤 일어날 것 같아요 🐔

Hugo Lu

Linkedin
Twitter