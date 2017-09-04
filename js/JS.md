# Js

* js严格区分大小写

* 由于js设计缺陷，不要使用 == 比较，始终坚持使用 === 比较

* NaN这个特殊的Number与所有其他值都不相等，包括它自己。唯一能判断NaN的方法是通过isNaN()函数

* 浮点数在运算过程中会产生误差，因为计算机无法精确表示无限循环小数，要比较两个浮点数是否相等，只能计算他们之差的绝对值，看是否小于某个阈值：Math.abs(1/3 - (1 - 2/3)) < 0.0000001; // true

* null表示一个“空”值，undefined表示值未定义。大多数情况下，都应该使用null。undefined仅仅在判断函数参数是否传递的情况下有用

* 在代码的第一行写上**‘use strict’;** 表示启用strict模式（严格模式）。不支持strict模式的浏览器会把它当做一个字符串语句执行。在strict模式下运行的js代码，强制通过var申明变量，未使用var申明变量就使用的，将导致运行错误。建议所有的js代码都应该使用strict模式

* \`...\` 表示多行字符串（ES6新增）

* ES6新增了一种模板字符串：

  ```
  var name = '小明'；
  var age = 20;
  var message = `你好，${name}, 你今年${age}岁了！`;
  alert(message)
  ```

### 字符串

* toUpperCase()：把一个字符串全部变成大写

* toLowerCase()：把一个字符串全部变为小写

* indexOf()：搜索指定字符串出现的位置：

  ```
  var s = 'hello, world';
  s.indexOf('world'); //返回7
  s.indexOf('World'); // 没有找到指定的字符串，返回-1
  ```

* substring()：返回指定索引区间的字符串

  ```
  var s = 'hello, world';
  s.substring(0, 5); // 从索引0开始到5（不包括5），返回'hello';
  s.substring(7); // 从索引7开始到结束，返回'world';
  ```

### 数组

* indexOf()，与string类似，搜索一个指定元素的位置

* slice()：对应string的substring，它截取Array的部分元素，并返回一个新的Array:

  ```
  var arr = ['a', 'b', 'c', 'd', 'e'];
  arr.slice(0,3); // ['a', 'b', 'c']
  arr.slice(3); // 从索引3开始到结束：['d', 'e']
  ```

  如果不给slice()传递任何参数，它就会从头到尾截取所有元素。利用这一点，可以很容易的复制一个Array

* push() 向Array的末尾添加若干元素

* pop() 删除Array的最后一个元素

* unshift() 往Array的头部添加若干元素

* shift() 删除Array的第一个元素

* sort() 对当前Array进行排序

  ```
  var arr = ['B', 'C', 'A'];
  arr.sort();
  arr; // ['A', 'B', 'C']
  ```

* reverse() 反转Array的元素

* splice() 可以从指定的索引开始删除若干元素，然后再从该位置添加若干元素

  ```
  var arr = ['Microsoft', 'Apple', 'Yahoo', 'AOL', 'Excite', 'Oracle'];
  // 从索引2开始删除3个元素,然后再添加两个元素:
  arr.splice(2, 3, 'Google', 'Facebook'); // 返回删除的元素 ['Yahoo', 'AOL', 'Excite']
  arr; // ['Microsoft', 'Apple', 'Google', 'Facebook', 'Oracle']
  // 只删除,不添加:
  arr.splice(2, 2); // ['Google', 'Facebook']
  arr; // ['Microsoft', 'Apple', 'Oracle']
  // 只添加,不删除:
  arr.splice(2, 0, 'Google', 'Facebook'); // 返回[],因为没有删除任何元素
  arr; // ['Microsoft', 'Apple', 'Google', 'Facebook', 'Oracle']
  ```

* concat() 连接两个Array，并返回一个新的Array

  ```
  var arr = ['A', 'B', 'C'];
  var added = arr.concat([1, 2, 3]);
  added; // ['A', 'B', 'C', 1, 2, 3]
  arr; // ['A', 'B', 'C']
  ```

* join() 把当前Array的每个元素都用指定的字符串连接起来，然后返回连接后的字符串:(如果Array的元素不是字符串，将自动转换为字符串后再连接)

  ```
  var arr = ['a', 'b', 'a', 1, 2, 3];
  arr.join('-'); // 'a-b-c-1-2-3'
  ```



要判断一个属性是否是某对象自身拥有的，而不是继承得到的，可以用hasOwnProperty()方法

```
var xiaoming = {
  name: '小明'
};
xiaoming.hasOwnProperty("name"); // true
xiaoming.hasOwnProperty("toString"); // fasle

