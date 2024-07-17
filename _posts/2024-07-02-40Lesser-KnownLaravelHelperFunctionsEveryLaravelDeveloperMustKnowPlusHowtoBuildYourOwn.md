---
title: "모든 Laravel 개발자가 알아야 할 40개의 잘 알려지지 않은 Laravel 헬퍼 함수와 직접 만드는 방법"
description: ""
coverImage: "/assets/img/2024-07-02-40Lesser-KnownLaravelHelperFunctionsEveryLaravelDeveloperMustKnowPlusHowtoBuildYourOwn_0.png"
date: 2024-07-02 22:29
ogImage: 
  url: /assets/img/2024-07-02-40Lesser-KnownLaravelHelperFunctionsEveryLaravelDeveloperMustKnowPlusHowtoBuildYourOwn_0.png
tag: Tech
originalTitle: "40 Lesser-Known Laravel Helper Functions Every Laravel Developer Must Know, Plus How to Build Your Own"
link: "https://medium.com/@prevailexcellent/40-lesser-known-laravel-helper-functions-every-laravel-developer-must-know-plus-how-to-build-your-064e8f1ad2d7"
---



![Image](/assets/img/2024-07-02-40Lesser-KnownLaravelHelperFunctionsEveryLaravelDeveloperMustKnowPlusHowtoBuildYourOwn_0.png)

라라벨의 잘 알려진 도우미 함수들은 널리 사용되지만, 개발 워크플로우를 크게 향상시킬 수 있는 많은 미처 알려지지 않은 도우미 함수들이 있습니다.

이러한 도우미 함수들이 얼마나 유용하고 라라벨 코드베이스의 어느 부분에서든 호출할 수 있는 점을 좋아합니다. 이들은 작업을 간소화하고 코드 가독성을 향상시키며 생산성을 향상시키는 데 도움이 됩니다. 여기에는 40가지 희귀하지만 가치 있는 라라벨 도우미 함수들이 있습니다:

일반적인 함수부터 시작하여 최상의 함수로 이동합니다. 애플리케이션 속도와 효율을 실제로 향상시킬 수 있는 많은 멋진 함수들을 볼 것입니다. 시작하겠습니다.


<div class="content-ad"></div>

# 1. filled()

filled 함수는 값이 비어 있지 않은지 확인하며, !empty()를 사용하는 것보다 직관적일 수 있습니다.

```js
$value = 'some text';

if (filled($value)) {
    echo '값이 비어 있지 않습니다.'; // 결과: 값이 비어 있지 않습니다.
} else {
    echo '값이 비어 있습니다.';
}
```

# 2. blank()

<div class="content-ad"></div>

빈 함수는 값이 비어 있는지 확인합니다. 이 함수는 filled() 함수의 반대 경우에 유용합니다.

```js
$value1 = '';
$value2 = null;
$value3 = 'Laravel';

if (blank($value1)) {
    echo 'Value 1 is blank'; // 이 부분이 출력됩니다
}

if (blank($value2)) {
    echo 'Value 2 is blank'; // 이 부분이 출력됩니다
}

if (blank($value3)) {
    echo 'Value 3 is blank'; // 이 부분은 출력되지 않습니다
}
```

### 3. retry()

retry 함수는 동작을 지정된 시간 간격으로 여러 번 다시 시도합니다. 이 함수는 네트워크 요청이나 데이터베이스 트랜잭션이 가끔 실패할 수 있는 경우에 유용합니다.

<div class="content-ad"></div>

재시도가 임시 오류를 처리하는 방법을 보세요. 시도를 하는 동안 100밀리초의 지연으로 최대 5번까지 작업을 재시도합니다.

```js
$result = retry(5, function () {
    // 실패할 수 있는 위험한 작업 시뮬레이션
    if (rand(0, 1)) {
        throw new Exception('실패함');
    }
    return '성공';
}, 100);

echo $result; // 작업이 성공적으로 완료되면 '성공'을 출력합니다
```

# 4. throw_if()

throw_if 함수는 주어진 조건이 참이면 예외를 throw합니다. 이는 중첩된 if 문을 줄여서 코드를 더 깔끔하게 만들 수 있습니다.

<div class="content-ad"></div>

```php
$user = null;

throw_if(!$user, Exception::class, 'User not found');
// 이 코드는 'User not found' 메시지를 포함한 예외를 throw합니다.
```

# 5. throw_unless()

