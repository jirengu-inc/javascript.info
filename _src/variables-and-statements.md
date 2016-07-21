> 译者:周鹭江

## 变量和声明
JavaScript确实是一种特殊的语言。特别是对那些之前用过PHP、C、Java的人来说。
咱们以语言基础开始：变量、编码风格等。
### 代码结构
代码由多条语句构成。
#### 分号
使用分号隔开语句。
```
<html>
	<body>
	  <script>
	    alert('Hello');
	    alert('World!');
	  </script> 
	</body>
</html>
```
空格和 tab 都被忽略，所以可在一行代码中实现：
```alert('Hello');   alert('World!');```
但是换行符不被忽略，相反，**一个换行符可能分开语句，就像分号一样。**

下面两行代码效果完全相同：
```
a = 5
a = 5;
```
#### 缺少分号隐患

也有因为分号问题而产生一些错误，导致初学者使用多行任务调用。
首先，这个不会起作用：
```
var a = "long
  line "
```
因为解析器认为第一行是一个完整的语句，像：
```
var a = "long;
  line ";
```
将会有未完成的字符串错误（未结束的常量）。

二，这两段代码是相同的：
```
return
  result;
```
因为一个换行符，等价于
```
return;
result;
```
那当然和下面的不同：
```
return result
```
只有最后的例子实际上输出`result`。

要插入一个便捷的换行符，你可以在换行前加上反斜杠，像：
```
var a = "long \
	 line "
	 
	return \
	   result
```
**在换行前加入一个反斜杠，强制解释器忽略换行符。**

如果表达式还没结束的话，换行符也会被忽略，特别对未完成的运算符或未闭合括号。
```
var a = "long " +
	 " line "
	 
	var b = 2 + (
	 2 + 3
	)
```
JavaScript试着更加智能。下面例子输出的是8，换行符被忽略，别问我为什么。
```
var b = 2 * 2
	+ 4
	 
	alert(b)
```
关于分号的插入是复杂的，有时候还很奇怪，它们在 ECMAScript规范中描述了。

**总之，你可以在大多数情况下忽略结尾的分号，但这样的话，解释器就有更多发挥的空间，可能因为滥用分号而产生bug。**
> 分号：写还是不写？
> 在程序员间总是会有讨论：“我应该用分号呢?还是省略？”。目前，大部分人同意他们应该写分号。
> 在该教程中，你会发现很多代码没有以分号结尾，那只是我的坏习惯。
> 因为我才开始用JavaScript没多久，我经常产生不用分号的隐患，所以我知道应该如何解决。我猜你也不想重复我的路，而是跳开这个坑，所以记得把分号放在语句之间。

> 哪里出错了？

```
var a = "Empty spaces - what are we living for?
	Abandoned places - I guess we know the score..
	On and on!
	Does anybody know what we are looking for?"

	alert(a)
```
#### 解决方案
多行字符串在换行前必须使用反斜杠：
```
var a = "Empty spaces - what are we living for? \
	Abandoned places - I guess we know the score.. \
	On and on! \
	Does anybody know what we are looking for?"
	
	alert(a)
```
### 变量

