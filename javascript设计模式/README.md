# javascript设计模式
## 对象实例
* 一个好的面向对象编程方法。
```
	var Tn=function(){};
	Tn.prototype={
		A:function(){
			// ...
			return this;
		},
		B:function(){
			// ...
			return this;
		},
	}
	var tn=new Tn();
	tn.A.B;
```
* 给所有函数添加一个方法
```
	Function.prototype.addMethod=function(name,fn){
		this[name] = fn;
	};
	// 创建成功后，这样做。
	var methods=function(){};
	methods.addMethod('checkName',function(){
		// 验证姓名
	})
	methods.checkName()
```
* 链式
```
	Function.prototype.addMethod=function(name,fn){
		this[name] = fn;
		return this;
	}
	var methods=function(){};
	methods.addMethod('checkName',function(){
		// 验证姓名
		return this;
	}).addMethod('checkEmail',function(){
		// 验证邮箱
		return this;
	})
	methods.checkName().checkEmail();
```
* 给所有函数添加一个方法，类式调用。
```
	Function.prototype.addMethod=function(name,fn){
		this.prototype[name]=fn;
		return this;
	}
	var Methods=function(){};
	Methods.addMethod('checkName',function(){
		// 验证姓名
		return this;
	}).addMethod('checkEmail',function(){
		// 验证邮箱
		return this;
	})
	// 这里就需要new一下了。
	let m=new Methods();
	m.checkName().checkEmail();
```
## 面向对象编程的各种属性，方法
* 隐私性
```
	var Book=function(id,name,price){
		// 私有属性（对象外边访问不到）
		var num=2; 
		// 私有方法（对象外边访问不到）
		function checkId(){
			console.log('checkId获取到了');
		};
		// 特权方法（对象外边可以调用，get获得,set输出）
		this.getName=function(){
			console.log('getName获取到了')
		};
		this.getPrice=function(){
			console.log('getPrice获取到了')
		};
		this.setName=function(){
			console.log(name)
		};
		this.setPrice=function(){
			console.log(price)
		};
		// 对象公有属性（对象外边可以获取到）
		this.id=id;
		// 对象公有方法（对象外边可以获取到）
		this.page=function(){
			console.log('page获取到了')
		};
		// 构造器（一遍历对象就会执行）
		 this.setName(name);
		 this.setPrice(price);
	}
	// 类静态公有属性（new过之后子类无法调用，父类自己可以调用）
	Book.isChinese=true;
	// 类静态公有方法（new过之后子类无法调用，父类自己可以调用）
	Book.resetTime=function(){
		console.log('new Tiem');
	}
	Book.prototype={
		// 公有属性（对象外边可以获取到）
		isJSBook : false,
		// 公有方法（对象外边可以获取到）
		display : function(){
			console.log('display获取到了');
		}
	}
	var book=new Book(10,'zhangsan',999);
```
## 闭包实现
* 闭包可以这样来实现,闭包是有权访问另一个函数作用域中变量的函数。即在一个函数中创建另一个函数。
* 我读了多遍，现在的理解是，在一个函数里边创建了另一个函数，里边函数可以访问外边函数的私有变量，然后把里边函数返出去。
```
	var Book=(function(){
		var num=1;
		function aa(){};
		function _book(newId,newName,newPrice){
			var name,price;
			function checkId(id){};
			this.getName=function(){};
			this.getPrice=function(){};
			this.setName=function(){};
			this.setPrice=function(){};
			this.id=newId;
			this.copy=function(){};
			num++;
			if(num>100){
				throw new Error("超过100了"); 
			}
			this.setName(name);
			this.setPrice(price);
		}
		_book.prototype=function(){
			isJSBook: false,
			display: function(){}
		}
		return _book;
	})()
```
* 为了解决实例对象的时候，忘记写new而出错的问题解决加法。
```
	if(this instanceof Book){
		this.title=title;
		this.time=time;
		this.type=type;
	}else{
		return new Book(title,time,type);
	}
	// 这样就可以解决忘记写new出错的问题。
```
## 继承
* 类的三个部分。
```
	var A=function(aa){
		this.aa=aa;
	};
	A.cc=function(){};
	A.prototype.bb=function(){
		b:function(){};
	};
```
### 类似继承
* instanceof是通过判断对象的prototype链来确定这个对象是否是某个类的实例。而不关心对象与类的结构。
```
	var A=function(){
		this.aa=true;
	}
	A.prototype.bb=function(){
		return this.aa;
	}
	var B=function(){
		this.cc=false;
	}
	B.prototype=new A();
	B.prototype.dd=function(){
		return this.cc;
	}
	var b=new B()
	console.log(b.aa);
	console.log(b.bb);
	console.log(b.cc);
	console.log(b.dd);
	// 以上四个控制台显示，都可以获取到。
```
* 它的缺点1，无法给父类传递参数。2，更改一个子类，所有子类都会被更改。
### 构造函数继承
* AA.call(this,id);这样可以将子类中的变量在父类中执行一遍。
```
	function AA(id){
		this.books=['javascript','html','css'];
		this.id=id;
	}
	AA.prototype.aa=function(){
		console.log(this.books);
	};
	function BB(id){
		AA.call(this,id);
	}
	var bb1=new BB(20);
	var bb2=new BB(22);
	bb1.books.push('php')
	console.log(bb1.id)
	console.log(bb2.id)
	console.log(bb1.books);
	console.log(bb2.books);
	// 以上四种可以实现。
	console.log(bb1.aa) // 报错
```
* 缺点：只能继承构造函数里边的，原型里的无法继承。
### 组合继承
* 吸收上边的优点，组合。
```
	function AA(id){
		this.books=['javascript','html','css'];
		this.id=id;
	}
	AA.prototype.aa=function(){
		console.log(this.books);
	};
	function BB(id){
		AA.call(this,id);
		this.name=2;
	}
	BB.prototype=new AA();
	BB.prototype.bb=function(){
		console.log(this.name)
	}
```
* 缺点：上边的方法调用了两次父类的构造函数。
### 原型式继承
* 最开始看的模模糊糊，回过头来看，发现就是把原型的值分出去了。
```
function inheritObject(o){
	function F(){};
	F.prototype=o;
	return new F();
}
var book={
	name: 'js book',
	alikeBook: ['css book','html book']
}
var newBook=inheritObject(book);
newBook.name='ajax book';
newBook.alikeBook.push('xml book');
var otherBook=inheritObject(book);
otherBook.name='flash book';
otherBook.alikeBook.push('as book');
console.log(newBook.name);  //ajax book
console.log(newBook.alikeBook);  // ["css book", "html book", "xml book", "as book"]
console.log(otherBook.name);  //flash book
console.log(otherBook.alikeBook);  // ["css book", "html book", "xml book", "as book"]
console.log(book.name);   // js book
console.log(book.alikeBook); js book  // ["css book", "html book", "xml book", "as book"]
```
### 寄生式继承
* 看懂点,相当于对inheritObject函数进行了第二次封装。
```
	function inheritObject(o){
		function F(){};
		F.prototype=o;
		return new F();
	}
	var book={
		name: 'js book',
		alikeBook: ['css book','html book']
	}
	function createBook(obj){
		var o= inheritObject(obj);
		o.getName=function(){
			console.log(this.name);
		};
		return o;
	}
	var aa=new createBook(book);
	aa.getName();
	console.log(aa.name);
	console.log(aa.alikeBook);
```
### 寄生组合式继承
* 先创建一个继承对象，里边返回一个实例化对象。
* 然后创建一个固有原型，里边把第二个参数的原型返回给继承对象，然后得到一个有第二个参数的对象P，
* 然后把第一个参数给了P的构造函数，最后第一个参数的原型等于P。
* 上边其实就是把第一个对象的构造函数继承下来，把第二对象的原型继承下来。
```
	function inheritObject(o){
		function F(){};
		F.prototype=o;
		return new F();
	}
	function inherPrototype(subClass,superClass){
		var p=inheritObject(superClass.prototype);
		p.constructor=subClass;
		subClass.prototype=p;
	}
	function SuperClass(name){
		this.name=name;
		this.colors=['red','green'];
	}
	SuperClass.prototype.getName=function(){
		console.log(this.name);
	};
	function SubClass(name,time){
		SuperClass.call(this,name);
		this.time=time;
	};
	inherPrototype(SubClass,SuperClass);
	SubClass.prototype.getTime=function(){
		console.log(this.time);
	};
	var instance1=new SubClass('js book',2014);
	var instance2=new SubClass('css book',2013);
	instance1.colors.push('yellow');
	console.log(instance1.colors);
	console.log(instance2.colors);
	instance2.getName();
	instance2.getTime();
```
* 缺点：子类想添加原型方法，必须通过prototype对象，通过点语法一个一个添加 。
### 多继承
* extend方法可以用来实现浅拷贝
```
	var extend=function(target,source){
		for (prototype in source){
			target[prototype]=source[prototype];
		}
		return target;
	}
	var book={
		name: 'javascript 设计模式',
		alike: ['css','html','javascript']
	}
	var anotherBook={
		color: 'blue'
	}
	extend(anotherBook,book);
	console.log(anotherBook.name);  // javascript 设计模式
	console.log(anotherBook.alike); // ['css','html','javascript']
	anotherBook.alike.push('ajax');
	anotherBook.name="设计模式";
	console.log(anotherBook.name); // 设计模式
	console.log(anotherBook.alike); // ['css','html','javascript','ajax'];
	console.log(book.name);  // javascript 设计模式
	console.log(book.alike); // ['css','html','javascript','ajax'];
```
* 多继承，直接写成绑定在Object上的方法。调用方法为给某个函数.mix(函数1,函数2)
```
	Object.prototype.mix=function(){
		var i=0,
			len=arguments.length,
			arg;
		for (;i<len;i++){
			arg=arguments[i];
			for(var prototype in arg){
				this[prototype]=arg[prototype];
			}
		}
	}
```
### 多态
* 利用arguments个数判断，来实现多态。
```
	function add(){
		var arg=arguments,
			len=arg.length;
		switch(len){
			case 0:
				return 10;
			case 1:
				return 10+arg[0];
			case 2:
				return arg[0]+arg[1];
		}
	}
	console.log(add())
	console.log(add(5))
	console.log(add(8,1))
```
## 工厂模式
* 简单工厂函数把多个类合并在一个函数里边。
```
	function Baskeball(){
		this.intro="篮球";
	};
	Baskeball.prototype={
		aa: function(){
			console.log("篮球方法");
		},
	};
	function Football(){
		this.intro="足球";
	};
	Football.prototype={
		bb: function(){
			console.log("足球方法");
		},
	};
	var SportFactory=function(name){
		switch(name){
			case 'NBA':
				return new Baskeball();
			case 'wordCup':
				return new Football();
		}
	};
	var bb=SportFactory('NBA');
	bb.aa();
	console.log(bb.intro);
```
* 简单工厂创建相似对象
```
	function createBook(type,text){
		var o = new Object();
		o.content=text;
		o.show=function(){
			// 显示方法
		}
		if (type=="alert") {
			// 执行体
		}else if(type=="prompt"){
			// 执行体
		}else if(type=="confirm"){
			// 执行体
		}
		return o;
	};
	var userNameAlert=createBook('alert','用户只能输入26个字母和数字');
```
## 工厂方法模式
* 安全的工厂方法模式
```
	var Factory = function(type,content){
		if(this instanceof Factory){
			var s=new this[type](content);
			return s;
		}else{
			return new Factory(type,content)
		}
	}
	Factory.prototype={
		Java:function(content){
			/*...*/
		},
		Javascript:function(content){
			/*...*/
		},
		Ui:function(content){
			this.content=content;
			(function(content){
				var div=document.createElement("div");
				div.innerHTML=content;
				div.style.border="1px solid #f00";
				document.getElementById("container").appendChild(div);
			})(content);
		},
		Php:function(content){
			/*...*/
		}
	};
	var data=[
		{type : 'Javascript' , content : "javascript哪家强", },
		{type : 'Ui' , content : "Ui哪家强", },
	];
	for( var i=0; i<data.length;i++){
		Factory(data[i].type,data[i].content)
	}
```
## 抽象的工厂模式
* 抽象工厂模式不需要用来创建对象。可以把它当作父类，来创建一些子类。
* 看到这里的时候，发现subType.constructor=subType这句看不懂，需要返回去多看几遍继承了。
```
	var VehicleFactory=function(subType,superType){
		if(typeof VehicleFactory[superType] === 'function'){
			function F(){};
			F.prototype= new VehicleFactory[superType]();
			subType.constructor=subType;
			subType.prototype=new F();
		}else{
			throw new Error('未创建该抽象类');
		}
	};
	/* 小汽车抽象类 */
	VehicleFactory.Car=function(){
		this.type="car";
	};

	VehicleFactory.Car.prototype={
		getPrice: function(){
			return new Error('抽象方法不能调用');
		},
		getSpeed:function(){
			return new Error("抽象方法不能调用")
		},
	};
	/* 公交车抽象类 */
	VehicleFactory.Bus=function(){
		this.type="bus";
	};

	VehicleFactory.Bus.prototype={
		getPrice: function(){
			return new Error('抽象方法不能调用');
		},
		getPassengerNum:function(){
			return new Error("抽象方法不能调用")
		},
	};

	/*宝马汽车子类*/
	var BMW=function(price,speed){
		this.price=price;
		this.speed=speed;
	};
	VehicleFactory(BMW,'Car');
	BMW.prototype.getPrice=function(){
		return this.price;
	};
	BMW.prototype.getSpeed=function(){
		return this.speed;
	}
	/* 兰博基尼汽车子类 */
	var Lamborghini=function(price,speed){
		this.sprice=speice;
		this.speed=speed;
	};
	VehicleFactory(Lamborghini,'Car');
	Lamborghini.prototype.getPrice=function(){
		return this.price;
	};
	Lamborghini.prototype.getSpeed=function(){
		return this.speed;
	};
	var truck=new BMW(1000000,1000);
	console.log(truck.getPrice());
	console.log(truck.type);
```
## 建造者模式
* 用来创建属性、方法都不一样的。
* 下边的Human传的参数不懂。
* 这个继承方法有点绕，还需要把前两章多复习N遍。
```
	var Human=function(param){
		this.skill=param&&param.skill||'保密';
		this.hobby=param&&param.hobby||'保密';
	};
	Human.prototype={
		getSkill:function(){
			return this.skill;
		},
		getHobby:function(){
			return this.hobby;
		},
	};
	var Named=function(name){
		var that=this;
		(function(name,that){
			that.wholeName= name;
			if(name.indexOf(' ')>-1){
				that.FirstName = name.slice(0,name.indexOf(' '));
				that.secondName= name.slice(name.indexOf(' '));
			}
		})(name,that);
	};
	var Work=function(work){
		var that=this;
		(function(work,that){
			switch (work){
				case 'code':
					that.work = '工程师';
					that.workDescript= '每天沉醉于编程';
					break;
				case 'UI':
				case 'UE':
					that.work = '设计师';
					that.workDescript= '设计更似一种艺术';
				case 'teach':
					that.work = '教师';
					that.workDescript= '分享也是一种快乐';
				default :
					that.work = work;
					that.workDescript= '对不起，我们不知道你的描述。';
			}
		})(work,that);
	}
	Work.prototype.changeWork=function(work){
		this.work=work;
	}
	Work.prototype.changeDescript=function(setence){
		this.workDescript=setence;
	}
	var Person=function (name,work){
		var _person=new Human();
		_person.name=new Named(name);
		_person.work=new Work(work);
		return _person;
	}
	var person=new Person('xiao ming','UI');
	console.log(person.skill);
	console.log(person.getHobby())
	console.log(person.name.FirstName);
	console.log(person.work.work);
	console.log(person.work.workDescript);
	person.work.changeDescript('更改一下描述');
	console.log(person.work.workDescript);
```
## 原型模式
* 用原型实例指向创建对象的类，使用于创建新的对象的类共享原型对象的属性以及方法。
```
	var LoopImages=function(imgArr, container){
		this.imageArray=imgArr;
		this.container=container;
	};
	LoopImages.prototype={
		createImage: function(){
			console.log('LoopImages createImage function');
		},
		changeImage: function(){
			console.log('LoopImages changeImage function');
		},
	};
	var SlideLoopImg=function(imgArr,container){
		LoopImages.call(this,imgArr,container);
	};
	SlideLoopImg.prototype=new LoopImages();
	SlideLoopImg.prototype.changeImage=function(){
		console.log('SlideLoopImg changeImage function');
	};
	var FadeLoopImage=function(imgArr,container,arrow){
		LoopImages.call(this,imgArr,container);
		this.arrow=arrow;
	};
	FadeLoopImage.prototype=new LoopImages();
	FadeLoopImage.prototype.changeImage=function(){
		console.log('FadeLoopImage changeImage function');
	};
	var fadeImg=new FadeLoopImage(['1.jpg','2.jpg','3.jpg','4.jpg'],'slide',['left.jpg','right.jpg']);
	fadeImg.changeImage();
	var aa=new LoopImages();
	aa.changeImage();
```
* 原型的拓展，不管是基类还是子类，它们的原型都是共享的。是可以拓展的。
```
	LoopImages.prototype.getImageLength=function(){
		return this.imageArray.length;
	};
	FadeLoopImage.prototype.getContainer=function(){
		return this.container;
	}
	console.log(fadeImg.getImageLength());
	console.log(fadeImg.getContainer());
```
* 原型继承
```
	function prototypeExtend(aa,bb){
		var F=function(){};
		var i=0,
			args=arguments,
			len=args.length;
		for (; i<len; i++){
			for (var j in args[i]){
				F.prototype[j]=args[i][j];
			}
		}
		return new F();
	}
	var penguin=prototypeExtend({
		speed:20,
		swim: function(){
			console.log('游泳速度'+ this.speed);
		}
	},{
		run: function(speed){
			console.log('奔跑速度'+ speed);
		}
	},{
		jump:function(){
			console.log('跳跃动作');
		}
	});
	penguin.swim();
	penguin.run(10);
	penguin.jump();
```
## 单例模式：只允许实例化一次。
* 小型代码库
```
	var Tn={
		on:function(){
			console.log('on方法');
		},
		css:function(){
			console.log('css方法');
		},
		html:function(){
			console.log('html方法');
		},
	}
	Tn.on();
```
* 无法修改的静态变量
```
	var Conf=(function(){
		var conf={
			MAX_NUM: 100,
			MIN_NUM: 1,
			COUNT: 1000,
		};
		return {
			get: function(name){
				return conf[name]?conf[name]:null;
			}
		}
	})()
	var count=Conf.get('COUNT');
	console.log(count)
```
* 惰性单例
* 这样写有什么好处呢？没看懂。绕来绕去。还不是获取那个值么？贱人就是矫情
```
	var LazySingle=(function(){
		var _instance=null;
		function Single(){
			return {
				publicMethod: function(){},
				publicProperty: '1.0',
			}
		}
		return function(){
			if(!_instance){
				_instance=Single();
			}
			return _instance;
		}
	})()
	console.log(LazySingle().publicProperty)
```
## 外观模式
* 事件兼容模式
```
	function addEvent(dom,type,fn){
		if (dom.addEventListener) {
			dom.addEventListener(type,fn,false);
		}else if(dom.attachEvent){
			dom.attachEvent('on'+type,fn);
		}else{
			dom['on'+type]=fn;
		}
	};
	addEvent(document.getElementById('box'),'click',function(){
		console.log(1)
	})
	addEvent(document.getElementById('box'),'click',function(){
		console.log(2)
	})
```
* 阻止承认行为兼容和获取事件对象、获取元素兼容。
* 最后一个点击事件的判断很让人费解，为什么要判断不全等呢？
```
	var getEvent=function(event){
		return event||window.event;
	}
	var getTarget=function(event){
		var event=getEvent(event);
		return event.target||event.srcElement;
	}
	var preventDefault = function(event){
		var event=getEvent(event);
		if (event.preventDefault) {
			event.preventDefault();
		}else{
			event.returnValue=false;
		}
	}
	document.onclick=function(e){
		preventDefault(e);
		if (getTarget(e) !== document.getElementById('box')) {
			console.log(getTarget(e) )
			console.log(1)
			console.log(document.getElementById('box'))
		}
	}
```
* 小型代码库
```
	var Tn={
		getEvent:function(event){
			return event||window.event;
		},
		getTarget:function(event){
			var event=this.getEvent(event);
			return event.target||event.srcElement;
		},
		preventDefault : function(event){
			var event=this.getEvent(event);
			if (event.preventDefault) {
				event.preventDefault();
			}else{
				event.returnValue=false;
			}
		},
		getId:function(id){
			return document.getElementById(id);
		},
		css:function(id,key,value){
			this.getId(id).style[key]=value;
		},
		attr:function(id,key,value){
			this.getId(id)[key]=value;
		},
		html:function(id,html){
			this.getId(id).innerHTML=html;
		},
		on:function(id,type,fn){
			this.getId(id)['on'+type]=fn;
		},
	}
	Tn.css('box','background','red');
	Tn.html('box','内容');
	Tn.attr('box','class','box');// 不管用，看不懂。
	Tn.on('box','click',function(e){
		Tn.preventDefault(e);
		console.log(1)
	})
```
## 适配器模式
* 框架与框架之间适配。
```
// 相似的框架A 与jQuery适配。
window.A = A = jQuer;
```
* 参数适配器
```
	function(obj){
		var _adapter={
			name: '随便',
			title: '标题',
			age: 20,
			color: 'red',
			size: 100,
			prize: 50,
		}
		for (var i in _adapter){
			_adapter[i]=obj[i]||_adapter[i];
		}
	}
```
* 数据适配
```
	var arr=['zangsan','干嘛','标题','9/22'];
	function arrToObjAdapter(arr){
		return {
			name: arr[0],
			type: arr[1],
			title: arr[2],
			time: arr[3],
		};
	};
	console.log(arrToObjAdapter(arr));
```
## 代理模式 ***********************这个看了好久还是不理解*****************************
* src图片跨域。
```
	var Count=(function(){
		var _img=new Image();
		return function(param){
			var str="http://www.count.com/a.gif?";
			for (var i in param){
				str += i + '=' + param[i];
			}
			_img.src=str;
			console.log(str)
		}
	})()
	Count({num:10});
```
## 修饰者模式
* 在不改变原对象的基础上，通过对其分装拓展使原有对象可以满足用户的更复杂需求。
* 下边理解，先判断有没有绑定点击事件，没有，直接调用后边的回调函数，有，把旧的点击事件内容保存到一个函数，然后重新添加一个点击事件，调用旧的，再调用新的回调函数。
```
	var decorator=function(input,fn){
		var input=document.getElementById(input);
		if (input.onclick === 'function') {
			var oldClickFn=input.onclick;
			input.onclick=function(){
				oldClickFn();
				fn();
			}
		}else{
			input.onclick=fn;
		}
	}
	decorator(input,function(){
		input.style.display="none";
	})
```
## 桥接模式
* 就是把逻辑相似的代码打包一个函数，每次来实现调用即可。
```
	var spans=document.getElementsByTagName('span');
	function changeColor(dom,color,bg){
		dom.style.color=color;
		dom.style.background=bg;
	}
	spans[0].oumouseover=function(){
		changeColor(this,'#ddd','#fff');
	}
	spans[0].onmouseout=function(){
		changeColor(this,'#f00','#00f');
	}
```
## 组合模式
* 把对象组合成树状结构，通过一个主对象，一层一层往下继承。
* 这个回头再研究下。太多代码量。
```
	var News=function(){
		this.children = {};
		this.element = null;
	};
	News.prototype=function(){
		init: function(){
			throw new Error('请重写你的方法');
		},
		add: function(){
			throw new Error("请重写你的方法");
		},
		getElement: function(){
			throw new Error("请重写你的方法");
		},
	};
	var Container=function(id,parent){
		News.call(this);
		this.id=id;
		this.parent=parent;
		this.init();
	};

	// 下边这个函数是寄生组合式继承里定义的函数。
	// 把News的原型继承给了Container。
	inherPrototype(Container,News);
	Container.prototype.init=function(){
		this.element=document.createElement('ul');
		this.element.id=this.id;
		this.element.className='new-container'
	};
```
## 享元模式
* 将共有的属性方法提取出来，可以更好的加快速度。
```
// 代码省略，回头继续研究。
```
## 模板方法模式
* 定义一套模板，样式什么滴设置成参数。需要用到，直接继承。
```
	var data={
		content: '内容',
		confirm: '提交',
		width: 300,
		height: 200,
	};
	var Alert=function(data){
		if (!data) {
			return console.log(1);
		}
		this.content = data.content;
		this.panel = document.createElement('div');
		this.panel.style.width=data.width+'px';
		this.panel.style.height=data.height+'px';
		this.contentNode = document.createElement('p');
		this.confirmBtn = document.createElement('span');
		this.closeBtn = document.createElement('b');
		this.panel.className = 'alert';
		this.closeBtn.className = 'a-close';
		this.confirmBtn.className = 'a-confirm';
		this.confirmBtn.innerHTML = data.confirm || '确认';
		this.closeBtn.innerHTML = data.close || '关闭';
		this.contentNode.innerHTML = this.content;
		this.success=data.success || function(){};
		this.fail=data.fail || function(){};
	};
	Alert.prototype={
		init : function(){
			this.panel.appendChild(this.closeBtn);
			this.panel.appendChild(this.contentNode);
			this.panel.appendChild(this.confirmBtn);
			document.body.appendChild(this.panel);
			this.bindEvent();
			this.show();
		},
		bindEvent : function(){
			var me=this;
			this.closeBtn.onclick=function(){
				me.fail();
				me.hide();
			};
			this.confirmBtn.onclick=function(){
				me.success();
				me.hide();
			}
		},
		hide : function(){
			this.panel.style.display = 'none';
		},
		show : function(){
			this.panel.style.display = 'block';
		},
	};
	var RightAlert=function(data){
		Alert.call(this,data);
		this.confirmBtn.className=this.confirmBtn.className+'-right';
	};
	RightAlert.prototype=new Alert(data);
	var bb=new RightAlert(data);
	bb.init();
```
## 观察者模式
* 定义了一个观察者，里边有一个内容模块，注册模块，发步模块，移除模块。
* 感觉Observer.regist里边的else里边的push好像有问题。
```
	var Observer=(function(){
		var _messages={};
		return {
			regist : function(type,fn){
				if(typeof _messages[type] === 'undefined'){
					_messages[type]=[fn];
				}else{
					_messages[type].push(fn);
				}
				return this;
			},
			fire : function(type,args){
				if(!_messages[type]){
					return;
				}
				var events={
					type: type,
					args: args || {},
				},
					i=0,
					len=_messages[type].length;
				for (; i <len; i++) {
					_messages[type][i].call(this,events);
				}
			},
			remove: function(type,fn){
				if (_messages[type] instanceof Array) {
					var i = _messages[type].length-1;
					for ( ; i >= 0 ; i--){
						_messages[type][i] === fn && _messages[type].splice(i,1);
					}
				}
			}
		}
		
	})
	var observer=new Observer()
	observer.regist('aaa',function(e){
		console.log(e.type,e.args.msg);
	});
	observer.fire('aaa',{
		msg: '传递参数'
	})
```
* 第二个例子
```
<body>
	<ul id='msg'></ul>
	<textarea name="" id="user_input" cols="30" rows="10"></textarea>
	<input type="submit" id="user_submit">
	<p id="msg_num">0</p>
</body>
<script>
	var Observer=(function(){
		var _messages={};
		return {
			regist : function(type,fn){
				if(typeof _messages[type] === 'undefined'){
					_messages[type]=[fn];
				}else{
					_messages[type].push(fn);
				}
				return this;
			},
			fire : function(type,args){
				if(!_messages[type]){
					return;
				}
				var events={
					type: type,
					args: args || {},
				},
					i=0,
					len=_messages[type].length;
				for (; i <len; i++) {
					_messages[type][i].call(this,events);
				}
			},
			remove: function(type,fn){
				if (_messages[type] instanceof Array) {
					var i = _messages[type].length-1;
					for ( ; i >= 0 ; i--){
						_messages[type][i] === fn && _messages[type].splice(i,1);
					}
				}
			}
		}
		
	})
	var observer=new Observer()

	// 简易的获取ID元素
	function $(id){
		return document.getElementById(id);
	}
	// 工程师A
	(function(){
		function addMsgItem(e){
			var text = e.args.text,
				ul=$('msg'),
				li=document.createElement('li'),
				span=document.createElement('span');
			li.innerHTML=text;
			span.innerHTML='&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;删除';
			span.onclick=function(){
				ul.removeChild(li);
				observer.fire('removeCommentMessage',{
					num: -1,
				});
			}
			li.appendChild(span);
			ul.appendChild(li);
		}
		observer.regist('addCommentMessage',addMsgItem);
	})();
	// 工程师B
	(function(){
		function changeMsgNum(e){
			var num=e.args.num;
			$('msg_num').innerHTML= parseInt($('msg_num').innerHTML)+num;
		};
		observer
			.regist('addCommentMessage',changeMsgNum)
			.regist('removeCommentMessage',changeMsgNum);
	})();
	// 工程师C
	(function(){
		$('user_submit').onclick=function(){
			var text=$('user_input');
			if (text.value==='') {
				return ;
			}
			observer.fire('addCommentMessage',{
				text: text.value,
				num: 1,
			});
			text.value='';
		};
	})();
</script>
```
* 还是不懂，再来个例子
```
	var Observer=(function(){
		var _messages={};
		return {
			regist : function(type,fn){
				if(typeof _messages[type] === 'undefined'){
					_messages[type]=[fn];
				}else{
					_messages[type].push(fn);
				}
				return this;
			},
			fire : function(type,args){
				if(!_messages[type]){
					return;
				}
				var events={
					type: type,
					args: args || {},
				},
					i=0,
					len=_messages[type].length;
				for (; i <len; i++) {
					_messages[type][i].call(this,events);
				}
			},
			remove: function(type,fn){
				if (_messages[type] instanceof Array) {
					var i = _messages[type].length-1;
					for ( ; i >= 0 ; i--){
						_messages[type][i] === fn && _messages[type].splice(i,1);
					}
				}
			}
		}
		
	})
	var observer=new Observer();
	var Student=function(result){
		var that=this;
		that.result=result;
		that.say=function(){
			console.log(that.result);
		};
	};
	Student.prototype.answer=function(question){
		observer.regist(question,this.say);
	};
	Student.prototype.sleep=function(question){
		console.log(this.result+''+question+'已被注销')
		observer.remove(question,this.say);
	};
	Teacher=function(){};
	Teacher.prototype.ask=function(question){
		console.log('问题是：' +question);
		observer.fire(question)
	};
	var student1=new Student('学生1回答问题'),
		student2=new Student('学生2回答问题'),
		student3=new Student('学生3回答问题');
	student1.answer('什么是设计模式');
	student1.answer('简述观察者模式');
	student2.answer('什么是设计模式');
	student3.answer('什么是设计模式');
	student3.answer('简述观察者模式');
	student3.sleep('简述观察者模式');
	var teacher=new Teacher();
	teacher.ask('什么是设计模式');
	teacher.ask('简述观察者模式');
```
## 状态模式
* 先创建好状态，然后来调用
```
	var MarryState=function(){
		var _currentState={},
			states = {
				jump: function(){
					console.log('跳跃');
				},
				move: function(){
					console.log('移动');
				},
				shoot: function(){
					console.log('射击');
				},
				squat: function(){
					console.log('蹲下');
				},
			},
		 	Action = {
				changeState: function(){
					var arg=arguments;
					_currentState={};
					if (arg.length) {
						for (var i = 0 , len=arg.length;i<len;i++){
							_currentState[arg[i]]=true;
						}
					}
					return this;
				},
				goes: function(){
					console.log('触发一次动作');
					for (var i in _currentState){
						// 如果该对象有，则执行。
						states[i]&&states[i]();
					}
					return this;
				},
			};
		return {
			change: Action.changeState,
			goes: Action.goes,
		};
	}
	var marry=new MarryState;
	marry
		.change('jump','shoot')
		.goes()
		.goes()
		.change('shoot')
		.goes()
```
## 策略模式
* 
