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
* 数组的解构是按顺序的
```
// 对号入座，一一对应。
var [a,b,c]=[1,2,3];
// 加省略号代表本数组，只能给最后一位加省略号,赋值多于出来的都会放在数组里。
// 那个省略号为...运算符。详情参考Iterator接口
var [a,b,...c]=[1,2,3];
// 空值也占位置
let [x, , y] = [1, 2, 3];
// a=1,b=2,d=4;
let [a, [b], d] = [1, [2, 3], 4];
// 可以添加默认值 默认值必须已经声明。如果它的值是null将不执行默认值
let [a,b="b"]=["a"];
```
### 对象解构
* 变量必须与属性同名，对象的结构不需要按顺序
```
// foo为匹配的模式，aa才是变量，foo只是模式
let { foo:aa,bar:bb } = { foo: "aaa", bar: "bbb" };
// 还可以嵌套
var node = {
  loc: {
    start: {
      line: 1,
      column: 5
    }
  }
};
var { loc, loc: { start }, loc: { start: { line }} } = node;
line // 1
loc  // Object {start: Object}
start // Object {line: 1, column: 5}
// 也可以指定默认值 如果它的值是null将不执行默认值
var {x: y = 3} = {x: 5};
y // 5
var { message: msg = 'Something went wrong' } = {};
msg // "Something went wrong"
// 如果解构失败，变量的值等于undefined。
let {foo} = {bar: 'baz'};
foo // undefined
// {不能写在行首，不然javascript会理解为代码块
let x;
({x} = {x: 1});
//数组本质是特殊的对象，因此可以对数组进行对象属性的解构
let arr = [1, 2, 3];
let {0 : first, [arr.length - 1] : last} = arr;
first // 1
last // 3
```
### 字符串解构
* 字符串解构是因为此时字符串被转换为一个类似数组的对象
```
// 类似数组的对象都有一个length属性，因此还可以对这个属性解构赋值。
let {length : len} = 'hello';
len // 5
```
### 数值和布尔值的解构赋值
* 解构赋值时，如果右边是数值和布尔值时，则先回转换为对象
// 这个看不懂。
### 函数参数的解构赋值
```
function add([x, y]){
  return x + y;
}
add([1, 2]); // 3
```
### 解构赋值的用途
>1. 交换变量的值
>2. 从函数返回多个值
>3. 函数参数的定义
>4. 提取JSON数据
>5. 函数参数的默认值
>6. 遍历map结构
>7. 输入模块的指定方法












## 第三章 字符串的扩展
1. 字符串的Unicode表达式
* Unicode码一般为\uXXXX,但有的时候会超出，那样就无法识别，遇到这样的可以使用{}来解决
2. codePointAt()
* 它与charCodeAt()类似，它可以返回32位的UTF-16的正确编码。返回的是十进制的，如果需要别的进制的使用toString(进制数)来改变
3. fromCodePoint()
* 这个方法定义在String对象上，参数为Unicode编码，返回字符串。
4. 字符串的遍历器接口
* 字符串的Unicode编码可以使用for...of循环遍历。普通的for循环无法遍历大于UTF-16 32字节的Unicode编码。
5. at()
* 用于获取字符串的字符。返回正确的字符，它可以识别Unicode编码大于0xFFFF的字符。
6. normalize()
* 可以使Unicode编码大于0xFFFF的字符的长度合并成一个汉字的长度。
7. includes() startsWith() endsWith()
* 是否找到了参数字符串/参数字符串是否在原字符串的头部/参数字符串是否在原字符串的结尾。（它们返回的是布尔值，可以添加第二个参数，表示开始搜索的位置）
8. repeat()
>1. 返回一个新字符串，参数表示重复n次。
>2. 参数如果是小数，向下取整。
>3. 参数不可以是负数，也不可以是无穷数。-0,NaN都会视为0
>4. 如果是字符串，则会先转换成数字。
9. padStart() padEnd()
* 用于头部补全/用于尾部补全
* 可以添加两个参数，第一个参数为最小的位数。第二个参数为补全的字符串。
* 主要用途：补全数值长度。另一个用途是提示字符串格式。
10. 模板字符串
* `内容` 使用`来定义字符串，定义的字符串可以换行
* 模板字符串中嵌入变量，需要将变量名写在${}里边。{}里边可以放任意的javascript变量，函数，对象。











## 第四章 正则的扩展
### RegExp构造函数
1. 它里边的参数有两种情况
```
// 第一种
new RegExp('xyz', 'i');
// 第二种
new RegExp(/xyz/i);
// 错误的
new RegExp(/xyz/, 'i');
// 正确的
new RegExp(/abc/ig, 'i');
```
2. 回忆下正则对象的方法
>1. test() 返回字符串是否与Reg匹配。
>2. exec() 返回字符串中与reg首次匹配的值
>3. compile() 用于改变RegExp
```
	以上三种都是这种调用模式:
	reg.test/exec/compile(str)
