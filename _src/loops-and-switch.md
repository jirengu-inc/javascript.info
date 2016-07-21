> 译者: 李栋

循环结构在给定的条件为真的情况下，允许一段代码多次重复使用，
javascript有三种循环

## `while`和`do..while`循环
这两种循环会在循环前/后检查给定的条件

~~~
while(i<10) {
  // do something
}
do {
  // something
} while (i<10)

~~~
当给定条件(例子中的`i<10`)为真时，大括号里的代码会持续循环
下面会无限循环 

~~~
while(true) {
  // do something
}
~~~

像许多别的语言一样，你像这样检查是否为零

~~~
var i = 3
while(i) {
  alert(i--)
}
~~~

上面的代码当`i==0`的时候停止循环，因为`0`在布尔类型中代表`false`
要注意的是在用`i--`递减`i`的值前会返回值，那就是为什么开始的时候输出`3`，结束的时候输出`1`

## `for`循环
循环如下：
~~~
for(i=0;i<10;i++) {
  alert(i)
}
~~~

声明部分由；和内容分成了三部分
这里是每部分的描述(通常类似在C，Java,PHP等等)

- `i=0` - 初始化
   - 在循环开始的时候运行
- `i<10` - 循环条件
   - 这个条件用来在每次循环内容执行之后，检查循环是否结束
- `i++` - 增量
   - 这个动作在每次迭代之后、循环内容检查前执行

在以上给定的例子中，当`i`的值从`0`到`9`变化的时候循环体会执行。当`i++`使得`i=10`的时候，循环被条件`i<10`停止。
一般我们这样来定义`for`的声明条件内的循环变量
~~~ 
for(var i=0; i<10; i++) { ... }
~~~
你可以让for的声明条件为空，例如，像下面这样的简单无限循环：

~~~
for(;;) {
  // will repeat eternally (in theory)
}
~~~
问题：
查看页面!(tutorial/intro/source/loop.html)[]
将`for`循环改写成`while`循环，发挥同样的作用
答案：
解决方案的源代码!(点击这里)[]

问题：
创建一个声明条件使数字超过100的循环，当用户键入的数大于100的时候，循环必须停止。
点击ESC键或Cancel键也必须停止循环
点击这里查看代码是如何运行的!(here)[]
答案：
解决方案源代码!(here)[]

## 用`break`跳出循环
有时候需要立即跳出循环，这里我们用到了`break`声明。
下面的例子当`i`变成`6`的时候，跳出循环。
在循环之后继续执行后面的语句(标记×的部分)

~~~
var i=0
while(true) {
  i++   // increase i by one
  if (i>5) break
  alert(i)
}
// (*)
~~~

## 用`continue`跳转
`continue`语句可以跳过不需要运行的语句
例如，下面的代码忽略了负值

~~~
var i = -3
while(i<3) {
  i++
  if (i<0) continue
  alert(i)  // (*)
}
~~~

标记了×的`i`值为负数，代码永远不会执行，这多亏了有`continue`

## 使用lable来跳转语句块
在Javascript中，循环里的label非常特殊
他们用来break/continue多个嵌套的循环
标签通常放在循环声明语句前，可以加下划线标示

~~~
outer:
for(;;) {
  // ...
  inner: for(;;) {
    // ...
  }
}
~~~

我们可以用`break label`或者`continue label`来跳转多个循环
下面的例子来示范如何跳转两个循环块

~~~
outer:
for(;;) {
  for(i=0; i<10; i++) {
    if (i > 3) break outer;
  }
  // the part of code after inner loop is never executed,
  // because break jumps right to outer label
}
alert("i="+i);
~~~

你可以用同样的方法来用`continue`，它可以跳转到下一个迭代label循环
有趣的是label可以让你把它们附加到语句块上

~~~
my: {
  for(;;) {
    for(i=0; i<10; i++) {
      if (i>4) break my;
    }
  }
  some_code;
}
alert("after my");
~~~

在上面给出的例子中，`break`直接跳过了`some_code`，跳到了被my标记的语句块的结束
那很像goto，但是它并不是，因为你不能跳转到循环外面，而且你也不能跳转到循环代码前
Javascript即使在最新版本的标准中也没有`goto`语句

问题：
如果一个数它有1和它本身两个因数，那么它就是素数。
或者，换一句话来说，如果数字`2..n-1`用非0余数来分割`n`，那么`n-1`为素数
创建一个输出小于10的素数的代码。那么肯定是2，3,5,7.
P.S.如果用100，1000来代替10，那么代码也可以运行
打开提示1

答案：
~~~
next_prime:
for(var i=2; i<10; i++) {
  for(var j=2; j<i; j++) {
    if ( i % j == 0) continue next_prime
  }
  alert(i)  // prime
}
~~~

## `switch`结构
在Javascript中另一种常用的结构是`switch`。它为多个相同的并发循环提供了一个简短的语法
像这样：

~~~
switch(x) {
  case 'value1':  // if (x === 'value1')
    ...
  case 'value2':  // if (x === 'value2')
    ...
  default:
    // default code
}
~~~

switch操作符检查变量与`case`的值，来执行下面的代码直到遇到`break`
如果没有case匹配，那么执行default标签的代码
 
例子

让我们看一个运行的代码

~~~
var x = 2

switch(x) {
  case 1:
    alert('never executes')
  case 2:
    alert('x === 2') 
  case 3:
    alert('x === 3')
}

~~~
上面的例子输出的是2,3.那是因为代码从`case 2`开始执行，然后继续执行下面的语句
为了停止执行一个case。我们需要用到`break`

~~~
var x = 2

switch(x) {
  case 1:
    alert('never executes')
    break
  case 2:
    alert('x === 2')   // <-- start
    break              // stop!
  case 3:
    alert('x === 3')
    break
}

~~~

在上一个例子中，每一个`case`都添加了一个`break`语句。那一通常用来确保只有一个case运行。

### 需要用户输入的例子

~~~
var arg = prompt("Enter arg?")
switch(arg) {
  case '0':
  case '1':
    alert('One or zero')
  case '2':
    alert('Two')
    break
  case 3:
    alert('Never happens')
  case false:
    alert('False')
    break
  default:
    alert('Unknown value: '+arg)
}
~~~

如果你输入`0`或者`1`，它会执行相应的`case`，执行第一个alert，然后是第二个，最后`break`结束
如果你输入`2`，`switch`会运行`case 2`，跳过第一个alert
如果你键入``'3'``，`switch`会执行default case，那是因为``'3'``提示是个字符串，不是个数字，switch用`===`来检查是否绝对相等，所以`case 3` 没有被匹配到。

这也就是例子中`case false`永远不会运行的原因。也许 当`arg===false`的时候，它会运行，返回一个字符串或者`null`。