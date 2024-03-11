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

## Functional Programming의 기본 '형 변환'

### List 매핑
```dart
void main(){
    List<String> usersNameList = [nameA, nameB, nameC]

    print(usersNameList); // [nameA, nameB, nameC]
    print(usersNameList.asMap()); // {0: nameA, 1: nameB, 2: nameC}
    print(usersNameList.toSet()); // {nameA, nameB, nameC} -> Set에서 중복은 제거됨

    // List -> Map 형태 변환
    Map usersNameMap = usersNameMap.asMap();

    print(userNameMap.keys); // (0, 1, 2) -> 이터러블을 반환
    print(userNameMap.values); // (nameA, nameB, nameC) -> 이터러블을 반환

    print(userNameMap.keys.toList()); // [0, 1, 2[] -> List로 변환
    print(userNameMap.values.toList()); // [nameA, nameB, nameC] -> List로 변환

    // List -> Set
    Set usersNameSet = Set.from(usersNameList);
    // Set -> List
    print(Set usersNameSet.toList());

}
```

### Map 매핑
```dart
Map<String, String> usersNameMap = {
    'userA' = 'nameA',
    'userB' = 'nameB',
    'userC' = 'nameC',
};

final mapResult = usersNameMap.map((key, value) => MapEntry('유저' $key,'이름' $value));

print(mapResult); // {유저 userA: 이름 nameA, 유저 userB: 이름 nameB, 유저 userC: 이름 nameC}
print(usersNameMap); // {userA = nameA, userB = nameB, userC = nameC} => 기존 Map은 불변함
```

### Set 매핑
```dart
Set usersNameSet = {
    nameA,
    nameB,
    nameC,
};

final setResult = userNameSet.map((x) => user$x).toSet(); // {usernameA, usernameB, usernameC} // List와 같은 형식
```

### .where()
필터링 기능
```dart
List<Map<String, String>> users = [
    {
    'user1' = 'name1',
    'group' = 'A'
    },
    {
    'user2' = 'name2',
    'group' = 'A'
    },
    {
    'user3' = 'name3',
    'group' = 'B'
    },
    {
    'user4' = 'name4',
    'group' = 'B'
    },
];

// 원래의 리스트는 변경하지 않으며, 만들어진 새 리스트만 리턴
//.toList()등을 붙혀 원하는 이터러블로 반환가능
final groupA = users.where((x) => x['group'] == 'A').toList(); // [{'user1' = 'name1', 'group' = 'A'}, {'user2' = 'name2', 'group' = 'A'}]
```

### .reduce()
```dart
// 숫자
List<int> numbers = [1, 2, 3, 4];

final result = number.reduce((prev, next){
    print('---------------')
    print('previous: $prev');
    print('next: $next');
    print('total: ${prev + next}');

    return prev + next
});

print(result); 
// ---------------
// previous: 1
// next: 2
// total: 3
// ---------------
// previous: 3
// next: 3
// total: 6
// ---------------
// previous: 6
// next: 4
// total: 10


// 글자
List<String> words = [
    'Hello ',
    'World ',
    'with dart'
];

final sentence = words.reduce((prev, next) => prev + next);

print(sentence); // Hello World with dart

// 유의사항 : 리턴 타입은 반드시 최초 선언한 타입과 일치해야한다.
```
<br>

### .fold()
최초타입과 관계없이 아무 형태로 리턴이 가능하는 점에서 .reduce()와 차이가 있다.
```dart
List<int> numbers = [1, 2, 3, 4];

// .reduce()는 리턴값이 최초 타입과 일치해야 하는 반면, .fold()는 그렇지 않아도 되기 때문에 어떤 값이 리턴이 될 것인지 제네릭으로 제공해야한다. 
final result = number.fold<int>(0, (prev, next) {
    print('---------------');
    print('previous: $prev');
    print('next: $next');
    print('total: ${prev + next}');
    return prev + next;
});
// ---------------
// previous: 0
// next: 1
// total: 1
// ---------------
// previous: 1
// next: 2
// total: 3
// ---------------
// previous: 3
// next: 3
// total: 6
// ---------------
// previous: 6
// next: 4
// total: 10

List<String> words = [
    'Hello ',
    'World ',
    'with dart'
];

// .reduce()에선 불가함
final count = words.fold<int>(0, (prev, next) => prev.length + next.length); 

print(count); // 21
```

### cascading operator
```dart
List<int> even = [2, 4, 6, 8];
List<int> odd = [1, 3, 5, 7];

final result = [...even, ...odd]; // [2, 4, 6, 8, 1, 3, 5, 7]
```

### Functional Programming 실사용 예시
```dart
List<Map<String, String>> users = [
    {
    'user1' = 'name1',
    'group' = 'A'
    },
    {
    'user2' = 'name2',
    'group' = 'A'
    },
    {
    'user3' = 'name3',
    'group' = 'B'
    },
    {
    'user4' = 'name4',
    'group' = 'B'
    },
];

class User {
    final String name;
    final String group;

    User ({
        required this.name, 
        required this.group
        });
}
```

### 데이터의 형태를 왜 class로 바꾸는가?
Map과 같은 경우 자유도가 높다. 예를들어 위와 같이 Json형식으로 데이터를 받을 경우, 예외적인 Key와, 오타 같은 것들이 포함되어 있어도 프로그램적으로 알 수 있는 방법이 없기 때문에, 클래스로 변경하여 대응되는 기능들을 수행할 수 있도록 한다. 즉, **데이터의 구조를 신뢰할 수 있는 상태**로 만든다.

```dart
// 앞서 말했듯, Map은 자유도가 높기 때문에, null check operator를 사용하지 않으면 에러가 발생한다.
final parsedUsers = users.map(
    (x) => User(
        name:x['name']!, // null check operator
        group:x['group']! // null check operator
        ),
    ).toList;

```

### 여기서 잠깐!
위 코드를 프린트 해보면, 다음과 같이 'Instance of User'이 4개가 출력되는데, 왜 일까?
```dart
print(parsedUsers); // [Instance of User, Instance of User, Instance of User, Instance of User]
```
**이유**: 원래 클래스의 기본값은 'Instance of ..'로 나오는게 default이다. 이것을 변경하려면 `@override`를 해야한다.
```dart
@override
String toString(){ // .toString() -> 모든 클래스의 최상위 부모 클래스인 'Object' 클래스에서 제공받는 toString을 클래스의 목적에 맞게 override하여 정의하고 사용한다.
    return User 'User(name: $name, group: $group)';
}

print(parsedUsers); // [User(name: name1, group: A, ...)]
```

### 그럼 이것으로 뭘 할수 있을까?
Key와 오타를 걱정하지 않고, 신뢰성 있게 사용할 수 있다.
```dart
// (x)로 User의 키워드(name, group)에 접근하여 데이터 선에서 오류를 줄일 수 있다.
final userGroupB = parsedUsers.where((x) => x.group == 'B'); // 필터링
```


### Functional Programming의 주의사항!
함수를 계속 붙혀가며 사용할 수 있어, 코드가 간결해진다는 장점이 있지만, 이로인해 다른 사람들과 협업 과정에서 문제가 생기거나, 시간이 지나 본인도 코드의 의미을 이해하기 어려울 수 있다.