# Python grammar

<01/15> python practice -random함수

- from random import *

    random() ⇒ 0.0~10.0미만의 임의이 값 생성

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