> 译者: 朱志国

## 修饰符
- 一个函数修饰符接受一个函数，在包装或者修饰的调用并返回包装器，改变默认行为。
例如，一个`checkpermission`修饰可能只允许函数如果用户有足够的权限。
## 列子
```
fuction checkPermissionDecorator(f){
   return function() {
     if (user.isAdmin()) f()
     else alert('Not an admin yet')
  }
}
//  用法：请保存后检查程序权限
save = checkPermissionDecorator(save)
// 现在保存（）调用将检查权限。
```
- 继续看一个复杂的修饰符。`doublingDecorator`接受一个函数`func`并返回装饰变体双打（这个词怪怪的错误请指正）的结果。
```
fuction doublingDecorator(f){
    return function( ){
       return 2*f.apply(this,arguments)  
 }
}
// 用法
function sum(a,b){
   return a + b
}
var doubleSum = doublingDecorator(sum)

alert( doubleSum(1,2) )
alert( doubleSum(1,2) )
```
修饰符创建一个新的匿名函数通过调用func和双精度数的结果。需要注意的是，这个func.apply(this,arguments)的技巧是常用于javascript提出同一上下文调用相同的参数。

修饰符是一种编程模式，因为它允许现有功能的扩张/修改它的行为。

- 有两个原因为什么知道`decorator`出色。
1. 修饰符可以重用。doublingDecorator可以应用于minus和divide以及sum。
2. 可以组合多个`decorator`更大的灵活性。

## 更多任务例子

- 创建一个函数makeLogging(f)将一个任意的函数f,在它的日志调用包装器。 包装器应该有一个静态的outputLog()输出日志的方法。

应该是这样的:

```
function work(a,b)

function makelogging(f)

work = makelogging(work)

work(1,2)
work(5,6)
work.outputLog()

没有修改的work是允许的，这个代码会驻留在makelogging。

怎么去解决呢？只需要在`wrapper`函数将参数放入日志，并将调用转发f。

如下面代码。


function work(a,b)

function makelogging(f)
   
     var log = [ ]

     function wrapper( ) {
     
         log.push(arguments)
         return f .apply(this,arguments)
}
   wrapper.outputlog = function( ) {
       for(var i=0; i<log.length;i++) {
       alert( [ ] . join.call(log[i].','))
}
}
   return wrapper
}
   work = makelogging(work)

   work(1,10)
   work(2.10)
   work.outputlog( )

```
运行后的结果是酱紫的。、
![0_1456740885505_upload-19803e33-a92f-47c4-aa5b-6a2812e3d94c](http://7xpvnv.com2.z0.glb.qiniucdn.com/6c1e708b-84b4-419c-8199-1564c758cb40) 

![0_1456740899478_upload-7467a253-43fb-46e3-bdbd-a9c704f07f72](http://7xpvnv.com2.z0.glb.qiniucdn.com/3466279b-260b-403a-877d-ca73aa882462) 

细节：
1. 通过日志实现。
2. 日志和向前的调用，包括`this`所以，如果`work`是一个对象的方法，一切都还好。
3. 我们的日志`arguments`这不是一个数组。所以，我们借`join`来实现。

- 使用一个包装器提供了一些不好的作用。所有函数的静态方法不能被访问。因为他们在功能上，这里是包裹：
```
work.a = 5
work = makelogging（work）
alert（work.a）
```
所以，静态方法和功能修饰符不是友好的朋友。