---
title: "코드 향상을 위한 놀라운 25가지 파이썬 트릭"
description: ""
coverImage: "/assets/img/2024-09-10-25AmazingPythonTricksThatWillInstantlyImproveYourCode_0.png"
date: 2024-09-10 18:06
ogImage: 
  url: /assets/img/2024-09-10-25AmazingPythonTricksThatWillInstantlyImproveYourCode_0.png
tag: Tech
originalTitle: "25 Amazing Python Tricks That Will Instantly Improve Your Code"
link: "https://medium.com/pythoneers/25-amazing-python-tricks-that-will-instantly-improve-your-code-8bfefbd62f2f"
isUpdated: false
---


## 더 나은 Python 코드를 작성하기 위한 여정!!

![이미지](/assets/img/2024-09-10-25AmazingPythonTricksThatWillInstantlyImproveYourCode_0.png)

저는 Python을 거의 5년간 사용해왔고, 여전히 새로운 방법을 발견해서 코드를 개선하는 것이 저를 흥분하게 만듭니다. 지난 1년 동안 제가 코딩 방법을 개선하고 생산성 및 효율성을 크게 향상시킬 수 있는 기술과 요령을 발견하는 데 집중해왔습니다. 이 블로그에서는 코드의 품질을 즉시 향상시키는 데 큰 영향을 미친 일부 Python 요령을 공유하겠습니다.

## 1. x.append 대신 시간을 낭비하지 마세요. x.extend는 리스트를 확장하는 훨씬 빠르고 깔끔한 방법을 제공합니다.

<div class="content-ad"></div>

여러 항목을 목록에 추가할 때 각 요소마다 반복적으로 append()를 사용하는 대신 한꺼번에 전체 iterable을 효율적으로 추가하기 위해 extend() 메서드를 선택하세요.  

```js
## 나쁨 
data = [1,2,3]
data.append(4)
data.append(5)
data.append(6)

## 좋음
data = [1,2,3]
data.extend((4,5,6))
```

## 2. suppress()를 사용하여 에러 처리 단순화하기

때로는 예외를 처리하고 무시하고 싶을 때가 있습니다. 이때 try/except 블록을 사용하여 except 블록에서 한 번에 처리할 수 있지만, contextlib에서 제공하는 suppress() 메서드를 사용하는 간단하고 간결한 방법이 있습니다.

<div class="content-ad"></div>

```js
## 나쁜 방법
try: 
 f() 
except FileNotFoundError: 
 pass

## 좋은 방법
with suppress(FileNotFoundError): 
 f()
```

## 3. 왜 더 많은 타이핑을 해야 하나요? print()로 간단하게 출력할 수 있어요.

```js
## 나쁜 방법
print("")

## 좋은 방법
print()
```

## 4. 파일 쓰기 효율 향상: 여러 개의 .write() 호출 대신 .writelines() 사용하기

<div class="content-ad"></div>

```js
## 나쁜 방법
lines = ["라인 1", "라인 2", "라인 3"] 
with open("file", "w") as f: 
    for line in lines: 
        f.write(line + "\n")

## 좋은 방법 
lines = ["라인 ", "라인 2", "라인 3"]
with open("file") as f: 
 f.writelines(lines)
```

## 5. 여러 옵션 중에 값 비교를 할 때, 여러 개의 `or` 대신 `in`을 사용하세요

```js
## 나쁜 방법
if x == "abc" or x == "mno" or x == "xyz": 
 pass

## 좋은 방법
if x in ("abc", "mno", "xyz"): 
 pass
```

## 6. 한 줄로 쓴 것이 지나치게 길 경우


<div class="content-ad"></div>

한 줄짜리 코드는 논리적인 기술을 많이 나타내며 좋습니다. 하지만 소스 코드에서 한 줄짜리 코드가 과도하면 혼란을 야기하고 가독성을 줄일 수 있습니다.

```js
## 나쁜 예
names = sorted([emp['name'] for emp in employees if emp['salary'] > 50000], \
                   key=lambda name: next(emp['salary'] \
                   for emp in employees if emp['name'] == name
                  ))

## 좋은 예
# 단계 1: 급여가 50,000 이상인 직원 필터링
filtered_employees = [emp for emp in employees if emp['salary'] > 50000]

# 단계 2: 필터된 직원들을 급여순으로 정렬
sorted_employees = sorted(filtered_employees, key=lambda emp: emp['salary'])

# 단계 3: 정렬된 직원들의 이름 추출
names = [emp['name'] for emp in sorted_employees]
```

## 7. 슬롯을 사용하여 메모리 사용량 줄이기

```js
## 나쁜 예
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

# 더 나은 방법
class Point:
    __slots__ = ['x', 'y']
    
    def __init__(self, x, y):
        self.x = x
        self.y = y
```

