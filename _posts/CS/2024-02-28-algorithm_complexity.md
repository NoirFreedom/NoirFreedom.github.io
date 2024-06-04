---
layout: post
title: "[CS] 알고리즘 복잡도(Algorithm Complexcity)"
date: 2024-02-28
categories: [CS]
tags: [cs]
---


알고리즘 복잡도는 알고리즘이 문제를 해결하기 위해 필요한 자원(시간 또는 공간)의 양을 측정하는 방법이고, 이를 이해하는 것은 그 알고리즘이 특정 문제를 효율적으로 해결할 수 있는지를 판단하는 데 중요하다. 복잡도는 크게 시간 복잡도와 공간 복잡도로 나눌 수 있다. 

이 글에서는 각 복잡도의 개념을 단계별로 설명하고, 자바스크립트로 예시 코드를 작성하여 복잡도의 개념을 명확히 이해하려한다.

### 1. 시간 복잡도(Time Complexity)

시간 복잡도는 알고리즘이 문제를 해결하는 데 필요한 시간의 양을 측정한다. 이는 일반적으로 알고리즘 내부의 기본 연산이 실행되는 횟수로 표현된다. 시간 복잡도는 대표적으로 빅 오 표기법(O-notation)을 사용하여 표현된다.

- **O(1) - 상수 시간(Constant Time):** 입력 크기와 관계없이 알고리즘의 실행 시간이 일정하다.
  
```javascript
function findFirstElement(array) {
  return array[0];
}
```

- **O(n) - 선형 시간(Linear Time):** 알고리즘의 실행 시간이 입력 크기에 비례한다.
  
```javascript
function findMax(array) {
  let max = array[0];
  for(let i = 1; i < array.length; i++) {
    if(array[i] > max) {
      max = array[i];
    }
  }
  return max;
}
```

- **O(n^2) - 이차 시간(Quadratic Time):** 알고리즘의 실행 시간이 입력 크기의 제곱에 비례한다. 주로 이중 반복문을 사용할 때 발생한다.
  
```javascript
function bubbleSort(array) {
  for(let i = 0; i < array.length; i++) {
    for(let j = 0; j < array.length - i - 1; j++) {
      if(array[j] > array[j + 1]) {
        let temp = array[j];
        array[j] = array[j + 1];
        array[j + 1] = temp;
      }
    }
  }
  return array;
}
```

### 2. 공간 복잡도(Space Complexity)

공간 복잡도는 알고리즘이 문제를 해결하는 데 필요한 총 저장 공간의 양을 측정한다. 이는 알고리즘이 사용하는 변수, 자료 구조, 스택 공간 등을 포함한다.

- **O(1) - 상수 공간(Constant Space):** 알고리즘의 필요한 메모리 공간이 입력 크기와 무관하게 고정되어 있다.
  
```javascript
function increment(x) {
  return x + 1;
}
```

- **O(n) - 선형 공간(Linear Space):** 알고리즘의 필요한 메모리 공간이 입력 크기에 비례하여 증가한다.
  
```javascript
function duplicateArray(array) {
  let newArray = [];
  for(let i = 0; i < array.length; i++) {
    newArray.push(array[i]);
  }
  return newArray;
}
```

결론적으로, 알고리즘 복잡도는 시간 복잡도와 공간 복잡도로 나뉘며, 알고리즘의 효율성을 평가하는 핵심 지표이다. 시간 복잡도는 알고리즘 실행 시간에, 공간 복잡도는 알고리즘이 사용하는 메모리 공간에 초점을 맞춘다. 빅 오 표기법을 사용하여 복잡도를 표현하고, 이를 통해 알고리즘 성능을 예측하고 비교할 수 있다.

자바스크립트 예시 코드를 통해 각 복잡도의 개념을 이해함으로써, 개발자는 주어진 문제에 맞는 효율적인 알고리즘을 선택하고, 성능 향상을 위한 최적화를 수행할 수 있다. 특히 대규모 데이터 처리 시 복잡도 분석은 필수적이며, 이를 통해 시스템 자원 낭비를 방지하고 사용자에게 더 나은 경험을 제공할 수 있다.