---
layout: post
title: 学习tkinter
category: 学习
---

{:toc}

学习tkinter开发入门以及一个小demo



`tkinter`是Python自带的一个gui库，对图像处理库tk的一个封装

其他gui库，`pythonwin`,`wxpython`,`pyQT`，



### 基本框架

```python
import tkinter as tk
window = tk.Tk()
window.title('my window')
window.geometry('200x200')

window.mainloop()
```



### 标签

```python
l = Label(window,text='hello world',bg='green',font=('Arial',12),width=15,height=2)
l.pack()
# l.place() 可以指定在哪个位置加载
l.config(text='something') #可以更改文本内容
```



### 按钮

```python
tk.Button(window, text="hit me", command=hit_me)
```



### 输入、文本框

```python
var = tk.StringVar()
var.set('hello')
tk.Entry(window,textvariable=var).pack()
tk.Entry(window,show='*').pack()
```



### 列表

```python
var1 = tk.StringVar()
tk.Label(window,bg='yellow',width=4,textvariable=var1_.pack())

def print_selection():
  value = lb.get(lb.curselection())
  var1.set(value)

var2 = tk.StringVar()
var2.set((11,22,33,44))
lb = tk.Listbox(window,listvariable=var2)

list_item = [1,2,3,4]
for item in list_item:
  lb.insert('end',item)
  
lb.insert(1,'first')
lb.insert(2,'second')
lb.delete(2)
```



### 选择按钮

```python
var = tk.StringVar()
l = tk.Label(window,bg='yellow',width=4,text='empty')
l.pack()

def print_selection():
  l.config(text='you have selected '+var.get())

tk.Radiobutton(window,text='Option A',variable=var,value='A',command=print_selection)
```





### 勾选项

```python
tk.Checkbutton(window,text='python',variable=var,onvalue=1,offvalue=0)
```





### Canvas画布

```python
canvas = tk.Canvas(window,bg='blue',height=100,width=200)
image_file = tk.PhotoImage(file='xxx.gif')
image = canvas.create_image(0,0,anchor='nw',image=image_file)

x0,y0,x1,y1 = 50,50,80,80
line = canvas.create_line(x0,y0,x1,y1)
oval = canvas.create_oval(x0,y0,x1,y1,fill='red')


canvas.pack()
```



### 菜单

```python
editmenu = tk.Menu(menubar,tearoff=0)
menubar.add_cascade(label='Edit',menu=editmenu)
editmenu.add_command(label='Cut',command=do_job)
editmenu.add_command(label='Copy',command=do_job)
editmenu.add_command(label='Paste',command=do_job)

submenu = tk.Menu(filemenu)
filemenu.add_cascade(label='Import',menu=submenu,underline=0)
submenu.add_command(label='Submenu1',command=do_job)

window.config(menu=menubar)
```



### 框架 Frame

```python
window = tk.Tk()
window.title('my window')
window.geometry('200x200')
tk.Label(window,text='on the window').pack()

frm = tk.Frame(window)
frm.pack()
frm_left = tk.Frame(frm)
frm_right = tk.Frame(frm)
frm_left.pack('left')
frm_right.pack('right')

tk.Label(frm_left,text='on the left').pack()
tk.Label(frm_left,text='on the left').pack()
tk.Label(frm_right,text='on the right').pack()
```



### 弹窗

```python
def hit_me():
    messagebox.showinfo(title="Hi", message="hahaha")
    messagebox.showwarning(title="Hi", message="hahaha")
    messagebox.showerror(title="Hi", message="hahaha")
    messagebox.askquestion(title="Hi", message="hahaha") # yes no
    messagebox.askyesno(title="Hi", message="hahaha") # True False


tk.Button(text="hit me", command=hit_me)
```



### 放置位置

```python
tk.Label(window,text=1).pack(side='left')


for i in range(4):
  for j in range(3):
    tk.Label(window,text=1).grid(row=i,column=j,padx=10,pady=10)
    

tk.Label(window,text=1).place(x=10,y=100,anchor='nw')
```



### 注册窗口

```python
def usr_sign_up():
  window_sign_up = tk.Toplevel(window)
  window_sign_up.geometry('200x200')
  window_sign_up.title('sign up window')
  window_sign_up.destroy() #不太确定有没写对
```



### 改变app图标

```python
import tkinter as tk

root = tk.Tk()
img = tk.Image("photo", file="icon.gif")
# root.iconphoto(True, img) # you may also want to try this.
root.tk.call('wm','iconphoto', root._w, img)

root.mainloop()
```



### 一个小demo