//判断一个属性是否存在
'toString' in xiaoming; // true
```



### Map

Map是一组键值对的结构，具有极快的查找速度

```
var m = new Map([['Michael', 95], ['Bob', 75], ['Tracy', 85]]);
m.get('Micheal'); // 95
```

初始化Map需要一个二维数组，或者直接初始化一个空Map:

```
var m = new Map();
m.set('Adam', 67); // 添加新的key-value
m.has('Adam'); // 是否存在key 'Adam' : true
m.get('Adam'); // 67
m.delete('Adam'); // 删除key 'Adam'
m.get('Adam'); // undefined
```

由于一个key只能对应一个value，所以，多次对一个key放入value，后面的值会把前面的值冲掉



### Set

Set和Map类似，也是一组key的集合，但不存储value。在Set中，没有重复的Key

```
var s1 = new Set(); // 空Set
var s2 = new Set([1, 2, 3,3]);
s; // Set {1, 2, 3} 重复元素在Set中自动被过滤
s.add(4); // 添加新元素
s.delete(3); // 删除元素
```



### **iterable** 类型

为了统一集合类型，ES6标准引入了新的**iterable** 类型，Array、Map和Set都属于iterable类型；具有iterable类型的集合可以通过新的**for...of** 循环来遍历

for...in循环由于历史遗留问题，它遍历的实际上是对象的属性名称。一个Array数组实际上也是一个对象，它的每个元素的索引被视为一个属性

```
var a = ['A', 'B', 'C'];
a.name = 'Hello';
for(var x in a){
  alert(x); // '0','1','2','name'
}
```

for...of只循环集合本身的元素：

```
var a = ['A', 'B', 'C'];
a.name = 'Hello';
for (var x of a){
  alert(x); // 'A', 'B', 'C'
}
```

**更好的方式** 是直接使用iterable内置的forEach方法，它接收一个函数，每次迭代就自动回调该函数

```
//Array
var a = ['A', 'B', 'C'];
a.forEach(function (element, index, array) {
  // element: 指向当前元素的值
  // index: 指向当前索引
  // array: 指向Array对象本身
})
//Set
var s = new Set(['A', 'B', 'C']);
s.forEach(function (element, set) {
  // set没有索引
})
//Map 
var m = new Map([[1, 'x'], [2, 'y'], [3, 'z']]);
m.forEach(function (value, key, map) {
    alert(value);
});
```



### 函数

避免函数接收到undefined，可以对参数进行检查：

```
function abs(x){
  if(typeof x !== 'number'){
    throw 'Not a number';
  }
  if(x > 0){
    return x;
  }esle{
    return -x;
  }
}
```

**arguments** ，它只在函数内部起作用，并且永远指向当前函数的调用者传入的所有参数（arguments最常用于判断传入参数的个数）

```
function foo(x) {
  alert(x); //10
  for (var i=0;i<arguments.length;i++){
    alert(arguments[i]); // 10,20,30
  }
}
foo(10, 20, 30)
```

**rest参数** ：获取除了已定义参数之外的参数

rest参数只能写在最后，前面用`...`标识，从运行结果可知

```
function foo(a, b, ...rest) {
  console.log('a=' + a);
  console.log('b=' + b);
  console.log(rest);
}
foo(1, 2, 3, 4)
// 结果:
// a = 1
// b = 2
// Array [ 3, 4, 5 ]
```

用**let**替代var可以申明一个块级作用域的变量

**const**定义常量。

全局变量会绑定到`window`上，不同的JavaScript文件如果使用了相同的全局变量，或者定义了相同名字的顶层函数，都会造成命名冲突，并且很难被发现。

减少冲突的一个方法是把自己的所有变量和函数全部绑定到一个全局变量中。例如：

```
// 唯一的全局变量MYAPP:
var MYAPP = {};

