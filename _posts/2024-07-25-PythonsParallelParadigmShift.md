---
title: "Python의 병렬 처리 패러다임 변화"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-07-25 11:48
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Pythons Parallel Paradigm Shift"
link: "https://medium.com/towards-data-science/pythons-parallel-paradigm-shift-2bb40d3c2dd0"
isUpdated: true
---




## 무슨 일이 일어나고 있나요?

Python에서 가장 기대되는 변경 사항 중 하나인 GIL 제거에 관한 변화(PEP 703 참조)가 지금 테스트를 위해 준비되어 있습니다. 현재 미리 릴리스된 Python 버전(3.13.0b4)은 --disable-gil 플래그로 빌드된 경우 글로벌 인터프리터 락을 해제하여 실행할 수 있는 실험적인 지원이 가능합니다. 이 플래그로 빌드된 Python은 free-threaded 버전의 Python이라고도 합니다.

이 문서에서는 이 Python 버전을 빌드하는 방법과 GIL이 활성화되지 않은 경우와 활성화된 경우의 코드 예제 몇 가지를 보여주어 실행 시간에 어떤 차이가 있는지 살펴볼 것입니다.

## 이게 왜 중요한가요?

<div class="content-ad"></div>

한 마디로, "성능".

자유 스레딩 실행은 시스템의 모든 사용 가능한 코어를 동시에 사용할 수 있기 때문에 코드가 종종 더 빠르게 실행됩니다. 데이터 과학자, ML 또는 데이터 엔지니어로서 이는 귀하의 코드뿐만 아니라 시스템, 프레임워크 및 라이브러리를 빌려온 코드에도 적용됩니다.

많은 기계 학습 및 데이터 과학 작업은 CPU 집약적이며 특히 모델 훈련 및 데이터 전처리 중에 CPU를 많이 사용합니다. GIL이 제거되면 CPU 집약적 작업에 대한 중요한 성능 향상이 있을 수 있습니다.

파이썬의 많은 인기 있는 ML 및 데이터 과학 라이브러리(예: NumPy, Dask, Pandas, Scikit-learn)는 GIL 주변에서 작업해야 했기 때문에 제약을 겪고 있습니다. 이것이 제거된다면:-

<div class="content-ad"></div>

- 이러한 라이브러리의 간소화된, 효율적인 구현
- 기존 라이브러리에서의 새로운 최적화 기회
- 병렬 처리를 최대한 활용할 수 있는 새로운 라이브러리 개발

## 사전 준비물

이미 GIL이 없는 Python 버전을 빌드하고 제공한 사람을 찾을 수 있는 경우가 아니라면 직접 빌드해야 합니다.

걱정 마세요. 들릴 때만큼 복잡하지 않아요.

<div class="content-ad"></div>

이렇게 소프트웨어를 개발하는 것은 Linux 기반 시스템에서 훨씬 쉬우며 저는 윈도우를 사용하기 때문에 먼저 WSL2 Ubuntu를 설치하기로 결정했습니다.

이전에 이렇게 한 적이 없다면, 아래 링크를 따라 읽을 수 있는 설치 가이드를 작성했습니다.

## 무료 쓰레드 Python 빌드 및 설치

다음 단계를 따라 주세요.

<div class="content-ad"></div>

1. 파이썬 버전 선택

현재까지 알기로, 모든 미리 릴리스된 3.13 버전에서 GIL 비활성화 플래그를 사용할 수 있습니다. 따라서 3.13.0b1, 3.13.0b2, 3.13.0b3 및 3.13.0b4가 이에 해당합니다. 다른 버전들도 있을 수 있습니다.

제가 선택한 것은 최신 3.13.0b4 버전입니다.

다운로드하려면 여기를 클릭하세요.

<div class="content-ad"></div>

화면은 이렇게 보여야 해요.

시스템에 다운로드하려는 버전을 클릭하세요. 저는 3.13.0b4 버전을 위한 압축 해제된 소스 tarball을 사용했어요.

2/ 빌드 파일을 보관할 디렉토리를 생성하세요.

제가 보통 작업하는 우분투 시스템에는 프로젝트 디렉토리가 있어요.

<div class="content-ad"></div>


```js
$ cd
$ mkdir projects
```

3/ 다운로드한 Python tarball 파일을 프로젝트 디렉토리로 복사하고 해제하세요.

