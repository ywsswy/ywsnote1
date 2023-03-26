```               top
                   ^
                   |
      ----------------------------
      |          margin          |
      |  ||||||||border||||||||  |
      |  |||     padding    |||  |
      |  |||  ------------  |||  |
      |  |||  |       ^  |  |||  |
      |  |||  |       |  |  |||  |
      |  |||  |<-width+->|  |||  |
      |  |||  |       |  |  |||  |
left<-|  |||  |CONTENT|  |  |||  |->right
      |  |||  |       |  |  |||  |
      |  |||  |   height |  |||  |
      |  |||  |       |  |  |||  |
      |  |||  |       ^  |  |||  |
      |  |||  ------------  |||  |
      |  |||                |||  |
      |  ||||||||||||||||||||||  |
      |                          |
      ----------------------------
                   |
                   ^
                  bottom


margin是指从[自身边框]到[另一个容器边框]之间的距离，就是容器外边距，显然两个相邻的容器margin可以重合。
padding是指自身边框到自身内部另一个容器边框之间的距离，就是容器内距离。（内边距）
border设置1px，但是实际可能不是1px（0.909）？border的正确写法是:solid 1px;
```

# display布局:
- none 不显示
- block 块级，（独占一行，可以设置width/height属性）： div p h1 table ul
- inline 行内，（不独占一行，width/height设置是无效的，顶部和底部的margin会被忽略，顶部和底部的padding不会推动上下元素，横向叠积木）： em q span b i br img
- inline-block，在block属性基础上，允许一行放多个
- flex
- grid

## position
- inhert 同父元素
- static（默认）忽略定位属性
- relative（在static的基础上加了定位属性）（内部实际占位是static，但是前端显示是偏移了的）
-- block的只能设置top/bottom
-- inline的只能设置left/right
- fixed 不随其他内容滚动（以可视窗口为参照）
- absolute 元素框从文档流删除（不占文档流/好像不存在）（一般不用这个）

## float 布局在父元素框内游移，脱离文档流，父元素不会管这个元素的高度了，不会被原兄弟识别到
- left
- right
- none

## flex （推荐flex而不是float也不是inline-block，前者无法撑起父元素，后者两个子元素之间的空格会有宽度以至于将本该放到一行的块被挤到下一行-比如10%+90%却换行了，但是flex的子元素之间就可以做到没有缝隙）
见 https://www.bilibili.com/video/BV1tt4y1K7Xc/?spm_id_from=333.1007.top_right_bar_window_history.content.click
- flex-direction: 默认是row（横向排列）、column、
- flex-wrap: 默认是nowrap（排列放不下的时候，等比缩放）、wrap（排列放不下的时候，换行新一列）

## grid