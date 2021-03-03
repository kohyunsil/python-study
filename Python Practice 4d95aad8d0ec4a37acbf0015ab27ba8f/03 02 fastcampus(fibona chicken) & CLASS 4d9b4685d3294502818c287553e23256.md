# 03/02 fastcampus(fibona chicken) & CLASS

fibona chicken

```python
def fibo_chicken(people):
    # 피보나치 수열 데이터
    a, b, fibo = 0, 1, [1]
    # 피보나치킨 수열 데이터 (idx:피보나치수열에서 떨어진수)
    idx, fc = 0, []
    
    for number in range(1, people + 1):
        # print(number, end=" ")
        
        # 피보나치 수열 생성 : 사람 숫자가 생성되는 피보나치 수보다 크거나 같을때 생성
        if a + b <= number:
            a, b = b, a + b
            fibo.append(b)
        
        # 피보나치킨 수열 생성
        # 1. 사람의 수가 피보나치 수인 경우
        if number == fibo[-1]:
            fc.append(fibo[-2])
            idx = 0
        
        # 2. 사람의 수가 피보나치 수가 아닌 경우
        else:
            fc.append(fibo[-2] + fc[idx])
            idx += 1
                    
    print(fibo)
    print(fc)
    
    return fc[-1]
```

### 상속

만들어 놓은 클래스의 기능을 물려 받아서 추가 기능만 코드로 작성하는 방법

```
class Iphone1:
    def call():

class Iphone2:
    def call():
    def sms():

class Iphone3:
    def call():
    def sms():
    def internet():

```

- 상속 사용

```
class Iphone1:
    def call():

class Iphone2(Iphone1):
    def sms():

class Iphone3(Iphone2):
    def internet():

```

- 다중상속이 가능

```
class Galaxy1:
    def video_call():

# Iphone1 > galaxy1 > galaxy2
class Galaxy2(galaxy1, Iphone1):
    def samsung_pay():

```

```python
class Iphone1:
    def call(self):
        print("call")

class Iphone2(Iphone1):
    def sms(self):
        print("sms")
    
class Iphone3(Iphone2):
    def internet(self):
        print("internet")

i1 = Iphone1()
i2 = Iphone2()
i3 = Iphone3()

[var for var in dir(i1) if var[0] != "_"],\
[var for var in dir(i2) if var[0] != "_"],\
[var for var in dir(i3) if var[0] != "_"]

class Galaxy1:
    def video_call(self):
        print("video_call")

# Iphone1 > galaxy1 > galaxy2
class Galaxy2(Galaxy1, Iphone1):
    def samsung_pay(self):
        print("samsung_pay")

g1=Galaxy1()
g2=Galaxy2()

[var for var in dir(g1) if var[0] != "_"],\
[var for var in dir(g2) if var[0] != "_"]

```

상속 퀴즈

```python
# quiz
# 종족 
# human : health=40, attack_pow=10, defence=5, steam_pack(): attack_pow*=2
# orc : health=60, attack_pow=5, defence=10, shild(): defence*=2
# unit class : warrior(spec() : health+10), wizard(spec() : attack+10)

def disp_var(obj):
    return [var for var in dir(obj) if var[0] != "_"]

class Human:
    health,attack_pow,defence = 40,10,5
    def steam_pack(self):
        self.attack_pow *= 2

class Orc:
    health, attack_pow, defence=60,5,10
    def shild(self):
        self.defence *= 2

class HumanWizard(Human):
    def spec(self):
        self.attack_pow += 10

class OrcWarrior(Orc):
    def spec(self):
        self.health += 10
        
hwi = HumanWizard()
owa = OrcWarrior()

hwi.health, hwi.attack_pow, hwi.defence,\
disp_var(hwi)
owa.health, owa.attack_pow, owa.defence,\
disp_var(owa)
```

오버라이딩 (Overriding, overwrite)

```python
# Orc : health, attack_pow, defence
# 오버라이딩(overriding, overwrite)
# 부모 클래스의 변수(메서드)를 다시 선언해주면 변수(메서드)가 재선언
class Human(Orc):
    health, attack_pow, defence = 40, 10, 5
    def steam_pack(self):
        self.attack_pow *= 2
        
# Human : health, attack_pow, defence 상속
class HumanOrcWizard(Human, Orc):
    
    def __init__(self):
        self.spec()
    
    def spec(self):
        self.attack_pow += 10
        
howi = HumanOrcWizard()
howi.health, howi.attack_pow, howi.defence
```

메서드 오버라이딩

- 메서드 오버로딩 : 같은 메서드 이름으로 아큐먼트를 다르게 했을때 다른 코드가 동작되도록 하는법

```python
def test(num1, num2=None):
    if num2 is None:
        print("test1")
        return num1
    else:
        print("test2")
        return num1, num2

test(1)
test(1,2)
```

super - 부모 클래스의 메서드 코드를 가져올때 사용

```python
class Human:
    
    def __init__(self):
        self.health=40
    def move(self):
        print("move")

class Marine(Human):
    def __init__(self):
        self.attack_pow=5
        super(Marine, self).__init__()
    def attack(self):
        super(Marine, self).move()
        print("attack")

m=Marine()
disp_var(m)
m.attack()
```

private 변수

- mangling을 이용하여 private 변수(메서드)를 설정합니다.
- mangling은 변수(메서드)명 앞에 `__`를 붙여 줍니다.

```python
class Calculator:
    
    def __init__(self, num1=1, num2=2):
        self.__num1 = num1
        self.__num2 = num2
        
    def setter_num1(self, num1):
        if isinstance(num1, (int, float)):
            self.__num1 = num1
        else:
            print("wrong datatype(able int, float)!")
        
    def getter_num1(self):
        datas = {1: "one", 2: "two", 3: "three"}
        return datas[self.__num1]
        
    def plus(self):
        return self.__num1 + self.__num2
    
    number1 = property(getter_num1, setter_num1)
```

is a / has a

- 클래스를 설계할때 사용하는 방법(개념)
- is a  : A is a B : A는 B이다 : B는 A를 상속 받았다.
- has a : 객체를 생성할때 생성자 메서드에서 객체를 받아서 생성하는 방법 : 객체 안에 객체

```python
# is a
class Info:
    def __init__(self, name, age):
        self.name= name
        self.age=age
        
class User(Info):
    def disp(self):
        print(self.name, self.age)

u = User("peter",20)
u.disp()
disp_var(u)
```

```python
# has a
class Name:
    def __init__(self, name):
        self.name_str=name

class Age:
    def __init__(self, age):
        self.age_int=age

class User:
    def __init__(self,name_obj,age_obj):
        self.name=name_obj
        self.age=age_obj
    def disp(self):
        print(self.name.name_str, self.age.age_int)

name=Name("peter")
age=Age(20)

u=User(name,age)
u.disp()
disp_var(u)
```

클래스 메서드의 종류

- Instance Method
    - 인스턴스(객체)를 통해서 호출하는 메서드
    - 파라미터에 self를 사용
- Class Method
    - 클래스를 통해서 호출
    - 파라미터에 cls를 사용
- Static Method
    - 객체를 생성하지 않아도 바로 클래스를 통해서 일반 함수처럼 메서드 사용가능
    - 일반함수와 같지만 위치만 클래스 내부