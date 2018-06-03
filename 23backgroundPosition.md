当background-position以百分比作为值时，有特殊的计算规则：
positionX = (容器的宽度 - 图片的宽度) * percentX; 
positionY = (容器的高度 - 图片的高度) * percentY;
而且：
在background-position：100% 100%;中是left top的对应值。
另外，根据计算规则：
当容器尺寸小于图片尺寸时，计算结果为负值，
当容器尺寸小于图片尺寸时，同时percentX也为负值，则计算结果为正值。