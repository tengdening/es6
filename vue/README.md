## vue
### vue 模板语法
#### 发现的方法
* el 获取app这个id的。
* data 存储数据的
* methods 存储方法的
* filters 添加过滤器的
* computed 计算属性关键词
* methods 计算属性关键词
> 以上两个的效果都是一样的，但是computed是基于它的依赖缓存，只有相关依赖发生改变时才会重新取值。而使用methods，在重新渲染的时候，函数总会重新调用执行。<b>可以说使用 computed 性能会更好，但是如果你不希望缓存，你可以使用 methods 属性。</b>
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
##### 属性和方法
* 每个Vue实例都会代理其data对象里所有的属性
```
	var data = { a: 1 };
	var vm= new Vue({
		data: data
	})
	vm.a === data.a     // true
	vm.a= 2;
	data.a              // 2
	data.a= 3;
	vm.a                // 3
```
* 除了 data 属性， Vue 实例暴露了一些有用的实例属性与方法。这些属性与方法都有前缀 $，以便与代理的 data 属性区分
```
var data= { a: 1 };
var vm= new Vue({
	el: '#app',
	data: data
})
vm.$el === document.getElementById("app")      // true
vm.$data === data 							   // true
vm.watch('a',function(nveVal,oldVal){
	// 这个回调将在'vm.a'改变之后调用
})
```
### Vue.js 条件与循环
#### 条件判断
##### v-if
* 判断条件使用v-if指令
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
##### v-else
* 和 v-if 对应，否则的意思
```
	<div id="app">
		<p>{{ num }}</p>
		<div v-if="num > 0.5">
		  Sorry
		</div>
		<div v-else>
		  Not sorry
		</div>
	</div>
		
	<script>
	new Vue({	
	  	el: '#app',
		data: {
			num: Math.random()
		}
	})
	</script>
``` 
##### v-else-if 
* 也就是js的else if
```
	<div id="app">
	    <div v-if="type === 'A'">
	      A
		</div>
		<div v-else-if="type === 'B'">
		  B
		</div>
		<div v-else-if="type === 'C'">
		  C
		</div>
		<div v-else>
		  Not A/B/C
		</div>
	</div>
		
	<script>
	new Vue({
	  el: '#app',
	  data: {
	    type: 'C'
	  }
	})
	</script>
```
##### v-show
* 可以根据它来判断是否显示
```
	<div id="app">
	    <h1 v-show="ok">Hello!</h1>
	</div>
		
	<script>
	new Vue({
	  el: '#app',
	  data: {
	    ok: true
	  }
	})
	</script>
```
#### 循环语句
##### v-for
* 以site in sites 形式的特殊语法
```
	<div id="app">
		<ol>
			<li v-for="site in sites">
				{{ site.name }}
			</li>
		</ol>
	</div>
	<script>
		new Vue({
			el: '#app',
			data: {
				sites:[
					{name: 'Runoob'},
					{name: 'Google'},
					{name: 'Taobao'}
				]
			}
		})
	</script>
```
##### v-for 迭代对象
* v-for 可以通过一个对象的属性来迭代数据。
```
	<div id="app">
		<ul>
			<li v-for="value in object">
				{{ value }}
			</li>
		</ul>
	</div>
	<script>
		new Vue({
			el: '#app',
			data: {
				object: {
					name: '题目',
					url: 'www.baidu.com',
					slogan: '这叫技术'
				}
			}
		})
	</script>
```
* 也可以传第二个参数获取它的键名
```
	<div id="app">
		<ul>
			<li v-for="(value,key) in object">
				{{ key }} : {{ value }}
			</li>
		</ul>
	</div>
	<script>
		new Vue({
			el: '#app',
			data: {
				object: {
					name: '题目',
					url: 'www.baidu.com',
					slogan: '这叫技术'
				}
			}
		})
	</script>
```
* 第三个参数为索引
```
	<div id="app">
		<ul>
			<li v-for="(value,key,index) in object">
			{{ index }} . {{ key }} : {{ value }}
			</li>
		</ul>
	</div>
	<script>
		new Vue({
			el: '#app',
			data: {
				object: {
					name: '题目',
					url: 'www.baidu.com',
					slogan: '这叫技术'
				}
			}
		})
	</script>
```
* v-for 迭代整数,它也可以循环整数
```
	<div id="app">
	  <ul>
	    <li v-for="n in 10">
	     {{ n }}
	    </li>
	  </ul>
	</div>

	<script>
	new Vue({
	  el: '#app'
	})
	</script>
```
#### vue.js计算属性
* 计算属性关键词 computed
```
	<div id="app">
		<p>
			原始字符串：{{ message }}
		</p>
		<p>
			计算后字符串：{{ reversedMessage }}
		</p>
	</div>
	<script>
		new Vue({
			el: "#app",
			data: {
				message: 'Runoob!'
			},
			computed: {
				reversedMessage: function(){
					return this.message.split('').reverse().json('')
				}
			}
		})
	</script>
```
* 计算属性关键词 methods
```
	<div id="app">
		<p>
			原始字符串：{{ message }}
		</p>
		<p>
			计算后字符串：{{ reversedMessage() }}
		</p>
	</div>
	<script>
		new Vue({
			el: "#app",
			data: {
				message: 'Runoob!'
			},
			methods: {
				reversedMessage: function(){
					return this.message.split('').reverse().json('')
				}
			}
		})
	</script>
```
