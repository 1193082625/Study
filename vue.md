# vue.js

引入外链VUE：

```
<script src="https://unpkg.com/vue/dist/vue.js"></script>
```

引入外链axios（http请求）:

*axios是一个基于Promise用于浏览器和nodejs的HTTP客户端*

```
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
```

_.debounce是一个通过lodash限制操作频率的函数：

```
<script src="https://unpkg.com/lodash@4.13.1/lodash.min.js"></script>
```



示例：

```
<div id="app">
	//绑定内容 直接用 {{}}
	//v-bind: 绑定属性缩写为 ：    
	//v-on: 监听DOM事件缩写为  @
	<a :href="dataUrl" @click="clickMethod">
		{{message}}	<br>
		"数据message是否成功获取："{{message?'yes':'no'}}
	</a>
	//根据条件切换class
	//当v-bind:style使用需要特定前缀的css属性时，如transform，vue.js会自动侦测并添加相应的前缀
	<div v-bind:class="[{active:isActive},errorClass]"></div>
	//从2.3开始可以为style绑定中的属性提供一个包含多个值的数组，常用于提供多个带前缀的值
	<div :style="{display:['-webkit-box','-ms-flexbox','flex']}"></div>
	//v-model 为 数据的双向绑定
	<input v-model="message">
	//执行一次性的插值：
	<span v-once>This will never change: {{msg}}</span>
	//解析Html
	<div v-html="message"></div>
	//过滤器
	<div>{{message | reverse}}</div>
	//使用计算属性
	<span>{{reverseMessage}}</span>
	//当想要在数据变化响应时，执行异步操作或开销较大的操作，可以用watch
	<input v-model="question">
	<span>{{answer}}</span>
</div>
var app = new Vue({
  el:'#app',
  data:{
    dataUrl:'www.baidu.com',
    message:'hello world',
    question:'',
    answer:'i cannot give you an answer until you ask a question!'
  },
  created:function(){
    //created这个钩子在实例被创建之后被调用
    console.log("this instance has been created");
  },
  methods:{//事件触发都写在methods
    clickMethod:function(){
      console.log("i am click!");
    },
    //_.debounce是一个通过lodash限制操作频率的函数
    //在这个例子中，希望限制访问yesno.wtf/api的频率
    //ajax请求直到用户输入完毕才会发出
    getAnswer:_.debounce(
    	function(){
      		var vm = this;
      		if(this.question.indexf('?') === -1){
              vm.answer = 'Questions usually contain a question mark. ;-)';
              return
      		}
      		vm.answer = 'Thinking...';
      		axios.get('https://yesno.wtf/api')
      		.then(function(response){
              vm.answer = _.capitalize(response.data.answer);
      		})
      		.catch(function(error){
              vm.answer = 'Error! Could not reach the API. ' + error;
      		})
    	},
    	//为用户停止输入等待的毫秒数
    	500
    )
  },
  filters:{//过滤器
    reverse:function(value){
      if(!value) return;
      return value.split('').reverse().join('');
    }
  },
  computed:{//计算属性。计算属性只有在它的相关依赖发生改变时才会重新求值。如果不希望有缓存，用method代替
    reverseMessage:function(){
      return this.message.split('').reverse().join('');
    }
  },
  watch:{//监听
    question:function(newQuestion){
      this.answer = 'you question has changed:'+newQuestion;
      this.getAnswer();
    }
  }
})
```

**条件渲染**

例1：

```
<h1 v-if="type === 'A'">A</h1>
<h1 v-else-if="type === 'B'">B</h1>
<h1 v-else>NO A/B</h1>
```

例2：

*最终的渲染结果不会包含<template>元素* 

```
<template v-if="ok">
	<h1>Title</h1>
	<p>Paragraph 1</p>
	<p>Paragraph 2</p>
</template>
```

例3：

*在下面的代码中切换 `loginType` 将不会清除用户已经输入的内容。因为两个模版使用了相同的元素，`<input>` 不会被替换掉——仅仅是替换了它的 `placeholder`* 

```
<template v-if="loginType === 'username'">
  <label>Username</label>
  <input placeholder="Enter your username">
</template>
<template v-else>
  <label>Email</label>
  <input placeholder="Enter your email address">
</template>
```

例4：

*每次切换时，输入框都将被重新渲染（即这两个元素是完全独立的——不要复用它们）*

```
<template v-if="loginType === 'username'">
	<label>Username</label>
	<input placeholder="Enter your username" key="username-input">
</template>
<template v-else>
	<label>Email</label>
	<input placeholder="Enter your email address" key="email-input">
</template>
```

**列表渲染**

例1、

