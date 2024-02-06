---
layout: post
title: "[Python] 모듈 & 패키지 & 라이브러리"
date: 2023-07-09
categories: [Python]
tags: [python]
---







# 모듈, 패키지, 라이브러리의 관계

- #### 모듈(module): .py 확장자의 파일. 파일 안에 변수나 함수, 클래스 등을 저장할 수도 있고, 모듈 자체가 실행할 수 있는 코드일 수도 있습니다.
- #### 패키지(package): 모듈 여러 개를 묶은 것
- #### 라이브러리(library): 여러 패키지와 모듈을 모아둔 것

      라이브러리 ≥ 패키지 ≥ 모듈

  ## 표준 모듈과 외부 모듈

   > 파이썬을 설치할 때 기본적으로 깔리는 모듈을 표준 모듈 또는 내장 모듈이라고 하고 다른 개발자들이 특수한 목적으로 작성한 모듈은 외부 모듈이라고 합니다.
  
#### 표준모듈의 예

  ```python
  import [모듈] (as [변경할 변수])
  ```

## 모듈의 종류
- ### random
  - random()
  - randint()
  - seed()
- ### math
  - math.pi
  - math.e
  - math.pow(n, x)
  - math.ceil()
  - math.floor()
 
- time
  - time.sleep()
  - time.localtime()
- keyword
  - keyword.kwlist
  - keyword.iskeyword()
> 참고 | round() - 파이썬 기본 내장 함수, 반올림하는 함수


### from import: 모듈 일부분만 가져오기
```python
from 모듈 import 일부분 

from 모듈 import * # 다음 코드를 사용하면 math 모듈의 모든 변수, 함수, 클래스를 가져옵니다.
```

### from import as: 모듈 일부분만 가져오기 + 키워드

```python
from 모듈 import 변수 as 이름
from 모듈 import 함수 as 이름
from 모듈 import 클래스 as 이름
```


## 패키지(package)

```python
import package
import package.모듈
import package.모듈1, 패키지.module2
package.module.변수
package.module.함수()
package.module.클래스()
```

## 라이브러리와 프레임워크(Library and Framework)
> **라이브러리**는 여러 패키지와 모듈을 모아둔 것, 즉 특정 기능에 대한 도구를 모아둔 집합이다.
> 프로그래머가 어떠한 기능을 수행할 때, 도움을 주고 필요한 것을 제공한다.
> **프레임워크**는 뼈대나 기반구조를 뜻하고, 프로그래밍을 진행할 때 필수적인 코드, 알고리즘 등과 같이 어느 정도의 구조를 제공해주기 때문에 프레임워크를 사용하는 프로그래머는 이 프레임워크의 뼈대 위에서 코드를 작성하여 프로그램을 개발하면 된다.
> 완성된 제품이 아닌 완성된 제품을 만들기 위해서 개발자를 도와주는 또는 기반을 만들어 준다.
