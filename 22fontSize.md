line-height 的部分类别属性值是相对于 font-size 计算的，vertical-align 百分比值属性值又是相对于 line-height 计算的，于是，看上去八辈子都搭不上边的 vertical- 
align 和 font-size 属性背后其实也有有着关联的。 

ex 是字符 x 高度

em 在传统排版中指一个字模的高度
（可以脑补下活字印刷的字模），注意是字模的高度，不是字符的高度。
em 完全取决于 M 的字形，毕竟英文
26 个字母方方正正的不算多。如果按照这种说法，那方方正正的汉字岂不是每一个字都正好一
个 em？没错，确实是这样，尤其作为印刷体的宋体效果最为明显，这种表现在 CSS 中也有非
常明显的体现。


注意：
```
h1 { 
 font-size: 2em; 
-webkit-margin-before: 0.67em;
-webkit-margin-after: 0.67em;
} 
```
假设页面没有任何 CSS 重置，根元素 font-size 就是默认的 16px，
 margin-before 的像素计算值是16*2*0.67 px

CSS 世界的渲染是一次渲染，是不会有死循环的。这里是先计算
font-size，然后再计算给其他使用 em 单位的属性值大小。 
总结如下：在 CSS 中，1em 的计算值等同于当前元素所在的 font-size 计算值，可以将
其想象成当前元素中（如果有）汉字的高度。

理论上，有一个布局，希
望小屏时整体缩小，大屏时再弹性扩大，此时就可以让所有元素宽高尺寸等都使用 em，于是，
最后只要改变布局祖先元素的 font-size 大小就可以实现整体的弹性变化。 
这种策略很棒，也确实可行，但是有一个比较麻烦的事情，它和上面`<h1>`元素计算一样的
麻烦，em 是根据当前 font-size 大小计算的，一旦布局中出现标题这种跟基础 font-size
大小不一样的场景的时候，标题里面所有元素 em 都要重新计算一遍，甚为麻烦，最终的成品
维护成本就比较高了。

rem应运而生：
在：
```
h1 { 
 font-size: 2em; 
-webkit-margin-before: 0.67rem;
-webkit-margin-after: 0.67rem;
} 
```
那么 2em 的 font-size 计算值会被忽略，直接使用根元素的 16px 进行计算，于是 margin- 
before 计算值是 16 像素×0.67 = 10.72 像素。 
因此，要想实现带有缩放性质的弹性布局，使用 rem 是最佳策略，但 rem 是 CSS3 单位，IE9 以上浏览器才支持，需要注意兼容性，


em 实际上更适用于图文内容展示的场景，对此进行弹性布局。例如，`<h1>`～
`<h6>`以及`<p>`等与文本内容展示的元素的 margin 都是用 em 作为单位，这样，当用户把浏览
器默认字号从“中”设置成“大”或改成“小”的时候，上下间距也能根据字号大小自动调整，
使阅读更舒服。
再举个适用于 em 的场景，如果我们使用 SVG 矢量图标，建议设置 SVG 宽高如下： 
svg { 
 width: 1em; height: 1em; 
} 
这样，无论图标是个大号文字混在一起还是和小号文字混在一起，都能和当前文字大小保持一
致，既省时又省力。




业界常用的这套东西，其实可以优化成下面这样：
@font-face { 
 font-family: ICON; 
 src: url('icon.eot'); 
src: local('☺'),
url('icon.woff2') format("woff2"), 
url('icon.woff') format("woff"), 
url('icon.ttf'); 
} 



因为有些字体可能会有专门的斜体字体，注意这个斜体字体并不
是让文字的形状倾斜，而是专门设计的倾斜的字体，所以很多细节会跟物理上的请求不一样。
于是，我在 CSS 代码中使用 font-style:italic 的时候，就会调用这个对应字体，如下
示意：
@font-face { 
 font-family: 'I'; 
 font-style: normal; 
 src: local('FZYaoti'); 
} 
@font-face { 
 font-family: 'I'; 
 font-style: italic; 
 src: local('FZShuTi'); 
} 


font-weight 和 font-style 类似，只不过它定义了不同字重、使用不同字体。



