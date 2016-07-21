# DOM节点
> 译者: 丁贝津

Ilya Kantor

贡献者：kichinsky

1、[DOM的概念](http://javascript.info/tutorial/dom-nodes#an-idea-of-dom)
1. [另一个文档](http://javascript.info/tutorial/dom-nodes#another-document)

2、[运用开发者工具运行DOM](http://javascript.info/tutorial/dom-nodes#walking-dom-using-developer-tools)
3、[空格节点](http://javascript.info/tutorial/dom-nodes#whitespace-nodes)
4、[其他节点类型](http://javascript.info/tutorial/dom-nodes#other-node-types)
1. [文档类型](http://javascript.info/tutorial/dom-nodes#doctype)
2. [评论](http://javascript.info/tutorial/dom-nodes#comments)

5、[总结](http://javascript.info/tutorial/dom-nodes#summary)

DOM通过树的形式来表现一种文档。树是由父-子关系组成的，一个父亲可以有一个或多个子节点。

## DOM的概念
让我们首先考虑一下如下的HTML：
```
<html>
  <head>
    <title>The title</title>
  </head>
  <body>
     The body
   </body>
</html>
```
DOM就会如下图所示：
![HTML-DOM.png](https://ooo.0o0.ooo/2016/01/31/56adc6e280aa6.png)
在最上面一层是`html`节点，它有两个子节点：`head`和`body`，并且他们中只有`head`有一个子标签`title`。

HTML标签是DOM树中的元素节点，众多的文本组成文本节点。他们都是节点，只是形式不同而已。

* DOM的概念就是每一个节点都是一个目标。我们可以获取一个节点从而改变其性质，比如这样：
```
// change background of the <BODY> element, make all red
document.body.style.backgroundColor = 'red';
```
如果你运行了上述例子，下面的代码可以将样式重置为默认的：
```
// revert background of <BODY> to default, put it back
document.body.style.backgroundColor = '';
```
* 同样，我们也可以改变节点的文本，搜索DOM树中的特定节点，创造新的元素并且将他们插入到实时的文档中。

但是首先外面需要知道DOM长什么样以及它包含什么。

### 另一个文档
---
让我们来看一个更加复杂的DOM文档。
```
<!DOCTYPE HTML>
<html>
    <head>
        <title>The document</title>
    </head>
    <body>
        <div>Data</div>
        <ul>
            <li>Warning</li>
            <li></li>
        </ul>
        <div>Top Secret!</div>
    </body>
</html>
``` 
如果我们按照DOM树来呈现的话是下面这样的。

![HTML-DOM-2.png](https://ooo.0o0.ooo/2016/01/31/56adce81221e7.png)

## 运用开发者工具运行DOM
运用浏览器开发者工具可以很容易的运行DOM。

比如：
1、打开simpledom2.html
2、打开Firbug，Chrome或者IE内建的开发者工具或者其他开发者工具。
3、运行Firebug和IE开发者工具中的HTML标签页或者Safari/Chrome中的元素标签页或者其他。
4、大功告成。

下面是对于上述文档的Firebug截图。基本上它的布局设计跟HTML是一样的，加上节点处的减号。

![Firebug截图.png](https://ooo.0o0.ooo/2016/01/31/56add5e6e042b.png)

**呈现在开发者工具中的DOM并不是完整的**。 有一些存在于DOM中的元素并没有在开发者工具中展现出来。

## 空格节点
现在让我们将画面拉入到现实，隆重介绍空格文档元素。HTML中的空格标志被识别为文本成为文本节点。这些空格节点并不在开发者工具中显示，但是他们确实存在。

下图显示的就是包含空格的文本节点。

![whitespace.png](https://ooo.0o0.ooo/2016/01/31/56add8152001c.png)

顺带一提，我们可以注意到最后的`li`并没有在其中包含空格文本标签。这是因为它的里面压根就没有文本。

空白节点是在节点之间的空间被创造出来的。所以如果我们消除标签之间的空间的话这些空白节点就会随之消除。

下面的例子就没有任何空白节点。

```
<!DOCTYPE HTML><html><head><title>Title</title></head><body></body></html>
```

IE<9

低于IE9的版本不同于其他浏览器是因为他们并不从空白节点中产生标签。

所以这也就是为什么老版本的IE中DOM树与其他浏览器中是不同的。

## 其他节点样式

### 文档类型
---
有没有从上述例子中注意到`DOCTYPE`？图片中并没有显示出来，但是DOCTYPE也同样是DOM节点，位于`HTML`的最顶端。

文档类型属于HTML说明的一部分，并且它介绍了关于网页浏览器中的网页使用哪一种标记语言。

### 评论
---
同样，HTML评论也是DOM节点：
```
<html>
...
<!-- comment -->
...
</html>
```
上面的评论同样被存储在DOM树中，伴随着一起存储的还有*** 评论节点 ***类型和文本内容。我们将会看到在遍历节点时很重要。

## 总结
现在你应该明白了DOM的结构，长什么样子以及它包含什么节点。
下一章将描述如何运用JavaScript遍历它。