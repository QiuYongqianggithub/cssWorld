line-height 的好朋友 vertical-align 
终于轮到 line-height 的好朋友 vertical-align 上场了，为什么说它们是好朋友
呢？因为凡是 line-height 起作用的地方 vertical-align 也一定起作用，只是很多时候，
vertical-align 默默地在背后起作用，你没有感觉到而已。 


我们给文字内容设置 vertical-align:10px，
则文字内容就会在当前基线位置再往上精确偏移 10px，
实际上，负值全部都是往下偏移，正值全部都是往上偏移，
而且数值大小全部都是相对于基线位置计算的，
因此，从这一点来看，vertical-align:baseline等同于vertical-align:0。
如果我们使用类似 vertical-align:10px 这样的定位，是不
会有任何兼容性问题的


vertical-align 属性的百分比值则是相对于 line-height 的计算值计算的
在如今的网页布局中，line-height 的计算值都是相对固定并且已知的，
因此，直接使用具体的数值反而更方便。



vertical-align 作用的前提 
vertical-align 起作用是有前提条件的，这个前提条件就是：只能应用于内联元
素以及 display 值为 table-cell 的元素。 
换句话说，vertical-align 属性只能作用在 display 计算值为 inline、inline-block，inline-table 或 table-cell 的元素上。因此，默认情况下，`<span>`、`<strong>`、
`<em>`等内联元素，`<img>`、`<button>`、`<input>`等替换元素，非 HTML 规范的自定义标签
元素，以及`<td>`单元格，都是支持 vertical-align 属性的，其他块级元素则不支持。 
当然，现实世界是没有这么简单的。CSS 世界中，有一些 CSS 属性值会在背后默默
地改变元素 display 属性的计算值，从而导致 vertical-align 不起作用。比方说，
浮动和绝对定位会让元素块状化，导致vertical-align没有作用

此外，设置vertical-align如果要使内联元素能够自由调整位置，
往往需要有足够的行框盒子高度
例如：
```
.box { 
 height: 128px; 
} 
.box > img { 
 height: 96px; 
 vertical-align: middle; 
} 
<div class="box"> 
 <img src="1.jpg"> 
</div> 
```
此时图片顶着.box 元素的上边缘显示，根本没垂直居中，完全没起作用！ 
这种情况看上去是 vertical-align:middle 没起作用，实际上，vertical-align
是在努力地渲染的，只是行框盒子前面的“幽灵空白节点”高度太小，如果我们通过设置一个
足够大的行高让“幽灵空白节点”高度足够，就会看到 vertical-align:middle 起作用了， 
CSS 代码如下：
``` 
.box { 
 height: 128px; 
 line-height: 128px; /* 关键 CSS 属性 */ 
} 
.box > img { 
 height: 96px; 
 vertical-align: middle; 
} 
```

注意：为了要实现（设置为display:table-cell的元素）的子元素垂直表现，要把vertical-align设置到自身上。
虽然就效果而言，table-cell 元素设置 vertical-align 垂直对齐的是子元素，但是其作用的并不是子元素，而是 table-cell 元素自身。就算 table-cell 元素的子元素是一个块级元素，也一样可以让其有各种垂直对齐表现。

```
.cell { 
 height: 128px; 
 display: table-cell; 
} 
.cell > img { 
 height: 96px; 
 vertical-align: middle; 
} 
<div class="cell"> 
 <img src="1.jpg"> 
</div> 
```
结果图片并没有要垂直居中的迹象，还是紧贴着父元素的上边缘
但是，如果 vertical-align:middle 是设置在 table-cell 元素上，就能实现需求，CSS 代码如下：

```
.cell { 
 height: 128px; 
 display: table-cell; 
 vertical-align: middle; 
} 
.cell > img { 
 height: 96px; 
} 
```


vertical-align 和 line-height 之间的关系
vertical-align 的百分比值是相对于 line-height 计算的


内联盒子位移问题：

```
.box { line-height: 32px; } 
.box > span { font-size: 24px; } 
<div class="box"> 
 x<span>文字x</span> 
</div> 
```
 24px 的 font-size 大小是设置在`<span>`元素上的，这就导
致了外部`<div>`元素的字体大小和`<span>`元素有较大出入

