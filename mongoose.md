

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

// 构造函数
// 新建一个kittySchema表，其中包含name字段为字符串类型
var kittySchema = new mongoose.Schema({
  name:String,//定义一个用户名字段，为字符串类型
})

// 添加属性
kittySchema.add( { email: 'String', age: 'Number' } )

// 有时候Schema不仅要为后面的Model和Entity提供公共的属性，还要提供公共的方法
Schema.method( 'say', function(){console.log('hello');} ) 
// 这样Model和实例就能使用这个方法了

// 添加静态方法
Schema.static( ‘say’, function(){console.log(‘hello’);} ) 
// 静态方法，只限于在Model层就能使用

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

10、查询所有数据并根据id倒序，取其中5条数据，并跳过前4条数据

```
Kitten.find().sort({_id: -1}).limit(5).skip(4)
```

11、修改id是123456的数据，把name改成张三

```
 Kitten.update({
 	_id: '123456'
 }, {
 	name: '张三'
 })
 
 Kitten.findOne({
    _id: '123456'
  }).then((Kitten) => {
    if (Kitten) {
       return Category.update({
        _id: id
      }, {
        name: '张三'
      })
    }
  }).then(() => {
   console.log('修改成功！')
  }).catch((err) => {
    console.log(err)
  })
```

12、删除id等于123456的数据

```
Kitten.remove({
    _id: '123456'
})
```

13、查询数据并分页

```
router.get('/Kitten', (req, res) => {
  var page = Number(req.query.page || 1)
  var limit = 2
  var pages = 0

  Kitten.count().then((count) => {
    // 获取总页数
    pages = Math.ceil(count / data.limit)
    // 取值不能超过pages
    page = Math.min(page, pages)
    // 取值不能小于1
    page = Math.max(page, 1)

    var skip = (page - 1) * data.limit
    /*
     *sort 排序
     * 1:升序
     * -1: 降序
     *
     * populate 连表查询
     * */
    Kitten.find().sort({addTime: -1}).limit(limit).skip(skip).populate(['category', 'user']).then((articles) => {
      res.render('article/index', {
        userInfo: req.userInfo,
        articles: articles,

        pageName: 'article',
        page: page,
        count: count,
        pages: pages,
        limit: data.limit
      })
    })
  }).catch((err) => {
    console.log(err)
  })
})
```

14、取当前数据的上一条数据和下一条数

```
var mongoose = require('mongoose')

//上一条
Kitten.find({
    _id: {'$lt': mongoose.Types.ObjectId(当前ID)}
}).limit(1).then((Kittens) => {
  // 获取到的Kittens是一个包含一条数据的数组
  上一条数据的ID是 = Kittens[0]._id
})

//下一条
Kitten.find({
    _id: {'$gt': mongoose.Types.ObjectId(当前ID)}
}).limit(1).then((Kittens) => {
  // 获取到的Kittens是一个包含一条数据的数组
  上一条数据的ID是 = Kittens[0]._id
})
```

15、查找符合 id等于123456 并且 名称不是张三 的所有数据

```
var where = {}
where._id = '123456'
where.name = {'$ne': '张三'}

Article.where(where).find()
```



## 修改器和更新器

**更新修改器**

1、‘$inc’ 增减修改器,只对数字有效.下面的实例: 找到 age=22的文档,修改文档的age值自增1

```
Model.update({‘age’:22}, {’$inc’:{‘age’:1} } ); 
执行后: age=23
```

2、‘$set’ 指定一个键的值,这个键不存在就创建它.可以是任何MondoDB支持的类型.

```
Model.update({‘age’:22}, {’$set’:{‘age’:‘haha’} } ); 
执行后: age=‘haha’
```

3、‘$unset’ 同上取反,删除一个键

```
Model.update({‘age’:22}, {’$unset’:{‘age’:‘haha’} } ); 
执行后: age键不存在
```



#### **数组修改器:**

1、‘$push’ 给一个键push一个数组成员,键不存在会创建

```
Model.update({‘age’:22}, {’$push’:{‘array’:10} } ); 
执行后: 增加一个 array 键,类型为数组, 有一个成员 10
```

2、‘$addToSet’ 向数组中添加一个元素,如果存在就不添加

