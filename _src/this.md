# “this”的四种用法

> 译者: 裘曾渊

作者：Ilya Kantor



在JavaScript中this的值是动态的。它是由哪个方法呼叫来决定的，而不是哪里被定义来决定。
**任何方法都可以用this**。和方法有没有被赋给对象并没有关系。
this的真实值由哪里调用来决定，这有四种情况。

**第一，在方法中被调用**
如果函数在对象中调用（点号或者方括号都可以），this指向对象。
```
var john = { 
  firstName: "John" 
}

function func() { 
  alert(this.firstName + ": hi!")
}

john.sayHi = func

john.sayHi()  // this = john
```
在上例中，函数是对象的一部分。但当调用john.sayHi()，把this只想点号前面的对象：john。

**第二，在函数中被调用**
如果函数用this，意味着当做方法调用。一个简单的func（）调用是个bug。没有上下文的时候，this指向window：
```
func() 

function func() { 
  alert(this) // [object Window] 或者[object global]或者其他。。
}

```
在现代语言规范中，this的行为是变化的，this＝undifined。默认情况下，浏览器仍然采用老方式（更兼容），但是有些浏览器，像火狐4，支持严格模式。会切换成现代标准。
在火狐4中尝试：
```
func()  

function func() { 
  "use strict"

  alert(this) // undefined in Firefox 4
}

```
引用类型
你怎么想？如下这些obj.go作用相同吗？
```
1. obj.go()
2. (obj.go)()
3. (a = obj.go)()
4. (0 || obj.go)()
```
答案是不，因为最后两个调用不接受this。
你怎么想？为什么？试试这：
```
obj = {
  go: function() { alert(this) }
}

obj.go(); // object

(obj.go)(); // object 

(a = obj.go)(); // window

(0 || obj.go)(); // window
```
原因是引用类型，在语言规范中描述。
为了传递this，value()括号前面必须是引用类型，或者简短的说，一个入口，像obj.a或者obj['a'],不能是任何更复杂的。用括号包裹不会有任何变化，所以(obj.a) 也可以。

任何其他表达式，像(a = obj.method)() 或者 (a.method || b.method)()不会传递this。

下面这段代码的结果是什么？
```
obj = {
  go: function() { alert(this) }
}

(0 || obj.go)()
```
解答
error！ 尝试运行下
这是因为在obj后没有分号。javascript会忽略空格，把代码看做是：
```
obj = { go: ... }(0 || obj.go)()
```
试着当做一个函数调用object{...}(...)，把右边认为是表达式，结果赋值给obj。
最好是加分号：
```
obj = {
  go: function() { alert(this) }
};

(0 || obj.go)()  // window

```

**第三，构造函数**
当调用构造函数，this做为一个新对象初始化。
在[对象构造器中][1]讨论


  [1]: http://javascript.info/tutorial/objects#new
  
  
  **第四，显示绑定this**
  这是Javascript中很有巧妙的部分。
  一个函数可以硬绑定this来调用。两种函数来完成这种绑定：call或apply。
  
  **call**
  call的语法是：
```
  func.call(obj, arg1, arg2,...)
```
  
  第一个call的参数变成this，其他参数arg1, arg2变成参数。
```
var john = { 
  firstName: "John" 
}

function func() { 
  alert( this.firstName )
}

func.call(john)  // "John"
```
上面的例子，func调用时this=john.
加上其他参数：
```
var john = { 
  firstName: "John",
  surname: "Smith"
}

function func(a, b) { 
  alert( this[a] + ' ' + this[b] )
}

func.call(john, 'firstName', 'surname')  // "John Smith"
```
现在func调用的this＝john和两个参数，结果是john['firstName'] 和 john['surname'].
**func.call(上下文，参数...)基本和简单的调用func（参数...）相同，除了额外的绑定this。**

**apply**
func.apply和func.call一样，他接受一组参数数组，而不是列表。
下面两行是一样的：
```
func.call(john, 'firstName', 'surname')
 
func.apply(john, ['firstName', 'surname'])

```
在某种程度上，apply比call强大，因为他能传递数组。
```
var args = ['firstName', 'surname']
func.apply(john, args)
```
**总结**
在方法中
```
obj.func(...)    // this = obj
obj["func"](...)
```
在函数中
```
func(...)        // this = window
```
在构造器中
```
new func()       // this = {} (new object)
```
用call/apply绑定this
```
func.call(context, arg1, arg2, ...)  // this = context
func.apply(context, [args])
```