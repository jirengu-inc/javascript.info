# 函数声明与表达式

> 译者: 裘曾渊

函数和变量一样，可以在代码的任何地方定义。
JavaScript提供一些方式去定义方法：

* 函数声明
* 函数表达式
* 函数作为另一个函数结果被调用


----------


### 语法
基本创建函数的语法是函数声明。语法如下：

```
function f(arg1, arg2, ...) {
   ... code ...
}
```
比如：

```
function sayHi(name) {
  alert("Hi, "+name)
}

sayHi('John')
```

这个例子声明一个只有一个参数的函数sayHi，运行sayHi。


----------


###返回值
用return声明来返回值
```
function sum(a, b) {
  return a+b
}

var result = sum(2,5)
alert(result)
```

如果函数没有返回值，结果被认为是一个特殊值“undefined”。如下例：
```
function getNothing() {
  // no return 
}

var result = getNothing()

alert(result)
```

返回空值也是一样结果：
```
function getNothing() {
  return 
}
alert( getNothing() === undefined ) // true
```


----------


###局部变量
函数内可以包含变量，用var来定义。这种变量叫做局部变量，只能被内部函数访问。
```
function sum(a, b) {
  var sum = a + b

  return sum
}
```

创建一个函数min(a,b)，接收两个数字，返回一个较小的值。
min(2, 5)  == 2
min(3, -1) == -1

答案
方案一 用if判断:
```
function min(a, b) {
  if (a < b) {
    return a
  } else {
    return b
  }
}
```
方案二 用三目运算符:
```
function min(a, b) {
  return a < b ? a : b  //  如果是true运行a，否则运行b
}
```

---
写一个函数pow(x,n)返回x的n次方，或者说，x被自己乘了n次。
pow(3, 2) = 3*3 = 9
pow(3, 3) = 3*3*3 = 27
pow(1, 100) = 1*1*...*1 = 1
```
function(x,n){
    var result
    for(var i = 0;i<n;i++){
        result*=x;
    }
    return result;
}
```


----------


### 函数声明
**函数声明在预执行阶段（当浏览器准备执行代码）解析。**所以函数声明在前后都能被呼叫到。
```
function sayHi(name) {
  alert("Hi, "+name)
}

sayHi("John")
```
```
sayHi("John")

function sayHi(name) {
  alert("Hi, "+name)
}
```
**所以函数在任何位置都可以声明**

比如，我们可能希望根据条件声明不同的函数
```
sayHi()

if (1) {
  function sayHi() {  alert(1)  }
} else {
  function sayHi() {  alert(2)  } // <--
}
```

尝试在不同浏览器中运行这个例子。火狐会报错，其他浏览器输出2。

那是因为声明在执行前被传递了。根据文档(p.10.5)，后同名的函数，后
声明的会覆盖存在的函数。

当执行时，其他声明会被忽略，所以if语句没有任何效果。


----------
**函数表达式**
JavaScript中函数是一等公民，和数字字符一样。
在任何你可以写值的地方，你都可以写函数。函数表达式的语法
```
function(arguments) { ... }
```
举例，你可以写
```
var f = 5
```
但你也可以赋函数表达式
```
var f = function(name) {
    alert("Hi, " + name + "!");
}
```


什么时候函数是表达式，什么时候是声明呢？
规则很简单。
但javascript解析器在主代码流中，他会认为这是函数声明。
当函数是声明语句的一部分是，他就是函数表达式。


当执行流到达函数表达式被创建，结果是，函数表达式只有他们被执行后才能使用。
```
var sayHi

// sayHi() <-- 这里不能执行，还没有sayHi函数

if (1) {
  sayHi = function() {  alert(1)  }
} else {
  sayHi = function() {  alert(2)  }
}

sayHi()
```
上述代码所有浏览器执行结果相同。


**请用声明**
经验不足的开发人员写的代码中，方法经常用表达式来声明：
```
var f = function() { ... }
```
函数声明更加可读简洁，还是用函数声明吧。
```
function f() { ... }
```
除此以外，函数声明可在定义前呼叫。
**只有你在执意要用函数表达式时采用。如例子中，条件性的函数定义。**

**函数是值**
javascript中的函数是一般值，我们可以输出他。
```
function f() { alert(1) }

alert(f)
```
上面这个输出函数的例子。*通常用作源代码。*( Usually as the source code.)
Both declarations and expression declare a variable and put the function into it. Only the creation time is different.
**声明和表达式都可以作为变量的值。**只是创建的时间不同。

**传递函数**

函数和任何值一样可以被赋值，做为其他函数的参数传递等。

如下例，怎么定义函数没有关系。
```
function sayHi(name) {
  alert("Hi, "+name)
}

var hi = sayHi // 把函数赋值给变量

hi("dude")     // 执行函数
```

函数通过引用被赋值。函数保存在内存的某处，sayHi是它的引用（指向）。当我把函数赋值给hi，变量开始引用同一个函数。