<div class="content-ad"></div>

## 8. os.walk을 사용하여 디렉터리 탐색을 간단히하세요

os.walk은 Python의 os 모듈에서 제공하는 유용한 함수입니다. 이 함수를 사용하면 주어진 루트 디렉터리에서 시작하여 디렉터리 트리를 탐색할 수 있습니다. 이 함수는 방문한 각 디렉터리에 대한 튜플을 생성하는 제너레이터를 반환합니다. 이 튜플에는 디렉터리 경로, 하위 디렉터리 목록 및 해당 디렉터리에 포함된 파일 목록이 포함됩니다.

위 예제를 사용하여 os.walk를 이용해 Duck1.png 파일에 액세스해보겠습니다.

```python
## Nahhh
import os
root_dir = '/path/to/root/directory'

## Yup!!!!!!!!
for dirpath, _, filenames in os.walk(root_dir):
    if 'Duck1.png' in filenames:
        file_path = os.path.join(dirpath, 'Duck1.png')
        print("Using os.walk:", file_path)
        break
```

<div class="content-ad"></div>

os.walk은 큰 디렉토리 구조 안의 파일에 쉽게 접근할 수 있는 편리한 방법입니다. 이를 사용하면 코드의 가독성이 높아지고 더 간결하고 명확해집니다. 웹 스크레이퍼, ML 엔지니어 및 Django 개발자에게 유용한 도구입니다.

## 9. ExitStack를 사용하여 여러 컨텍스트를 원활하게 관리하세요

중첩된 with 문을 사용하여 유지보수가 어려워지고 읽기가 어려운 "파라다임의 피라미드"를 만드는 대신, 동적으로 컨텍스트 매니저를 관리하기 위해 Contextlib.ExitStack()을 사용해보는 것이 좋습니다.

```js
## 나쁜 예시
def process_files(file1, file2, file3):
    with open(file1, 'r') as f1:
        with open(file2, 'r') as f2:
            with open(file3, 'r') as f3:
                # 파일 처리
                pass

## 훨씬 나은 예시
from contextlib import ExitStack

def process_files(file1, file2, file3):
    with ExitStack() as stack:
        f1 = stack.enter_context(open(file1, 'r'))
        f2 = stack.enter_context(open(file2, 'r'))
        f3 = stack.enter_context(open(file3, 'r'))
        # 파일 처리
```

<div class="content-ad"></div>

## 10. 서로 다른 네이밍 규칙을 섞지 마세요

함수에는 카멜 케이스를 사용하고 변수에는 파스칼 케이스를 사용하는 등 서로 다른 네이밍 규칙을 혼합하는 것은 일관성을 무너뜨리고 코드를 읽기 어렵게 만듭니다. 일관된 네이밍 규칙을 유지하는 것은 가독성과 유지보수성을 향상시키는 데 필수적입니다.

