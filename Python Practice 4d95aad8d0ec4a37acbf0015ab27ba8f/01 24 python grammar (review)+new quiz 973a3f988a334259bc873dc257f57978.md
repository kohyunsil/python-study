# 01/24 python grammar (review)+new quiz

- quiz 4

```python
from random import *

customer = range(1,21)
customer = list(customer)  #type 설정
shuffle(customer)          #1~20까지 섞기

winners = sample(customer,4) # 4개 뽑기

print("--- 당첨자 발표 ---")
print("치킨 당첨자 : {0}".format(winners[0]))
print("커피 당첨자 : {0}".format(winners[1:]))
print("--- 축하합니다 ---")
```

- quiz 5

```python
from random import *

customer=0
for i in range(1,51):
    time=randrange(5,51)

    if 5 <= time <= 15:
        print("[O] {0}번째 손님 (소요시간 : {1}분)".format(i,time))
        customer += 1
    else:
        print("[ ] {0}번째 손님 (소요시간 : {1}분)".format(i,time))

print("총 탑승 승객 : {0}분".format(customer))
```

- def 함수 연습

```python
def open_account():
    print("새로운 계좌가 생성되었습니다.")

def deposit(balance, money): #입금
    print("입금이 완료되었습니다. 잔액은 {0}원입니다.".format(balance+money))
    return balance + money

def withdraw(balance, money):
    if balance >= money:
        print("출금이 완료되었습니다. 잔액은 {0}원입니다.".format(balance-money))
        return balance - money
    else:
        print("출금이 완료되지 않았습니다. 잔액은 {0}원입니다.".format(balance))
        return balance
def withdraw_night(balance, money):
    commission = 100
    return commission, balance-money-commission

balance = 0
balance = deposit(balance, 1000)
# balance = withdraw(balance, 500)
commission, balance = withdraw_night(balance, 500)
print("수수료 {0}원, 잔액은 {1}원입니다.".format(commission,balance))
```

- quiz 3

```python
def std_weight(height,gender):
    if gender == "남자":
        return height*height*22
    else:
        return height*height*21

height = 160
gender = "여자"
weight = round(std_weight(height/100,gender),2)

print("키 {0}cm {1}의 표준 체중은 {2}입니다.".format(height,gender,weight))
```

- quiz #1

```python
names = ["유튜버1","유튜버2","유튜버3","유튜버4"]
for name in names:
    with open("{}.txt".format(name),"w",encoding="utf8") as email_file:
        email_file.write(f"""
안녕하세요? {name}님.

(주)나도출판 편집자 나코입니다.
현재 저희 출판사는 파이썬에 관한 주제로 책 출간을 기획 중입니다.
{name}님의 유튜브 영상을 보고 연락을 드리게 되었습니다.
자세한 내용은 첨부드리는 제안서를 확인 부탁드리며, 긍정적인 회신 기다리겠습니다.

좋은 하루 보내세요 ^^
감사합니다.

- 나코 드림.
""")
```

![01%2024%20python%20grammar%20(review)+new%20quiz%20973a3f988a334259bc873dc257f57978/Untitled.png](01%2024%20python%20grammar%20(review)+new%20quiz%20973a3f988a334259bc873dc257f57978/Untitled.png)

![01%2024%20python%20grammar%20(review)+new%20quiz%20973a3f988a334259bc873dc257f57978/Untitled%201.png](01%2024%20python%20grammar%20(review)+new%20quiz%20973a3f988a334259bc873dc257f57978/Untitled%201.png)

- 다양한 출력포맷

```python
# 빈 자리는 빈공간으로 두고, 오른쪽 정렬을 하되, 총 10자리 공간을 확보
print("{0: >10}".format(500))

#양수일 땐 +로 표시, 음수일 땐 - 로 표시
print("{0: >+10}".format(-500))

#왼쪽 정렬하고, 빈칸으로 _로 채움
print("{0:_<10}".format(500))

#3자리 마다 콤마를 찍어주기
print("{0:,}".format(5000000000))

#3자리 마다 콤마를 찍어주기,+-부호도붙이기
print("{0:+,}".format(5000000000))
print("{0:+,}".format(-5000000000))

#3자리 마다 콤마를 찍어주면서 부호도 붙이고 자릿수도 확보하기, ^로 빈자리 채워주기
print("{0:^<+30,}".format(5000000000))

#소수점 출력
print("{0:f}".format(5/3))
#반올림
print("{0:.2f}".format(5/3))
```

with 함수 퀴즈

```python
 for i in range(1,51):
     with open(str(i)+"주차.txt","w", encoding="utf8") as work_file:
         work_file.write("- {0} 주차 주간보고".format(i))
         work_file.write("부서 : ")
         work_file.write("이름 : ")
         work_file.write("업무 요약 : ")
```