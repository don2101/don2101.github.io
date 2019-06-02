---
title: Python GUI based programming
tags: ["Python", "GUI"]
---



# GUI 기반 프로그램

GUI기반 데스크탑 프로그램 제작

<br><br>

## I. Python GUI 프로그램

**tkinter** 사용

기본적으로 **무한 루프**를 사용한 방법

- **mainloop()** 속에서 계속 실행

```python
root = Tk()	# root 프로그램 선언

root.mainloop()
```

<br>

**Label** : text 집어넣어 띄우기

- **.pack()** : 기본 위치에 띄우기
- **.config()** : 지정된 label의 설정을 변경

```python
#Label("어떤 프로그램에 넣을지", text="무슨 말을 쓸지")
label1 = Label(root, text="Hello world!")
label1.pack()
label1.config(text="recheck")
```

<br>

**Button** : 버튼 만들기

- **command** : 클릭시 실행할 함수

```python
button2 = Button(root, text="긁을 정보", command=webtoonList)
button2.pack()
```

<br>

객체를 **선언한순서**에 따라 위치 선정