// 其他变量:
MYAPP.name = 'myapp';
MYAPP.version = 1.0;

// 其他函数:
MYAPP.foo = function () {
    return 'foo';
};
```

**方法**： 绑定到对象上的函数，在一个方法内部，this是一个特殊变量，它始终指向当前对象

要指定函数的this指向哪个对象，可以用函数本身的**apply方法**，它接收两个参数，第一个参数是需要绑定的this变量，第二个参数是Array，表示函数本身的参数

```
function getAge() {
  var y = new Date().getFullYear();
  return y - this.birth;
}
var xiaoming = {
  name: '小明',
  birth: 1990,
  age: getAge
};
xiaoming.age(); 
getAge.apply(xiaoming, []);
```

另一个与apply()类似的方法是call()，唯一的区别是：

* apply()把参数打包成Array再传入
* call() 把参数按顺序传入

比如调用Math.max(3, 5, 4)，分别用apply()和call()实现如下：

```
Math.max.apply(null, [3, 5, 4]);
Math.max.call(null, 3, 5, 4);
//对普通函数调用，通常把this绑定为null
```

利用apply()，还可以动态改变函数的行为

```
var count = 0;
var oldParseInt = parseInt; //保存原函数
window.parseInt = function() {
  count += 1;
  return oldParseInt.apply(null, arguments); //调用原函数
};
// 测试
parseInt('10');
parseInt('20');
parseInt('30');
count; // 3
```

### 高阶函数

javascript的函数其实都是指向某个变量。既然变量可以指向函数，函数的参数能接受变量，那么一个函数就可以接受另一个函数作为参数。这种函数就称之为高阶函数

```
function add(x, y, f) {
  return f(x) + f(y);
}
add(-5, 6, Math.abs);
```

```
把函数f(x) = x*x作用在一个数组[1,2,3,4,5,6]上

function pow(x){
  return x*x;
}
var arr = [1,2,3,4,5,6];
arr.map(pow); // [1, 4, 9, 16, 25, 36]

// 把Array的所有数组转成字符串
arr.map(String);
```

**reduce()**函数必须接收两个参数，reduce()把结果继续和序列的下一个元素做累积计算

```
[x1, x2, x3, x4].reduce(f) = f(f(f(x1, x2), x3), x4)

// 对Array求和
var arr = [1,3,5,7,9];
arr.renduce(function (x, y) {
  return x+y;
})
```

**filter()** 也是一个常用的操作，用于把Array的某些元素过滤掉，返回剩下的元素

```
// 在一个Array中，删掉偶数，只保留奇数
var arr = [1, 2, 3, 4, 5, 6];
var r = arr.filter(function(x) {
  return x%2 !== 0;
})
r; // [1, 3, 5]

// 去掉Array中的空字符串
var arr = ['A', '', 'B', null, undefined, 'C', ' '];
var r = arr.filter(function(a) {
  return a && a.trim(); // IE9以下的版本没有trim()方法
});
r; // ['A', 'B', 'C']

// filter() 接收的回调函数，其实可以有多个参数。通常仅使用第一个参数，表示Array的某个元素。回调函数还可以接受另外两个参数，表示元素的位置和数组本身
var arr = ['A', 'B', 'C'];
var r = arr.filter(function(element, index, self) {
  console.log(element); // 依次打印'A', 'B', 'C'
  console.log(index); // 依次打印 0， 1， 2
  console.log(self); // self就是变量arr
  return true;
})

