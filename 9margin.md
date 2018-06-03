首先定义：
元素尺寸：包括 padding和 border，也就是元素的 border box 的尺寸。
元素内部尺寸：表示元素的内部区域尺寸，包括 padding 但不包括 border，也就是元素的 paddingbox 的尺寸。
元素外部尺寸：不仅包括 padding 和 border，还包括 margin

已经知道，当width:auto时，具有充分利用可用空间的特性。
现在又有：
只要元素的尺寸表现符合“充分利用可用空间”，无论是垂直方向还是水
平方向，都可以通过 margin 改变尺寸(注意：这里说的改变尺寸既可以指增大尺寸也可以是减小尺寸)

比方说，如下 CSS中margin不可改变尺寸
此时元素宽度还是 300 像素，尺寸无变化。
```
.father { 
 width: 300px; 
 margin: 0 -20px; 
} 
```

如下margin可以改变尺寸 

```
<div class="father"> 
 <div class="son"></div> 
</div> 
.father { width: 300px; } 
.son { margin: 0 -20px; } 
```

margin对于元素内部尺寸的影响情形：
可以改变：
1CSS 世界默认的流方向是水平方向，因此，对于普通流体元素，margin 只能改变元素（width:auto时）水
平方向尺寸；
2但是，对于具有拉伸特性的绝对定位元素，则水平或垂直方向都可以，因为此时
的尺寸表现符合“充分利用可用空间”。



margin对于元素外部尺寸：
垂直方向 margin 无法改变元素的内部尺寸，但却能改变外部尺寸，这里我们设置了
margin-bottom:-9999px 意味着元素的外部尺寸在垂直方向上小了 9999px。默认情况下，垂
直方向块级元素上下距离是 0，一旦 margin-bottom:-9999px 就意味着后面所有元素和上面元
素的空间距离变成了-9999px，也就是后面元素都往上移动了 9999px。



内联元素垂直方向的 margin 是没有任何影响的，既不会影响外部尺寸，
也不会影响内部尺寸，有种石沉大海的感觉。对于水平方向，由于内联元素宽度表现为“包裹
性”，也不会影响内部尺寸。




 margin 的百分比值 
和 padding 属性一样，margin 的百分比值无论是水平方向还是垂直方向都是相对于父元素宽度计算
的。不过相对于 padding，margin 的百分比值的应用价值就低了一截，根本原因在于和 padding
不同，元素设置 margin 在垂直方向上无法改变元素自身的内部尺寸，往往需要父元素作为载体，此
外，由于 margin 合并的存在，垂直方向往往需要双倍尺寸才能和 padding 表现一致。例如：
``` 
.box { 
 background-color: olive; 
 overflow: hidden; 
} 
.box > div { 
 margin: 50%; 
} 
<div class="box"> 
 <div></div> 
</div> 
```
结果.box 是一个宽高比为 2:1 的橄榄绿长方形。是不是有点儿奇怪：50%+50%应该是 100%，
应该上下一样，是 1:1 的正方形，怎么最后是 2:1 的长方形呢？ 
这是因为margin合并了。



margin合并的要点：
（1）块级元素，但不包括浮动和绝对定位元素，尽管浮动和绝对定位可以让元素块状化。
（2）只发生在垂直方向，需要注意的是，这种说法在不考虑 writing-mode 的情况下才
是正确的，严格来讲，应该是只发生在和当前文档流方向的相垂直的方向上。由于默认文档流
是水平流，因此发生 margin 合并的就是垂直方向。 


margin 合并的 3 种场景 
（1）相邻兄弟元素 margin 合并。例如：
```
p { margin: 1em 0; } 
<p>第一行</p>
<p>第二行</p>
```
（2）父级和第一个/最后一个子元素。

注意：当父元素没有显式设置margin时，但子元素设置了margin-top,
则虽然是在子元素上设置的 margin-top，但实际上就等同于在父元素上设置了 margin-top

（3）空块级元素的 margin 合并
例如空`<div>`元素的 margin-top 和 margin-bottom 合并在一起



那该如何阻止这里 margin 合并的发生呢？ （满足一个条件即可）： 
• 父元素设置为块状格式化上下文元素；
• 父元素设置 border-top 值；
• 父元素设置 padding-top 值；
• 父元素和第一个子元素之间添加内联元素进行分隔。


如果有人不希望空div元素有 margin 合并，可以进行如下操作： 
• 设置垂直方向的 border；
• 设置垂直方向的 padding；
• 里面添加内联元素（直接 Space 键空格是没用的）；
• 设置 height 或者 min-height。


margin 合并的计算规则
我把 margin 合并的计算规则总结为“正正取大值”“正负值相加”“负负最负值”3 句话。

margin:auto
margin:auto 就是为了填充宽度设置之后闲置的尺寸而设计的！
margin:auto 的填充规则如下。 
（1）如果一侧定值，一侧 auto，则 auto 为剩余空间大小。
（2）如果两侧均是 auto，则平分剩余空间。

margin 的初始值大小是 0（即没有明确设置margin时，margin为0）:所以下面的margin-left是100
```
.father { 
 width: 300px; 
}
.son { 
 width: 200px; 
 margin-left: auto; 
} 
```

如果想让某个块状元素右对齐，脑子里不要就一个 float:right，很多时候，margin- 
left:auto 才是最佳的实践，浮动毕竟是个“小魔鬼”。我甚至可以这么说：margin 属性的
auto 计算就是为块级元素左中右对齐而设计的，和内联元素使用 text-align 控制左中右对
齐正好遥相呼应！


