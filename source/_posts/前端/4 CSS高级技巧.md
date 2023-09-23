---
title: 4 CSS高级技巧
date: 2021-7-19 00:00:00
tags: css
categories: 前端
---

## CSS高级技巧

### 1. 精灵图

<!-- more -->

有效地减少服务器接收和发送请求的次数，提高页面的加载速度

#### 1.1 精灵图的使用

1. 精灵技术主要针对于背景图片使用。就是把多个小背景图片整合到一张大图片中。 
2. 这个大图片也称为 sprites 精灵图
3. 移动背景图片位置， 此时可以使用 background-position 。 
4. 移动的距离就是这个目标图片的 x 和 y 坐标。注意网页中的坐标有所不同 
5. 因为一般情况下都是往上往左移动，所以数值是负值。 
6. 使用精灵图的时候需要精确测量，每个小背景图片的大小和位置。

 ```css
 .box {
     width: 60px;
     height: 60px;
     margin: 100px auto;
     background: url(./images/sprites.png) no-repeat -182px 0;
 }
 ```

### 2. 字体图标

字体图标使用场景： 主要用于显示网页中通用、常用的一些小图标

字体图标可以为前端工程师提供一种方便高效的图标使用方式，展示的是图标，本质属于字体。

#### 2.1 优点

-  轻量级

- 灵活性

- 兼容性

#### 2.2 字体图标的引入

通过css将字体图标引入（到阿里巴巴矢量图库上复制代码）

```css
@font-face {
  font-family: "iconfont"; /* Project id 2682552 */
  src: url('http://at.alicdn.com/t/font_2682552_i74b4euo3zg.woff2?t=1626590070178') format('woff2'),
       url('http://at.alicdn.com/t/font_2682552_i74b4euo3zg.woff?t=1626590070178') format('woff'),
       url('http://at.alicdn.com/t/font_2682552_i74b4euo3zg.ttf?t=1626590070178') format('truetype');
}

.iconfont {
  font-family: "iconfont" !important;
  font-size: 16px;
  font-style: normal;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.icon-sousuo_huaban1:before {
  content: "\e60d";
}
```

### 3. CSS三角

三边设置透明，一边有颜色

```css
div {
 width: 0;
 height: 0;
 line-height: 0;
 font-size: 0;
 border: 50px solid transparent;
 border-left-color: pink;
}
```

#### 3.1 案例

```css
.jd {
    position: relative;
    width: 120px;
    height: 249px;
    background-color: pink;
}
        
.jd span {
    position: absolute;
    right: 15px;
    top: -10px;
    width: 0;
    height: 0;
    font-size: 0;
    line-height: 0;
    border: 5px solid transparent;
    border-bottom-color: pink;
}
```

### 4. 用户界面样式

#### 4.1 鼠标样式cursor

设置或检索在对象上移动的鼠标指针采用何种系统预定义的光标形状。

```css
li {
    cursor: pointer;
}
```

