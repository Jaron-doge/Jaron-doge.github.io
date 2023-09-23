---
title: 4 js 事件高级
date: 2021-8-15 00:00:00
tags: js
categories: 前端
---

## js 事件高级

### 1. 注册事件

<!-- more -->

注册事件有两种方式：传统方式和方法监听注册方式

#### 1.1 传统注册方式

- 利用 on 开头的事件 onclick 

- <button onclick=“alert('hi~')”>\</button>

- btn.onclick = function() {} 

- 特点： 注册事件的**唯一性**

- 同一个元素同一个事件只能设置一个处理函数，**最后注册的处理函数将会覆盖前面注册的处理函数**

#### 1.2 addEventListener事件监听方式

- addEventListener() 它是一个方法

- 特点：**同一个元素同一个事件可以注册多个监听器**，按注册顺序依次执行

```js
eventTarget.addEventListener(type, listener[, useCapture])  
```

eventTarget.addEventListener()方法将指定的监听器注册到 eventTarget（目标对象）上，当该对象触发指定的事件时，就会执行事件处理函数。

该方法接收三个参数：

- type：事件类型是字符串，比如 click 、mouseover ，注意这里不要带 on

- listener：事件处理函数，事件发生时，会调用该监听函数

- useCapture：可选参数，是一个布尔值，默认是 false。学完 DOM 事件流后，我们再进一步学习

#### 1.3 attachEvent事件监听方式

```js
eventTarget.attachEvent(eventNameWithOn, callback) 
```

eventTarget.attachEvent()方法将指定的监听器注册到 eventTarget（目标对象） 上，当该对象触发指定的事件时，指定的回调函数就会被执行。

该方法接收两个参数：

- eventNameWithOn：事件类型字符串，比如 onclick 、onmouseover ，这里要带 on

- callback： 事件处理函数，当目标触发事件时回调函数被调用

### 2. 删除事件

#### 2.1 传统注册方式

```js
eventTarget.onclick = null;
```

#### 2.2 方法监听注册方式

```js
eventTarget.removeEventListener(type, listener[, useCapture]);
eventTarget.detachEvent(eventNameWithOn, callback);
```

### 3. DOM事件流

事件流描述的是从页面中接收事件的顺序。

事件发生时会在元素节点之间按照特定的顺序传播，这个传播过程即 DOM 事件流

DOM 事件流分为3个阶段： 

1. 捕获阶段

2. 当前目标阶段

3. 冒泡阶段 

