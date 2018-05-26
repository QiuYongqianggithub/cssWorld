1．border-style:none 
注意，border-style 的默认值是 none，有一部分人可能会误以为是 solid。这也是单
纯设置 border-width 或 border-color 没有边框显示的原因
如下示意： 
```
div { border: 10px; } /* 无边框出现 */ 
div { border: red; } /* 无边框出现 */ 
```
重置边框可以用以下两种方法：或同时使用，性能更好
```
div { 
 border-bottom: 0 none; 
} 
```

border-style:dotted 
虚点边框在表现上同样有兼容性差异，虽然规范上明确表示是个圆点，但是 Chrome 以及
Firefox 浏览器下虚点实际上是个小方点，而 IE 浏览器下则是小圆点,根据这个特性可以实现
ie8下的圆：
```
.dotted { 
 width: 150px; height: 150px; 
 overflow:hidden;
 border: 149px dotted #cd0000; 
} 
```


border-color 和 color 
border-color 有一个很重要也很实用的特性，就是“border-color 默认颜色就是
color 色值”。具体来讲，就是当没有指定 border-color 颜色值的时候，会使用当前元素的
color 计算值作为边框色。例如，下面这个例子： 
.box { 
 border: 10px solid; 
 color: red; 
} 
此时，.box 元素的 10px 边框颜色就是红色。 
具有类似特性的 CSS 属性还有 outline、box-shadow 和 text-shadow 等。 





