```
>4. replace() 替换reg匹配字符串
```
	str.replace(reg,"替换的字符");
	如果全部替换，需要给正则添加 g全部参数
```
>5. split() 按reg匹配的值拆分成数组
>6. match(n) 返回reg匹配的第n个字符串
>7. search() 返回reg匹配的第一个字符串，没有则返回-1。
```
以上三种都是这种调用模式:
str.split/match/search(reg)
```
>8. 参数 :g 全文查找   i 忽略大小写   m 多行查找   
3. 剩下的内容以后添加，现在看懵逼了。都一堆一堆的。
















## 第十五章 Generator函数的用法
### 基本用法
1. 在函数function后边加*号。
2. 内部有yield命令。执行到这个命令的时候，它会直接返回，下次调用会继续执行下一个yield命令。
3. 必须使用next()方法调用。return语句在yield命令之后仁执行，return之后的yield不再执行。里边可以传参，参数代表它上一个yield返回的值。next()第一个传参无效。它会返回value和done，value是返回的值，done返回的是布尔值，false表示遍历还没有结束。
4. 如果yield当表达式来用的话，需要加括号。















## 第十八章 Class的基本语法
### 简介
1. class和es5的差不多，es5的构造函数对应es6的构造方法。下边的constructor就是构造方法。this则代表实例对象。
2. 定义类的方法的时候，不需要加function关键字。方法之间不需要加逗号。
```
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }
  toString() {
    return '(' + this.x + ', ' + this.y + ')';
  }
}
```
3. es6的类，完全可以看作构造函数的另一种写法，使用也是使用new命令。
4. 构造函数的prototype方法，es6的类上边依然存在。
5. 类的方法都是添加在prototype上的，使用* Object.assign(类.prototype,{添加的方法}) *方法可以更方便的添加。
* 不可枚举：就是使用for ... in遍历，遍历不到。
6. es6类内部的方法是不可枚举的，而es5是可以枚举的。
```
class Point{
	constructor(x,y){
	// ...
	}
	toString(){

	}
}
Object.keys(Point.prototype);
// []
Object.getOwnPropertyNames(Point.prototype)
// [constructor,toString]
```
*该Object.getOwnPropertyNames()方法返回一个直接发现给定对象的所有属性（包括使用Symbol的属性除外的非枚举属性）的数组。
7. 类的属性名可以使用表达式。
```
let methodName = 'getArea';
class Square{
	constructor(length){
	// ...
	}
	[methodName](){
	// ...
	}
}
// Square 的[methodName]方法名为getArea。
```
### 严格方式
类和模块的内部，默认都是严格模式，所以不需要使用* use strict *指定运行模式。
### constructor方法
1. constructor方法是类的默认方法，每个类必须有constructor方法。如果没有，会默认添加。
2. constructor默认返回this 也可以指定返回的函数，那样的话就会返回另一个对象。那样导致实例化出来的对象不是原来的类的实例。
3. 类必须使用new调用，否则会报错，构造函数不需要使用new调用，这就是类和构造函数的区别。

### 类的实例对象
1. 使用new来实例，不使用new会报错。
* hasOwnProperty :用于指示一个对象自身(不包括原型链)是否具有指定名称的属性。如果有，返回true，否则返回false。
2. 与es5一样，实例的属性除非显示定义在其本身（即定义在this对象上），否则都是定义在原型上（即定义在class上）。
```
//定义类
class Point {

  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  toString() {
    return '(' + this.x + ', ' + this.y + ')';
  }

}

var point = new Point(2, 3);

point.toString() // (2, 3)