```js
$ cd ~/projects
$ # tarball 파일을 bash, DOS CMD Line 또는 수동으로 projects 폴더로 복사합니다.
$ #
$ tar xzf Python-3.13.0b4.tgz
```

4/ 미리 빌드된 종속성을 설치하세요


<div class="content-ad"></div>

```js
$ # 위의 tar 명령어는 이 폴더를 생성했습니다
$ cd ~/projects/Python-3.13.0b4
$ # 의존성 설치
$ sudo apt install build-essential zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev libssl-dev libsqlite3-dev libreadline-dev libffi-dev wget libbz2-dev
```

5/ disable-gil 옵션 구성

```js
$ ./configure --disable-gil
```

6/ Python 빌드 및 설치


<div class="content-ad"></div>

```js
$ make
$ sudo make install
```

7/ 빌드가 제대로 작동하는지 확인해주세요

## 개발 환경 설정

새로운 Python 버전을 사용하는 개발 환경을 설정해봅시다. 저는 이를 위해 conda를 사용하지만, 여러분이 편안하게 사용할 수 있는 도구를 선택해주시면 됩니다.

<div class="content-ad"></div>

```shell
$ # 아래의 파이썬 버전은 임시로 넣어둔 것이에요
$ # 곧 수정할 거에요
$ #
$ conda create -n gil_free_python python=3.11
$ conda activate gil_free_python
$ # 3.11 버전을 제거해줘요
$ conda remove python

채널:
 - defaults
 - conda-forge
플랫폼: linux-64
패키지 메타데이터 수집 (repodata.json): 완료
환경 해결: 완료
## 패키지 계획 ##
  환경 위치: /home/tom/miniconda3/envs/gil_free_python
  지정 삭제 사항:
    - python

다음 패키지가 제거됩니다:
  bzip2-1.0.8-h5eee18b_6
  ld_impl_linux-64-2.38-h1181459_1
  libffi-3.4.4-h6a678d5_1
  libstdcxx-ng-11.2.0-h1234567_1
  libuuid-1.41.5-h5eee18b_0
  ncurses-6.4-h6a678d5_0
  pip-24.0-py311h06a4308_0
  python-3.11.9-h955ad1f_0
  readline-8.2-h5eee18b_0
  setuptools-69.5.1-py311h06a4308_0
  sqlite-3.45.3-h5eee18b_0
  tk-8.6.14-h39e8969_0
  tzdata-2024a-h04d1e81_0
  wheel-0.43.0-py311h06a4308_0
  xz-5.4.6-h5eee18b_1
  zlib-1.2.13-h5eee18b_1

계속하시겠습니까? ([y]/n) y
트랜잭션 준비: 완료
$
$ # 이제 새로운 3.13 버전에 링크하기
$ ln -s /usr/local/bin/python3.13 $CONDA_PREFIX/bin/python
$ python --version


Python 3.13.0b4
```

방금 빌드한 Python 버전은 Python이 실행되는 방식을 제어하는 여러 구현별 플래그를 전달할 수 있게 해줍니다. 이를 보려면 다음 명령을 실행하세요.

