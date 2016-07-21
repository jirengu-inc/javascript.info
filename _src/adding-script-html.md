> 译者: 朱超然

# **在HTML中加入script标签**
**Ilya Kantor**
**Contributed:kichinsky**

 1. 页面渲染script标签
 2. 添加script标签到head标签中
 3. 添加script标签到body标签底部
 4. 外链文件
 5. 小结
 

一个script标签可以放在页面上的任何位置。最常用的位置包括:
 
* 放在HTML文档的head部分
* 放在body标签的底部
 
...但是,一般情况下script可以放在任何地方。
 
 当浏览器渲染HTML页面并找到一个<script>标签,它将会切换到javascript模式并在内部执行这些代码,一旦执行浏览器便继续渲染剩余的页面。
 
 ---
## **页面渲染script标签**

以下的例子示范了浏览器是怎么进入和退出JavaScript模式的
```javascript
<html>
<body>
  <h1>Counting rabbits</h1>

    <script>
        for(var i=1; i<=3; i++) {
            alert("Rabbit "+i+" out of the hat!")
        }
    </script>

    <h1>...Finished counting</h1>

</body>
</html>
//页面会先渲染出Counting rabbits,然后分别有三次alert,对应Rabbit 1 out of the hat!、Rabbit 2 out of the hat!、Rabbit 3 out of the hat!最后再渲染出...Finished counting。
```
注意,上面例子的执行顺序:
1.当页面开始渲染,只有文档的开始部分展示出来了。
2.当浏览器到了script标签这里就运行里面的代码,执行了3次alert。
3.当script脚本完成了,浏览器回到了HTML,只能在这之后页面剩余部分才能展现。

---
## **添加script标签到head标签中**

如果HTML非常大,jascript代码放在哪里比较合适呢?如果你想在页面展示之前去执行js代码,放在HEAD标签中比较合适。
```javascript
<html>
    <head>
        <script>
            for(var i=1; i<=3; i++) {
                alert("Rabbit "+i+" out of the hat!")
            }
        </script>
    </head>
    <body>

        <h2>Press the button to starts</h2>

        <input type="button" onclick="count_rabbits()" value="Count rabbits!"/>
        
    </body>
</html>
```
把scritp标签放在head标签部分是一种简单常见的方法,但是对于高性能要求的网站有另一种方法。

---
## **添加script标签到body标签底部**

script标签也可以放在body标签的底部,这种情况下js代码将在页面展现的时候再执行。

* 优点:用户不用等待js脚本加载
* 缺点:一些功能必须在HTML元素加载完之后才能运行。js加载前页面上的按钮点击可能没有效果,通常会加一些代码去隐藏功能性的那部分代码直到脚本加载完。

还有,根据HTML标准css样式必须声明在head标签中,只有script标签允许放在任何地方。

---
## **外链文件**

一般情况,大多数JavaScript代码放在一个添加到HTML的外部文件,比如：
```javascript
<script src="/path/to/script.js"></script>
```
```/path/to/script.js```是一个相对路径。如果你有一个包含完整URL的特殊位置,那就是绝对路径。相对路径就是相对你现在的位置。

```/path/to/script.js```中包含了JavaScript代码,当浏览器接收后会立即执行。

这个方法非常实用的,因为同样的文件可能在很多页面上都被应用到了,如果web服务器配置正确的话,浏览器将会缓存文件并前不会每次都去下载。

这里是一个例子:
```javascript
<html>
    <head>
        <script src="/files/tutorial/browser/script/rabbits.js"></script>
    </head>
    
    <body>
<input type="button" onclick="count_rabbits()" value="Count rabbits!"/>
    </body>
    
</html>
```
这里是/files/tutorial/browser/script/rabbits.js里的内容
```javascript
function count_rabbits() {
    for(var i=1; i<=3; i++) {
        alert("Rabbit "+i+" out of the hat!")
    }
}
```
注意,在文件中是没有SCRIPT标签的,只有在HTML中才有SCRIPT标签。

外联脚本和内嵌的一样会阻塞页面渲染,所以如果在HEAD中引入外联脚本,页面在脚本下载并执行之前是不会展现的。


**闭合```</script>```和XHTML**
注意,一般我们不能用XML的样式的自闭标签像```<script src="..."/>```即使声明了DOCTYPE XHTML。
必须有一个分开的闭合</script>,用```<script src="..."/>```替代将不会起作用。
如果你的服务器使用了```Content-Type:``` text/html,会使浏览器用“HTML-mode”解析页面```

增加多个脚本-用多个标签:
```
<script src="/js/script1.js"></script>
<script src="/js/script2.js"></script>
...
```

如果存在src属性则标签的内容将会被忽略

单独的```<script>```不能引入和执行外联脚本文件,需要分开的两个标签 ：一种有用作外联文件的src属性,另一种没有src属性但是其中存在代码,在文件后会执行。

**对于```<script>```标签的现代标记**

如今只有验证器(根据标准来检测页面正确性的工具)可以识别错误标记,有些时候人们用古老丑陋的代码,而且这些代码能运作。

虽然知道正确的标记是有用的,至少你可以区分专业的JavaScript代码和多年以前的一团糟的JavaScript代码。

**属性 ```<script type=...>```**
老的HTML4标准要求设置这个属性,但是HTML5允许属性空缺。这是一个前HTML5时代的值```type="text/javascript"```。

如果你在type里设置一个不支持的值,比如```<script type="text/html">```,标签中的内容就会被忽略。这是一种用来向页面添加不用渲染的数据的奇巧淫技。浏览器不会执行和展现识别不出类型的```<script>```,这类script就像是设置了style="display:none"的div。

**属性 ```<script language=...>```**
你可以在在过时的script文件中找到这个属性,它已经淘汰了并且不用于JavaScript。

**scripts前后的注释**
老的手册有时会推荐从浏览器上"隐藏"这些它不支持的JavaScript,通过HTML注释```<!-- ... -->```

要求这种方式的浏览器(网景的老浏览器)早已经不存在了。另一些浏览器则忽略这些注释。不要在代码中放这些,没有必要。一旦没有用就删除它们。

### 练习:
用外链文件方式改造"rabbits"例子

* 打开在线文件:[rabbits_ext.html](http://javascript.info/files/tutorial/browser/script/rabbits_ext.html)。

* 将HTML和JS文件放到你的硬盘上并使之能打开、按钮能起效。

注意,如果你从你的文件系统打开html而不是从一个网站上,"/my/script.js"会是从文件根目录开始的一个绝对路径。

你也可以在HTML中用相对路径比如"script.js"引入同一个文件夹下的脚本文件,或者是"my/script.js"用于HTML所在文件夹的子文件夹。

---
## **小结**

可以使用SCRIPT标签直接嵌入js脚本或者用带有```src="path/to/file.js"```的SCRIPT添加外链文件, 外链文件中是纯JavaScript代码。

不管怎么样,HTML页面的渲染将会被阻塞知道script下载并执行完。

这就是为什么高明的方法是把scripts放在body标签的底部,但这种情况下用户可以在JavaScript加载完成之前和页面交互。

把scripts放在HEAD标签中是保证他们可以在页面展现之前就可用的一种简单方法。