> 译者: 周鹭江

`[备注1]`：关于`代码编辑器`部分，根据最新的[内容](https://github.com/iliakan/javascript-tutorial/blob/en/1-js/1-getting-started/3-editor/article.md)翻译。

`[备注2]`：需翻墙。

`[备注3]`：页面无效。

`[备注4]`：[wiki中文](https://zh.wikipedia.org/wiki/%E5%8F%98%E9%87%8F_(%E7%A8%8B%E5%BA%8F%E8%AE%BE%E8%AE%A1))，需翻墙。
* * *
## 设置你的环境
1. 代码编辑器
2. 浏览器
3. 概要

在开始JavaScript开发之前，需要一个良好的工作环境。
这部分包含设置你需要的程序介绍。

### 代码编辑器 `[备注1]`

首先，我们需要一个代码编辑器。
周围有很多种选择，但是你选择的那个应该支持：
1.	语法高亮
2.	自动完成
3.	可折叠：折叠/打开代码块
4.	有更多功能当然更好
### IDE
“IDE”的术语（Integrated Development Environment，集成开发环境）代表了一种可以集成到外部系统，比如追踪bug、版本控制等的高级的编辑器。
一个IDE运行在一个“完整的项目”中：加载它，文件之间的定位、提供基于整个项目自动完成、做其他“项目级别”的东西。
如果你没有考虑过选择一个IDE，请看下面：
* IntelliJ 编辑器：[WebStorm](http://www.jetbrains.com/webstorm/)适合前端开发和[PHPStorm (PHP)](http://www.jetbrains.com/phpstorm/)、[IDEA (Java)](http://www.jetbrains.com/idea/), [RubyMine (Ruby)](http://www.jetbrains.com/ruby/) 和你需要其他额外的语言。
* Visual Studio还不错，如果你是一位.NET 开发者。
* 基于Eclipse的产品，像[Aptana](http://www.aptana.com/) 和 Zend Studio。
* [Komodo IDE](http://www.activestate.com/komodo-ide) 并且它是轻量级的免费版本[Komodo Edit](http://www.activestate.com/komodo-edit).
* [Netbeans](http://netbeans.org/)

除了Visual Studio外，所有上面的都跨平台。
大多数IDE都是付费的，但有一个试用期，其费用相对于一个合格开发人员的工资来说通常是可以忽略不计，所以只需要选择最适合你的一款。
### 轻量级编辑器
一个轻量级编辑器可能没有一个IDE那么功能强大，但是它够快够简单。
他们主要用于即时打开和编辑文件。
一个“轻量级编辑器”和一个”IDE”的主要区别在于：后者工作在项目级别，意味着它启动时需要加载大量数据，并且前者只是打开文件，相对后者要快得多。
实际使用中“轻量级”编辑器可能有很多插件，包括目录级别的语法分析和自动完成，所以在轻量级编辑器和IDE中没有严格的界限，没有意义争论哪个更好。
下面的选项值得你注意：
* Sublime Text （跨平台，共享软件）
* Atom （跨平台，免费）
* Notepad++ （Windows，免费）
* Vim, Emacs 都很棒，如果你知道如何使用它们。
### 我喜欢的
我相信每个开发者应该同时有开发项目级的IDE编辑器，和一款用于快速方便编辑文件的轻量级编辑器。
我正在用：
* [WebStorm](http://www.jetbrains.com/webstorm/)`[备注2]`用于JS，同时如果项目中需要更多语言，我就切换到其他Jetbrains 编辑器，比如： [PHPStorm](http://www.jetbrains.com/phpstorm/) (PHP)、[IDEA](http://www.jetbrains.com/idea/) (Java)、[RubyMine](http://www.jetbrains.com/ruby/) (Ruby)。还有其他语言的编辑器，但是我还没用过。
* 轻量级编辑器 -- Sublime Text.
如果你不知道该如何选择，你可以考虑上面两个。
### 我们别争论
上面列出来的编辑器都是我或者我的朋友—不错的开发者使用了很长一段时间还很满意的。
在这个大世界中，还有很多其他优秀的编辑器，请选择你最喜欢的。
选择一款编辑器，就像选择其他的工具，是因人而异的，取决于你的项目、习惯、个人偏好。
***
### 浏览器
你应该安装支持的所有浏览器。目前的趋势是，浏览器趋向支持标准和实现的汇集，尽管它们也有很多不同之处。
你至少需要：火狐浏览器、谷歌浏览器 和IE浏览器。
在这里下载：
* http://getfirefox.com （火狐浏览器）
* http://www.google.com/chrome （谷歌浏览器）`[备注2]`

通常你还需要Safari、Opera和其他版本的IE浏览器
如果你使用Linux或者MacOS，那么需要为Windows IE浏览器安装一台虚拟机。
### 小结
大多数程序员使用火狐浏览器第一次开发脚本语言。如果所有的运行正确，那么再到谷歌和IE浏览器测试，然后再去别的主流浏览器测试。
所以使用虚拟机安装不同版本的IE浏览器也合情合理了。