> 译者: 付太

[Ilya Kantor](http://javascript.info/users/ilya-kantor) 
1. [Table](http://javascript.info/tutorial/more-references-forms-and-tables#table) 
2. [Form](http://javascript.info/tutorial/more-references-forms-and-tables#form)
3. [Form elements](http://javascript.info/tutorial/more-references-forms-and-tables#form-elements)
1.[Select](http://javascript.info/tutorial/more-references-forms-and-tables#select)
3. [Summary](http://javascript.info/tutorial/more-references-forms-and-tables#summary)

DOM form对象，table对象，select对象，和其它元素包括附加的链接以简化导航。
它们可以保持代码更加简洁整齐。     




## Table


table参考它的rows（行），行参考cells（单元格）：


```javascript
<table>
  <tr> <td>one</td>   <td>two</td>  </tr>
  <tr> <td>three</td> <td>four</td> </tr>
</table>

<script>
var table = document.body.children[0]

alert(table.rows[0].cells[0].innerHTML) // "one"
</script>
```
当一个表格可以包括另一个表格时，这些*行*/*单元格*就会变得更加的方便。这种情况下，outerTable.getElementsByTagName('TD') 会从里面的表格里返回我们也许不需要的单元格。属性table.rows/row.cells仅仅涉及精确的表格。

参考规范： [HTMLTableElement](https://www.w3.org/TR/DOM-Level-2-HTML/html.html#ID-64060425) and [HTMLTableRowElement](https://www.w3.org/TR/DOM-Level-2-HTML/html.html#ID-6986576).

------
## Form
![0_1454313281021_QQ截图20160201155323.png](/uploads/files/1454313280966-qq截图20160201155323.png)` form的使用是通过它的文件名称或索引形式[name/index]。`
![0_1454313540127_QQ截图20160201155323.png](/uploads/files/1454313540160-qq截图20160201155323.png) `form.elements[name/index]属性请参考它的元素。`

![0_1454313594059_form.png](/uploads/files/1454313594090-form.png) 


例如：
```javascript
<body>
<form name="my">
  <input name="one" value="1">
  <input name="two" value="2">
</form>

<script>
var form = document.forms.my // also document.forms[0]

var elem = form.elements.one  // also form.elements[0]

alert(elem.value) // "one"
</script>
</body>
```
具有相同名称的多个元素也可访问。
在这种情况下，相应的元素[name]返回集合：

```javascript
<body>
<form>
  <input type="radio" name="age" value="10">
  <input type="radio" name="age" value="20">
</form>

<script>
var form = document.forms[0]

var elems = form.elements.age

alert(elems[0].value) // 10
</script>
</body>
```
参考不依赖于嵌套。一个元素可以深嵌在表格内，但仍可以直接使用form.elements
参考规范：[HTMLFormElement](https://www.w3.org/TR/DOM-Level-2-HTML/html.html#ID-40002357).

![0_1454313890051_QQ截图20160201160420.png](/uploads/files/1454313890096-qq截图20160201160420.png) `form.name`也能用，但是有bugs。
Form元素可以以[index/name]的形式访问。
所有浏览器都能使用。但是在火狐浏览器中，当你删除该元素后，仍然可以通过[name]访问到。
```javascript
<form name="f"> <input name="text"> </form>

<script>
var form = document.forms.f
var input = form.text // input

form.removeChild(input) // remove input

alert(form.elements.text) // => undefined (correct)
alert(form.text) // => element, still accessible in Firefox!
</script>
```
在上面的例子中，一个元素被删除，但形式[名称]仍然引用它。所以，一般来说，使用更长的语法form.elements[name]是更可靠的。

------

## Form元素
Form元素参考它们的form，例如element.form。
```javascript
<body>
<form>
  <input type="text" name="surname">
</form>

<script>
var form = document.forms[0]

var elem = form.elements.surname

alert(elem.form == form) // true
</script>
</body>
```
参见[HTMLInputElement](https://www.w3.org/TR/DOM-Level-2-HTML/html.html#ID-6043025) 和其它元素类型的规范。

------
## Select

Select元素提供访问它的选项。
```javascript
<form name="form">
  <select name="genre">
    <option name="blues" value="blues">Soft blues</option>
    <option name="rock" value="bock">Hard rock</option>
  </select>
</form>

<script>
var form = document.body.children[0]

alert(form.elements['genre'].options[0].value) // blues
</script>
```
在这个元素中，我们可以同时使用name: options['blues'] and index: option[0].
Select元素还提供SelectedIndex属性选定选择选项的索引。如果选择不多还是可以用的。

下面的例子演示如何选择一个值：
```javascript
<form name="form">
  <select name="genre">
    <option name="blues" value="blues">Soft blues</option>
    <option name="rock" value="rock">Hard rock</option>
  </select>
</form>

<script>
var form = document.forms.form

var select = form.elements.genre
var value = select.options[select.selectedIndex].value

alert(value) // blues
</script>
```
参考规范：[ HTMLSelectElement](https://www.w3.org/TR/DOM-Level-2-HTML/html.html#ID-94282980).

------

## 总结  

* TABLE  
 直接进入行和单元格。

* FORM       
Forms是 document.forms[name/index]. 元素是 form.elements[name/index].

* SELECT
直接通过select.options[name/index]进行选择。选定选项的索引：select.selectedindex。


相比一般的DOM机械搜索，这些方法都非常方便的。