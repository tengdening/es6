### less语法
#### 变量
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
#### 混合
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
#### 嵌套
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
```
> 输出
```
#header{
	background: #f00;
	.box{
		font-size: 12px;
	}
	.right{
		float: right;
	}
}
```