throw_unless 함수는 주어진 조건이 true가 아닐 때 예외를 throw합니다. throw_if의 반대 동작을 합니다. 이 함수는 조건이 충족되었는지 확인하고 그렇지 않으면 에러를 처리할 때 유용합니다.

```php
$user = User::find(1);

throw_unless($user, Exception::class, 'User not found');
// $user가 null인 경우 예외를 throw합니다.
```

<div class="content-ad"></div>

# 6. transform()

transform 함수는 주어진 콜백을 사용하여 값을 조건적으로 변환합니다. 값들을 깔끔하고 가독성 있게 수정하는 데 유용합니다. 아래는 입력 문자열이 null이 아닌 경우 대문자로 변환되는 예제입니다. 입력이 null이면 기본 값이 반환됩니다.

```js
$input = 'hello world';

$output = transform($input, function ($value) {
    return strtoupper($value);
}, 'default value');

echo $output; // 결과: HELLO WORLD
```

# 7. optional()

<div class="content-ad"></div>

선택 사항 함수는 오류를 발생시키지 않고 널일 수 있는 객체의 속성에 액세스하거나 메서드를 호출할 수 있도록 합니다. 널 체크를 피하는 데 유용합니다.

```js
$user = null;

$name = optional($user)->name;

echo $name; // 출력: 에러를 발생시키지 않고 아무것도 출력되지 않음 (null)
```

# 8. cache()

캐시 함수는 캐시에서 항목을 검색하거나 저장합니다. 비용이 많이 드는 작업을 캐싱하여 성능을 최적화하는 데 유용합니다. 캐시가 유지될 시간을 설정할 수 있습니다.

<div class="content-ad"></div>

데이터베이스에 반복적으로 호출하지 않고 캐시를 활용하여 시간과 리소스를 절약할 수 있습니다.

```js
$value = cache('key', function () {
    return DB::table('users')->get();
}, 60);

echo $value; // 캐시된 사용자 데이터 출력
```

# 9. logger()

logger 함수는 메시지를 로그 파일에 기록합니다. 이는 디버깅 및 모니터링에 유용하며 응용 프로그램의 흐름을 추적하고 문제를 진단하는 데 도움이 됩니다.

<div class="content-ad"></div>

```js
logger('디버그 메시지입니다.');
```

# 10. cookie()

cookie 함수는 새로운 쿠키 인스턴스를 생성합니다. 이는 애플리케이션에서 쿠키를 설정하는 데 유용합니다.

```js
$cookie = cookie('name', 'value', 60);
return response('안녕')->cookie($cookie);
```

<div class="content-ad"></div>

# 11. csrf_field()

csrf_field 함수는 CSRF 토큰을 포함하는 숨겨진 입력 필드를 생성합니다. 이는 폼을 사이트간 요청 위조로부터 보호하는 데 필수적입니다.

```js
<form method="POST" action="/submit">
    { csrf_field() }
    <input type="text" name="name">
    <button type="submit">제출</button>
</form>
```

# 12. csrf_token()

<div class="content-ad"></div>

`csrf_token` 함수는 현재 CSRF 토큰을 검색합니다. 이는 API 요청이나 사용자 정의 양식에 토큰을 포함하는 데 유용합니다.

```js
$token = csrf_token();
echo $token; // 현재 CSRF 토큰을 출력합니다
```

## 13. encrypt() 및 decrypt()

`encrypt` 함수는 주어진 값을 암호화합니다. 이는 저장 전에 민감한 정보를 안전하게 보호하는 데 유용합니다.

<div class="content-ad"></div>

복호화 함수는 주어진 값의 복호화를 수행합니다. 이는 데이터베이스에 저장된 또는 사용자로부터 수신된 암호화된 데이터를 처리하는 데 유용합니다.

```js
$encryptedValue = encrypt('mySecret');
$decryptedValue = decrypt($encryptedValue);

echo $decryptedValue; // 출력: mySecret
```

# 14. e()

e 함수는 문자열에서 HTML 엔티티를 이스케이프합니다. 이는 XSS 공격을 방지하기 위해 사용자 입력을 이스케이프하는 데 유용합니다. HTML 엔티티를 이스케이프함으로써 사용자 입력이 안전하게 브라우저에 표시되도록 할 수 있습니다.

<div class="content-ad"></div>

```js
$userInput = '<script>alert("Hello")</script>';
echo e($userInput); // 출력: &lt;script&gt;alert(&quot;Hello&quot;)&lt;/script&gt;
```

