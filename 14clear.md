设置了 clear 属性的元素是自身如何
如何，而不是让 float 元素如何如何，有种“己所不欲勿施于人”的意味在里面。因此，我对
clear 属性值的理解是下面这样的。 
• none：默认值，左右浮动来就来。
• left：左侧抗浮动。不和我前面（也可以理解成html中的哥哥）的浮动元素（左浮动）在同一行。
• right：右侧抗浮动。不和我前面的浮动元素（右浮动）在同一行。(注意也是前面，注意这里“前面”，
也就是 clear 属性对“后面的”浮动元素是不闻不问的)
• both：两侧抗浮动。由于凡是设置浮动的元素，不是设置float:left,就是right,不可能同时设置left,right,
所以使用clear:both;就表示left,right两种情况之一。

综上，clear:both;完全可以取代clear:left或right,换言之，clear:left或right在实际应用中无用武之地。
同时设置float和对应的clear，就等于没有设置float,但这两个属性同时设置不会有css语法错误。


注意：
（1）如果 clear:both 元素前面的元素就是 float 元素，则 margin-top 负值即使设
成-9999px，也不见任何效果。 
（2）clear:both 后面的元素（设置了负margin-top）有多行文字时会发生文字环绕的现象。举个例子，如下 HTML
和 CSS：
```
<!DOCTYPE html>
<html>
    <head>
        <title></title>
       <meta charset="utf-8"/>
       <style>
       .father {
           border: 1px solid #444;
          }
          .father::after {
              content: ''; 
              display: table; 
              clear: both; 
             }
          .father > div {
              width: 60px; height: 64px;
              float: left;
              background: pink;
          }
           .father + div { 
             margin-top: -4px; 
             background: yellow;
            } 
       </style>
    </head>
    <body>
        <div class="father"> 
           <div ></div>
               我是帅哥，好巧啊，我也是帅哥，原来看这本书的人都是帅哥~ 
        </div> 
        <div>虽然你很帅，但是我对你不感兴趣。</div>
    </body>
</html>
```