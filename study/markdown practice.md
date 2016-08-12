# MarkDown语法
　　标题是每篇文章都需要也是最常用的格式，在 Markdown 中，如果一段文字被定义为标题，只要在这段文字前加 # 号即可。内容缩进直接写半方大的空白`&ensp;`或`&#8194;`全方大的空白`&emsp`;或`&#8195;`不断行的空白格`&nbsp;`或`&#160;`。或者使用全角在段落头部加两个空格ｌｉｋｅ　ｔｈｉｓ

# 标题
一级标题 可以使用#或者在下面加三个或者以上的=
===
二级标题  可以使用##或者在下面加三个或者以上的-
----
### 三级标题
#### 四级标题

## 代码片段  
代码区块 我习惯用一个TAB，PS，TAB后需要回车再输入（回车具有隔断上下关系的效果）

	public static void mian(String[] args){ 
		System.out.println("hello,world"); 
	}


	public void codeBlock(int i）{
		return null;	
	}


## 引用
当>和文字之间添加五个blank时，块注释的文字会有变化。）
>    *这是引用的方式加斜体*  
>>     引用内部再引用（中间有五个空格）

## 无序列表
+ djdf  
加点文字
* dfks
-  dfs
*  dfds

## 分割线
在一行中用三个或以上的**星号、减号、下划线**来建立一个分隔线，行内不能有其他东西。你也可以在星号中间插入空白。下面每种写法都可以建立分隔线： 
减号效果：
---
*号 和 下划线效果一样：
___
***
## 有序列表

1. 有序列表1
2. 有序列表2

## 换行
换行时候，文字末尾跟两个空格或以上   
换到下面

隔行两个回车。

## 图片表现方式

![亦菲表演机器猫](http://ww2.sinaimg.cn/bmiddle/88070423gw1ep30aw8an7g204d04gkgd.gif)  
注：Markdown不能设置图片大小，如果必须设置则应使用HTML标记来居中或者设置图片大小
<div align=center>
<img src="http://ww2.sinaimg.cn/bmiddle/88070423gw1ep30aw8an7g204d04gkgd.gif" width="400" height="400" alt="亦菲表演机器猫"/>
</div>

## 链接
Markdown支持两种风格的链接：Inline和Reference。  
语法：  
Inline：**以中括号标记显示的链接文本**，后面紧跟用小括号包围的链接。如果链接有title属性，则在链接中使用空格加"title属性"。  
Reference：**一般应用于多个不同位置使用相同链接**。好处是显而易见的，可以集中控制链接。不用通篇去找链接。集中管理嘛，缺点写起来稍微费劲点。
  
[**我是Inline Style示例链接哦**](http://www.baidu.com "百度title")

[这是一个Reference示例][reference]
[reference]: http://www.baidu.com ("我是标题")

## 自动链接
使用尖括号  <>  
<http://www.baidu.com>

## 转义字符  
Markdown中的转义字符为\，可以转义的有[^hello]：  
* \\\ 反斜杠  
* \\` 反引号  
* \\* 星号  
* \\_ 下划线  
* \\{\} 大括号  
* \\[\] 中括号  
* \\(\) 小括号  
* \\# 井号  
* \\+ 加号  
* \\- 减号  
* \\. 英文句号  
* \\! 感叹号

[^hello]:我是个脚注
