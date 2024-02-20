---
layout: post
title: "[Python] 예외처리(Exception Handling)"
date: 2023-07-09
categories: [Python]
tags: [python]
---








# 예외처리

> ## 예외 처리를 하는 이유
  > 문법적으로는 오류가 없더라도 코드를 실행할 때(런타임) 프로세스에서 발생할 수 있는 예외


## 예외 처리를 하는 방법
### 1. 조건문
### 2. try - except 구문

## 조건문

  > 입력이 다양한 자료형을 염두해두고 코드를 짜야하므로 int형이 아닌 자료형들을 조건문으로 걸러내야 한다.
  > 그런데 input() 함수는 들어온 값을 모두 문자열로 인식하기 때문에 문자열이 숫자인지를 확인하는 함수인 isdigit() 함수를 사용해야한다.

  - isdigit(): 숫자(digit)만 판별하는 함수이므로 음수를 표시하는 -는 걸러내지 못하기때문에 lstrip() 함수를 사용하여 거른다.

```python
# lstrip 함수의 예시
# 정수가 아닌 값을 입력해서 예외 처리를 잘 하는지 확인해 보세요.  
num = input('정수를 입력하세요 : ')  # int() 함수 제거

# 입력값이 0과 양의 정수일 때
# Q. 아래의 빈 코드를 채워보세요. 
if num.isdigit():
    print(f'입력한 정수는 {num}입니다.')
    
# 입력값이 음의 정수일 때
# Q. 아래의 빈 코드를 채워보세요. 
elif num.lstrip('-').isdigit():
    print(f'입력한 정수는 {num}입니다.')

# 입력값이 정수가 아닐 때
else:
    print('정수가 아닙니다.')
```


## try - except 구문
  > 어떤 상황에서 예외가 발생하는지 완벽하게 이해하지 않아도 프로그램이 강제로 죽어 버리는 상황을 막을 수 있다.
```python
    try:
      예외가 발생할 수 있는 코드
    except (발생 오류 (as 발생 오류 변수)):
      에외가 발생했을 때 실행할 코드
```

## try - except - pass 구문
> 프로그래밍을 하다가 예외가 발생하면 일단 코드는 돌아가게 처리해야 하지만, 해당 코드가 딱히 중요하지 않아 '일단 프로그램이 강제 종료되는 것부터 막자'라는 목적으로 except 구문에 아무 것도 넣지 않고 try 구문을 사용하게 된다. 하지만 아무 것도 넣지 않으면 구문 오류가 발생하므로 pass 키워드를 넣어 준다.


## try - except - else 구문
```python
try:
    예외가 발생할 수 있는 코드
except:
    에외가 발생했을 때 실행할 코드
else:
    예외가 발생하지 않았을 때 실행할 코드
```

## try - except - else - finally 구문
  - 예외가 발생하든 발생하지 않든 무조건 실행하고 싶은 코드는 finally 구문을 사용하면 된다.

```python
# try - except - else - finally 구문 

# Q. 위에서 배운 코드를 바탕으로 아래의 빈 코드를 채워보세요. 
# 시도할 코드 
try:
    # 예외 걱정 없이 입력값에 float 함수를 씌움
    num = float(input('수를 입력하세요 : '))

# try에서 예외가 발생했을 때 실행할 코드
except:
    print('수가 아닙니다.')

# try에서 예외가 발생하지 않았다면 실행할 코드
else:
    print(f'입력한 수는 {num}입니다.')

# finally 구문 추가
finally:
    print('*' * 25)
    print('프로그램이 끝났습니다.')
```

## 예외 처리 구문의 규칙

```python
try 구문은 단독으로 쓰일 수 없고 except 또는 finally 구문과 함께 쓰여야 하고,
else 구문은 except와 함께 쓰이고, except 구문 뒤에 사용해야 한다.

# 구문 조합
try + except
try + except + else
try + except + finally
try + except + else + finally
try + finally
```

## Error
  ### raise
```python
raise 에러('에러 메시지')
```

## 예외 처리 전략
### EAFP
> “It’s Easier to Ask Forgiveness than Permission”, 미리 허락을 구하는 것보다는 나중에 용서를 구하는 게 쉽다는 뜻. 일단 항상 예외가 발생하지 않을 것으로 가정하고 코딩을 한 후, 코드 실행 중 에러가 발생 시, 그때 예외처리 코딩을 하는 게 좋다는 전략. (일단 수행(try)시키고, 만약 에러가 발생하면 그때 처리(except)한다는 스타일로 코딩)

### LBYL
> “Look Before You Leap”, 누울 자리를 보고 다리를 뻗으라는 뜻. 어떤 코드를 실행하기 전에 에러가 발생할 수 있는 조건을 미리 다 따져보고 그걸 어떻게 처리할지 조건문(if문) 스타일로 코딩하는 전략.

