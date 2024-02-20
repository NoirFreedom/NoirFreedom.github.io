---
layout: post
title: "[Python] 리스트 & 딕셔너리"
date: 2023-07-09
categories: [Python]
tags: [python]     # TAG names should always be lowercase
---

# List & Dictionary


## List

###  1차원 리스트
 
 ```python
list_o = ['a','b','c','d','e','f'] # 한 줄로 늘어선 리스트
 ```



### 2차원 리스트

```python
list_tw = [[1, 2], ['ㄱ', 'ㄴ'], ['a', 'b']] # 아래 이미지와 같이 상상해 볼 수 있다
```

<img src="/assets/img/Python/List-Dictionary/python_list_dictionary1.png" alt="" width="600" >

#### 3차원 리스트
```python
# 4행, 3열, depth 2인 3차원 리스트
list_th = [[[1, 2, 3], [4, 5, 6], [7, 8, 9], [10, 11, 12]],\
           [['a', 'b', 'c'], ['d', 'e', 'f'], ['g', 'h', 'i'], ['j', 'k', 'l']]]
```
##### 📍 depth의 길이는 len()으로 확인가능

<img src="/assets/img/Python/List-Dictionary/python_list_dictionary2.png" alt="" width="600" >

## Dictionary

##### key와 value를 추가하는 함수 : setdfault(), update()

```python
setdfault() # 이미 해당 key가 있을 때: 그 key의 value를 반환
            # key가 없을 때: 딕셔너리에 key:value를 저장하고 value를 반환

update() # 특정 key-value 조합을 삽입

```
> #### Q. setdefault() 함수와 update() 함수의 차이는?
> 
> setdefault() 함수에 key와 value를 모두 넣을 경우, key가 딕셔너리에 이미 존재한다면 setdefault() 함수는 데이터에 영향을 주지 못한다. (setdefault() 함수에 입력된 > value 값 무시)
> 반면 update() 함수는 기존의 value 값과 상관없이 딕셔너리의 데이터를 업데이트할 수 있다.


##### key-value 삭제하는 함수 : pop(), popitem()
##### 딕셔너리 생성하는 함수 :  fromkeys()  

<br>

### [예약어] in / not in
- ##### 리스트와 딕셔너리에 공통으로 사용가능
- ##### 요소가 들어있는지 확인하는 예약어
- ##### 조건문과 자주 쓰임
- ##### in / not in 의 리턴값은 bool. 즉 True 또는 False로 리턴

<br>
<br>

## List Comprehension

### 반복문과 컴프리헨션

<img src="/assets/img/Python/List-Dictionary/python_list_dictionary3.png" alt="" width="600" >

반복문으로 리스트를 만들 때는 리스트명에 비어있는 리스트를 먼저 할당해야 한다. 반면 컴프리헨션으로 리스트를 만들 때는 가장 바깥에 대괄호 [ ]만 써주면 된다.

<img src="/assets/img/Python/List-Dictionary/python_list_dictionary4.png" alt="" width="600" >

반복문은 평소대로 쓰면 되지만 리스트 컴프리헨션은 대괄호 안에 쓴다.

<img src="/assets/img/Python/List-Dictionary/python_list_dictionary5.png" alt="" width="600" >

반복문에서는 콜론(:) 다음 들여쓰기 후에 append() 함수와 그 안에 변수를 썼지만, 컴프리헨션에서는 반복문 앞에 변수만 쓰면된다.



### 반복문 + 조건문 컴프리헨션

<img src="/assets/img/Python/List-Dictionary/python_list_dictionary6.png" alt="" width="600" >

반복문에서는 반복문 콜론(:) 뒤에 조건문을 쓰고, 컴프리헨션에서는 for문 뒤에 조건문이 온다.

<img src="/assets/img/Python/List-Dictionary/python_list_dictionary7.png" alt="" width="600" >

변수의 위치도 반복문에서와 동일하다




### 중복 표현과 컴프리헨션
- 컴프리헨션에 for문과 if문을 중복해서 쓸 수 있음

```python
items = ['a', 'b', 'c']
words = ['hi', 'halo', 'hello'] 

[[x, y] for x in items for y in words]
```
### 셋 컴프리헨션 & 딕셔너리 컴프리헨션

### 셋 컴프리헨션
```
셋(set) 컴프리헨션을 만드는 방법은 간단합니다. 리스트를 만들 때 쓰는 대괄호 [ ] 대신에 중괄호 { }를 써주면 된다.
```



### 딕셔너리 컴프리헨션

중괄호를 이용하면서 동시에 key:value의 형태로 내용을 채운다면 딕셔너리 컴프리헨션이 됨. 
리스트 함수에서 배웠던 zip() 함수를 활용해서 키와 밸류를 지정하면 된다.

```python
# {이름:나이} 딕셔너리
user_name = ['a', 'b', 'c', d'']
user_age = [10, 20, 30, 40] 
{k:v for k, v in zip(user_name, user_age)}
```
