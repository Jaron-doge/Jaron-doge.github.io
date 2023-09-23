---
title: 5 js BOM
date: 2021-8-22 00:00:00
tags: js
categories: 前端
---

## js BOM

### 1. BOM概述

<!-- more -->

#### 1.1 什么是BOM

BOM（Browser Object Model）即浏览器对象模型，它提供了独立于内容而与浏览器窗口进行交互的对象，其核心对象是 window。

**DOM**

- 文档对象模型 

- DOM 就是把「文档」当做一个「对象」来看待

- DOM 的顶级对象是 document

- DOM 主要学习的是操作页面元素

- DOM 是 W3C 标准规范

**BOM**

- 浏览器对象模型

- 把「浏览器」当做一个「对象」来看待

- BOM 的顶级对象是 window

- BOM 学习的是浏览器窗口交互的一些对象

- BOM 是浏览器厂商在各自浏览器上定义的，兼容性较差

#### 1.2 BOM的构成

![1.png](https://i.loli.net/2021/08/29/sm7kTpJMAP4LZCg.png)

### 2. window 对象的常见事件

#### 2.1 窗口加载事件

```js
window.onload = function(){}
或者 
window.addEventListener("load",function(){});
```

window.onload 是窗口 (页面）加载事件,当文档内容完全加载完成会触发该事件(包括图像、脚本文件、CSS 文件等), 就调用的处理函数。

**注意**

1. 有了 window.onload 就可以把 JS 代码写到页面元素的上方，因为 onload 是等页面内容全部加载完毕，再去执行处理函数。

2. window.onload 传统注册事件方式 只能写一次，如果有多个，会以最后一个 window.onload 为准。

3. 如果使用 addEventListener 则没有限制

```js
document.addEventListener('DOMContentLoaded',function(){})
```

DOMContentLoaded 事件触发时，仅当DOM加载完成，不包括样式表，图片，flash等等。

#### 2.2 调整窗口大小事件

```js
 window.onresize = function(){}

 window.addEventListener("resize",function(){});
```

window.onresize 是调整窗口大小加载事件, 当触发时就调用的处理函数。

**注意：**

1. 只要窗口大小发生像素变化，就会触发这个事件。

2. 我们经常利用这个事件完成响应式布局。 window.innerWidth 当前屏幕的宽度

### 3. 定时器

#### 3.1 setTimeout

```js
 window.setTimeout(调用函数, [延迟的毫秒数]);
```

setTimeout() 方法用于设置一个定时器，该定时器在定时器到期后执行调用函数。

**注意**

1. 因为定时器可能有很多，所以我们经常给定时器赋值一个标识符。

#### 3.2 clearTimeout

```js
window.clearTimeout(timeoutID)
```

clearTimeout()方法取消了先前通过调用 setTimeout() 建立的定时器。

#### 3.3 setInterval

```js
window.setInterval(回调函数, [间隔的毫秒数]);
```

setInterval() 方法重复调用一个函数，每隔这个时间，就去调用一次回调函数。

#### 3.4 clearInterval

```
window.clearInterval(intervalID);
```

clearInterval()方法取消了先前通过调用 setInterval()建立的定时器。

**例：发送短信**

```js
var btn = document.querySelector('.btn1');
btn.disabled = true;
var time = 60;
var timer = setInterval(() => {
    if (time == 0) {
        clearInterval(timer);
        btn.disabled = false;
        btn.innerHTML = '获取验证码';
        time = 60;
    } else {
        btn.innerHTML = time + 's';
        time--;
    }
}, 1000);
```

### 4. JS 执行机制

#### 4.1 同步任务

同步任务都在主线程上执行，形成一个执行栈。

#### 4.2 异步任务

JS 的异步是通过回调函数实现的。

一般而言，异步任务有以下三种类型:

1、普通事件，如 click、resize 等

2、资源加载，如 load、error 等

3、定时器，包括 setInterval、setTimeout 等

异步任务相关回调函数添加到**任务队列**中（任务队列也称为消息队列）。

#### 4.3 执行机制

1. 先执行执行栈中的同步任务。

2. 异步任务（回调函数）放入任务队列中。

3. 一旦执行栈中的所有同步任务执行完毕，系统就会按次序读取任务队列中的异步任务，于是被读取的异步任务结束等待状态，进入执行栈，开始执行。

   ![2.png](https://i.loli.net/2021/08/29/6IoeB4v8srmWjih.png)

![3.png](https://i.loli.net/2021/08/29/P8au3RyM5Ubt4ze.png)

由于主线程不断的重复获得任务、执行任务、再获取任务、再执行，所以这种机制被称为事件循环（ event loop）。

### 5. location对象

#### 5.1 location对象

window 对象给我们提供了一个 location 属性用于获取或设置窗体的 URL，并且可以用于解析 URL 。 因为这个属性返回的是一个对象，所以我们将这个属性也称为 location 对象。

#### 5.2 URL

统一资源定位符 (Uniform Resource Locator, URL) 是互联网上标准资源的地址。互联网上的每个文件都有一个唯一的 URL，它包含的信息指出文件的位置以及浏览器应该怎么处理它。

```js
 protocol://host[:port]/path/[?query]#fragment

 http://www.itcast.cn/index.html?name=andy&age=18#link
```

![4.png](https://i.loli.net/2021/08/29/lm7IQnF8xA9RvKB.png)

#### 5.3 location 对象的属性

![5.png](https://i.loli.net/2021/08/29/AX5Csl1nI7vKWoc.png)

**例：页面间数据传递**

```html
<!-- login.html -->
<body>
    <form action="index.html">
        用户名：<input type="text" name="username">
        <input type="submit" value="登录">
    </form>
</body>
```

```html
<!-- index.html -->
<body>
    <div></div>
    <script>
        var params = location.search.substr(1);
        var arr = params.split('=');
        var div = document.querySelector('div');
        div.innerHTML = arr[1] + '欢迎你';
    </script>
</body>
```

#### 5.4 location 对象的方法

![6.png](https://i.loli.net/2021/08/29/XWSG6flriHUy9w4.png)

- assign记录浏览历史，可以后退

- replace不可以后退

### 6. navigator 对象

navigator 对象包含有关浏览器的信息，它有很多属性，我们最常用的是 userAgent，该属性可以返回由客户机发送服务器的 user-agent 头部的值。

下面前端代码可以判断用户那个终端打开页面，实现跳转

```js
if((navigator.userAgent.match(/(phone|pad|pod|iPhone|iPod|ios|iPad|Android|Mobile|BlackBerry|IEMobile|MQQBrowser|JUC|Fennec|wOSBrowser|BrowserNG|WebOS|Symbian|Windows Phone)/i))) {
    window.location.href = "";     //手机
 } else {
    window.location.href = "";     //电脑
 }
```

### 7. history 对象

window 对象给我们提供了一个 history 对象，与浏览器历史记录进行交互。该对象包含用户（在浏览器窗口中）访问过的 URL。

![7.png](https://i.loli.net/2021/08/29/lbWxZGBwA7fae5D.png)
