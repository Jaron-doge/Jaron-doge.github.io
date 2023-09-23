---
title: 1 js 简介
date: 2021-8-1 00:00:00
tags: js
categories: 前端
---

## js 简介

### js简介

<!-- more -->

浏览器本身并不会执行JS代码，而是通过内置 JavaScript 引擎(解释器) 来执行 JS 代码 。JS 引擎执行代码时逐行解释每一句源码（转换为机器语言），然后由计算机去执行，所以 JavaScript 语言归为脚本语言，会逐行解释执行。

### js书写位置

JS有3中书写位置，分别为行内、内嵌、外部

 **行内**

```html
 <input type="button" value="1" onclick="alert('2')">
```

**内嵌**

```html
<script>
    alert('123');
</script>
```

**外部**

```html
<script src="./index.js"></script>
```