```python
import tkinter as tk
from tkinter import messagebox
import time
import pandas as pd


window = tk.Tk()
window.title("今天你干了啥")
window.geometry("1000x800")

img = tk.Image(
    "photo",
    file="/Users/liuchengyi/刘承翊/vscode-files/python_files/learn_tkinter/icon.png",
)
# root.iconphoto(True, img) # you may also want to try this.
window.tk.call("wm", "iconphoto", window._w, img)

edit_mode = 1
summary_dict = {
    "日期": time.strftime("%Y-%m-%d"),
    "游戏": 0,
    "学习时间": [],
    "总学习时间": 0,
    "健身": 0,
    "早睡": 0,
}

## 打了几把游戏
tk.Label(text="你今天打了几把游戏").place(x=20, y=20)
game_num = tk.IntVar()
tk.Entry(textvariable=game_num, width=2).place(x=150, y=20)


def game_num_plus():
    game_num.set(game_num.get() + 1)
    summary_dict["游戏"] += 1


tk.Button(text="+", fg="blue", command=game_num_plus).place(x=180, y=20)

# 是否健身

jianshen = tk.IntVar()
tk.Checkbutton(window, text="健身", variable=jianshen, onvalue=1, offvalue=0).place(
    x=240, y=20
)

# 是否早睡

zaoshui = tk.IntVar()
tk.Checkbutton(window, text="早睡", variable=zaoshui, onvalue=1, offvalue=0).place(
    x=320, y=20
)


## 学了多久时间
learn_time = tk.StringVar()
start_time = 0


def start_learn():
    global start_time
    if start_time:
        messagebox.showinfo(message="你已经开始学习")
    else:
        start_time = time.time()
        a = time.strftime("本次学习开始时间:%H时%M分%S秒")
        learn_time.set(a)


def end_learn():
    global start_time, learn_time
    a = time.time() - start_time
    a = int(a)
    summary_dict["总学习时间"] += a
    h = a // 3600
    a = a - h * 3600
    m = a // 60
    s = a - m * 60
    t = "%s时%s分%s秒" % (h, m, s)
    summary_dict["学习时间"].append([h, m, s])
    learn_time.set("本次学习时间:%s" % t)
    start_time = 0


tk.Button(text="开始学习", fg="blue", command=start_learn).place(x=20, y=60)
tk.Button(text="结束学习", fg="blue", command=end_learn).place(x=80, y=60)
tk.Entry(textvariable=learn_time, width=30).place(x=180, y=60)

### 今日概况
def count_total_learn_time():
    if not summary_dict["学习时间"]:
        summary_dict["总学习时间"] = 0
        return

    total_time = 0
    for (h, m, s) in summary_dict["学习时间"]:
        total_time += 3600 * h + 60 * m + s
    summary_dict["总学习时间"] = total_time


def hms(a):
    h = a // 3600
    a = a - h * 3600
    m = a // 60
    s = a - m * 60
    t = "%s时%s分%s秒" % (h, m, s)
    return t


def show_summary():
    global edit_mode
    edit_mode = 1
    summary_dict["健身"] = jianshen.get()
    summary_dict["早睡"] = zaoshui.get()
    # today_summary.set(str(summary_dict))
    txt = ""
    for k, v in summary_dict.items():
        if k == "总学习时间":
            txt += "%s:%s\n" % (k, hms(v))
        elif k == "学习时间":
            continue
        else:
            txt += "%s:%s\n" % (k, v)
    today_summary.set(txt)


def edit():
    global summary_dict
    today_summary.set(str(summary_dict))


def finish():
    global summary_dict
    summary_dict = eval(today_summary.get())
    count_total_learn_time()
    # messagebox.showinfo(message="修改成功")
    show_summary()


today_summary = tk.StringVar()
tk.Entry(textvariable=today_summary).place(x=20, y=100, width=600)
tk.Button(text="编辑", command=edit).place(x=700, y=100)
tk.Button(text="完成", command=finish).place(x=740, y=100)
tk.Button(text="show", command=show_summary).place(x=20, y=130)


### 保存
def save_today():
    # messagebox.showinfo(message=summary_dict)
    a = today_summary.get()
    if not a:
        messagebox.showinfo(message="请先点击show")
        return

    b = summary_dict.copy()
    b["学习时间"] = str(b["学习时间"])
    try:
        a = pd.read_csv("record.csv")
        ind = len(a)
        df = pd.DataFrame(b, index=[ind])
        a = a[a.日期 != b["日期"]]
        df = pd.concat([a, df], sort=False)
    except:
        df = pd.DataFrame(b, index=[0])

    df.to_csv("record.csv", index=False)
    df.to_excel("record.xls", index=False)
    messagebox.showinfo(message="保存成功")


tk.Button(text="save", command=save_today).place(x=80, y=130)


window.mainloop()


```