# 15. rescue()

rescue() 함수는 콜백을 실행하고 예외가 발생할 경우 기본값을 반환합니다. 이는 제어된 방식으로 예외를 처리하는 데 유용합니다.

```js
$value = rescue(function () {
    return riskyOperation();
}, 'default');

echo $value; // 출력: 예외가 발생할 경우 'default'
```

<div class="content-ad"></div>

# 16. broadcast()

방송 함수는 이벤트를 전파합니다. 이는 응용 프로그램에서 실시간 알림 및 업데이트에 유용합니다.

```js
broadcast(new UserRegistered($user));
```

# 17. bcrypt()

<div class="content-ad"></div>

bcrypt 함수는 Bcrypt를 사용하여 값을 해시화합니다. 이것은 비밀번호를 안전하게 저장하는 데 필수적입니다.

```js
$hashedPassword = bcrypt('password');
echo $hashedPassword; // 해시된 비밀번호 문자열을 출력함
```

# 18. data_fill()

이것은 정말 좋아요. data_fill 함수는 "점" 표기법을 사용하여 배열이나 객체에서 누락된 데이터를 채웁니다. 기본값을 설정하는 데 유용합니다.

<div class="content-ad"></div>

```php
$data = ['product' => ['name' => 'Desk']];
data_fill($data, 'product.price', 100);

print_r($data); // 결과: ['product' => ['name' => 'Desk', 'price' => 100]]
```

## 19. data_get()

data_get 함수는 "점" 표기법을 사용하여 중첩된 배열이나 객체에서 값을 검색합니다. 이는 깊게 중첩된 데이터 구조에 액세스하는 데 유용합니다.

```php
$data = ['product' => ['name' => 'Desk', 'price' => 100]];
$name = data_get($data, 'product.name');

echo $name; // 결과: Desk
```

<div class="content-ad"></div>

내가 작성한 단계별 기사가 있는 곳에서 데이터를 자유롭게 변경할 수 있는 도우미 기능을 만들어볼 수 있다는 걸 알고 계셨나요? 네, 가능해요.

# 20. data_set()

data_set 기능은 "점" 표기법을 사용하여 깊게 중첩된 배열이나 객체 내의 값을 설정합니다. 이는 복잡한 데이터 구조의 특정 부분을 업데이트하는 데 유용합니다.

```js
$data = ['product' => ['name' => 'Desk']];
data_set($data, 'product.price', 200);

print_r($data); // 출력: ['product' => ['name' => 'Desk', 'price' => 200]]
```

<div class="content-ad"></div>

# 21. report()

report 함수는 예외를 보고합니다. 이것은 예외를 로깅하여 오류 추적 서비스에 기록하는 데 유용합니다.

```js
try {
    // 예외를 발생시킬 수 있는 코드
} catch (Exception $e) {
    report($e);
}
```

# 22. resolve()

<div class="content-ad"></div>

resolve 함수는 라라벨 서비스 컨테이너에서 서비스를 해결합니다. 서비스를 직접 주입하지 않고도 서비스에 액세스하는 데 유용합니다.

```js
$service = resolve('App\Services\MyService');
$service->performTask();
```

## 23. session

이것은 그리 드문 것이 아닙니다. session 함수는 세션 값을 가져오거나 설정합니다. 사용자 세션을 관리하는 데 유용합니다.

<div class="content-ad"></div>

```js
session(['key' => 'value']);
$value = session('key');

echo $value; // 결과: value
```

# 24. validator()

validator 함수는 새로운 유효성 검사기 인스턴스를 생성합니다. 이는 정의된 규칙에 대해 데이터를 유효성 검사하는 데 유용합니다.

```js
$validator = validator(['name' => 'John'], ['name' => 'required|string|max:255']);

if ($validator->fails()) {
    // 유효성 검사 실패 처리
    echo '유효성 검사 실패';
} else {
    // 유효성 검사 성공 처리
    echo '유효성 검사 성공';
}
```

<div class="content-ad"></div>

# 25. value()

value 함수는 주어진 값을 반환합니다. Closure가 주어지면 실행되고 결과가 반환됩니다. 이것은 값이 조건에 따라 설정될 때 유용합니다.

```js
$value = value(function () {
    return 'result';
});

echo $value; // 결과: result
```

# 26. windows_os()

<div class="content-ad"></div>

windows_os 함수는 애플리케이션이 Windows에서 실행 중인지 확인합니다. 플랫폼별 동작을 처리하는 데 유용합니다. 

