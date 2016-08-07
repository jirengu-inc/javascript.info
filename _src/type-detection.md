> 译者: 金晓云

#类型检测
`注：有背景色的为注释，非原文内容`

1. Typeof运算符
    1.原始值
    2.typeof的正确用法
    3.typeof的错误用法

2.[[class]]区分本地对象

3.检查自定义对象的类型

4.综述


*多态函数*是一个很好的编程概念，这种函数对不同类型参数的处理方式不同。

为了支持多态性，我们需要一种获得变量类型的方法。这里是一个小型的JS动物园`[这里的zoo翻译需确认]`，让我们来看看吧。

众所周知，Javascript中有许多原始类型：
**null**，**undefined**
　　特殊值。
　　
**number**
　　所有数字，如**0**和**3.14**，同样包括伪数值**NaN**和**Infinity**

**boolean**
　　**True**和**false**
**string**
　　字符串，如“Meow”和“”。

#Typeof运算符
其他值均为**对象**。举例，如函数和数组都是对象。
typeof运算符返回一个值的数据类型。有以下两种可行的语法：
1.运算符：typeof x
2.类函数：typeof(x)
以上两种语法作用相同，但第一种更为简短。
```javascript
typeof undefined // "undefined"
typeof 0  //"number"
typeof true   //"boolean"
typeof "foo"  //"string"
typeof {} //"object"
typeof null //"object"
typeof function(){} //"function"
typeof NaN //"number"
```
可以发现最后三行为红色。`[上述代码少了红色背景]`原因如下。
1.第一， typeof null == "object"是这个语言的一个官方错误，是为了兼容性性而从90x`[不明白90x什么意思]`保持的。
我们可以确认：
```javascript
var x = null
x.prop = 1 // error, can't assign a property to primitive
```
2.尽管对函数进行特殊对待，但仍然属于对象。
3.typeof NaN == 'number'十分可笑，因为NaN是'Not-A-Number'的缩写。

#原始值
typeof在处理原始值(null除外)的效果很好。但是它没有对对象类型进行任何的说明。
**typeof 不能区分对象**
举个例子，Object,Array和Date对象得到的结果相同。
```javascript
alert( typeof {} ) // 'object'
alert( typeof [] ) // 'object'
alert( typeof new Date ) // 'object'
```
我们也可以发现原始字符串和new String之间的不同。
```javascript
alert( typeof "lalala" ) // string
alert( typeof new String("lalala") ) // object
```
##typeof 的正确用法
让我们写一个多态函数。
```javascript
function f(x) {
 if (typeof x == 'function') {
    ... // in case when x is a function
  } else {
    ... // in other cases
  }
}
```
这是一个合理的使用。

##typeof 的错误用法

与现在的模式相反。在以前的代码中可以发现利用typeof来测试已经存在的变量：
```javascript
// check if global variable jQuery exists
if(typeof(jQuery) !== 'undefined') ...
```
这里使用typeof是因为直接使用一个未定义的变量会出错：
```javascript
// error if jQuery is not defined
if(jQuery) { .. }
```
**千万别用这种方式使用typeof**。
一个全局变量可以作为window对象内置的属性来访问。让我们来确认一下：
```javascript
if(window.jQuery !== undefined) { ... }
```
这样不会发生错误，因为没人会要求（可能未定义）jQuery。我们仅仅要求对象的属性。
在大多数情况下，比如这里，我们知道jQuery如果存在的话不可能是错误的，所以检测可以简写为：
```javascript
if(window.jQuery) { ... }
```
**typeof不能用于检测已经存在的变量**

##[[class]]区分原生对象

typeof 的主要问题在于它不告诉我们对象的更多信息。function是个例外。
```javascript
alert( typeof {key:'val'} ) // Object is object
alert( typeof [1,2] ) // Array is object
alert( typeof new Date ) // Date object
```
幸运的是，Javascript原生对象中有一个隐藏的[[class]]属性。它等同于"Arrays"对应arrays,"Date"对应dates等。
这个属性不能直接访问，但是可以借用本地对象的toString方法返回一个小数据包`[小数据包？哈哈]`。
```javascript
var toClass = {}.toString // (1)
alert( toClass.call( [1,2] ) ) // [object Array]
alert( toClass.call( new Date ) ) // [object Date]
```
这里我们是这样做的：
1.把一个对象的toString赋值给toClass.
2.提供我们需要检查的对象并调用这个方法.所以，对象toString开始应用于数组、日期等。我们必须这么做，因为他们自身toString行为不同。对于数组而言，返回一个元素列表，日期类型的则返回对应的字符串。
这个方法同样适用于原始值：
```javascript
alert( toClass.call(123) ) // [object Number]
alert( toClass.call("primitive") ) // [object String]
```
null和undefined除外，因为通过null和undefined调用call把window传递给了this。

对于自定义对象，[[class]]为"Object"：
```javascript
function Animal(name) { 
  this.name = name
}
var animal = new Animal("Goofy")
var class = {}.toString.apply( animal )
alert(class) // [object Object]
```
所以，这只能用于本地对象。

##自定义对象的类型检查
通过构造函数创建的对象，我们通常使用instanceof 操作符。
语法为 obj instanceof Func:
```javascript
function Animal(name) { 
  this.name = name
}
var animal = new Animal("Goofy")
alert( animal instanceof Animal ) // true
```
更多讨论见文章["instanceof"操作符](http://javascript.info/tutorial/instanceof#ref-instanceof)

目前，对于本地对象而言，[[class]]方法比较好并且被应用于大多数的框架。

为什么不使用"arr instanceOf Array"?
我们可以通过instanceof运算符检测一个数组：
```javascript
var arr = [1,2,3]
alert(arr instanceof Array) // true
```
尽管如此，如果arr是在别的窗口或者内嵌框架(iframe)创建后再传递进来的，instanceof将不起作用。这是因为每个窗口都有自己的对象层次。
为了解决这个问题，对于本地对象，大多数框架使用[[class]]。

##综述
typeof
    适用于原始类型和函数，不适用于null.
[[class]]
    通过{}.toString暴露。适用于内置对象和原始类型，null和undefined除外。
instanceof
    适用于自定义对象。也可用于本地对象，但不适用于从其他窗口或内嵌框架(iframe)传递的本地对象。