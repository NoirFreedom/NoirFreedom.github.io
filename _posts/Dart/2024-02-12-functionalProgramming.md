---
layout: post
title: "[Dart] Functional Programming 개념정리"
date: 2024-02-12
categories: [Dart]
tags: [dart]
---


Dart에서 함수형 프로그래밍은 프로그램을 순수 함수의 집합으로 구성하여 문제를 해결하는 패러다임이다. 함수형 프로그래밍은 부작용을 피하고, 불변성을 지향하며, 고차 함수를 사용한다. Dart는 객체 지향 프로그래밍 언어이지만, 함수형 프로그래밍 개념을 지원하여 객체 지향 프로그래밍과 함수형 프로그래밍의 장점을 모두 활용할 수 있다.

### 순수 함수
순수 함수는 같은 입력에 대해 항상 같은 출력을 반환하며, 외부 상태를 변경하지 않고 외부 상태에 의존하지 않는 함수다. 이러한 특성 덕분에 테스트와 디버깅이 용이하다.

### 불변성
불변성은 데이터가 생성된 후 변경되지 않음을 의미한다. 함수형 프로그래밍에서는 데이터를 변경하는 대신, 변경된 새 데이터를 생성하여 사용한다. 이는 프로그램의 복잡성을 줄이고, 여러 스레드 환경에서 안전하게 데이터를 공유할 수 있는 방법을 제공한다.

### 고차 함수
고차 함수는 함수를 매개변수로 받거나 결과로 반환하는 함수다. Dart에서는 익명 함수, 람다 표현식, 및 클로저를 사용하여 고차 함수를 구현할 수 있다. 이를 통해 코드의 재사용성을 높이고, 추상화 수준을 향상시킬 수 있다.

### 예시코드

```dart
List<int> numbers = [1, 2, 3, 4, 5];

// 순수 함수 예시: 주어진 리스트의 모든 요소에 2를 곱한다.(맵을 사용하면 새로운 리스트가 만들어짐)
List<int> doubled = numbers.map((n) => n * 2).toList();
// (위 doubled와 값이 같지만 'doubled = doubled2' 는 false)
List<int> doubled2 = numbers.map((n) => n * 2).toList();

// 불변성 예시: 원본 리스트는 변경되지 않는다.
print(numbers); // [1, 2, 3, 4, 5]
print(doubled); // [2, 4, 6, 8, 10]

// 고차 함수 예시: 함수를 매개변수로 받는 함수.
void applyOperation(List<int> list, Function operation) {
  for (int i = 0; i < list.length; i++) {
    list[i] = operation(list[i]);
  }
}
applyOperation(numbers, (n) => n * 3); // 모든 요소에 3을 곱한다.
```



<br>

### Functional Programming의 기본 '형 변환'

### List 매핑
```dart
void main(){
    List<String> userNameList = [nameA, nameB, nameC]

    print(userNameList); // [nameA, nameB, nameC]
    print(userNameList.asMap()); // {0: nameA, 1: nameB, 2: nameC}
    print(userNameList.toSet()); // {nameA, nameB, nameC} -> Set에서 중복은 제거됨

    // List -> Map 형태 변환
    Map userNameMap = userNameMap.asMap();

    print(userNameMap.keys); // (0, 1, 2) -> 이터러블을 반환
    print(userNameMap.values); // (nameA, nameB, nameC) -> 이터러블을 반환

    print(userNameMap.keys.toList()); // [0, 1, 2[] -> List로 변환
    print(userNameMap.values.toList()); // [nameA, nameB, nameC] -> List로 변환

    // List -> Set
    Set userNameSet = Set.from(userNameList);
    // Set -> List
    print(Set userNameSet.toList());

}
```

### Map 매핑
```dart
Map<String, String> userNameMap = {
    'userA' = 'nameA',
    'userB' = 'nameB',
    'userC' = 'nameC',
};

final mapResult = userNameMap.map((key, value) => MapEntry('유저' $key,'이름' $value));

print(mapResult); // {유저 userA: 이름 nameA, 유저 userB: 이름 nameB, 유저 userC: 이름 nameC}
print(userNameMap); // {userA = nameA, userB = nameB, userC = nameC} => 기존 Map은 불변함
```

### Set 매핑
```dart
Set userNameSet = {
    nameA,
    nameB,
    nameC,
};

final setResult = userNameSet.map((x) => user$x).toSet(); // {usernameA, usernameB, usernameC} // List와 같은 형식
```