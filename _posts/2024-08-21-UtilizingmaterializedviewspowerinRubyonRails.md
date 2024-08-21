---
title: "Ruby on Rails에서 Materialized Views를 활용하는 방법"
description: ""
coverImage: "/assets/img/2024-08-21-UtilizingmaterializedviewspowerinRubyonRails_0.png"
date: 2024-08-21 18:07
ogImage: 
  url: /assets/img/2024-08-21-UtilizingmaterializedviewspowerinRubyonRails_0.png
tag: Tech
originalTitle: "Utilizing materialized views power in Ruby on Rails"
link: "https://medium.com/@michal.a.rudzki/utilizing-materialized-views-power-in-ruby-on-rails-bcd2332604bb"
isUpdated: true
updatedAt: 1724244463214
---


재료화된 뷰에 대해 여러 번 읽었습니다. RoR에서 어떻게 사용할지 여러 번 계획을 세웠습니다. 마침내 실천했습니다. 어떻게 만들고 사용하는지, 그리고 잠재력까지 나누고 싶습니다.

![](/assets/img/2024-08-21-UtilizingmaterializedviewspowerinRubyonRails_0.png)

# 기본

## SQL 뷰란 무엇인가요?

<div class="content-ad"></div>

SQL에서 View는 SELECT 쿼리의 결과를 기반으로 하는 데이터베이스의 가상 테이블입니다. 열에 대한 수학 연산 결과, 여러 테이블에서의 집계 데이터 또는 열 필터링과 같은 더 복잡한 데이터를 보유할 수 있습니다. 실제로 데이터를 보유하지는 않지만 필요할 때 쿼리를 실행합니다.

## SQL 메테리얼라이즈드 뷰란 무엇인가요?

메테리얼라이즈드 뷰는 단순히 데이터베이스에 저장되는 뷰입니다. 미리 정의된 쿼리를 실행하는 것이 아닌 새로 고칠 때 실행되고 그 결과를 일반 SQL 테이블처럼 보관합니다.

두 메커니즘 모두 매우 강력하고 유용합니다.

<div class="content-ad"></div>


![이미지](/assets/img/2024-08-21-UtilizingmaterializedviewspowerinRubyonRails_1.png)

# 설정

저는 pets_app을 테스트 용도로 사용했습니다. 먼저 실제 materialized view를 생성해야 합니다. rails migration bundle exec rails g migration create_materialized_view_pet_statistics 명령어를 사용해 materialized view를 생성할 수 있습니다.

이제 migration 내용을 정의해야 합니다. 단순히 DB에 있는 각 종류의 애완동물이 얼마나 있는지 데이터를 얻고 싶습니다:


<div class="content-ad"></div>

```js
      dir.up do
        execute <<-SQL
          CREATE MATERIALIZED VIEW pet_statistics
          AS
          SELECT kind, COUNT(*) AS pet_count
          FROM pet_strings
          GROUP BY kind;
        SQL
      end
      dir.down do
        execute <<-SQL
          DROP MATERIALIZED VIEW pet_statistics RESTRICT;
        SQL
      end
```

이렇게 하면 materialized view가 생성되어 채워집니다.

## 얼마나 걸릴까요?

내 간단한 선택과 소스 테이블에 1000만 개의 레코드가 있을 때, 약 700ms가 소요됩니다.

<div class="content-ad"></div>

## 이제 이 데이터에 액세스해야 합니다.

저는 루비 개발자로, 루비 메커니즘을 사용하는 것을 좋아합니다. 그래서 우리는 모델을 만들 수 있습니다:

```js
class PetStatistic < ApplicationRecord
  self.primary_key = 'kind'

  def readonly?
    true
  end

  def self.refresh
    ActiveRecord::Base.connection.execute('REFRESH MATERIALIZED VIEW pet_statistics')
  end
end
```

Id 열이 없기 때문에 kind를 primary_key로 사용했습니다. 항상 고유합니다.

<div class="content-ad"></div>

테이블 태그를 수정하지 않도록 하기 위해 readonly 속성을 추가해주세요.

또한 간단한 새로고침 방법을 추가하고 싶은데요.

## 데이터 접근

이제 일반적인 루비 모델처럼 데이터에 접근할 수 있습니다 - PetStatistic.find(`cat`).pets_count

<div class="content-ad"></div>

## 데이터 새로 고침하기

PetStatistic.refresh가 얼마나 자주 실행되길 원하는지 설정하려면 cron 작업과 rake 작업을 설정할 수 있어요

![이미지](/assets/img/2024-08-21-UtilizingmaterializedviewspowerinRubyonRails_2.png)

# 제한사항

<div class="content-ad"></div>

먼저, 제가 보여드린 코드는 단순화된 예시입니다. 실제로 사용하기보다는 개념을 증명하는 용도로 제공한 것이죠. 데이터가 매우 한정된 enum 형식이기 때문에 통계에 사용되기 어렵습니다.

둘째로, 자료화된 뷰가 항상 최신 정보가 아닌 것으로 가정해야 합니다. 필요에 따라 매일, 매주, 매달 한 번씩 새로고침해야 합니다. 시간에 따른 데이터 추적에는 사용할 수 없습니다.

세 번째 문제는 공간을 차지하고 일반 SQL 테이블처럼 동작한다는 것입니다. 마법같은 것이 아닙니다. 일일이 데이터를 그룹화해야 하는 경우 테이블보다 자료화된 뷰에 더 많은 레코드가 생길 수 있습니다.

![UtilizingmaterializedviewspowerinRubyonRails_3.png](/assets/img/2024-08-21-UtilizingmaterializedviewspowerinRubyonRails_3.png)

<div class="content-ad"></div>

# 그 값어치가 있을까요?

사용 방식에 따라 다릅니다. 그러나 크거나 복잡한 요청에 대해서는 필수적입니다. 데이터에 접근하는 시간은 선형적입니다. 제 사용 사례에서는 약 0.3 밀리초가 걸리는 반면, 1000만 개의 레코드를 갖는 테이블에서 count를 사용하는 일반적인 사용시에는 약 180밀리초가 소요됩니다.
여기서 제 테스트 절차를 확인할 수 있어요.

![테스트 절차](/assets/img/2024-08-21-UtilizingmaterializedviewspowerinRubyonRails_4.png)

# 요약

<div class="content-ad"></div>

분석 데이터에 빠르게 액세스하고 싶다면 소량의 오차 마테리얼라이즈 뷰가 강력한 해결책입니다. SQL 메커니즘은 초보자 친화적이지 않지만 우리는 그것을 다룰 수 있고 레일스 내에 함께 포함시킬 수 있어요.