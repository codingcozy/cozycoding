---
title: "루비에서의 에러 처리 최상의 관행과 기술"
description: ""
coverImage: "/assets/img/2024-09-10-HandlingErrorsinRubyBestPracticesandTechniques_0.png"
date: 2024-09-10 19:13
ogImage: 
  url: /assets/img/2024-09-10-HandlingErrorsinRubyBestPracticesandTechniques_0.png
tag: Tech
originalTitle: "Handling Errors in Ruby Best Practices and Techniques"
link: "https://medium.com/@patrykrogedu/handling-errors-in-ruby-best-practices-and-techniques-86a04a071bde"
isUpdated: true
updatedAt: 1726022822386
---


<img src="/assets/img/2024-09-10-HandlingErrorsinRubyBestPracticesandTechniques_0.png" />

# 반환 값으로 오류 처리하기

일부 경우에는 Ruby 메서드가 예외를 발생시키는 대신 실패를 나타내기 위해 특정 값을 반환하여 오류를 처리합니다. 이 접근 방식은 특정 상황에서 핵심 Ruby 메서드에서 볼 수 있으며 특정 시나리오에서 장점이 있습니다.

# 예시: 해시 키 검색

<div class="content-ad"></div>

루비가 해시에서 존재하지 않는 키를 검색하는 방법을 살펴봅시다:

```js
hash = {'a' => 2}
result = hash['b']
puts result  # 결과: nil
```

이 경우 루비는 해당 키가 없을 때 예외를 발생시키지 않고 nil을 반환합니다. 이 접근 방식은 예외 처리 없이 누락된 키를 보다 유연하게 처리할 수 있게 해줍니다.

# 메모리제이션 기술

<div class="content-ad"></div>

루비의 해시 검색 방식은 간단한 메모이제이션 기법을 사용할 수 있게 합니다:

```js
def memoized_calculation(key)
  @cache ||= {}
  @cache[key] ||= expensive_calculation(key)
end
```

이 기법은 hash[key]가 존재하지 않는 키에 대해 nil을 반환하기 때문에, ||= 연산자가 계산을 수행하고 결과를 저장할 수 있습니다.

# 오류 처리를 위한 반환 값의 장단점

<div class="content-ad"></div>

장점:

- 많은 경우에 더 나은 성능을 보여줍니다.
- 일반적인 오류 시나리오를 처리하기가 더 쉽습니다.

단점:

- 반환 값이 확인되지 않을 경우 은실적인 실패로 이어질 수 있습니다.
- 오류 감지를 지연시켜 디버깅을 복잡하게 할 수 있습니다.

<div class="content-ad"></div>

# 예외를 사용한 오류 처리

예외를 발생시키는 것은 루비에서 오류를 처리하는 가장 일반적인 방법입니다. 이 접근 방식은 루비의 핵심 메소드에서 널리 사용되며, 예상치 못한 또는 드물게 발생하는 오류에 대해 일반적으로 선호됩니다.

# 예시: 권한 시스템

권한 시스템의 예시를 살펴보면, 적절한 오류 처리를 보장하기를 원할 것입니다.

<div class="content-ad"></div>

```js
class Authorizer
  class InvalidAuthorization < StandardError; end

  def self.check(user, action)
    unless new(user, action).authorized?
      raise InvalidAuthorization, "#{user.name} is not authorized to perform #{action}"
    end
  end

  # ... other methods ...
end
```

예외를 발생시켜 호출자가 오류를 명시적으로 처리해야 하도록 강제함으로써 실수로 권한 상승을 방지합니다.

# 예외 기반 오류 처리의 장단점

장점:

<div class="content-ad"></div>

- 명시적인 오류 처리를 강요함
- 자세한 오류 정보를 제공함 (스택 트레이스)
- 중앙화된 오류 처리를 가능하게 함

단점:
- 특히 큰 호출 스택을 가지는 경우 성능에 영향을 줄 수 있음
- 과도하게 사용될 경우 코드가 지나치게 복잡해질 수 있음

# 성능 고려사항

<div class="content-ad"></div>

예외 처리에 예외를 사용할 때는 특히 코드의 성능이 중요합니다. 특히 성능이 중요한 섹션에서 성능 영향을 고려해야 합니다.

# 예외 성능 최적화

예외를 발생시킬 때 성능을 향상시키려면 다음과 같은 기술을 사용할 수 있습니다:

```js
EMPTY_ARRAY = [].freeze

def optimized_raise
  raise ArgumentError.new("메시지").tap { |e| e.set_backtrace(EMPTY_ARRAY) }
end
```

<div class="content-ad"></div>

이 방식을 사용하면 비용이 많이 드는 전체 백트레이스를 생성하는 것을 피할 수 있습니다. 그러나 디버깅을 어렵게 만들기 때문에 이 기술을 신중하게 사용해야 합니다.

# 일시적 오류 재시도

일부 시나리오에서는 특히 네트워크 작업과 같은 경우, 일시적 오류가 발생하는 작업을 재시도하는 것이 유용할 수 있습니다.

# 기본 재시도 메커니즘

<div class="content-ad"></div>

```js
def perform_network_operation
  retries = 0
  begin
    # 네트워크 작업 수행
  rescue NetworkError => e
    retries += 1
    retry if retries <= 3
    raise
  end
end
```

# 지수 백오프를 이용한 고급 재시도

더 정교한 방법으로 지수 백오프를 구현하세요:

```js
def perform_network_operation
  retries = 0
  begin
    # 네트워크 작업 수행
  rescue NetworkError => e
    retries += 1
    raise if retries > 3
    sleep(3 * (0.5 + rand/2) * 1.5**(retries-1))
    retry
  end
end
```

<div class="content-ad"></div>

# 예외 클래스 계층구조 설계

라이브러리 또는 복잡한 응용 프로그램을 만들 때, 신중하게 설계된 예외 계층구조를 만드는 것이 중요합니다.

# 권장 사항

- 라이브러리를 위한 기본 예외 클래스를 만드세요.

<div class="content-ad"></div>

```js
module MyLibrary
  class Error < StandardError; end
end
```

- 필요할 때만 특정 예외 클래스를 생성하세요:

```js
module MyLibrary
  class Error < StandardError; end
  class NetworkError < Error; end
  class ValidationError < Error; end
end
```

- 사용자가 다르게 처리하고 싶을 때 특정 예외 클래스를 사용하세요:

<div class="content-ad"></div>


begin
  MyLibrary.perform_operation
rescue MyLibrary::NetworkError
  # Handle network-specific errors
rescue MyLibrary::ValidationError
  # Handle validation-specific errors
rescue MyLibrary::Error
  # Handle general errors
end
