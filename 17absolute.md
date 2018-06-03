“块状化”和浮动类似，元素一旦 position 属性值为 absolute 或 fixed，其
display 计算值就是 block 或者 table。

“破坏性”，指的是破坏正常的流
特性。和 float 类似

能“块状格式化上下文”，
也就是 BFC。 

都具有“包裹性”，也就是尺寸收缩包裹，同时具有自适应性。
 absolute 天然具有“包裹性”，因此没有必要使用 display:inline-block


absolute 的自适应性最大宽度往往不是由父元素决定的，本质上说，这个差异是由“包
含块”的差异决定的。

absolute 的包含块:
（1）根元素（很多场景下可以看成是`<html>`）被称为“初始包含块”，其尺寸等同于浏览
器可视窗口的大小。
（2）对于其他元素，如果该元素的 position 是 relative 或者 static，则“包含块”
由其最近的块容器祖先盒的 content box 边界形成。 
（3）如果元素 position:fixed，则“包含块”是“初始包含块”。
（4）如果元素 position:absolute，则“包含块”由最近的 position 不为 static
的祖先元素建立


absolute 绝对定位元素的“包含块”有以下 3 个明显差异： 
（1）内联元素也可以作为“包含块”所在的元素；
（2）“包含块”所在的元素不是父块级元素，而是最近的 position 不为 static 的祖先
元素或根元素；
（3）边界是 padding box 而不是 content box。


注意：内联盒子作为包含块时，其大小是由设置absolute的元素前后的内联元素决定的。

height100%和height:inherit对于普通元素，两者确实没
什么区别，但是对于绝对定位元素就不一样了。height:100%是第一个具有定位属性值的祖先
元素的高度，而 height:inherit 则是单纯的父元素的高度继承

当包含块空间不足，导致定位元素内容“一柱擎天”时，可以：
给包含块元素添加 white-space: 
nowrap，让宽度表现从“包裹性”变成“最大可用宽度”


具有相对特性的无依赖 absolute 绝对定位：
一个绝对定位元素，没
有任何 left/top/right/bottom 属性设置，并且其祖先元素全部都是非定位元素，其位置在
哪里？
很多人都认为是在浏览器窗口左上方。实际上，还是当前位置，不是在浏览器左上方

我把这种没有设置 left/top/ 
right/bottom 属性值的绝对定位称为“无依赖绝对定位”。很多场景下，
“无依赖绝对定位”要比使用 left/top 之类属性定位实用和强大很多，因
为其除了代码更简洁外，还有一个很棒的特性，就是“相对定位特性”。

“无依赖绝对定位”本质上就是“相对定位”，仅仅是不占据 CSS 流的尺寸空间而已

 无依赖绝对定位position:absolute，然后简单的 margin 偏移实现。此方法兼容性很好




同时设置浮动和绝对定位，浮动是没有任何效果的。



如果overflow 不是定位元素，同时绝对定位元素和 overflow 容器之间也没有定位元素，则
overflow 无法对 absolute 元素进行剪裁。 
如果 overflow 的属性值不是 hidden 而是 auto 或者 scroll，即使绝对定位元素高宽
比 overflow 元素高宽还要大，也都不会出现滚动条。

由于 position:fixed 固定定位元素的包含块是根
元素，因此，除非是窗体滚动，否则上面讨论的所有 overflow 剪裁规则对固定定位都不适用。




clip 属性要想起作用，元素必须是绝对定位或者固定定位

clip: rect(top, right, bottom, left) 

最后总结一下：clip 隐藏仅仅是决定了哪部分是可见的，非可见部分无法响应点击事件
等；然后，虽然视觉上隐藏，但是元素的尺寸依然是原本的尺寸，在 IE 浏览器和 Firefox 浏览
器下抹掉了不可见区域尺寸对布局的影响，Chrome 浏览器却保留了。




 当 absolute 遇到 left/top/right/bottom 属性 
当 absolute 遇到 left/top/right/bottom 属性的时候，absolute 元素才真正变成
绝对定位元素。例如：
.box { 
 position: absolute; 
 left: 0; top: 0; 
} 
表示相对于绝对定位元素包含块的左上角对齐，此时原本的相对特性丢失。但是，如果我们仅
设置了一个方向的绝对定位，又会如何呢？例如：
.box { 
 position: absolute; 
 left: 0; 
} 
此时，水平方向绝对定位，但垂直方向的定位依然保持了相对特性。



 absolute 的流体特性

当absolute元素对立方向同时发生定位的时候，具有流体特性。


如果只有 left 属性或者只有 right 属性，则由于包裹性，此时absolute元素 宽度是 0。
但如果 left 和 right 同时存在，所以宽度就不是 0，而是表现为“格式化宽度”，宽
度大小自适应于absolute元素 包含块的 padding box，也就是说，如果包含块 padding box 宽度发生变
化，.box 的宽度也会跟着一起变。 

假设.box 元素的包含块是根元素，则下面的代码可以让.box 元素正好完全覆盖浏
览器的可视窗口，并且如果改变浏览器窗口大小，.box 会自动跟着一起变化：
``` 
.box { 
 position: absolute; 
 left: 0; right: 0; top: 0; bottom: 0; 
} 
```
绝对定位元素的这种流体自适应特性从 IE7 就开始支持了，但是出于历史习惯或者其他什
么原因，很多同行依然使用下面这样的写法：
下面的这个.box已经完全丧失了流动性（已经是一个固定大小的元素了）
```
.box { 
 position: absolute; 
 left: 0; top: 0; 
 width: 100%; height: 100%; 
} 
```
设置了对立定位属性的绝对定位元素的表现神似普通的
`<div>`元素，无论设置 padding 还是 margin，其占据的空间一直不变，变化的就是 content box
的尺寸，这就是典型的流体表现特性。


当绝对定位元素处于流体状态的时候，各个盒模型相关属性的解析和普通流体元素都是一
模一样的，margin 负值可以让元素的尺寸更大，并且可以使用 margin:auto 让绝对定位元
素保持居中。
绝对定位元素的 margin:auto 的填充规则和普通流体元素的一模一样： 
• 如果一侧定值，一侧 auto，auto 为剩余空间大小；
• 如果两侧均是 auto，则平分剩余空间。
唯一的区别在于，绝对定位元素 margin:auto 居中从 IE8 浏览器开始支持，而普通元素
的 margin:auto 居中很早就支持了。

如果项目不需要管 IE7 浏览器的话，下面这种绝对定位元素水平垂直居中用法就可以直接
淘汰了：
```
.element { 
 width: 300px; height: 200px; 
 position: absolute; left: 50%; top: 50%; 
 margin-left: -150px; /* 宽度的一半 */ 
 margin-top: -100px; /* 高度的一半 */ 
} 
```

如果绝对定位元素的尺寸是已知的，也没有必要使用下面这种用法，因为按照我的经验，
在有些场景下，百分比 transform 会让 iOS 微信闪退，还是尽量避免的好。 
```
.element { 
 width: 300px; height: 200px; 
 position: absolute; left: 50%; top: 50%; 
 transform: translate(-50%, -50%); /* 50%为自身尺寸的一半 */ 
} 
```
首推的方法就是利用绝对定位元素的流体特性和 margin:auto 的自动分配特性实现居
中，例如：
```
.element { 
 width: 300px; height: 200px; 
 position: absolute; 
 left: 0; right: 0; top: 0; bottom: 0; 
 margin: auto; 
} 
```