// Array去重
var r,
    arr = ['apple', 'strawberry', 'banana', 'pear', 'apple', 'orange', 'orange', 'strawberry'];
    r = arr.filter(function (element, index, self) {
    return self.indexOf(element) === index;
});
```

**sort()**方法也是一个高阶函数，它还可以接受一个比较函数来实现自定义的排序

```
// 按数组大小排序
var arr = [10, 20, 1, 2];
arr.sort(function(x,y){
  if(x < y){
    return -1;
  }
  if(x > y) {
    return 1;
  }
  return 0;
}); // [1, 2, 10, 20]
```

**sort()方法会直接对Array进行修改，它返回的结果仍是当前Array**

```
var a1 = ['B', 'A', 'C'];
var a2 = a1.sort();
a1; // ['A', 'B', 'C']
a2; // ['A', 'B', 'C']
a1 === a2; // true, a1和a2是同一对象

```

### 闭包

```
function lazy_sum(arr) {
  var sum = function() {
    return arr.reduce(function(x,y) {
      return x+y;
    });
  }
  return sum;
}
```

当调用lazy_sum()时，返回的并不是求和结果，而是求和函数

```
var f = lazy_sum([1, 2, 3]); // function sum()
```

调用函数f时，才真正计算求和的结果

```
f(); // 6
```

在这个例子中，在函数lazy_sum中又定义了函数sum，并且内部函数sum可以引用外部函数lazy_sum的参数和局部变量，当lazy_sum返回函数sum时，相关参数和变量都保存在返回的函数中，这种称为“**闭包**”的程序结构拥有极大的威力。

**返回闭包时牢记的一点是：返回函数不要引用任何循环变量，或者后续会发生变化的变量**

如果一定要引用循环变量，方法是再创建一个函数，用该函数的参数绑定循环变量当前的值，无论该循环变量后续如何更改，已绑定到函数参数的值不变

```
function count() {
  var arr = [];
  for(var i=1; i<3; i++){
    arr.push((function(n) {
      return function() {
        return n * n;
      }
    })(i));
  }
  return arr;
}
var results = count();
var f1 = results[0];
var f2 = results[1];
var f3 = results[2];

f1(); // 1
f2(); // 4
f3(); // 9

//这里用了一个“立即执行的匿名函数”:
(function(x) {
  return x*x;
})(3); // 9
```

借助闭包，可以在没有class机制，只有函数的js里，封装一个私有变量

```
// 计数器
function create_counter(initial) {
  var x = initial || 0;
  return {
    inc: function() {
      x += 1;
      return x;
    }
  }
}

var c1 = create_counter(10);
c1.inc(); // 11
c1.inc(); // 12
```

在返回的对象中，实现了一个闭包，该闭包携带了局部变量x，并且，从外部代码根本无法访问到变量x。

**闭包就是携带状态的函数，并且它的状态可以完全对外隐藏起来**

闭包还可以把多参数的函数变成单参数的函数。例如，要计算x的y次方可以用`Math.pow(x, y)`函数，不过考虑到经常计算x的2次方或x的3次方，我们可以利用闭包创建新的函数`pow2`和`pow3`：

```
function make_pow(n) {
    return function (x) {
        return Math.pow(x, n);
    }
}

// 创建两个新函数:
var pow2 = make_pow(2);
var pow3 = make_pow(3);

pow2(5); // 25
pow3(7); // 343

```

**箭头函数** 相当于匿名函数，并且简化了函数定义。

```
(x, y, ...rest) => {
  var i, sum = x + y;
  for(i=0; i<rest.length; i++){
    sum += rest[i];
  }
  return sum;
}

