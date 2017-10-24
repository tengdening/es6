## vue
### vue 模板语法
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