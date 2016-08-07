> 译者: soliloquy

`[备注1]`：下面代码部分有高亮显示，未表示出来
`[备注2]`：[2]处可能翻译不准，为避免造成误读，英文部分被放在括号内
***
### "与"操作
#### 1. `lookup`的例子
#### 2. `Setting`的例子
#### 3. 为什么‘with’被弃用  
#### 4. 总结 
‘with’操作允许使用作用域内的任意对象。
它应用于`code around`,并不支持现在的JavaScript。
它的语法结构是这样的：
~~~
with(obj) {
  ...
}
~~~
在‘with’块中，当解释器遇到一个变量，首先会查找这个对象的属性，然后才会出来。
#### `lookup`的例子
下面的例子展示了‘with’如何从对象中获取a的值。
~~~
var a = 5
var obj = { a : 10 }
with(obj) {
  alert(a) // 10
}
~~~
解释器在obj中没有找到属性b，于是从with的外面去寻找它。
‘with’嵌套也是可以的：
~~~
var box = {
  weight: 10,
  size: {
    width: 5,
    height: 7
  }
}
 
with(box) {
  with(size) { // size is taken from box
    alert( width*height / weight ) // width,height 来自 size, weight来自 box
  }
}
~~~
注意：如何从对象不同的作用域获取属性用在同一个地方。很神奇！
变量的查找顺序是这样的：`size => box => window:`
![](http://javascript.info/files/tutorial/intro/scope/with_obj_size.png)
#### `Setting`的例子
当变量在对象中被找到后，它可以被改变`[1]`：
~~~
var a = 5
var obj = { a : 10 }
with(obj) {
  a = 20
}
alert(obj.a)
~~~
现实中的另一种形式：
~~~
with(elem.style) {
  position = 'absolute'
  left = '10px'
  top = '0'
}
~~~
在上面的例子中elem.style.position, elem.style.left 和 elem.style.top被赋予了新的值。
### 为什么‘with’被弃用
>##### 人们不喜欢用‘with’，因为它给对象的工作以误解。
不幸的是，这只是一个误解。如果你尝试着加它的话，它可能找到一个变量并且将其改变。
Whops! Failure!..
在下面的例子中，我们尝试着改变box的属性。一切都没问题，直到我们试着添加一个新的属性：
~~~
var box = {
  weight: 10
}
with(box) {
  weight = 20
  size = 35 // (*)
}
alert(box.size)
alert(window.size)
~~~
在box中没有size这个属性，所以查找的过程在with中不会停止，它会继续向上。在这个例子中，根本没有size这个属性，所以解释器创建了一个新的 window.size 变量。
这也许不是我们想要的结果。这样的bug不多，但是却很难去调试。
另外一个不推荐使用with的原因是：JavaScript的压缩算法（`compression algorithms[2]`）不能有效的处理with。
最后，中间作用部分可能因为’with‘减缓执行（` the intermediate scope introduced by with slows down the execution[2]`），现在的JavaScript解释器要非常快速的处理，尽可能的接近本地代码。因此，不用’with‘就减少了一个性能问题。
建议用一个临时变量来代替’with‘：
~~~
var s = elem.style
s.position = 'absolute'
s.top = '10px'
s.left = '0'
~~~
虽然不是那么简洁，但是避免了额外的嵌套和更多的错误。
### 总结
1.`with(obj) { ... } `将obj作为一个额外的范围。所有这个作用域里面obj的属性都会先被查找，然后再向外查找。
2.`with`被弃用，并且因为这些原因而不被推荐，尝试着避免它。
3.当你使用with的时候。JavaScript的编译器将会出现bug。