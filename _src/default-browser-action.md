> 译者: 宇卿

对于一些特定的事件，浏览器有它默认的行为。

例如：

- 点击链接会进行跳转
- 点击鼠标右键会呼出浏览器右键菜单
- 填写表单时按回车会自动提交到服务器，等

但我们自己处理点击事件时，默认的浏览器的行为往往是没有必要的。在大多数情况下，应该在事件处理函数中取消这种默认行为。


### 取消浏览器默认行为

一般有两种方法取消浏览器默认行为。

对于 兼容 W3C 标准的浏览器，我们可以使用 event 的特殊方法： `event.preventDefault()`，对于 IE < 9 的版本可以使用 `event.returnValue = false`。

兼容各种浏览器的代码示例：

```
element.onclick = function(event) {
    event = event || window.event 

    if (event.preventDefault) {  // W3C variant 
        event.preventDefault()
    } else { // IE<9 variant:
        event.returnValue = false
    }
}

```
或者可以写成一行：

```
..
event.preventDefault ? event.preventDefault() : (event.returnValue=false)
...
```

另外一种方法是在时间处理函数中返回 false；

```
element.onclick = function(event) {
  ...
  return false
}

```

返回 false 的方法更加简单也非常常用，但是 `event.preventDefault()` 方法没有结束时间的处理，因此它也有特殊的用途。

下面的例子中，链接被点击后并没有触发默认的跳转事件，因为它默认的跳转行为被取消了。

`<a href="/" onclick="return false">Click me</a>`

> ### jQuery 框架中 `return false` 
> jQuery 有他自己的事件处理策略。它封装了原生的事件处理函数，如果事件处理函数返回了 `false`，jQuery的事件处理函数会同时取消事件冒泡和默认的事件行为。
> 这和一般的浏览器行为不太一样，常常会引起混淆。
> 通常来说，浏览器仅仅取消默认的事件行为，并不会阻止事件冒泡，要注意两者的区别。


### 浏览器的『预处理』行为

浏览器在执行时间处理函数之前会有一些预处理的行为，这些行为是不能被取消的。

例如：当一个链接被选中后，大多数浏览器会在它周围加上虚线边框。

这种行为法中在 `focus` 事件触发之前，所以并不能再事件处理函数：`onfocus` 中取消。

相反，URL变化这种行为发生在 `focus` 事件触发之后，因此是可以被取消的。

```
<a href="/" onclick="return false" onfocus="return false">I get outlined on click, but don't go anywhere</a>

I get outlined on click, but don't go anywhere
```

> ### 冒泡和默认行为
> 浏览器默认行为和冒泡行为是独立的
> 取消事件默认行为是不会取消事件冒泡的，反之亦然

### 事件处理函数是独立的

一个元素的多个事件处理函数是完全独立的，调用的顺序也没有明确的规定。

下面的例子中，有一个 `TestStop` 按钮，这个按钮绑定了两个事件处理函数：`stop` 用来尽可能阻止事件，另外一个是 alert 事件处理函数。

但是点击按钮后，`alert` 总是会被触发：

```
<input type="button" id="TestStop" value="Test stop"/>

<script>
  elem = document.getElementById('TestStop')

  function stop(e) {
    e.preventDefault() // browser - don't act!
    e.stopPropagation() // bubbling - stop
    return false // added for completeness
  }

  elem.addEventListener('click', stop, false)

  // this handler will work
  elem.addEventListener('click', function() { alert('I still work') }, false)
</script>

```

### 总结

- 对于某些事件，浏览器有它自己默认的处理行为。大多数行为可以被取消。
- 通常有两种取消默认行为的方法：使用 `preventDefault()/returnFalse` 或者 在事件处理函数中 `return false`，两者是等价的。
- 默认的浏览器行为和冒泡行为是独立的。同一个元素的多个事件处理函数也是相互独立的。