```
Model.update({‘age’:22}, {’$addToSet’:{‘array’:10} } ); 
执行后: array中有10所以不会添加
```

3、‘$each’ 遍历数组, 和 $push 修改器配合可以插入多个值

```
Model.update({‘age’:22}, {’$push’:{‘array’:{’$each’: [1,2,3,4,5]}} } ); 
执行后: array : [10,1,2,3,4,5]
```

4、‘$pop’ 向数组中尾部删除一个元素

```
Model.update({‘age’:22}, {’$pop’:{‘array’:1} } ); 
执行后: array : [10,1,2,3,4] tips: 将1改成-1可以删除数组首部元素
```

5、‘$pull’ 向数组中删除指定元素

```
Model.update({‘age’:22}, {’$pull’:{‘array’:10} } );
执行后: array : [1,2,3,4] 匹配到array中的10后将其删除
```



#### **条件查询:**

- “$lt”	        小于
- “$lte”	小于等于
- “$gt”	大于
- “$gte”	大于等于
- “$ne”	不等于

```
Model.find({“age”:{ “$get”:18 , “$lte”:30 } } ); 
查询 age 大于等于18并小于等于30的文档
```



#### **或查询 OR:**

- ‘$in’    一个键对应多个值
- ‘$nin’   同上取反,  一个键不对应指定值
- “$or”    多个条件匹配, 可以嵌套 $in 使用
- “$not”	同上取反, 查询与特定模式不匹配的文档​	

```
Model.find({“age”:{ “$in”:[20,21,22.‘haha’]} } ); 
查询 age等于20或21或21或’haha’的文档
```

```
Model.find({"$or" : [ {‘age’:18} , {‘name’:‘xueyou’} ] }); 
查询 age等于18 或 name等于’xueyou’ 的文档
```

#### 

#### **类型查询:**

null 能匹配自身和不存在的值, 想要匹配键的值 为null, 就要通过  “$exists” 条件判定键值已经存在
"$exists" (表示是否存在的意思)

```
Model.find(“age” : { “$in” : [null] , “exists” : true } ); 
查询 age值为null的文档
```

```
Model.find({name: {$exists: true}},function(error,docs){
  //查询所有存在name属性的文档
});
```

```
Model.find({telephone: {$exists: false}},function(error,docs){
  //查询所有不存在telephone属性的文档
});
```



#### **正则表达式:**

MongoDb 使用 Prel兼容的正则表达式库来匹配正则表达式

```
find( {“name” : /joe/i } ) 
查询name为 joe 的文档, 并忽略大小写
```

```
find( {“name” : /joe?/i } ) 
查询匹配各种大小写组合
```



#### **查询数组:**

```
Model.find({“array”:10} );
查询 array(数组类型)键中有10的文档,  array : [1,2,3,4,5,10]  会匹配到
```

```
Model.find({“array[5]”:10} );
查询 array(数组类型)键中下标5对应的值是10,  array : [1,2,3,4,5,10]  会匹配到
```

```
// ‘$all’ 匹配数组中多个元素
Model.find({“array”:[5,10]} );
查询 匹配array数组中 既有5又有10的文档
```

‘$size’ 匹配数组长度

```
Model.find({“array”:{"$size" : 3} } );
查询 匹配array数组长度为3 的文档
```

‘$slice’ 查询子集合返回

```
Model.find({“array”:{"$skice" : 10} } );
查询 匹配array数组的前10个元素

Model.find({“array”:{"$skice" : [5,10] } } );
查询 匹配array数组的第5个到第10个元素
```



#### **where**

用它可以执行任意javacript语句作为查询的一部分,如果回调函数返回 true 文档就作为结果的一部分返回

```
find( {"$where" : function(){
      for( var x in this ){
          //这个函数中的 this 就是文档
      }

      if(this.x !== null && this.y !== null){
          return this.x + this.y === 10 ? true : false;
      }else{
       return true;
      }
	}  
})
```



#### **游标:**

- limit(3)	限制返回结果的数量,
- skip(3)	    跳过前3个文档,返回其余的
- sort( {“username”:1 , “age”:-1 } )	    排序 键对应文档的键名, 值代表排序方向, 1 升序, -1降序