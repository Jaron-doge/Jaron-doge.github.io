---
title: 2 HTML常用标签
date: 2021-7-15 00:00:00
tags: html
categories: 前端
---

## HTML常用标签

### 1. 段落和换行

<!-- more -->

- p：段落标签

- br：换行标签

### 2. 文本格式化标签

- strong或b：加粗
- em或i：倾斜t
- del或s：删除线

- ins或u：下划线

### 3. \<div>和\<span>标签

用来装内容的盒子，无语义

**特点**

- div一行只能放一个，大盒子。
- span一行上可以多个，小盒子。

### 4. image标签

![图片属性.png](https://i.loli.net/2021/07/21/vdHcM21f7qaEz8n.png)

#### 5. 相对路径与绝对路径

![路径.png](https://i.loli.net/2021/07/21/NAHObQGc7qXs2zx.png)

**绝对路径**

```html
src=“https://i0.hdslb.com/bfs/face/4a425d42e4e3829a07e4a1b0d16c97062b8a3c5b.jpg@160w_160h_1c.webp”
```

### 6. 超链接标签

- a：超链接

```html
 <a href="跳转目标" target="目标窗口的弹出方式"> 文本或图像 </a>
```

![image-20210706102901832.png](https://i.loli.net/2021/07/21/AipuHNTxfh8WR1b.png)

#### 链接分类

- 外部链接
- 内部链接
- 空链接：href="#"

- 下载链接：地址链接的是文件

```html
href="img.zip"
```

- 网页元素的链接

```html
<a href="http://www.baidu.com"> <img src="img.jpg"/> </a>
```

- 锚点链接

```html
<a href="#two"> </a>
<h3 id="two">
   	h3
</h3>
```

### 7. 注释和特殊字符

**注释**

```html
<!-- more -->
```

**特殊字符**

![特殊字符.png](https://i.loli.net/2021/07/21/sduGSZzqaUMI3k8.png)

### 8. 表格标签

```html
<table>
    <tr>
        <th>	</th>
    	<td>
        </td>
    </tr>
</table>
```

1. \</table> 是用于定义表格的标签。
2. \</tr> 标签用于定义表格中的行，必须嵌套在 \</table>标签中。
3. \</td> 用于定义表格中的单元格，必须嵌套在\</tr>标签中。
4. 字母 td 指表格数据（table data），即数据单元格的内容。

5. th：表头单元格，会加粗居中 

#### 表格属性

![表格属性.png](https://i.loli.net/2021/07/21/GK3iaqusnjcl694.png)

#### 表格结构标签

- thead：表格的头部区域
- tbody：表格的主体区域

#### 合并单元格

- rowspan：跨行合并，最上侧单元格作为目标单元格
- colspan：跨列合并，最左侧单元格作为目标单元格

找到目标单元格，写上合并方式，删除多余的单元格。

### 9. 列表标签

#### 无序列表

```html
<ul>
    <li></li>
    <li></li>
</ul>
```

ul里只能放li，li里面可以用其他的标签

#### 有序列表

```html
<ol>
    <li></li>
    <li></li>
</ol>
```

#### 自定义列表

```html
<dl>
    <dt>关注我们</dt>
    <dd>新浪微博</dd>
    <dd>官方微信</dd>
</dl>
```

- dl用于定义列表
- dt表示项目
- dd表示名字

### 10. 表单标签

 由表单域、表单控件、提示信息组成

#### 表单域

```html
<form action="url地址" method="提交方式" name="表单域名称">
    
</form>
```

![表单常用属性.png](https://i.loli.net/2021/07/21/MUQ4awtriW1eZKX.png)

#### 表单控件

##### input输入表单元素

```html
<input type="属性值" />
```

![表单标签.png](https://i.loli.net/2021/07/21/AawbfjX3WGoFP72.png)

- 单标签

![表单其他属性.png](https://i.loli.net/2021/07/21/ZlC9F5fkotqI1xN.png)

- 多选1必须具有相同的name
- value主要针对后台人员使用
- checked针对按钮选择

##### label标签

label标签用于绑定一个表单元素，当点击label内文本时，会自动聚焦

```html
<label for="sex">男</label>
<input type="radio" name="sex" id="sex"/>
```

- label标签的for属性应当与相关元素的id属性相同

##### select下拉表单元素

```html
<select>
    <option></option>
</select>
```

- 可以在option中定义selected="selected"

##### texarea文本域元素

```html
<textarea rows="3" cols="50">
</textarea>
```

 
