---
title: 学习jekyll搭建github page
category: 学习
---

### 利用`jekyll-now`搭建github page

首先fork主页[jekyll-now](https://github.com/barryclark/jekyll-now)，然后创建名为`username.github.io`的仓库

`clone`到本地，然后可以往里边加md文件，命名为`%Y-%M-%D-filename.md`，如果是未来的日期，则在主页上显示不了，开头为

```markdown
---
layout: post
title: # 想在主页上显示的标题
---
```

可以在本地预览博客主页，满意后再`git push`到GitHub

```bash
gem install github-pages

jekyll serve 
# 在 http://127.0.0.1:4000/ 可以预览效果

# 觉得满意之后
git add -A
git commit -m "aaa"
git push origin master 
```

利用jekyll搭建个人博客

```bash
sudo gem install jekyll
sudo gem install jekyll bundler

jekyll new myblog
cd myblog 
jekyll serve
```



#### frant matter

date和title会覆盖从文件名中提取的信息，将md文件保存在`_post`文件里面就好，`_post`文件夹里可以创建很多文件夹

```md
---
layout: post
title:  "hi!"
date:   2019-07-25 12:42:33 +0800
categories: test1
author: lcy
---
```



#### draft

可以创建一个`_drafts`文件夹，里面的内容只有运行在`jekyll serve --draft`时才会显示



#### page

例如，在根目录下创建`donate.md`文件

```md
---
layout: page
title: donation
permalink: /donate-me/
---

you can donate me via xxxx@xx.com
```



#### permalink

```md
---
layout: post
title:  hey
date:   2019-07-20
categories: test1 cat2
author: lcy
permalink: /:categories/:year/:month/:day/:title #或者任意填写
---
```



#### frant matter default

```yml
# 修改_config.yml

defaults: 
  -
    scope:
      path: ''
      type: posts # 应用于`_posts`文件下的所有内容
    values:
      layout: post
      title: 没想好名字
```



#### themes

[搜索jekyll theme](https://rubygems.org/search?utf8=✓&query=jekyll+theme)

Gemfile文件，添加

```
gem "minima", "~> 2.0"
gem "jekyll-theme-primer"
```

修改`_config.yml`中的theme

然后执行

```
bundle install
bundle exec jekyll serve
```

 

如果报错，需要修改layout，可能没有post,page,home这些layout



#### layout

创建`_layouts`文件夹

 `post.html`文件

```

---
layout: wrapper
author: lcy
---


<h1>{ {page.title}}</h1>         # {和{间多加了一个空格
<h2>{ {layout.author}}</h2>
<hr>

"{ { content }} "


```

wrapper.html，site文件配置在`_config.yml`中

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>{{site.title}}</title>
</head>

<body>
    wrapper <br>
    { % include header.html color="blue" % }
    { {content}}
    <br> wrapper

</body>

</html>
```

`/_includes/header.html`

```
<h1 style="color: { { include.color }}">{ { site.title }}</h1>
<hr>
```

`_layout/home.html`

```
{ % for post in site.posts %}
<li><a href="{ { post.url }} ">{ { post.title }}</a></li>
{ % endfor %}
```



#### if语句

mypost.html

```
---
layout: wrapper
author: lcy
---


{ % for post in site.posts %}
<li><a style="{ % if post.title==page.title%}color:red{ % endif %}" href="{ { post.url }} ">{ { post.title }}</a></li>
{ % endfor %}


{ % if page.title == "bbb" and/or %}
bbbbb
{ % elsif page.title == "hey" %}
heyhey
{ % else %}
nothing
{ % endif %}

<h1>{ {page.title}}</h1>
<h2>{ {layout.author}}</h2>
<hr>

{ { content }}
```



#### database

`/_data/people.yml`

```
- name: "lcy"
  age: 24

- name: "yzx"
  age: 28
```

`home.html`

```
{ % for person in site.data.people %}
{ {person.name}},{ {person.age}} <br>
{ % endfor %}
```



#### static files

```
{ % for file in site.static_files %}
{ {file.path}} <br>
{ % endfor %}
```

可以设置`_config.yml`将某个文件夹的图片都显示出来，[详情请看](https://www.youtube.com/watch?v=fqFjuX4VZmU&list=PLLAZ4kZ9dFpOPV5C5Ay0pHaa0RJFhcmcB&index=18)














