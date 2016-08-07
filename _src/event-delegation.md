> 译者: 宇卿

事件委托巧妙利用了事件冒泡的特性简化了事件的处理。是一种非常重要 javascript模式。

> ### 什么是委托
> 如果一个父元素中有很多个子元素，如果你希望处理这些子元素上发出的事件，那么请不要把事件处理函数把绑定在每>> 个子元素上，而是在父元素上绑定，并通过 `event.target` 追踪产生事件的子元素。

### 八卦阵的例子

例如，现在有一个八卦阵，这是中国古时一种哲学上的表格。

现在点击任意单元格，都会被点亮。

这个表格包含 9 个单元格。每个单元格内有文字和一个 `strong` 标签用来格式化方向：`South,North` 等。源代码在  tutorial/browser/events/delegation/bagua/index.html 上。

这里重要的是，点亮是如何实现的？

这个例子用到了事件委托的思想。我们并没有给每个单元格都绑定一个事件处理函数，而是给整个表格绑定了一个事件处理函数，使用 `event.target` 得到发出事件的那个元素。

```
table.onclick = function(event) {
  event = event || window.event
  var target = event.target || event.srcElement
  // ...
}
```

点击事件可以从表格中任意标签上发生。例如 `<strong>` 标签，事件发生后一直向上冒泡：

我们可以沿着 `parentNode` 链找到所有需要点亮的单元格：

```
table.onclidk = function(event) {
  event = event | window.event
  var target = event.target || event.srcElement
  
  while(target != table) {
    if(target.nodeName == 'TD' ) {
      toggleHighlight(target)
    }
    target = target.parentNode
  
  }
}
```

上面的代码遵循了常规的事件委托的思想：

1. 在事件处理函数中，我们可以获得触发事件的 `target`。这可能是一个 `TD`, 也有可能是其他元素，比如 `strong`。点击也可能在单元格之间产生，这个例子中，`TR` 和 `TABLE` 都可能是事件的产生源。
2. 我们沿着父节点链向上查找，一直到我们找到 `TD` 或者触碰到 `TABLE` 边界元素。
3. (*) 如果我们碰到 `TD`，就处理它
4. (*) 反之，意味着我们在单元格之间或者表头做了点击操作，这种情况就不用处理 

### 菜单的例子

事件委托能够很优雅地处理树形结构和嵌套的菜单。

首先我们来讨论一个列表中的一层可点击的菜单：

```
<ul id="menu">
  <li><a class="button" href="/php">PHP</a></li>
  <li><a class="button" href="/html">HTML</a></li>
  <li><a class="button" href="/javascript">JavaScript</a></li>
  <li><a class="button" href="/flash">Flash</a></li>
</ul>
```

上面的这个例子仅仅是简单的 HTML/CSS 语法。 由 Javascript 来处理点击菜单元素的事件，为了保证在没有 javascript 的情况下依然可以访问，每一项都用 A 标签表示。

像下面的例子一样，链接都是可以点击的。

> ### Mouse over(hover) 状态
> 上面菜单中每项的 Hover 状态是由纯 css 实现的。
> 至今为止，所有浏览器都支持这种特性。IE6 中对任意元素的 hover 状态支持有所欠缺，但对于 A 标签是没有问题的。

通过事件代理，我们仅需给整个菜单设置一个事件处理函数：

```
document.getElementById('menu').onclick = function(e) {
  e = e || event   
  var target = e.target || e.srcElement 

  if (target.nodeName != 'A') return
    
  var href = target.href
  alert( href.substr(href.lastIndexOf('/')+1) )

  return false // prevent url change
}
```

这段代码并没有向上查找 `parentNode` 链，因为在 A 标签中不存在嵌套的标签。

完整的代码示例参考：tutorial/browser/events/delegation/menu/index.html.

### 嵌套菜单的例子

嵌套菜单的文档结构也是比较类似的：

```
<ul id="menu">
<li><a class="button" href="/php">PHP</a>
  <ul>
    <li><a href="/php/manual">Manual</a></li>
    <li><a href="/php/snippets">Snippets</a></li>
  </ul>
</li>
<li><a class="button" href="/html">HTML</a>
  <ul>
    <li><a href="/html/information">Information</a></li>
    <li><a href="/html/examples">Examples</a></li>
  </ul>
</li>
</ul>
```
在 css 的帮助下，二级 ul 标签可以设置为隐藏的，直到有鼠标移动到外层的 li 元素时才会显示。

