# String
> 译者: 魏旭

在JavaScript中字符串是目前最常用的一个类型。

在其他程序语言“字符”和“字符串”是有区别的。但在JavaScript中，只有字符串，没有字符。使得更加简单。

另一个特点是，内部所有字符串都是Unicode ，无论哪个编码。

## String 创建

String 对象通常用字符串字面量来创建：

```
var text = "my value";
var anotherText = 'another string';
var str = "012345";
```

在JavaScript中单引号和双引号并没有区别。

字符串字面量可以包含表示为转义序列、一个换行符号或者其他特殊字符。

### 特殊字符

这里有一些特殊字符和序列列表：

|字符|含义|
|:----:|:-----|
|\b|退格|
|\f|进纸|
|\n|换行|
|\r|回车|
|\t|制表|
|\uNNNN|以十六进制代码NNNN表示的一个Unicode字符。例如，\u00A9表示一个版权©符号|

值得注意的是，在JavaScript中字符串都是Unicode编码。

换行符是目前最常用的：

```
var multiLine = " first \n second \n third line "
alert(miltiLine)	//弹出3行
```

### 转义, 特殊字符

转义是通过前置反斜杠字符表示。

首先，如果单引号出现一个单引号字符串中，需要进行转义：

```
var str = 'I\'m the Valrus
```

在这种特殊情况下，双引号可以正常工作：

```
var str = " I'm the Valrus "
```

这同样适用于双引号。一个双引号字符串内的双引号需要被转义：

```
var str = " double \" quote "
```

反斜杠也需要转义才能显示：

```
var str = " '\\' "
alert(str)	//显示为： '\'
```

## 方法和属性

在JavaScript 字符串中有许多属性。让我们首先来讨论最重要的。

### 长度

每个字符串都有一个长度（1个unicode符号中的数字）。

```
var str = "My\n"	// 3 个字符。 第三个符号是一个换行符
alert(str.length)	// 3
```

### 查看字符

使用charAt方法来访问单个字符。第一个字符开始于0：

```
var str = "catty"
alert( str.charAt(0) )	// "c"
```

在JavaScript中没有character类型，所以charAt实际上返回正好包含一个字符的字符串。

在最新的浏览器中（不包括IE<8）你可以使用索引来访问字符：

```
var str = "I'm the modern browser!"
alert(str[0])	// "I"
```

### 操作字符串

在JavaScript中，字符串是不能被改变的。你可以读取一个字符，但你不能替换它。

通常的解决办法是改变一个字符串变量：创建一个新的字符串，并重新分配它来替代旧的，像下面的例子：

```
var str = "string"
str = str.charAt(2) + str.charAt(3) + str.charAt(4) + str.charAt(5)
alert(str) // ring
```

在这个例子中str.charAt(2)，charAt(3)... 方法用给定位置得到字符串和使用操作符"+"来合并（接合）它们。

### 小写/大写

方法 `toLowerCase()`和`toUpperCase()`可以改变整个字符串：

```
alert( "Hey-ho!".toUpperCase() )	// HEY-HO!
```

下面的代码获得的第一个字符并且是小写的。

`alert( "Hey-ho!".charAt(0).toLowerCase() )	// h`

创建一个方法`ucFirst(str)`用来返回第一个字符为大写的字符串：

`ucFirst("john") == "John"`

注意，在JavaScript没有一个方法可以这样做。请使用的charAt。

**解决方法：**

我们不能只替换第一个字符，因为在JavaScript中字符串是不可改变的。

唯一的方法是重新创建一个字符串：

```
function ucFirst(str) {
  var newStr = str.charAt(0).toUpperCase()
  for(var i=1; i<str.length; i++) {
    newStr += str.charAt(i)
  }
  return newStr
}
alert( ucFirst("john") )	// John
```

P.S.更好的解决方案可以使用substr方法或正则表达式。

### 查询字符串

