可以将BFC理解成一个结界：
里面的人出不去，外面的人进不来，具有极强的防御力。

如果一个元素具有 BFC，内部子元素再怎么翻江倒海、翻云覆雨，都不会影响外部的元素。
（也可以理解成，一个具有BFC的元素，不会影响其他元素的布局）
根据上面这条规则，可以推导出：
BFC 元素是不可能发生 margin 重叠的，因为 margin
重叠是会影响外面的元素的；BFC 元素也可以用来清除浮动的影响，因为如果不清除，子元素
浮动则父元素高度塌陷，必然会影响后面元素布局和定位，这显然有违 BFC 元素的子元素不会
影响外部元素的设定。

那什么时候会触发 BFC 呢？常见的情况如下： 
• `<html>`根元素；
• float 的值不为 none；
• overflow 的值为 auto、scroll 或 hidden；
• display 的值为 table-cell、table-caption 和 inline-block 中的任何一个； 
• position 的值不为 relative 和 static。

换言之，只要元素符合上面任意一个条件，就无须使用 clear:both 属性去清除浮动的
影响了。


具有 BFC 特性的元素的子元素不会受外部元素影响，也不会影响外
部元素。于是，具有 BFC 特性的元素为了不和浮动元素产生任何交集，顺着浮动边缘形成自己
的封闭上下文。
比如，普通流体元素在设置了 overflow:hidden 后，会自动填满容器中除了浮
动元素以外的剩余空间，形成自适应布局。
而且这种自适应布局要比纯流体自适应
更加智能。比方说，我们让图片的尺寸变小或变大，右侧自适应内容无须更改任何样式代
码，都可以自动填满剩余的空间。
但如果有左浮动元素，右边元素overflow:hidden;那么右边元素不要使用margin-left,
除非已知左浮动元素的尺寸。


实际项目开发的时候，两栏自适应布局中图片和文字不可能靠这么近，如果想要
保持合适的间距，那也很简单，如果元素是左浮动，则浮动元素可
以设置 margin-right 成透明 border-right 或 padding- 
right；又或者右侧 BFC 元素设置成透明 border-left 或者
padding-left，但不包括 margin-left，因为如果想要使用 margin-left，则其值必须
是浮动元素的宽度加间隙的大小，就变成动态不可控的了，无法大规模复用。因此，套用上面
例子的 HTML，假设我们希望间隙是 10px，则下面这几种写法都是可以的： 
• img { margin-right: 10px; }
• img { border-right: 10px solid transparent; }
• img { padding-right: 10px; }
• .animal { border-left: 10px solid transparent; }
• .animal { padding-left: 10px; }
一般而言，我喜欢通过在浮动元素上设置 margin 来控制间距，也就是下面的 CSS 代码： 
img { 
 float: left; 
 margin-right: 10px; 
} 
.animal { 
 overflow: hidden; 
} 


关于多栏自适应布局的总结：
最后，我们可以提炼出两套 IE7 及以上版本浏览器适配的自适应解决方案。
（1）借助 overflow 属性，如下：
.lbf-content { overflow: hidden; } 
（2）融合 display:table-cell 和 display:inline-block，如下：
```
.lbf-content { 
 display: table-cell; width: 9999px; 
 /* 如果不需要兼容 IE7，下面样式可以省略 */ 
 *display: inline-block; *width: auto;
}
``` 
这两种基于 BFC 的自适应方案均支持无限嵌套，因此，多栏自适应可以通过嵌套方
式实现。这两种方案均有一点不足，前者如果子元素要定位到父元素的外面可能会被隐藏，
后者无法直接让连续英文字符换行。所以，大家可以根据实际的项目场景选择合适的技术
方案。
最后，关于 display:table-cell 元素内连续英文字符无法换行的问题，事实上是可以
解决的，就是使用类似下面的 CSS 代码： 
.word-break { 
 display: table; 
 width: 100%; 
 table-layout: fixed; 
 word-break: break-all; 
} 


补充：display:table-cell
单元格(table-cell)有
一个非常神奇的特性，就是宽度值设置得再大，实际宽度也
不会超过表格容器的宽度。
因此，如果我们把 display:table-cell 这个 BFC
元素宽度设置得很大，比方说 3000px，那其实就跟 block
水平元素自动适应容器空间效果一模一样了，除非你的容器
宽度超过 3000px。实际上，一般 Web 页面不会有 3000px
宽的模块，所以，要是实在不放心，设个 9999px 好了！

