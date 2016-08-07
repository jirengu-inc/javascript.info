> 译者：洪凯敏 

  * Static
  * Relative
  * Fixed
  * Absolute
  * 存在父级定位元素下的Absolute效果
  * 更多

CSS定位在创建生动的页面的时候非常重要。
在这个章节，我们将来学习并使用它。

### Static

如果你没有设置其他的定位属性，那Static就是默认的定位属性。
我们用下面的例子来学习下Static定位属性。

源码图：
```
<style> /* 基础样式 */
.widget { 
  background-color: #ffe4c4;   
  width: 500px;
}

#today-header {
  background-color: #5f9ea0;
  margin: 0;
}
</style>

<h1>Static positioning</h1>

<div class="widget">
    The widget header.

    <h1 id="today-header">Information</h1>

    <p>A new byte-code standard is defined for JavaScript... <br/>
         Ok, just dreaming.</p>
</div>

The footer.
```

结果图：
![Static](http://upload-images.jianshu.io/upload_images/1667593-79654530a614a961.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**一个节点如果设置了``position:static``属性，则意味着他是一个没有设置定位的节点，如果使用的是其他定位属性则意味着这是一个设置了定位的节点。**

### Relative

一个节点能够相对于自己原来的位置进行定位。
如果你想要使用该定位属性，你需要设置节点的CSS中的定位属性(position:relative)和相对应的定位坐标，一般都选择以下定位坐标中的一个或者两个。
* ``left/right``
* `` top/bottom``

我们经常选择其中的一个做为水平方向上的偏移量，另外一个做为垂直方向上的偏移量。
然后节点就会相对自己原来的位置根据给予的偏移量进行水平和垂直方向上的偏移。
我们用下面的例子来学习下Static定位属性。

源码：
```
<style> /* basic style */
.widget { 
  background-color: #ffe4c4;   
  width: 500px;
}

#today-header {
  background-color: #5f9ea0;
  margin: 0;
}
</style>

<style>
.widget{//注意这边属性
    position: relative;
    left: 30px; 
    top: -30px;
  }
</style>


<h1>Relative positioning</h1> 

<div class="widget">
    The widget header.

    
    <h1 id="today-header">Information</h1>

    <p>A new byte-code standard is defined for JavaScript... <br/>
         Ok, just dreaming.</p>
</div>

The footer.
```

结果图：

![Relative](http://upload-images.jianshu.io/upload_images/1667593-6be7e9307558b57b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

正如你在例子上看到的，``.widget``节点偏移了：
* ``left：30px`` 节点相对原来的位置向右偏移了30px.
* ``top: -30px``: 如果top的值是``30px``，那节点就会相对原来的位置向下偏移30px，但是在这边由于是负值，导致节点会相对原来的位置向上偏移30px.

但是当``position``为``static``的时候，设置left、right、top、bottom的值无效，既不会产生偏移。

**``position: relative``这个属性会移动节点，但是节点原来的位置还是会被空白占据，并不会消失。
在上面的例子里，我们看到The footer并没有向上移动并占据设置了widget节点原来的位置。**

### Fixed

该定位属性不被IE8以下的版本(不包括IE8)支持。
他做了以下两件事:
  1. 定位节点从它原来的位置上脱离，其他临近元素将会占据定位节点原来的位置。
  2. 定位节点会相对于window进行偏移，偏移的量由(top/bottom/left/right)决定，由于是相对window进行偏移，不是相document进行偏移，所以当document滚动的时候，该节点并不会随着滚动。

在下面的例子里，页面是可以滚动的, 但是 ``#go-top``这个a节点一直固定不动(在IE8以下的版本并不是这样的表现)。

源码图：
```
<style>
#go-top {//注意这个元素的设置
    position: fixed;
    right: 10px;
    top: 10px;
    font-size: 125%;
  }
</style>

<a href="#top" id="go-top">Scroll, I don't move!</a>


<h1 id="top">Fixed positioning</h1>
    The widget header.

<div class="widget">
    
    <h1 id="today-header">Information</h1>

    <p>A new byte-code standard is defined for JavaScript... <br/>
         Ok, just dreaming.</p>

   <p>SquirrelFish is a register-based, direct-threaded, high-level bytecode engine, with a sliding register window calling convention. It lazily generates bytecodes from a syntax tree, using a simple one-pass compiler with built-in copy propagation.</p>

</div>

The footer.
```
结果图：

![fixed](http://upload-images.jianshu.io/upload_images/1667593-23da008cad10bacc.gif?imageMogr2/auto-orient/strip)

### Absolute

**绝对定位属性是所有定位属性当中最重要的**

他做了以下两件事:
  1. 定位节点从它原来的位置上脱离，其他临近元素将会占据定位节点原来的位置。
  2. 定位节点会相对于最近的已经存在定位属性的父节点产生偏移，如果不存在这样的父节点，则会相对于document产生偏移。

最近的存在定位属性的父节点是指存在定位属性且值不为``static``的父节点(包括父级的父级，父级的父级的父级一直到根节点)。
在下面的例子里，``.widget``被设置了``position:absolute``，但是由于他的父节点不存在定位属性，所以他就相对于document产生了偏移。

源码：
```
<style>
  @import url(/files/tutorial/browser/dom/poscolor.css)
</style>

<style>
.widget{
    border: 1px solid black;
    position: absolute;
    right: 0;
    bottom: 0;
  }
</style>


<h1>Absolute positioning</h1>

<div class="widget">
    The widget header.

    <h1 id="today-header">Information</h1>

    <p>A new byte-code standard is defined for JavaScript... <br/>
         Ok, just dreaming.</p>
</div>

The footer.
```

效果图：

![absolute](http://upload-images.jianshu.io/upload_images/1667593-b22e3ba7e5f40418.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
这边可以看到由于``.widget``节点脱离了文档流，``The footer ``占据了``.widget``节点原来的位置。

###  存在父级定位元素下的Absolute效果

如果我们需要``.widget``元素相对于一个header节点进行right和top方向上的定位。那么我们就应该像如下一样设置：

源码：
```
<style>
  @import url(/files/tutorial/browser/dom/poscolor.css)
</style>

<style>
.widget{//注意这边
    position: relative;
  }
  #today-header {//注意这边
    position: absolute;
    right: 0px;
    top: 0px;
  }
</style>


# Absolute positioning   

<div class="widget">
    The widget header.

    <h1 id="today-header">Information</h1>

    <p>A new byte-code standard is defined for JavaScript... <br/>
         Ok, just dreaming.</p>
</div>

The footer.
```
结果图：

![absolute](http://upload-images.jianshu.io/upload_images/1667593-b9e36803dc5c8d5d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在上面的这个例子里，由于``.widget``节点设置了除了``position:static``以外的定位属性，且该节点是``today-header``的父节点，所以导致设置了"position:absolute"的``#today-header``节点的相对``.widget``节点产生了偏移。

给一个节点设置``position:relative``，但是不做偏移，从而使得，该节点的子节点中设置``position:absolute``的节点相对父元素偏移，是一个经常使用的方法。

`` #today-header``节点由于已经脱离了文档流，导致其周围的节点开始占据他原来的位置。

有些时候，这样的情况是没有问题的，但是当我们需要保留脱离文档流的节点的原来的位置的时候，我们就需要给周围的元素设置例如``padding``等额外的样式属性，通过"障眼法"，从而来保留脱离文档流的节点的原来的位置。

### 更多

在这个说明文档中，对CSS的定位属性做了生动的描述。
 [Visual Formatting Model, 9.3 and below](http://www.w3.org/TR/CSS2/visuren.html#positioning-scheme)

当然，这边也有一个覆盖了主要定位属性和浮动的教程。
 [CSS Positioning in 10 steps](http://www.barelyfitz.com/screencast/html-training/css/positioning/)