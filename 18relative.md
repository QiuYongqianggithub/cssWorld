相对定位元素的 left/top/right/bottom
的百分比值是相对于包含块计算的，而不是自身。注意，虽然定位位移是相对自身，但是百分
比值的计算值不是。

top 和 bottom 这两个垂直方向的百分比值计算跟 height 的百分比值是一样的，都
是相对高度计算的。同时，如果包含块的高度是 auto，那么计算值是 0，偏移无效，也就
是说，如果父元素没有设定高度或者不是“格式化高度”，那么 relative 类似 top:20%
的代码等同于 top:0。 


当相对定位元素同时应用对立方向定位值的时候，也就是 top/bottom 和 left/right
同时使用的时候，其表现和绝对定位差异很大。绝对定位是尺寸拉伸，保持流体特性，但是相
对定位却是“你死我活”的表现，也就是说，只有一个方向的定位属性会起作用。而孰强孰弱
则是与文档流的顺序有关的，默认的文档流是自上而下、从左往右，因此 top/bottom 同时使
用的时候，bottom 被干掉；left/right 同时使用的时候，right 毙命。
``` 
.example { 
 position: relative; 
 top: 10px; 
 right: 10px; /* 无效 */ 
 bottom: 10px; /* 无效 */ 
 left: 10px; 
} 
```


对于：
```
<div style="position:relative;"> 
 <img src="icon.png" style= 
 "position:absolute;top:0;right:0;"> 
<p>内容 1</p>
<p>内容 2</p>
<p>内容 3</p>
<p>内容 4</p>
...
</div> 
```
如果采用“relative 的最小化影响原则”则应该是如下面这般实现： 
```
<div> 
 <div style="position:relative;"> 
 <img src="icon.png" style="position:absolute;top:0;right:0;"> 
 </div> 
<p>内容 1</p>
<p>内容 2</p>
<p>内容 3</p>
<p>内容 4</p>
...
</div> 
```
差别在于，此时 relative 影响的元素只是我们的图标，后面的“内容 1”之类的元素依
然保持开始时干净的状态。































