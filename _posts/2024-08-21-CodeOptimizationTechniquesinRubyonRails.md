---
title: "루비 온 레일스의 코드 최적화 기술 "
description: ""
coverImage: "/assets/img/2024-08-21-CodeOptimizationTechniquesinRubyonRails_0.png"
date: 2024-08-21 18:02
ogImage: 
  url: /assets/img/2024-08-21-CodeOptimizationTechniquesinRubyonRails_0.png
tag: Tech
originalTitle: "Code Optimization Techniques in Ruby on Rails "
link: "https://medium.com/@rajputlakhveer/code-optimization-techniques-in-ruby-on-rails-084d4692dceb"
isUpdated: false
---


루비온 레일즈는 웹 개발을 간단하게 해주는 강력한 프레임워크입니다. 그러나 애플리케이션이 커지면 최적의 성능을 보장하는 것이 중요해집니다. 코드 최적화는 효율성을 향상시키고 응답 시간을 줄이며 사용자 경험을 향상시키는 데 도움이 될 수 있습니다. 이 블로그에서는 루비온 레일즈 코드베이스를 최적화하는 여러 기술을 살펴보고 성능 문제를 식별하고 해결하는 데 도움이 되는 유용한 젬을 소개할 것입니다. 🛠️

![이미지](/assets/img/2024-08-21-CodeOptimizationTechniquesinRubyonRails_0.png)

캐싱은 서버 부하를 줄이고 응답 시간을 빠르게 하여 응용 프로그램의 성능을 현저히 향상시킬 수 있습니다. 레일즈는 다양한 캐싱 메커니즘을 지원합니다:

- 페이지 캐싱: 페이지 전체를 캐싱하여 다시 렌더링하는 것을 피합니다.
- 액션 캐싱: 컨트롤러 액션의 결과를 캐싱합니다.
- 프래그먼트 캐싱: 자원을 많이 사용하는 뷰 일부를 캐싱합니다.
- 저수준 캐싱: Rails.cache를 사용하여 임의의 데이터를 캐싱합니다.

<div class="content-ad"></div>

테이블 태그를 Markdown 형식으로 바꾸어보세요.

| Example of fragment caching: |
|---|
| js |
| <% cache('popular_posts') do %> |
|   <%= render @popular_posts %> |
| <% end %> |
|  | 

Gems to Consider:

- dalli: A high-performance client for Memcached.
  - Gemfile gem `dalli`
- redis-rails: Integrates Redis as a cache store.
  - Gemfile gem `redis-rails`

<div class="content-ad"></div>

비효율적인 데이터베이스 쿼리는 애플리케이션을 느리게 만들 수 있어요. 여기 몇 가지 팁이 있어요:

- 콜럼 개수 제한을 위해 select 사용: 필요한 데이터만 검색하세요.

```js
User.select(:id, :name).where(active: true)
```

- N+1 쿼리를 피하세요: includes나 eager_load를 사용해 사전로드하세요.

<div class="content-ad"></div>

```js
# 최적화 이전
@posts = Post.all 
@posts.each do |post|
  puts post.author.name
end

# 최적화 이후
@posts = Post.includes(:author).all
```

- 자주 조회되는 열에 인덱스 추가: 검색 효율성 향상하기.

```js
add_index :users, :email
```

고려할 젬:


<div class="content-ad"></div>

- bullet: N+1 쿼리와 사용되지 않는 사용말파기를 감지하는 데 도움이됩니다.
Gemfile에 gem `bullet`을 추가하세요.
- rack-mini-profiler: 애플리케이션의 데이터베이스 쿼리에 대한 자세한 성능 프로파일링을 제공합니다.
Gemfile에 gem `rack-mini-profiler`을 추가하세요.

백그라운드 작업은 시간이 오래 걸리는 작업을 처리하는 데 중요합니다. 최적화하기 위해:

