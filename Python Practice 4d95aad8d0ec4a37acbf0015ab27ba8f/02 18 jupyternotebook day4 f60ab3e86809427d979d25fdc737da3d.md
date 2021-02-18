# 02/18 jupyternotebook day4

1. 7의 9제곱을 출력하세요.

```python
7**9
```

2. 1234 / 123 의 몫과 나머지를 출력하세요.

```python
1234//123
1234%123
```

3. 아래의 리스트 데이터에서 "python" 이 있으면 True를 출력, 없으면 False를 출력 하는 코드를 작성하세요.

```
data1 = ["python", "data", "fast"]
data2 = ["campus", "data", "fast"]

if "python" in data1:
    print("True")
else:
    print("False")

if "python" in data2:
    print("True")
else:
    print("False")
```

4. 아래의 코드를 삼항연산자로 변경하세요.

```python
```
keyword = "mac"
data = ["mac", "os", "windows", "linux"]
if keyword in data:
    print("데이터가 있습니다.")
else:
    print("데이터가 없습니다.")
    
```

keyword = "os"
data = ["mac", "os", "windows", "linux"]

print("데이터가 있습니다.") if keyword in data else print("데이터가 없습니다.")
```

5. not 연산자를 사용해서 홀수와 짝수를 구분하는 코드를 작성하세요.

```python
number = 5
결과 : 홀수
------------------------

number=5

if not number % 2 == 1:
    print("짝수")
if not number % 2 == 0:
    print("홀수")
```

5. not 연산자를 사용해서 홀수와 짝수를 구분하는 코드를 작성하세요.

```python
number = 5
결과 : 홀수
------------------------

number=5

if not number % 2 == 1:
    print("짝수")
if not number % 2 == 0:
    print("홀수")
```

**6. num1과 num2가 둘다 홀수 일때 True를 출력하는 코드를 작성하세요.**

```
num1, num2 = 3, 5
결과 : True

num1, num2 = 3, 6
결과 : False
```

```python
num1 = int(input("num1 : "))
num2 = int(input("num2 : "))

if num1 % 2 == 1 and num2 % 2 == 1:
    print(True,"{},{}".format(num1,num2))
else:
    print(False,"{},{}".format(num1,num2))
```

**7. number 변수의 정수가 짝수이면서 3의 배수인경우 True를 출력하는 코드를 작성하세요.**

```
number = 9
Fasle

number = 6
True
```

```python
number = int(input("number : "))

if number % 2 == 0 and number % 3 ==0:
    print(True)
else:
    print(False)
```

**8. 한자리수를 입력하면 앞에 0을 붙이고 두자리수를 입력하면 0을 붙이지 않도록 코드를 작성하세요.**

```
insert number : 7
result => 07

insert number : 22
result => 22
```

```python
number=str(input("insert number : "))
result=number.zfill(2)
print(result)
```

**9. 1월 1일이 월요일일때 1월 x일은 무슨 요일일까요?**

```
x는 31이하의 수를 입력받습니다.

week = ["일", "월", "화", "수", "목", "금", "토"]

결과
	insert day : 23
	'화'
```

```python
x=int(input("31이하의 수를 입력하세요. "))

if x % 7==1:
    print('월')
elif x % 7==2:
    print('화')
elif x % 7 == 3:
    print('수')
elif x % 7 == 4:
    print('목')
elif x % 7 == 5:
    print('금')
elif x % 7 == 6:
    print('토')
else:
    print('일')
```

**10. 요일을 입력받아 주말인지 아닌지를 판별하는 코드를 작성하세요.**

```
아래와 같이 변수를 선언해서 코드를 작성하세요.
weekend = ["토", "일"]
weekday = ["월", "화", "수", "목", "금"]

결과
	insert week : 월
	주중

	insert week : 일
	주말
```

```python
weekend = ["토", "일"]
weekday = ["월", "화", "수", "목", "금"]

a=input("요일을 입력하세요 : ")

if a in weekend:
    print("주말")
else:
    print("주중")
```