box-sizing: content-box;
默认情况下指定的是内容区宽度，元素实际宽度是自动计算出来的：
width(内容区宽度) + padding(内边距) + border(边框) = 元素实际宽度
height(内容区高度) + padding(内边距) + border(边框) = 元素实际高度

box-sizing: border-box;
指定的就是元素实际宽度，反而内容区宽度是自动计算出来的：
内容区宽度 + padding(内边距) + border(边框) = width(元素实际宽度)
内容区高度 + padding(内边距) + border(边框) = height(元素实际高度)