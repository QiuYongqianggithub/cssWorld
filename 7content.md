我们把 content 属性生成的对象称为“匿名替换元素”。
content 属性生成的内容都是替换元素？没错，就是替换元素！ 
content 属性生成的内容的特点：
（1 ）我们使用 content 生成的文本是无法选中、无法复制的
content 生成的文本无法被屏幕阅读设备读取，也无法被搜索引擎抓取
（2）不影响:empty 伪类。:empty 是一个 CSS 选择器，当元素里面无内容的时候进行
匹配。
（3）content 动态生成值无法获取。


content 内容生成技术

1．content 辅助元素生成 
此应用的核心点不在于 content 生成的内容，而是伪元素本身。通常，我们会把 content
的属性值设置为空字符串，像这样：
```
.element:before { 
 content: ''; 
} 
```


最常见的应用之一就是清除浮动带来的影响：
```
.clear:after { 
 content: ''; 
 display: table; /* 也可以是'block' */ 
 clear: both; 
} 
```

辅助实现“两端对齐”以及“垂直居中/上边缘/下边缘对齐”效果
http://demo.cssworld.cn/4/1-7.php 


2．content 字符内容生成 
content 字符内容生成就是直接写入字符内容，中英文都可以，比较常见的应用就是配合
@font-face 规则实现图标字体效果。例如，下面这个例子： 
 http://demo.cssworld.cn/4/1-8.php 
```
@font-face { 
 font-family: "myico"; 
 src: url("/fonts/4/myico.eot"); 
 src: url("/fonts/4/myico.eot#iefix") format("embedded-opentype"), 
 url("/fonts/4/myico.ttf") format("truetype"), 
 url("/fonts/4/myico.woff") format("woff"); 
} 
.icon-home:before { 
 font-size: 64px; 
 font-family: myico; 
 content: "家"; 
} 
<span class="icon-home"></span> 
```


```
HTML：
正在加载中<dot>...</dot>


CSS：
dot {
    display: inline-block; 
    height: 1em;
    line-height: 1;
    text-align: left;
    vertical-align: -.25em;
    overflow: hidden;
}
dot::before {
    display: block;
    content: '...\A..\A.';
    white-space: pre-wrap;
    animation: dot 3s infinite step-start both;
}
@keyframes dot {
    33% { transform: translateY(-2em); }
    66% { transform: translateY(-1em); }
}

```


IE6 至 IE9 浏览器下是静态的点点点，支持 animation 动画的浏览器下全
部都是打点 loading 动画效果，颜色大小可控

只是其他一些细节怕是很多人反而有疑问。
（1）为什么使用`<dot>`这个元素？
	`<dot>`是自定义的一个标签元素，除了简约、语义化明显外，更重要的是方便向下兼
容，IE8 等低版本浏览器不认识自定义的 HTML 标签，因此，会乖乖地显示里面默认的 3 个点，
对我们的 CSS 代码完全忽略。
（2）为什么使用::before，可不可以使用::after？
伪元素使用::before 同时 display 设置为 block，是为了在高版本浏览器下原来
的 3 个点推到最下面，不会影响 content 的 3 行内容显示，如果使用::after 怕是效果就很
难实现了。
（3）从 content 属性值来看，是 3 个点在第 1 行，而 1 个点反而在最后一行，为什么这
么处理？
3 个点在第一行的目的在于兼容 IE9 浏览器，因为 IE9 浏览器认识<dot>以
及::before，但是不支持 CSS 新世界的 animation 属性，所以，为了 IE9 也能正常显示静
态的 3 个点，故而把 3 个点放在第一行。 
（4）这里 white-space 值为何使用的是 pre-wrap 而不是 pre？
这 4 个问题的答案分别如下。
这里的 white-space:pre-wrap 改成 white-space:pre 效果其实是一样的。 
还有最后几个小技巧，首先，'\A'是不区分大小写的；其次，'\D'也能实现换行效果，
但是，要想上下行对齐，需要用在::before 伪元素上，因为 CR 是将光标移动到当前行的开
头，而 LF 是将光标“垂直”移动到下一行。 




3．content 图片生成 
content 图片生成指的是直接用 url 功能符显示图片，例如： 
```
div:before { 
 content: url(1.jpg); 
} 
```
url 功能符中的图片地址不仅可以是常见的 png、jpg 格式，还可以是 ico 图片、svg
文件以及 base64URL 地址，但不支持 CSS3 渐变背景图。 
虽然支持的图片格式多种多样，但是实际项目中，content 图片生成用得并不多，主要原
因在于图片的尺寸不好控制，我们设置宽高无法改变图片的固有尺寸。所以，伪元素中的图片
更多的是使用 background-image 模拟，类似这样： 
```
div:before { 
 content: ''; 
 background: url(1.jpg); 
} 
```





























