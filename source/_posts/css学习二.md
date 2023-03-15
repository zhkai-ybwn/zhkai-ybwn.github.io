---
title: css学习二
date: 2022-04-04 08:52:38
tags:
  - css
categories: [前端]
---

## CSS 学习二

继续

### CSS 单位

#### 相对长度

- `em`：当前元素的字体尺寸
- `ex`：英文字母小x的高度
- `ch`：数字0的高度
- `vw`：视窗宽度，1vm=视窗宽度的1%
- `vh`：视窗高度，1vm=视窗高度的1%
- `vmin`：vm和vh中较小的那个
- `vmax`：vm和vh中较大的那个
- `%`：😁😁😁

#### 绝对长度

- `cm`：厘米
- `mm`：毫米
- `in`：英寸
- `px`：像素
- `pt`：1pt = 1/72in
- `pc`：1pc = 12 pt

### CSS Text

- `color`：设置文本颜色
- `direction`：设置文字方向
  - `ltr`：从左向右（默认）
  - `rtl`：从右向左
  - `ingerit`：继承父元素

- `letter-spacing`：设置字符间距
  - `noraml`：字符间没有额外的空间
  - `length`：使用固定空间（可以为负值）
  - `inherit`：继承父元素

- `line-height`：设置行间距
  - `normal`：默认
  - `number`：设置数字，会与字体尺寸相乘得到行间距
  - `length`：固定值
  - `%`：字体尺寸的百分比为行间距
  - `inherit`：继承父元素

- `text-align`：文本的水平对齐方式
  - `left`：左对齐（默认）
  - `right`：右对齐
  - `center`：居中
  - `justify`：两端对齐
  - `inherit`：继承父元素

- `text-decoration`：添加文本修饰
  - `none`：无修饰（默认）
  - `underline`：下划线
  - `overline`：上划线
  - `line-through`：删除线
  - `blink`：闪烁效果
  - `inherit`：继承父元素

- `text-indent`：首行缩进
  - `length`：固定缩进
  - `%`：基于父元素宽度的百分比缩进
  - `inherit`：继承父元素

- `text-shadow`：文本阴影
  - `h-shadow`：必需，允许负值，水平阴影位置
  - `v-shadow`：必需，允许负值，垂直阴影位置
  - `blur`：模糊的距离
  - `color`：阴影的颜色

- `text-transform`：字母样式
  - `none`：默认
  - `capitalize`：每个单词以大写字母开头
  - `uppercase`：全部大写
  - `lowercase`：全部小写
  - `inherit`：继承父元素

- `unicode-bidi`：是否重写文本，需要配合`direction`使用
  - `normal`：默认
  - `embed`：创建一个附加的嵌入层面
  - `bidi-override`：创建一个附加的嵌入层面，按照`direction`属性重新排序
  - `initial`：设置默认值
  - `inherit`：继承父元素
这里`embed`属性和`normal`属性如果单独使用，最终样式看来是没有区别的。
但是`embed`会创建一个附加的嵌入层面，因为在`bidi-override`属性值里面，`normal`是不会起作用的，因为`bidi-override`也创建了一个嵌入层面，这个时候就可以使用`embed`了

- `vertical-align`：文本的垂直对齐方式
  - `baseline`：默认
  - `sub`：垂直对齐文本下标
  - `super`：垂直对齐文本上标
  - `top`：元素顶端与行中最高元素的顶端对齐
  - `text-top`：元素顶端与父元素字体的顶端对齐
  - `middle`：元素放于父元素的中部
  - `bottom`：使元素及其后代元素的底部与整行的底部对齐
  - `text-bottom`：元素底端与父元素字体的底端对齐
  - `length`：使元素的基线对齐到父元素的基线之上的给定长度。可以是负数。
  - `%`：使用 "line-height" 属性的百分比值来排列此元素。允许使用负值。
  - `inherit`：继承父元素

- `white-space`：空白处理方式
  - `noraml`：连续的空白符会被合并，换行符会被当作空白符来处理
  - `nowwrap`：和 normal 一样，连续的空白符会被合并。但文本内的换行无效
  - `pre`：连续的空白符会被保留。在遇到换行符或者`<br>`元素时才会换行
  - `pre-wrap`：连续的空白符会被保留。在遇到换行符或者`<br>`元素时才会换行
  - `pre-line`：连续的空白符会被合并。在遇到换行符或者`<br>`元素时会换行
  - `inherit`：继承父元素

    |    | 换行符 | 空格和制表符 | 文字换行 | 行尾空格 |
    | ---- | ---- | ---- | ---- | ---- |
    | `normal` | 合并 | 合并 | 换行 | 删除 |
    | `nowwrap` | 合并 | 合并 | 不换行 | 删除 |
    | `pre` | 保留 | 保留 | 不换行 | 保留 |
    | `pre-wrap` | 保留 | 保留 | 换行 | 挂起 |
    | `pre-line` | 保留 | 保留 | 换行 | 换行 |

- `word-spacing`：字间距
  - `normal`：默认，使用标准空间
  - `length`：使用指定空间
  - `inherit`：继承父元素

瑞了瑞了！！！

### 有垣曰苑，无垣曰囿😉😉

**<font size=4>临江仙</font>** <font size=1>夜归临皋</font> <font size=2>苏轼</font>

**夜饮东坡醒复醉，**

*归来仿佛三更。*

**家童鼻息已雷鸣。**

*敲门都不应，*

**倚杖听江声。**

*长恨此身非我有，*

**何时忘却营营？**

*夜阑风静彀纹平。*

**小舟从此逝，**

*江海寄余生。*
