---
title: css学习四
date: 2022-04-09 10:00:37
tags:
  - css
categories: [前端]
---

继续

### outline

设置轮廓属性，不占用空间

- `outline-color`：轮廓颜色
- `out-style`：样式
  - `none`：默认，不设置样式
  - `dotted`：点状
  - `dashed`：虚线
  - `solid`：实线
  - `double`：双实线
  - `groove`：3D凹槽
  - `ridge`：3D垄状
  - `inset`：陷入
  - `ouset`：突出
  - `inherit`：继承父元素
- `out-width`：轮廓宽带

### display visibility

`display`设置一个元素如何显示，设置`none`时，元素会被隐藏且不占用空间，但是不会被继承
`visibility`知道一个元素可见还是隐藏，设置为`hidden`时，元素会被隐藏但空间仍被占用，可以被继承

#### display属性

- `block`：会独占一行，多个元素会另起一行，可以设置`width`、`height`、`margin`、`padding`属性
- `inline`：不会独占一行，设置`width`、`height`属性无效，可以设置水平方向的`margin`和`padding`属性，不能设置垂直方向的`padding`和`margin`
- `inline-block`：将对象设置为`inline`对象，但对象的内容作为`block`对象呈现，之后的内联对象会被排列在同一行内。

#### 行内元素和块级元素

- 行内元素
  - 设置宽度无效
  - 可以设置水平方向的`margin`和`padding`属性，但不能设置垂直方向的`padding`和`margin`属性
  - 不会自动换行
- 块级元素
  - 可以设置宽度
  - 设置`margin`和`padding`都可以
  - 可以自动换行
  - 多个块级元素，默认排列从上到下

### position

`absolute`：生成绝对定位的元素，相对于`static`定位以外的一个父元素进行定位。
元素的位置通过`left、top、right、bottom`属性进行规定。
浏览器会递归查找该元素的所有父元素，如果找到了设置`position：relative/absolute/fixed`的元素，就以该元素为基准定位，如果没找到，就以浏览器的边界进行定位

`relative`：生成相对定位的元素，相对于其原来的位置进行定位。元素的位置通过`left、top、right、bottom`属性进行规定。`relative`永远都相对于自身位置进行定位，与其他元素无关，也不会影响其他元素。

`fiexd`：生成绝对定位的元素，相对于屏幕视口（viewport）进行定位，元素的位置在屏幕滚动时并不会改变，回到顶部的按钮一般用这种方式进行定位

`static`：默认值，没有定位，会忽略`top、bottom、left、rioght`或者`z-index`属性。块级元素从上往下排布，行级元素从左到右排列

#### absolute和fixed比较

共同点：

- 改变行内元素的呈现方式，将`display`置为`inline-block`
- 使元素脱离普通文档流，不再占用文档物理空间
- 覆盖非定位文档元素
不同点：

- `absolute`和`fixed`根元素不同，`absolute`的根元素可以设置，`fixed`的根元素是浏览器
- 在有滚动条的页面中，a`bsolute`会跟着父元素移动，`fiexd`固定在页面的具体位置不变

### display float position 关系

- 首先判断`display`属性是否为`none`，如果为`none`，则`position`和`float`属性的值不影响元素最后的表现
- 然后判断`position`的值是否为`absolute`或者fixed，如果是，则`float`属性失效，并且`displa`y的值应该被设置为`table`或者`block`
- 如果`position`的值补位`absolute`或者`fixed`，则判断`float`属性的值是否为`none`，如果不是，则`display`的值则按上面的规则转换。注意，如果`position`的值为`relative`并且`float`属性的值存在，则`relative`相对于浮动后的最终位置定位
- 如果`float`的值为`none`，则判断元素是否为根元素，如果是根元素则`display`属性按照上面的规则转换，如果不是，则保持指定的`display`属性值不变

总的来说，可以把它看作是类似优先级的机制，`position:absolute`和`position:fixed`优先级最高，有它存在对的时候，浮动不起作用，`display`的值也需要调整。其次，元素的`float`特性的值不是`none`的时候或者它是根元素的时候，调整`display`的值。最后，非根元素，并且非浮动元素，并且非绝对定位的元素，`display`特性值同设置值

### 一与人同，未免屈意以循之😜😜😜

**<font size=4>临江仙</font>** <font size=1>送钱穆父</font> <font size=2>苏轼</font>

**一别都门三改火，**

*天涯踏尽红尘。*

**依然一笑作春温，**

*无波真古井，*

**有节是秋筠。**

*惆怅孤帆连夜发，*

**送行淡月微云。**

*尊前不用翠眉颦。*

**人生如逆旅，**

*我亦是行人。*
