# 概述：JavaScript, Flash, Java, Silverlight 和 ActiveX#
> 译者: 董晓明

*作者：Ilya Kantor*
1.	JavaScript是什么？
2.	JavaScript可以做什么？
3.	JavaScript无法做什么？
4.	JavaScript酷在哪里？
5.	JavaScript的趋势 HTML5.
6.	可以替代的技术

* 1.	Java
* 2.	Adobe Flash
* 3.	ActiveX, 浏览器插件/扩展
* 4.	其他技术: Silverlight, XUL, VBscript
7.	总结
    让我们看看javascript有何特别之处， 为什么选择javascript以及除它以为还有什么有用的技术。
JavaScript是什么？
    JavaScript是一种脚本语言，为创建动态页面而生。 它使得网页功能变得更加强大而不再只是相互关联的html页面。.
    JavaScript跟java没有任何关系。除了名称相似以外它们是完全不同的两种语言。JavaScript 采用的是 ECMAScript语言规范。
    JavaScript程序被称为脚本。他不用编译，你只需将写好的脚本放入HTML页面，就可以执行了。
有人说JavaScript 像Python,也有人说它跟Ruby或者Self很接近。 而事实上javascript是一个独立的语言，一个非常优雅而特别的语言。
JavaScript可以做什么？
•	修改HTM页面，添加文本，新增或删除标签，更改样式等。
•	执行事件响应：鼠标点击和移动，键盘输入等。
•	向服务器发送请求以及在不刷新页面的情况下获取数据。这种技术被称为： "AJAX".
•	获取和设置cookies,请求数据，输出信息…… 
•	……还有很多很多。
    现在的JavaScript是一个通用语言。不仅适用于浏览器。 像控制台程序和服务端语言Node.JS 也在使用JavaScript. 本书中只讨论浏览器中使用的JavaScript.
JavaScript无法做什么？
JavaScript是一个通用语言，速度快且功能强大。
但是在浏览器中执行脚本意味着会有一些安全限制。
那是因为你肯定不希望一个网页脚本可以有读写硬盘数据，安装软件等权限。脚本必须有严格的安全限制才能避免损害你的系统，这样你才可以放心的打开网页。也有非标准机制授权的JavaScript,但并未得到广泛支持。
大部分JavaScript功能被限制在了浏览器内。
![0_1454307049209_upload-fa7cf9fb-ed1c-432b-ae94-2a87d31be113](/uploads/files/1454307196697-upload-fa7cf9fb-ed1c-432b-ae94-2a87d31be113) 
 
