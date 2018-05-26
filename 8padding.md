对于内联元素：
padding在竖直方向上尺寸虽有效，但是对上下元素的原本布局却没有任何影响。
仅仅是垂直方向发生了层叠
CSS 中还有很多其他场景或属性会出现这种不影响其他元素布局而是出现层叠效果的现
象。比如，relative 元素的定位、盒阴影 box-shadow 以及 outline 等。这些层叠现象虽
然看似类似，但实际上是有区别的。其分为两类：
一类是纯视觉层叠，不影响外部尺寸；如box-shadow 以及 outline ；
另一类则会影响元素自身尺寸。例如 inline 元素的padding 层叠。

区分的方式很简单，如果父容器 overflow:auto，层叠区域超出父
容器的时候，没有滚动条出现，则是纯视觉的；如果出现滚动条，则会影响元素自身尺寸、影响布局。


应用：
内联元素的垂直 padding 的样式表现了，目的就是增加点击区域
同时对现有布局无任何影响；如果我们设置成 inline-block，则行间距等很多麻烦事就会出来。

可以借助内联元素和 padding 属性来实现选项之间的|，CSS 和
HTML 代码如下：

``` 
a + a:before { 
 content: ""; 
 font-size: 0; 
 padding: 10px 3px 1px; 
 margin-left: 6px; 
 border-left: 1px solid gray; 
} 
<a href="">登录</a><a href="">注册</a> 
```

对于非替换元素的内联元素，不仅 padding 不会加入行盒高度的计算，margin
和 border 也都是如此，都是不计算高度，但实际上在内联盒周围发生了渲染


关于 padding 的属性值:
对于块级元素
其一，和 margin 属性不同，padding 属
性是不支持负值的；其二，padding 支持百分比值，但是，和 height 等属性的百分比计算规则
有些差异，差异在于：padding 百分比值无论是水平方向还是垂直方向均是相对于父元素宽度计算的！


对于内联元素
同样相对于宽度计算；
默认的高度和宽度细节有差异：即使是空的内联元素，仍然占据高度（幽灵空白节点，由于内联元素默认的高度完全受 font-size 大小控制）
padding 会断行：不管文字是否换行，padding会在文字的开头和结尾处分别渲染


padding的一个未定义行为
如果容器（父元素）比子元素（设置了padding-bottom）高度要小，会出现滚动条，
在 IE 和 Firefox 浏览器下是会忽略 padding-bottom 值的，
Chrome 等浏览器则不会。也就是说，在 IE 和 Firefox 浏览器下：
``` 
<div style="height:100px; padding:50px 0;"> 
 <img src="0.jpg" height="300"> 
</div> 
```
底部没有 50 像素的 padding-bottom 间隙。 
此兼容性差异的本质区别在于：Chrome 浏览
器是子元素超过 content box 尺寸触发滚动条显示，
而 IE 和 Firefox 浏览器是超过 padding box 尺寸触发滚动条显示。
由于规范中并没有找到准确的说明，因此，浏
览器之间不同的做法不能说孰对孰错，可以看成是
一种“未定义行为”。

记住了，只能使用子元素的 margin-bottom 来实现滚动容器的底部留白。 



















