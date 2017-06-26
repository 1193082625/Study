# Mongoose

* **数据库安装：**

  官网：https://www.mongodb.com/ 下载安装包，直接运行，点击下一步选择安装路径即可。

* **通过命令行的方式启动服务端** 

  执行命令行窗口（win+R）-> cmd 

  cd MongoDB安装的目录\MongoDB\Server\版本号\bin

  MongoDB安装的目录\MongoDB\Server\版本号\bin>mongod --dbpath=项目所在目录下的db文件目录 --port=27018(设置数据库连接端口号，默认是27017)

* **连接数据库**

  可以使用MongoDB下的命令行来连接

  推荐使用可视化工具连接数据库： **Robomongo**   （第一次打开新建一个链接即可）

  ​

## 使用

1、在项目中安装mongoose模块 : npm install mongoose

2、在应用的入口（例如Node的app.js文件）加载数据库模块：var mongoose = require("mongoose");

3、链接数据库： 

```
mogoose.connect(‘协议://地址:端口/设置数据库名称’,回调函数);
//示例：
mogoose.connect(‘mongodb://localhost:27018/test’,function(err){
	if(err){
      console.log("数据库连接失败");
	}else{
      console.log("数据库连接成功");
      //一般情况下应用的监听可以放在这个位置
	}
});

```

4、用Schema模式管理Mongoose数据，一个Schema对象代表数据库中的一个表，Schema对象中的每一个属性，代表数据库中的每一个字段。

```
var mongoose = require('mongoose');
//新建一个kittySchema表，其中包含name字段为字符串类型
var kittySchema = new mongoose.Schema({
  name:String,//定义一个用户名字段，为字符串类型
})
//添加行为
kittySchema.method.sayHello = function(){
  console.log("hello, i am a kitty");
}
```

5、将模式编译成模型

```
var Kitten = mongoose.model('Kitten',kittySchema);
```

6、创建一条数据

```
var someone = new Kitten({
  name:'Slience'
});
someone.sayHello(); // "hello, i am a kitty"
```

7、保存数据

```
someone.save(function(err,someone){
  if(err) return console.error(err);
  fluffy.sayHello();
})
```

8、查询所有数据

```
Kitten.find(function(err,Kittens){
  if(err) return console.error(err);
  console.log(Kittens);
})
```

9、查询name为Slience的数据

```
Kitten.find({ name:'Slience' }，callback);
```



​	

​	