嵌套的菜单实际上并没有修改任意一行 javascript 代码。

完整的代码示例参考：tutorial/browser/events/delegation/menu-nested/index.html.

我们也可以在运行时动态添加删除菜单项，并不需要变动事件处理函数。

我们甚至可以把菜单中某些项替换掉，事件处理函数仍然不需要做任何变动。一切都归功于事件委托。


### 标记上的行为

表格单元格和嵌套菜单都是对同一种事件进行处理的例子。由于子元素上发生的都是同一种事件，用事件委托可以很好地应对这种情况。

而但对于那些完全不同的事件行为，事件委托同样可以很好地处理。

例如，现在我们要创建一个菜单，包含几种不同类型的按钮：保存，加载，查找等。

显而易见的做法是给每个按钮都绑定一个事件处理函数，更加聪明的做法是给整个菜单绑定一个事件处理函数。菜单内部的点击事件都委托给这个唯一的事件处理函数来处理。

那么问题来了，在这个唯一的事件处理函数中我们如何去辨别是哪个按钮被点击了呢？为了达到这个目的，我们可以把事件触发的方法绑定在每个按钮的自定义的 `data-action` 属性上。

```
<button data-action="Save">Click to Save</button>
```

事件处理函数获得 data 属性后执行对应的方法，看下面的例子：

```
<div id="menu">
  <button data-action="Save">Click to Save</button>
  <button data-action="Load">Click to Load</button>
</div>

<script>
function Menu(elem) {
  this.onSave = function() { alert('saving') }
  this.onLoad = function() { alert('loading') }

  var self = this

  elem.onclick = function(e) {
    var target = e && e.target || event.srcElement
var action = target.getAttribute('data-action')
    if (action) {
      self["on"+action]()
    }
  }
}

new Menu(document.getElementById('menu'))
</script>
```

这里要注意 `var self = this` 这个技巧用来保存对 `Menu` 对象的引用。否则，事件处理函数中就没有办法调用 `Menu` 的方法了，因为此时 `this` 表示的是触发事件元素的引用。


> #### data-* 属性
> Html5 标准中允许 html 元素拥有形如 `data-...` 的属性。
> 这是[标准相关的章节](http://dev.w3.org/html5/spec/Overview.html#embedding-custom-non-visible-data-with-the-data-attributes)。浏览器端也提供了操作这种属性的 API，但并不是所有浏览器都支持。
> 因此，至今为止，使用 `data-*` 属性主要为了验证 html5 的可用性和未来的兼容性。

这里我们为何使用事件委托？

>- 不需要给每个按钮都绑定事件处理函数。更少的代码量，更少的初始化事件
>- HTML 代码结构变得更为灵活。任何时候我们都可以新增或者删除按钮。所有按钮的触发方法只需要遵循 `onMethod`，非常简单。
>-这种方法和语义化标签无缝连接。我们可以使用类似 `action-save`, `action-load` 这样语义化的类来修饰我们的元素，而避免使用 `data-action` 这种非语义化的属性。事件处理函数会检查 `action-` 这样的类并调用对应的方法。真的非常简单

### 总结

事件委托非常棒。它是最有用的 javascript 设计模式之一。

使用的前提是一个单独的容器内有一些元素，这些元素有一些类似的行为。

算法流程：

1. 在容器上绑定事件处理函数
2. 在事件处理函数中获得触发事件的具体元素：`event.target`.
3. 有必要的话，向上遍历 `target.parentNode` 链，直到匹配道第一个匹配的元素，或者匹配容器(`this`)元素。

使用场景：

- 给子元素的公共行为定义一个唯一的事件处理函数
- 如果事件行为可以从触发事件的元素中得到，对于不同事件行为，可以简化代码结构。


优点：

- 简化初始化流程，避免额外事件处理函数带来的内存消耗
- 增加灵活度，便于升级和维护
- 可以使用 `innerHtml`，而无需额外的处理。

当然，和任何模式一样，事件委托也有一些局限。

- 首先，事件必须冒泡，在 IE 中大多数事件会冒泡，但不绝对。对于其他浏览器，捕获的过程同样适用
- 其次，事件委托理论上增加了浏览器的额外开销，因为当容器内任意元素触发了事件，容器的事件处理函数都会被调用，而很多时候，事件处理函数在做无用功。当然，这不是什么太大的问题。