```shell
$ python --help-xoptions



다음의 구현별 옵션들을 사용할 수 있어요:
-X cpu_count=N: os.cpu_count()의 반환 값을 재정의해요;
         -X cpu_count=default를 사용하면 재정의가 취소돼요; 또한 PYTHON_CPU_COUNT
-X dev: Python 개발 모드를 활성화해요; 또한 PYTHONDEVMODE
-X faulthandler: 치명적인 오류 발생 시 Python 추적 정보를 출력해요;
         또한 PYTHONFAULTHANDLER
-X frozen_modules=[on|off]: 냉동 모듈의 사용 여부; 설치된 Python에는 기본적으로 "on"이 되고
         로컬 빌드의 경우 "off"가 기본값이 됩니다; 또한 PYTHON_FROZEN_MODULES
-X gil=[0|1]: GIL을 활성화(1) 또는 비활성화(0)시켜요; 또한 PYTHON_GIL
-X importtime: 각 import에 소요되는 시간을 표시해요; 또한 PYTHONPROFILEIMPORTTIME
-X int_max_str_digits=N: int<->str 변환의 크기 제한을 설정해요;
         0으로 설정하면 제한이 해제돼요; 또한 PYTHONINTMAXSTRDIGITS
-X no_debug_ranges: 코드 객체에 추가 위치 정보를 포함하지 않아요;
         또한 PYTHONNODEBUGRANGES
-X perf: Linux "perf" 프로파일러를 지원해요; 또한 PYTHONPERFSUPPORT=1
-X pycache_prefix=경로: .pyc 파일을 코드 트리가 아닌 병렬 트리에 써요;
         또한 PYTHONPYCACHEPREFIX
-X showrefcount: 프로그램 종료 시나 인터랙티브 인터프리터에서 각 문장 이후에
         참조 카운트와 사용 중인 메모리 블록 수를 출력해요; 디버그 빌드에서만 작동돼요
-X tracemalloc[=N]: Python 메모리 할당을 추적해요; N은 추적 범위 제한을 설정해요
         N 프레임 (기본값: 1); 또한 PYTHONTRACEMALLOC=N
-X utf8[=0|1]: UTF-8 모드를 활성화(1) 또는 비활성화(0)시켜요; 또한 PYTHONUTF8
-X warn_default_encoding: 'encoding=None'에 대한 선택적인 EncodingWarning을 활성화해요;
         또한 PYTHONWARNDEFAULTENCODING
```

여기서 우리가 관심을 가지는 것이에요.

<div class="content-ad"></div>

- -X gil=[0|1]: GIL을 활성화(1) 또는 비활성화(0)합니다. 또한 PYTHON_GIL

## GIL 없이 Python 테스트

테스트를 위해 멀티스레딩 및 멀티프로세싱 Python 스크립트를 생성하고, 위의 플래그를 사용하여 Python이 GIL을 사용하는지 여부를 지정할 것입니다.

예제 Python 스크립트 1

<div class="content-ad"></div>

우리의 첫 번째 스크립트는 소수를 찾기 위해 몇 가지 계산을 수행합니다.

```js
import threading
import time
import multiprocessing

def is_prime(n):
    """주어진 숫자가 소수인지 확인합니다."""
    if n < 2:
        return False
    for i in range(2, int(n**0.5) + 1):
        if n % i == 0:
            return False
    return True

def find_primes(start, end):
    """지정된 범위 내의 모든 소수를 찾습니다."""
    primes = []
    for num in range(start, end + 1):
        if is_prime(num):
            primes.append(num)
    return primes

def worker(worker_id, start, end):
    """특정 범위에서 소수를 찾는 워커 함수입니다."""
    print(f"워커 {worker_id} 시작")
    primes = find_primes(start, end)
    print(f"워커 {worker_id}가 {len(primes)}개의 소수를 찾았습니다")

def main():
    """다중 스레드로 소수를 찾는 메인 함수입니다."""
    start_time = time.time()

    # CPU 코어 수 가져오기
    num_cores = multiprocessing.cpu_count()
    print(f"CPU 코어 수: {num_cores}")

    # 소수 검색을 위한 범위 정의
    total_range = 2_000_000
    chunk_size = total_range // num_cores

    threads = []
    # 코어 수와 동일한 수의 스레드 생성 및 시작
    for i in range(num_cores):
        start = i * chunk_size + 1
        end = (i + 1) * chunk_size if i < num_cores - 1 else total_range
        thread = threading.Thread(target=worker, args=(i, start, end))
        threads.append(thread)
        thread.start()

    # 모든 스레드가 완료될 때까지 기다림
    for thread in threads:
        thread.join()

    # 총 실행 시간 계산 및 출력
    end_time = time.time()
    total_time = end_time - start_time
    print(f"모든 워커가 {total_time:.2f}초 안에 완료되었습니다")

if __name__ == "__main__":
    main()
```

is_prime 함수는 주어진 숫자가 소수인지 확인합니다.

find_primes 함수는 주어진 범위 내의 모든 소수를 찾습니다.

<div class="content-ad"></div>

테이블 태그를 마크다운 형식으로 변경하세요.

<div class="content-ad"></div>


$ # GIL_ENABLED
$ #
$ python -X gil=1 pytest.py

