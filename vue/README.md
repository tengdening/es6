## vue
### vue 模板语法
#### 发现的方法
* el 获取app这个id的。
* data 存储数据的
* methods 存储方法的
* filters 添加过滤器的
* computed 计算属性关键词
* methods 计算属性关键词
* component 创建组件用到的
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
* computend setter
```
	<div id="app">
	  <p>{{ site }}</p>
	</div>

	<script>
		var vm = new Vue({
		  el: '#app',
		  data: {
			name: 'Google',
			url: 'http://www.google.com'
		  },
		  computed: {
		    site: {
		      // getter
		      get: function () {
		        return this.name + ' ' + this.url
		      },
		      // setter
		      set: function (newValue) {
		        var names = newValue.split(' ')
		        this.name = names[0]
		        this.url = names[names.length - 1]
		      }
		    }
		  }
		})
		// 调用 setter， vm.name 和 vm.url 也会被对应更新
		vm.site = '菜鸟教程 http://www.runoob.com';
		document.write('name: ' + vm.name);
		document.write('<br>');
		document.write('url: ' + vm.url);
	</script>
```
> 上边的name和url被下边name和url所覆盖
#### vue.js 样式绑定
##### class属性绑定
* 可以为 v-bind:class 设置一个对象，来动态的切换class
```
	<style>
		.active {
			width: 100px;
			height: 100px;
			background: green;
		}
	</style>
	<div id="app">
	  <div v-bind:class="{ active: isActive }"></div>
	</div>

	<script>
		new Vue({
		  el: '#app',
		  data: {
		    isActive: true
		  }
		})
	</script>
```
* 也可以在对象中传入更多的属性来切换多个class
```
	<style>
		.active {
			width: 100px;
			height: 100px;
			background: green;
		}
		.red{
			background: red;
		}
	</style>
	<div id="app">
	  <div v-bind:class="{ active: isActive ,red: isRed}"></div>
	</div>

	<script>
		new Vue({
		  el: '#app',
		  data: {
		    isActive: true,
		    isRed: true
		  }
		})
	</script>
```
* 也可以直接绑定一个对象
```
	<style>
		.active {
			width: 100px;
			height: 100px;
			background: green;
		}
		.red{
			background: red;
		}
	</style>
	<div id="app">
	  <div v-bind:class="isObject"></div>
	</div>

	<script>
		new Vue({
		  el: '#app',
		  data: {
		  	isObject:{
			  	active: true,
			    'red': true
		  	}
		    
		  }
		})
	</script>
```
* 绑定返回对象的计算属性
```
	<div id="app">
	  <div v-bind:class="classObject"></div>
	</div>

	<script>
	new Vue({
	  el: '#app',
	  data: {
	  isActive: true,
	  error: null
	  },
	  computed: {
	    classObject: function () {
	      return {
	        active: this.isActive && !this.error,
	        'text-danger': this.error && this.error.type === 'fatal',
	      }
	    }
	  }
	})
	</script>
```
##### 数组语法
* 把一个数组传给v-bind:class
```
	<div id="app">
		<p v-bind:class="[activeClass,errorClass]"></p>	
	</div>
	<script>
		new Vue({
			el: #app,
			data: {
				activeClass: 'active',
				errorClass: 'text-danger'
			}
		})
	</script>
```
* 使用三元表达式来切换列表中的class
```
	<style>
	.text-danger {
		width: 100px;
		height: 100px;
		background: red;
	}
	.active {
		width: 100px;
		height: 100px;
		background: green;
	}
	</style>
	<div id="app">
		<div v-bind:class="[errorClass,isActive?activeClass: '']"></div>
	</div>
	<script>
		new Vue({
			el: '#app',
			data: {
				isActive: true,
				activeClass: 'active',
				errorClass: 'text-danger'
			}
		})
	</script>
```
##### vue.js style(内联样式)
* v-bind:style 直接设置样式
```
<div id="app">
	<div v-bind:style="{ color: activeClass,fontSize: fontSize + 'px' }">标题</div>
</div>
<script>
	new Vue({
		el: '#app',
		data: {
			activeClass: 'green',
			fontSize: 50
		}
	})
</script>
```
* 也可以直接绑定一个样式对象
```
<div id="app">
	<div v-bind:style="styleObject">标题</div>
</div>
<script>
	new Vue({
		el: '#app',
		data: {
			styleObject: {
				color: 'red',
				fontSize: '50px'
			}
		}
	})
</script>
```
* 页可以使用数组将多个样式对象应用到元素上
```
<div id="app">
	<div v-bind:style="[styleObject,fontWeightObject]">标题</div>
</div>
<script>
	new Vue({
		el: '#app',
		data: {
			styleObject: {
				color: 'red',
				fontSize: '50px'
			},
			fontWeightObject: {
				fontWeight: 'bold'
			}
		}
	})
</script>
```
#### vue.js事件处理器
##### 需要事件监听可以使用v-on事件
```
	<div id='app'>
		<button v-on:click="counter += 1">增加1</button>
		<p>这个按钮被点击了{{ counter }}次。</p>
	</div>
	<script>
		new Vue({
			el: '#app',
			data: {
				counter: 0
			}
		})
	</script>
```
* 通常情况下我们可以使用一个方法来调用javascript方法。
```
	<div id="app">
		<div v-on:click='greet'>点击</div>
	</div>
	<script>
		new Vue({
			el: "#app",
			data: {
				name: 'vue.js'
			},
			methods: {
				greet: function(event){
					alert('hello'+ this.name+ '!')
					if(event){
						alert(event.target.tagName)
					}
				}
			}
		})
	</script>
```
* 除了可以直接绑定到一个方法，也可以用来内联javascript语句
```
	<div id='app'>
		<button v-on:click="say(hi)">say hi</button>
		<button v-on:click="say(what)">say what</button>
	</div>
	<script>
		new Vue({
			el: "#app",
			methods: {
				say: function(message){
					alert(message)
				}
			}
		})
	</script>
```
##### 事件修饰符
* Vue.js 为 v-on 提供了事件修饰符来处理 DOM 事件细节，如：event.preventDefault() 或 event.stopPropagation()。Vue.js通过由点(.)表示的指令后缀来调用修饰符。
> .stop
> .prevent
> .capture
> .self
> .once
```
	阻止事件冒泡
	<a v-on:click.stop="doThis"></a>
	提交事件不再重复加载页面
	<form v-on:submit.prevent="onSubmit"></form>
	修饰词可以串联
	<a v-on:click.stop.prevent="doThat"></a>
	只有修饰符
	<form v-on:submint.prevent></form>
	添加事件侦听器时使用事件捕获模式
	<div v-on:click.capture="doThis">...</div>
	只当事件在该元素本身触发时触发回调
	<div v-on:click.self="doThat">...</div>
	click只能点击一次
	<div v-on:click.once="doThis"></div>
```
##### 按键修饰符
* vue允许为v-on在监听器事件时添加按键修饰符。
> .enter
> .tab
> .delete
> .esc
> .space
> .up
> .down
> .left
> .right
> .ctrl
> .alt
> .shift
> .meta
```
	只有在keyCode是13时调用 vm.submit()
	<input v-on:keyup:13='submit'>
	记住所有的keyCode不现实，Vue为最常用的设置了别名
	<input v-on:keyup.enter='submit'>
	缩写
	<input @keyup.enter='submit'>
	Alt + C
	<input @keyup.alt.67="clear">
	Ctrl + Click
	<div @click.ctrl="doSomething">Do something</div>
```
#### Vue.js 提交表单
##### v-model
* 会根据控件类型自动选取正确的方法来更新元素。
```
<div id="app">
	<p>{{ message }}</p>
	<input v-model="message">
</div>
<script>
	new Vue({
		el: "#app",
		data: {
			message: "aaaaa",
		}
	})
</script>
```
##### 复选框
* 复选框单个为逻辑值，多个则绑定到同一个数组中。
```
	<div id="app">
		<p>单个复选框</p>
		<input type="checkbox" v-model="checked">
		<label for="checkbox">{{ checked }}</label>
		<p>多个复选框</p>
		<input type="checkbox" value="Runoob" v-model='checkNames'>
		<label for="runoob">Runoob</label>
		<input type="checkbox" value="Goolgle" v-model='checkNames'>
		<label for="goolgle">Goolgle</label>
		<input type="checkbox" value="Taobao" v-model='checkNames'>
		<label for="taobao">Taobao</label>
		<br>
		<span>选择的值为：{{ checkNames }}</span>
	</div>
	<script>
		new Vue({
			el: "#app",
			data: {
				checked: false,
				checkNames: []
			}
		})
	</script>
```
##### 单选按钮
* 单选按钮的双向绑定
```
	<div id="app">
	  <input type="radio" id="runoob" value="Runoob" v-model="picked">
	  <label for="runoob">Runoob</label>
	  <br>
	  <input type="radio" id="google" value="Google" v-model="picked">
	  <label for="google">Google</label>
	  <br>
	  <span>选中值为: {{ picked }}</span>
	</div>

	<script>
	new Vue({
	  el: '#app',
	  data: {
		picked : 'Runoob'
	  }
	})
	</script>
```
##### select按钮
* 下拉列表的双向数据绑定
```
	<div id="app">
	  <select v-model="selected" name="fruit">
	    <option value="">选择一个网站</option>
	    <option value="www.runoob.com">Runoob</option>
	    <option value="www.google.com">Google</option>
	  </select>
	 
	  <div id="output">
	      选择的网站是: {{selected}}
	  </div>
	</div>

	<script>
	new Vue({
	  el: '#app',
	  data: {
		selected: '' 
	  }
	})
	</script>
```
##### 修饰符
###### .lazy
* 添加 .lazy 以后，双向数据绑定会转变为在change事件中同步
```
	在 "change" 而不是 "input" 事件中更新
	<input v-model.lazy="msg" >
```
###### .number
* 如果想自动将用户的输入值转为Number类型（如果原值的转换结果为NaN则返回原值），可以添加一个修饰符 number 给 v-model 来处理输入值
```
	<input v-model.number="age" type="number">
```
###### .trim
* 如果要自动过滤用户输入的首尾空格，可以添加 trim 修饰符到 v-model 上过滤输入
```
	<input v-model.trim="msg">
```
#### vue.js组件
##### 全局组件
* 所有实例都能使用全局组件
```
	<div id="app">
		<tem></tem>
	</div>
	<script>
		Vue.component('tem',{
			template: '<h1>自定义组件！</h1>'
		})
		new Vue({
			el: "#app"
		})
	</script>
```
##### 局部组件
* 我们也可以在实例选项中注册局部组件，这样组件只能在实例中使用。
```
	<div id="app">
		<tem></tem>
	</div>
	<script>
		var Child={
			template: '<h1>自定义组件！</h1>'
		}
		new Vue({
			el: "#app",
			components: {
				tem: Child
			}
		})
	</script>
```
##### prop
* 父组件用来传递数据的一个自定义属性。父组件的数据需要通过 props把数据传给子组件，子组件需要显式地用 props 选项声明 "prop"：
```
	<div id="app">
		<tem tems="hello word!"></tem>
	</div>
	<script>
		Vue.component('tem',{
			props: ['tems'],
			temlpate: '<span>{{ tems }}</span>'
		})
		new Vue({
			el: "#app"
		})
	</script>
```
##### 动态Prop 
* 类似于用 v-bind 绑定 HTML 特性到一个表达式，也可以用 v-bind 动态绑定 props 的值到父组件的数据中。每当父组件的数据变化时，该变化也会传导给子组件：
```
	<div id="app">
		<input v-model="message">
		<br>
		<tem v-bind:tems="message"></tem>
	</div>
	<script>
		Vue.component('tem',{
			props: ['tems'],
			template: '<span>{{ tems }}</span>'
		})
		new Vue({
			el: "#app",
			data: {
				message: '父组件内容'
			}
		})
	</script>
```
* 将v-bind指令传到每一个重复的组件中。
```

```