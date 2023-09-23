---
title: 2 CSS 特性、盒子模型、浮动
date: 2021-7-17 00:00:00
tags: css
categories: 前端
---

## CSS 特性、盒子模型、浮动

### 1. CSS 特性

<!-- more -->

CSS三大特性：层叠性、继承性、优先级

#### 1.1 层叠性

**层叠性原则**

- 样式冲突，遵循<font color="red">就近原则</font>，哪个样式离结构近，就执行哪个样式
- 样式不冲突，不会层叠

#### 1.2 继承性

 子标签会继承父标签的某些样式，如文本颜色和字号。

**行高的继承性**

行高为当前子元素大小的1.5倍

```css
font: 12px/1.5 'Microsoft YaHei';
```

#### 1.3 优先级

- 选择器相同，则执行层叠性
- 选择器不同，则根据<font color="red">选择器权重执行</font>

![1571490129794.png](https://i.loli.net/2021/07/21/t6ZYDoxUKdMFeqN.png)

**继承的权重是0**，不管父元素权重多高，子元素的权重都是0

##### 1.3.1 复合选择器的优先级

如果是复合选择器，则会有权重叠加，需要计算权重。

- div ul li → 0, 0, 3
- .nav ul li → 0, 1, 2
- a:hover → 0, 1, 1
- .nav a → 0, 1, 1

### 2. 盒子模型

边框border、外边距margin、内边距padding、实际内容content

<img src="https://i.loli.net/2021/07/21/41uKBLPczAXhQqi.png" alt="1571492529986.png" style="zoom:80%;" />

#### 2.1 border

边框由三部分组成：边框宽度、边框样式、边框颜色

```css
border: border-width || border-style || border-color
```

- 无顺序
- 边框宽度会影响盒子宽度

##### 2.1.1 border-collapse

合并边框

```css
border-collapse: collapse;
```

#### 2.2 内边距

```css
padding: 5px;
```

![1571493298248.png](https://i.loli.net/2021/07/21/h23AvciRkGUtSlp.png)

- padding会影响盒子大小

- 用width和height减去padding的值

- 不用width、height的话不会撑大盒子

#### 2.3 外边距

```css
margin: 0 auto;
```

##### 2.3.1 块级盒子水平居中

- 盒子必须指定宽度
- 左右外边距设为auto

##### 2.3.2 行内元素水平居中

- 给父元素添加text-align: center

##### 2.3.3 垂直外边距合并

**相邻块元素垂直外边距的合并**

![1571494239103.png](https://i.loli.net/2021/07/21/UYDV2jCgIKhpsNb.png)

**嵌套块元素垂直外边距的塌陷**

![1571494373778.png](https://i.loli.net/2021/07/21/n4Mu8cLg61A7ZxE.png)

- 可以为父元素定义上边框
- 可以为父元素定义上内边距
- 可以为父元素添加overflow:hidden

**清除内外边距**

```css
* {
    margin: 0;
    padding: 0;
}
```

- 行内元素尽量只设置左右的内外边距

#### 2.4 圆角边框

```css
border-radius: 10px;
```

#### 2.5 盒子阴影

```css
box-shadow: h-shadow v-shadow blur spread color inset;
```

![1571541874805.png](https://i.loli.net/2021/07/21/1IkehlaAvNYcR3V.png)

- 默认为outset，不用写

- 阴影不占用空间

#### 2.6 文字阴影

```cs
text-shadow: h-shadow v-shadow blur color;
```

![1571541954222.png](https://i.loli.net/2021/07/21/8vVUThxKawdGioW.png)

### 3. 浮动

**float**属性用于创建浮动框，将其移动到一边，直到左边缘或右边缘触及包含块或另一个浮动框的边缘。

多个块级元素纵向排列找标准流，多个块级元素横向排列找浮动。

```css
float: none || left || right;
```

#### 3.1 浮动特性

1. 脱离标准流的控制移动到指定位置（**脱标**）

2. 浮动的盒子**不再保留原先的位置**

3. 浮动元素具有行内块元素特性

   任何元素都可以浮动

   - 如果块级盒子没有设置宽度，默认宽度和父级一样宽，但是添加浮动后，它的大小根据内容来决定
   - 浮动的盒子中间是没有缝隙的，是紧挨在一起的
   - 行内元素同理

#### 3.2 浮动元素经常和标准流父级搭配使用

先用标准流的父元素排列上下位置，之后内部子元素采取浮动排列左右位置，符合网页布局第一准则

```html
<div class="box">
    <div class="left"></div>
    <div class="right"></div>
</div>
```

### 4. 常见网页布局

#### 4.1 常见网页布局

<img src="https://i.loli.net/2021/07/21/QHBEfoTRlMA158d.png" alt="1571555421784.png" style="zoom:50%;" />  				<img src="https://i.loli.net/2021/07/21/5fPXmqvE8VHrRT2.png" alt="1571555492364.png" style="zoom:50%;" />



<img src="https://i.loli.net/2021/07/21/HULhF9J4SqzoVQP.png" alt="image-20210713172158796.png" style="zoom:80%;" />

#### 4.2 浮动布局注意点

1. 先用标准流的父元素排列上下位置，之后内部子元素采取浮动排列左右位置
2. 浮动的盒子只会影响浮动盒子后面的标准流，不会影响前面的标准流

#### 4.3 清除浮动

![1571555883628.png](https://i.loli.net/2021/07/21/4wPetuXLW9gSpO7.png)

- 清除浮动的本质是清除浮动元素造成的影响
- 如果父盒子本身有高度，则不需要清除浮动
- 清除浮动之后，父级就会根据浮动的子盒子自动检测高度。父级有了高度，就不会影响下面的标准流了

```css
clear: left || right || both;
```

**清除浮动的策略**

闭合浮动：只让浮动在父盒子内部影响，不影响父盒子外面的其他盒子

##### 4.3.1 额外标签法（隔墙法）

在浮动元素末尾添加一个空的标签，必须是块级元素

```html
<style>
    .clear {
        clear: both;
    }
</style>

<body>
    <div class="clear"></div>
</body>
```

##### 4.3.2 父级添加overflow属性

```css
.box {
	overflow: hidden;
}
```

优点：代码简洁

缺点：无法显示溢出的部分

##### 4.3.3 父级添加after伪元素

```css
.clearfix::after {
    content: "";
    display: block;
    height: 0;
    clear: both;
    visibility: hidden;
}

.clearfix {			/* IE6、7专有 */
    *zoom: 1;
}
```

优点：没有增加标签，结构更简单

##### 4.3.4 父级添加双伪元素

```css
.clearfix::bofore, 
.clearfix::after {
    content: "";
    display: table;
}

.clearfix::after {
    clear: both; 
}

.clearfix {			/* IE6、7专有 */
    *zoom: 1;
}
```

优点：代码更简洁

#### 4.4 清除浮动总结

**为什么需要清除浮动**

① 父级没高度

② 子盒子浮动了

③ 影响下面布局了

### 5. CSS属性书写顺序

1. 布局定位属性：display / position / float / clear / visibility / overflow
2. 自身属性：width / height / margin / padding / border / background
3. 文本属性：color / font / text-decoration / text-align / vertical-align / white- space / break-word
4. 其他属性（CSS3）：content / cursor / border-radius / box-shadow / text-shadow / background:linear-gradient …

#### 5.2 页面布局分析

为了提高网页制作的效率，布局时通常有以下的布局流程：

1. 必须确定页面的版心（可视区），我们测量可得知。

2. 分析页面中的行模块，以及每个行模块中的列模块。其实页面布局，就是一行行罗列而成的。

3. 制作 HTML 结构。我们还是遵循，先有结构，后有样式的原则。结构永远最重要。

4. 开始运用盒子模型的原理，通过 DIV+CSS 布局来控制网页的各个模块。

#### 5.3 注意事项

##### 5.3.1 导航栏

实际开发中，我们不会直接用链接a而是用li包含链接a的做法

- li + a语义更清晰
- 如果直接用a，搜索引擎容易辨别为有堆砌关键字嫌疑，从而影响网站排名

**注意**

1.让导航栏一行显示, 给 li 加浮动, 因为 li 是块级元素, 需要一行显示.

2.这个nav导航栏可以不给宽度,将来可以继续添加其余文字

3.因为导航栏里面文字不一样多,所以最好给链接 a 左右padding 撑开盒子,而不是指定宽度

##### 5.3.2 浮动

浮动的盒子不会有外边距合并