Number of CPU cores: 32
Worker 0 starting
Worker 1 starting
Worker 2 starting
Worker 3 starting
Worker 4 starting
Worker 5 starting
Worker 0 found 6275 primes
Worker 6 starting
Worker 7 starting
Worker 8 starting
Worker 1 found 5459 primes
Worker 3 found 5080 primes
...
...
Worker 31 starting
Worker 10 found 4640 primes
Worker 16 found 4501 primes
Worker 23 found 4403 primes
...
...
Worker 22 found 4439 primes
Worker 30 found 4338 primes
Worker 29 found 4345 primes
Worker 27 found 4346 primes
Worker 28 found 4338 primes
Worker 31 found 4304 primes

All workers completed in 5.63 seconds



$ # GIL_DISABLED
$ #
$ python -X gil=0 pytest.py

Number of CPU cores: 32
Worker 0 starting
Worker 1 starting
Worker 2 starting
...
...
Worker 29 starting
Worker 30 starting
Worker 31 starting
Worker 0 found 6275 primes
Worker 1 found 5459 primes
Worker 2 found 5230 primes
...
...
Worker 26 found 4377 primes
Worker 29 found 4345 primes
Worker 30 found 4338 primes
Worker 25 found 4346 primes
Worker 28 found 4338 primes
Worker 31 found 4304 primes

All workers completed in 0.48 seconds

OK, that was impressive. If my maths are correct, the NO-GIL version outperformed the GIL version by … a lot!

Example Python Script 2


<div class="content-ad"></div>

이 예제에서는 concurrent.futures 모델을 사용하여 여러 텍스트 파일을 동시에 읽고 각 파일의 라인 수와 단어 수를 세어 표시할 것입니다.

이를 하기 전에 처리할 데이터 파일이 필요합니다. Faker 라이브러리를 사용하여 그 작업을 했습니다. 그냥 20개의 별도의 텍스트 파일에 각각 1,000,000개의 무작위, 허구의 문장을 작성합니다.

참고: faker를 설치해야 합니다. 그러나 이 작업을 수행하면 Python 3.12도 함께 설치됩니다. 따라서 Python 3.13 버전과 다른 환경에 설치하도록 주의하십시오.

여기 파일을 생성하는 코드입니다.

<div class="content-ad"></div>

```js
$ # 데이터 파일을 보관할 data 폴더를 생성해보세요
$ cd ~/projects
$ mkdir data
```

```js
# faker 라이브러리를 사용하여 1,000,000개의 임의의 문장을 포함하는
# 파일 20개를 생성해보세요
#
from faker import Faker
import random
import concurrent.futures
import time

def generate_sample_file(filename, num_sentences=1000000):
    start_time = time.time()
    fake = Faker()

    sentences = [fake.sentence(nb_words=random.randint(5, 15)) for _ in range(num_sentences)]

    with open(filename, 'w') as file:
        file.write('\n'.join(sentences))

    return filename, end_time - start_time

def main():
    num_files = 20
    max_workers = 10  # 동시 작업자 수

    with concurrent.futures.ThreadPoolExecutor(max_workers=max_workers) as executor:
        # Future 객체의 목록 생성
        futures = [executor.submit(generate_sample_file, f"./data/file_{i}.txt") for i in range(1, num_files + 1)]

        # 완료되는대로 결과 처리
        for future in concurrent.futures.as_completed(futures):
            try:
                filename, duration = future.result()
                print(f"{filename}을 {duration:.2f}초만에 생성했습니다")
            except Exception as exc:
                print(f'예외가 발생했습니다: {exc}')

if __name__ == "__main__":
    main()
```

첫 번째로 생성된 파일 일부의 예시입니다.

```js
$ sed 10q file_1.txt

Remember build raise get section leader accept.
His call read act opportunity for imagine full if build.
Any contain book south note describe.
Pull law seven wind add.
Test most material gas likely military arm wife mouth his.
Ability affect fire personal choose thing law heavy rock have.
How hospital check.
According choose pressure sell support.
Relate rule article address writer sense character these.
Fly traditional same us teacher manage develop either seven.
```

<div class="content-ad"></div>

잘 하셨습니다. 이제 파일을 읽고 처리하는 코드에 들어갑니다.

