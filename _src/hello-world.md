> 译者: 周鹭江

## 你好，世界！

JavaScript是一门解释型语言，这意味着不用编译，你只要编写代码，并将其添加到HTML中。

最简单的方法是把JavaScript的指令添加到页面中的某处`SCRIPT`标签中。

下面是一个`SCRIPT`标签，包含简单的JavaScript声明，请点击’Show’按钮查看例子。
```
	<!DOCTYPE HTML>
	<html>
	<body>
	 
	  <p>Header...</p>
	 
	  <script>
	    alert('Hello, World!')
	  </script>
	 
	  <p>...Footer</p>
	 
	</body>
	</html>
```
注意`SCRIPT`内容部分并没有显示，但是它执行了。

这个例子使用了下面的元素：
`<script> ... </script>`
`<script>` 标签告诉浏览器，这里有一个可执行的内容。在旧的HTML标准，即HTML4，一个特殊的属性`类型`是必要的，尽管更多最新的HTML5使得它可选的。

浏览器：
1. 开始显示页面，显示`Header`
2. 切换到该标签内的JavaScript模式，并执行<script>中的内容
3. 返回HTML模式，继续页面，输出Footer

`alert(...)`
在窗口上输出一个消息，并等到访问者按OK为止。
不仅阅读，但实际测试并且亲自做是很重要的。下面你可以看到一个“检查你自己”的区域，它包含一个任务和解决方案。
履行任务来保证你得到一切正确。
* * *
创建一个输出“我是JavaScript！”的页面，保存文档并在浏览器中打开，确保一切正常运行。
或者，换句话说，创建的页面应该看起来像：
http://lookatcode.com/files/tutorial/browser/script/alert/index.html `[备注3]`
solution
* * *
打开给出例子的源码或者下面的：
http://javascript.info/play/tutorial/browser/script/alert/index.html `[备注3]`

下一页，我们将更深入例子，并用基本脚本构建模块。