**一个函数可以接受另一个函数做为参数**
```
function runWithOne(f) {  // 运行做为参数1的函数
  f(1)
}

runWithOne( 
  function(a){ alert(a) } 
)
```

逻辑上说，函数是一个动作.那么，传递函数是传递一个动作，能够在程序另一部分来初始化。这种特性在javascript中被广泛使用。

在上面的例子中，我们创建一个没有名字的函数，没有赋值给任何变量。这样的函数叫匿名函数。


**就地执行**


可以用函数表达式创建并立即运行函数，像这样：
```

(function() {
  var a, b    // 局部变量
  // ...      // 其他代码
})()
```

立即执行函数大多被用在我们想做一些围绕局部变量的事情。我们不想我们的局部变量变成全局，所以包含在函数里面。

在执行后，全局命名空间还是很干净，很好的实践。

那为什么函数在括号中？是因为javascript只允许函数表达式立即执行。

函数声明不能被这样用：
```
function work() {
  // ...
}()  //  语法错误
```

即使我们去掉名称，javaScript会看到关键词函数，尝试转换成函数声明。
```
function() { // 错误，没有名称的函数声明。
  // ...
}()
```

那么。唯一的方式是把函数用括号包起来。就会打断他被认为是声明的一部分，所以是函数表达式。

如果函数是一个明显的表达式，那就没必要包起来，如下：
```
var result = function(a,b) { return a+b }(2,2)
alert(result) // 4
```

在上面的代码中，函数被创建且立即调用。
就像var result = sum(2,2)，用sum函数替换函数表达式。

下面的代码执行结果是什么？为什么？
```
var a = 5

(function() {
  alert(a)
})()
```
答案：
答案是error，尝试下面代码：
```
var a = 5
(function() {
  alert(a)
})()
```
在var a = 5后面没有分号。JavaScript把代码认为是：
```
var a = 5(function() {
  alert(a)
})()
```

那么，他会尝试运行5做为一个函数，这就会造成错误。能运行的代码如下：
```
var a = 5;
(function() {
  alert(a)
})()
```
这可能JavaScript中是最危险隐蔽的不写分号。
还有一种方式直接调用函数构造器。把参数列表和函数作为字符串，用他们创建函数。
```
var sayHi = new Function('name', ' alert("Hi, "+name) ');

sayHi("Benedict");
```
这种方式用的很少很少，几乎不用。


**命名函数表达式**
一个函数表达式可有名字：

```
var f = function sayHi(name) {
  alert("Hi, "+name)
}
```
语法叫命名函数表达式（NFE）在任何浏览器都可用除了IE9以下。

在那些支持的浏览器中，名字只在函数内可见

```
var f = function sayHi(name) {
  alert(sayHi) // 输出函数
}

alert(sayHi) // 错误：为定义的变量'sayHi'
```
在这种情况IE会创建两个函数对象：sayHi和f：
```
var f = function g() {  }
//
IE中是false，其他浏览器报错（g为定义）
alert(f=== g)
```

NEFS存在是为了递归匿名函数。
观察下面代码片段被包在serTimeout中调用：
```
setTimeout(function factorial(n) {
  return n == 1 ? n : n*factorial(n-1)
}, 100)
```
代码的结果是什么？为什么？
```
( function g() { return 1 } )
 
alert(g)
```
答案：
结果是error：

```
( function g() { return 1 } )

alert(g)
```

解决方案的关键是理解(function ... )是一个函数表达式（参考本章），不是函数声明。

那么，我们有一个有名函数表达式。

**有名函数表达式的名字只在内部可见。**

除了IE9.0以下，所有浏览器都支持NFEs，那么他们会给出错误“undefined variable”，因为g只有在函数内部可见。

ie9.0以下不支持NFE，就会输出函数


**函数命名**
函数是一个动作。所以命名应该是动词，像get,read,caculateSum,等等。
短函数名称的规则：

 - 一个函数是临时的且只使用在附近的代码。变量的短命名可用这个逻辑。
 - 一个函数用在代码任何地方。一方米昂，没有忘记他的危险，另一方面，你可以写的更少。

真实的例子‘$’, ‘$$’, ‘$A’, ‘$F’ 等。
JavaScript库中频繁调用的函数用这些短名称。
其他情况，函数的名字应该是一个动词，或者一个动词开头的多个词叠加。

**总结**

JavaScript中函数是普通值。他们可以按需求被赋值，传递，调用。

 - 一个不返回值的函数事实上会返回一个特殊值：undefined。
 - 使用动词命名函数。短命名允许被用在两种情况：被用在附近代码块，或被非常广泛使用。
 | 函数声明    | 函数表达式|
| :  ------:|    :   --------  :  |
| 函数用在主代码流中|方法在表达式中创建 |
| 在与执行阶段被创建，在定义前后都能调用|在执行时创建，只能在创建后调用|
|              |   就地调用   |

总体来说，推荐使用函数声明，除非有原因需要使用函数表达式。