```js
import concurrent.futures
import os
import time

def process_file(filename):
    """
    단일 파일을 처리하여 라인 수와 단어 수를 반환합니다.
    """
    try:
        with open(filename, 'r') as file:
            content = file.read()
            lines = content.split('\n')
            words = content.split()
            return filename, len(lines), len(words)
    except Exception as e:
        return filename, -1, -1  # 에러가 발생하면 두 카운트에 대해 -1을 반환합니다.

def main():
    start_time = time.time()  # 타이머 시작

    # 파일을 보관할 리스트
    files = [f"./data/file_{i}.txt" for i in range(1, 21)]  # file_1.txt부터 file_20.txt까지 20개의 파일 생성

    # 파일을 병렬로 처리하기 위해 ThreadPoolExecutor 사용
    with concurrent.futures.ThreadPoolExecutor(max_workers=10) as executor:
        # 모든 파일 처리 작업 제출
        future_to_file = {executor.submit(process_file, file): file for file in files}

        # 완료될 때마다 결과 처리
        for future in concurrent.futures.as_completed(future_to_file):
            file = future_to_file[future]
            try:
                filename, line_count, word_count = future.result()
                if line_count == -1:
                    print(f"{filename} 처리 중 오류 발생")
                else:
                    print(f"{filename}: {line_count} 라인, {word_count} 단어")
            except Exception as exc:
                print(f'{file}에서 예외 발생: {exc}')

    end_time = time.time()  # 타이머 끝
    print(f"총 실행 시간: {end_time - start_time:.2f} 초")

if __name__ == "__main__":
    main()
```

실행 시간

```js
$ # GIL_ENABLED
$ #
$ python -X gil=1 pytest_read_files.py

./data/file_3.txt: 1000000 라인, 9520545 단어
./data/file_4.txt: 1000000 라인, 9520829 단어
./data/file_7.txt: 1000000 라인, 9513063 단어
./data/file_6.txt: 1000000 라인, 9525163 단어
./data/file_9.txt: 1000000 라인, 9513434 단어
./data/file_1.txt: 1000000 라인, 9514814 단어
./data/file_11.txt: 1000000 라인, 9514737 단어
./data/file_10.txt: 1000000 라인, 9520212 단어
./data/file_12.txt: 1000000 라인, 9518785 단어
./data/file_15.txt: 1000000 라인, 9520427 단어
./data/file_5.txt: 1000000 라인, 9513879 단어
./data/file_2.txt: 1000000 라인, 9525032 단어
./data/file_8.txt: 1000000 라인, 9524547 단어
./data/file_13.txt: 1000000 라인, 9520297 단어
./data/file_14.txt: 1000000 라인, 9516835 단어
./data/file_17.txt: 1000000 라인, 9520110 단어
./data/file_16.txt: 1000000 라인, 9507996 단어
./data/file_18.txt: 1000000 라인, 9514181 단어
./data/file_20.txt: 1000000 라인, 9516411 단어
./data/file_19.txt: 1000000 라인, 9522945 단어

총 실행 시간: 10.04 초
```

<div class="content-ad"></div>

```json
# GIL_DISABLED
#
$ python -X gil=0 pytest_read_files.py

./data/file_2.txt: 1000000 lines, 9525032 words
./data/file_4.txt: 1000000 lines, 9520829 words
./data/file_7.txt: 1000000 lines, 9513063 words
./data/file_1.txt: 1000000 lines, 9514814 words
./data/file_5.txt: 1000000 lines, 9513879 words
./data/file_3.txt: 1000000 lines, 9520545 words
./data/file_8.txt: 1000000 lines, 9524547 words
./data/file_10.txt: 1000000 lines, 9520212 words
./data/file_6.txt: 1000000 lines, 9525163 words
./data/file_9.txt: 1000000 lines, 9513434 words
./data/file_16.txt: 1000000 lines, 9507996 words
./data/file_13.txt: 1000000 lines, 9520297 words
./data/file_18.txt: 1000000 lines, 9514181 words
./data/file_11.txt: 1000000 lines, 9514737 words
./data/file_12.txt: 1000000 lines, 9518785 words
./data/file_17.txt: 1000000 lines, 9520110 words
./data/file_15.txt: 1000000 lines, 9520427 words
./data/file_14.txt: 1000000 lines, 9516835 words
./data/file_19.txt: 1000000 lines, 9522945 words
./data/file_20.txt: 1000000 lines, 9516411 words

총 실행 시간: 2.13 초
```

첫 번째 예제와 마찬가지로 Python GIL이 해제된 상태에서 실행 시간이 크게 개선되었습니다.

