min-width/min-height 的初始值是 auto，
max-width/max- height的初始值是 none。
max-width/max-height的初始值为何是 none而不是 auto呢？
我们不妨举个简单的例子解释一下，已知父元素宽
度 400 像素，子元素设置宽度 800 像素，假如说 max-width 初始值是 auto，那自然使用和 width
一样的解析渲染规则，此时 max-width 的计算值就应该是父元素的 400 像素，此时，你就会
发现，子元素的 800 像素直接完蛋了，因为 max-width 会覆盖 width。
（因为width800，max-width400,这是有问题的。）
导致我们的 width永远不能设置为比 auto 计算值更大的宽度值了，这显然是不合理的，这就是为什么 max-width
初始值是 none 的原因。 

max-width 会覆盖 width，而且这种覆盖不是普通的覆盖，是超级
覆盖，覆盖到什么程度呢？大家应该都知道 CSS 世界中的!important 的权重相当高，在业界，往
往会把!important 的权重比成“泰坦尼克”，比直接在元素的 style 属性中设置 CSS 声明还要高，
一般用在 CSS 覆盖 JavaScript 设置上。但是，就是这么厉害的!important，直接被 max-width 一
个浪头就拍沉了。比方说，针对下面的 HTML 和 CSS 设置，图片最后呈现的宽度是多少呢？ 
```
<img src="1.jpg" style="width:480px!important;"> 
img { max-width: 256px; } 
```
答案是 256px。style、!important 通通靠边站！因为 max-width 会覆盖 width。

min-width覆盖 max-width，此规则发生在 min-width和 max-width
冲突的时候。

应用：
任意高度元素的展开收起动画技术 
“展开收起”效果是网页中比较常见的一种交互形式，通常的做法是控制 display 属性值
在 none 和其他值之间切换，虽说功能可以实现，但是效果略显生硬，所以就会有这样的需求：

希望元素展开收起时能有明显的高度滑动效果。传统实现可以使用 jQuery 的 slideUp()/ 
slideDown()方法，但是，在移动端，因为 CSS3 动画支持良好，所以移动端的 JavaScript 框
架都是没有动画模块的。
此时，使用 CSS 实现动画就成了最佳的技术选型。 
我们展开的元素内容是动态的，换句话说高度是不固定的，因此，height 使用的值是默认的 auto，
应该都知道的 auto 是个关键字值，并非数值，正如 height:100%的 100%无法和 auto 相计
算一样，从 0px 到 auto 也是无法计算的，因此无法形成过渡或动画效果。 
因此，下面代码呈现的效果也是生硬地展开和收起：

```
.element { 
 height: 0; 
 overflow: hidden; 
 transition: height .25s; 
} 
.element.active { 
 height: auto; /* 没有 transition 效果，只是生硬地展开 */ 
} 
```
难道就没有什么一劳永逸的实现方法吗？有，不妨试试 max-height，CSS 代码如下： 
```
.element { 
 max-height: 0; 
 overflow: hidden; 
 transition: max-height .25s; 
} 
.element.active { 
 max-height: 666px; /* 一个足够大的最大高度值 */ 
} 
```
其中展开后的 max-height 值，我们只需要设定为保证比展开内容高度大的值就可以，因为
max-height 值比 height 计算值大的时候，元素的高度就是 height 属性的
计算高度，在本交互中，也就是 height:auto 时候的高度值。于是，一个高度
不定的任意元素的展开动画效果就实现了。 
 http://demo.cssworld.cn/3/3-2.php 
但是，使用此方法也有一点要注意，即虽然说从适用范围讲，max- height 值越大使用
场景越多，但是，如果 max-height 值太大，在收起的时候可能会有“效果延迟”的问
题，比方说，我们展开的元素高度是 100 像素，而 max-height 是 1000 像素，动画时间
是 250 ms，假设我们动画函数是线性的，则前 225 ms 我们是看不到收起效果的，因为
max-height 从 1000 像素到 100 像素变化这段时间，元素不会有区域被隐藏，会给人动
画延迟 225 ms 的感觉，相信这不是你想看到的。 
因此，我个人建议 max-height 使用足够安全的最小值，这样，收起时即使有延迟，也
会因为时间很短，很难被用户察觉，并不会影响体验。