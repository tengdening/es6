# es6
保存我自己的es6练习

## 第一章 let和const命令
### let
1. let声明的变量只在其所在代码块有效
2. for循环可以使用let,每一次循环的let 值都是重新声明的变量。
3. let不存在变量提升，所以let声明的变量只能先声明，在使用，不能先使用再声明。
4. 在代码块内，使用let命令之前，该变量在代码块中是不可用的。
5. let不可以在代码块中重复声明.

### 块级作用域
1. 内层变量会覆盖外层变量
2. 用来计算的循环变量会变为全局变量
3. 因为快级作用域的出现，立即执行匿名函数就没必要了。

### const
1. 声明一个常量，一旦声明，常量的值不可以改变。
* 保存的只是地址，比如数组和对象，它里边还是可以插入值的
2. 只能在声明之后使用。
3. 不可重复声明。

#### es6有六种声明变量的方式
```
var function let const import class
````

### 顶层对象的属性
1. 在浏览器指window对象，在Node指global对象。
2. 在全局环境使用let声明的变量不会成为顶层对象。

#### 以下两个方法为顶层对象兼容。
```
// 方法一
(typeof window !== 'undefined'
	? window
	: (typeof process === 'object' &&
	  typeof require === 'function' &&
	  typeof global === 'object')
	? global
	: this);

// 方法二
var getGlobal = function () {
	if (typeof self !== 'undefined') { return self; }
	if (typeof window !== 'undefined') { return window; }
	if (typeof global !== 'undefined') { return global; }
	throw new Error('unable to locate global object');
};
```
* 以上内容借鉴于es6.ruanyifeng.com /2.let和const命令，最下边还有组件和包的顶层对象兼容方法。