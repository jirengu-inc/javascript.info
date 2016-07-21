# 函数参数
> 译者: 金晓云

1. 访问未命名参数
2. arguments的核心
3. 像数组一样使用参数对象arguments
4. 构造一个真正的数组
5. 默认值
6. 关键字参数
7. arguments的特殊属性
   1.arguments.callee
   2.arguments.callee.caller

在JavaScript中，不论函数定义时列出几个参数，在被调用时都可以传任何数量的参数。
例如：
```javascript
function go(a,b) {
  alert("a="+a+", b="+b)
}
go(1)     // a=1, b=undefined
go(1,2)   // a=1, b=2
go(1,2,3) // a=1, b=2, extra argument is not listed
```
函数调用时没提供的参数被认为是未定义的；所以我们看以下函数是否是无参数的情况下被调用：
```javascript
function check(x) {
  alert(x === undefined) // ok now I know there is no x
}
 check()
```
没有语法上的多态性
在一些语言中，程序员可以写2个名字相同而参数列表不同的函数，而解释器/编译器会自动匹配正确的函数：
```javascript
function log(a) {
  ...
}
function log(a,b,c) {
  ...
}
log(a) // first function is called
log(a,b,c) // second function is called
```
这个称为函数的多态性。在JavaScript中，并没有这回事。
只会有一个名字为log的函数，不论调用时传入任何参数，都只会调用那一个。

**访问未命名参数**
那我们如何获得比参数中所列参数个数更多的参数呢？
在每个函数里面有一个特殊的伪数组，叫做[arguments](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/arguments)。可用数字标识到arguments中包含的所有参数，比如arguments[0], arguments[1]等。
这里是一个例子，不论函数有多少参数，都可以列出来：
```javascript
function sayHi() {
  for(var i=0; i<arguments.length; i++) {
    alert("Hi, " + arguments[i])
  }
}
sayHi("Cat", "Alice")  // 'Hi, Cat', then 'Hi,
Alice'
```
所有的参数都包含在arguments对象中，尽管其中的一些函数名称形如：sayHi(a,b,c)。

**arguments的核心**
一个初学者经常犯的错误就是在arguments上使用数组对象的方法，简单的说，你不能如下使用：
```javascript
function sayHi() {
  var a = arguments.shift() // error! arguments is
not Array
  alert(a)
}
sayHi()
```
arguments并不是数组，那它是什么呢？让我们使用[[Class]]属性来看一下：
```javascript
(function() {
  alert( {}.toString.call(arguments) )  // [object
Arguments] 
})()
```
类型几乎和对象一样，它只是看起来是数组，因为它的键（keys）是数字而且有length，然而没有其他相似处了。

**像数组一样使用arguments**
依然有一个方法可以在arguments对象上调用数组的方法：
```javascript
function sayHi() {
  var args = [].join.call(arguments, ':')
  alert(args)  // 1:2:3
}
sayHi(1,2,3)
```
这里我们在arguments上执行数组的[join](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/join)方法，“：”作为第一个参数。方法可以运行，因为内置的大多数数组方法（包含join）都支持数字索引和length。如果格式正确，join方法也可以使用在一个自定义对象上：
```javascript
var obj = { 0: "A", 1: "B", length: 2 }
 alert( [].join.call(obj) ) // "A,B"
```
**构造一个真正的数组**
[arr.slice(start,end)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice)可以把arr从start到end的部分复制到一个新的数组。
这个方法也可以在arguments的上下文中被调用，把其中所有元素复制到一个真实的数组中去：
```javascript
function sayHi() {
  var args = [].slice.call(arguments) // slice without parameters copies all
  alert( args.join(':') ) // now .join works
}
sayHi(1,2)
```
明白了！'arguments'是参数
arguments和命名参数引用同样的值。
更新arguments[..]引发对应参数的改变，反之亦然。
举例：
```javascript
f(1)
function f(x) {
  arguments[0] = 5
  alert(x) // 5, updating arguments changed x
}
```
 相反的过程：
```javascript
f(1)
function f(x) {
  x = 5
  alert(arguments[0]) // 5, update of x reflected in arguments
}
```
Arry方法修改arguments的同时也修改局部参数：
```javascript
sayHi(1)
function sayHi(x) {
  alert(x)           // 1
  [].shift.call(arguments)  
  alert(x)           // undefined, no x any more :/  
}
```
实际上，在现代的ECMA-262 5 规范中，arguments是从局部变量中分离出来的。
到目前为止，浏览器仍然按照上述过程执行。可以尝试一下例子看看。
一般来说，不去修改arguments是一个良好的习惯。
**默认值**
如果你想函数的参数可选，有以下两种方法：
1.第一，检查是否为undefined并重新分配：
```javascript
function sayHi(who) {
  if (who === undefined) who = 'me' 
  alert(who)  
}
```
2.或者，使用或||运算符：
```javascript
function sayHi(who) {
  who = who || 'me'  // if who is falsy, returns 'me'
    
  alert(who)  
}
sayHi("John")
sayHi()  // 'me'
```
这种方式适用于没有提供参数和提供参数两种情况，在实际应用中，通常为这种情况。

