# 01/25 python grammar (review)

- class 정의하기

클래스 정의

cat 클래스 정의

class cat:

''' 인스턴스 변수와 메소드 구현 '''

인스턴스 생성과 메소드 호출

cat 클래스의 인스턴스 생성, cat1이 이를 참조

cat1 = cat()   #cat 인스턴스 생성 cat1이 이를 참조함

cat1.meow()  #cat이 구현한 메소드 호출

```python
class Cat:
	def meow(self): #meow() 메소드
			print("야옹 야옹 ~~~")

cat1 = Cat()
cat1.meow()
```

- 실습 1

```python
class Cat:
    def info(self):
        self.name="나비"
        self.color="검정색"
        print("고양이 이름은 : ",self.name,"\n색깔은 : ",self.color)

cat = Cat()
cat.info()
```

- 실습 2

```python
class Cat:
    def __init__(self, name,color):
        self.name=name
        self.color=color
        
    def info(self):
        print("고양이 이름은 : ",self.name,"/ 색깔은 : ",self.color)

cat1 = Cat("네로","검정색")
cat2 = Cat("미미","갈색")

cat1.info()
cat2.info
```

- class 관련 퀴즈

```python
class House:
    def __init__(self, location, house_type, deal_type, price, completion_year):
        self.location=location
        self.house_type=house_type
        self.deal_type=deal_type
        self.price=price
        self.completion_year=completion_year
    
    def show_detail(self):
        print(self.location, self.house_type, self.deal_type, \
self.price, self.completion_year)

houses=[]
house1=House("강남","아파트","매매","10억","2010년")
house2=House("마포","오피스텔","전세","5억","2007년")
house3=House("송파","빌라","월세","500/50","2000년")

houses.append(house1)
houses.append(house2)
houses.append(house3)

print("총 {0}대의 매물이 있습니다.".format(len(houses)))
for house in houses:
    house.show_detail()
```

- 예외처리,에러발생

```python
try:
    print("한 자리 나누기 전용 계산기입니다.")
    num1=int(input("첫번째 숫자를 입력하세요:"))
    num2=int(input("첫번째 숫자를 입력하세요:"))
    if num1 >= 10 or num2 >=10:
        raise ValueError
    print("{0} / {1} = {2}".format(num1,num2,int(num1/num2)))
except ValueError:
    print("에러! 잘못된 값을 입력하였습니다. 한 자리 숫자만 입력하세요.")
except ZeroDivisionError as err:
    print(err)
except Exception as err:
    print("알 수 없는 에러가 발생하였습니다.")
    print(err)
```

- 사용자 정의 예외처리, finally 구문(무조건 끝에 출력됨, 오류가 발생해도 출력됨)

```python
class BigNumberError(Exception):
    def __init__(self, msg):
        self.msg=msg
    
    def __str__(self):
        return self.msg

try:
    print("한 자리 나누기 전용 계산기입니다.")
    num1=int(input("첫번째 숫자를 입력하세요:"))
    num2=int(input("첫번째 숫자를 입력하세요:"))
    if num1 >= 10 or num2 >=10:
        raise BigNumberError("입력값 : {0}, {1}".format(num1,num2)) #에러발생후출력되는값
    print("{0} / {1} = {2}".format(num1,num2,int(num1/num2)))
except ValueError:
    print("에러! 잘못된 값을 입력하였습니다. 한 자리 숫자만 입력하세요.")
except BigNumberError as err:
    print("에러가 발생하였습니다. 한 자리 숫자만 입력하세요.")
    print(err)
finally:
    print("계산기를 이용해 주셔서 감사합니다.")
```

- 예외처리 퀴즈

```python
class SoldOutError(Exception):
    pass

chicken = 10
waiting = 1
while(True):
    try:
        print("[남은 치킨 : {0}]".format(chicken))
        order = int(input("치킨 몇 마리 주문하시겠습니까?"))

        if order > chicken: #남은 치킨보다 주문량이 많을때
            print("재료가 부족합니다.")
        elif order <= 0:
            raise ValueError
        else:
            print("[대기번호 {0}] {1} 마리 주문이 완료되었습니다." \
                .format(waiting, order))
            waiting += 1
            chicken -= order
        if chicken == 0:
            raise SoldOutError
    except ValueError:
        print("에러! 잘못된 값을 입력하였습니다.")
    except SoldOutError as err:
        print("재료가 소진되어 더 이상 주문을 받지 않습니다.")
        print(err)
        break
    except Exception as err:
        print("알 수 없는 에러가 발생하였습니다.")
        print(err)
```