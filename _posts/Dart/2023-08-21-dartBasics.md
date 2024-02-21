---
layout: post
title: "[Dart] Basics 개념정리"
date: 2023-08-21
categories: [Dart]
tags: [dart]
---


## **Dart란?**

- *구글에서 개발한 현대적인 객체지향 프로그래밍 언어*  

- *타입 추론 기능, 비동기 프로그래밍 등 강력한 기능을 제공*  

- *AOT, JIT컴파일을 통해 네이티브 코르스 플랫폼 개발에 적합*

<br>

### **Dart가 다른언어와 구분되는 점**

- **Null Safety:** Dart의 Null Safety는 코드에서 발생할 수 있는 null 값 관련 오류를 방지하고 안정성을 높이는 기능

- **Hybrid Compile:** 개발에서 JIT과 다트 가상머신위에서 프로그래밍이 동작하고, 배포에선 네이티브 환경에서 AOT컴파일로 실행파일이 작동

<br>


### **Variables**

Dart에서의 "Variable(변수)"은 값이나 데이터를 저장하고 참조하기 위한 이름을 가진 메모리 위치를 나타내는 개념. Dart는 정적 타입을 가지는 언어로, 변수의 타입을 선언하고 해당 타입의 값을 저장하는 방식을 사용하는데, 이때 변수는 데이터를 보관하거나 조작하기 위해 사용되며, 프로그램의 상태를 추적하고 관리하는 데 중요한 역할을 함


### **late**
변수가 초기화는 안됐지만, 언젠가 값을 할당해 줄 때 사용


### **final & const**
한 번 값을 할당하면 변경이 불가능한 키워드

<br>

### 값의 변경 가능성:

**final**: 변수의 값을 한 번 설정하면 이후에 변경할 수 없지만 객체의 내부 상태는 변경될 수 있음

**const**: const로 선언된 변수는 컴파일 시간에 값을 평가하고 할당되며 반드시 상수 표현식으로 초기화되어야 하고, 객체의 내부 상태도 변경할 수 없음

### 할당 가능한 시기:

**final:** final 변수는 런타임 이전에 초기화되어야 하지만, 런타임 중에 값이 결정될 수 있다. 즉, 런타임 중에 계산된 값을 할당할 수 있다.

**const**: const 변수는 컴파일 시간에 값이 결정되어야 하며, 반드시 컴파일 시에 결정 가능한 상수 표현식으로 초기화되어야 한다.


<br>

### **Control flow**
- 조건문 (Conditional Statements)

- 반복문 (Loops)

- 분기문 (Branching Statements)

<br>



#### **사칙연산 계산기 만들어보기**

```dart
class Calculator {
  int add(int a, int b){
    return a + b;
    }
  int minus(int a, int b){
    return a - b;
    }
  int multiply(int a, int b){
    return a * b;
    }
  double divide(int a, int b){
    return a / b;   
    }
}


void main() {
  Calculator calculator = Calculator();
  
  int sum = calculator.add(5,3);
  int difference = calculator.minus(10, 4);
  int product = calculator.multiply(6, 7);
  double quotient = calculator.divide(20, 5);

  print("Sum: $sum");
  print("Difference: $difference");
  print("Product: $product");
  print("Quotient: $quotient");
}
```

<br>

### **Class의 구성요소**

- **멤버 변수 (Member Variables) or 필드 (Fields):**
클래스 내부에 정의된 변수로, 클래스의 상태를 나타냄. 멤버 변수는 클래스 내부의 어디에서든지 사용할 수 있으며, 객체의 속성이나 데이터를 저장하는 역할을 함

- **생성자 (Constructor):**
객체가 생성될 때 호출되는 특별한 함수로, 클래스의 인스턴스를 초기화하고 필요한 작업을 수행하는 역할을 함

- **메서드 (Methods):**
클래스 내부에 정의된 함수로, 클래스의 동작을 정의

- **게터 (Getters)와 세터 (Setters):**
Getters 멤버 변수의 값을 읽는 메서드로, 클래스 외부에서 멤버 변수 값을 얻을 수 있게 해주고, Setters는 멤버 변수의 값을 변경하는 메서드로, 클래스 외부에서 멤버 변수 값을 설정할 수 있게 해줌

- **정적 멤버 (Static Members):**
클래스에 속하지만 특정 인스턴스와는 관련되지 않는 멤버 변수나 메서드를 의미

- **상속 (Inheritance):**
클래스 간에 부모-자식 관계를 정의하여, 하위 클래스가 상위 클래스의 특성과 동작을 상속받을 수 있도록 해줌

- **추상 클래스 (Abstract Classes):**
구체적인 구현을 가지지 않는 클래스로, 하위 클래스에서 구체적인 메서드 구현을 강제할 수 있음

- **인터페이스 (Interfaces):**
클래스가 어떤 메서드를 구현해야 할지 정의하는 명세서로, 클래스가 특정 동작을 보장하기 위해 사용

<br>

