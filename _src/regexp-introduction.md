> 译者: 宇卿

正则表达式是一种强大的字符串查找和替换的方式。在 javascript 中，它被集成在 String 的三个方法：`search`，`match`，`replace` 中。

一句标准的正则表达式由 *pattern（模式）* 和 *flags（标志位）* 构成。

一个基本的正则表达式查找等同于子串查找。正则表达式对象由字符串及包裹字符串的反斜杠 `/` 构成。

```
regexp = /att/

str = "Show me the pattern!"

alert( str.search(regexp) ) // 13
```

在上述例子中，`str.search` 方法返回子串在串 `Show me the pattern!` 中的位置


> ### 规定颜色
>为描述方便，定义如下三种颜色
> - 模式 用红色表示
> - 主串 用蓝色表示
> - 结果 用绿色表示

在实际场景中，正则表达式也许会更加复杂。下面的这个正则表达式匹配邮件地址：

```
regexp = /[a-z0-9!$%&'*+\/=?^_`{|}~-]+(?:\.[a-z0-9!$%&'*+\/=?^_`{|}~-]+)*@(?:[a-z0-9](?:[a-z0-9-]*[a-z0-9])?\.)+(?:[A-Z]{2}|com|org|net|edu|gov|mil|biz|info|mobi|name|aero|asia|jobs|museum)\b/
str = "Let's find an email@gmail.com here"
alert( str.match(regexp) )   // email@gmail.com
```
`str.match` 方法会呈现匹配的邮件地址。

学完这章你就能完全理解上面这串复杂的正则表达式了，当然你也能够写出更加复杂的正则。

在 javascript 控制台中，你可以直接调用 `str.match` 来测试正则表达式：

```
alert( "lala".match(/la/) )
```
为了使例子更加精简一些，我们使用 `showMatch` 函数直接打印出匹配的结果：

```
showMatch("show me the pattern!", /att/);

function showMatch(str, reg) {
  var res = [], matches;
  while(true) {
    matches = reg.exec(str);
    if(null === matches) break;
    res.push(matches[0])
    if (!reg.global) break
  }
  alert(res);
}
```

下一步，我们开始学习正则表达式的语法。