- 효율적인 작업 큐 백엔드 사용: 성능 및 확장성을 감안하여 Sidekiq을 고려해보세요.
- 작업 성능 모니터링: Sidekiq의 웹 UI와 같은 도구를 사용하여 작업을 관리하고 모니터링할 수 있습니다.
- 긴 작업 분해: 가능하다면 긴 실행 시간 작업을 더 작은 작업으로 분할하세요.

고려할 젬(Gem):

<div class="content-ad"></div>

- 사이드킥: 우수한 성능을 자랑하는 백그라운드 처리 도구.

# Gemfile 
gem `sidekiq`


- 사이드킥-크론: 반복 작업 스케줄링을 위한 도구

# Gemfile 
gem `sidekiq-cron`


로드 시간을 개선하기 위한 효율적인 자산 관리:

- 에셋 파이프라인 사용: Rails의 에셋 파이프라인은 CSS 및 JavaScript 파일을 컴파일하고 압축합니다.
- 자산을 최소화하고 압축: uglifier 및 sass-rails와 같은 젬을 사용하여 최소화합니다.

# Gemfile 
gem `uglifier`
gem `sass-rails`

- CDNs를 통해 에셋 제공: Content Delivery Networks (CDNs)를 사용하여 빠른 에셋 전송을 구현합니다.

고려해 볼 젬들:

<div class="content-ad"></div>

- webpacker: 웹팩을 활용하여 현대적인 JavaScript 및 CSS를 관리합니다.

# Gemfile gem `webpacker`


메모리를 효율적으로 관리하면 리소스 고갈을 방지할 수 있어요:

- 메모리 사용량 프로파일링: 메모리 핫스팟을 식별하기 위해 gem을 사용하세요.

# Gemfile gem `derailed_benchmarks`

- 메모리 누수 방지: 객체가 적절하게 관리되고 불필요하게 보존되지 않도록 확인하세요.

고려할만한 젬들:

<div class="content-ad"></div>

- memory_profiler: 애플리케이션의 메모리 사용에 대한 통찰을 제공합니다.
# Gemfile gem `memory_profiler`

잘 구조화된 코드는 성능을 향상시킵니다:

- Long Methods를 재구성하세요: 긴 메소드를 작은 조각으로 분해하세요.
- Heavy Computation에 대한 배경 처리 사용: 무거운 작업을 백그라운드 작업으로 오프로드하세요.
- 효율적인 알고리즘 채택: 핵심 작업에 대해 더 나은 시간 복잡도 알고리즘을 선택하세요.

고려할 젬들:

<div class="content-ad"></div>

- rails_best_practices: 코드를 분석하고 개선을 제안해주는 도구입니다.
 

# Gemfile gem `rails_best_practices`


성능 문제를 조기에 발견하기 위해 정기적으로 모니터링하는 것이 중요합니다:

- Rails의 내장 도구 활용: 성능 추적을 위해 Rails 콘솔과 로그를 활용합니다.
- 성능 모니터링 도구 도입: New Relic 및 Skylight와 같은 도구를 사용하여 자세한 통찰력을 얻을 수 있습니다.

고려해볼 젬들:

<div class="content-ad"></div>

- skylight: 실시간 통찰력을 제공하는 성능 모니터링 도구.

# Gemfile gem `skylight`


- newrelic_rpm: New Relic을 통해 애플리케이션 성능을 모니터링하는 도구.

# Gemfile gem `newrelic_rpm`


루비 온 레일즈 애플리케이션을 최적화하는 것은 효과적인 캐싱, 데이터베이스 쿼리 최적화, 백그라운드 작업 관리, 자산 처리, 메모리 사용 및 코드 개선의 조합이 필요합니다. 이러한 기술을 활용하고 유용한 젬을 활용함으로써 애플리케이션이 원할하고 효율적으로 동작할 수 있습니다. 하지만 SLAP, SOLID 등과 같이 코딩에 대한 최상의 원칙을 준수한다면 코드를 미래에 완벽하게 유지할 수 있습니다.

애플리케이션이 발전함에 따라 최적화 전략을 계속 탐구하고 개선해 나가세요.

행복한 코딩하세요! 🚀