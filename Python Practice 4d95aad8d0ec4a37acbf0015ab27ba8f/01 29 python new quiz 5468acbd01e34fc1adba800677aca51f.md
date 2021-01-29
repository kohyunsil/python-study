# 01/29 python new quiz

- 신조어 퀴즈 (class사용)

```python
class Word:
    def __init__(self, newword, ex1, ex2, answer):
        self.newword=newword
        self.ex1=ex1
        self.ex2=ex2
        self.answer=answer
    
    def show_question(self):
        print(f"\"{self.newword}\"의 뜻은?")
        print("1."+self.ex1)
        print("2."+self.ex2)

    def check_answer(self, user_input):
        if user_input == self.answer:
            print("정답입니다.")
        else:
            print("틀렸습니다.")

newword=Word("얼죽아","얼어 죽어도 아메리카노","얼굴만은 죽어도 아기피부", 1)
newword.show_question()
newword.check_answer(int(input("=> ")))
```

![01%2029%20python%20new%20quiz%205468acbd01e34fc1adba800677aca51f/Untitled.png](01%2029%20python%20new%20quiz%205468acbd01e34fc1adba800677aca51f/Untitled.png)