众所周知，Math.max是一个包含可变数量参数的函数，它返回最大值或者参数：
```javascript
alert( Math.max(1, -2, 7, 3) )
```
利用func.apply(context, args)和Math.max寻找数组中的最大元素：
```javascript
var arr = [1, -2, 7, 3]
/* your code to output the greatest value of arr */
```
**解决途径**
解决途径：
```javascript
var arr = [1, -2, 7, 3]
alert( Math.max.apply(Math, arr) ) // (*)
```
这里，我们调用Math.max方法，传递参数的数组args。
在“自然”调用Math.max(...)过程中，上下文this设定为Math,即"."前面的对象。在我们的代码中我们通过显示传递参数到apply来保持一致。
实际上，Math.max这个方法根本没有用上下文。我们可以进一步简化我们的代码：
```javascript
var arr = [1, -2, 7, 3]
alert( Math.max.apply(0, arr) ) // dummy '0' context
```
**关键字参数**
想象一下，如果你获得了一个含有许多参数的函数，而且大多数参数有默认值。如下所示：
```javascript
function showWarning(width, height, title, contents, showYesNo) {
  width = width || 200; // default values
  height = height || 100;
  var title = title || "Warning";
  ...
}
```
 这里我们获得了一个用于显示警告窗口的函数，它可以设置宽度、高度、标题、文本内容，显示按钮(如果设置了showYesNo)。
大多数参数具有默认值。
下面是我们怎么使用的：
```javascript
showWarning(null, null, null, "Warning text", true)
// or
showWarning(200, 300, null, "Warning text")
```
问题是：人们容易忘记参数的顺序和含义。
想象一下你有10个参数，而且大多数为可选参数。函数调用过程会变得十分可怕。
*关键字参数*技术存在于Python, Ruby 和其它语言当中。
在JavaScript中，这种技术通过一个参数对象实现：
```javascript
function showWarning(options) {
  var width = options.width || 200  // defaults 
  var height = options.height || 100
  var title = options.title || "Warning"
  // ...
}
```
调用这只能函数变得简单。你只要像这样传递一个参数的对象：
```javascript
showWarning({ 
  contents: "Text", 
  showYesNo: true
})
```
另外一个好处就是这个参数对象能够重新配置和重新使用：
```javascript
var opts = {
  width: 400,
  height: 200, 
  contents: "Text", 
  showYesNo: true
}
showWarning(opts)
opts.contents = "Another text"
showWarning(opts) // another text with same options
```
关键字参数应用于大多数框架。
**arguments的特殊属性**
`arguments.callee`
arguments有一个有趣的属性arguments.callee，它引用正在运行的函数。
为了支持命名函数表达式和更好的性能，这个属性已经被ECMA-262弃用。
如果他们知道arguments.callee不是必须的，JavaScript 的实现可以进一步优化代码。
一般情况下这个属性不会出现问题，但在“严格模式”下，启用这个属性会报错。

通常，这个属性应用于匿名函数的递归。
举例如下，setTimeout(func, ms)是一个在ms毫秒后调用func的内置函数。
```javascript
setTimeout(  
  function() { alert(1) }, // alerts 1 after 1000 ms (=1 second)
  1000
)
```
这个函数没有名称，让我们使用arguments.callee递归调用3次：
```javascript
var i = 1
setTimeout(  
  function() { 
    alert(i) 
    if (i++<3) setTimeout(arguments.callee, 1000)
  }, 
  1000
)
```
另外一个例子是阶乘的：
```javascript
// factorial(n) = n*factorial(n-1)
var func = function(n) {  
  return n==1 ? 1 : n*arguments.callee(n-1)  
}
```
上述的阶乘函数在func重新分配的情况下仍然会运行，这是由于它使用arguments.callee引用了自身。
`arguments.callee.caller`
arguments.callee.caller属性保持对调用函数的引用。

出于和arguments.caller同样的原因，这个属性已经被ECMA-262弃用。arguments.caller属性的意义与其相同，但兼容性更差。不要使用arguments.caller，尽量使用 arguments.callee.calle，所有浏览器都有这个属性。
在下面的例子中，arguments.callee.caller引用了g的调用函数，调用函数为f。
```javascript
f()
function f() {
  alert(arguments.callee.caller) // undefined
  g()
}
function g() {
  alert(arguments.callee.caller) // f
}
```