一个设置了overflow:hidden/auto的元素，如果它的子元素尺寸超出它，那么在子元素的右边和下边，
是以父元素的边框内边缘为边界剪裁的。
但是chrome浏览器和其它浏览器则不是，所以不应该给设置了overflow:hidden/auto的元素设置padding-bottom.


ie8以后，支持overflow-x/overflow-y,支持的属性值和 overflow 属性一模一样。 
• visible：默认值。
• hidden：剪裁。
• scroll：滚动条区域一直在。
• auto：不足以滚动时没有滚动条，可以滚动时滚动条出现。
注意：
如果 overflow-x 和 overflow-y 属性中的一个值设置为 visible 而另
外一个设置为 scroll、auto 或 hidden，则 visible 的样式表现会如同 auto。
也就是说，
除非 overflow-x 和 overflow-y 的属性值都是 visible，否则 visible 会当成 auto 来
解析。换句话说，永远不可能实现一个方向溢出剪裁或滚动，另一方向内容溢出显示的效果。
因此，下面 CSS 代码中的 overflow-y:auto 是多余的： 
```
html { 
 overflow-x: hidden; 
 overflow-y: auto; /* 多余 */ 
} 
```
但是，scroll、auto 和 hidden 这 3 个属性值是可以共存的。 

关于浏览器的滚动条，有以下几个小而美的结论。
（1）在 PC 端，无论是什么浏览器，默认滚动条均来自`<html>`，而不是`<body>`标签。
（2）在 PC 端滚动条会占用容器的可用宽度或高度。
要知道自己浏览器的滚动栏宽度是多少其实很简单，代码如下：
```
.box { width: 400px; overflow: scroll; } 
<div class="box"> 
 <div id="in" class="in"></div> 
</div> 
console.log(400 - document.getElementById("in").clientWidth);
``` 
(3)自定义滚动条：
对于 Chrome 浏览器： 
• 整体部分，::-webkit-scrollbar；
• 两端按钮，::-webkit-scrollbar-button；
• 外层轨道，::-webkit-scrollbar-track；
• 内层轨道，::-webkit-scrollbar-track-piece；
• 滚动滑块，::-webkit-scrollbar-thumb；
• 边角，::-webkit-scrollbar-corner。
但是我们平时开发中只用下面 3 个属性：
```
::-webkit-scrollbar { /* 血槽宽度 */ 
 width: 8px; height: 8px; 
} 
::-webkit-scrollbar-thumb { /* 拖动条 */ 
 background-color: rgba(0,0,0,.3); 
 border-radius: 6px; 
} 
::-webkit-scrollbar-track { /* 背景槽 */ 
 background-color: #ddd; 
 border-radius: 6px; 
} 
```


单行文字溢出点点点效果
```
.ell { 
 text-overflow: ellipsis; 
 white-space: nowrap; 
 overflow: hidden; 
} 
```
对-webkit-私有前缀支持良好的浏览器还可以实现多行文字打点效果，但是却无
须依赖 overflow:hidden。比方说，最多显示 2 行内容，再多就打点的核心 CSS 代码如下： 
```
.ell-rows-2 { 
 display: -webkit-box; 
-webkit-box-orient: vertical;
-webkit-line-clamp: 2;
} 
```


使用更精练的代码表示就是：
```
<a href="#1">发展历程></a> 

<a name="1"></a> 
```
就我个人而言，我更喜欢使用下面的做法，也就是利用标签的 id 属性，因为 HTML 会显
得更干净一些，也不存在任何兼容性问题：
```
<a href="#1">发展历程></a> 

<h2 id="1">发展历程</h2> 
```


1．锚点定位行为的触发条件 
下面两种情况可以触发锚点定位行为的发生：
（1）URL 地址中的锚链与锚点元素对应并有交互行为；
（2）可 focus 的锚点元素处于 focus 状态。
比方说，点击一个链接，改变地址栏的锚链值，或者新打开一个链接，后面带有一个锚
链值，当然前提是这个锚链值可以找到页面中对应的元素，并且是非隐藏状态，否则不会有任
何的定位行为发生。如果我们的锚链就是一个很简单的#，则定位行为发生的时候，页面是定位
到顶部的
所以我们一般实现返回顶部效果都是使用这样的 HTML： 
`<a href="#">返回顶部></a> `
然后配合 JavaScript 实现一些动效或者避免点击时候 URL 地址出现#，而很多人实现返回顶部
效果的时候使用的是类似下面的 HTML： 
`<a href="javascript:">返回顶部></a>` 
然后使用 JavaScript 实现定位或者加一些平滑动效之类。显然我是推荐上面那种做法的，因为
锚点定位行为的发生是不需要依赖 JavaScript 的，所以即使页面 JavaScript 代码失效或者加载缓
慢，也不会影响正常的功能体验，也就是用户无论在什么状态下都能准确地返回顶部。



“focus 锚点定位”指的是类似链接或者按钮、输入框等可以被 focus 的元素在被 focus
时发生的页面重定位现象。
举个很简单的例子，在 PC 端，我们使用 Tab 快速定位可 focus 的元素的时候，如果我们
的元素正好在屏幕之外，浏览器就会自动重定位，将这个屏幕之外的元素定位到屏幕之中。再
举一个例子，一个可读写的`<input>`输入框在屏幕之外，则执行类似下面的 JavaScript 代码的
时候：
document.querySelector('input').focus(); 
这个输入框会自动定位在屏幕之中，这些就是“focus 锚点定位”。 
同样，“focus 锚点定位”也不依赖于 JavaScript，是浏览器内置的无障碍访问行为，并且
所有浏览器都是如此。
虽然都是锚点定位，但是这两种定位方法的行为表现还是有差异的，“URL 地址锚链定位”
是让元素定位在浏览器窗体的上边缘，而“focus 锚点定位”是让元素在浏览器窗体范围内显
示即可，不一定是在上边缘。
还有一些技巧...