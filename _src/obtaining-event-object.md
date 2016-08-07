> 译者: 侯小威

事件对象会被传递给事件处理函数，它包含着大量有用的信息。

其中一种属性是通用的：

>event.type-事件的种类。如"click"

不同的事件类型提供着不同的属性。比如，onclick事件对象就包含：

>event.target-所点击元素的引用。IE用event.srcElement替代。

>event.clientX / event.clientY-在点击的瞬间，鼠标的坐标。

被点击按钮的其他信息和包含属性，我们将在后面详细介绍。

现在我们的目标是获取事件对象，有两种方法：

### W3C方式

遵循W3C标准的浏览器通常将事件对象作为第一个参数传递给处理函数，例子如下：

		element.onclick = function(event) {
		// 处理事件对象带来的数据
		}
		
也可以用一个命名函数：

		function doSomething(event) {
		
		// we've got the event
		
		}
		
		element.onclick = doSomething
		
		
也可以在标记事件处理函数中用一个event变量：

		<button onclick="alert(event)">See the event</button>（原文此处为展示按钮）

这段代码可以正常展示，浏览器将会根据属性值自动建立一个处理函数，并将event作为其中的参数。

### IE方式

IE将提供一个全局对象-window.event，IE9之前没有事件代理函数没有参数。如下：

		//函数没有参数
		element.onclick = function(){
		//window.event-是一个event对象
		}

### 跨浏览器解决方案

通常获取事件对象的方式是

		element.onclick = function(event){
			event = event||window.event
			//现在这个event在所有浏览器中都是event对象
		}
		
内嵌变种
***

所有符合标准的非IE浏览器和IE都允许通过在HTML标记内访问event对象。

		<input type="button" onclick="alert(event.type)" value="Alert event type"/>

对于老版IE，事件处理函数没有参数，所以事件对象绑定在全局环境中。对其他浏览器来说，事件处理函数将接受event对象作为第一个参数，所以，在标记内的函数中的event对象是可以跨浏览器传递和访问的。

### 小结

事件对象包含着关于事件的一系列重要且详细的信息。

在非IE浏览器中它将作为事件处理函数中第一个参数传递给函数，在IE浏览器中，将通过window.event传递。

对一个JS函数，我们可以：

		element.onclick = function(event){
			event = event || window.event
			//现在可以跨浏览器传递event对象了。
		}

对于HTML标记内嵌的函数，一个event就可以跨浏览器。