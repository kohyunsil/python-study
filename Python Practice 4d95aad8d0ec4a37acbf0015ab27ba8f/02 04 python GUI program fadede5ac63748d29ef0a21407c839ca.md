# 02/04 python GUI program

- create_frame

```python
from tkinter import *

root = Tk()
root.title("Siri GUI")
root.geometry("640x480") #가로*세로
# root.geometry("640x480+300+100") #가로*세로+x좌표+y좌표

root.resizable(False, False) # x(너비), y(높이) 값 변경 불가 (창 크기 변경 불가)

root.mainloop()
```

- button

```python
from tkinter import *

root = Tk()
root.title("Siri GUI")

btn1 = Button(root, text="버튼1")
btn1.pack()

btn2 = Button(root, padx=5, pady=10, text="버튼2")
btn2.pack()

btn3 = Button(root, padx=10, pady=5, text="버튼3")
btn3.pack()

btn4 = Button(root, width=10, height=5, text="버튼4")
btn4.pack()

btn5 = Button(root, fg="red", bg="yellow", text="버튼5")
btn5.pack()

photo = PhotoImage(file="gui_basic/img.png")
btn6 = Button(root, image=photo)
btn6.pack()

def btncmd():
    print("버튼이 클릭되었어요")
btn7 = Button(root, text="동작하는 버튼",command=btncmd)
btn7.pack()

root.mainloop()
```

- label

```python
from tkinter import *

root = Tk()
root.title("Siri GUI")
root.geometry("640x480")

label1=Label(root, text="안녕하세요")
label1.pack()

photo = PhotoImage(file="gui_basic/img.png")
label2=Label(root, image=photo)
label2.pack()

def change():
    label1.config(text="또 만나요")

    global photo2
    photo2= PhotoImage(file="gui_basic/img2.png")
    label2.config(image=photo2)

btn=Button(root, text="클릭", command=change)
btn.pack()

root.mainloop()
```

- text_entry

```python
from tkinter import *

root = Tk()
root.title("Siri GUI")
root.geometry("640x480") #가로*세로

txt = Text(root, width=30, height=5)
txt.pack()

txt.insert(END, "글자를 입력하세요")

e = Entry(root, width=30)
e.pack()
e.insert(0, "한 줄만 입력해요")

def btncmd():
    #내용 출력
    print(txt.get("1.0",END)) # 1 :  첫번째 라인, 0 : 0번째 column위치
    print(e.get())

    #내용 삭제
    txt.delete("1.0", END)
    e.delete(0,END)

btn = Button(root, text="클릭",command=btncmd)
btn.pack()

root.mainloop()
```

- listbox

```python
from tkinter import *

root = Tk()
root.title("Siri GUI")
root.geometry("640x480") #가로*세로

listbox = Listbox(root, selectmode = "extended", height=0)  #selectmode="single"-한개만선택
listbox.insert(0, "사과")
listbox.insert(1, "딸기")
listbox.insert(2, "바나나")
listbox.insert(END,"수박")
listbox.insert(END,"포도")
listbox.pack()

def btncmd():
    # listbox.delete(0) # 맨 앞 항목을 삭제
    # listbox.delete(END) # 맨 뒤에 항목을 삭제

    # 갯수 확인
    # print("리스트에는",listbox.size(),"개가 있어요")

    # 항목 확인 (시작 ind)
    # print("1번째부터 3번째까지의 항목 :", listbox.get(0,2))

    # 선택된 항목 확인 ( 위치로 반환 ex) 1,2,3 )
    print("선택된 항목 :" ,listbox.curselection())

btn = Button(root, text="클릭",command=btncmd)
btn.pack()

root.mainloop()
```

- check box

```python
from tkinter import *

root = Tk()
root.title("Siri GUI")
root.geometry("640x480") #가로*세로

chkvar=IntVar() #chkvar에 int 형으로 값을 저장한다
chkbox=Checkbutton(root, text="오늘 하루 보지 않기",variable=chkvar)
# chkbox.select() #자동 선택 처리
# chkbox.deselect() # 선택 해제 처리
chkbox.pack()

chkvar2=IntVar()
chkbox2 = Checkbutton(root, text="일주일 동안 보지 않기",variable=chkvar2)
chkbox2.pack()

def btncmd():
    print(chkvar.get()) # 0 : 체크 해제, 1: 체크
    print(chkvar2.get())
btn = Button(root, text="클릭",command=btncmd)
btn.pack()

root.mainloop()
```