![이미지](https://miro.medium.com/v2/resize:fit:464/0*DykGa9Q969r8BUI6.gif)

가장 많이 사용되고 널리 받아들여진 네이밍 규칙 중 하나는 스네이크 케이스입니다. 여기서 식별자는 단어 사이에 밑줄을 사용하여 모두 소문자로 작성됩니다.

<div class="content-ad"></div>

```js
## 😣
def myFunction(num):
  MyVar = num/3.5
  return MyVar

## 🤠
def my_function(num):
  my_var = num/3.5
  return my_var
```

## 11. Missing Keys ?? defaultdict got you!!

```js
## Kinda Bad
from collections import defaultdict

word_count = {}
text = "the quick brown fox jumps over the lazy dog"
for word in text.split():
    if word not in word_count:
        word_count[word] = 0
    word_count[word] += 1

## Much Better
from collections import defaultdict

word_count = defaultdict(int)
text = "the quick brown fox jumps over the lazy dog"
for word in text.split():
      word_count[word] += 1
```

<img src="https://miro.medium.com/v2/resize:fit:598/0*hWMXgZDNREeqMB3j.gif" />

<div class="content-ad"></div>

## 12. 긴 함수를 더 작고 관리하기 쉬운 함수로 분리하세요

긴 함수는 코드를 읽고 이해하기 어렵게 할 뿐만 아니라 디버깅과 유지보수를 복잡하게 만듭니다. 함수가 지나치게 길어지면 그 목적, 로직 및 흐름을 파악하기 어려워져 코드 가독성이 떨어지게 됩니다.

```js
def process_data(data):
    # ... 많은 코드 ...
    # ... 더 많은 코드 ...
    # ... 더더 많은 코드 ...
    # ... 그리고 더 ...
    # ... 그리고도 계속 ...
    # ... 마침내, 끝 ...
    pass
```

더 나은 코딩 관행은 복잡한 로직을 작고 더 관리하기 쉬운 함수로 분해하는 것입니다. 작은 함수에서 문제를 식별하고 해결하는 것이 큰 함수보다 더 쉽습니다.

<div class="content-ad"></div>

작은 기능들은 디버깅, 코드 가독성, 유지 관리 및 코드 재사용성을 크게 향상시킵니다. 전반적으로 코드 품질을 향상시킵니다.

```js
def process_data(data):
    # ... 일부 코드 ...
    remove_email()
    remove_html_tags()
    remove_phone_numbers()
    remove_special_chars()

def remove_email():
  # ... 일부 코드...
  # fun3() 호출

def remove_html_tags():
  # ... 일부 코드...

def remove_phone_numbers():
  # ... 일부 코드...

def remove_special_chars():
  # ... 일부 코드..
```

## 13. 문자열 조작을 버리세요. 파일 확장자 변경에는 .with_suffix() 사용

파일의 확장자를 변경하는 일은 빈번하게 발생합니다. 이미 Path 객체가 있는 경우, 이를 문자열로 변환한 다음 slice하고 새로운 확장자를 추가할 필요가 없습니다. 대신, 원활한 작업을 위해 with_suffix() 함수를 사용해 볼 수 있습니다.

<div class="content-ad"></div>

```js
## 나쁜 예
new_filepath = str(Path("file.txt"))[:4] + ".md"

## 좋은 예
new_filepath = Path("file.txt").with_suffix(".md")
```

## 14. 컨테이너가 비어있는지 여부를 판별하기 위해 부울 체크를 사용할 수 있습니다.

```js
## 나쁜 예
items = []
if len(items) == 0:
    print("리스트에 아이템이 없습니다.")

## 좋은 예
items = []
if not items:
    print("리스트에 아이템이 없습니다.")
```

## 15. 인라인 if 문을 단일 또는 표현식으로 간단하게 만들기

<div class="content-ad"></div>


# 나쁨
z = x if x else y


## 좋음
z = x or y


## 16. 사전에서 빠르고 쉬운 키 확인

사전에 키가 있는지만 확인하려면 .keys()를 호출할 필요가 없습니다. 사전 자체에 in을 사용하면 됩니다.


## 나쁨
d = {"key": "value"} if "key" in d.keys():

## 좋음
d = {"key": "value"} if "key" in d:


<div class="content-ad"></div>

## 17. 쓸모없는 반환 값의 함정 피하기

함수에서 의미 있는 반환 값을 사용하지 않으면 코드의 이해도가 떨어질 수 있습니다. 호출될 때 코드의 다른 부분에서 활용할 수 있는 유용한 정보를 제공하는 반환 값을 갖는 함수를 설계하는 것이 중요합니다.

```js
## 조금 나쁜 예
def calculate_sum_of_n_numbers(numbers):
    total = sum(numbers)
    print("합계:", total)

numbers_list = [11, 45, 32, 49, 56]
calculate_sum_of_n_numbers(numbers_list)
```

위의 예시에서 calculate_sum_of_n_numbers() 함수는 리스트의 합계를 계산하지만 반환하지 않고 데이터를 출력합니다. print 대신에 값을 반환하여 더 나은 코드 흐름을 만드는 것이 좋습니다.

<div class="content-ad"></div>

```js
## 좋은 코드
def calculate_sum_of_n_numbers(numbers):
    total = sum(numbers)
    return total

numbers_list = [1, 2, 3, 4, 5]
result = calculate_sum_of_n_numbers(numbers_list)
print("합계:", result)
```

## 18. 비교 체인을 사용하여 동등성 확인 간소화하기

여러 객체가 서로 동일한지 확인할 때는 and 표현식을 사용하지 말고 대신 비교 체인을 사용하세요. 예를 들어 다음과 같습니다.

```js
## 안 좋은 코드
if x == y and x == z: 
 pass

## 좋은 코드
if x == y == z: 
 pass
```

<div class="content-ad"></div>

## 19. 코드 최적화 팁: 람다 대신 연산자 활용하기

내장 연산자를 감싸는 람다/함수를 작성하지 말고, 대신 operatormodule을 사용하세요.

```js
## 부적절한 예
from functools import reduce 
nums = [1, 2, 3] 
print(reduce(lambda x, y: x + y, nums)) # 6


## 더 나은 예
from functools import reduce 
from operator import add 
nums = [1, 2, 3] 
print(reduce(add, nums)) # 6
```

## 20. 문자열에서 탭을 다루는 더 좋은 방법

<div class="content-ad"></div>

"\\t"을 " " * 8으로 바꾸는 대신에 expandtabs() 메소드를 사용할 수 있습니다. 이 방법이 더 간결하고 명확합니다. 또한, 선택적으로 탭 너비를 지정할 수 있는 매개변수를 허용합니다.

```js
## 나쁜 예시
spaces_8 = "hello\\tworld".replace("\\t", " " * 8) 
spaces_4 = "hello\\tworld".replace("\\t", " ")

## 좋은 예시
spaces_8 = "hello\\tworld".expandtabs() 
spaces_4 = "hello\\tworld".expandtabs(4)
```

![이미지](https://miro.medium.com/v2/resize:fit:598/0*lQWvKwZTQ-NEKyBa.gif)

## 21. 코드에 민감한 정보 하드코딩 피하기

<div class="content-ad"></div>

민감한 정보를 하드코딩하는 것은 비밀번호, 보안 코드 또는 API 키와 같은 정보를 잠재적인 공격자에게 노출시켜 중대한 보안 위험을 초래할 수 있습니다. 이러한 실천은 데이터 유출이나 계정 침해로 이어질 수 있습니다.

```js
password = "iLOVEcats356@33"
```

민감한 정보를 저장할 때 환경 변수나 구성 파일과 같은 대안적인 방법을 사용하는 것이 좋습니다. 환경 변수는 데이터를 코드베이스 외부에 안전하게 유지하는 안전한 방법을 제공하며, 구성 파일은 암호화되거나 코드베이스와 분리된 안전한 위치에 저장될 수 있습니다.

## 22. 파일에 쓰기 위해 항상 open이 필요한 것은 아닙니다

<div class="content-ad"></div>

간단히 파일에 내용을 저장해야 할 때는 with 블록을 사용하는 것보다 pathlib의 write_text() 함수를 사용하는 것이 더 나을 수 있습니다.

```js
## 나쁜 예
content = "안녕, 세상!"
with open("example.txt", "w") as file:
    file.write(content)

## 좋은 예
from pathlib import Path
content = "안녕, 세상!"
Path("example.txt").write_text(content)
```

## 23. except 블록을 비워두지 마세요

```js
## 나쁜 예
try:
    file = open("data.txt", "r")
    # 파일 작업 수행
except:
    # 에러 처리
    pass

## 좋은 예
try:
    file = open("data.txt", "r")
    # 파일 작업 수행
    # ...
    file.close()
except FileNotFoundError:
    print("파일을 찾을 수 없습니다!")
except IOError as e:
    print("파일 처리 중 오류가 발생했습니다:", str(e))
```

<div class="content-ad"></div>

## 24. 딕셔너리 값에 더 나은 액세스 방법: get() 메서드 시도하기

딕셔너리의 값을 액세스할 때, get() 메서드를 사용하는 것이 일반적인 인덱싱보다 더 안전하고 간결할 수 있습니다. get() 메서드를 사용하면 키가 발견되지 않을 경우 기본값을 제공할 수 있어서 잠재적인 KeyError 예외를 피할 수 있습니다.

```js
## 권장 X
data = {"name": "Alice", "age": 30}

# 일반적인 인덱싱, 키가 없을 경우 KeyError 발생 가능
name = data["name"]
city = data["city"] if "city" in data else "Unknown"

## 권장
data = {"name": "Alice", "age": 30}

# 기본값과 함께 get() 메서드 사용하여 안전하게 딕셔너리 값에 액세스
name = data.get("name", "Unknown")
city = data.get("city", "Unknown")
```

## 25. 조건문을 더 나은 방법으로 작성하기 위해 Match 및 Case 활용하기

<div class="content-ad"></div>


## 나쁜 방법
def describe_type(obj):
    if isinstance(obj, str):
        return "문자열입니다"
    elif isinstance(obj, int):
        return "정수입니다"
    elif isinstance(obj, list):
        return "리스트입니다"
    else:
        return "다른 것입니다"

## 괜찮은 방법
def describe_type(obj):
    match obj:
        case str():
            return "문자열입니다"
        case int():
            return "정수입니다"
        case list():
            return "리스트입니다"
        case _:
            return "다른 것입니다"


여기까지 읽어주셔서 감사합니다. 내 콘텐츠를 좋아하시고 지원하고 싶다면 —

- 박수👋와 생각💬을 남겨주세요.
- 저를 Medium에서 팔로우해주세요.
- LinkedIn에서 저와 연결해주세요.
- 제 메일 목록에 가입하여 제 다음 기사를 놓치지 마세요.
- 비슷한 이야기를 더 읽으려면 Pythoneers Publication을 팔로우해주세요.

<img src="https://miro.medium.com/v2/resize:fit:996/0*6nbkLdnB_zc3wWXQ.gif" />
