> 译者: 侯小威

绝大多数JavaScript的应用程序的执行，都是由浏览器事件来触发。

事件就是用来传递浏览器或文档正在或已经发生了一些特定的交互的信号。

JS中有很多种事件类型：

>DOM事件:其通过DOM元素来触发。例如，点击一个元素会触发click事件，当鼠标悬停到一个元素上方时会触发mouseover事件
	
>窗口事件:例如当浏览器窗口尺寸变化时会触发resize事件。
	
>其他事件:比如load,readystatechange。你在AJAX中会经常看到它们。

DOM事件是连接JS代码与HTML文档的桥梁，在两者间建立起一个动态的接口。

## 指定事件处理程序

响应某个事件的函数（一段脚本），就叫事件处理程序。

它们经常以“on+事件类型”的形式示人，比如onclick。

>JS中的事件处理是单线程的，所以相应的处理程序会按顺序执行，这意味着如果出现两个事件同时发生的情况时，比如mouseover(鼠标既悬停在某元素的上方)和mousemove(鼠标也在这个元素上方移动时)，它们对应的事件处理程序将会按顺序依次执行。
	
有好几种指定事件处理程序的方法，下面是对每种方法的全面详述。

### HTML标签内嵌属性法
***
一个事件处理程序可以直接写在HTML标记中，你可以认为它就是一个元素的属性，以onevent的格式。

比如想给一个input按钮指定一个click事件，可以像下面这样编写代码：

	<input id="b1" value="Click me" onclick="alert('Thanks!')"; type="button"/>

效果如下：

	<input id="b1" value="请点击" onclick="alert('Thanks!')"; type="button"/>（原文为可点击的click按钮）

请注意上例中双引号中内嵌了一对儿单引号。菜鸟常犯的错误就是忽略了这种书写形式的本质：就是一个属性名值对儿。切勿忘了加引号的同时别忽略单双引号的适用场景。	
	onclick="alert("Click")"会因错误识别后引号而无法执行。如果真的想实现，可以试试将引号转义：为"&quot"。但在你熟悉了绑定事件的其他方法后，会觉得这种方式实在麻烦。

通常会给事件绑定一个函数的调用。下例中的button被点击时，会调用count_rabbits()：
	
	<!DOCTYPE HTML>
		<html>
  			<head>
    			<script type="text/javascript">
      				function count_rabbits() {
        				for(var i=1; i<=3; i++) {
          				alert("Rabbit "+i+" out of the hat!")
        				}
     				 }
    			</script>
 			</head>
 			<body>
    			<input type="button" onclick="count_rabbits()" value="Count rabbits!"/>
 			</body>
		</html>

请记住，HTML标签的属性名是不区分大小写的，oNcLiCk和onClick或者onclick都可以正常工作。但人们将一律小写看做良好的书写习惯。

#### 什么时候适用此种方法：

如此指定处理函数显得非常方便，简单的内嵌，常用来处理需求非常单一的场景。

当然内嵌法还是有很明显的缺陷，当处理函数非常长的时候，代码的阅读性将变得非常差。

所以几乎没人会把复杂的函数直接嵌入HTML标签中。

>内嵌法优点：处理简单情况非常方便。

>内嵌法缺点：耦合了JS代码和HTML标记、复杂函数难以阅读。

#### this对象
***
虽然这种绑定事件的方法不被推荐，但是我们还是来探究一下this在里面的指向。

**在事件处理函数中，this指向当前的元素。**

如下代码通过this.innerHTML来输出button的内容。

	<button onclick="alert(this.innerHTML)">Click me to see me</button>（原文为可点击的click按钮）
	

### 用DOM对象的属性来绑定事件处理函数
***
与上述方法长的最像的，就是给DOM对象直接指定一个onevent形式的属性。

你需要做到：

1. 获取一个元素
2. 给onevent形式的属性指定一个函数。

下方是给一个id="myElement"的DOM对象指定click函数的例子：
	
	<input id="myElement" type="button" value="Press me"/>

	<script>

	document.getElementById('myElement').onclick = function() {

    alert('Thanks')

	}

	 </script>

需要注意两点：