point.hasOwnProperty('x') // true
point.hasOwnProperty('y') // true
point.hasOwnProperty('toString') // false
point.__proto__.hasOwnProperty('toString') // true
```
3. 与es5一样，类的所有实例都共享一个原型。
* 写到这里的时候，好像明白了 __proto__ 和prototype的关系。
> __proto__是new实例后出现的，如果实例后想改类的方法，如要在这里改。
> prototype是原来的类上的。
> 又看了下边的感觉自己错了，__proto__是各个浏览器厂商的私有属性,反正都不推荐使用，会把原型改掉。
```
var p1 = new Point(2,3);
var p2 = new Point(3,2);
p1.__proto__ === p2.__proto__
//true
```
### Class表达式
* 下边的这个看的很懵，我的理解是一个MyClass的类，Me只能在MyClass内部调用，如果内部不需要调用，可以不写Me.
1. 与函数一样，类也可以使用表达式的形式定义。
```
const MyClass = class Me {
  getClassName() {
    return Me.name;
  }
};
// 上面代码使用表达式定义了一个类。需要注意的是，这个类的名字是MyClass而不是Me，Me只在 Class 的内部代码可用，指代当前类。
let inst = new MyClass();
inst.getClassName() // Me
Me.name // ReferenceError: Me is not defined
// 上面代码表示，Me只在 Class 内部有定义。
// 如果类的内部没用到的话，可以省略Me
```
2. 采用Class表达式，可以写出立即执行的Class，下边定义了一个person的立即执行的类的实例。
```
let person=new class{
	contructor(name){
		this.name=name;
	}
	sayName(){
		console.log(this.name);
	}
}("张三");
person.sayName(); // 张三
```
### 不存在变量提升
1. 类不存在变量提升，否则会报错。

### 私有方法
1. 一般在命名上下功夫，来提示人这是私有还是公有，这个方法只能是规范，它还是可以获取到的。
```
class Widget{
	// 公有方法
	foo(baz){
		this._baz(baz);
	}
	// 私有方法
	_baz(baz){
		return this.snaf=baz;
	}
}
```
2. 另一种方法就是把私有方法移出模块，然后在公有方法里边* bar.call(this,baz) *上、就可以使bar成为了当前模块的私有方法。
```
class Widget{
	// 公有方法
	foo(baz){
		bar.call(this,baz)
	}
}
function bar(baz){
	return this.snaf=baz;
}
```
### 私有属性

* 属性名前边加#，这个也可以应用于私有方法。

### this的指向
1. 类的内部都会定义this，它指向类的实例，但是单独使用，有可能会报错。

### name属性
1. name属性总是返回紧跟在class关键字后面的类名。

### class的取值函数(getter)和存值函数(setter)
1. 在类的内部可以使用* get ** set *关键字，对某个属性设置存值函数和取值函数。拦截该属性的存取行为。

### class的Generator方法。
1. 如果某个方法之前加（*）号，表示该方法是一个Generator函数。

### class的静态方法
1. 类相当于实例的原型，所有类中定义的方法，都会被实例继承。如果在方法前加* static *关键字，就表示该方法不会被继承。而是只能通过类来调用。这种称为静态方法。
```
class Foo{
	static classMethod(){
		return 'hello';
	}
}
Foo.classMethod() //'hello'
var foo=new Foo();
foo.classMethod() //报错 foo.classMethod is a not function
```
2. 如果静态方法包含this关键字，这个this指的是类，而不是实例。
```
class Foo {
  static bar () {
    this.baz();
  }
  static baz () {
    console.log('hello');
  }
  baz () {
    console.log('world');
  }
}

Foo.bar() // hello
```
3. 父类的静态方法，可以被子类继承。
4. 静态方法也可以从super对象上调用。
* 这个super不懂，准备去查一查。
* super的作用是调用父类的protected函数。只有通过"super"这个魔咒，我们才能操作父类的protected成员，别无它法。
```
class Foo {
  static classMethod() {
    return 'hello';
  }
}
class Bar extends Foo {
  static classMethod() {
    return super.classMethod() + ', too';
  }
}
Bar.classMethod() // "hello, too"
```
### class的静态属性和实例属性
#### 实例属性
1. 类的实例属性可以用等式，写入类的定义之中。
```
class MyClass {
  myProp = 42;

  constructor() {
    console.log(this.myProp); // 42
  }
}
```
#### 类的静态属性
1. 类的静态属性在实例属性前面加上，加上static关键字就可以了。
```
class MyClass {
  static myStaticProp = 42;