### **Class 용례**
```dart
class Person {
  var name;
  var age;
  
  Person(this.name, this.age);
  
  void printInfo() {
    print ("Name: $name, Age: $age ");
  }
}


void main() {
  var person = Person('Dave', 30);
  person.printInfo();
  
  var person2 = Person("Claire", 30);
  person2.printInfo();
}
```

### **Polymorphism이란?**

**폴리모피즘(Polymorphism)** 은 객체 지향 프로그래밍에서 중요한 개념 중 하나로, 서로 다른 클래스가 동일한 인터페이스나 기반 클래스를 공유하며 사용될 수 있는 능력을 나타냄

### **Polymorphism 주요 형태**

- **Compile-Time Polymorphism - Method Overloading:**
메서드 오버로딩은 같은 이름의 메서드를 다양한 매개변수 집합과 함께 정의하는 것을 의미하고, 컴파일 시간에 어떤 메서드가 호출될지는 전달되는 매개변수의 개수나 타입에 의해 결정

- **Run-Time Polymorphism - Method Overriding:**
메서드 오버라이딩은 서브클래스에서 슈퍼클래스의 메서드를 재정의하는 것을 의미하고, 같은 이름의 메서드가 서브클래스와 슈퍼클래스에서 각각 정의되어 있을 때, 객체의 타입에 따라 어떤 메서드가 호출될지는 실행 시간에 결정

### **Polymorphism 용례**

```dart
class IronMan {
  String name;
  int powerLevel;
  
  IronMan(this.name, this.powerLevel);
  
  void shoot() {
    print("$name is shooting guns!");
  }
}


class IronManMK3 extends IronMan{
  var flyheight;
  
  
  IronManMK3(String name, int powerlevel, this.flyheight):super(name,powerlevel);
  
  @override
  void shoot(){
    print("$name is shooting Laser!");
  }
  
  void fly(){
    print("$name is flying in $flyheight");
  }
}

void main(){
  IronMan mk1 = IronMan("mk1", 10);
  
  IronManMK3 mk3 = IronManMK3("mk3", 20, 1000);
  
  mk1.shoot();
  mk3.shoot();
  mk3.fly();
}
```

### **Abstract Class 용례**

```dart
abstract class IronMan {
  var name;
  var suitColor;
  
  
  IronMan(this.name, this.suitColor);

  void fly();
  void shootLasers();
  void withstandDamage();
}

class Mark50 extends IronMan {

    Mark50(String name, String suitColor):super(name, suitColor);

    @override
    void fly(){
        print("$name is flying");
    }

    @override
    void shootLasers(){
        print("$name is shoot Laser!");
    }

    @override
    void withstandDamage(){
        print("$name is make huge damage");
    }
}

void main(){
    var mark50 = Mark50("Mark50", "red");

    mark50.fly();
    mark50.shootLasers();
    mark50.withstandDamage();
}
```

### **Mixin 용례(다중상속과 유사한 기능)**
```dart
mixin Flyable on Animal {
  void fly() {
    print("I am flying!");
  }
}

class Animal {
  String name;
  
  Animal(this.name);
}

class Bird extends Animal with Flyable {
  
  
  Bird(String name) : super(name);
}

void main() {
  var bird = Bird("Bird");
  bird.fly();
}
```


### **Async와 Await:**

**async:** 함수가 비동기적으로 실행될 것임을 나타내는 키워드. async 함수 내에서는 await 키워드를 사용하여 비동기 작업을 기다리고 결과를 반환.

**await:** 비동기 작업의 완료를 기다리고 그 결과를 반환하는 키워드. await 키워드를 사용하면 해당 비동기 작업이 완료될 때까지 함수의 실행이 일시 중단됨.

### **Future:**

Future는 비동기 작업의 결과를 나타내는 클래스. 비동기 작업을 시작하면 Future 객체가 생성되고, 작업이 완료되면 해당 결과를 포함. 비동기 작업이 완료되기를 기다리는데 사용되며, async 함수 내에서 await와 함께 사용

### **Stream:**

Stream은 비동기적으로 흐르는 데이터의 연속을 나타내는 개념. 이벤트의 시퀀스를 제공하며, 데이터를 일련의 스냅샷 대신 실시간으로 처리하는 데 사용. Stream은 데이터가 연속적으로 흘러들어오는 상황에서 유용하며, 비동기적으로 데이터를 받아오고 처리하는 데 활용됨

### **Async, Await, future, Stream 용례**
```dart
import 'dart:async';

Future<int> fetchData() async {
  await Future.delayed(Duration(seconds: 2));
  return 42;
}

Stream<int> streamData() async* {
  for (int i = 0; i < 5; i++) {
    await Future.delayed(Duration(seconds: 1));
    yield i;
  }
}

void main() async {
  print("Start");

  // Future와 await
  int result = await fetchData();
  print("Fetched data: $result");

  // Stream
  await for (int value in streamData()) {
    print("Stream value: $value");
  }

  print("End");
}

```