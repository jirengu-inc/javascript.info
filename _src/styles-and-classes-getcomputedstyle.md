> 译者: MsFu

[Ilya Kantor](http://javascript.info/users/ilya-kantor) 
1. [className](http://javascript.info/tutorial/styles-and-classes-getcomputedstyle#classname) 
2. [style](http://javascript.info/tutorial/styles-and-classes-getcomputedstyle#style)
1.[cssText](http://javascript.info/tutorial/styles-and-classes-getcomputedstyle#csstext)
2.[理解样式](http://javascript.info/tutorial/styles-and-classes-getcomputedstyle#reading-the-style)
3. [getcomputedstyle和currentstyle](http://javascript.info/tutorial/styles-and-classes-getcomputedstyle#getcomputedstyle-and-currentstyle)
4. [总结](http://javascript.info/tutorial/styles-and-classes-getcomputedstyle#summary)

这部分是关于理解及如何改变一个元素。
请确保你在阅读之前熟悉CSS盒模型。前提是知道什么是内边距,外边距,边框。className属性与类属性总是同步的。     




## className


className属性与类属性总是同步的。
例如下面的例子：


```javascript
<body class="class1 class2">
<script>
  alert(document.body.className)
  document.body.className += ' class3'
</script>
</body>
```
删除一个单独的类，它是通过字符串的替换功能或空字符串分割再组合得到的。
所有的JavaScript框架包含的内置函数都可以这样工作。

------
## style
样式属性可以访问元素样式的读写。
当使用它时，最可能改变CSS的属性。例如，element.style.width='100px' 的意思是如果元素有一个属性是style="width:100px"。
在CSS中一样,你需要提供单位:“px”、“pt”,“%”或“em”。

对于一个多单词属性，你需要使用骆驼拼写法。
background-color  => backgroundColor
z-index           => zIndex
border-left-width => borderLeftWidth

样式的值总是优先于CSS属性，所以你可以改变它，然后恢复改变，通过指定空字符串：elem.style.width=''将会改变宽的设置。
在下面的例子中演示了如何使用样式属性:

例如：
```javascript
// enter an empty value to reset the color
document.body.style.backgroundColor = prompt('background color?', 'green')
```

![0_1455523967850_QQ截图20160215160526.png](/uploads/files/1455523967931-qq截图20160215160526.png) 
特定于浏览器的样式属性,如-moz-border-radius、-webkit-border-radius分配是这样的: 
button.style.MozBorderRadius = '5px'
button.style.WebkitBorderRadius = '5px'

当然,最好是让他们在类中保留，在 elem.className中通过添加/删除元素。

------

## cssText
属性style.cssText 允许读/写整个声明:
```javascript
<div>Button</div>

<script>
  var div = document.body.children[0]
 
  div.style.cssText='color: red !important; \
    background-color: yellow; \
    width: 100px; \
    text-align: center; \
    blabla: 5; \
  '

  alert(div.style.cssText)
</script>
```
浏览器解析cssText并适用它，对于未知的属性没有特例，所以大部分浏览器会忽略它。
在上面的示例中,cssText在火狐浏览器中不会显示特性。
style.cssText 是唯一可以添加!important.

------
## 理解样式
样式只给访问属性设置,或“样式”属性。
所以,在下面的示例中，marginTop将不成功:

```javascript
<style>
  body { margin: 10px }
</style>
<body>
  <script> 
    alert(document.body.style.marginTop) 
  </script>
</body>
```
这是因为margin外边距是使用了css级联的结果，不是样式。
如果你用样式设定，那么它是这样工作的：
```javascript
<style>
  body { margin: 10px }
</style>
<body>
  <script> 
    document.body.style.margin = '20px'
    alert(document.body.style.marginTop) 
  </script>
</body>
```
注意从上面的示例浏览器如何解包边缘属性,提供可读的子属性。同样适用于 border，background 等。
在下面的示例中,颜色在火狐浏览器中显示为rgb(…)。
```javascript
<body style="color:#abc">
  <script> 
    alert(document.body.style.color)  // rgb(170, 187, 204)
  </script>
</body>
```
以下为最常用的样式属性。
![0_1455524359990_QQ截图20160215161851.png](/uploads/files/1455524360336-qq截图20160215161851.png) 
阅读文件[ tutorial/browser/dom/roundedButton/source.html](http://javascript.info/play/tutorial/browser/dom/roundedButton/source.html)通过纯JavaScript的样式创建a链接，不使用HTML的任何标签：
```javascript
<!DOCTYPE HTML>
<html>
<head>
<style>
.button {
  -moz-border-radius: 8px;
  -webkit-border-radius: 8px;
  border-radius: 8px;
  border: 2px groove green;
  display: block;
  height: 30px;
  line-height: 30px;
  width: 100px;
  text-decoration: none;
  text-align: center;
  color: red;
  font-weight: bold;
}
</style>
</head>
<body>

<a class="button" href="/">Click me</a>

</body>
</html>
```
[![0_1455524531072_QQ截图20160215162128.png](/uploads/files/1455524531502-qq截图20160215162128.png) ](http://javascript.info/)

[在新窗口打开代码](http://javascript.info/files/tutorial/browser/dom/roundedButton/task.html)

同时，自我检查：记住每个样式属性是什么意思,为什么这样描述。
![0_1455524643206_QQ截图20160215162346.png](/uploads/files/1455524643605-qq截图20160215162346.png) 
------

## getComputedStyle 和currentStyle
样式可以通过elem.style使用。
在下面的例子中,样式没有告知任何关于margin在css中的定义。
```javascript
<style>
  body { margin: 10px }
</style>
<body>

  <script> 
    alert(document.body.style.marginTop) 
  </script>

</body>
```
例如我们想使用animation，并给margin增加10px。怎么做？首先，我们需要得到现在的值。在css和样式应用后理解真正的属性，DOM 标准2标准描述了 window.getComputedStyle 方法。

语法是：
 getComputedStyle(element, pseudo)
**element**  获得元素的样式
**pseudo**  伪元素选择器例如“hover”或者null如果是不必要的。

所有浏览器都希望IE9以下都支持它。下面的代码在非IE中可以运行。
```javascript
<style>
  body { margin: 10px }
</style>
<body>

  <script> 
    var computedStyle = getComputedStyle(document.body, null)
    alert(computedStyle.marginTop) 
  </script>

</body>
```
在IE中有一个属性 currentStyle和这个是非常相似的。
容易犯错误的是currentStyle返回的值是“as-is”，单位是css而不是像素。
```javascript
<style>
  body { margin: 10% }
</style>
<body>
  <script> 
    if (window.getComputedStyle) {
      var computedStyle = getComputedStyle(document.body, null)
    } else {
      computedStyle = document.body.currentStyle
    }
    alert(computedStyle.marginTop) 
  </script>
</body>
```
1、在Firefox中,像素可能是小数。
2、其它浏览器四舍五入像素。
3、IE保持单位。上面的例子给的都是百分数。

如果你的值是用的像素，IE和其它浏览器一样。

![0_1455525149805_QQ截图20160215160526.png](/uploads/files/1455525149949-qq截图20160215160526.png) 
**IE:将“pt、em %”转化为像素**
有时候css用的是其它单位而不是像素，比如百分数或者em。我们需要将像素来计算，因为不可能将10px和10%来相加。
为了得到一个像素的值而不是百分数，这有一个runtimeStyle+pixel，由Dean Edwards描述。
它只基于IE runtimeStyle 和 pixelLeft。
在下面的示例中,getCurrentPixelStyle(elem，prop)为urrentStyle属性得到一个像素值。
如果你对它如何工作很感兴趣，那么仔细阅读代码，检查MSDN里的runtimeStyle和pixelLeft属性。
```javascript
function getCurrentPixelStyle(elem, prop) {
  var value = elem.currentStyle[prop] || 0

  // we use 'left' property as a place holder so backup values
  var leftCopy = elem.style.left
  var runtimeLeftCopy = elem.runtimeStyle.left

  // assign to runtimeStyle and get pixel value
  elem.runtimeStyle.left = elem.currentStyle.left
  elem.style.left = (prop === "fontSize") ? "1em" : value
  value = elem.style.pixelLeft + "px";

  // restore values for left
  elem.style.left = leftCopy 
  elem.runtimeStyle.left = runtimeLeftCopy 
 
  return value
}
```
下面的功能（只在IE）：
```javascript
<style> #margin-test { margin: 1% } </style>
<div id="margin-test">margin test</div>

<script>
  var elem = document.getElementById('margin-test')
  if (elem.currentStyle) // IE
    document.write(getCurrentPixelStyle(elem, 'marginTop')) 
  else 
    document.write('Open the page in IE please')
</script>
```
Margin测试
请在IE中打开页面
现代JavaScript框架使用它，对于IE浏览器它是不同的，getComputedStyle是对于可以使用它的浏览器。
## 总结  
所有DOM元素提供以下属性。

* className属性与类属性总是同步的。  

* CSS 骆驼拼写的样式属性是一个对象。它的工作原理对读写很好。它允许读属性,被样式设定,但不是在CSS中定义的属性。

* cssText允许获取/设置样式文本形式的全部值。你不能添加!important样式属性。cssText——可以。
* currentStyle属性(IE)和getComputedStyle(标准)方法允许在所有样式应用后，取得现有的样式属性的值。

特别指出:
1. Firefox可以返回一个分数值。
2. IE返回给定单位的值,而不是翻译成像素。但有一个函数可以。