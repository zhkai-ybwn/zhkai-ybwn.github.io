---
title: css学习一
date: 2022-04-03 09:55:57
tags:
  - css
categories: [前端]
---

## CSS 学习一

从涉及前端开发到现在，差不多有四年了，框架有`Vue`、`Angular`，组件库用了`ElementUI`、`NG-Zorro`、`Ant-Design`。
而对于`CSS`，可谓东一榔头，西一棒子，用时查找，不用忘掉。这次决定花点时间，系统的学习一下`CSS`，毕竟上一次学，还是大二的时候。（我逝去的青春啊！！😭😭）

### 定义

层叠样式表，用于设计`HTML`的风格和布局，字体、颜色、大小、间距等等。

### 语法

`CSS`由两部分组成：选择器，以及声明。
一个熟悉与值的键值被称为声明，而将一个或者多个声明用`{}`括起来，就是声明块。
声明作用到对应的`HTML`元素，就要加上选择器。

### 选择器

#### 基础选择器

- 标签选择器：`h1`
- 类选择器：`.checked`
- ID 选择器：`#picker`
- 通配选择器：`*`

#### 属性选择器

- [attr]：指定属性的元素
- [attr=val]：属性等于指定值的元素
- [attr*=val]：属性包含指定值的元素
- [attr^=val]：属性以指定值开头的元素
- [attr$=val]：属性以指定值结尾的元素
- [attribute~=val]：属性包含指定值（完整单词）的元素（不推荐使用）
- [attr|val]：属性以指定（完整单词）开头的元素（不推荐使用）

#### 组合选择器

- 相邻兄弟选择器：`A+B`
- 普通兄弟选择器：`A~B`
- 子选择器：`A>B`
- 后代选择器：`AB`

#### 伪类

##### 条件伪类

- `:lang()`：基于元素语言来匹配页面元素
- `:dir()`：匹配特定文字书写方向的元素
- `:has()`：匹配包含指定元素的元素
- `:is()`：匹配指定选择器列表里的元素
- `:not()`：用来匹配不符合一组选择器的元素

##### 行为伪类

-`:active`：鼠标激活的元素 -`:hover`：鼠标悬浮的元素 -`::selection`：鼠标选中的元素

##### 状态伪类

- `:target`：当前锚点的元素
- `:link`：未访问的链接元素
- `:visited`：已访问的链接元素
- `:focus`：输入聚焦的表单元素
- `:required`：输入必填的表单元素
- `:valid`：输入合法的表单元素
- `：invalid`：输入非法的表单元素
- `:in-range`：输入范围以内的表单元素
- `out-of-range`：输入范围以外的表单元素
- `checked`：选项选中的表单元素
- `optional`：选项可选的表单元素
- `enabled`：事件启用的表单元素
- `disabled`：事件禁用的表单元素
- `read-only`：只读的表单元素
- `read-write`：可读可写的表单元素
- `blank`：输入为空的表单元素
- `current()`：浏览中的元素
- `past()`：已浏览的元素
- `future`：未浏览的元素

##### 结构伪类

- `：root`：文档的根元素
- `:empty`： 无子元素的元素
- `:first-letter`：元素的首字母
- `:first-line`：元素的首行
- `:nth-child(n)`：元素中指定书序索引的元素
- `:nth-last-child(n)`：元素中指定逆序索引的元素
- `:first-child`：元素中为首的元素
- `:last-child`：元素中为尾的元素
- `:only-child`：父元素仅有该元素的元素
- `:nth-of-type(n)`：标签中指定顺序索引的标签
- `:nth-last-of-type(n)`：标签中指定逆序索引的标签
- `:first-of-type`：标签中为首的标签
- `:last-of-type`：标签中为尾标签
- `:only-of-type`：父元素仅有该标签的标签

##### 伪元素

- `::before`：在元素前插入内容
- `::after`：在元素后插入内容

#### 优先级

- !important
- 内联样式
- ID 选择器
- 类选择、伪类选择器、属性选择器
- 元素选择器、伪元素选择器
- 通配选择器、后代选择器、兄弟选择器

### CSS Backgrounds

- `background`：将背景属性设置在一个声明中

- `background-attachment`：背景图像是否固定或者随着页面的其余部分滚动
  - `scroll`：随着页面的滚动而滚动（默认值）
  - `fixed`：不会随着页面的滚动而滚动
  - `local`：随着元素的内容的滚动而滚动
  - `initial`：使用默认值
  - `inherit`：继承父元素的属性

- `background-color`：设置元素的背景颜色
  - `color`：背景颜色
  - `transparent`：背景颜色透明
  - `inherit`：继承父元素

- `background-image`：把图像设置为背景
  - `linear-gradient()`：创建一个线性渐变的"图像"（从上而下）
  - `radial-gradient()`：用径向渐变创建"图像"
  - `repeating-linear-gradient()`：创建重复的线性渐变"图像"
  - `repeating-radial-gradient()`：创建重复的径向渐变"图像"
  - `inherit`：继承父元素

- `background-position`：设置起始位置
  - `left/top/centet/bottom/right`：仅指定一个时，其他值会是center
  - `x% y%`：水平位置% 垂直位置%，左上角是0%0%，右下角是100%100%，仅指定一个值，其他将是50%
  - `xpos ypos`：水平位置 垂直位置，左上角是0，仅指定一个值，其他将是50%
  - `inherit`：继承父元素
  
- `background-repeat`：设置是否及如何重复
  - `repeat`：垂直和水平方向重复（默认值）
  - `repeat-x`：水平重复
  - `repeat-y`：垂直重复
  - `no-repeat`：不重复
  - `inherit`：继承父元素的属性

## 喟然叹息😒😒

**<font size=4>凤栖梧** </font><font size=2>柳永</font>

**伫倚危楼风细细，**

*望极春愁，*

**黯黯生天际。**

*草色烟光残照里，*

**无言谁会凭栏意。**

*拟把疏狂图一醉，*

**对酒当歌，**

*强乐还无味。*

**衣带渐宽终不悔，**

*为伊消得人憔悴。*
