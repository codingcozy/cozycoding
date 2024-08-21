---
title: "NET의 숨겨진 보석들 모든 개발자가 알아야 할 트릭과 기능들"
description: ""
coverImage: "/blocktong.github.io/assets/no-image.jpg"
date: 2024-07-06 11:23
ogImage: 
  url: /blocktong.github.io/assets/no-image.jpg
tag: Tech
originalTitle: "Hidden Gems of .NET: Tricks and Features Every Developer Should Know"
link: "https://medium.com/@justinamiller_52480/hidden-gems-of-net-tricks-and-features-every-developer-should-know-ca6e006288d2"
isUpdated: true
---





.NET은 세계 각지의 개발자들에게 필수적인 강력하고 다재다능한 프레임워크입니다. 그 핵심 기능들은 널리 알려져 있지만, 개발 경험과 효율성을 현저히 향상시킬 수 있는 몇 가지 숨겨진 기능과 요령이 있습니다. .NET 개발자라면 알아야 할 몇 가지 보석들에 대해 살펴봅시다.

1. Global Using Directives (C# 10 이상) 모든 파일에서 using 문을 반복하기 지겹다면 C# 10은 Global Using Directives를 소개합니다. 프로젝트 파일에서 한 번 선언하면 모든 파일에 자동으로 적용되어 중복을 줄이고 유지 관리성을 향상시킵니다.

```csharp
// 프로젝트 파일에서 (예: .csproj)
<ItemGroup>
    <Using Include="System" />
    <Using Include="System.Collections.Generic" />
    <Using Include="System.Linq" /> 
</ItemGroup>
```

2. 문자열 보간 향상 기능

<div class="content-ad"></div>

문자열 보간은 훨씬 표현력이 뛰어납니다! 보간된 문자열 내에서 변수를 직접 사용하거나 더 복잡한 표현식을 만들며, 숫자를 정밀하게 형식화할 수도 있습니다:

```js
double price = 19.99;
int quantity = 3;
string message = $"주문 수량: {quantity}개. 총액: {price * quantity:C2}"; // 총액: $59.97
```

3. Span`T`Span`T`의 힘 및 읽기 전용 버전인 ReadOnlySpan`T`은 메모리를 할당하지 않고 배열 및 문자열의 슬라이스와 효율적으로 작업할 수 있는 방법을 제공합니다. 이는 고성능 시나리오에 빛을 발합니다:

```js
ReadOnlySpan<char> text = "안녕, 세상!".AsSpan(); 
ReadOnlySpan<char> substring = text.Slice(0, 5); // "안녕"
```

<div class="content-ad"></div>

4. C# 9부터 원시 내용을 작성하여 보일러플레이트를 제거하여 프로그램을 최적화하세요. 최상위 문은 네임스페이스나 클래스로 둘러싸지 않은 프로그램의 중심 논리를 작성할 수 있습니다:

```csharp
// 네임스페이스나 클래스가 필요 없습니다!
Console.WriteLine("안녕하세요, 최상위 문에서 출력하는 내용입니다!");
```

5. Source Generators 소스 생성기는 컴파일 시간에 C# 코드를 생성할 수 있는 강력한 도구입니다. 이를 통해 동적 코드 생성, 사용자 정의 빌드 단계, 그리고 컴파일 시간 최적화를 통한 성능 향상이 가능합니다.

6. 파일 범위 네임스페이스 (C# 10부터 이어짐) 더 이상의 상세함을 줄이는 기능 중 하나로 파일 범위 네임스페이스가 있습니다:

<div class="content-ad"></div>

```js
// 파일 전체에 적용됨
namespace MyProject.Utilities;
```

7. Nullable Reference Types (C# 8 및 이후) 코드의 안전성과 신뢰성을 향상시키세요. 변수를 명시적으로 nullable 또는 non-nullable로 표시하면 됩니다.

C# 8.0에서 소개된 Nullable Reference Types는 NullReferenceException을 피할 수 있도록 도와줍니다. 이 기능을 활성화하면 어떤 변수가 null이 될 수 있는지, 불가능한지 명시적으로 지정할 수 있습니다.

다음 줄을 .csproj 파일에 추가하세요:


<div class="content-ad"></div>

`Nullable`을 사용하여 `값이 없음/값이 있음`을 지정합니다.

이제 참조 유형은 기본적으로 비-nullable로 지정되며, nullable 참조 유형에 대해는 ? 접미사를 사용해야 합니다:

```js
string? name = null; // Nullable
string message = "Hello!"; // Non-nullable
```

8. 모든 것에 대한 사용자 정의 속성 속성은 클래스와 메서드에 꾸미기 위한 것뿐만 아니라 필드, 매개변수, 열거형 등에 메타데이터를 추가하여 코드에 사용자 정의 동작을 추가하는 데 사용할 수 있습니다.

<div class="content-ad"></div>

9. 레코드 (C# 9 및 그 이상) 레코드는 불변의 참조 유형을 간결하게 생성하는 방법으로, 데이터 모델 및 값 동등성을 강제하고 싶은 시나리오에 완벽합니다:

```csharp
public record Person(string FirstName, string LastName);
```

10. .NET Interactive 노트북 탐험 아직 시도하지 않은 경우, .NET Interactive 노트북을 살펴보세요. 이 노트북은 학습, 프로토타입 또는 프로젝트를 공개하는 데 코드, 시각화 및 문서를 결합하는 매력적인 방법을 제공합니다.

## 11. 패턴 매칭 개선

<div class="content-ad"></div>

C# 9.0부터는 여러 패턴 매칭 개선 사항이 도입되어 코드가 더 간결하고 표현력이 풍부해졌습니다.

```cs
public string GetDescription(object obj) =>
    obj switch
    {
        Person p => $"Person: {p.FirstName} {p.LastName}",
        int i => $"Integer: {i}",
        _ => "Unknown"
    };
```

이 스위치 표현은 복잡한 조건 로직을 간소화합니다.

## 12. 레코드와 Init-Only 세터

<div class="content-ad"></div>

C# 9.0에서 소개된 init-only setters를 통해 객체 이니셜라이저를 사용하면서 불변 객체를 생성할 수 있습니다.

```csharp
public class Product
{
    public string Name { get; init; }
    public decimal Price { get; init; }
}
```

```csharp
var product = new Product { Name = "노트북", Price = 999.99m };
```

이 기능을 사용하면 속성이 객체 초기화 중에만 설정될 수 있도록 보장할 수 있습니다.

<div class="content-ad"></div>

## 13. System.Text.Json

.NET Core 3.0에서 소개된 System.Text.Json은 고성능 JSON 직렬화 라이브러리를 제공합니다. Newtonsoft.Json과 비교했을 때 더 효율적이고 가벼워요.

```csharp
var options = new JsonSerializerOptions { WriteIndented = true };
var jsonString = JsonSerializer.Serialize(product, options);
Console.WriteLine(jsonString);
```

```csharp
var deserializedProduct = JsonSerializer.Deserialize<Product>(jsonString);
```

<div class="content-ad"></div>

**이 라이브러리는 직렬화 및 역직렬화를 위한 다양한 사용자 정의 옵션을 지원합니다.**

### 14. BenchmarkDotNet을 사용한 벤치마킹

BenchmarkDotNet은 .NET 코드의 벤치마킹에 유용한 라이브러리입니다. 성능을 측정하고 최적화하는 데 도움이 됩니다.

BenchmarkDotNet 패키지를 설치하고 벤치마크 클래스를 만들어보세요:

<div class="content-ad"></div>

```csharp
[MemoryDiagnoser]
public class StringConcatBenchmark
{
    [Benchmark]
    public string PlusOperator() => "Hello" + " " + "World";

    [Benchmark]
    public string StringConcat() => string.Concat("Hello", " ", "World");
}
class Program
{
    static void Main(string[] args)
    {
        BenchmarkRunner.Run<StringConcatBenchmark>();
    }
}
```

벤치마크를 실행하고 결과를 분석하여 성능 병목 현상을 식별하세요.

### 15. 비동기 작업에서 오버헤드를 줄이기 위한 ValueTask

작업(Task) 유형은 .NET 내에서 비동기 프로그래밍에서 불가피하게 사용됩니다. 그러나 특정 시나리오에서는 필요하지 않은 오버헤드를 도입합니다. 그때 ValueTask가 나타납니다.


<div class="content-ad"></div>

- ValueTask: 이 구조는 메서드가 자주 동기적으로 반환되는 경우 Task와 관련된 오버헤드를 줄입니다. 할당을 피하므로 고성능 애플리케이션에 효율적인 선택지가 됩니다.

```csharp
public ValueTask<int> GetDataAsync() 
{     
    if (dataAvailable)    
    {        
        return new ValueTask<int>(data);     
    }     
    else   
    {         
        return new ValueTask<int>(FetchDataAsync());   
    }
}
```

## 16. 고처리량 데이터 처리를 위한 채널

고성능, 스레드 안전한 데이터 처리를 위해 System.Threading.Channels은 강력한 도구를 제공합니다.

<div class="content-ad"></div>

- 채널: 생산자와 소비자 간의 고 처리량 및 낮은 대기 시간 통신이 필요한 시나리오를 위해 설계되었습니다. BlockingCollection이나 ConcurrentQueue와 비교하여 더 많은 제어 및 성능을 제공합니다.

```csharp
var channel = Channel.CreateUnbounded<int>();
await channel.Writer.WriteAsync(42); 
var value = await channel.Reader.ReadAsync();
```

## 17. 효율적인 캐싱을 위한 MemoryCache

캐싱은 성능을 향상시키는 잘 알려진 기술이며, .NET의 MemoryCache는 유연하고 효율적인 방식으로 이를 구현할 수 있습니다.

<div class="content-ad"></div>

- MemoryCache: 이 클래스는 데이터를 메모리에 저장할 수 있게 해 주어 반복적인 데이터 검색 작업이 줄어듭니다. 만료 정책과 캐시 종속성을 지원하여 성능 최적화에 강력한 선택지가 됩니다.

```js
var cache = new MemoryCache(new MemoryCacheOptions());
cache.Set("key", value, TimeSpan.FromMinutes(5));
var cachedValue = cache.Get("key");
```

## 18. Struct 사용 및 박싱 피하기

박싱과 언박싱은 상당한 오버헤드를 발생시킬 수 있으며 값 형식에 대해 클래스 대신 구조체를 사용함으로써 이를 피할 수 있습니다.

<div class="content-ad"></div>

- 구조체: 이들은 값 형식이며 메모리 사용량과 성능 측면에서 효율적일 수 있습니다. 그러나 적절하지 않게 사용할 경우 더 많은 메모리 압력을 초래할 수 있으므로 신중히 사용해야 합니다.

```csharp
public struct Point
{
  public int X { get; set; }
  public int Y { get; set; }
}
```

- 박싱(Boxing)은 값 형식(예: int, bool, struct 등)을 참조 형식(예: object)으로 변환하는 과정입니다. 값 형식을 박싱할 때:

```csharp
int num = 42;      // 값 형식 (int)
object obj = num;   // 박싱 (int가 object로 변환됨)
```

<div class="content-ad"></div>

언박싱은 박싱의 반대 과정입니다. 객체 참조에서 값 유형을 추출합니다. 언박싱은 다음과 같이 이루어집니다:

```js
object obj = 42;    // obj가 박싱된 int라고 가정
int num = (int)obj; // 언박싱 (객체가 다시 int로 변환됨)
```

# 결론

.NET의 이러한 숨겨진 보석들은 개발 경험과 생산성을 크게 향상시킬 수 있습니다. 이러한 기능을 활용하여 보다 깔끔하고 효율적인 코드를 작성하고.NET 개발의 끊임없이 발전하는 세계에서 선도할 수 있습니다. 즐거운 코딩되세요!

<div class="content-ad"></div>

아래 댓글로 여러분의 .NET 팁과 꿀팁을 자유롭게 공유해 주세요! 🚀✨