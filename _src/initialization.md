> 译者: 李栋

## 函数和变量的初始化
在JavaScript中的变量和函数的结构完全不同于大多数别的语言。
一旦你知道它怎么运行的，更高级的就变的易于掌握。
 - 在JavaScript中，所有的局部变量和函数都是特殊内部对象的属性，我们称之为运行环境(`LexicalEnvironment`) 
在浏览器中这个顶层对象`LexicalEnvironment`就是`window`,我们称之为全局变量。
### 顶层变量的实例化
当JavaScript被执行，有一个预处理阶段，我们称之为变量实例化。
1. 首先，像`function name {...}`在主函数代码流中声明那样，解释器扫描代码`<a href="/tutorial/functions-declarations-and-expressions">Function Declarations</a>`。
它找到所有声明，创建函数并把函数放到`window`中。

例如，看下面的代码：
~~~
var a = 5
function f(arg) { alert(`f:`+arg) }
var g = function(arg) { alert(`g:`+arg) }
~~~
在这个时候，浏览器找到`function f`,创建函数并把函数存储为`window.f`

~~~
// 1. Function Declarations are initialized before the code is executed.
// so, prior to first line we have: window = { f: function }
var a = 5
function f(arg) { alert(`f:`+arg) } //FunctionDeclaration
var g = function(arg) { alert(`g:`+arg) }
~~~

作为一个副作用，`f`可以在它声明之前被调用：

~~~
f()
function f() { alert(`ok`) }
~~~

2. 其次，解释器扫描`var`声明，并创建`window`属性。在这个阶段声明并没有执行，所有变量开始是都是未定义`undefined`的。

~~~
// 1. Function declarations are initialized before the code is executed.
// window = { f: function }
// 2. Variables are added as window properties.
// window = { f: function, a: undefined, g: undefined }
var a = 5   // var
function f(arg) { alert(`f:`+arg) }
var g = function(arg) { alert(`g:`+arg) } //var
~~~

这里的`g`的值是一个函数表达式，但是解释器并不关心。它创建了变量，但是不分配它们。
总而言之：
 - `函数声明`成为现成的函数。它允许在声明前调用。
 - 变量都是以`undefined`开始的。
 - 当执行到它们的时候，才会运行。

作为副作用，变量和函数不能有相同的名字。
3. 然后，代码开始执行.
当一个变量或者函数被访问到，解释器就会从`window`里面找到它。
 
~~~
alert("a" in window) // true, because window.a exists
alert(a) // undefined, because assignment happens below
alert(f) // function, because it is Function Declaration
alert(g) // undefined, because assignment happens below
var a = 5 
function f() { /*...*/ }
var g = function() { /*...*/ }
~~~

4. 运行之后，`a`变成了`5`,而`g`变成了一个函数。在下面的代码中。`alerts`在下面移动。

注意不同点：

~~~
var a = 5 
var g = function() { /*...*/ }
alert(a) // 5
alert(g) // function
~~~

如果变量没有用`var`声明，当然，它就不会在初始化的时候创建。解释器就不会管它：

~~~
alert("b" in window) // false, there is no window.b
alert(b) // error, b is not defined
b = 5
~~~

但是运行之后，`b`就变成了常规变量`window.b`就像它被声明了:

~~~
b = 5
alert("b" in window) // true, there is window.b = 5
~~~

问题：下面代码答案是什么？

~~~
if ("a" in window) {
    var a = 1
}
alert(a)
~~~

答案：是1.
让我们回溯代码来看看是为什么。
  
  1. 在初始化阶段，`window.a`被创建：
~~~
// window = {a:undefined}
if ("a" in window) {
    var a = 1
}
alert(a)
~~~

  2. `" a "在window中`是`true`.
~~~
// window = {a:undefined}
if (true) {
    var a = 1
}
alert(a)
~~~

所以`if`被执行了，因此`a`的值变成了`1`.

问题：如果`a`前没有`var`，结果会是什么呢？
~~~
if ("a" in window) {
    a = 1
}
alert(a)
~~~
答案：是`Error: no such variable`,这是因为在查看`window里的” a “`时，没有变量`a`.
所以，`if` 分支没有执行，也就没有打出`a`。
### 函数变量
当函数运行，每一个函数调用，新的`运行环境`就被创建，其中包括参数，变量和嵌套的函数声明。
对象在内部被用于读/写变量。不像`window`,这个函数的`运行环境`不能直接访问。
让我们来看看下面函数的执行细节：
 
~~~
function sayHi(name) {
  var phrase = "Hi, " + name
  alert(phrase)
}
sayHi(`John`)
~~~

1. 当解释器准备开始函数代码运行时，在第一行代码运行前，一个空的`运行环境`就被创建，其中包含参数，局部变量和嵌套函数。
~~~
function sayHi(name) {
// LexicalEnvironment = { name: `John`, phrase: undefined }
  var phrase = "Hi, " + name
  alert(phrase)
}
sayHi(`John`)
~~~

自然，参数开始赋值，但是局部变量没有。
2. 然后函数的代码开始运行，最后赋值被执行。
一个变量内部赋值的意思相当于`运行环境`的属性得到一个新值。
所以`phrase = "Hi, "+name`改变了`运行环境`：
~~~
function sayHi(name) {
// LexicalEnvironment = { name: `John`, phrase: undefined }
  var phrase = "Hi, " + name
// LexicalEnvironment = { name: `John`, phrase: `Hi, John`}
  alert(phrase)
}
sayHi(`John`)
~~~

最后的`alert(phrase)`找到在`运行环境`中的`phrase`,然后输出它的值。
3. 在执行的最后，`运行环境`，通常抛弃它所有内容，因为变量不在长时间被需要，但是也不是总是这样。
规范：
如果我们看最近的ECMA-262规范，这实际上是两个对象。
第一个是`变量环境`对象，通常包含变量和函数，被`函数声明`声明，然后变的无法改变。
第二个是`运行环境`对象，通常类似与`变量环境`，实际上在运行期间被用到。
更加详细的描述可以在ECMA-262标准的10.2-10.5和13章节中找到。
同样需要注意的是，在JavaScript实现过程中，有连个对象可以被合并到其中。所以，我们避免那些无关紧要的细节，到处用`运行环境`术语。
### 语句块没有范围
下面语句直接没有区别：
~~~
var i = 1
{
  i = 5
}
~~~
和下面语句：
~~~
i = 1
{
  var i = 5
}
~~~

在执行两种情况之前首先处理`var`声明。
不像Java，C语言等等语言，在JavaScript中，变量循环存在。
这是因为，它们的范围是这个函数。
~~~
for(var i=0; i<5; i++) { }

alert(i) // 5, variable survives and keeps value
~~~

循环声明变量非常方便，但是是没有循环的范围。
问题：下面的测试会输出什么，为什么?(请在执行前回答)

~~~

function test() {
  alert(window)
  var window = 5
}
test()
~~~

答案：`var`在预执行阶段被处理。
所以，在它被输出前，`window`变成了一个局部变量。
~~~
LexicalEnvironment = {
  window: undefined
}
~~~

所以当开始执行时，第一行，变量`window`存在，并且是未定义的。

问题：你认为会输出什么，为什么？
~~~
var value = 0
function f() { 
  if (1) {
    value = `yes`
  } else {
    var value = `no`
  }
  alert(value)
}
f()
~~~

答案：
`var`被处理，在预执行阶段被创建类似于`运行环境`属性。
所以，这行代码`` value=`yes` ``会执行于局部变量，最后输出`yes`