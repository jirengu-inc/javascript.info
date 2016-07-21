# **浏览器环境**
> 译者: 朱超然

**Ilya Kantor**
**Contributed:kichinsky**

 1. 全局结构
 2. 小结
 
浏览器给我们提供了一个可以控制和访问一个页面上的时间、屏幕、页面、元素等不同信息的对象分层结构。

---
## **全局结构**
浏览器给开发者们提供了操作一个庞大对象分层结构的访问权限,你可以在下图看到这个分成结构的一部分:
![](http://javascript.info/files/tutorial/browser/JSTop.png)
在顶端的是```window```,我们也叫做全局对象。
其他的对象来自三个部分。

### **文档对象模型**
```document```以及与之有关的对象有访问页面内容和修改元素的权限, 大部分和HTML的交互都是在这捕获的。

这是W3C为DOM开发的一个标准包。你可以在[W3C DOM](https://www.w3.org/DOM/DOMTR)的页面上找到它。有三个级别的DOM,每一个级别都在前一级的基础上扩展。现代浏览器普遍都支持W3C之前混乱时代的特征,称为DOM 0。

### **浏览器对象模型**
BOM是一个对象包,可以用来控制浏览器等,改变当前的URL,访问frames属性,在后台用XMLHttpRequest向服务器端发请求等,像```alert```,```confirm```,```prompt```等一类函数也属于BOM,他们都是浏览器提供的。

许多BOM特性在HTML5中都标准化了,但并不是所有的。

### **JavaScript对象和函数**
JavaScript自身是一门语言,他给了我们访问DOM、BOM的权限和他的对象与方法。

JavaScript遵从ECMA-262标准。

全局的```window```对象混合了浏览器的window函数(```focus()```、```open()```等方法)和JavaScript的全局对象,这就是为什么在文章开始中的那张全局结构图中他同时显示绿色和红色。

---
## **小结**

虽然这些仅仅是些理论,但还是很高兴知道这些关系的。

建议读一些DOM资料和其他标准,因为对解决问题和教学都有帮助。

还有,顺便说一下,这些window下的组件都是可以分开来使用的。

* JavaScript是一个普遍的目标语言,你可以在命令行里使用他,建立一个[基于Node.js的服务端](https://nodejs.org/en/)。
* BOM和DOM可以通过浏览器的API由第三方程序或插件来控制,而不仅仅只有JavaScript。例如Webdriver```[备注1]```单元测试框架。
* DOM也用作XML文档,不只是HTML,当然不仅限于前端编程。

---
## **备注:**
```[备注1]:```原地址:http://docs.seleniumhq.org/docs/09_webdriver.html无法打开。