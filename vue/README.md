## vue
### vue 模板语法
#### 发现的方法
* el 获取app这个id的。
* data 存储数据的
* methods 存储方法的
* filters 添加过滤器的
#### 插值
##### 文本
* 最常用的就是双大括号的方式插值 '{{}}';
##### html
* v-html	输出html代码。
```
	<div id="app">
	    <div v-html="message"></div>
	</div>
		
	<script>
	new Vue({
	  el: '#app',
	  data: {
	    message: '<h1>菜鸟教程</h1>'
	  }
	})
	</script>
```
##### 属性
* v-bind    html属性中的值应使用它，如果为true它执行，如果为false不执行
```
	<style>
	.class1{
	  background: #444;
	  color: #eee;
	}
	</style>
	<div id="app">
	  <label for="r1">修改颜色</label><input type="checkbox" v-model="class1" id="r1">
	  <br><br>
	  <div v-bind:class="{'class1': class1}">
	    directiva v-bind:class
	  </div>
	</div>
		
	<script>
	new Vue({
		el: '#app',
	  data:{
	  	class1: false
	  }
	});
	</script>
```
##### 表达式
* vue完全支持javascript的表达式规则
```
	<div id="app">
		{{5+5}}<br>
		{{ ok ? 'YES' : 'NO' }}<br>
		{{ message.split('').reverse().join('') }}
		<div v-bind:id="'list-' + id">菜鸟教程</div>
	</div>
		
	<script>
	new Vue({
	  el: '#app',
	  data: {
		ok: true,
	    message: 'RUNOOB',
		id : 1
	  }
	})
	</script>
```
##### 指令
* v-前缀的特殊属性，
```
	<div id="app">
	    <p v-if="seen">现在你看到我了</p>
	    <template v-if="ok">
	      <h1>菜鸟教程</h1>
	      <p>学的不仅是技术，更是梦想！</p>
	      <p>哈哈哈，打字辛苦啊！！！</p>
	    </template>
	</div>
	    
	<script>
	new Vue({
	  el: '#app',
	  data: {
	    seen: true,
	    ok: true
	  }
	})
	</script>
```
##### 参数
* 参数在指令后边用冒号'：'隔开。
```
	<div id="app">
	    <pre><a v-bind:href="url">菜鸟教程</a></pre>
	</div>
		
	<script>
	new Vue({
	  el: '#app',
	  data: {
	    url: 'http://www.runoob.com'
	  }
	})
	</script>
```
* v-on 指令，它用于监听 DOM 事件
```
	<a v-on:click="doSomething">
```
##### 修饰符
* 修饰符是以半角句号 . 指明的特殊后缀，用于指出一个指定应该以特殊方式绑定。例如，.prevent 修饰符告诉 v-on 指令对于触发的事件调用 event.preventDefault()：
```
	<form v-on:submit.prevent="onSubmit"></form>
```
##### 用户输入
* 在input中，我们使用v-model指令来实现双向数据绑定
```
	<div id="app">
		<p>{{message}}</p>
		<input v-model="message">
	</div>
	<script>
		new Vue({
			el: "#app",
			data: {
				message: 'Runoob!'
			}
		})
	</script>
```
* 按钮的事件我们可以使用 v-on 监听事件，并对用户的输入进行响应。
```
	<div id="app">
		<p>{{message}}</p>
		<button v-on:click="reverseMessage">反转字符串</button>
	</div>
	<script>
		new Vue({
			el: "#app",
			data: {
				message: 'Runoob!'
			},
			methods: {
				reverseMessage: function(){
					this.message=this.message.split('').reverse().join('')
				}
			}
		})
	</script>
```
##### 过滤器
* vue.js允许自己定义过滤器，被用作一些常见的文本格式化。由"管道符"指示
```
	在俩个大括号中
	{{ message | capitalize }}
	在v-bind指令中
	<div v-bind:id="rawId | formatId"></div>
```
* 过滤器函数接受表达式的值作为第一个参数。
```
<div id="app">
	{{ message | capitalize }}
</div>
<script>
	new Vue({
		el: "#app",
		data: {
			runoob
		},
		filters: {
			capitalize: function(value){
				if(!value){
					return ''
				}
				value = value.toString()
				return value.charAt(0).toUpperCase()+value.Slice(1)
			}
		}
	})
</script>
```
* 过滤器可以串联
```
	{{ message | filterA | filterB }}
```
* 过滤器是javascript函数，因此可以传递参数
```
	message是第一个参数，'arg1'是第二个参数，arg2表达式将求值之后作为第三个参数
	{{ message | filterA('arg1',arg2) }}
```
#### 缩写
* vue.js 为两个最为常用的指令提供了特别的缩写
##### v-bind 缩写
```
	完整写法
	<a v-bind:href="url"></a>
	缩写
	<a :href="url"></a>
```
##### v-on 缩写
```
	完整写法
	<a v-on:click="doSomething"></a>
	缩写
	<a @click="doSomething"></a>
```
#### Vue 实例
##### 构造器
* 每一个vue.js都是通过构造函数Vue创建一个Vue的根实例来启动的。
```
	var vm = new Vue({
		// 选项
	})
```