// 返回一个对象
x => ({foo: x})
```

箭头函数和匿名函数有个明显的区别：箭头函数内部的this是词法作用域，由上下文确定；this总是指向词法作用域，也就是外层调用者obj

```
var obj = {
  birth: 1990,
  getAge: function () {
    var b = this.birth; // 1990
    var fn = () => new Date().getFullYear() - this.birth; // this指向obj对象
    return fn();
  }
};
obj.getAge();
```

由于this在箭头函数中已经按照词法作用域绑定了，所以，用call()或者apply()调用箭头函数时，无法对this进行绑定，即传入的第一个参数被忽略：

```
var obj = {
  birth: 1990,
  getAge: function(year) {
    var b = this.birth;
    var fn = (y) => y - this.birth;
    return fn.call({birth:2000}, year);
  }
};
obj.getAge(2015);
```

**generator（生成器）** 是ES6标准引入的新的数据类型，一个generator看上去像一个函数，但可以返回多次

**generator由function*定义，并且除了return语句，还可以用yield返回多次**

```
// 编写一个斐波那契数列： 0 1 1 2 3 5 8 13 21 34 ...

function* fib(max) {
  var t, a=0, b=1, n=1;
  while (n < max) {
    yield a;
    t = a+b;
    a = b;
    b = t;
    n ++;
  }
  return a;
}
// 直接调用一个generator和调用函数不一样，fib(5)仅仅是创建了一个generator对象，还没有去执行它。
```

调用generator对象有两个方法，一是不断调动generator对象的next()方法

```
var f = fib(3);
f.next(); // {value: 0, done: false}
f.next(); // {value: 1, done: false}
f.next(); // {value: 1, done: true}

//next()方法会执行generator的代码，然后，每次遇到yield x;就返回一个对象{value: x, done: true/false}，然后“暂停”。返回的value就是yield的返回值，done表示这个generator是否已经执行结束了。如果done为true，则value就是return的返回值。
```

第二个方法是直接用`for ... of`循环迭代generator对象，这种方式不需要我们自己判断`done`

```
for (var x of fib(5)) {
    console.log(x); // 依次输出0, 1, 1, 2, 3
}
```

generator还有另一个巨大的好处，就是把异步回调代码变成“同步”代码

```
// 用ajax
try {
  r1 = yield ajax('http://url-1', data1);
  r2 = yield ajax('http://url-2', data2);
  r3 = yield ajax('http://url-3', data3);
  success(r3);
}
catch(err) {
  handle(err);
}
```

### 面向对象

为了区分普通函数和构造函数，按照约定，构造函数首字母应当大写，而普通函数首字母应当小写

```
function Student(props) {
  this.name = props.name || '匿名';
  this.grade = props.grade || 1;
}
// 实例化的Student对象共享同一个函数
Student.prototype.hello = function() { 
  alert('Hello,' + this.name + '!');
}
function createStudent(props) {
  return new Student(props || {})
}

var xiaoming = createStudent({
  name: '小明'
})
xiaoming.grade; // 1
```

**原型继承**

```
// 可复用
function inherita(Child, Parent) {
  var F = function () {};
  F.prototype = Parent.prototype;
  Child.prototype = new F();
  Child.prototype.constructor = Child;
}

function Student(props) {
  this.name = props.name || 'Unnamed';
}
Student.prototype.hello = function () {
  alert('Hello, ' + this.name + '!');
}
function PrimaryStudent(props) {
  Student.call(this, props);
  this.grade = props.grade || 1;
}
// 实现原型继承链
inherits(PrimaryStudent, Student);
// 绑定其他方法到PrimaryStudent原型
PrimaryStudent.prototype.getGrade = function () {
  return this.grade;
}

// 创建小明
var xiaoming = new PrimaryStudent({
  name: '小明',
  grade: 2
});
xiaoming.name; // 小明
xiaoming.grade; // 2

//验证原型
xiaoming.__proto__ === PrimaryStudent.prototype; // true
xiaoming.__proto__.__proto__ === Student.prototype; // true

