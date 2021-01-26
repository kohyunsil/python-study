# 01/26 python grammar (review)+new quiz

- 모듈

```python
import theater                                    # 모듈 부를때
theater.price(3)
theater.price_morning(4)
theater.price_soldier(5)

import theater as mv                              # 모듈 별명 mv 지정
mv.price(3)
mv.price_morning(4)
mv.price_soldier(5)

from theater import *                             # 모듈에 있는 모든 것 부르기 가능
price(3)
price_morning(4)
price_soldier(5)

from theater import price, price_morning          # 모듈에 있는 몇개만 부르기 가능
price(3)
price_morning(4)

from theater import price_soldier as price        # 모듈에 있는 어떤 함수에 별명 지정가능
price(5)
```

- if **name** == "**main**": 의 의미

    **`__name__` 변수란?**

    파이썬의 `__name__` 변수는 파이썬이 내부적으로 사용하는 특별한 변수 이름이다. 만약 `C:\doit>python mod1.py`처럼 직접 mod1.py 파일을 실행할 경우 mod1.py의 `__name__` 변수에는 `__main__` 값이 저장된다. 하지만 파이썬 셸이나 다른 파이썬 모듈에서 mod1을 import 할 경우에는 mod1.py의 `__name__` 변수에는 mod1.py의 모듈 이름 값 mod1이 저장된다.

- 내장함수

```python
language=input("무슨 언어를 좋아하세요?")
print("{0}은 아주 좋은 언어입니다!".format(language))

dir : 어떤 객체를 넘겨줬을 때 그 객체가 어떤 변수와 함수를 가지고 있는지 표시
print(dir())
import random #외장함수
print(dir())
import pickle
print(dir())
print(dir(random))

lst=[1,2,3]
print(dir(lst))

name="Jim"
print(dir(name))
```

- 외장함수

```python
# 외장함수

# glob : 경로 내의 폴더 / 파일 목록 조회 (윈도우에서의 dir)

import glob
print(glob.glob("*.py")) #확장자가 py인 모든 파일

--------------------------------------------------------

import os
print(os.getcwd()) #현재 디레토리

folder = "sample_dir"

if os.path.exists(folder):
    print("이미 존재하는 폴더입니다.")
    os.rmdir(folder)
    print(folder,"폴더를 삭제하였습니다.")
else:
    os.makedirs(folder) #폴더 생성
    print(folder, "폴더를 생성하였습니다.")
```

- time 외장함수

```python
import time
print(time.localtime())
print(time.strftime("%Y-%m-%d %H:%M:%S"))

import datetime
print("오늘 날짜는 ",datetime.date.today())

#timedelta : 두 날짜 사이의 간격
today=datetime.date.today() # 오늘 날짜 저장
td = datetime.timedelta(days=100)
print("우리가 만난지 100일은", today+td) # 오늘부터 100일 후
```

- quiz 10

```python
import byme
byme.sign()
```

![01%2026%20python%20grammar%20(review)+new%20quiz%206ff84954384a454cb2400ff8d08b4e81/Untitled.png](01%2026%20python%20grammar%20(review)+new%20quiz%206ff84954384a454cb2400ff8d08b4e81/Untitled.png)

- 핵맨게임

```python
from random import *
words = ["apple","banana","orange"]
word = choice(words)
print("answer : "+word)
letters="" #사용자로부터 지금까지 입력받은 모든 알파벳

while True:
    succeed = True
    print()
    for w in word:
        if w in letters:
            print(w, end=" ")
        else:
            print("_", end=" ")
            succeed = False
    print()
    
    if succeed:
        print("Success")
        break

    letter=input("Input letter > ") #사용자 입력 받기
    if letter not in letters:
        letters += letter
    
    if letter in word:
        print("correct")
    else:
        print("wrong")
```

- 함수 연습 (예제 1)

```python
def s_replace(str_test):
    return str_test.replace("ㅋ","")

s = input("문자열 입력>>")
result = s_replace(s)
print(result)
```