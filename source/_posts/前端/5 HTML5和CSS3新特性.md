---
title: 5 HTML5和CSS3新特性
date: 2021-7-20 00:00:00
tags: [html,css]
categories: 前端
---

## HTML5和CSS3新特性

### 1. HTML5的新特性

<!-- more -->

#### 1.1 HTML5新增的语义化标签

- header：头部标签 
- nav：导航标签 
- article：内容标签 
- section：定义文档某个区域 
- aside：侧边栏标签 
- footer：尾部标签

<img src="https://i.loli.net/2021/07/21/SmM4zH56teo8hFx.png" alt="语义化标签.png" style="zoom:80%;" />

#### 1.2 HTML5 新增的多媒体标签

- \<audio>：音频

- \<video>：视频

##### 1.2.1 视频

<img src="https://i.loli.net/2021/07/21/uBnWdxFNgoTLv49.png" alt="video支持格式.png" style="zoom:80%;" />

**常用属性**

![video常用属性.png](https://i.loli.net/2021/07/21/mWlzqeAB3FCNari.png)

**语法**

```html
<video src="文件地址" controls="controls"></video>
```

```html
<!-- 兼容性写法 -->
<video controls="controls" width="300">
	<source src="move.ogg" type="video/ogg" >
	<source src="move.mp4" type="video/mp4" >
	您的浏览器暂不支持 <video> 标签播放视频
</ video >
```

##### 1.2.2 音频

<img src="https://i.loli.net/2021/07/21/mNBvePubMaXr3hI.png" alt="audio支持格式.png" style="zoom:80%;" />

**常用属性**

![audio常用属性.png](https://i.loli.net/2021/07/21/7k29OAvJ5mQf3hC.png)

**语法**

```html
<audio src="文件地址" controls="controls"></audio>
```

```html
<!-- 兼容性写法 -->
< audio controls="controls" >
	<source src="happy.mp3" type="audio/mpeg" >
	<source src="happy.ogg" type="audio/ogg" >
	您的浏览器暂不支持 <audio> 标签。
</ audio>
```

#### 1.3 HTML5 新增的 input 类型

验证的时候必须添加form表单域

![新增input表单.png](https://i.loli.net/2021/07/21/FvgxbzMHa1IAruj.png)

#### 1.4 HTML5 新增的表单属性

![image-20210718201124525.png](https://i.loli.net/2021/07/21/FV1mzHgLOufabSi.png)

```css
/* 可以通过以下设置方式修改placeholder里面的字体颜色。 */
input::placeholder {
	color: pink;
}
```

### 2. CSS3的新特性

- 属性选择器 
- 结构伪类选择器
- 伪元素选择器

#### 2.1 属性选择器

![属性选择器.png](https://i.loli.net/2021/07/21/drmwtCzW8vnOaF9.png)

```css
input[type="text"] {
    color: pink;
}
```

**注意**

类选择器、属性选择器、伪类选择器，**权重为 10**。

#### 2.2 结构伪类选择器

结构伪类选择器主要根据文档结构来选择器元素， 常用于根据父级选择器里面的子元

![结构伪类选择器-01.png](https://i.loli.net/2021/07/21/WxrlQdfcGuwCTv8.png)

**nth-child(n)**

nth-child（n） 选择某个父元素的一个或多个特定的子元素（重点）

- n 可以是数字，关键字和公式
- n 如果是数字，就是选择第 n 个子元素， 里面数字从1开始… 
- n 可以是关键字：even 偶数，odd 奇数 
- n 可以是公式：常见的公式如下 ( 如果n是公式，则从0开始计算，但是第 0 个元素或者超出了元素的个数会被忽略）

![nth-child公式.png](https://i.loli.net/2021/07/21/JfUacM297QKSrW6.png)

**区别**

1. **nth-child**：对父元素里面所有孩子排序选择（序号是固定的） 先找到第n个孩子，然后看看是否和E匹配 
2. **nth-of-type**：对父元素里面指定子元素进行排序选择。 先去匹配E ，然后再根据E 找第n个孩子

#### 2.3 伪元素选择器

伪元素选择器可以帮助我们利用CSS创建新标签元素，而不需要HTML标签，从而简化HTML结构。

![伪元素.png](https://i.loli.net/2021/07/21/Ca5LdDnzmJYUTZv.png)

- before 和 after 创建一个元素，但是属于行内元素
- 新创建的这个元素在文档树中是找不到的，所以我们称为伪元素 
- 语法： element::before {}  
- **before 和 after 必须有 content 属性**
- before 在父元素内容的前面创建元素，after 在父元素内容的后面插入元素 
- 伪元素选择器和标签选择器一样，**权重为 1**

```css
/* 权重是2 */
div::before {
    content: '';
}
```

**使用场景**

- 可以用来配合字体图标

- 可以用来做遮罩层

  ```css
  .box::before {
      display: none;
      position: absolute;
      content: "";
      width: 100%;
      height: 100%;
      background: rbga(0, 0, 0, .3);
  }
  
  .box:hover::before {
      display: block;
  }
  ```

- 清除浮动

  ```css
  .clearfix:after {
      content: "";
      display: block;
      height: 0;
      clear: both;
      visibility: hidden;
  } 
  ```

#### 2.4 CSS3 盒子模型

CSS3 中可以通过 **box-sizing** 来指定盒模型，有2个值：即可指定为 **content-box**、**border-box**，这样我们 计算盒子大小的方式就发生了改变。

可以分成两种情况： 

1. box-sizing: content-box 盒子大小为 width + padding + border （以前默认的） 
2. box-sizing: border-box 盒子大小为 width

```css
* {
    box-sizing: border-box;
}
```

#### 2.5 CSS3 其他特性

##### 2.5.1 CSS3滤镜filter：

filter CSS属性将模糊或颜色偏移等图形效果应用于元素。

```css
/* filter: 函数(); */ 
filter: blur(5px);
```

##### 2.5.2 CSS3 calc函数：

```css
width: calc(100% - 80px);
```

括号里面可以使用 + - * / 来进行计算。

#### 2.6 CSS3 过渡

过渡动画： 是从一个状态渐渐的过渡到另外一个状态，经常和 :hover 一起搭配使用

```css
transition: 要过渡的属性 花费时间 运动曲线 何时开始;
```

1. 属性 ： 想要变化的 css 属性， 宽度高度 背景颜色 内外边距都可以 。如果想要所有的属性都 变化过渡， 写一个all 就可以。 
2. 花费时间： 单位是 秒**（必须写单位）** 比如 0.5s  
3. 运动曲线： 默认是 ease **（可以省略）**
4. 何时开始 ：单位是 秒**（必须写单位）**可以设置延迟触发时间 默认是 0s **（可以省略）**

多个属性用 , 进行分割

```css
transition: width 1s ease 1s, height 1s ease 1s;
transition: all 1s;
```

##### 2.6.1 案例

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        .header-logo {
            position: relative;
        }
        /* 设置a标签的样式 */
        
        .logo {
            display: block;
            width: 55px;
            height: 55px;
            overflow: hidden;
            background-color: #ff6700;
            text-align: left;
            text-indent: -9999em;
        }
        /* mi logo的样式 */
        
        .logo::before {
            /* 定位 */
            position: absolute;
            /* 伪元素必须要设置content属性 */
            content: '';
            /* 左偏移 */
            left: 0;
            /* 上偏移 */
            top: 0;
            width: 55px;
            height: 55px;
            /* 设置过渡 */
            transition: all .3s;
            /* 背景图片 */
            background: url(./images/mi-logo.png) no-repeat center center;
            /* 透明度 */
            opacity: 1;
        }
        /* mi home 的样式 */
        
        .logo::after {
            position: absolute;
            content: '';
            left: 0;
            top: 0;
            width: 55px;
            height: 55px;
            transition: all .3s;
            background: url(./images/mi-home.png) no-repeat center center;
            margin-left: -55px;
            opacity: 0;
        }
        /* 鼠标移入 让mi logo 往右侧进行滑动 */
        
        .logo:hover::before {
            opacity: 0;
            margin-left: 55px;
        }
        /* 鼠标移入 让mi home 回到盒子中间 */
        
        .logo:hover::after {
            opacity: 1;
            margin-left: 0;
        }
    </style>
</head>

<body>
    <div class="header-logo">
        <a href="" class="logo" title="小米官网">小米官网</a>
    </div>
</body>

</html>
```

