# 02/24 fastcampus(python)

dictionary

- 데이터의 순서가 없고 {key:value} 로 이루어진 데이터 타입
- 리스트나 튜플의 index 값 대신 내가 key값으로 직접 지정해서 사용하는 방법
- key값으로 사용할 수 있는  데이터타입은 int,str 만 가능

```python
#create
dic = {1:"one", "two":2, -1:[1,2,3]}
type(dic), dic

#read
dic["two"]

# update(수정)
dic["two"]=22
dic

#dict method
dic.keys()

dic.items() # for 문에서 데이터를 하나씩 가져와서 사용할때 사용

# key값이 딕셔너리에 없을때 추가 가능
dic.get(2,"no data")
```

- 교집합, 합집합, 차집합 (set)

```python
# set : 집합 : 중복데이터가 없는 데이터 집합 형태 
# 교집합, 합집합, 차집합 연산수행

s = set([1,2,3,2,3])
type(s),s

s1=set([1, 2, 3])
s2=set([2, 3, 4])

#교집합 : &
s1 & s2
# 합집합 : |
s1 | s2
# 차집합 : -
s1 - s2
```

### 데이터 형변환

- 이미 선언된 데이터 타입은 형변환 함수를 통해서 데이터 타입을 변경할 수 있습니다.
- 데이터 타입과 가지고 있는 데이터에 따라서 변환할수 있는경우도 있고 없는 경우도 있습니다.=

```python
# quiz : 중복 제거, 내림차순 정렬 출력
ls = [1, 4, 2, 4, 3, 5, 1]
# 중복 데이터 제거 : set()
result = list(set(ls))

# 오름차순 정렬
result.sort()

# 내림차순 정렬
result = result[::-1]

result
```

연산자

- 산술 : +,-,*,/,//,%,** (부동소수점문제 : 반올림round,decimal,...)
- 할당 : 변수에 산술연산을 한 결과를 대입
- 비교 : ==, !=, >, <, >=, <= : 결과가 True,False
- 논리 : and, or, not     결과가 True,False
- 멤버 : A in B, not in : collection 데이터에서 특정 값이 있는지 판단 : 결과가 True, False
- 식별 : is : 주소값을 비교할때 사용 : 결과가 True, False

```python
# quiz : 윤년 판별하기!
# 4년에 한번씩 윤년입니다. 2021 % 4 == 0
# 100년에 한번씩 윤년이 아닙니다.
# 400년에 한번씩은 윤년입니다.

year=int(input("insert number : "))

(year % 4 == 0) and (year % 100 != 0) or (year % 400 == 0)

# 논리연산자는 and가 or보다 우선순위가 높습니다.
```

조건문

- 특정 조건에 따라서 코드를 실행하는 문법
- if,elif,else

```python
# quiz : 숫자를 입력바다 3의 배수이면 fizz, 5의 배수이면 buzz, 15의 배수이면 fizzbuzz 출력

number = int(input("숫자를 입력하세요 : "))

if number % 15 == 0:
    print("fizzbuzz")
elif number % 3 == 0:
    print("fizz")
elif number % 5 == 0:
    print("buzz")
else:
    print(number)
```

반복문

- while
- for
- break, continue
- list comprehension

### while

- 조건이 False가 될때까지 코드를 반복합니다.
- break ; 반복문을 중단할 때 사용
- continue ; 코드 실행 순서를 바로 조건 확인하는 코드 부분으로 이동

```python
count = 0

while True:
    
    count += 1
    
    if count >= 10:
        break
    
    if count % 2:
        continue
        
    print(count)
```

```python
# quiz

words = []
while True:
    word = input("insert word(q:exit): ")
    
    if word == "q":
        print(words)
        break
    words.append(word)
```

```python
#1~45 :중복되지 않는 랜덤한 숫자
# while,list.sort(),not in, in, list.append()

import random

lotto = []

while True:
    number = random.randint(1,45)
    
    if number not in lotto:
        lotto.append(number)
    
    if len(lotto)==6:
        lotto.sort()
        break

print(lotto) # [1, 31, 35, 41, 42, 45]
```