居中对齐左右同时 auto 计算即可，CSS 如下： 
.son { 
 width: 200px; 
 margin-right: auto; 
 margin-left: auto; 
} 

如何实现水平垂直居中？
```
.father { 
 width: 300px; height:150px; 
 position: relative; 
} 
.son { 
 position: absolute; 
 top: 0; right: 0; bottom: 0; left: 0; 
} 
```
其中
`position: absolute; top: 0; right: 0; bottom: 0; left: 0;`的设置可以让
.son 这个元素的尺寸表现为“格式化宽度和格式化高度”，和`<div>`的“正常流宽度”一
样，同属于外部尺寸，也就是尺寸自动填充父级元素的可用尺寸，此时我们给.son 设置尺寸。
例如：
```
.son { 
 position: absolute; 
 top: 0; right: 0; bottom: 0; left: 0; 
 width: 200px; height: 100px; 
} 
```
此时宽高被限制，原本应该填充的空间就被空余了出来，这多余的空间就是 margin:auto
计算的空间，因此，如果这时候我们再设置一个 margin:auto：
``` 
.son { 
 position: absolute; 
 top: 0; right: 0; bottom: 0; left: 0; 
 width: 200px; height: 100px; 
 margin: auto; 
} 
```
那么我们这个.son 元素就水平方向和垂直方向同时居中了。因为 auto 正好把上下左右剩余空
间全部等分了，自然就居中！



由于绝对定位元素的格式化高度即使父元素 height:auto 也是支持的，因此，其应用场
景可以相当广泛，可能唯一的不足就是此居中计算 IE8 及以上版本浏览器才支持。至少对我来
讲，如果项目无须兼容 IE7 浏览器，绝对定位下的 margin:auto 居中是我用得最频繁的块级
元素垂直居中对齐方式，比 top:50%然后 margin 负一半元素高度的方法要好使得多。


假如说这里面的元素尺寸比外面的大，那这个 auto
该怎么计算呢？
很简单，如果里面元素尺寸大，说明剩余可用空间都没有了，会被当作 0 来处理，也就是
auto 会被计算成 0，其实就等于没有设置 margin 属性值，因为 margin 的初始值就是 0。

对于替换元素，如果我们设置 display:block，则 margin:auto 的计算规则同
样适合。



 margin 无效情形解析 

（1）display 计算值 inline 的非替换元素的垂直 margin 是无效的
对于内联替换元素，
垂直 margin 有效，并且没有 margin 合并的问题，所以图片永远不会发生 margin 合并。

（2）表格中的`<tr>`和`<td>`元素或者设置 display 计算值是 table-cell 或 table-row 的
元素的 margin 都是无效的。
但是，如果计算值是 table-caption、table 或者 inline-table
则没有此问题，可以通过 margin 控制外间距，甚至::first-letter 伪元素也可以解析 margin。 

（3）margin 合并的时候，更改 margin 值可能是没有效果的。以父子 margin 重叠为例，
假设父元素设置有 margin-top:50px，则此时子元素设置 margin-top:30px 就没有任何
效果表现，除非大小比 50px 大，或者是负值。

（4）绝对定位元素非定位方位的 margin 值“无效”。
即：绝对定位中设置了left,则在left方向的margin才有效

（5）定高容器的子元素的 margin-bottom 或者宽度定死的子元素的 margin-right 的
定位“失效”（实际上是 margin 自身的特性导致，有渲染只是你看不到变化而已。）
关于此处“失效”的解释：
这个现象的本质和上面绝对定位元素非定位方位 margin 值“无效”类似。原因在于，若想使
用 margin 属性改变自身的位置，必须是和当前元素定位方向一样的 margin 属性才可以，否
则，margin 只能影响后面的元素或者父元素。 
例如，一个普通元素，在默认流下，其定位方向是左侧以及上方，此时只有 margin-left
和 margin-top 可以影响元素的定位。但是，如果通过一些属性改变了定位方向，如
float:right 或者绝对定位元素的 right 右侧定位，则反过来 margin-right 可以影响元
素的定位，margin-left 只能影响兄弟元素。 


（6）鞭长莫及导致的 margin 无效。我们直接看下面这个例子：
```
<div class="box"> 
 <img src="mm1.jpg"> 
<p>内容</p>
</div> 

.box > img { 
 float: left; 
 width: 256px; 
} 
.box > p { 
 overflow: hidden; 
 margin-left: 200px; 
} 
```
其中的 margin-left:200px 是无效的，准确地讲，此时的`<p>`的 margin-left 从负无穷到
256px 都是没有任何效果的。


（7）内联特性导致的 margin 无效。我们直接看下面这个例子：
```
<div class="box"> 
 <img src="mm1.jpg"> 
</div> 
.box > img { 
 height: 96px; 
 margin-top: -200px; 
} 
```
这里的例子也很有代表性。一个容器里面有一个图片，然后这张图片设置 margin-top
负值，让图片上偏移。但是，随着我们的负值越来越负，结果达到某一个具体负值的时候，图
片不再往上偏移了。比方说，本例 margin-top 设置的是-200px，如果此时把 margin-top
设置成-300px，图片会再往上偏移 100px 吗？不会！它会微丝不动，margin-top 变得无效
了。要解释这里为何会无效，需要对 vertical-align 和内联盒模型有深入的理解











