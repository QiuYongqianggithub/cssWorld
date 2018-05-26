


width 的默认值是 auto。auto少包含了以下 4 种不同的宽度表现。
width:auto 在不同场景下的宽度表现的简介： 
（1）width充分利用可用空间。比方说，`<div>`、`<p>`这些元素的宽度默认是 100%
  （当有padding，border,margin时，要先从父元素中扣除这些值）于父级容器的。
这种充分利用可用空间的行为还有个专有名字，叫作 fill-available。 
（2）width收缩到正好可以包裹内容。典型代表就是浮动、绝对定位、inline-block 元素或 table 元素，
英文称为 shrink-to-fit，直译为“收缩到合适”，我一直把这种现象称为“包裹性”。
CSS3 中的 fit-content 指的就是这种宽度表现。 
（3）width收缩到最小。这个最容易出现在 table-layout 为 auto 的表格中，想必有经验的
人一定见过图 3-4 所示的这样一柱擎天的盛况吧！ 
（4）无法超出容器。除非有明确的 width 相关设置，否则上面 3 种情况尺寸都不会主动
超过父级容器宽度的，但是存在一些特殊情况。例如，内容很长的连续的英文和数字，或者内联
元素被设置了 white-space:nowrap，则表现为“恰似一江春水向东流，流到断崖也不回头”

尺寸分“内部尺寸”和“外部尺寸”。其中“内部尺寸”表示尺寸由内部元素(如子元素)决定；
还有一类叫作“外部尺寸”，宽度由外部元素（如父元素）决定。
也就是`<div>`默认宽度 100%显示，是“外部尺寸”，
其余全部是“内部尺寸”。而这唯一的“外部尺寸”，是“流”的精髓所在。



1.外部尺寸与流体特性 
（1）流动性
流动性是一种 margin/border/padding和 content 内容区域自动分配水平空间的机制。 
此时不设置宽，将实现流动性。
（2）格式化宽度。
格式化宽度仅出现在“绝对定位模型”中，也就是出现在 position
属性值为 absolute 或 fixed 的元素中。在默认情况下，绝对定位元素的宽度表现是“包
裹性”，宽度由内部尺寸决定，但是，有一种情况其宽度是由外部尺寸决定的，是什么情况呢？
对于非替换元素，当 left/right 或 top/bottom 对立方位的属性值同时
存在的时候，元素的宽度表现为“格式化宽度”，其宽度大小相对于最近的具有定位 特 性
（position 属性值不是 static）的祖先元素计算。
“格式化宽度”具有完全的流动性，也就是 margin、border、 padding 和 content 内容区域同样会自动分配水平（和垂直）空间。
2．内部尺寸与流体特性 
如何快速判断一个元素使用的是否为“内部尺寸”呢？
很简单，假如这个元素里面没有内容，宽度就是 0，那就是应用的“内部尺寸”。

“自适应性”，指的是元素尺寸由内部元素决定，但永远小于“包含块”容器的尺寸。

“内部尺寸”有下面 3 种表现形式。 
（1）包裹并自适应。
对于一个元素，如果其 display 属性值是 inline-block，那么即使其里面内容
再多，只要是正常文本，宽度也不会超过容器（父元素）。

请看这个需求：页面某个模块的文字内容是动态的，可能是几个字，也可能是一句话。然
后，希望文字少的时候居中显示，文字超过一行的时候居左显示。该如何实现？

```
HTML：
<div class="box">
  <p id="conMore" class="content">文字内容</p>
</div>
<!-- 按钮 -->
<p><button id="btnMore">更多文字</button></p>

CSS：
.box {
  padding: 10px;
  background-color: #cd0000;
  text-align: center;
}
.content {
  display: inline-block;
  text-align: left;
}

JavaScript：
var btn = document.getElementById('btnMore'), 
  content = document.getElementById('conMore');

if (btn && content) {
  btn.onclick = function() {
    content.innerHTML += '-新增文字';
  };
}
```

除了 inline-block 元素，浮动元素以及绝对定位元素都具有包裹性，
均有类似的智能宽度行为。


（2）首选最小宽度。
所谓“首选最小宽度”，指的是元素最适合的最小宽度。我们接着上面的例子，在上面例子
中，外部容器的宽度是 240 像素，假设宽度是 0，请问里面的 inline-block 元素的宽度是多少？ 
是 0 吗？不是。在 CSS 世界中，图片和文字的权重要远大于布局，因此，CSS 的设计者显
然是不会让图文在 width:auto 时宽度变成 0 的，此时所表现的宽度就是“首选最小宽度”。
具体表现规则如下。
• 东亚文字（如中文）最小宽度为每个汉字的宽度，
• 西方文字最小宽度由特定的连续的英文字符单元
决定。并不是所有的英文字符都会组成连续单元，
一般会终止于空格（普通空格）、短横线、问号以
及其他非英文字符等。例如，“display:inline- 
block”这几个字符以连接符“-”作为分隔符，形
成了“display:inline”和“block”两个连续
单元，由于连接符“-”分隔位置在字符后面，因此，
最后的宽度就是“display:inline-”的宽度。

