---
title: "자바스크립트 예제와 함께 배우는 빅오 표기법 규칙"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-26 18:21
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Rules of Big-O Notation with JavaScript Examples"
link: "https://medium.com/@Abdullah-Umer/rules-of-big-o-notation-with-javascript-examples-060a3fd31b42"
isUpdated: false
---


빅-O 표기법은 입력 크기가 증가함에 따라 알고리즘의 실행 시간 또는 공간 요구 사항이 어떻게 증가하는지를 이해하는 데 필수적입니다. 이제 주요 규칙에 대해 자세히 설명하고 JavaScript 예제를 살펴봅시다.

## 1. 상수 곱셈 규칙

설명:
함수를 상수로 곱할 때, 빅-O 표기법은 변하지 않습니다. 이유는 빅-O가 런타임이 입력 크기에 따라 어떻게 증가하는지에 초점을 맞추기 때문이며, 상수는 큰 입력에 대한 성장 속도에 큰 영향을 미치지 않습니다.

```js
function constantMultiplication(arr) {
    let sum = 0;
    for (let i = 0; i < arr.length; i++) {
        sum += 5 * arr[i];
    }
    return sum;
}
```

<div class="content-ad"></div>

Big-O 분석:

- 루프가 n번(여기서 n은 배열의 길이) 실행됩니다.
- 각 요소를 5로 곱해도 성장률에 영향을 미치지 않습니다.
- Big-O: O(n), O(5n)이 아닙니다.

## 2. 합의 법칙

설명:
여러 단계가 추가되는 경우(서로 다른 루프 또는 연산 등), 전체 복잡성은 가장 빠르게 성장하는 항에 의해 결정됩니다. 느리게 성장하는 항들은 입력 크기가 커질수록 무시됩니다.

<div class="content-ad"></div>

```js
function sumRule(arr) {
    let sum1 = 0;
    for (let i = 0; i < arr.length; i++) {
        sum1 += arr[i]; // O(n)
    }
    
    let sum2 = 0;
    for (let i = 0; i < arr.length * arr.length; i++) {
        sum2 += i; // O(n^2)
    }
    
    return sum1 + sum2;
}
```

Big-O Analysis:

- The first loop runs n times, contributing O(n)
- The second loop runs n² times, contributing O(n²)
- The total Big-O is determined by the largest term, O(n²).
- Big-O: O(n²)

## 3. Product Rule

<div class="content-ad"></div>

설명:
중첩된 루프가 있을 때는 각 루프의 복잡성을 곱하여 총 Big-O 값을 얻습니다. 그 이유는 각 루프의 복잡성이 서로 합쳐지기 때문입니다.

```js
function productRule(matrix) {
    let sum = 0;
    for (let i = 0; i < matrix.length; i++) {  // O(n)
        for (let j = 0; j < matrix[i].length; j++) {  // O(m)
            sum += matrix[i][j];
        }
    }
    return sum;
}
```

Big-O 분석:

- 외부 루프는 n번 실행됩니다.
- 내부 루프는 외부 루프의 각 반복에 대해 m번 실행됩니다.
- 전체 시간 복잡도는 O(n) × O(m) = O(n×m)입니다.
- Big-O: O(n×m)

<div class="content-ad"></div>

## 4. 비주요항을 제거하십시오

설명:
여러 항이 있는 표현식(예: n³ + n² + n)이 있을 때, n이 증가함에 따라 가장 빠르게 증가하는 항에 의해 Big-O가 지배됩니다. 큰 입력 크기에 대해서는 느리게 증가하는 항이 무시되기 때문에 삭제됩니다.

```js
function dropNonDominantTerms(arr) {
    let sum = 0;
    
    // O(n)
    for (let i = 0; i < arr.length; i++) {
        sum += arr[i];
    }
    
    // O(n^3)
    for (let i = 0; i < arr.length; i++) {
        for (let j = 0; j < arr.length; j++) {
            for (let k = 0; k < arr.length; k++) {
                sum += i + j + k;
            }
        }
    }
    
    return sum;
}
```

Big-O 분석:

<div class="content-ad"></div>

- 첫 번째 루프의 시간 복잡도는 O(n)입니다.
- 두 번째 중첩된 루프 세트의 시간 복잡도는 O(n³)입니다.
- 전체 Big-O는 O(n³)에 의해 지배됩니다.
- Big-O: O(n³)

## 5. 로그 적용 규칙

설명:
각 단계에서 문제 크기를 일정한 비율로 줄이는 알고리즘(예: 문제를 반으로 나누는 것)은 로그 시간 복잡도를 가지게 됩니다. 이는 이진 탐색과 같은 분할 정복 알고리즘에서 일반적으로 발견됩니다.

```js
function binarySearch(arr, target) {
    let left = 0;
    let right = arr.length - 1;
    
    while (left <= right) {
        let mid = Math.floor((left + right) / 2);
        
        if (arr[mid] === target) {
            return mid; // 타겟 발견
        } else if (arr[mid] < target) {
            left = mid + 1; // 오른쪽 반으로 이동
        } else {
            right = mid - 1; // 왼쪽 반으로 이동
        }
    }
    
    return -1; // 타겟을 찾을 수 없음
}
```

<div class="content-ad"></div>

빅오 분석:

- 각 단계는 문제 크기를 절반으로 줄입니다.
- 배열 크기를 1로 줄이는 데 필요한 단계 수는 O (nlogn)입니다.
- 빅오: O (logn).

## 결론

빅오 표기법의 규칙을 이해하는 것은 알고리즘의 효율성을 평가하고 비교하는 데 중요합니다. 이러한 규칙들 - 상수 곱셈자를 무시하는 것, 주된 항에 초점을 맞추는 것, 로그 증가와 같은 패턴을 인식하는 것 - 은 알고리즘의 시간 복잡도를 결정하는 과정을 단순화하는 데 도움이 됩니다. 이러한 원칙을 적용함으로써 입력 크기에 따라 알고리즘이 어떻게 확장되는지 더 잘 이해할 수 있으며 코딩 과제에 대해 더 효율적인 해결책을 선택하거나 설계할 수 있게 될 것입니다.