如果你不熟悉常规编程变量的概念，这有个很棒的[wiki文章](https://en.wikipedia.org/wiki/Variable_(computer_science)) `[备注4]` 

长话短说，一个变量就是一个命名的“盒子”，你可以把值放进盒子里。

#### 定义

首先，一个变量应该被定义，那可以在代码的任何地方直接使用 `var`。
```
var x
```
当一个变量被定义后，才可以操作它，比如，将一个值赋给它：
```
var x
x = 5
```
或者你可以用一个`var`语句定义多个变量：
```
var a, b, c
```
也可以在定义中指定一个变量：
```
var name = "John", song = "La-la-la"
```
> 在JavaScript中，你可以指定一个还没有用 `var`定义过的变量：
``` noVar = "value" ```
从技术上讲，它不会产生错误，但不要这样做。始终使用 `var`定义变量，这是一个很好的方式，并且能帮忙规避某些错误，像下面的代码：
请在IE中运行：
```
<html>
	<body>
	<div id="test"></div>
	
	<script>
	 test = 5
	alert(test)
	</script>
	
	</body>
</html>
```
> 会出错。
IE浏览器、Safari、Chrome和Opera浏览器会为每个id元素创建一个变量，所以变量测试引用上面例子中的DIV。
但是在IE浏览器中，自动生成变量是一个不能改变的内部参考，这就是赋值给测试产生错误的原因。
以下代码有效：
```
<html>
	<body>
	<div id="test"></div>
	
	<script>
	 var test = 5
	alert(test)
	</script>
	
	</body>
</html>
```
#### 变量名

**一个变量名首字符应该是一个字母、`$` 或 `_`，第二个或者后面的字符可以使用数字。**
奇怪，但是有效命名:
```
var $this,
	_private,
	 $,
	 _,
	$1,
	user15
```
> JavaScript 是大小写敏感！
**Apple和APPLE是两个不同的名字。**

#### 保留字

另外还有些不能用作变量名的保留字。包括 `var`、`function`、`return`、`class`、`implements`和其他字，她们大部分被用于语言本身。
有些字，像`class`，并不用于现代JavaScript中，但仍然被保留。有些浏览器允许使用它们，但是使用它们可能会产生隐患。
下面的代码能在火狐浏览器中允许 `’class’`作为变量名运行，但它会在Safari因为这个变量名而给出语法错误：
```
var class = 5
	alert(class)
```
阅读更多关于命名准则，还有如何给变量取个好名字[变量命名]( http://javascript.info/draft/variable-naming)文章。

#### 语言类型

JavaScript定义了下面的基本类型：
**数字**
任何数字，整数与非整数：1、2、1.5等等
**字符串**
一个字符串，像 `’cat’`、`’dog’`或者 `’my mommie bought a puppy’`
**布尔**
两个可能值：true和false
**对象**
我们会在以后讨论它们。
**特殊值**
没有类型的特殊值：null和undefined

#### 弱类型
所有的变量在JavaScript中都是*弱类型*，意味着两样事：
1. 每一个值都有一个类型
2. 你可以在任意变量中加入任意类型的值。
比如：
```
var userId = 123;   // 123 is a number
var name = "John";  // "John" is a string
```
但你可以任意给变量赋到另一种类型的值：
```
var userId = 123;   // 123 is a number
userId = false;     // now userId is boolean
```
**在变量赋值前，它有 `undefined` 值。** 下面的语句是一样的：
```
var x
var x = undefined
```
大体上，`undefined` 意味着“没有值”。

### 注释
你是否还记得前面例子中的”//”？ 它们是用来注释的。
注释在JavaScript中有两种不同形式。单行评论以`//` 开始，持续到行的末尾。
```
// let's see who is here:
var name = "John"; // My most valued visitor
```
在`//`后输入的任何东西都会被解释器忽略。

#### 多行注释
多行注释附在`/*…*/`中，并可跨多行。
```
/*
The following variable has a short name.

Usually a short name means that the variable is
temporary and used only in nearest code.
*/
var a = "John";
```
#### 区块
下一个构建元素是一个 *block*，它是由包裹在花括号中的多种语句组成，像下面：
```
var i = 5;
{
  i = 6;
}
var b;
```
独立块从未在JavaScript中使用，但花括号内的语句作为更复杂语法构建中的一部分，像for、if、while、functions等等。

我们会在下面的章节中学习。
### 小结
一些要点概况：
* 变量是被 `var`定义在代码中的任意位置，它们可以在定义中被赋值。
* 一个变量可以包含任意类型的一个值。
* 单行注释 `//`和 多行注释 `/…*/ `
* 语句是被分号分隔的。一个换行符通常意味着一个分号，所以技术上来说，大部分时间可以忽略分号。对于是否使用分号还是省略存在争议，请睁开眼睛，选择你自己的方式。