---
title: 4 xml
date: 2021-9-4 00:00:00
tags: 
categories: JAVA EE
---

## xml

### 1. xml语法

<!-- more -->

自己去菜鸟看去

**例子**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- xml 声明version 是版本的意思encoding 是编码-->
<books> <!-- 这是xml 注释-->
<book id="SN123123413241"> <!-- book 标签描述一本图书id 属性描述的是图书的编号-->
<name>java 编程思想</name> <!-- name 标签描述的是图书的信息-->
<author>华仔</author> <!-- author 单词是作者的意思，描述图书作者-->
<price>9.9</price> <!-- price 单词是价格，描述的是图书的价格-->
</book>
<book id="SN12341235123"> <!-- book 标签描述一本图书id 属性描述的是图书的编号-->
<name>葵花宝典</name> <!-- name 标签描述的是图书的信息-->
<author>班长</author> <!-- author 单词是作者的意思，描述图书作者-->
<price>5.5</price> <!-- price 单词是价格，描述的是图书的价格-->
</book>
</books>
```

### 2. xml解析 dom4j

1. 导入 dom4j.jar 包

2. 创建SaxReader对象

```java
SAXReader reader = new SAXReader();
// 这个对象用于读取xml 文件，然后返回一个Document。
```

3. 读取XML文件

```java
Document document = reader.read("src/books.xml");
// 打印到控制台，看看是否创建成功
System.out.println(document);
```

4. 通过根元素对象，获取book标签对象

```
Element root = document.getRootElement();
List<Element> books = root.elements("book");
```

5. 拿到book 下面的name 元素对象

```xml
Element nameElement = book.element("name");
```

6. getText()得到元素内的文本内容

```xml
nameElement.getText();
```

7. elementText()得到元素内的文本内容

```xml
book.elementText("name");
```