此时，我们可以明显看到两处大小完全不同的文字。一处是字母 x 构成了一个“匿名内联
盒子”，另一处是“文字 x”所在的`<span>`元素，构成了一个“内联盒子”。由于都受 line- 
height:32px 影响，因此，这两个“内联盒子”的高度都是 32px。

因为文字默认全部都是基线对齐，所以当字
号大小不一样的两个文字在一起的时候，彼此就会发生上下位移，如果位移距离足够大，两个内联盒子会发生上下相对位移，导致父元素高度大于其所设置的行高值

知道了问题发生的原因，那问题就很好解决了。我们可以让“幽灵空白节点”和后面span
元素字号一样大，也就是：
```
.box { 
 line-height: 32px; 
 font-size: 24px; 
} 
.box > span { } 
```
或者改变垂直对齐方式，如顶部对齐，这样就不会有参差位移了：
```
.box { line-height: 32px; } 
.box > span { 
 font-size: 24px; 
 vertical-align: top; 
} 
```


图片底部间隙问题
任意一个块级元素，里面若有图片，则块级元素高度基本上都要比图片的高度高。
知道了原理，要清除该间隙，就知道如何对症下药了。方法很多，具体如下。
（1）图片块状化。可以一口气干掉“幽灵空白节点”、line-height 和 vertical- 
align。 
（2）容器 line-height 足够小。只要半行间距小到字母 x 的下边缘位置或者再往上，自
然就没有了撑开底部间隙高度空间了。比方说，容器设置 line-height:0。 
（3）容器 font-size 足够小。此方法要想生效，需要容器的 line-height 属性值和当
前 font-size 相关，如 line-height:1.5 或者 line-height:150%之类；否则只会让下
面的间隙变得更大，因为基线位置因字符 x 变小而往上升了。 
（4）图片设置其他 vertical-align 属性值。间隙的产生原因之一就
是基线对齐，所以我们设置 vertical-align 的值为 top、middle、bottom 中的任意一个都是可以的。


内联特性导致的 margin 无效
```
<div class="box"> 
 <img src="mm1.jpg"> 
</div> 
.box > img { 
 height: 96px; 
 margin-top: -200px; 
} 
```
此时，按照理解，-200px 远远超过图片的高度，图片应该完全跑到容器的外面，但是，
图片依然有部分在.box 元素中，而且就算 margin-top 设置成-99999px，图片也不会继续
往上移动，完全失效。


这里涉及到两条原理：

1.非人为设置位移的内联元素是不可能跑到计算容器（父元素）外面的
2.内联元素总以基线对齐

所以导致图片的位置被“幽灵空白节点”的 vertical-align:baseline 给限死了。



vertical-align 属性的默认值 baseline 在文本之类的内联元素那里就是字符 x 的下
边缘，对于替换元素则是替换元素的下边缘。但是，如果是 inline-block 元素，则规则要
复杂了：一个 inline-block 元素，如果里面没有内联元素，或者 overflow 不是 visible，
则该元素的基线就是其 margin 底边缘；否则其基线就是元素里面最后一行内联元素的基线。


vertial-align:top 就是垂直上边缘对齐，具体定义如下。 
• 内联元素：元素底部和当前行框盒子的顶部对齐。
• table-cell 元素：元素底 padding 边缘和表格行的顶部对齐。
用更通俗的话解释就是：如果是内联元素，则和这一行位置最高的内联元素的顶部对齐；
如果 display 计算值是 table-cell 的元素，我们不妨脑补成`<td>`元素，则和`<tr>`元素上边缘对齐。
vertial-align:bottom 声明与此类似，只是把“顶部”换成“底部”，把“上边缘”
换成“下边缘”。

需要注意的是，内联元素的上下边缘对齐的这个“边缘”是当前“行框盒子”的上下边缘，
并不是块状容器的上下边缘。

vertial- align:middle 的定义有关。 
• 内联元素：元素的垂直中心点和行框盒子基线往上 1/2 x-height 处对齐。
• table-cell 元素：单元格填充盒子相对于外面的表格行居中对齐。


vertical-align:super：提高盒子的基线到父级合适的上标基线位置。
z vertical-align:sub：降低盒子的基线到父级合适的下标基线位置。


