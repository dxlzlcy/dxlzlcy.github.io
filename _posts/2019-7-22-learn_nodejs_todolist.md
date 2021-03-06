---
title: nodejs实现todolist
category: 其他
---


Nodejs+Express+Mongo实战TodoList(共17讲)

[视频地址](https://www.bilibili.com/video/av20196752?from=search&seid=1584195348370140715)

```bash
npm init -y
npm install express -save
npm install -g nodemon
```

新建一个`app.js`文件

```javascript
var express = require('express');

var app = express();

// .get .post .delete
app.get('/', (req, res) => {
  	// 可以返回json，列表，字符串
    res.send('hello world');
})

app.listen(3000)
console.log('listen to 3000')
```

[官方api链接](https://expressjs.com/en/4x/api.html)



路由参数

```javascript
app.get('/profile/:id', (req, res) => {
  	console.dir()
    res.send('the id is '+req.params.id);
})
```

也可以用正则表达式

```javascript
app.get('/ab?cd', (req, res) => {
  	console.dir()
    res.send('ab?cd';
})
```

类似这种query请求，可以这样取出

```javascript
// localhost:3000?name=lcy

app.get('/', (req, res) => {
    console.dir(req.query);
    res.send('homepage' + req.query.name);
})
```

post请求

```bash
npm install body-parser
```

可以用vscode插件REST Client来发送请求

```http
POST http://localhost:3000 HTTP/1.1
content-type: application/json

{
    "name":"Hendry",
    "salary":"61888",
    "age":"26"
}

### 普通get请求
http://localhost:3000?name=lcy
```

REST client还支持代码生成，通过 Generate Code Snippet 命令来把HTTP请求生成出不同编程语言的代码：JavaScript, Python, C, C#, Java, PHP, Go, Ruby, Swift等等主流语言。

示例，生成的python的requests库的代码

```python
import requests

url = "http://localhost:3000/"

payload = "{\"name\":\"Hendry\",\"salary\":\"61888\",\"age\":\"26\"}"
headers = {'content-type': 'application/json'}

response = requests.request("POST", url, data=payload, headers=headers)

print(response.text)
```

#### bodyParser可以处理表单内容和json内容

```javascript
var express = require('express');
var bodyParser = require('body-parser');
var fs = require('fs');

var app = express();
urlencodeParser = bodyParser.urlencoded({
    extended: false
})

jsonParser = bodyParser.json();

app.get('/', (req, res) => {
    console.dir(req.query);
    res.send('homepage');
})

app.get('/form', (req, res) => {
    var form = fs.readFileSync('./form.html', {
        encoding: 'utf8'
    })
    res.send(form);
})

app.post('/', urlencodeParser, (req, res) => {
    console.dir(req.body);
    res.send('helloworld');
})

app.post('/upload/', jsonParser, (req, res) => {
    console.dir(req.body);
    res.send('helloworld');
})



app.listen(3000)
console.log('listen to 3000')
```

#### `multer`处理文件上传的库

```javascript
var multer = require('multer');
var upload = multer({
    dest: 'uploads/'
})

app.get('/form', (req, res) => {
    var form = fs.readFileSync('./form.html', {
        encoding: 'utf8'
    })
    res.send(form);
    // res.sendfile(__dirname + '/form.html')

})

app.post('/upload/', upload.single('logo'), (req, res) => {
    // console.dir(req.body);
    res.send({
        'ret_code': 0
    });
})
```

对应的`form.html`

```html
    <form action="/upload" method="POST" enctype="multipart/form-data">
        <h2>单图上传</h2>
        <input type="file" name="logo">
        <input type="submit" value="提交">
    </form>
```

#### 模板引擎

例如`ejs`

创建一个`views`文件夹，里面放入`form.ejs`文件

```html
    <h2><%= person %></h2>
    <form action="/upload" method="POST" enctype="multipart/form-data">
        <h2>单图上传</h2>
        <input type="file" name="logo">
        <input type="submit" value="提交">
    </form>
```

修改app.js文件

```javascript
app.set('view engine', 'ejs');

app.get('/form/:name', (req, res) => {
    person = req.params.name;
    res.render('form', {
        person: person
    })
})

```

#### 中间件

```javascript
app.use((req, res, next) => {
    console.log('first middleware');
    next();
})

app.use((req, res, next) => {
    console.log('second middleware');
    res.send('hi');
})
```



静态文件，可通过`http://localhost:3000/1.png`访问

```javascript
app.use(express.static('static_files'));
```



#### 路由中间件

`routes/index.js`文件

```javascript
var express = require('express');

var router = express.Router();

router.get('/', function (req, res, next) {
    res.send('root');
})

module.exports = router;
```

`app.js`文件

```javascript
var express = require('express');
var indexRouter = require('./routes/index');
var usersRouter = require('./routes/users');

var app = express();

app.use('/', indexRouter)
app.use('/users', usersRouter)



app.listen(3000)
console.log('listen to 3000')
```










