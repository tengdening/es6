# es6
保存我自己的es6练习

## 第一章 let和const命令
### let
1. let声明的变量只在其所在代码块有效
2. for循环可以使用let,每一次循环的let 值都是重新声明的变量。
3. let不存在变量提升，所以let声明的变量只能先声明，在使用，不能先使用再声明。
4. 在代码块内，使用let命令之前，该变量在代码块中是不可用的。
5. let不可以在代码块中重复声明。

### 块级作用域
1. 内层变量会覆盖外层变量
2. 用来计算的循环变量会变为全局变量
3. 因为快级作用域的出现，立即执行匿名函数就没必要了。

### const
1. 声明一个常量，一旦声明，常量的值不可以改变。
* 保存的只是地址，比如数组和对象，它里边还是可以插入值的
2. 只能在声明之后使用。
3. 不可重复声明。
4. const声明的变量只在其所在的代码块有效。

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
* 以上内容借鉴于es6标准入门第二版和es6.ruanyifeng.com /2.let和const命令，最下边还有组件和包的顶层对象兼容方法。

#### 跨模块常量
* 必须在es6环境下才可以使用，否则会报错。
```
	//constants.js模块
	export const A=1;
	export const B=2;
	//test1.js模块 
	import *as constants from './constants';
	console.log(constants.A);
	//test2.js模块
	import {A,B} from './constants';
	console.log(A);
```


## 第二章 变量的解构赋值
### 数组的解构赋值
1. 数组解构
```
// 对号入座，一一对应。
var [a,b,c]=[1,2,3];
// 加省略号代表本数组，只能给最后一位加省略号,赋值多于出来的都会放在数组里。
// 那个省略号为...运算符。详情参考Iterator接口
var [a,b,...c]=[1,2,3];
// 空值也占位置
let [x, , y] = [1, 2, 3];
// 依然对号入座。b=2,d=3;
let [a, [b], d] = [1, [2, 3], 4];
// 可以添加默认值 默认值必须已经声明。
let [a,b="b"]=["a"];
```
2. 对象解构
>1. 变量必须与属性同名
```
// foo为匹配的模式，aa才是变量。
let { foo:aa,bar:bb } = { foo: "aaa", bar: "bbb" };



















## 第十五章 Generator函数的用法
### 基本用法
1. 在函数function后边加*号。
2. 内部有yield命令。执行到这个命令的时候，它会直接返回，下次调用会继续执行下一个yield命令。
3. 必须使用next()方法调用。return语句在yield命令之后仁执行，return之后的yield不再执行。里边可以传参，参数代表它上一个yield返回的值。next()第一个传参无效。它会返回value和done，value是返回的值，done返回的是布尔值，false表示遍历还没有结束。
4. 如果yield当表达式来用的话，需要加括号。
