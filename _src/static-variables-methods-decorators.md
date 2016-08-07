> 译者: 金晓云

**静态变量和方法**
1.静态变量
2.静态方法

函数是一个对象，为我们提供了一个极好的方式来创建静态变量，或者换句话说，这个变量存在于多次调用过程中。
比如，我们希望得到一个记数函数调用的变量。

**静态变量**
有许多语言允许在一个变量前面加一个static关键词，加了关键词的变量在下次调用过程中不会被清除。
PHP中的静态变量举例：
```javascript
function f() { // PHP code!
  static $count = 0;
  echo ++$count;
}
f(); f(); f(); // 1 2 3
```
在JavaScript中没有static关键词或其他类似的词，但是我们可以把数据直接放在函数里面(像在其他任何对象一样)
```javascript
function f() {
  f.count = ++f.count || 1 // f.count is undefined at first 
  alert("Call No " + f.count)
}
f(); // Call No 1 
f(); // Call No 2
```
当然，一个全局变量能够保持记数，但是利用静态变量的方式能够得到更优的结构体系。
用arguments.callee替代f的代码更为通。
```javascript
 function f() {
  arguments.callee.count = ++arguments.callee.count || 1 
  alert("Called " + arguments.callee.count + " times")
}
```
现在，你可以放心的重命名这个函数了。:smile: 

**静态方法**
跟静态变量类似，静态方法也绑定到函数上面，一般用于对象。
```javascript
function Animal(name) {
  arguments.callee.count = ++arguments.callee.count || 1 
  this.name = name
}
Animal.showCount = function() {
  alert( Animal.count )
}
var mouse = new Animal("Mouse")
var elephant = new Animal("elephant")
Animal.showCount()  // 2
```