```
<ul id="example-1">
	//普通示例
  <li v-for="item in items">
      {{item.message}}
  </li>
  <br>
  //添加索引
  <li v-for="(item,index) in items">
  	{{index}} - {{item.message}}
  </li>
  //用of替代in 作为分隔符，因为它是最接近javascript迭代器的语法
  <div v-for="item of items"></div>
  //使用<template>标签来渲染，同v-if的template使用一样
  <template v-for="item in items">
  	<li>{{item.message}}</li>
  </template>
</ul>
var example = new Vue({
  el:"#example-1",
  data:{
    items:[
      {message:'Foo'},
      {message:'Bar'}
    ]
  }
})
```

例2、

_迭代对象的属性_

```
<ul id="repeat-object" class="demo">
	<li v-for="value in object">
		{{value}}
	</li>
	//添加键名
	<li v-for="(value,key) in object">
		{{key}} : {{value}}
	</li>
	//添加索引
	<li v-for="(value,key,index) in object">
		{{index}}、 {{key}} : {{value}}
	</li>
</ul>
new Vue({
  el:"#repeat-object",
  data:{
    object:{
      FirstName:'John',
      LastName:'Doe',
      Age:30
    }
  }
})
```

例3、

*证书迭代，在这种情况下，它将重复多次模板*

```
<span v-for="n in 10">{{n}}</span>
```

**当v-if和v-for处于同一节点，v-for的优先级比v-if更高。**

例：遍历节点时，只渲染一些符合条件的节点：

```
<li v-for="todo in todos" v-if="!todo.isComplate">
	{{todo}}
</li>
```

例：如果是有条件的跳过循环的执行：

```
<ul v-if="shouldRenderTodos">
	<li v-for="todo in todos">
		{{todo}}
	</li>
</ul>
```

**利用索引直接设置某个项：**

```
Vue.set(explate1.items,index,newValue);
```

**修改数组的长度**

```
explate1.items.splice(newLength);
```

**在内联语句处理器中访问原生DOM**

```
<button v-on:click="warn($event)">submit</button>

methods:{
  warn:function(event){
    if(event){
      event.preventDefault();
      alert("hello");
    }
  }
}
```

**事件修饰符**

阻止单击事件冒泡

```
<a v-on:click.stop="doThis"></a>
```

提交事件不再重载页面

```
<form v-on:submit.prevent="onSubmit"></form>
```

修饰符可以串联

```
<a v-on:click.stop.prevent="doThat"></a>
```

只有修饰符

```
<form v-on:submit.prevent></form>
```

添加事件侦听器时使用事件捕获模式

```
<div v-on:click.capture="doThis"></div>
```

只有当事件在该元素本身（而不是子元素）触发时触发回调

```
<div v-on:click.self="doThat"></div>
```

点击事件将只会触发一次

```&lt;/a&gt;
<a v-on:click.once="doThis"></a>
```

## 组件

全局组件：

```
<div id="example">
	<my-component></my-component>
</div>

//注册
Vue.component('my-component',{
  template: '<div>A custom component!</div>'
})
//创建根实例
new Vue({
  el:'#example'
})
```

局部组件：

```
var Child = {
  template:'<div>A custom component</div>'
}
new Vue({
  //...
  components:{
    //<my-component>将只在父模板可用
    'my-component':Child
  }
})
```



```
Vue.component('simple-counter',{
  template:'<button v-on:click="counter += 1">{{counter}}</button>',
  data:function(){
    return {
      counter:0
    }
  }
})
new Vue({
  el:'#example-2'
})
```

**使用Prop传递数据**

```
Vue.component('child',{
  //声明props
  props:['message'],
  template:'<span>{{message}}</span>'
})
//向组件传递数据
<child message="hello!"></child>
```

动态Prop

```
<div>
	<input v-model="parentMsg">
	<br>
	<child v-bind:my-message="parentMsg"></child>
</div>
Vue.component('child',{
  props:['myMessage'],
  template:'<span>{{myMessage}}</span>'
})
```

*prop是单向绑定的，当父组件的属性变化时，将传导给子组件，但是不会反过来*

**定义一个局部变量，并用prop的值初始化它**

```
props:['initialCounter'],
data:function(){
  return {counter:this.initalCounter}
}
```

**定义一个计算属性，处理prop的值并返回**

```
props:['size'],
computed:{
  normailzedSize:function(){
    return this.size.trim().toLowerCase()
  }
}
```

每个Vue实例都实现了事件接口，即：

* 使用$on（eventName）监听事件
* 使用$emit(eventName)触发事件

**非父子组件通信** 在简单的场景下，可以使用一个空的Vue实例作为中央事件总线

```
var bus = new Vue();
//触发组件A中的事件
bus.$emit('id-selected',1);
//在组件B创建的钩子中监听事件
bus.$on('id-selected',function(id){
  
})
```

