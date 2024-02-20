---
layout: post
title: "[Python] Class & Object"
date: 2023-07-09
categories: [Python]
tags: [python]
---







# 클래스와 객체

> 클래스는 객체(Object)를 정의하고 만들기 위한 변수와 메서드의 집합이기 때문에 무엇을 계속 만들어 내는 틀에 비유된다.

<img src="/assets/img/Python/Class-Object/python_class_object1.png" alt="" width="600" >



### 객체:
클래스에서 객체란 클래스에 선언한 모양 그대로 생성되는 대상이며, 모든 인스턴스를 포함하는 개념으로, 객체 지향 프로그램의 관점에서 클래스의 타입으로 선언되면 객체

### 인스턴스:
클래스로 구현된 구체적인 실체이고, 객체를 실체화하면 인스턴스, 객체 지향 프로그램의 관점에서는 객체가 메모리에 할당되어 실제로 사용되면 인스턴스

<br>

## 클래스 / 메서드 / 객체

### 클래스 만들기
```python
class 클래스이름:
    def 메서드이름(self): # 매개변수는 꼭 self를 넣어줘야 함
        명령블록
```

### 생성자, 속성
```python
class Aiffel:
    def __init__(self, name):  # 생성자 안에 속성 (__가 붙은 메서드는 파이썬이 자동으로 호출해주는 메서드로,
                               # 스페셜 메서드(special method) 또는 매직 메서드(magic method)라고 부름.
        self.name = name
    def cheer(self):
        print(f"{self.name}, 끝까지 화이팅!")
```


<img src="/assets/img/Python/Class-Object/python_class_object2.png" alt="" width="600" >


<img src="/assets/img/Python/Class-Object/python_class_object3.png" alt="" width="600" >



### self 동작순서

1. 클래스에 속성값을 넣은 객체를 만든다.

2. 자동으로 __init__ 생성자가 호출되면서 매개변수 self에 해당 객체가 입력되고, 다음 매개변수에는 속성값이 들어간다.

3. self.xxx이 해당객체.속성값' 으로 완성된 뒤 이 속성이 해당객체에 할당된다.

4. 해당 객체의 메서드가 호출된다.

5. 현재 객체가 매개변수 self에 들어간다.


## 상속

> 클래스에도 부모 - 자식 관계가 있어서 부모 클래스가 자식 클래스에게 속성(데이터)과 메서드(함수)를 상속한다

  - 부모 클래스: Parent class 또는 Super class
  - 자식 클래스: Child class 또는 Sub class

```python
class Parent: 
    # 속성
    def __init__(self):
         self.name = 'Dave'
         self.city = 'Seoul'

    # 메서드
    def identify(self):
         print('say:')

class Child(Parent):
    def __init__(self, name):
         self.name = name
    def identify_name(self):
        print('my name is', self.name)
    def identify_city(self):
         print("now I'm living in", self.city)
```


> 부모 클래스의 인스턴스 속성은 부모 클래스의 객체가 생성될 때 __init__()이 실행되면서 생성된다.
> 하지만 자식 클래스를 생성할 때에 부모 클래스의 __init__() 함수가 실행되지는 않기 때문에 인스턴스 속성은 자식 클래스에 상속되지 않는다.
> 만약 부모 클래스의 인스턴스 속성을 자식 클래스에서도 사용하기를 원한다면 super().__init__() 함수를 사용하여 부모 클래스의 __init__()를 자식 클래스의 __init__()에서 실행시켜야 한다.