![1571521805183.png](https://i.loli.net/2021/07/21/LjC4XIKGqAR3QFV.png)

#### 4.2 轮廓线 outline

给表单添加 outline: 0; 或者 outline: none; 样式之后，就可以去掉默认的蓝色边框。

```css
input {
    outline: none;
}
```

#### 4.3  防止拖拽文本域 resize

```css
textarea{ resize: none;}
```

注：文本域尽量放到一行，不然首字会有空隙

### 5. vertical-align

经常用于设置图片或者表单(行内块元素）和文字垂直对齐。

用于设置一个元素的垂直对齐方式，但是它只针对于行内元素或者行内块元素有效。

```css
vertical-align : baseline | top | middle | bottom
```

![1571522023413.png](https://i.loli.net/2021/07/21/KXMGtqLyEDH5VNf.png)

<img src="https://i.loli.net/2021/07/21/OqobtJjlnQGi63A.png" alt="1571522040645.png" style="zoom:80%;" />

#### 5.1 图片和文字垂直居中

```css
img {
    vertical-align: middle;
}
```

#### 5.2 解决图片底部默认空白缝隙问题

图片底侧会有一个空白缝隙，原因是行内块元素会和文字的基线对齐。

**解决方法**

1. 给图片添加 vertical-align: middle | top| bottom（提倡使用的） 
2. 把图片转换为块级元素 display: block;

### 6. 溢出的文字省略号显示

#### 1. 单行文本溢出显示省略号

```cs
/*1. 先强制一行内显示文本*/
 white-space: nowrap; （ 默认 normal 自动换行）
 /*2. 超出的部分隐藏*/
 overflow: hidden;
 /*3. 文字用省略号替代超出的部分*/
 text-overflow: ellipsis;
```

#### 2. 多行文本溢出显示省略号

多行文本溢出显示省略号，有较大兼容性问题， 适合于webkit浏览器或移动端（移动端大部分是webkit内核）

```css
overflow: hidden;
text-overflow: ellipsis;
/* 弹性伸缩盒子模型显示 */
display: -webkit-box;
/* 限制在一个块元素显示的文本的行数 */
-webkit-line-clamp: 2;
/* 设置或检索伸缩盒对象的子元素的排列方式 */
-webkit-box-orient: vertical;
```

### 7. 常见布局技巧

#### 7.1 margin负值运用

1. 让每个盒子margin 往左侧移动 -1px 正好压住相邻盒子边框

```css
li {
    float: left;
    list-style: none;
    width: 100px;
    height: 100px;
    border: 1px solid red;
    margin-left: -1px;
}
```

2. 鼠标经过某个盒子的时候，提高当前盒子的层级即可（如果没有有定位，则加相对定位（保留位置），如 果有定位，则加z-index）

```css
ul li:hover {
    position: relative;
    border: 1px solid blue;
}
```

#### 7.2 文字围绕浮动元素

利用浮动元素不会压住文字的 特性

#### 7.3 行内块巧妙运用

**页码在页面中间显示**

1. 把这些链接盒子转换为行内块， 之后给父级指定 text-align:center; 
2. 利用行内块元素中间有缝隙，并且给父级添加 text-align:center; 行内块元素会水平会居中

#### 7.4 CSS 三角强化

**原理：**

![1571523024087.png](https://i.loli.net/2021/07/21/a4Hl6MDkuA72wVp.png)

**代码：**

```css
.box {
    width: 0;
    height: 0;
    /* 只保留右边的边框有颜色 */
    border-color: transparent red transparent transparent;
    border-style: solid;            
    /* 左边和下边的边框宽度设置为0，上边框调大 */
    border-width: 100px 50px 0 0;
}
```

#### 7.4.1 示例

<img src="https://i.loli.net/2021/07/21/sgLRzUrI3M8mWVc.png" alt="1571548099631.png" style="zoom:80%;" />

```html
<style>
    * {
        margin: 0;
        padding: 0;
    }
        
    .price {
        width: 160px;
        height: 24px;
        line-height: 24px;
        border: 1px solid red;
        margin: 100px auto;
    }

    .price .discount {
        float: left;
        position: relative;
        width: 90px;
        height: 100%;
        background-color: red;
        text-align: center;
        color: #fff;
        font-weight: 700;
        margin-right: 8px;
    }

    .price .discount i {
        position: absolute;
        right: 0;
        top: 0;
        width: 0;
        height: 0;
        border-color: transparent #fff transparent transparent;
        border-style: solid;
        border-width: 24px 12px 0 0;
    }

    .price .origin {
        font-size: 12px;
        color: #ccc;
        text-decoration: line-through;
    }
</style>

<body>
    <div class="price">
        <span class="discount">
            ￥1650 <i></i>
        </span>
        <span class="origin">￥5650</span>
    </div>
</body>
```

### 8. CSS 初始化

不同浏览器对有些标签的默认值是不同的，为了消除不同浏览器对HTML文本呈现的差异，照顾浏览器的兼 容，我们需要对CSS 初始化

**例**

```css
/* 把所有内外边距清零 */
* {
    margin: 0;
    padding: 0
}

/* 去掉li的小圆点 */
li {
    list-style: none
}

img {
    /* 取消图片底侧有空白缝隙的问题 */
    vertical-align: middle
}
```

**Unicode编码字体**

把中文字体的名称用相应的Unicode编码来代替，这样就可以有效的避免浏览器解释CSS代码时候出现乱码的问题。

```css
font-family: ' \9ED1\4F53', '\5B8B\4F53', '\5FAE\8F6F\96C5\9ED1'
```

- 黑体 \9ED1\4F53 
- 宋体 \5B8B\4F53 
- 微软雅黑 \5FAE\8F6F\96C5\9ED1