•	JavaScript无法读写硬盘数据，复制文件及调用其它程序。它没有直接进入操作系统的权限。新版本浏览器也许提供了此类功能，但也是在一种被严格限制着及安全的方式下。
•	JavaScript无法对其他域产生作用。也有例外， 即当两个窗口来自相同的域名。
•	一个页面可以不受限制地在当前域名下发送请求。也可以向其他域名发送请求，但需采用安全方式。
JavaScript酷在哪里？
有至少三个杀手级优点。
1.	被HTML/CSS全面支持
2.	简单的事可被轻松完成
3.	被所有浏览器支持且默认可用
这些优势是其他语言所没有的。
并且，记住JavaScript很灵活，正在持续的发展。很多新特性出现，新的 ECMAScript标准带来了很好的功能。新的JavaScript引擎也更完善更迅速。
JavaScript趋势。 HTML5.
当您想去学习一门技术，通常需要投入时间。最好的办法是先去大概的了解下趋势。
除了最新的ECMAScript标准出现，增强了其语言本身，浏览器的设计者们也采用了HTML 5的新特性。 这是一个相关联的标准，或者说更准确的标准，包含的许多人们期盼已久的功能。
举些例子：
•	读写访客的硬盘文件（在适当的安全条件下保障其安全）。
•	在浏览器中嵌入了一个数据库，使得在客户端可以储存数据。
•	多线程 (可以使用多核CPU)。
•	视频播放。
•	 2d和3d绘图，采用硬件加速，类似新版的游戏。
大多数HTML5主题仍处于“草案”阶段，但是浏览器已经在尝试支持他们了。.
“HTML5”这个标题也有一些误导人。正如你所说的，新的标准不只是关于HTML, 也包括浏览器功能的交互性和完善。
趋势是：JavaScript正在不断增强它的功能。它正变得越来越强大，试图达到桌面级应用。
新版浏览器提升了他们的引擎使JavaScript 获得更高的执行速度。他们也在修补漏洞和试着去遵循这些标准。
趋势是: JavaScript正在变得更快更稳定。
同时也很重要的一点是最新的HTML5 and ECMAScript 5/6标准也极大的兼容了老版本。 这意味着他不会破坏现有的程序。
不过，老实说，HTML5也有一个小问题， 名为“浏览器走的太快”。有些浏览器会支持一些尚未在标准中（草案）描述清楚的功能，只是因为它酷的让人等不及了。
然后，随着标准的发展和更新，浏览器不得不重装或修这个功能。这会破坏以来早期版本的程序。所以我们在使用这些草案阶段解决方案时需要三思。这主要指的是某个先进的东西。
趋势是：一切都将是兼容的。
当然直到我们不使用浏览器特定特性和标准中早期支持的部分草案。
替代技术
JavaScript的能力在特定场合是受到限制的。因此使用了替代技术。
需指出的是：他们与JavaScript都配合的很好。有时，一个任务JavaScript无法解决， 但可以采用JavaScript + Java 或JavaScript + Flash或者drop in ActiveX.
Java
正如您所知， JavaScript不是Java.事实上，除了名称外他们没有什么共通的地方。Java是一个可以写程序并将其嵌入网页的不同的语言。
一个浏览器的Java程序类似一个可执行文件。 一个用java编写的程序，编译后将一个链接放入HTML文件。 当浏览器打开一个页面，找到并引用那个程序，下载并执行它 (如果java允许的话)。
Java程序和JavaScript的一个最重要的区别是他们的功能。
•	Java可以做任何事，就像安装了一个可执行程序.。出于安全，非安全的动作需要访客的确认。
•	Java开发更容易： 开发工具很酷。
•	Java需要花更多的时间安装并且启动时很笨重。
•	Java 需要事先安装和启用。
•	Java 没有集成进HTML页面，它在浏览器外的单独容器中运行。
Adobe Flash
Adobe Flash最初以跨浏览器平台和语言的多媒体出现，为了使页面播放动画，音频和视频。. 然而Flash也有其他有趣的特性。.
一个flash影片就是一个编译过的程序，通常捆绑了图片和其他资源。
•	伟大的网络产物 (sockets, UDP for P2P)
•	支持复杂的多媒体：图片，音频，视频比HTML5更好。摄像头和麦克风也支持。
•	便捷的集成开发环境，没有浏览器不兼容。
•	Flash必须安装和启用
•	Flash并没有集成进HTML页面，它运行在浏览器之外的独立容器中。
•	Flash的安全限制几乎与JavaScript一样严格。
到目前为止，在很多地方的垄断遇到一个很大的挑战。比如， HTML5为浏览器提供了播放视频，绘图等方式。浏览器可以用HTML5实现的东西不需要浏览器再做同样的事情。 并且很多浏览器真的在推进HTML5特性的应用。
但是Java和 Flash函数都可以与JavaScript互相调用， 所以大部分网站都使用Javascript,当然Java/Flash会出现在 JavaScript应付不了的地方。
ActiveX, browser 插件/扩展
ActiveX 很伟大，但支持IE.它可以将用c写的程序集成进网页在访客允许的情况下。
•	集成HTML/CSS
•	C语言编写，因此非常快和特别。
•	如果访客允许可以做任何事。
•	只支持IE. Chrome部门支持了必要的部分。
•	ActiveX 开发很难。
Windows程序提供可供ActiveX使用的接口。所以页面可以调用Microsoft Word, 或将文件导入Excel, 等等。
其他浏览器允许使用NPAPI编写插件和扩展.
个人认为，我不是一个微软粉。 但我见过了不起的ActiveX应用， 而且我也理解为什么人们喜欢用他们并且将其绑入Windows IE。
其他技术: Silverlight, XUL, VBscript
这些技术较少广泛应用
•	XUL是一种接口语言，当你的代码支持Mozilla或者为Firefox写扩展时很有用。并且也可以写桌面应用。
•	Silverlight 是一个微软基于.NET的Adobe Flash的竞争产品。它适用于Windows，其跨平台支持随着时间再逐步改善。主要用于基于Windows的应用和内联网。
•	VBscript 是一个已经过时的，基于Visual Basic的尝试做Javascript同样事情的微软产品。它已经不维护了，VBScripts比JavaScript少很多功能因此在新的web程序中已经不使用了。
总结
JavaScript是独特的因为它被广泛使用并且最棒的是它集成HTML/CSS.
JavaScript有更光明的广泛兼容的未来。
但是一个好的JavaScript程序员也需要懂得其他语言。例如， Flash, Java有其独特的特性。他们与JavaScript函数可以互相调用。
所以很有很多任务和可以用 JavaScript + Flash, JavaScript + Java结合的方式解决。
比如：选择一次上传多个文件(Flash), 使用摄像头和麦克风(Flash), 制作复杂的多媒体和图形,包括计算(Flash, Java) 等等。你以后会遇到的。