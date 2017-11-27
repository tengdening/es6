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
* color :解析颜色，将代表颜色的字符串转换为颜色值。
```
	color('#888');
```
> 输出
```
	#888;
```
* image-size:从文件获取的图形尺寸。
```
	images-size('logo.png');
```
> 输出
```
	100px 100px;
```
* image-width:从文件获取的图像宽度。
```
	image-width('logo.png');
```
> 输出 <b>只能在node中运行</b>
```
	100px;
```
* image-height:从文件获取的图像高度。
```
	image-height('logo.png');
```
> 输出 <b>只能在node中运行</b>
```
	100px;
```
* convert :将数字从一个类型转化为另一个类型。
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
* 
```

```
> 输出
```

```
###
```

```
> 输出
```

```
###
```

```
> 输出
```

```
###
```

```
> 输出
```

```
###
```

```
> 输出
```

```



































###
```

```
> 输出
```

```