---
title: 3 js DOM操作
date: 2021-8-8 00:00:00
tags: js
categories: 前端
---

## js DOM操作

### 1. 获取元素

<!-- more -->

#### 1.1 根据 ID 获取

```js
document.getElementById('id');
```

使用 console.dir() 可以打印我们获取的元素对象，更好的查看对象里面的属性和方法。

#### 1.2 根据标签名获取

返回带有指定标签名的对象的集合。

```js
document.getElementsByTagName('标签名');
```

- 因为得到的是一个对象的集合，所以我们想要操作里面的元素就需要遍历。

- 得到元素对象是动态的

还可以获取某个元素(父元素)内部所有指定标签名的子元素。

```js
var ol = document.getElementById('ol');
var li = ol.getElementsByTagName('li');
```

#### 1.3 通过 HTML5 新增的方法获取

```js
1. document.getElementsByClassName(‘类名’)；// 根据类名返回元素对象集合
2. document.querySelector('选择器');        // 根据指定选择器返回第一个元素对象
3. document.querySelectorAll('选择器');     // 根据指定选择器返回
```

querySelector 和 querySelectorAll里面的选择器需要加符号,比如:document.querySelector('#nav'); 

#### 1.4 获取特殊元素（body，html）

获取body元素

```js
1. doucumnet.body  // 返回body元素对象
```

获取html元素

```js
1. document.documentElement  // 返回html元素对象
```

### 2. 事件

1. 获取事件源

2. 注册事件（绑定事件）

3. 添加事件处理程序（采取函数赋值形式）

```js
var btn = document.getElementById('btn');
btn.onclick = function() {
  alert('你好吗');  
};
```

### 3. 操作元素

#### 3.1 改变元素内容

从起始位置到终止位置的内容, 但它去除 html 标签， 同时空格和换行也会去掉

```js
element.innerText = '无法加粗';
```

起始位置到终止位置的全部内容，包括 html 标签，同时保留空格和换行

```js
element.innerHTML = '<strong>加粗</strong>';
```

- 两种都可读写

#### 3.2 样式属性操作

```
1. element.style     行内样式操作
2. element.className 类名样式操作
```

- js 里面的样式采取驼峰命名法 比如 fontSize、 backgroundColor
- js 修改 style 样式操作，产生的是行内样式，CSS 权重比较高

**例：循环精灵图**

```js
<script>
    var lis = document.querySelectorAll('li');
    for (var i = 0; i < lis.length; i++) {
    // 让索引号 乘以 44 就是每个li 的背景y坐标 index就是我们的y坐标
    var index = i * 44;
    lis[i].style.backgroundPosition = '0 -' + index + 'px';
    }
</script>
```

**例：通过类名修改样式**

```js
<style>
    .change {
        color: #fff;
    }
</style>
<script> 
	var test = document.querySelector('div');
	test.onclick = function() {
        // 让当前元素类名改为change
		this.className = 'change';
    }
</script>
```

- 如果样式修改较多，可以采取操作类名方式更改元素样式
- className 会直接更改元素的类名，会覆盖原先的类名

#### 3.3 排他思想

如果有同一组元素，我们想要某一个元素实现某种样式， 需要用到循环的排他思想算法：
1. 所有元素全部清除样式（干掉其他人）
2. 给当前元素设置样式 （留下我自己）

```js
<script>
    var btns = document.getElementsByTagName('button');
	for (var i = 0; i < btns.length; i++) {
    btns[i].onclick = function() {
        for (var i = 0; i < btns.length; i++) {
            btns[i].style.backgroundColor = '';
        }
        this.style.backgroundColor = 'pink';
    }
}
</script>
```

#### 3.4 自定义属性值

1. 获取属性值

  element.属性  获取属性值。

  element.getAttribute('属性');

**区别**

- element.属性  获取内置属性值（元素本身自带的属性）

- element.getAttribute(‘属性’);  主要获得自定义的属性 （标准） 我们程序员自定义的属性

2. 设置属性值

   element.setAttribute('属性', '值');

