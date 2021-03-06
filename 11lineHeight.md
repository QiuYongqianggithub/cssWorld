对于非替换元素的纯内联元素，其可
视高度完全由 line-height 决定。注意这里的措辞—“完全”，什么
padding、border 属性对可视高度是没有任何影响的

因此，对于文本这样的纯内联元素，line-height 就是高度计算的
基石，用专业说法就是指定了用来计算行框盒子高度的基础高度。比方说，line-height 设为
16px，则一行文字高度是 16px，两行就是 32px，三行就是 48px，所有浏览器渲染解析都是
这个值，1 像素都不差。 


行距 = line-height - font-size

内容区域（content area）出马了。在本书中，内容区域可以近似理解为 Firefox/IE
浏览器下文本选中带背景色的区域


当我们的字体是宋体的时候，内容区域和 em-box 是等同的


关于替换元素的高度与 line-height 的关系首先需要弄明白这个问题：line-height
可以影响替换元素（如图片的高度）吗？答案是，不可以！



实际开发的时候，图文和文字混在一起是很常见的，那这种内联替换元素和内联非替换元
素在一起时的高度表现又是怎样的呢？
由于同属内联元素，因此，会共同形成一个“行框盒子”，line-height 在这个混合元素
的“行框盒子”中扮演的角色是决定这个行盒的最小高度，听上去似乎有点儿尴尬，对于纯文
本元素，line-height 非常威风，直接决定了最终的高度。但是，如果同时有替换元素，则
line-height 的表现一下子弱了很多，只能决定最小高度，对最终的高度表现有望尘莫及之
感。为什么会这样呢？一是替换元素的高度不受 line-height 影响，二是 vertical-align
属性在背后作祟。


对于块级元素，line-height 对其本身是没有任何作用的，我们平时改变 line-height，
块级元素的高度跟着变化实际上是通过改变块级元素里面内联级别元素占据的高度实现的。


比方说，一个`<p>`元素中有 10 行图文内容，则这个`<p>`元素的高度就是由这 10 行内容产
生的 10 个“行框盒子”高度累加而成；一个`<article>`元素中可能会有 20 个`<p>`元素，则这
个`<article>`元素又是由这 20 个块级`<p>`元素高度累加而成，同时块级元素还可以通过
height 和 min-height 以及盒模型中的 margin、padding 和 border 等属性改变占据的高
度，所有这一切就构成了 CSS 世界完整的高度体系。 


要想让单行文字垂直居中，只要设置 line-height 大小和
height 高度一样就可以了
只需要 line-height 这一个属性就可以


多行文本或者替换元素的垂直居中实现原理和单行文本就不一样了，需要 line-height
属性的好朋友 vertical-align 属性帮助才可以，示例代码如下： 
```
.box { 
 line-height: 120px; 
 background-color: #f0f3f9; 
} 
.content { 
 display: inline-block; /*生成一个独立的行框盒子（前面有幽灵空白节点）*/
 line-height: 20px; 
 margin: 0 20px; 
 vertical-align: middle; 
} 
<div class="box"> 
 <div class="content">基于行高实现的...</div> 
</div> 
```

原理：
我们需要的其实并不是
“行框盒子”，而是每个“行框盒子”都会附带的一个产物—“幽灵空白节点”，即一个宽度为
0、表现如同普通字符的看不见的“节点”。有了这个“幽灵空白节点”，我们的 line- 
height:120px 就有了作用的对象，从而相当于在.content 元素前面撑起了一个高度为
120px 的宽度为 0 的内联元素。 



line-height 的属性值 normal 
只要字体确定，各个浏览器下的默认 line-height 解析值基本上都是一样的。
然而，关键问题是，不同的浏览器所使用的默认中英文字体并不是一样的，并且不同操作系统
的默认字体也不一样，换句话说，就是不同系统不同浏览器的默认 line-height 都是有差异
的。因此，在实际开发的时候，对 line-height 的默认值进行重置是势在必行的。


 数值，如 line-height:1.5，其最终的计算值是和当前 font-size 相乘后的值。
百分比值，如 line-height:150%，其最终的计算值是和当前 font-size 相乘后
的值。
长度值，也就是带单位的值，如 line-height:21px 或者 line-height:1.5em
等，此处 em 是一个相对于 font-size 的相对单位，因此，line-height:1.5em
最终的计算值也是和当前 font-size相乘后的值。例如，假设我们此时的 font-size
大小为 14px，则 line-height 计算值是 1.5`*`14px=21px。

如何计算line-height值？注意要计算的是line-height的值，而不是font-size的值！
似乎 line-height:1.5、line-height:150%和 line-height:1.5em 这 3 种
用法是一模一样的，最终的行高大小都是和font-size计算值，但是，实际上，line-height:1.5
和另外两个有一点儿不同，那就是继承细节有所差别。
如果使用数值作为 line-height 的属性值，那么所有的子元素line-height继承的都是这个值；
但是，如果使用百分比值或者长度值作为属性值，那么所有的子元素line-height继承的是最终的计算值。
简言之，line-height的属性值：
数字是继承自己这个数字给子元素，
百分比和em是继承计算值给子元素

会发现： line-height:1.5 的最终表现，排版令人
舒畅

我们可以不使用通配符而使用下面的方法来实现line-height的继承： 
```
body { 
 line-height: 1.5; 
} 
input, button { 
 line-height: inherit; 
} 
```

内联元素 line-height 的大值特性:
考虑：
```
.box { 
 line-height: 96px; 
} 
.box span { 
 line-height: 20px; 
} 
```
和
```
.box { 
 line-height: 20px; 
} 
.box span { 
 line-height: 96px; 
} 

```

回顾“幽灵空白节点”：
在
```
<div class="box"> 
   <span>内容...</span> 
</div> 
```
中的`<span>`是一个内联
元素，因此自身是一个“内联盒子”，本例就这一个“内联盒子”，只要有“内联盒子”在，就
一定会有“行框盒子”，就是每一行内联元素外面包裹的一层
看不见的盒子。然后，重点来了，在每个“行框盒子”前面有
一个宽度为 0 的具有该元素的字体和行高属性的看不见的“幽
灵空白节点”，如果套用本案，则这个“幽灵空白节点”就在
`<span>`元素的前方

由于幽灵空白节点的存在，就效果而言，我们的 HTML 实际上等同于：
``` 
<div class="box"> 
 字符<span>内容...</span> 
</div> 
```
这下就好理解了，当.box 元素设置 line-height:96px 时，“字符”高度 96px；当设
置 line-height:20px 时，`<span>`元素的高度则变成了 96px，而行框盒子的高度是由高度
最高的那个“内联盒子”决定的，这就是.box 元素高度永远都是最大的那个 line-height
的原因。
知道了原因也就能够对症下药，要避免“幽灵空白节点”的干扰，例如，设置`<span>`元
素 display:inline-block，创建一个独立的“行框盒子”，这样`<span>`元素设置的
line-height:20px 就可以生效了，这也是多行文字垂直居中示例中这么设置的原因

