//验证继承关系
xiaoming instanceof PrimaryStudent; // true
xiaoming instanceof Student; // true
```

**class继承** 从ES6开始正式被引入到js中。class的目的就是让定义类更简单（现在不是所有主流浏览器都支持ES6的class，可以使用Babel工具把class代码转换为传统的prototype代码）

```
class Student {
  constructor(name) {
    this.name = name;
  }
  hello() {
    alert('Hello, ' + this.name + '!');
  }
}

var xiaoming = new Student('小明');
xiaoming.hello();
```

用calss定义对象的另一个巨大的好处是继承直接通过extends来实现：

```
class PrimaryStudent extends Student {
	constructor(name, grade) {
      super(name); // 用super调用父类的构造方法，否则父类的name属性无法正常初始化
      this.grade = grade;
	}
	myGrage() {
      alert('I am at grade ' + this.grade);
	}
  	...
}
```



### DOM

document.getElementById()可以直接定位唯一的一个DOM节点；

document.getElementsByTagName()和document.getElementsByClassName()总是返回一组DOM节点。

```
// 给文档添加新的css定义
var d = document.createElement('style');
d.setAttribute('type', 'text/css');
d.innerHTML = 'p {color: red}';
document.getElementsByTagName('head')[0].appendChild(d);
```

**parentElement.insertBefore(newElement, referenceElement)** 把子节点插入到指定的位置，子节点会插入到referenceElement之前

```
<!-- HTML结构 -->
<div id="list">
    <p id="java">Java</p>
    <p id="python">Python</p>
    <p id="scheme">Scheme</p>
</div>

var list = document.getElementById('list'),	
	ref = document.getElementById('python'),
	haskell = dcoument.createElement('p');
	
haskell.id = 'haskell';
haskell.innerText = 'Haskell';
list.insertBefore(haskell, ref);
```

### 表单

```
<form id="test-form" onsubmit="return checkForm()" >
	<input type="text" name="test">
	<button type="submit">Submit</button>
</form>
<script>
	function checkForm() {
      var form = document.getElementById('test-form');
      ...
      return true/false;
	}
	// reutrn true告诉浏览器继续提交，如果return false。浏览器将不会继续提交form
</script>
```

### File API

HTML5的File API提供了File 和 FileReader两个主要对象，可以获得文件信息并读取文件

```
// 通过HTML5的File API读取文件内容。以DataURL的形式读取到的文件是一个字符串，类似于data:image/jpeg;base64,/9j/4AAQSk...(base64编码)...，常用于设置图像。如果需要服务器端处理，把字符串base64,后面的字符发送给服务器并用Base64解码就可以得到原始文件的二进制内容。

var fileInput = document.getElementById('test-image-file'),
	info = document.getElementById('test-file-info'),
	preview = document.getElementById('test-image-preview');
// 监听change事件
fileInput.addEventListener('change', function () {
  // 消除背景图片
  preview.style.backgroundImage = '';
  // 检查文件是否选择
  if(!fileInput.value) {
    info.innerHTML = '没有选择文件';
    return;
  }
  // 获取File引用
  var file = fileInput.files[0];
  // 获取File信息
  info.innerHTML = '文件：' + file.name + '<br>' +
  					'大小' + file.size + '<br>' +
  					'修改' + file.lastModifiedData;
  if(file.type !== 'image/jpeg' && file.type !== 'image/png' && file.type !== 'image/gif') {
    alert('不是有效的图片文件！');
    return;
  }
  // 读取文件
  var reader = new FileReader();
  reader.onload = function(e) { // 当文件读取完成后，自动调用此函数:
    var data = e.target.result; // 'data:image/jpeg;base64,/9j/4AAQSk...(base64编码)...'  
    preview.style.backgroundImage = 'url(' + data + ')';
  };
  // 以DataURL的形式读取文件
  reader.readAsDataURL(file);
})
```

### Canvas

由于浏览器对HTML5标准支持不一致，所以通常在canvas内部添加一些说明HTML代码，如果浏览器支持canvas，它将忽略<canvas>内部的HTML，如果不支持，将显示HTML

```
var canvas = document.getElementById('test-shape-canvas');
var ctx = canvas.getContext('2d');
// 拿到一个CanvasRenderingContext2D对象，所有的绘图操作都需要通过这个对象完成
g1 = canvas.getContext('webg1'); // 绘制3D

