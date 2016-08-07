> 译者: 曾安

在JavaScript中对象在三种环境下会被转换为基本类型：
1.Numeric（数字）类型
2.String（字符串）类型
3.Boolean（布尔）类型
理解类型转换的方式有助于避免可能出现的陷阱和写出更整洁的代码。
## String转换
String转换发生在需要一个对象的字符串表示时。
例如，在alert(obj)时是这样输出obj的：

	var obj = { name: 'John' }
	 
	alert(obj) // [object Object]
也可以用显式转换：String(obj)。
### 对象到String的转换算法
1.如果toString方法存在并且返回一个基本类型时，将返回这个基本类型。**执行会正常停止在这里，因为在默认情况下所有对象都存在toString方法。**
2.如果valueOf方法存在并且返回一个基本类型时，将返回这个基本类型。
3.否则，抛出异常。
此外，通常情况下所有对象都具有toString方法。内置对象有他们自己的toString实现：

	alert( {key: 'value'} ) // Object类的toString方法输出[object Object]
	alert( [1,2] )          // toString for Arrays lists elements "1,2"
	alert( new Date )       // Date类的toString方法输出字符串形式的日期
### 自定义的toString：
对于我们的对象，我们可以实现一个自定义的toString：

	var user = {
	 
	  firstName: 'John',
	 
	  toString: function() {
	    return 'User ' + this.firstName
	  }
	}
	 
	alert( user )  // User John
## Numeric转换
在JavaScript中有另一种转换，它没有toString那样广泛地为人所知，但是在内部它被调用得更加频繁。
Numeric转换在两种主要情况下进行：
1.在需要数字的函数中：比如Math.sin(obj)，isNaN(obj)，还包括算术运算符：+obj。
2.在比较中，类似obj == 'John'。例外情况是字符串相等===，因为它不会做任何类型转换，还有当两个参数是对象而不是基本类型的非严格相等时：obj1== obj2。只有当两个参数引用同一个对象时结果才为真。
也可以用Number(obj)实现显式转换。
Numeric转换的算法:
1.如果valueOf方法存在并且返回一个基本类型时，将返回这个基本类型。
2.否则，如果toString方法存在并且返回一个基本类型时，将返回这个基本类型。
3.否则，抛出异常。
在内置对象当中，Date同时支持Numeric转换和String转换：

	alert( new Date() ) // 人类可以读懂的date形式
	alert( +new Date() ) // 从1970年1月1日开始的微秒计时
但大多数对象不具有valueOf方法。这意味着，Numeric转换是通过toString方法处理的。
### 自定义的valueOf例子
那个神奇的方法valueOf可以像toString方法一样自定义：

	var room = {
	 
	  num: 777,
	 
	  valueOf: function() {
	    return this.num
	  }
	}
 
	alert( +room )  // 777
如果有一个自定义的toString方法，但没有valueOf，则解释器将使用它来进行Numeric转换：

	var room = {
	 
	  num: 777,
	 
	  toString: function() {
	    return this.num
	  }
	}
	 
	alert( room / 3 )  // 259
### Numeric转换和成为一个数字
Numeric转换并不意味着返回了一个数字。它必须返回一个基本类型但在它的具体类型上没有限制。
正因为如此，一个将对象转换为字符串的好方法是使用二进制加法：

	var arr = [1,2,3]
	 
	alert( arr + '' )
	// 首先尝试arr.valueOf(), 但是数组没有valueOf
	// 所以调用了arr.toString()并且返回了元素列表：'1,2,3'
由于历史的原因，new Date + ''也返回日期的字符串表示，尽管new Date有valueOf方法。这是一个例外。
其他的数学函数不仅执行Numeric转换，而且强制是一个数字类型。例如，一元增加+arr会给出NaN：

	alert( +[1,2,3] ) // [1,2,3] -> '1,2,3' -> 不是一个数字类型
## 在相等/比较测试中的转换
非严格相等和比较使用数字环境。
只有在和基本类型比较时，相等才会转换对象。
if (obj == true) { ... }
**在两个对象的相等检查时不会发生转换：obj1 == obj2只有当他们引用同一个对象时才为真。**
下面的比较总是会转换为基本类型：

	var a = {
	  valueOf: function() { return  1 }
	}
	var b  = {
	  valueOf: function() { return  0 }
	}
	 
	alert( a > b )  // 1 > 0, true
#### 问题
为什么下面的例子是真的？

	alert( ['x'] == 'x' )
