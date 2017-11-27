## less语法
### 变量
```
	@red:#f00;
	@pink: @red+#099;
	#header{
		background: @pink;
	}
```
> 输出
```
	#header{
		background: #f99;
	}
```
### 混合
```
	.bordered{
		border-bottom: 1px solid #ddd;
		border-top: 1px solid #ddd;
	}
	#header{
		background: #f00;
		.bordered;
	}
```
> 输出
```
	.bordered{
		border-bottom: 1px solid #ddd;
		border-top: 1px solid #ddd;
	}
	#header{
		background: #f00;
		border-bottom: 1px solid #ddd;
		border-top: 1px solid #ddd;
	}
```
### 嵌套
```
	#header{
		background: #f00;
		.box{
			font-size: 12px;
		}
		.right{
			float: right;
		}
		&:hover{
			background: #0f0;
		}
	}
```
> 输出
```
	#header{
		background: #f00;
	}
	#header .box{
		font-size: 12px;
	}
	#header .right{
		float: right;
	}	
	#header:hover{
		background: #0f0;
	}
```
### 媒体嵌套
```
#header{
	@media screen{
		color: green;
		@media (min-width: 768px){
			color: red;
		}
	}
	@media tv{
		color: black;
	}
}
```
> 输出
```
@media screen{
	#header{
		color: green;
	}
}
@media screen and (min-width: 768px){
	#header{
		color: red;
	}
}
@media tv{
	#header{
		color: black;
	}
}
```
### 运算
```
	@base: 5%;
	@baseTwo: @base*2;
	@baseThree: @base + @baseTwo;
	.color{
		color : #888 / 4;
		background-color: #888 - #333;
		height: 100% / 2 + @baseTwo;
	}
	@val :1px +5;
	#header{
		border-width: @val;
	}
```
> 输出
```
	.color{
		color: #222;
		background-color: #555;
		height: 60%;
	}
	#header{
		border-width: 6px;
	}
```
### 函数
#### 杂项函数
* color: 解析颜色,将代表颜色的字符串转换为颜色值。
```
	color('#888');
```
> 输出
```
	#888;
```
* image-size: 从文件获取的图形尺寸。
```
	images-size('logo.png');
```
> 输出
```
	100px 100px;
```
* image-width: 从文件获取的图像宽度。
```
	image-width('logo.png');
```
> 输出 <b>只能在node中运行</b>
```
	100px;
```
* image-height: 从文件获取的图像高度。
```
	image-height('logo.png');
```
> 输出 <b>只能在node中运行</b>
```
	100px;
```
* convert: 将数字从一个类型转化为另一个类型。
```
	convert(9s,'ms');
	convert(14cm,'mm');
	convert(8,'mm');
```
> 输出 <b>只能在node中运行</b>
```
	9000ms
	140mm
	8
```
* data-uri: 将一个资源内嵌到样式文件,如果开启了ieCompat选项,而且资源文件的体积过大,或者是在浏览器中使用,则会使用url()进行回退。如果没有指定MIME,则Node.js会使用MIME包来决定正确的MIME。
> MIME: 一种文件类型。它的格式是：类型/子类型。如：image/jpeg、image/svg+xml
```
	data-uri('../data/image.jpg');
	data-uri('image/jpeg;base64','../data/image.jpg');
	data-uri('image/svg+xml;charset=UTF-8','image.svg');
```
> 输出
```
	url('data:image/jpeg;base64,bm90IGFjdHVhbGx5IGEganBlZyBmaWxlCg==');
	url('data:image/jpeg;base64,bm90IGFjdHVhbGx5IGEganBlZyBmaWxlCg==');
	url('data:image/svg+xml;charset=UTF-8,%3Csvg%3E%3Ccircle%20r%3D%229%22%2F%3E%3C%2Fsvg%3E')
```
* defalut: 只能边界条件中使用,没有匹配到其他自定义函数的时候,返回true,否则返回false。
```
	.aa(1){x:11};
	.aa(2){y:22};
	.aa(@x)when(defalut()){z:@x};
	div{
		.aa(2);
	}
	span{
		.aa(8)
	}

	/*<b>ispixel</b>传入的参数如果是带单位返回ture,否则返回false。*/
	/*除去自己至少还有一条才可以执行.bb() when not(default()) {}。*/
	.bb(@value) when (ispixel(@value)) {width: @value}
	.bb(@value) when not(default())    {padding: (@value / 5)}
	.div-1 {
	  .bb(100px);
	}

	.x {
	  .m(red)                                    {case-1: darkred}
	  .m(blue)                                   {case-2: darkblue}
	  .m(@x) when (iscolor(@x)) and (default())  {default-color: @x}
	  .m('foo')                                  {case-1: I am 'foo'}
	  .m('bar')                                  {case-2: I am 'bar'}
	  .m(@x) when (isstring(@x)) and (default()) {default-string: and I am the default}

	  &-blue  {.m(blue)}
	  &-green {.m(green)}
	  &-foo   {.m('foo')}
	  &-baz   {.m('baz')}
	}
```
> 输出
```
	div{
		y: 22;
	}
	span{
		z: 8;
	}

	.div-1{
		width: 100px;
		padding: 20px;
	}

	.x-blue {
	  case-2: #00008b;
	}
	.x-green {
	  default-color: #008000;
	}
	.x-foo {
	  case-1: I am 'foo';
	}
	.x-baz {
	  default-string: and I am the default;
	}
```
* unit: 移除或者改变属性值的单位。
```
	unit(5px,mm);
	unit(5px);
	unit(5,px)
```
> 输出
```
	5mm;
	5;
	5px
```
* get-unit: 返回一个值得单位。
```
	get-unit(5px);
	get-unit(5)
```
> 输出
```
	px;
	//nothing,也就是什么也不输出。
```
* svg-gradient: 生成多停靠的svg渐变。
> 可以传入三个参数,第一个方向,第二个颜色,第三个位置。方向必须是to bottom, to right, to bottom right, to top right, ellipse 或 ellipse at center 。
```
	p {
	  height: 20px;
	  background-image: svg-gradient(to right, red 10%, green 15%, blue);
	}
```
> 输出
```
	p {
	  height: 20px;
	  background-image: url('data:image/svg+xml,%3C%3Fxml%20version%3D%221.0%22%20%3F%3E%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20version%3D%221.1%22%20width%3D%22100%25%22%20height%3D%22100%25%22%20viewBox%3D%220%200%201%201%22%20preserveAspectRatio%3D%22none%22%3E%3ClinearGradient%20id%3D%22gradient%22%20gradientUnits%3D%22userSpaceOnUse%22%20x1%3D%220%25%22%20y1%3D%220%25%22%20x2%3D%22100%25%22%20y2%3D%220%25%22%3E%3Cstop%20offset%3D%2210%25%22%20stop-color%3D%22%23ff0000%22%2F%3E%3Cstop%20offset%3D%2215%25%22%20stop-color%3D%22%23008000%22%2F%3E%3Cstop%20offset%3D%22100%25%22%20stop-color%3D%22%230000ff%22%2F%3E%3C%2FlinearGradient%3E%3Crect%20x%3D%220%22%20y%3D%220%22%20width%3D%221%22%20height%3D%221%22%20fill%3D%22url(%23gradient)%22%20%2F%3E%3C%2Fsvg%3E');
	}
```
#### 字符串函数
* escape(转义函数): 适用URL编码到输入字符串中发现的特殊字符。
> 不转义编码的字符：,/？@&+'~!$
```
	escape('a=1')
```
> 输出
```
	a%3D1
```
* e: css转义,用~符号代替。
>> 看不懂的函数。
```
	div{
		color: e('blue');
	}
```
> 输出
```
	div{
		color: blue;
	}
```
* %format: 函数 %(string, arguments ...) 格式化一个字符串。
>>看不懂的函数。
```
	format-a-d: %("repetitions: %a file: %d", 1 + 2, "directory/file.less");
	format-a-d-upper: %('repetitions: %A file: %D', 1 + 2, "directory/file.less");
	format-s: %("repetitions: %s file: %s", 1 + 2, "directory/file.less");
	format-s-upper: %('repetitions: %S file: %S', 1 + 2, "directory/file.less");
```
> 输出
```
	format-a-d: "repetitions: 3 file: "directory/file.less"";
	format-a-d-upper: "repetitions: 3 file: %22directory%2Ffile.less%22";
	format-s: "repetitions: 3 file: directory/file.less";
	format-s-upper: "repetitions: 3 file: directory%2Ffile.less";
```
* replace(替换): 用另一个字符串替换文本。
> *string: 搜索和替换用的字符串。
> *pattern: 一个字符串或者能搜索的正则表达式。
> *replacement: 匹配成功的替换字符串。
> *flags: (可选的) 正则表达式匹配标识（全匹配还是...）
```
	replace("Hello, Mars?", "Mars\?", "Earth!");
	replace("One + one = 4", "one", "2", "gi");
	replace('This is a string.', "(string)\.$", "new $1.");
	replace(~"bar-1", '1', '2');
```
> 输出
```
	"Hello, Earth!";
	"2 + 2 = 4";
	'This is a new string.';
	bar-2;
```
#### 列表函数
* length: 返回集合中的值的数目。
> 参数：逗号或者空格隔开的值集合
```
	@list: "banana", "tomato", "potato", "peach";
	n: length(@list);
```
> 输出
```
	n: 4;
```
* extract(提取): 返回集合中指定索引的值。
> 参数1：逗号或者空格隔开的值集合。参数2：索引值
```
	@list: "banana", "tomato", "potato", "peach";
	n:extract(@list,2);
```
> 输出
```
	n:tomato;
```
#### 数学函数
* ceil: 向上取整。
```
	ceil(2.4);
```
> 输出
```
	3;
```
* floor: 向下取整
```
	floor(2.9);
```
> 输出
```
	2;
```
* parcentage: 将浮点数转换为百分比字符串。
```
	parcentage(0.5);
```
> 输出
```
	50%;
```
* round: 四舍五入。
> 参数1：浮点数。参数2：保留几位小数。
```
	round(1.65666);
	round(1.65566,3);
```
> 输出
```
	2;
	1.656;
```
* sqrt: 计算一个数的平方根,原样保持单位。
```
	sqrt(25cm);
	sqrt(18.6%);
```
> 输出
```
	5cm;
	4.312771730569565%;
```
* abs: 计算数中的绝对值,原样保持单位。
```
	abs(25cm);
	abs(-15%);
```
> 输出
```
	25cm;
	15%;
```
* sin: 正弦函数。
```
	sin(1); // 1弧度角的正弦值
	sin(1deg); // 1角度角的正弦值
	sin(1grad); // 1百分度角的正弦值
```
> 输出
```
	0.8414709848078965; // 1弧度角的正弦值
	0.01745240643728351; // 1角度角的正弦值
	0.015707317311820675; // 1百分度角的正弦值
```
* asin: 反正弦函数。
> 参数number - 区间在 [-1, 1] 之间的浮点数。
```
	asin(-0.8414709848078965);
	asin(0);
	asin(2);
```
> 输出
```
	-1rad;
	0rad;
	NaNrad;
```
* cos: 余弦函数。
```
	cos(1) // 1弧度角的余弦值
	cos(1deg) // 1角度角的余弦值
	cos(1grad) // 1百分度角的余弦值
```
> 输出
```
	0.5403023058681398 // 1弧度角的余弦值
	0.9998476951563913 // 1角度角的余弦值
	0.9998766324816606 // 1百分度角的余弦值
```
* acos: 反余弦函数。
> 参数： number - 区间在 [-1, 1] 之间的浮点数。
```
	acos(0.5403023058681398)
	acos(1)
	acos(2)
```
> 输出
```
	1rad
	0rad
	NaNrad
```
* tan: 正切函数。
```
	tan(1) // 1弧度角的正切值
	tan(1deg) // 1角度角的正切值
	tan(1grad) // 1百分度角的正切值
```
> 输出
```
	1.5574077246549023   // 1弧度角的正切值
	0.017455064928217585 // 1角度角的正切值
	0.015709255323664916 // 1百分度角的正切值
```
* 
```

```
> 输出
```

```
* atan: 反正切函数。
```
	atan(-1.5574077246549023)
	atan(0)
	round(atan(22), 6) // 22四舍五入保留6位小数后的反正切值
```
> 输出
```
	-1rad
	0rad
	1.525373rad;
```
* pi: 返回π。
```
	pi()
```
> 输出
```
	3.141592653589793
```
* pow: 乘方运算。
> 假设第一个参数为A,第二个参数为B,返回A的B次方。返回值与A有相同单位,B的单位被忽略。
```
	pow(0cm, 0px)
	pow(25, -2)
	pow(25, 0.5)
	pow(-25, 0.5)
	pow(-25%, -0.5)
```
> 输出
```
	1cm
	0.0016
	5
	NaN
	NaN%
```
* mod: 取余运算。
> 假设第一个参数为A,第二个参数为B,返回A对B取余的结果。返回值与A有相同单位,B的单位被忽略。这个函数也可以处理负数和浮点数。
```
	mod(0cm, 0px)
	mod(11cm, 6px);
	mod(-26%, -5);
```
> 输出
```
	NaNcm;
	5cm
	-1%;
```
* min: 最小值运算。
```
	min(5,10,9,2);
	min(10px,11px,5px,9px);
```
> 输出
```
	2;
	5px;
```
* max: 最大值运算。
```
	max(5,10,9,2);
	max(3%, 42%, 1%, 16%);
```
> 输出
```
	10;
	42%;
```
#### 类型函数
* isnumber: 如果一个值是数字,返回true,否则返回fasle。
```
	isnumber(#f00);
	isnumber(100);
	isnumber(55px);
	isnumber(18%);
	isnumber(100,99)
```
> 输出
```
	false;
	true;
	true;
	true;
```
* iscolor: 如果一个值是颜色,返回true,否则返回false。
```
	iscolor(#fee);
	iscolor(red);
	iscolor(ljlkjlkjlk);
```
> 输出
```
	true;
	true;
	false;
```
* iskeyword: 如果一个值是关键字,返回true,否则返回false。
```
	iskeyword(keyword);
	idkeyword(526569);
```
> 输出
```
	true;
	false;
```
* isurl: 如果一个值是url地址,返回true,否则返回false。
```
	isurl(url(....));
	isurl(545454);
```
> 输出
```
	true;
	false;
```
* ispixel: 如果一个值是带像素长度单位的数字,返回true,否则返回false。
```
	ispixel(15px);
	ispixel(154154);
```
> 输出
```
	true;
	false;
```
* isem: 如果一个值是带em长度单位的数字,返回true,否则返回false。
```
	isem(10em);
	isem(26546);
```
> 输出
```
	true;
	false;
```
* ispercentage: 如果一个值是百分比单位的数字,返回true,否则返回false。
```
	ispercentage(50%);
	ispercentage(56565);
```
> 输出
```
	true;
	false;
```
































###
```

```
> 输出
```

```