  constructor() {
    console.log(MyClass.myStaticProp); // 42
  }
}
```
### new.target属性
1. new是构造函数生成实例的命令，es6为new命令引入了new.target属性，该属性一般使用在构造函数中，返回new命令作用于的那个构造函数，如果构造函数不是通过new命令调用的，new.target会返回undefined。这个属性用来确定构造函数是怎么调用的。
2. Class内部调用会new.target,返回当前Class。
```
function Person(name) {
  if (new.target !== undefined) {
    this.name = name;
  } else {
    throw new Error('必须使用new生成实例');
  }
}

// 另一种写法
function Person(name) {
  if (new.target === Person) {
    this.name = name;
  } else {
    throw new Error('必须使用 new 生成实例');
  }
}

var person = new Person('张三'); // 正确
var notAPerson = Person.call(person, '张三');  // 报错
```









## 十九章 Class的继承 
### 简介
1. class 可通过extends来实例继承。
```
class aa{
	
}
class bb extends aa{
	
}
```
2. 字类必须在constructor方法中调用super方法，否则新建实例会报错。因为子类没有自己的this对象，而是继承父类的this对象，然后对其加工，如果不使用super方法，子类就得不到this对象。
```
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }
}
class ColorPoint extends Point {
  constructor(x, y, color) {
    this.color = color; // ReferenceError
    super(x, y);
    this.color = color; // 正确
  }
}
```
3. 子类的实例既是子类的实例，也是父类的实例
```
let cp = new ColorPoint(25, 8, 'green');
cp instanceof ColorPoint // true
cp instanceof Point // true
```
### Object.getPrototypeOf()
* 可以用来判断，一个类是否继承了另一个类。
```
Object.getPrototypeOf(ColorPoint) === Point
```
### super关键字
* super关键字既可以当函数来使用，也可以当对象来使用。
#### 作为函数来使用的时候 
1. super代表函数来使用时，代表父类的构造函数，es6规定，子类的构造函数必须使用一次super关键字。
2. super虽然代表的是父类的构造函数，但是它返回的是子类的实例。即super内部的this指的是子类。因此super在这里相当于父类.prototype.constructor.call(this);
```
class A {
  constructor() {
    console.log(new.target.name);
  }
}
class B extends A {
  constructor() {
    super();
  }
}
new A() // A
new B() // B
```
3. super作为函数来使用的时候，必须放在子类的构造函数里边使用，否则会报错。

#### 作为对象来使用的时候
1. super作为对象来使用的时候，在普通方法中，指向父类的原型对象；在静态方法中，指向父类。
```
class A{
	p(){
		return 1;
	}
}
class B extends A{
	constructor(){
		super()
		console.log(super.p)
	}
}
let b=new B();
```
* 需要注意的是super指向的是原型对象，所以定义在实例上的方法或属性，是无法通过super调用的。
```
class A{
	constructor(){
		this.a=1;
	}
}
class B extends A{
	constructor(){
		super();
	}
	get m(){
		return super.p;
	}
}
let b=new B();
b.m // undefined;
```
* 如果p定义在父类的原型上，super就可以取到。
```
class A{}
A.prototype.p=1;
class B extends A{
	constructor(){
		super()
		console.log(super.p)
	}
}
let b=new B();
```
*es6规定，通过super调用父类的方法时，super会绑定子类的this。
```
class A{
	constructor(){
		this.x=1;
	}
	print(){
		console.log(this.x)
	}
}
class B extends A{
	constructor(){
		super();
		this.x=2;
	}
	m(){
		super.print()
	}
}
let b=new B;
b.m()  //2
// super.print()绑定的是字类的this。所以输出的是2。
```













## 第二十三章 编程分格
### 块级作用域
1. let 取代 var 
2. 常量const在不需要改变值的情况下，const替换let.
3. 字符串一律使用单引号。
4. 数组，函数如果赋值，优先使用解构赋值。
5. 单行定义的对象不要以逗号结尾，多行定义的对象，最后要以逗号结尾。
6. 对象写好之后，就不要再重新添加新属性了，必要的话使用Object.assign(新对象，要添加的属性)来添加。
7. 使用扩展运算符（...）拷贝数组
```
const itemsCopy=[...items];
```
8. 立即执行函数可以写成箭头函数的形式。
```
(()=>{
	console.log("表达式")
})();
```
9. 需要用到函数表达式的场合，使用箭头表达式代替
```
[1,2,3].map((x)=>{
	return x*x;
});
```
10. 

```
```










* 以上内容借鉴于es6标准入门第二版和es6.ruanyifeng.com