ctx.clearReact(0, 0, 200, 200); // 擦除（0,0）位置大小为200*200的矩形，擦除的意思是把该区域变成透明
ctx.fillStyle = '#ddd'; // 设置颜色
ctx.fillRect(10, 10, 130, 130); // 把（10,10）位置大小为130*130的矩形涂色

// 利用Path绘制复杂路径
var path = new Path2D();
path.arc(75, 75, 50, 0, Math.PI*2, true);
path.moveTo(110,75);
path.arc(75, 75, 35, 0, Math.PI, false);
path.moveTo(65, 65);
path.arc(60, 65, 5, 0, Math.PI*2, true);
path.moveTo(95, 65);
path.arc(90, 65, 5, 0, Math.PI*2, true);
ctx.strokeStyle = '#0000ff';
ctx.stroke(path);
```

### jQuery

给jQuery对象绑定一个新方法是通过扩展$.fn对象实现的

```
$.fn.highlight = function (options) {
	// 合并默认值和用户设定值
	var opts = $.extend({}, $.fn.highlight.defaults, options);
    // this已绑定为当前jQuery对象:
    this.css('backgroundColor', opts.backgroundColor).css('color', opts.color);
  return this; // 使jQuery对象可以继续链式操作
}
// 设定默认值
$.fn.highlight.defaults = {
   color: '#d85030',
    backgroundColor: '#fff8de'
}
// 使用
$.fn.highlight.defaults.color = '#fff';
$.fn.highlight.defaults.backgroundColor = '#000';
$('#demo').highlight();
```

* 给$.fn绑定函数，实现插件的代码逻辑
* 插件函数最后要return this; 以支持链式调用
* 插件函数要有默认值，绑定在$.fn.<pluginName>.defaults上
* 用户在调用时可传入设定值以覆盖默认值

**针对特定元素的扩展**

可以借助filter()方法来实现针对特定元素的扩展

```
// 给所有指向外链的超链接加上跳转提示
$('#main a').external();

$.fn.external = function() {
  // return 返回的each()返回结果，支持链式调用：
  return this.filter('a').each(function () {
    // 注意 each()内部的回调函数的this绑定为DOM本身
    var a = ($this);
    var url = a.attr('href');
    if(url && (url.indexOf('http://') === 0 || url.indexOf('https://') === 0)){
      a.attr('href', '#0')
      	.removeAttr('target')
      	.append('<i class="uk-icon-external-link"></i>')
      	.click(function () {
          if(confirm('你确定要前往' + url +'?')) {
            window.open(url);
          }
      	})
    }
  })
}
```

### 错误

js有一个标准的Error对象表示错误，还有从Error派生的TypeError、ReferenceError等错误对象，在处理错误时，可以通过catch(e)捕获的变量e访问错误对象

```
try {
  ...
} catch(e) {
  if (e instanceof TypeError) {
    alert('Type error!');
  } else if(e instanceof Error) {
    alert(e.message);
  } else {
    alert('Error: ' + e);
  }
}
```

**抛出错误** 使用throw语句

```
var r, n, s;
try {
    s = prompt('请输入一个数字');
    n = parseInt(s);
    if (isNaN(n)) {
        throw new Error('输入错误');
    }
    // 计算平方:
    r = n * n;
    alert(n + ' * ' + n + ' = ' + r);
} catch (e) {
    alert('出错了：' + e);
}
```

如果在一个函数内部发生了错误，它自身没有捕获，错误就会被抛到外层调用函数，如果外层函数也没有捕获，该错误会一直沿着函数调用链向上抛出，直到js引擎捕获，代码终止执行





