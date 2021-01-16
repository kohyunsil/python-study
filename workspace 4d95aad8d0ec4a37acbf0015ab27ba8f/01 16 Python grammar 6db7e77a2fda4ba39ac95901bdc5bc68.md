# 01/16 Python grammar

- if/elif/else

```python
weather = input("오늘 날씨는 어때요? ")
if weather == "비" or weather=="눈":
    print("우산을 챙기세요")
elif weather == "미세먼지":
    print("마스크를 챙기세요")
else:
    print("준비물 필요 없어요")

temp = int(input("기온은 어때요? "))
if 30<=temp:
    print("너무 더워요. 나가지 마세요")
elif 10 <=temp and temp<30:
    print("괜찮은 날씨에요")
elif 0<=temp<10:
    print("외투를 챙기세요")
else:
    print("너무 추워요. 나가지 마세요")
```

- for / while
- for / while

```python
#################### for
for waiting_no in range(1,5):
    print("대기번호 : {0}".format(waiting_no))

starbucks=["아이언맨","토르","머쉬베놈"]
for customer in starbucks:
    print("{0}, 커피가 준비되었습니다.".format(customer))

################### while
customer = "토르"
index = 5
while index >=1 :
    print("{0}님 커피가 준비 되었습니다. {1}번 남았어요.".format(customer,index))
    index -=1
    if index == 0:
        print("커피는 폐기처분되었습니다.")

customer = "토르"
person = "Unknown"

while person != customer :
    print("{0}님 커피가 준비 되었습니다.".format(customer))
    person = input("이름이 어떻게 되세요?")

absent = [2,5] #결석
no_book = [7] #책을 깜빡했음
for student in range (1,11): 
    if student in absent: #2,5번 뛰어넘고 계속
        continue
    elif student in no_book:
        print("오늘 수업 여기까지. {0}는 교무실로 따라와".format(student))
        break
    print("{0}, 책을 읽어봐.".format(student))
```

- 한 줄 for문

```python
students=[1,2,3,4,5]
students=[i+100 for i in students]
print(students)

## 학생 이름을 길이로 변환
students=["iron men", "thor", "i am groot"]
students=[len(i) for i in students]
print(students)

students=["iron men", "thor", "i am groot"]
students=[i.upper() for i in students]
print(students)
```

- 함수

```python
def deposit(balance, money): #입금
    print ("입금이 완료되었습니다. 잔액은 {0}원입니다.".format(balance+money))
    return balance+money

def withdraw(balance, money): #출금
    if balance >= money: #잔액이 출금보다 많으면 출금가능
        print("출금이 완료되었습니다. 잔액은 {0}원입니다.".format(balance-money))
        return balance-money
    else:
        print("출금이 불가합니다. 잔액은 {0}원입니다.".format(balance))
        return balance

def withdraw_night(balance, money): #저녁에 출금
    commission = 100 #수수료 100원
    return commission, balance-money-commission

balance=0
balance = deposit(balance,1000)
#balance = withdraw(balance,500)
commission, balance = withdraw_night(balance,500)
print("수수료 {0}원이며, 잔액은 {1}원입니다.".format(commission,balance))
```

- 함수기본값

```python
#################### 함수 기본값

def profile(name, age=17, main_lang="파이썬"):
    print("이름:{0}\t나이:{1}\t  주 사용 언어:{2}"\
    .format(name, age, main_lang))

profile("유재석")
profile("김태호")
```

- 가변인자 *

```python
def profile(name,age,*language):
    print("이름 : {0}\t나이 : {1}\t".format(name,age), end=" ")
    for lang in language:
        print(lang, end=" ")
    print()

profile("유재석",20, "python","c","c++","java","swift","c#")
```

- 지역변수와 전역변수

```python
gun = 10

def checkpoint(soldiers): #경계근무
    global gun #전역 공간에 있는 gun 사용
    gun = gun-soldiers
    print("[함수내] 남은 총: {0}".format(gun))

def checkpoint_ret(gun,soldiers):
    gun = gun-soldiers
    print("[함수내] 남은 총: {0}".format(gun))
    return gun

print("전체 총 :{0}".format(gun))
#checkpoint(2) # 2명이 경계 근무 나감
gun = checkpoint_ret(gun,2)
print("남은 총 :{0}".format(gun))
```

- QUIZ

당신은 Cocoa 서비스를 이용하는 택시 기사님입니다.
50명의 승객과 매칭 기회가 있을 때, 총 탑승 승객 수를 구하는 프로그램을 작성하시오.

조건1 : 승객별 운행 소요 시간은 5분 ~ 50분 사이의 난수로 정해집니다.
조건2 : 당신은 소요 시간 5분 ~ 15분 사이의 승객만 매칭해야 합니다.

(출력문 예제)
[0] 1번째 손님 (소요시간 : 15분)
[ ] 2번째 손님 (소요시간 : 50분)
[0] 3번째 손님 (소요시간 : 5분)
...
[ ] 50번째 손님 (소요시간 : 16분)

총 탑승 승객 : 2 분

```python
from random import *
cnt=0 # 총 탑승 승객 수
for customer in range(1,51): # 1~50이라는 수
    time = randrange(5,51) # 소요시간 5분~50분
    if 5 <= time <=15: #매칭 성공(5~15분), 탑승수카운트
        print("[0] {0}번째 손님 (소요시간 : {1}분)".format(customer,time))
        cnt += 1
    else:
        print("[ ] {0}번째 손님 (소요시간 : {1}분)".format(customer,time))
print("총 탑승 승객 : {0}분".format(cnt))
```

- QUIZ

QUIZ ) 표중 체중을 구하는 프로그램을 작성하시오
* 표준 체중 : 각 개인의 키에 적당한 체중

(성별에 따른 공식)
남자 : 키(m)x키(m)x22
여자 : 키(m)x키(m)x21

조건1 : 포준 체중은 별도의 함수 내에서 계산
* 함수명 : std_weight
* 전달값 : 키(height),성별(gender)
조건2 : 표준 체중은 소수점 둘째자리까지 표시                                                                                         <출력 => 키 175cm 남자의 표준 체중은 67.38kg 입니다.>

```python
def std_weight(height,gender):
    if gender == "남자":
        return height * height * 22
    else:
        return height * height * 21

height= 175
gender= "남자"
weight= round(std_weight(height/100,gender),2)
print("키 {0}cm {1}의 표준 체중은 {2}kg 입니다.".format(height, gender, weight))
```