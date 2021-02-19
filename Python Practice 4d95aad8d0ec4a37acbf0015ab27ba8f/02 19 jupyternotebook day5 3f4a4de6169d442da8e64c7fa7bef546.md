# 02/19 jupyternotebook day5

1. List & For
- list와 for문 이용하여 10 ~ 20까지 출력하는 코드를 만드세요.

```python
ls = [10,11,12,13,14,15,16,17,18,19,20]

for i in ls:
    print(i,end=" ")
```

2. Range

- list와 for문과 range를 이용하여 10 ~ 20까지 출력하는 코드를 만드세요.

```python
for number in range(10,21):
    print(number,end=" ")
```

3. 짝수 출력 (for, if, range)

- 0 ~ 20까지 짝수만 출력하는 코드를 작성하세요.

```
for i in range(10,21,2):
    print(i, end=" ")
```

4. 홀수 더하기 (for, if, range)

- 10 ~ 20까지의 숫자에서 홀수를 다 더한 결과를 출력하는 코드를 작성하세요.

```python
result=0

for i in range(11,21,2):
        result += i
print(result)
```

5. 카드 사용하기 (break)

- 50000 원이 들어 있는 카드가 있습니다.
- 카드를 계속 사용해서 카드에 있는 돈으로 결제가 불가능한경우 "결제가 불가능합니다. 현재금액(x)" 라고 출력하고 프로그램을 종료하는 코드를 작성하세요.
- while과 break를 사용하세요.

```python
money = 50000
print("현재 결재가능 금액 : {}".format(money))

while True:
    data = int(input("결제할 금액을 입력해주세요 : "))
    money -= data
    
    if money < 0:
        money += data
        break  
    else:
        print("결제되었습니다. 현재금액({}원)".format(money))

print("결제가 불가능합니다. 현재금액{}원".format(money))
```

6. 랜덤 숫자 맞추기 (continue)

- 0 ~ 9 까지 랜덤한 숫자를 생성하여 숫자를 맞출때까지 숫자를 입력받는 코드를 작성하세요.
- continue를 사용하세요.

숫자를 입력하세요 : 1
숫자를 입력하세요 : 2
숫자를 입력하세요 : 3
숫자를 입력하세요 : 4
숫자를 입력하세요 : 5
숫자를 입력하세요 : 6
숫자를 입력하세요 : 7
숫자를 입력하세요 : 8
숫자를 맞췄습니다

```python
import random
import random
number = random.randint(0,9)

while True:
    i = int(input("숫자를 입력하세요 : "))
    
    if i == number:
        print("숫자를 맞췄습니다.")
        break
```

7. 달리기 시합 결과 입력 및 출력 하기 (enumerate)

- 토끼, 거북이, 사슴이 시합을 했는데 1등부터 결과를 입력받아서 등수와 동물을 출력하는 코드를 작성하세요.
- 등수를 출력할때 enumerate 를 사용해주세요.
- 결과

```
1등 한 동물은 ? 사슴
2등 한 동물은 ? 토끼
3등 한 동물은 ? 거북이

결과
1등 - 사슴
2등 - 토끼
3등 - 거북이

```

```python
animals = [input("1등 한 동물은? "),input("2등 한 동물은? "),input("3등 한 동물은? ")]

print("\n결과")
for idx,animal in enumerate(animals):
    print("{}등 - {} ".format(idx+1,animal))
```

8**.** 중복 문자의 갯수 찾기 (zip)

- 두개의 문자열을 입력받아 문자가 같고 자리가 같은 중복된 문자와 갯수를 출력하는 코드를 작성하세요.
- "abcde" 와 "adcbe"의 중복된 문자는 3개 입니다.
- 문자열의 갯수는 같게 설정이 되어야 합니다.

```python
a = input("입력하세요:")
b = input("입력하세요:")

count = 0

overlap = ""

for n,v in zip(a,b):
    if n==v:
        count+=1
        overlap += n

print("중복된 문자는 {}이고, 중복된 문자의 갯수는 {}개 입니다.".format(overlap,count))
```

9. 역삼각형 별 출력

- 5를 입력했을때 아래와 같이 별이 출력되도록 코드를 작성하세요.
- 홀수만 입력이 가능합니다.

```
*****
 ***
  *

```

```python
number = 5
space = 0
for star in range(number,0,-2):
    print(" "*space,"*"*star)
    space +=1
```

10. fizz, buzz

- 시작 숫자와 끝나는 숫자를 입력받으세요.
- while 문을 사용하세요.
- 결과 출력
    - 3의 배수는 fizz 출력
    - 5의 배수는 buzz 출력
    - 3과 5의 배수는 fizzbuzz 출력
    - 3과 5의 배수가 아니면 그냥 숫자가 출력

    ```python
    start = int(input("입력하세요 : "))
    end = int(input("입력하세요 : "))
    print()

    number = start

    while number <= end:
        
        if number % 15 ==0:
            print("fizzbuzz", end=" ")
        elif number % 3 ==0:
            print("fizz", end=" ")
        elif number % 5 ==0:
            print("buzz", end=" ")
        else:
            print(number, end=" ")
        
        number += 1
    ```

    11. 가로로 구구단 출력

    - print 문에서 tab으로 띄우는 방법 : end 옵션에 "\t"를 사용
    - 아래와 같이 결과가 출력되도록 반복문을 사용하여 코드를 작성하세요.

    ```python
    for j in range(0,10):
        
        for dan in range(2,10):
            
            if j == 0:
                print("{}단".format(dan),end="\t")
            else:
                print("{}*{}={}".format(dan, num1, dan*num1), end="\t")
        print()
    ```