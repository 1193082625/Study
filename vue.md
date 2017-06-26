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

