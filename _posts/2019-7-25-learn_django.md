---
layout:post
title:学习django
---





### Django 2 零基础 - 待办清单网站

[视频地址](https://www.bilibili.com/video/av24293644/?p=6&t=192)



```shell
pip install django==2.0.5
django-admin startproject to_do_list
cd to_do_list
python manage.py runserver
python manage.py startapp todolist # add it in setting.py
```

创建文件夹`templates`，在里面新建一个`html`文件

强制覆盖css样式，不管优先级

```css
nav input {
  width:39% !inportant
}
```