#### 答案
这里左边是一个数组，右边是一个基本类型的值。
因此这样，数组将会发生一个数字类型的转换。这个数组没有valueOf方法，所以会使用toString方法。
数组的toString方法默认实现是以逗号为分隔列出它的值：

	alert( ['a','b'] + '' )   // 'a,b'
因为这个数组只有一个单一的值，所以['x']变成'x'。
附：
相同的逻辑导致：
['x','y'] == 'x,y'
[] == ''
## Boolean（布尔）环境
在JavaScript中还有一个标准的转换，在规范里被称为[[toBoolean]。
它发生在Boolean环境下，就像if(obj)、while(obj)等。
对象可能不会自己实现这样的转换，也没有神奇的方法。取而代之的，有一个转换的硬编码表：
| 值          | 转换后…           | 
| ------------- |:-------------:| 
| true/false          | 不会发生转换 | 
| undefined, null  | false | 
| Number     | 0, NaN变成false, 其他true | 
| String        | "" 变成false, 其他 - true    |
| Object       | true  |
不像许多编程语言（比如PHP），"0"是在JavaScript为真。
在下面的例子中，我们有数字转换（相等使其发生）：

	alert( [0] == 0 )  // true
	alert( "\n0\n" == 0 ) // true
	alert( "\n0\n" == false ) // true
因此，人们可能会猜测，[0]和"\n0\n"是假值，因为他们和0相等。
但是，现在让我们看看在Boolean环境下左边部分的表现：

	if ([0]) alert(1)  // 1, 如果认为[0]为true
	if ("\n0\n") alert(2) // 2, 如果认为"\n0\n"为true
**a == b但在Boolean环境下a为真b为假，这是可能的。**
### 一种可以用来吓唬Java程序员的方法。
要将一个值转换为布尔值，你可以使用双否定：!!val或者直接调用Boolean(val)。
当然，我们在任何目的下都从来不使用new Boolean。如果我们这样做，将会有有趣的事情发生。
例如，让我们尝试从零得到一个布尔值：

	alert( new Boolean(false) ) // false
但是...

	if ( new Boolean(false) ) {
	  alert(true) // true
	}
这是因为new Boolean是一个对象。alert会将其转换为字符串，然后它变为了"false"...对的，就是这样！
但是，如果将其转换为布尔基本类型，这里的任何对象都为true...
当Java程序员看到这一点，通常他们的眼睛简直要蹦出来了。
#### 问题
为什么他们相等？

	alert( [] == ![] ) // true
#### 答案
1.首先，对比较的两边进行评估。右侧是![]。逻辑非'!'将其转换为布尔值。根据上文中的表，[]为true。因此，右侧变成了![] = !true = false。这给我们带来了：
[] == false
2.一个对象和一个基本类型之间的相等性检查会以数字的方式将对象转换为基本类型。
该数组没有valueOf方法，但它有toString方法将其转换为以逗号分隔的元素列表。在我们的例子中，没有元素会导致空字符串：
'' == false
3.不同基本类型的比较并将其转换为数字：
0 == 0
现在的结果是显而易见的。
#### 练习
弄清楚表达式的结果。当你完成后，查看解决方案后再次检查。

	6 / "3"
	"2" * "3"
	4 + 5 + "px"
	"$" + 4 + 5
	"4" - 2
	"4px" - 2
	7 / 0
	{}[0]
	parseInt("09")
	5 && 2
	2 && 5
	5 || 0
	0 || 5
#### 答案

	6 / "3" = 2
	"2" * "3" = 6
	4 + 5 + "px" = "9px"
	"$" + 4 + 5 = "$45"
	"4" - 2 = 2
	"4px" - 2 = NaN
	7 / 0 = Infinity
	{}[0] = undefined
	parseInt("09") = "0" or "9" // 八进制或十进制，取决于浏览器
	5 && 2 = 2
	2 && 5 = 5
	5 || 0 = 5
	0 || 5 = 5
## 小结
在JavaScript中有三种转换，这取决于具体的环境：
1.String: 输出, 使用toString方法.    
2.Numeric: 算数, 操作, 使用valueOf -> toString.
3.Boolean: 根据上文中的表转换.
这是不同于其他大多数编程语言，但是当你明白了后是十分简单的。
附：实际上，转换是有点比这里描述的更复杂。我省略了一些复杂情况而专注于它到底是如何工作的。
为了一个最大精确的转换算法，请参考规范：ECMA-262第5版，尤其11.8.5（关系比较），和11.9.3（相等比较）和9.1（toPrimitive）和9.3（toNumber）。