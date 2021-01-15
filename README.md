# Python grammar

<01/15> python practice -random함수

- from random import *

    random() ⇒ 0.0~10.0미만의 임의의 값 생성

    int(random()*10) ⇒ 0~10 미만의 임의의 값 생성

    int(random()*10+1) ⇒ 1~10 미만의 임의의 값 생성

    randrange(1,11) ⇒ 1~11 미만

    randint(1,10) ⇒ 1~10을 포함하여 랜덤출력

```python
from random import *
date = randint(4,28)
print("오프라인 스터디 모임 날짜는 매월 "+ str(date) +"일로 선정되었습니다.
```

- sentence 함수

```python
sentence ='나는 소년입니다'
print(sentence)
sentence2 = "파이썬은 쉬워요"
print(sentence2)
sentence3 = """   #줄바꾸기포함출력
나는 소년이고,
파이썬은 쉬워요
"""
print(sentence3)
```

- 슬라이싱

```python
jumin = "990120-1234567"

print("성별 : " + jumin[7])
print("연 : " +jumin[0:2]) #0번째부터 실제값보다하나더큰거까지 적어줘야함.
print("월 :" +jumin[2:4])
print("일 :" +jumin[4:6])

print("생년월일 : " +jumin[:6]) #콜론은 처음부터 6직전까지
print("뒤 7자리 : " +jumin[7:]) #7부터 끝까지
print("뒤 7자리 : " +jumin[-7:])#맨뒤에서 일곱번째부터 끝까지
```

- 문자열처리함수

```python
python = "Python is Amazing"

print(python.lower()) #소문자
print(python.upper()) #대문자
print(python[0].isupper()) #true 첫번째 문자가 대문자가 맞는지 isupper로 확인
print(len(python))
```

<01/16> python practice 

- 문자열 포맷

```python
# 방법 1
print("나는 %d살입니다."%20) #%d는 정수
print("나는 %s을 좋아해요."%"파이썬") # %s는 정수,문자열
print("Apple 은 %c로 시작해요" % "A") # %c는 문자

%s
print("나는 %s색과 %s색을 좋아해요."%("파란","빨강"))

방법 2
print("나는 {}살입니다.".format(20))
print("나는 {}색과 {}색을 좋아해요.".format("파란","빨강"))
print("나는 {0}색과 {1}색을 좋아해요.".format("파란","빨강"))
print("나는 {1}색과 {0}색을 좋아해요.".format("파란","빨강"))

방법 3 
print("나는 {age}살이며, {color}색을 좋아해요.".format(age=20,color="빨간"))

방법 4 (v3.6 이상~)
age = 20
color = "빨간"
print(f"나는 {age}살이며, {color}색을 좋아해요.")
```

- 탈출 문자

```python
print("백문이 불여일견 \n백견이 불여일타")

저는 "나도코딩"입니다.
print("저는 \"나도코딩\"입니다.")
print("저는 \'나도코딩\'입니다.")

\\ : 문장 내에서 \
print("C:\\Users\\Hyunsil\\Desktop\\pythonwork>")

\r : 커서를 맨 앞으로 이동
print("Red Apple\rPine") # Red < 부분이 사라지고 Pine 삽입됨.

\b : 백스페이스 (한 글자 삭제)
print("redd\bApple") # 키보드에서 backspace 역할처럼 d를 하나 지워줌.

\t :탭
print("Red\tApple") # 키보드 탭 역할
```

- 리스트

