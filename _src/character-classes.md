> 译者: 宇卿

假设我们要从一个字符串中查找一个十进制位。如果没指定 0~9 中哪一位，对于串 `Only 1`，会找到 `1`，对于串 `Give me a 5` 会找到 `5`。

通过循环，子串匹配能够找到 0~9 所有的十进制位。但对于这种场景，正则匹配会处理得更加优雅。

**正则表达式能表示一类字符，而不仅限于某一个特定字符**。

例如，正则表达式中 `\d` 表示任意十进制位。下面的例子匹配了一个十进制位：

```
showMatch( "I'm 5 years old", /\d/ ) //5
```

**最常用字符类有：**

`\d`: 任意十进制位，即 0 ~ 9 中任意一位
`\s`: 任意空白字符，例如：空格、制表符、换行符等
`\w`: 任意拉丁字母，十进制位或者 `_`

**正则表达式可以是普通字符和字符类的组合**

```
showMatch( "I'm the 1st one", /\dst/ )   // matches '1st'
```
下面例子把几个字符类组合成一个正则表达式：

```
showMatch( "I'm 1 year old", /\d\s\w\w\w\w/ )   // 1 year
```

有字符类，那必然会有 「反字符类」：

`\D`: 任意非十进制位，`\d` 取反 
`\S`: 任意非空白字符，`s` 取反
`\W`: 任意非拉丁字符、十进制位、下划线，`\w` 取反

正则表达式也可以包含转义字符（不可见的字符），例如：`\n`, `\t` 等。当然它们仅仅是字符，而并非字符类。

> **空格很重要**
> 通常，我们不会太关注空格。比如：`1-5` 和 `1 - 5`，看起来并没有什么不同。
> 但在正则表达式中，空格字符和其他字符的地位是等同的！
> 下面的正则表达式正是由于没考虑空格的情况而匹配失败：
> ```
> showMatch( "1 - 5", /\d-\d/ )  // no matches!
> ```
>要解决这个问题，我们应该把在正则表达式中加入空格字符，或者最好加入通用空格字符，也就是『字符类』。
> ```
>  showMatch( "1 - 5", /\d - \d/ )   // works
>  showMatch( "1 - 5", /\d\s-\s\d/ ) // also works
>showMatch( "1-5", /\d - \d/ ) // fails! (no spaces in string)
> ```

在正则表达式中，`.` 代表除了换行外任意字符：

```
showMatch( "A char", /ch.r/ ) // "char"
showMatch( "A ch-r", /ch.r/ ) // "ch-r"
showMatch( "A ch r", /ch.r/ ) // "ch r", the space is also a char
```
但要注意，`.` 代表任意一个字符，不能匹配空字符：

```
showMatch( "A chr", /ch.r/ ) // not found
```