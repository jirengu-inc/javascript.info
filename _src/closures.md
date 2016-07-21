> 译者: 徐月星

`[备注1]`：为避免造成误读，原文中  `LexicalEnvironment` (词法环境）、`Scope`(作用域)保留
`[备注2]`：[2]处可能翻译不准，为避免造成误读，英文部分被放在括号内
***
### 闭包
#### 1. 访问外部变量
#### 2. 嵌套函数
#### 3. 闭包     
1. 词法环境（`LexicalEnvironment` ）的可变性；
2. 声名远扬的闭合回路；
3. 作用域（`[[Scope]]`）的新功能；
#### 4. 总结；
***
从[前面的文章中](http://javascript.info/tutorial/initialization),我们了解到`LexicalEnvironment`对象的属性是一个变量。
在这里，我们讨论访问外部变量和嵌套函数。深入的了解闭包的自动搜寻机制。
#### 访问外部变量
如果访问的一个变量不是本地变量怎么办？就像这里：
~~~
var a = 5;
function f(){
    alert(a);
}
~~~
在这种情况下，解释器发现这个对象是一个外部变量。
这个过程由两个部分组成：
1. 首先，函数f不会在一个空的空间中被创建
现在，这是一个`LexicalEnvironment`对象。在上述情况中，它属于window（当函数被创建时，a是未定义的）。
![](http://javascript.info/files/tutorial/intro/scope/le_window1.png)
当一个函数被创建时，得到一个隐藏属性，叫做`[[Scope]]`，它代表当前的词法环境。
![](http://javascript.info/files/tutorial/intro/scope/le_window2.png)

2. 随后，当函数运行时，它会创建自己的`LexicalEnvironment`并且将它和`[[Scope]]`连接起来。
因此，当它在当前的函数中找不到这个变量时，他会继续向外搜索：
![](http://javascript.info/files/tutorial/intro/scope/le_window3.png)

如果一个可读变量，在其他任何地方都不存在，则会报错。
~~~
function f(){
    alert(x);//读取到x却会报错，因为不存在x
}
~~~
在某些语言情境下不会报错，例如：如果没有x，typeof x将返回undefined，但这是一个例外。
如果一个变量被定义了，却没有发现在任何地方被声明，那么久相当于它被创建在了最外层的window里面。
~~~
function f(){
    x = 5;//将x放在了window里面
}
~~~
#### 嵌套函数
一个函数可以被嵌套在另一个函数里面，形成一个`LexicalEnvironment`链，也可以叫做作用域链。
~~~
var a=1;
function f(){
    function g(){
         alert(a);
    }
    return g;
}
var func = f();
func();//1
~~~
形成`LexicalEnvironment`链（由内到外）：
~~~
// LexicalEnvironment = window = {a:1, f: function}
var a=1;
function f(){
    // LexicalEnvironment = {g:function}
    function g(){
        // LexicalEnvironment = {}
         alert(a);
    }
    return g;
}
~~~
因此。函数g可以访问函数f和变量a。
>##### 创建一个函数sum，使他实现这样的功能：sum(a)(b) = a+b.
>##### 是的，这个函数语法是双括号。有趣吗？比如：
>     sum(1)(2) = 3;
>     sum(5)(-1) = 4
>##### 解决方案：
>##### 思路是：sum函数应该返回一个函数，即可以访问a，也可以访问到下一个参数。
>##### 可以这样做：
>     function sum(a) {
>         return function(b) { // takes a from outer LexicalEnvironment
>             return a+b;
>         }
>     }
>     alert( sum(1)(2) );
>     alert( sum(5)(-1) );

>##### 创建一个函数sum，使他实现这样的功能：sum(a)(b) = a+b,并且**可以有无数个括号**：
>##### 比如：
>     sum(1)(2) = 3;
>     sum(5)(-1)(2) = 6;
>     sum(6)(-1)(-2)(-3) = 0
>     sum(0)(1)(2)(3)(4)(5) = 15
>##### 解决方案：
>##### 为了使sum(1)可以像sum(1)(2)一样调用，它必须返回一个函数。
>##### 这个函数既可以被调用，也可以被转换为一个有用的数字类型`(converted to a number with valueOf)[2]`.
>##### 那么解决方法就不言而喻了：
>     function sum(a) {
>         var sum = a;
>         function f(b) { 
>             sum += b;
>             return f;
>         }
>         f.toString = function() { returnsum };
>         return f;
>     }
>     alert( sum(1)(2) );  // 3
>     alert( sum(5)(-1)(2) );  // 6
>     alert( sum(6)(-1)(-2)(-3) );  // 0
>     alert( sum(0)(1)(2)(3)(4)(5) );  // 15
#### 闭包
嵌套函数可以在外面的函数执行完之后依旧有效：
~~~
function User(name) {
    this.say = function(phrase) {
        alert(name + ' says: '+ phrase)；
    }
}
varuser = new User('John')；
~~~
标记出`LexicalEnvironment`：
![](http://javascript.info/files/tutorial/intro/scope/user1.png)
**注意：这里的上下文与作用域和变量无关。** 它不作用在这里。
正如我们所看到的，this.say是User对象的一个属性，所以当User函数执行完之后，它依旧是有效的。
你应该还记得，当this.say被创建的时候，这个函数就会创建一个内部空间（每个函数都是如此），用this.say的作用域代替当前的词法环境`(gets an internal reference this.say.[[Scope]] to current LexicalEnvironment )[2]`。因此，在当前的词法环境下，User函数的执行结果就被保存在了存储器中。所有User函数的变量都是这样，它们都会被保存起来，不会随意被毁坏。
重点是要确保内部函数以后要访问外部函数的时候是可行的。
>##### 1.内部函数保存的东西可以被用于外部环境；
>##### 2.即使外部函数执行完了，内部函数也可以通过外部函数访问任何变量；
>##### 3.浏览器会在内存中保存`LexicalEnvironment`和他所有的属性（变量），知道有内部的函数来访问它。
>
>##### 这就是所谓的闭包。
#### `LexicalEnvironment`的可变性
当一些函数可以共享外部环境的时候，它们也可以修改它的属性。
在下面的例子中，由于this.say的作用， this.fixName改变了name：
~~~
function User(name){
    this.fixName = function(){
        name = 'Mr.'+ name.toUpperCase();
    }
    this.say = function(phrase) {
        alert(name + ' says: '+ phrase)
    }
}
var user = new User('John');
user.fixName();
user.say("I'm alive!") ; // Mr.JOHN says: I'm alive!
~~~