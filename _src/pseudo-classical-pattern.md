> 译者: 王楠

[伪类声明](http://javascript.info/tutorial/pseudo-classical-pattern#pseudo-class-declaration)

[继承](http://javascript.info/tutorial/pseudo-classical-pattern#inheritance)

[调用父类的构造函数](http://javascript.info/tutorial/pseudo-classical-pattern#calling-superclass-constructor)

[重写方法(多态性)](http://javascript.info/tutorial/pseudo-classical-pattern#overriding-a-method-polymorphism)
　　1.[重写后调用父方法](http://javascript.info/tutorial/pseudo-classical-pattern#calling-a-parent-method-after-overriding)
　　2.[Sugar: 消除父类的直接引用](http://javascript.info/tutorial/pseudo-classical-pattern#sugar-removing-direct-reference-to-parent)

[私有/保护方法 (封装)](http://javascript.info/tutorial/pseudo-classical-pattern#private-protected-methods-encapsulation)

[静态方法和属性](http://javascript.info/tutorial/pseudo-classical-pattern#static-methods-and-properties)

[总结](http://javascript.info/tutorial/pseudo-classical-pattern#summary)

在非典型模式下，对象被构造函数所创建并且它的方法是一个原型。

在框架中使用非典型模式，例如，在谷歌*Closure*图书馆，原生JavaScript对象也遵循这种模式。

**伪类声明**

选择伪类这个词，是因为在JavaScript中不存在真正的类，只是类似于C，Java，PHP中的类，并且和它们的模式比较接近。

**注意**：本文假定你是熟悉原生的继承是怎么样工作的，这个内容在[ Prototypal inheritance](http://javascript.info/tutorial/inheritance)中有涉及。
一个伪类由构造函数和方法组成。例如，这里有一个包含单独的*sit*方法和两个属性的Animal伪类
~~~
function Animal(name) {
  this.name = name
}
Animal.prototype = {
  canWalk: true,
  sit: function() {
    this.canWalk = false
    alert(this.name + ' sits down.')
  }
}

var animal = new Animal('Pet') // (1)
alert(animal.canWalk) // true
animal.sit()             // (2)
alert(animal.canWalk) // false
~~~

1.当*new Animal* 被调用的时候，新对象接收了到Animal.prototype的_proto_的引用。可以观察左边那张图的一部分.
2.在实例中，*animal.sit* 方法改变了*animal.canWalk* ,所以现在这个*animal* 对象不能walk，但是其他的*animals* 可以。
![](http://javascript.info/files/tutorial/advanced/oop/animal_constructor.png)

**伪类的结构**

* 方法和默认属性在原型中。

* 原型中的方法使用this，this是指当前对象，因为this的值只取决于上下文里被调用的地方，所以*animal.sit()* 将this指向animal。

在这个体系中有一些风险，看下面的例子。

你是一个仓鼠农场的团队领导。一位程序员有一个任务创建仓鼠构造函数和原型。

仓鼠们都应该有一个食物储存（food storage）和发现食物（found）的方法。

他给你带来的解决方案（如下图所示）。代码看起来不错，但是当你创建两个仓鼠，然后喂其中一个，不知怎的，两个仓鼠都饱了。

发生了什么？如何修复这种情况？

~~~javascript
function Hamster() {  }
Hamster.prototype = {
  food: [],
  found: function(something) {
    this.food.push(something)
  }
}

// Create two speedy and lazy hamsters, then feed the first one
speedy = new Hamster()
lazy = new Hamster()

speedy.found("apple")
speedy.found("orange")

alert(speedy.food.length) // 2
alert(lazy.food.length) // 2 (!??)
~~~

**继承**

让我们创建一个新类并且从*Animal* 继承它。

这里有一个 *A Rabbit* !

~~~javascript
function Rabbit(name) {
  this.name = name
}

Rabbit.prototype.jump = function() {
  this.canWalk = true
  alert(this.name + ' jumps!')
}

var rabbit = new Rabbit('John')
~~~

正如你说看到的，和*Animal* 类有相同的结构，原型中也有相同的方法。

为了从*Animal* 类继承，我们需要*Rabbit.prototype.__proto__ == Animal.prototype* 。这是一个非常自然地必要条件，因为如果一个方法不能在*Rabbit.prototype*  中找到，那么它应该在父类的方法中被找到。

这里可以看它们是如何工作的：
![](http://javascript.info/files/tutorial/advanced/oop/animal_inherit.png)

为了实现原型链，我们需要创建初始rabbit.prototype为空的对象继承animal.prototype，然后添加方法。
~~~javascript
function Rabbit(name) {
  this.name = name
}

Rabbit.prototype = inherit(Animal.prototype)

Rabbit.prototype.jump = function() { ... }
~~~

**继承**

在上面的代码中，继承是一个函数，它创建一个给定__proto__的空对象。
~~~javascript
function inherit(proto){
  function F(){}
  F.prototype=proto
  return new F 
}
~~~
更多细节请看 [Prototypalinheritance](http://javascript.info/tutorial/inheritance#inherit)。

最后，这是两个对象的全部代码：

~~~javascript
// Animal 
function Animal(name) {
  this.name = name
}

// Animal methods
Animal.prototype = { 
  canWalk: true,
  sit: function() {
    this.canWalk = false
    alert(this.name + ' sits down.')
  }
}

// Rabbit
function Rabbit(name) {
  this.name = name
}

// inherit
Rabbit.prototype = inherit(Animal.prototype)

// Rabbit methods
Rabbit.prototype.jump = function() {
  this.canWalk = true
  alert(this.name + ' jumps!')
}

// Usage
var rabbit = new Rabbit('Sniffer')

rabbit.sit()   // Sniffer sits.
rabbit.jump()  // Sniffer jumps!
~~~

注意：不要创建新的*Animal* 去继承。

这是一个著名的但错误的继承方式。人们没有使用*Rabbit.prototype = inherit(Animal.prototype) * 而是这样：
~~~javascript
// inherit from Animal
Rabbit.prototype = new Animal()
~~~

结果就是我们得到了一个新的*Animal* 对象原型。继承在这生效，因为新的*Animal* 自然地继承了*Animal.prototype*

...但是谁说 *new Animal()* 可以没有name就被调用？构造函数可能会严格地要求参数并且没有参数无法正常工作。

事实上，问题是比这更概念化。**我们不想创造一个Animal。我们只想从中继承。**

这也是为什么*Rabbit.prototype = inherit(Animal.prototype)* 更棒的原因。没有负效果的干净的继承。

**调用父类的构造函数**

“父类” 构造函数不会自动调用。我们可以通过将Animal函数应用到当前对象上来手动调用：

~~~javascript
function Rabbit(name) {
  Animal.apply(this, arguments)
}
~~~

在当前对象的上下文中执行动物构造函数，所以它可以在实例中设置这个 *name*。


**重写方法（多态）**

为了覆盖掉父方法，在子类的原型中替换掉它：

~~~javascript
Rabbit.prototype.sit = function() {
  alert(this.name + ' sits in a rabbity way.')
}
~~~

当调用*rabbit.sit()* 方法时，会在*rabbit*  的原生链上寻找*sit* 方法 ，先在*Rabbit.prototype* 上找，找不到就继续在*Animal.prototype* 上找。并且如果在*Rabbit.prototype* 上找到就不会继续上升到*Animal.prototype* 上。

当然，我们甚至可以更具体地说。一个方法可以直接在对象上被重写：

~~~javascript
rabbit.sit = function() {
  alert('A special sit of this very rabbit ' + this.name)
}
~~~

**重写后调用父方法**

当一个方法被重写之后，我们可能还想要调用之前那个老的方法。如果我们直接请求它的父类的原型，这是有可能的：
~~~javascript
Rabbit.prototype.sit = function() {
  alert('calling superclass sit:')
Animal.prototype.sit.apply(this, arguments)
}
~~~

所有的父方法可以通过，appll/call以this的方式来传递当前对象的方法，来被调用。一个简单的调用*Animal.prototype.sit()* 可以使用Animal.prototype当做this。

**Sugar: 消除父类的直接引用**

在上面的例子中，我们直接调用父类。或者是构造函数：Animal.apply……或方法：Animal.prototype.sit.apply....

通常，我们不应该这样做。重构可能会在层次结构中改变父类名或引入中间类。

通常编程语言允许通过使用一个特殊的密钥来调用父方法，像*parent.method() *or *super()*.

JavaScript没有这样的功能，但我们可以效仿它。

下面的功能扩展了继承，并且还指定父类和构造函数调用没有直接引用的父类：

~~~javascript
function extend(Child, Parent) {
  Child.prototype = inherit(Parent.prototype)
  Child.prototype.constructor = Child
  Child.parent = Parent.prototype
}
~~~

Usage：

~~~javascript
function Rabbit(name) {
  Rabbit.parent.constructor.apply(this, arguments) // super constructor
}

extend(Rabbit, Animal)

Rabbit.prototype.run = function() {
    Rabbit.parent.run.apply(this, arguments) // parent method
    alert("fast")
}
~~~

作为结果，我们现在可以重新命名*Animal* ，或者创建一个中间类*Grasseatinganimal* 并且这些改变只会触碰到Animal和扩展（…）。

**私有/保护方法 (封装)**

保护的方法和属性被命名约定支持。所以，一个以下划线“_”开始的方法不应该被从外部调用（技术上是可调用的）。

![](http://javascript.info/files/tutorial/advanced/oop/animal_private.png)

私有方法通常也不支持。


**静态方法和属性**

静态属性/方法直接分配给构造函数：

~~~javascript
function Animal() { 
  Animal.count++
}
Animal.count = 0

new Animal()
new Animal()

alert(Animal.count) // 2
~~~

**总结**

最后，整个suppa-mega-oop 框架：

~~~javascript
function extend(Child, Parent) {
  Child.prototype = inherit(Parent.prototype)
  Child.prototype.constructor = Child
  Child.parent = Parent.prototype
}
function inherit(proto) {
  function F() {}
  F.prototype = proto
  return new F
}
~~~

使用：

~~~javascript
// --------- the base object ------------
function Animal(name) {
  this.name = name
}

// methods
Animal.prototype.run = function() {
  alert(this + " is running!")
}

Animal.prototype.toString = function() {
  return this.name
}


// --------- the child object -----------
function Rabbit(name) {
  Rabbit.parent.constructor.apply(this, arguments)
}

// inherit
extend(Rabbit, Animal)

// override
Rabbit.prototype.run = function() {
  Rabbit.parent.run.apply(this)
  alert(this + " bounces high into the sky!")
}

var rabbit = new Rabbit('Jumper')
rabbit.run()
~~~

框架可以多加一点糖，像功能混合，它可以从一个对象上复制许多属性到另一个对象上：

~~~javascript
mixin(Animal.prototype, { run: ..., toString: ...})
~~~

但是实际上，如果你只有两个小功能要做的话，你不需要使用OOP模式。