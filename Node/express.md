# Express

**npm init** ：为应用创建一个package.json文件。

**npm install --save express** ： 安装Express并将其保存到依赖列表中

**npm install express** ：只是临时安装Express，不添加到依赖列表中

**npm install express-generator -g** ：通过应用生成器工具express快速创建一个应用的骨架



**Hello World实例**

创建app.js：

```
var express = require('express');
var app = express();

app.get('/',function(req,res,next){
  res.send('Hello World');
})

app.listen(3000);
```

运行app.js ：右击运行，或者 node app.js



**路由**

```
//对网站首页的访问返回'hello world'
app.get('/',function(req,res,next){
  res.send("hello world");
})

//网页首页接收 POST请求
app.post('/',function(req,res,next){
  res.send('got a post request');
})

//匹配acd 和 abcd
app.get('ab?cd',function(req,res){
  res.send('ab?cd');
})

//匹配abcd,abbcd,abbbcd等
app.get('/ab+cd',function(req,res){
  res.send('ab+cd');
});

//匹配abcd,abxcd,abRABDOMcd,ab123cd等
app.get('/ab*cd',function(req,res){
  res.send('ab*cd');
});

//匹配abe和abcde
app.get('/ab(cd)e',function(req,res){
  res.send('ab(cd)?e');
});

路径可以使用正则表达式匹配
```

**使用多个回调函数处理路由（记得指定next对象）**

```
app.get('/example/b',function(req,res,next){
  console.log('1111');
  next();
},function(req,res){
  res.send('Hello')
})
```



**为静态资源目录指定一个挂载路径**

```
app.use('/static',express.static('public'));
//使用带有‘/static’前缀的地址访问public目录下的文件
```