예제 Python 스크립트 3

마지막 예제에서는 multiprocessing 모듈을 사용하여 행렬 곱셈을 해보겠습니다.

<div class="content-ad"></div>

여기 코드입니다.

```js
import multiprocessing
import time

def multiply_matrices(A, B, result, start_row, end_row):
    """A와 B의 부분 행렬을 곱하고 결과를 result의 해당 부분 행렬에 저장합니다."""
    for i in range(start_row, end_row):
        for j in range(len(B[0]):
            sum = 0
            for k in range(len(B)):
                sum += A[i][k] * B[k][j]
            result[i][j] = sum

def main():
    """Multi-process 행렬 곱셈을 조정하는 주요 기능입니다."""
    start_time = time.time()

    # 행렬 크기 정의
    size = 1000
    A = [[1 for _ in range(size)] for _ in range(size)]
    B = [[1 for _ in range(size)] for _ in range(size)]
    result = multiprocessing.Manager().list([[0 for _ in range(size)] for _ in range(size)])

    # CPU 코어 수 가져오기
    num_cores = multiprocessing.cpu_count()
    print(f"CPU 코어 수: {num_cores}")

    chunk_size = size // num_cores

    processes = []
    # 코어 수와 동일한 수의 프로세스 생성 및 시작
    for i in range(num_cores):
        start_row = i * chunk_size
        end_row = size if i == num_cores - 1 else (i + 1) * chunk_size
        process = multiprocessing.Process(target=multiply_matrices, args=(A, B, result, start_row, end_row))
        processes.append(process)
        process.start()

    # 모든 프로세스가 완료될 때까지 대기
    for process in processes:
        process.join()

    end_time = time.time()  # 타이머 끝내기
    print(f"총 실행 시간 (행렬 곱셈): {end_time - start_time:.2f} 초")

if __name__ == "__main__":
    main()
```

측정 시간

```js
$ # GIL_ENABLED
$ #
~$ python -X gil=1 pytest_matmult.py
CPU 코어 수: 32
총 실행 시간 (행렬 곱셈): 68.46 초
```

<div class="content-ad"></div>

```js
$ # GIL_DISABLED
$ #
python -X gil=0 pytest_matmult.py
Number of CPU cores: 32
Total execution time (matrix multiplication): 9.21 seconds
```

다시 한 번 GIL이 비활성화된 버전에서 놀라운 성능 향상을 볼 수 있었습니다. 이것은 단순한 예제에 불과함을 명심하세요. 외부 라이브러리인 NumPy를 사용하여 행렬 곱셈을 수행하면 심지어 내 GIL이 비활성 상태인 예제보다 속도가 빠릅니다.

이번 테스트에서 흥미로운 점은 멀티 스레딩 버전의 코드를 가지고도 테스트했을 때 였습니다. 이 버전이 다중 프로세스 버전보다 상당히 느렸다는 사실이 놀라웠습니다. 이는 GIL이 없어야 할 것으로 예상되는 것과는 상반되는 결과입니다. 실제로 이전 두 예제에서는 시도한 다중 프로세스 버전이 제시한 멀티 스레딩 버전보다 훨씬 느렸습니다. 이것은 버그일 수도 있고 다른 메모리/동기화 문제일 수도 있습니다. 이 이상현상을 설명해 줄 아이디어가 있는 독자분이 있다면 댓글로 의견을 주시면 감사하겠습니다.

## 요약


<div class="content-ad"></div>

벌써 흥분되셨나요? 파이썬의 다가오는 버전에서 GIL 없이 Python 프로그램을 실행할 수 있는 능력이 생긴다면 정말 기대할 만한 일이 될 것입니다. 제가 보았던 타이밍 개선을 생각해 본다면 말이죠.

그러나 우리는 GIL이 파이썬에 도입된 이유를 기억해야 합니다. 파이썬 코더로서, 우리는 모두 작성한 코드가 메모리 조각을 넘어서거나 레이스 조건을 일으키거나 프로그램을 의도치 않게 망가뜨리지 않도록 책임져야 합니다. 아무튼 GIL이 있든 없든 항상 그렇죠!

만약 이 콘텐츠를 즐기셨다면, 다른 내가 쓴 기사들도 궁금해하실 수 있습니다.