---
title: 1 CSS 基础、文本、显示模式、背景
date: 2021-7-16 00:00:00
tags: css
categories: 前端
---
## CSS 基础、文本、显示模式、背景

### 1. 语法规范

<!-- more -->

选择器+声明

### 2. 基础选择器

#### 标签选择器

html标签

#### 类选择器

- class属性中可以写多个类名，用空格隔开

- 标签可以调用公共的类，再调用自己独有的类

#### id选择器

````html
<style>
    #id {
        
    }
</style>
````

#### 通配符选择器

```html
<style>
    * {
        
    }
</style>
```

### 3. 字体属性

#### 3.1 字体系列

```html
font-family: 'Microsoft Yahei';
```

- 各字体之间用逗号隔开
- 可以有多个字体，系统按顺序选择

#### 3.2 字体大小

```html
font-size: 20px;
```

#### 3.3 字体粗细

```css
font-weight: 400;
```

#### 3.4 文字样式

```css
font-style: normal/italic;
```

#### 3.5 字体复合属性

```css
font: font-style font-weight font-size/line-height font-family;
```

- 必须保留font-size和font-family 

### 4. 文本属性

#### 4.1 文本颜色color

- 预定义的值
- 十六进制
- RGB代码

#### 4.2 文本对齐text-align

让盒子内的元素水平对齐

```css
text-align: left/center/right;
```

#### 4.3 文本装饰text-decoration

none/underline/overline/line-through

```css
text-decoration: underline;
```

#### 4.4 文本缩进text-indent

```css
text-indent: 10px/2em;
```

#### 4.5 line-height

行间距 = 上间距 + 文本高度 + 下间距

```css
line-height: 26px;
```

### 5. CSS引入方式

#### 5.1 行内样式表

写在行内

#### 5.2 内部样式表

style理论上可以放在HTML的任何地方，但一般会放在head标签中

#### 5.3 外部样式表

样式单独写到CSS文件中，把CSS文件引入到HTML页面中使用

通过\<link>标签引入文件

```html
<link rel="stylesheet" href="css文件路径">
```

### 6. Emmet语法

#### 6.1 快速生成html标签

- div + tab

- 生成多个相同标签：div * 3

- 父子关系： ul > li * 3

- 兄弟关系：div + p
- 带有类名或id： div.demo、p#two

- 生成带有顺序的类名：div.demo$*5

- 在生成的标签内部写内容可以用{}表示

#### 6.2 快速生成css样式

```css
tac = text-align: center;
ti2em = text-indent: 2em;
w100 = width: 100px;
```

### 7. CSS复合选择器

复合选择器是基础选择器的组合，常用的复合选择器包括：后代选择器、子选择器、并集选择器、伪类选择器等

#### 7.1 后代选择器

选择父元素里面子元素

```css
ol li { }
```

#### 7.2 子选择器

只能选择最近的一级子元素

```css
ol>li
```

#### 7.3 并集选择器

选择多组标签，同时为它们定义相同的样式，任何形式的选择器都可以作为并集选择器的一部分

```css
html,
body {
    
}
```

#### 7.4 伪类选择器

伪类选择器用于向某些选择器添加特殊的效果，比如给链接添加特殊效果，或选择第一个，第n个元素。

伪类选择器用 **:** 表示

##### 7.4.1 链接伪类选择器

- a:link		  没有点击过的(访问过的)链接
- a:visited	 点击过的(访问过的)链接
- a:hover	  鼠标经过的那个链接
- a:active	  鼠标正在按下还没有弹起鼠标的那个链接

```css
a:link {
    color: #333;
    text-decoration: none;
}
a:visited {
    color: orange;
}
a:hover {
    color: skyblue;
}
a:active {
    color: green;
}
```

- 需要按照l、v、h、a的顺序来写

- a链接在浏览器中具有默认样式，所以需要单独指定样式

##### 7.4.2 focus伪类选择器

用于选取获得焦点的表单元素

```css
input:focus {
    background-color: yellow;
}
```

#### 7.5 总结

![1570868930472.png](https://i.loli.net/2021/07/21/buyrw5WPsUQ1TMj.png)

#### 8. 元素显示模式

#### 8.1  块元素

div、p……

- 可以设置w、h、margin

**注意**

- 文字类的元素不能使用块级元素（p中不能放div）

#### 8.2 行内元素

a、span……

- w、h设置无效
- 默认宽度就是本身内容宽度
- 行内元素只能容纳文本或其他行内元素

**注意**

- 链接里面不能再放链接

#### 8.3 行内块元素

一行可以放多个，且可以设置w、h（input）

#### 8.4 元素显示模式转换

一个模式的元素需要另外一种模式的特性（增加\<a>的触发范围）

**行内转块**

```css
width: 150px;
height: 50px;
display: block;
```

**块转行**

```css
display: inline;
```

**转换为行内块**

```css
display: inline-block;
```

### 9. 背景

 #### 9.1 背景颜色

```css
background-color: pink;
```

#### 9.2 背景图片

```css
background-image: url();
```

#### 9.3 背景平铺

```css
background-repeat:repeat | norepeat | repeat-x;
```

#### 9.4 背景图片位置

```css
background-position: x y;
```

![1570887034135.png](https://i.loli.net/2021/07/21/Gqr4JpAeHalkWiw.png)

- 如果指定的两个值都是方位名词，则前后顺序无关
- 如果只指定了一个方位名词，则另一个默认center

**精确单位**

- 第一个肯定是x，第二个肯定是y

#### 9.5 背景固定（背景附着）

```css
background-attachment: scroll | fixed
```

![1570887699177.png](https://i.loli.net/2021/07/21/2fyuIFax9GXYv7j.png)

#### 9.6 背景复合写法

背景颜色、背景图片地址、背景平铺、背景图像滚动、背景图片位置

- 没有顺序要求

```css
background: 背景颜色 背景图片地址 背景平铺 背景图像滚动 背景图片位置;
```

#### 9.7 背景色半透明

```css
background: rgba(0, 0, 0, 0.3);
```

#### 9.8 背景总结

![1570888283511.png](https://i.loli.net/2021/07/21/o7yYjeVBuMxag4J.png)