![1.png](https://i.loli.net/2021/08/29/sY4qeXgctF2zxSP.png)

**注意**

1. JS 代码中只能执行捕获或者冒泡其中的一个阶段。

2. onclick 和 attachEvent 只能得到冒泡阶段。

3. addEventListener(type, listener[, useCapture])第三个参数如果是 true，表示在事件捕获阶段调用事件处理程序；如果是 false（不写默认就是false），表示在事件冒泡阶段调用事件处理程序。

4. 实际开发中我们很少使用事件捕获，我们更关注事件冒泡。

5. 有些事件是没有冒泡的，比如 onblur、onfocus、onmouseenter、onmouseleave

### 4. 事件对象

#### 4.1 事件对象

```js
 eventTarget.onclick = function(event) {} 
```

这个 event 是个形参，系统帮我们设定为事件对象，不需要传递实参过去。

当我们注册事件时， event 对象就会被系统自动创建，并依次传递给事件监听器（事件处理函数）。

官方解释：event 对象代表事件的状态，比如键盘按键的状态、鼠标的位置、鼠标按钮的状态。

简单理解：事件发生后，跟事件相关的一系列信息数据的集合都放到这个对象里面，这个对象就是事件对象 event，它有很多属性和方法。

#### 4.2 事件对象的常见属性和方法

**e.target和this的区别**

 this 是事件绑定的元素， 这个函数的调用者（绑定这个事件的元素） 

 e.target 是事件触发的元素。

例：绑ul点li，this指向ul，target指向li

![2.png](https://i.loli.net/2021/08/29/pmYqvsZn71OkexM.png)

### 5. 阻止事件冒泡

```js
e.stopPropagation() 
```

### 6. 事件委托

**事件委托的原理**

**<font color="red">不是每个子节点单独设置事件监听器，而是事件监听器设置在其父节点上，然后利用冒泡原理影响设置每个子节点。</font>**

例：给 ul 注册点击事件，然后利用事件对象的 target 来找到当前点击的 li，因为点击 li，事件会冒泡到 ul 上， ul 有注册事件，就会触发事件监听器。

**事件委托的作用**

我们只操作了一次 DOM ，提高了程序的性能。

### 7. 常用的鼠标事件

#### 7.1 常用的鼠标事件

![3.png](https://i.loli.net/2021/08/29/wSmFjEDdh7otIOg.png)

**1. 禁止鼠标右键菜单**

contextmenu主要控制应该何时显示上下文菜单，主要用于程序员取消默认的上下文菜单

```js
document.addEventListener('contextmenu', function(e) {
	e.preventDefault();
})
```

**2. 禁止鼠标选中**

```js
document.addEventListener('selectstart', function(e) {
    e.preventDefault();
})
```

#### 7.2 鼠标事件对象

![4.png](https://i.loli.net/2021/08/29/tDrvaywFBu2Vcb8.png)

**例：跟随鼠标移动**

① 鼠标不断的移动，使用鼠标移动事件： mousemove

② 在页面中移动，给document注册事件

③ 图片要移动距离，而且不占位置，我们使用绝对定位即可

④ 核心原理： 每次鼠标移动，我们都会获得最新的鼠标坐标， 把这个x和y坐标做为图片的top和left 值就可以移动图片

### 8. 常用的键盘事件

#### 8.1 常用键盘事件

![5.png](https://i.loli.net/2021/08/29/tsY8zfcICESnNBk.png)

三个事件的执行顺序是： keydown → keypress → keyup

#### 8.2 键盘事件对象

![6.png](https://i.loli.net/2021/08/29/mZIKtMUwDyQ9dsr.png)

onkeydown 和 onkeyup 不区分字母大小写，onkeypress 区分字母大小写。

**例**

```html
<style>
    * {
        margin: 0;
        padding: 0;
    }

    .search {
        position: relative;
        width: 178px;
        margin: 100px;
    }

    .con {
        display: none;
        position: absolute;
        top: -40px;
        width: 171px;
        border: 1px solid rgba(0, 0, 0, .2);
        box-shadow: 0 2px 4px rgba(0, 0, 0, .2);
        padding: 5px 0;
        font-size: 18px;
        line-height: 20px;
        color: #333;
    }

    .con::before {
        content: '';
        width: 0;
        height: 0;
        position: absolute;
        top: 28px;
        left: 18px;
        border: 8px solid #000;
        border-style: solid dashed dashed;
        border-color: #fff transparent transparent;
    }
</style>
<body>
    <div class="search">
        <div class="con">123</div>
        <input type="text" placeholder="请输入您的快递单号" class="jd">
    </div>
    <script>
        // 快递单号输入内容时， 上面的大号字体盒子（con）显示(这里面的字号更大）
        // 表单检测用户输入： 给表单添加键盘事件
        // 同时把快递单号里面的值（value）获取过来赋值给 con盒子（innerText）做为内容
        // 如果快递单号里面内容为空，则隐藏大号字体盒子(con)盒子
        var con = document.querySelector('.con');
        var jd_input = document.querySelector('.jd');
        jd_input.addEventListener('keyup', function() {
                // console.log('输入内容啦');
                if (this.value == '') {
                    con.style.display = 'none';
                } else {
                    con.style.display = 'block';
                    con.innerText = this.value;
                }
            })
            // 当我们失去焦点，就隐藏这个con盒子
        jd_input.addEventListener('blur', function() {
                con.style.display = 'none';
            })
            // 当我们获得焦点，就显示这个con盒子
        jd_input.addEventListener('focus', function() {
            if (this.value !== '') {
                con.style.display = 'block';
            }
        })
    </script>
</body>
```