```python
지하철 칸별로 10명, 20명, 30명
subway = ["유재석","조세호","박명수"]
print(subway)

# 조세호씨가 몇 번째 칸에 타고있는가?
print(subway.index("조세호"))

# 하하씨가 다음 정류장에서 다음 칸에 탐
subway.append("하하")
print(subway)

# 정형돈씨를 유재석 / 조세호 사이에 태워봄
subway.insert(1,"정형돈")
print(subway)

지하철에 있는 사람을 한 명씩 뒤에서 꺼냄
print(subway.pop())
print(subway)

print(subway.pop())
print(subway)

print(subway.pop())
print(subway)

같은 이름의 사람이 몇 명 있는지 확인
subway.append("유재석")
print(subway)
print(subway.count("유재석"))

정렬도 가능
num_list = [5,2,4,3,1]
num_list.sort()
print(num_list)

순서 뒤집기도 가능 
num_list.reverse()
print(num_list)

모두지우기
num_list.clear()
print(num_list)

다양한 자료형 함께 사용
num_list = [5,2,4,3,1]
mix_list = ["조세호", 20, True]
#print(mix_list)

#리스트 확장
mix_list.extend(num_list)
print(mix_list)

cabinet = {3:"유재석",100:"김태호"}
print(cabinet[3])

print(cabinet.get(100))
print(cabinet.get(5)) #none으로 나옴
print(cabinet.get(5,"사용가능"))
print("hi")

print(3 in cabinet) #true
print(5 in cabinet) #faluse
```

- 캐비넷

```python
cabinet = {"A-3":"유재석","B-100":"김태호"}
print(cabinet["A-3"])
print(cabinet["B-100"])

# 새 손님
print(cabinet)
cabinet["A-3"]="김종국"
cabinet["C-20"]="조세호"
print(cabinet)

# 간 손님
del cabinet["A-3"]
print(cabinet)

# key 들만 출력
print(cabinet.keys())

#value 들만 출력
print(cabinet.values())

# key, value 함께
print(cabinet.items())

#목욕탕 폐점
cabinet.clear()
print(cabinet)
```

- 튜플

```python
menu = ("돈까스", "치즈까스")
print(menu[0])
print(menu[1])
# menu.add("생선까스")
(name,age,hobby)=("김종국",20,"코딩")
print(name,age,hobby)
```

- 집합(SET)

```python
######### 집합(set) 중복 안됨, 순서 없음
my_set={1,2,3,3,3}
print(my_set)

java = {"유재석","김태호","양세형"}
python = set(["유재석","박명수"])

# 교집합 (java와 python 모두 할 수 있는 사람)
print(java & python)
print(java.intersection(python))

# 합집합 (java도 할 수 있거나 python 할 수 있는 개발자)
print(java|python)
print(java.union(python))

# 차집합 (java 할 수 있지만 python 할 줄 모르는 개발자)
print(java-python)
print(java.difference(python))

#python 할 줄 아는 사람이 늘어남
python.add("김태호")
print(python)

#java를 잊었어요
java.remove("김태호")
print(java)
```

- 자료구조의 변경

```python
 커피숍
 menu={"커피","우유","주스"}
 print(menu, type(menu))

 menu = list(menu)
 print(menu, type(menu))

 menu = tuple(menu)
 print(menu, type(menu))

 menu = set(menu)
 print(menu, type(menu))
```

- QUIZ (비밀번호생성)

```python
############### QUIZ3 (복습필요)
사이트별로 비밀번호를 만들어주는 프로그램을 작성하시오
예) http://naver.com
규칙1 : http:// 부분은 제외 => naver.com
규칙2 : 처음 만나는 점(.) 이후 부분은 제외 => naver
규칙3 : 남은 글자 중 처음 세자리 + 글자 갯수 + 글자 내 'e' 갯수 + "!"로 구성

예) 생성된 비밀번호 : nav51!

url = "http://naver.com"
my_str = url.replace("http://","") #규칙1
my_str = my_str[:my_str.index(".")] #규칙2
password = my_str[:3]+str(len(my_str))+str(my_str.count("e"))+"!"
print("{0} 의 비밀번호는 {1} 입니다.".format(url,password))
```

- QUIZ 2 (댓글이벤트 만들기)

```python
####################### 댓글이벤트 quiz

from random import *
users = range(1,21) #1~20번까지 아이디가 숫장인 사람들
#print(type(users))
users = list(users) #list로 class변경
# print(type(users))

print(users) 
shuffle(users) # 무작위 셔플
print(users)

winners = sample(users,4) #당첨자 총 4명 뽑기
print("--- 당첨자 발표 --")
print("치킨 당첨자 :{0}".format(winners[0]))
print("커피 당첨자 :{0}".format(winners[1:]))
print("---축하합니다---")
```