如果想让英文字符和中文一样，每一个字符都用最小宽度
单元，可以试试使用 CSS 中的 word-break:break-all。


（3）最大宽度。
最大宽度就是元素可以有的最大宽度。我自己是这么理解的，“最大宽
度”实际等同于“包裹性”元素设置 white-space:nowrap 声明后的宽
度。如果内部没有块级元素或者块级元素没有设定宽度值，则“最大宽度”实际上是最大的连
续内联盒子的宽度。
什么是连续内联盒子？这里你就简单地将其理
解为 display 为 inline、inline-block、inline-table 等元素。“连续内联盒子”指
的全部都是内联级别的一个或一堆元素，中间没有任何的换行标签`<br>`或其他块级元素。



“宽度分离”

.father { 
 width: 102px; 
} 
.son { 
 border: 1px solid; 
 padding: 20px; 
} 

多一个父级元素用来设定元素的总体宽，其余的border,padding,margin等都在元素自身上设置。
如此可以实现box-sizing:border-box;

“内在盒子”
的 4 个盒子它们分别是 content box、padding box、border box 和 margin box。默认情况下，width
是作用在 content box 上的，box-sizing 的作用就是可以把 width 作用的盒子变成其他几个，
因此，理论上，box-sizing 可以有下面这些写法：
``` 
.box1 { box-sizing: content-box; } 
.box2 { box-sizing: padding-box; } 
.box3 { box-sizing: border-box; } 
.box4 { box-sizing: margin-box; } 
```

理论美好，现实残酷。实际上，支持情况如下：

```
.box1 { box-sizing: content-box; } /* 默认值 */ 
.box2 { box-sizing: padding-box; } /* Firefox 曾经支持 */ 
.box3 { box-sizing: border-box; } /* 全线支持 */ 
.box4 { box-sizing: margin-box; } /* 从未支持过 */ 
```

如果要实现 box-sizing 的 margin-box 效果，如果
是 IE10 及以上版本浏览器，可以试试 flex 布局，如果要兼容 IE8 及以上版本可以使用“宽度分离”

box-sizing 被发明出来最大的初衷应该是解决替换元素宽度自适应问题：
在 CSS 世界中，唯一离不开 box-sizing:border-box 的
就是原生普通文本框`<input>`和文本域`<textarea>`的 100%自适应父容器宽度。 

拿文本域`<textarea>`举例，`<textarea>`为替换元素，替换元素的特性之一就是尺寸由
内部元素决定，且无论其 display 属性值是 inline 还是 block。这个特性很有意思，对于
非替换元素，如果其 display 属性值为 block，则会具有流动性，宽度由外部尺寸决定，但
是替换元素的宽度却不受 display 水平影响，因此，我们通过 CSS 修改`<textarea>`的
display 水平是无法让尺寸 100%自适应父容器的： 
```
textarea { 
 display: block; /* 还是原来的尺寸 */ 
} 
```
所以，我们只能通过 width 设定让`<textarea>`尺寸 100%自适应父容器。那么，问题就来了，
`<textarea>`是有 border 的，而且需要有一定的 padding 大小，否则输入的时候光标会顶
着边框，体验很不好。于是，width/border 和 padding 注定要共存，同时还要整体宽度 100%
自适应容器。如果不借助其他标签，肯定是无解的。


在浏览器还没支持 box-sizing 的年代，我们的做法有点儿类似于“宽度分离”，外面嵌
套`<div>`标签，模拟 border 和 padding，`<textarea>`作为子元素，border 和 padding
全部为 0，然后宽度 100%自适应父级`<div>`。 
手动输入 http://demo.cssworld.cn/3/2-9.php 。 
然而，这种模拟也有局限性，比如无法使用:focus 高亮父级的边框，
因为 CSS 世界中并无父选择器，只能使用更复杂的嵌套加其他 CSS 技巧来
模拟。
因此，说来说去，也就 box-sizing:border-box 才是根本解决之道！ 
```
textarea { 
 width: 100%; 
-ms-box-sizing: border-box; /* for IE8 */ 
box-sizing: border-box;
} 
```


























