```js 
if (windows_os()) {
    echo 'Windows에서 실행 중입니다.';
} else {
    echo 'Windows에서 실행 중이 아닙니다.';
}
```

# 27. method_field()

method_field 함수는 스푸핑된 HTTP 동사를 포함하는 숨은 입력 필드를 생성합니다. 이는 GET 또는 POST가 아닌 메소드를 제출해야 하는 양식에 필수적입니다.

<div class="content-ad"></div>

```js
<form method="POST" action="/update">
    { method_field('PUT') }
    { csrf_field() }
    <input type="text" name="name">
    <button type="submit">Update</button>
</form>
```

# 28. trans()

`trans` 함수는 주어진 메시지를 번역합니다. 응용 프로그램을 로컬라이징하는 데 유용합니다.

```js
echo trans('messages.welcome'); // 결과: '환영합니다' 또는 번역된 메시지
```

<div class="content-ad"></div>

# 29. trans_choice()

trans_choice 함수는 주어진 메시지를 개수에 따라 번역합니다. 복수형 처리에 유용합니다.

```js
echo trans_choice('messages.apples', 10); // 결과: '10 apples' 또는 번역된 메시지
```

# 30. __()

<div class="content-ad"></div>

위의 함수는 주어진 키에 대한 번역을 가져옵니다. 이것은 trans()의 약칭입니다.

```js
echo __('messages.welcome'); // 출력: '환영' 또는 번역된 메시지
```

사용자 정의 도우미 함수를 쉽게 만들 수 있다는 것을 알고 계셨나요? 네, 그렇습니다. 저는 여기에 이에 관한 단계별 기사를 작성했어요.

# 31. class_uses_recursive()

<div class="content-ad"></div>

class_uses_recursive 함수는 클래스가 사용하는 모든 트레이트를 반환합니다. 부모 클래스들이 사용하는 트레이트도 포함됩니다. 이 함수는 클래스의 구성을 이해하는 데 도움이 되며, 특히 복잡한 상속 구조에서 유용합니다.

```js
use Illuminate\Support\Str;
use Illuminate\Database\Eloquent\Model;

class User extends Model {
    use Str;
}

$traits = class_uses_recursive(User::class);

print_r($traits); // 결과: User 및 부모 클래스에서 사용하는 트레이트들의 배열
```

# 32. collect()

collect 함수는 주어진 값으로 새로운 컬렉션 인스턴스를 생성합니다. 이 함수는 배열과 데이터 구조를 보다 유창하고 표현력 있게 다루는 데 유용합니다.

<div class="content-ad"></div>

```js
$collection = collect([1, 2, 3, 4, 5])
    ->map(function ($item) {
        return $item * 2;
    })
    ->reject(function ($item) {
        return $item < 8;
    });

print_r($collection->all()); // 결과: [8, 10]
```

# 33. dispatch()

`dispatch` 함수는 작업을 해당 핸들러로 보냅니다. 이것은 작업을 깨끗하고 가독성 있게 대기열에 넣는 데 유용합니다.

```js
dispatch(new App\Jobs\SendEmailJob($user));
```

<div class="content-ad"></div>

# 34. head()

head 함수는 배열의 첫 번째 요소를 반환합니다. 배열 키를 재설정할 필요 없이 첫 번째 항목에 빠르게 액세스하는 데 유용합니다.

```js
$array = [100, 200, 300];
$firstElement = head($array);

echo $firstElement; // 출력: 100
```

# 35. last()

<div class="content-ad"></div>

마지막 함수는 배열의 마지막 요소를 반환합니다. 이것은 배열 키를 재설정할 필요 없이 빠르게 마지막 항목에 액세스하는 데 유용합니다.

```js
$array = [100, 200, 300];
$lastElement = last($array);

echo $lastElement; // 결과: 300
```

# policy()

policy 함수는 지정된 클래스에 대한 정책 인스턴스를 검색합니다. 이는 권한 규칙을 확인하는 데 유용합니다.

<div class="content-ad"></div>

```php
$policy = policy(App\Models\Post::class);

if ($policy->update($user, $post)) {
    echo '사용자는 게시물을 업데이트할 수 있습니다';
} else {
    echo '사용자는 게시물을 업데이트할 수 없습니다';
}
```

# 37. with()

