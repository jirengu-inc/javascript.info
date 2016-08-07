> 译者: 陆恩泽

**DOM节点基本属性**
文档对象模型(DOM)节点是一个对象，其属性包含了节点的自身信息以及内容信息，一些属性为只读。
1**结构与内容属性**
* 1.1节点类型
所有节点都被分类，总共有十二种类型的节点，详细描述见[DOM Level1](https://www.w3.org/TR/REC-DOM-Level-1/level-one-core.html#ID-1950641247)。
```
interface Node {
  // NodeType
  const unsigned short      ELEMENT_NODE       = 1;
  const unsigned short      ATTRIBUTE_NODE     = 2;
  const unsigned short      TEXT_NODE          = 3;
  const unsigned short      CDATA_SECTION_NODE = 4;
  const unsigned short      ENTITY_REFERENCE_NODE = 5;
  const unsigned short      ENTITY_NODE        = 6;
  const unsigned short      PROCESSING_INSTRUCTION_NODE = 7;
  const unsigned short      COMMENT_NODE       = 8;
  const unsigned short      DOCUMENT_NODE      = 9;
  const unsigned short      DOCUMENT_TYPE_NODE = 10;
  const unsigned short      DOCUMENT_FRAGMENT_NODE = 11;
  const unsigned short      NOTATION_NODE      = 12;
   
  ...
}

```
最重要的是1号节点 ELEMENT_NODE 和3号节点TEXT_NODE，其他类型的节点很少使用。
例如，为了列出所有非元素节点，我们可以遍历子节点，用childNodes[i].nodeType != 1 来筛选非元素节点。
可以通过下面的例子来验证：
```
<body>
  <div>Allowed readers:</div>
  <ul>
    <li>John</li>
    <li>Bob</li>
  </ul>
 
  <!-- a comment node -->

  <script>   
     var childNodes = document.body.childNodes
     for(var i=0; i<childNodes.length; i++) {
if (childNodes[i].nodeType != 1) continue
       alert(childNodes[i])
     }
  </script>
</body>
```
运行下面代码页面会发出什么警告类容？
```
<!DOCTYPE HTML>
<html>  
<body>  
   <script>
      alert(document.body.lastChild.nodeType)
   </script>
</body>  
</html>
```
一个小陷阱是在脚本执行事，最后一个子节点就是脚本本身。所以结果是1，即是为元素节点。
* 1.2 节点名，标签名
节点名和标签名包含了元素节点名。
document.body：
```
alert( document.body.nodeName )   // BODY
```

* 在HTML中任何节点名是大写的，无论你在文件的哪一种情况下使用。
**但是也有特殊情况，节点名是小写字母，那么在什么时候会遇到了？**
这种情形少且特别。如果你对此好奇那么阅读这一块内容。
正如你可能知道的，浏览器有俩种解析模式：HTML模式和XML模式。通常情况下，主要使用前一种模式，但是XML文件通过HTMLhttp请求被接收（一种AJAX技术），XML节点是有效的。
在火狐浏览器中，当XHTML文件具有xmlish内容特性时，XML节点的节点名也被使用。
在XML模式节点名特有情况下，所以可能出现 “body” 和“bOdY”这种的节点名。
所以，如果出现从发出XMLHttpRequest 的服务器加载XML和传递XML节点到 html文件中，正如上这种情况将会保持。 
元素节点的节点名和标签名是一样的。
但是节点名的属性也存在于非节点元素，如下例所示，在这样的节点中，其具有特殊的值
```
alert(document.nodeName)
```
在大多数节点类型中标签名属性未被定义，IE浏览器的注释节点名为！。
总的来说，标签名不如节点名信息丰富。但这是单符号缺少。因此如果你只使用节点元素，你发现节点元素优于节点名。

*  1.3 innerHTML
innerHTML属性是HTML5标准的一部分，看这个网页[embedded content](https://www.w3.org/TR/html5/embedded-content-0.html)
其可以获得文本形式的节点内容，下面的例子将输出文件中的所有内容。body内容被新的内容替换。
```
<body>
  <p>The paragraph</p>
  <div>And a div</div>
  <script>
    alert( document.body.innerHTML ) // read current contents
    document.body.innerHTML = 'Yaaahooo!' // replace contents
  </script>
</body>

```

*  1.4 innerHTML陷阱
innerHTML并不是看上去这么简单，有许多陷阱在等待新手。有时甚至一个有经验的程序猿也不能幸免。
**waring:**    **在IE浏览器table标签innerHTML属性为只读**
在网络浏览器中，对于 COL, COLGROUP, FRAMESET, HEAD, HTML, STYLE, TABLE, TBODY, TFOOT, THEAD, TITLE, TR，innerHTML属性为只读。
对于IE浏览器，除了TD对于所有table标签innerHTML属性都为只读。
**waring:**  **innerHTML不能被附加**
语法意义上，可以添加内容到innerHTML中，innerHTML += "New text"，看下面：
```
chatDiv.innerHTML += "<div>Hi <img src='smile.gif'/> !</div>"
chatDiv.innerHTML += "How you doing?"
```
但是事实上是进行的操作是：
老的内容被覆盖
新的innerHTML量被解析和插入
内容并没有被添上，而是重新生成了。因此，执行“+=”后所有图片和其他资源将会被重载，这也包括了下面例子中的smile.gif 文件
幸运的是，还有方法来更新内容，这些方法使得innerHTML失效从而不存在这一问题。

* 1.5 节点值
innerHTML只对元素节点有效。
对于其他类型节点，有一个属性叫节点值保存着节点的内容。
下面的例子证明了它是怎么作用于文本节点和注释的：
```
<body>
  The text
  <!-- A comment -->
  <script>
    for(var i=0; i<document.body.childNodes.length; i++) {
      alert(document.body.childNodes[i].nodeValue)
    }
  </script>
</body>

```
在上面的例中，一些alerts是空，这是由于文本节点空白。
注意，script的节点值为空，这是因为script是一个元素节点。元素节点使用innerHTML。
**总结**

* 节点类型
节点类型。对于元素来说最值得注意的类型是1，对于文本节点来说是3。具有只读属性。

* 节点名/标签名
标签名大写，对于非元素节点，节点名有特殊值。具有只读属性。

* innerHTML
元素节点的内容，具有可写属性。

* 节点值
文本节点内容。具有可写属性。
DOM节点也有其他的属性，这取决于其标签。例如一个输入元素有值和可检测属性。A标签有href属性等。