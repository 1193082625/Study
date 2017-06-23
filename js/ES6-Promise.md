# 2017-6-12学习Promise对象

​	2015年6月，ECMAScript6发布了。其中ES6提供了Promise对象，中文意思是“承诺”。

​	**有了Promise对象，就可以将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调函数。** 即在then方法中继续写Promise对象并返回，然后继续调用then来进行回调操作。如果某些事件不断的反复发生，一般来说，使用**Stream** 模式是比部署Promise更好的选择。

![输出promise](http://images2015.cnblogs.com/blog/520134/201603/520134-20160311003722741-755677508.png)



​	ES6规定，Promise对象是一个构造函数，用来生成Promise实例。

​	所谓Promise，简单说就是一个容器，用来传递异步操作的消息。它代表了某个未来才会知道结果的事件（通常是一个异步操作）

​	Promise有三种状态：Pending（进行中）、Resolved（已完成，又称Fulfilled）和Rejected（已失败）。只有异步操作的结果，可以决定当前是哪一种状态。

​	resolve函数的作用是，将Promise对象的状态从“Pending”变为“成功”，在异步操作成功时调用，并将异步操作的结果，作为参数传递出去；reject函数的作用是，将Promise对象的状态从“Pending”变成“失败”，在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。

​	Promise的构造函数接收一个参数，是函数，并且传入两个参数：resolve和reject，分别表示异步操作执行成功后的回调函数和异步操作执行失败后的回调函数

​	**1、then方法** 可以接收两个回调函数作为参数，第一个对应resolve的回调，第二个对应reject的回调。所以能够分别拿到他们传过来的数据。其中，第二个参数是可选的。

```
	function getNumber(){
      var p = new Promise(function(resolve,reject){
        //做一些异步操作
        setTimeout(function(){
            var num = Math.ceil(Math.random()*10);//生成1-10的随机数
            if(num < 5){//成功
            	reolve(num);
            }else{//失败
            	reject('数字太大了');
            }
         },2000);
      });
      return p;
	}
	getNumber()
	.then(function(data){//成功回调
      console.log('resolved');
      console.log(data);
	},function(reason,data){//失败回调
      console.log('rejected');
      console.log(reason);
	})
```

​	

​	从表面上看，Promise只是能够简化层层回调的写法，而实质上，Promise的精髓是“状态”，用维护状态、传递状态的方式来使得回调函数能够及时调用。它比传递callback函数要简单、灵活的多。

​	**在then方法中，可以直接return数据而不是Promise对象，在后面的then中就可以接受到数据了**

​	**2、Promise.prototype.catch()** 是 .then(null,rejection)的别名，用于指定发生错误时的回调函数 

```
getNumber()
.then(function(data){//成功回调
  console.log('resolved');
  console.log(data);
})
.catch(function(reason){//失败回调
  console.log('rejected');
  console.log(reason);
})
```

**在执行resolve的回调时，如果抛出异常了（代码出错了），不会卡死js，而是会进到这个catch方法中。一般来说，不要在then方法里面定义reject状态的回调函数（即then的第二个参数），总是使用catch方法。** 这与try/catch语句有相同的功能 。

**3、Promise.all方法** 提供了并行执行异步操作的能力，并且在所有异步操作执行完成后才执行回调。all会把所有异步操作的结果放进一个数组中传给then（即下面的results）。

```
var p = Promise.all([fun1(),fun2(),fun3()]);
p.then(function(results){
	console.log(results);
}).catch(function(reason){
  console.log(reason);
})
```

只要fun1、fun2、fun3之中有一个被rejected，p的状态就会变成rejected，此时第一个被rejected的实例的返回值，会传递给p的回调函数

**如果作为参数的Promise实例定义了自己的catch方法，那么它一旦被rejected，并不会触发Promise.all()的catch方法**

**4、Promise.race()** ，race是赛跑的意思，即谁跑的快，以谁为准执行回调。比如可以用race给某个请求设置请求超时时间，并且在超时后执行相应的操作：

```
//请求某个图片资源
function requestImg(){
  var p = new Promise(function(resolve,reject){
    var img = new Image();
    img.onload = function(){
      resove(img);
    }
    img.src = 'xxxx';
  });
  return p;
}
//延时函数，用于给请求计时
function timeout(){
  var p = new Promise(function(resolve,reject){
    setTimeout(function(){
      reject('图片请求超时');
    },5000);
  });
  return p;
}

//把这两个返回Promise对象的函数放进race，于是他俩就会赛拍，如果5秒之内图片请求成功了，那么进入then方法，执行正常的流程。如果5秒钟图片还未成功返回，那么timeout就跑赢了，进入catch，报出“图片请求超时”的信息
Promise
.race([requestImg(),timeout()])
.then(function(results){
  console.log(results);
})
.catch(function(reason){
  console.log(reason);
})
```