`with` 함수는 주어진 값을 반환합니다. 이는 메소드를 연속적으로 사용하거나 연쇄적으로 변수에 값을 할당하는 데 유용합니다. 이를 통해 코드를 더 읽기 쉽게 만들 수 있습니다.

```php
$value = with('some value', function ($value) {
    return strtoupper($value);
});

echo $value; // 결과: SOME VALUE
```

<div class="content-ad"></div>

# 38. preg_replace_array()

preg_replace_array 함수는 대상에서 정규 표현식 검색 및 치환을 수행하며 치환 배열을 사용합니다. 이는 정규 표현식에 기반한 문자열에서 여러 번 치환을 수행하는 데 유용합니다.

```js
$pattern = '/(Laravel|PHP)/';
$replacements = ['Lumen', 'JavaScript'];

$text = 'Laravel is a PHP framework. Laravel is great.';
$replaced = preg_replace_array($pattern, $replacements, $text);

echo $replaced; // 출력: 'Lumen is a JavaScript framework. Laravel is great.'
```

# 39. today() and now()

<div class="content-ad"></div>

`today` 함수는 현재 날짜를 자정으로 반환합니다. 이는 하루의 시작으로 표준화된 날짜 값을 생성하는 데 유용합니다.

`now` 함수는 현재 날짜와 시간에 대한 Carbon 인스턴스를 반환합니다. 이는 Carbon 라이브러리를 사용하여 표준화되고 강력한 방식으로 날짜와 시간 값을 다루는 데 유용합니다. Carbon은 날짜 조작 및 형식 지정에 광범위한 기능을 제공합니다.

```js
$today = today();
echo $today; // 출력: 현재 날짜 00:00:00

$now = now();
echo $now; // 출력: 기본 형식의 현재 날짜와 시간

// 날짜 형식 지정
$formattedNow = now()->format('Y-m-d H:i:s');
echo $formattedNow; // 출력: 'YYYY-MM-DD HH:MM:SS' 형식의 현재 날짜와 시간

// 시간 추가
$future = now()->addDays(5);
echo $future; // 출력: 현재로부터 5일 후의 날짜와 시간

// 시간 빼기
$past = now()->subHours(3);
echo $past; // 출력: 3시간 전의 날짜와 시간
```

# 40. type_of()

<div class="content-ad"></div>

type_of 함수는 주어진 값의 유형을 반환합니다. 이는 디버깅에 유용하며 값이 예상한 유형인지 확인하는 데 도움이 됩니다.

```js
$type = type_of(123);
echo $type; // 결과: integer

$type = type_of('Laravel');
echo $type; // 결과: string
```

# 결론

이러한 함수를 좋아하는 이유는 Laravel 코드의 어느 부분에서든 호출할 수 있고 가져오기(import)나 해결(resolve) 없이 사용할 수 있다는 것입니다. 이들은 매우 쉽게 사용할 수 있고 코드를 깨끗하고 가독성 있게 유지하는 데 도움이 됩니다. 또한 코드베이스 전체에서 호출하고 사용할 수 있는 사용자 정의 도우미(helper) 함수를 만들 수도 있습니다.

<div class="content-ad"></div>

즐기세요! 

여러분은 쉽게 자신만의 도우미 함수를 만들 수 있다는 것을 알고 계셨나요? 네, 가능합니다. 저는 여기에 그 방법을 단계별로 설명한 기사를 썼어요.

곧 더 멋진 라라벨 자습서를 다음 기사에서 소개할 예정이니 기대해 주세요. 기사가 마음에 드셨길 바래요. 저를 팔로우하지 않으셨다면 잊지 말고 😇 클립을 좀 남겨주세요 👏. 또한 궁금한 점이 있으면 자유롭게 댓글을 남겨주세요.

감사합니다.

<div class="content-ad"></div>

끝까지 읽어주셔서 정말 감사합니다. 아래의 정보로 저를 팔로우하거나 연락해주세요:
- Twitter: [https://twitter.com/EjimaduPrevail](https://twitter.com/EjimaduPrevail)
- Email: prevailexcellent@gmail.com
- Github: [https://github.com/PrevailExcel](https://github.com/PrevailExcel)
- LinkedIn: [https://www.linkedin.com/in/chimeremeze-prevail-ejimadu-3a3535219](https://www.linkedin.com/in/chimeremeze-prevail-ejimadu-3a3535219)
- BuyMeCoffee: [https://www.buymeacoffee.com/prevail](https://www.buymeacoffee.com/prevail)
Chimeremeze Prevail Ejimadu