1. onclick是一个property，而不是一个attribute。所有字母必须小写，比如onClick是不能正常执行的。
2. 绑定的必须是一个处理函数，不能是一个字符串。
译者注：property和attribute的详细区别，请移步[https://www.zhihu.com/question/40064849](https://www.zhihu.com/question/40064849)

如果浏览器在HTML标记中解析到了on...开头的属性，本质上就是基于绑定的内容创建一个函数并把它指定给它的属性。

下面两行代码是一样的：

1. 只有HTML标记：
	
		<input type="button" onclick="alert('Click!')" value="Button"/>

2. HTML+JS:

		<input type="button" id="button" value="Button"/>
		
		<script>
		
			document.getElementById('button').onclick = function() {
			
			alert('Click!');
			
			}
						
		</script>

如果标记中已绑定事件处理函数，JS代码将会覆盖它。如下示例：

		<input type="button" onclick="alert('Before')" value="Press me"/>

		<script>

		document.getElementsByTagName('input')[0].onclick = function() 		{
			alert('After')
		}
		</script>
		
当然，也可以绑定一个已存在的函数：

		function doSomething(){
			alert("Thanks");
		}
		
		document.getElmentById("button").onclick = doSomething;
		
> #### 菜鸟陷阱：

 此方法给事件绑定的应该是个不带括号的函数名。比如上例中绑定的是doSomething,而不是doSomething()。

 doSomething():是一个立即执行的函数，在没有返回值的情况下，将返回一个undefined。此例中就直接将undefined赋给了onclick事件。

 对比来看HTML内嵌绑定的方式，是需要括号的：

		<input type="button" id="button" onclick="doSomething()"/>

 两者之间的差异很好理解：当浏览器解析到onclick属性时，会自动基于属性值建立一个函数，所以上例等同于下面的形式：

		document.getElementById('button').onclick = function(){			doSomething()}

译者注：更详细的解析可以移步下面这篇答案：https://www.zhihu.com/question/25835666

#### 何时使用

给DOM对象的属性指定一个事件处理函数的方法是非常简单的，也是最为流行的。但还是存在致命问题：一个特定的事件只能绑定一个事件处理函数。

比如：

		input.onclick = function() { alert(1) }
		// ...
		input.onclick = function() { alert(2) } // 将覆盖上一个绑定的函数
 
>属性法优点：简单又可靠

>属性法缺点：一个事件只能绑定一个事件处理函数。

当然，也可以在一个通过在事件处理函数里嵌套其他函数来达到绑定多个的效果，但远不如下面要介绍的这种更高级的绑定方法来的方便。
 
### 特殊方法
***

在一个普通的JS应用程序中，给同一个事件绑定不同的接口组件的场景是非常常见的。

一个经典的场景就是同一个页面的不同图片组件要等"document loaded"事件加载完毕后才能初始化。

#### 微软的解决方案：

这个方案由微软发明，且只适用于IE9以下的浏览器。

它也被Opera浏览器兼容支持，但是开发者很少用它，因为Opera也支持我们在下一章节要介绍的非IE解决方案。

添加一个事件处理函数：

		element.attachEvent( "on"+event, handler)

移除一个事件处理函数：
	
		element.detachEvent( "on"+event, handler)

如下例所示：

		var input = document.getElementById('button')

		function handler() {
	
    		alert('Thanks!')

		}

		input.attachEvent( "onclick" , handler) // 添加一个handler

		// ....

		input.detachEvent( "onclick", handler) // 移除handler

> #### 菜鸟陷阱：
通过attachEvent()添加的事件可以通过detachEvent()来移除，条件是必须提供相同的参数。这也意味着添加的匿名函数将不能被移除。下例将是错误的：

		input.attachEvent( "onclick" ,function() {alert('Thanks')})
		// ....
		input.detachEvent( "onclick",function() {alert('Thanks')}

在上面的例子中，两个匿名函数只是长的一样，实际是两个不同的引用。

因此，如果计划在未来做删除处理，一定要给函数起个名或者赋给一个变量。

使用attachEvent，可以给同一个事件绑定多个事件处理函数，下面的示例将只能在IE和Opera上正常运行：

		<input id="myElement" type="button" value="Press me"/>
			
			<script>
				
				var myElement = document.getElementById("myElement")
				
				var handler = function() {
					
					alert('Thanks!')}
				
				var handler2 = function() {
					
					alert('Thanks again!')}
				
				myElement.attachEvent("onclick", handler)
				
				myElement.attachEvent("onclick", handler2)

			</script>

#### WARINING:
attachEvent方法有一些特殊的地方：绑定的处理函数内部要避免使用this，因为this始终指向window。

#### W3C标准解决方案：

W3C官方认证的绑定事件处理函数的方法对所有非IE的主流浏览器都适配，IE则支持IE9及以上。

绑定一个事件处理函数：

	element.addEventListener(event,handler,phase)

解绑一个事件处理函数：

	element.removeEventListener(event,handler,phase)

需要注意，事件名中并没有前缀“on”。

与微软解决方案的不同的是，W3C的方案还有第三个布尔值参数，可省略，默认为false。

书写格式基本与attachEvent一致：

		elem.addEventListener( "click" , handler, false) // 指定一个事件处理函数

		// ....

		elem.removeEventListener( "click", handler, false) // 移除一个事件处理函数
		
综上，这两种方法的优点就是你可以绑定任意多的事件处理函数，但同样存在一个致命缺点：对跨浏览器的支持不足。

### 函数执行顺序
***

特殊方法准许给同一个事件绑定多个事件处理函数，但浏览器不能保证他们的执行顺序。所以，函数的内部逻辑一定不要依赖于执行顺训，否则会出现意外的错误。

### 跨浏览器解决方案
***

最简单的解决方案是自定义一个函数，自动识别浏览器种类，使用相应的绑定方法：

		if (document.addEventListener) {
		
		var addEvent = function(elem, type, handler) {

        elem.addEventListener(type, handler, false)

    }

    	var removeEvent = function(elem, type, handler) {

        elem.removeEventListener(type, handler, false)}

		} else {

    	var addEvent = function(elem, type, handler) {

        elem.attachEvent("on" + type, handler)

    	}

    	var removeEvent = function(elem, type, handler) {

        elem.detachEvent("on" + type, handler)

    	}

	}

 

		...

	addEvent(elem, "click", function() { alert('hi') })
	
上例的调用可以应付大多数情况，但注意在IE中，函数内不要用this，因为this总是指向window。

弥补这个问题看起来容易，但实际上并不是这么回事。但实际上涉及诸如内存泄露等较为进阶的主题。如果你不使用this也不在乎内存泄露，那确实有很多即简便又高效的解决方法。

# 小结

JS中有三种绑定事件处理函数的方法：HTML标记内嵌、onevent属性法、特殊方法。