3. 移除属性

   element.removeAttribute('属性');

#### 3.5 H5自定义属性

1. 设置H5自定义属性

   H5规定自定义属性data-开头做为属性名并且赋值。

   ```js
   比如 <div data-index="1"></div>
   ```

2. 获取H5自定义属性

   兼容性获取  element.getAttribute(‘data-index’);

   H5新增 element.dataset.index 或者 element.dataset[‘index’]  ie 11才开始支持

   ```js
   // 如果自定义属性里面又多个-链接的单词，获取的时候采取驼峰命名法
   div.dataset.listName;
   ```

### 4. 节点

#### 4.1 节点概述

一般地，节点至少拥有nodeType（节点类型）、nodeName（节点名称）和nodeValue（节点值）这三个基本属性。

- 元素节点  nodeType 为 1

- 属性节点 nodeType 为 2

- 文本节点 nodeType 为 3 （文本节点包含文字、空格、换行等）

#### 4.2 节点层级

**1. 父级节点**

```js
node.parentNode
```

​	返回最近的一个父节点

**2. 子节点**

```js
1. parentNode.childNodes
```

​	返回值里面包含了所有的子节点，包括元素节点，文本节点等。

​	如果只想要获得里面的元素节点，则需要专门处理。 

```js
var ul = document. querySelector(‘ul’);
    for(var i = 0; i < ul.childNodes.length;i++) {
    if (ul.childNodes[i].nodeType == 1) {    
        console.log(ul.childNodes[i]);
    }
}
```

```js
2. parentNode.children
```

​	parentNode.children 是一个只读属性，返回所有的子元素节点。它只返回	子元素节点，其余节点不返回 

```js
3. parentNode.firstChild   
```

```js
4. parentNode.lastChild
```

```js
5. parentNode.firstElementChild
```

​	firstElementChild  返回第一个子元素节点

```js
6. parentNode.lastElementChild
```

​	lastElementChild 返回最后一个子元素节点

```js
7. parentNode.children[index];
```

**3. 兄弟节点**

```js
1. node.nextSibling  
2. node.previousSibling    
```

​	包含所有的节点

```js
3. nextElementSibling
4. node.previousElementSibling    
```

​	返回元素节点

#### 4.3 创建节点

```js
document.createElement('tagName')
```

#### 4.4 添加节点

```js
1. node.appendChild(child) 
```

​	node.appendChild() 方法将一个节点添加到指定父节点的子节点列表末尾

```js
2. node.insertBefore(child, 指定元素) 
```

​	node.insertBefore() 方法将一个节点添加到父节点的指定子节点前面

#### 4.5 删除节点

```js
node.removeChild(child) 
```

​	node.removeChild() 方法从 DOM 中删除一个子节点，返回删除的节点

#### 4.6 复制节点

```js
node.cloneNode() 
```

​	node.cloneNode() 方法返回调用该方法的节点的一个副本

**注意**

1. 如果括号参数为空或者为 false ，则是浅拷贝，即只克隆复制节点本身，不克隆里面的子节点。

2. 如果括号参数为 true ，则是深度拷贝，会复制节点本身以及里面所有的子节点。

#### 4.7 三种动态创建元素区别

- document.write()

- element.innerHTML

- document.createElement()

**区别**

1. document.write 是直接将内容写入页面的内容流，但是文档流执行完毕，则它**<font color="red">会导致页面全部重绘</font>**

2. innerHTML 是将内容写入某个 DOM 节点，不会导致页面全部重绘

3. innerHTML 创建多个元素效率更高（不要拼接字符串，采取数组形式拼接），结构稍微复杂

   ```js
   var array = [];
   for (var i = 0; i < 1000; i++) {
       array.push('<div style="width:100px; height:2px; border:1px solid blue;"></div>');
   }
   document.body.innerHTML = array.join('');
   ```

4. createElement() 创建多个元素效率稍低一点点，但是结构更清晰

总结：不同浏览器下，innerHTML 效率要比 creatElement 高