为了找到一个子串，存在一个的[indexOf](https://developer.mozilla.org/en/JavaScript/Reference/Global_Objects/String/indexOf)方法。

它返回一个字符串的第一次出现的位置，如果没有发现则返回-1：

```
var str = "Widget with id"

alert( str.indexOf("Widget") ) // 0
alert( str.indexOf("id") ) // 1
alert( str.indexOf("Lalala") ) // -1
```

第二个参数是可选的，允许从一个索引值开始搜索：

例如，第一次出现“id”是在索引1的位置。所以，让我们找到其他的：

```
var str = "Widget with id"

alert( str.indexOf("id", 2) ) // 12, 从索引值2 开始搜索
```

也有一个类似的方法lastIndexOf用来从后搜索，从字符串的结尾开始。

在文章操作符章节，有一个按位非'～'操作符：~n和-（n + 1）是一样的。

该功能用来检查-1，因为~-1 == 0：

```
alert( ~-1 ) // 0
```

这个检查方法变成了`~indexOf`:

```
var str = "Widget"
if( ~str.indexOf("get") ) {
  alert('match!')
}	// match!
```

一般情况下，滥用以非显而易见的方法的语言功能是一件坏事，因为降低了可读性。

但在这里，一切都是没有问题的。请记住：'~读作“不是减一”，并且“if ～indexOf”读作“if found”。

创建一个方法 `checkSpam（str）`返回真，如果str含有“viagra”或“xxx”。

该功能应该不区分大小写：

```
checkSpam('buy ViAgRA now') == true
checkSpam('free xxxxx') == true
checkSpam("innocent rabbit") == false
```

**解决方案：**

要检查不区分大小写的方法，首先我们需要把str改为小写，然后再寻找（也要小写）字符串：

```
function checkSpam(str) {
  str = str.toLowerCase()
    
  return str.indexOf('viagra') >= 0 || str.indexOf('xxx') >= 0
}
```

该完整的解决方案在[tutorial/intro/checkSpam.html](http://javascript.info/play/tutorial/intro/checkSpam.html)

### 获取字符串：substr, substring, slice.

在JavaScript中，有3个（！）方法提取的字符串的一部分，但具有较小的差异。

**`substring(start [, end])`**

方法`substring(start, end)`中提取的子串，从开始到，但不包括位置结束。计数从0开始。

```
var str = "stringify"
alert(str.substring(0,1)) // "s"
```

如果end被省略了，它会到字符串的结尾：

```
var str = "stringify"
alert(str.substring(2)) // ringify
```

**`substr(start [, length])`**

第一个参数和substring的是一样的，但第二个参数是“有多少字符将被提取”，而不是结束位置。

```
var str = "stringify"
str = str.substr(2,4) // ring
alert(str)
```

同样的，省略第二个参数意味着直到字符串的末尾。

**`slice(start [, end])`**

返回字符串从位置为start ，但不包括位置结束。这就像substring一样。

在substring 和slice 它们之间的差异是如何对待负值和差异值：

**`substring(start, end)`**

负值变为零。过大的值变为字符串长度：

```
alert( "testme".substring(-2) )  // "testme", -2 变成 0
```

此外，如果start>end，则参数交换：

```
alert( "testme".substring(4, -1) )  // "test"
// -1 变成 0 -> 给我们的就是 substring(4, 0) 
// 4 > 0 这样参数会被交换 -> 则变成 substring(0, 4) = "test"
```

这种参数交换是一种反直觉的。但是感觉这个想法是得到两个索引之间的字符串。

**`slice`**

负值表示，从末尾往前：

```
alert( "testme".slice(-2) )  // "me", 从倒数第2个开始

alert( "testme".slice(1, -1) )  // "estm", 从第1个到倒数第1个
```

比起substring更加方便。

除了IE浏览器，都支持substr使用负索引值。

结论。

选择的方法是`slice(start, end)`。

或者，alternatively，`substr(start, length)`有着非负的start（在IE中负值不起作用）

写一个方法truncate（str，maxlength）用来检查字符串str的长度。

如果字符串str的长度超过maxlength，它会缩短str并且添加'...'，使长度等于maxlength。

返回的值是一个（可能）修正后的字符串

例如：

```
truncate("and here is what I want to say on that matter:", 20)) = "and here is what ..."
 
truncate("hi to all!", 20)) = "hi to all!"
```

这个功能能非常有用，并不只是一个工作，对于现实生活中截取用户指定的项目等。

**解决方案**

方案： [tutorial/intro/string/truncate.html](http://javascript.info/play/tutorial/intro/string/truncate.html)


##  对比

**(存在一点问题)**字符串是lexicographicaly比较。对于两个字符串不平等s1 > s2采用简单的算法进行检查：
1. 比较第一字符：a = s1.charAt（0）和 b = s2.charAt（0）。如果它们相等则继续比较下去，否则返回 > 或 <
2. 比较第二个字符等

该标准更精确地定义，虽然这点是清楚的（希望如此）：字符进行逐个比较。这令我们可以在一本字典或电话簿中看到的。

```
"Z" > "A" // true
"Bob" > "Bar" // true, 因为 o > a
"aa" > "a"  // true, 因为在比较中没有总比一个字符小
```

一探究竟：

```
alert("a" > "Z") // true, 原因小写字母位于浏览器的字符列表中的较高
```

### 字符串与数字

注意在字符串和数字的比较之间的行为差​​异：

```
alert(2 > 14);  // false
alert("2" > "14"); // true, 因为 "2" > "1" (第一个字符决定)
```

但要注意：

```
alert(2 > "14"); // false
```

这是因为，如果任一操作数不是字符串，那么两个操作数都成为数字，并比较变得正确。（存在一点点问题）

## 概要

需要知道：

- 如何写一个字符串，用特殊符号和引号。
- 如何比较字符串。
- 如何提取字符串的一部分。

除了级联这是由“+”操作​​符做的（存在一些问题），在大多数情况下这就是我们所需要的。

稍后我们将讨论正则表达式，在JavaScript中是非常重要的。

要了解字符串的其他方法，你可以浏览[Mozilla的手册](https://developer.mozilla.org/en/Core_JavaScript_1.5_Reference/Objects/String)。