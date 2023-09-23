---
title: 1 Java EE
date: 2021-9-1 00:00:00
tags: 
categories: JAVA EE 
---

## Java EE

<!-- more -->

![1.png](https://i.loli.net/2021/09/10/tEQTmnpUJKaDdl3.png)

![2.png](https://i.loli.net/2021/09/10/eDuJ26NmzLh9nxs.png)

```java
String sql = "select"
```

### 1. 路径

获取绝对路径路径 ,开发项目一定要使用绝对路径，不然肯定出错

```jsp
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
<base href="<%=basePath%>">
```

request.getSchema()可以返回当前页面使用的协议，http 或是 https;

request.getServerName()可以返回当前页面所在的服务器的名字;

request.getServerPort()可以返回当前页面所在的服务器使用的端口,就是80;

request.getContextPath()可以返回当前页面所在的应用的名字;

### 2. Debug

#### 2.1 测试工具栏

![3.png](https://i.loli.net/2021/09/10/ED6S4duG1hqfY3z.png)

![4.png](https://i.loli.net/2021/09/10/akT3cIZNnMvhREU.png)

![5.png](https://i.loli.net/2021/09/10/XyuBN7SPYi153Ap.png)

#### 2.2 方法栈调用窗口

1、方法调用栈可以查看当前线程有哪些方法调用信息
2、下面的调用上一行的方法

![6.png](https://i.loli.net/2021/09/10/EoCUDlFtVLHQW1d.png)

### 快捷键

- ctrl + shift + T：新建测试
- ctrl + alt + T：try + catch

- ctrl + p：查看函数需要的参数
- ctrl + alt + shift + L：格式化代码

- ctrl + shift + / ：多行注释
