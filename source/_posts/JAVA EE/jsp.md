---
title: 2 jsp
date: 2021-9-2 00:00:00
tags: 
categories: JAVA EE
---

## jsp

### 1. jsp简介

<!-- more -->

jsp 的全换是java server pages。Java 的服务器页面。

jsp 的主要作用是代替Servlet 程序回传html 页面的数据。

jsp 页面本质上是一个Servlet 程序。

### 2. jsp语法

#### 2.1 jsp头部的 page 指令

jsp 的page 指令可以修改jsp 页面中一些重要的属性，或者行为。

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
```

1. **language** 属性

   表示jsp 翻译后是什么语言文件。暂时只支持java。

2. **contentType** 属性

   表示jsp 返回的数据类型是什么。也是源码中response.setContentType()参数值

3. **pageEncoding** 属性

   表示当前jsp 页面文件本身的字符集。

4. **import** 属性

   跟java 源代码中一样。用于导包，导类。

5. **autoFlush** 属性

   设置当out 输出流缓冲区满了之后，是否自动刷新缓冲区。默认值是true。

6. **buffer** 属性

   设置out 缓冲区的大小。默认是8kb

7. **errorPage** 属性

   设置当jsp 页面运行时出错，自动跳转去的错误页面路径。

8. **isErrorPage** 属性

   设置当前jsp 页面是否是错误信息页面。默认是false。如果是true 可以获取异常信息。

9. **session** 属性

   设置访问当前jsp 页面，是否会创建HttpSession 对象。默认是true。

10. **extends** 属性

    设置jsp 翻译出来的java 类默认继承谁。

#### 2.2 jsp 声明脚本

```jsp
<%! 声明java 代码%>
```

作用：可以给jsp 翻译出来的java 类定义属性和方法甚至是静态代码块。内部类等。

![1.png](https://i.loli.net/2021/09/10/JsijRt9L1VkErbD.png)

#### 2.3 jsp 表达式脚本

```jsp
<%=表达式%>
```

表达式脚本的作用是：的jsp 页面上输出数据。

**表达式脚本的特点**

1、表达式脚本都会被翻译成为out.print()输出到页面上

2、由于表达式脚本翻译的内容都在_jspService() 方法中,所以_jspService()方法中的对象都可以直接使用。

3、表达式脚本中的表达式不能以分号结束。

```jsp
<%=12 %> <br>
<%=12.12 %> <br>
<%="我是字符串" %> <br>
<%=map%> <br>
<%=request.getParameter("username")%>
```

#### 2.4 jsp 代码脚本

```jsp
<%
	java 语句
%>
```

代码脚本的作用是：可以在jsp 页面中，编写我们自己需要的功能（写的是java 语句）。

![2.png](https://i.loli.net/2021/09/10/KYQHrFA78wUeDuJ.png)

#### 2.5 jsp 中的三种注释

##### 2.5.1 html注释

```jsp
<!-- 这是html 注释-->
```

html 注释会被翻译到java 源代码中。在_jspService 方法里，以out.writer 输出到客户端。

##### 2.5.2 java 注释

```jsp
<%
    // 单行java 注释
    /* 多行java 注释*/
%>
```

java 注释会被翻译到java 源代码中。

##### 2.5.3 jsp 注释

```jsp
<%-- 这是jsp 注释--%>
```

jsp 注释可以注掉jsp 页面中所有代码。

### 3. jsp 九大内置对象

jsp 中的内置对象，是指 Tomcat 在翻译 jsp 页面成为 Servlet 源代码后，内部提供的九大对象，叫内置对象。

![3.png](https://i.loli.net/2021/09/10/aYieUXRo7pu1OZI.png)

#### 3.1 jsp 四大域对象

四个域对象分别是：

![4.png](https://i.loli.net/2021/09/10/btTO6vIhSpQrDKV.png)

域对象是可以像Map 一样存取数据的对象。四个域对象功能一样。不同的是它们对数据的存取范围。

#### 3.2 jsp 中的 out 输出和 response.getWriter 输出的区别

response 中表示响应，我们经常用于设置返回给客户端的内容（输出）
out 也是给用户做输出使用的。

![5.png](https://i.loli.net/2021/09/10/Ds5YpfGFERIVxXd.png)

由于jsp 翻译之后，底层源代码都是使用out 来进行输出，所以**一般情况下。我们在jsp 页面中统一使用out 来进行输出**。避免打乱页面输出内容的顺序。

out.write() 输出字符串没有问题
out.print() 输出任意数据都没有问题（都转换成为字符串后调用的write 输出）

### 4. jsp 常用标签

#### 4.1 jsp 静态包含

示例说明：

<%@ include file=""%> 就是静态包含
    file 属性指定你要包含的jsp 页面的路径
    地址中第一个斜杠/ 表示为http://ip:port/工程路径/ 映射到代码的web 目录
    静态包含的特点：
    1、静态包含不会翻译被包含的jsp 页面。

​    2、静态包含其实是把被包含的jsp 页面的代码拷贝到包含的位置执行输出。

```jsp
<%@ include file="/include/footer.jsp"%>
```

#### 4.2 jsp 动态包含

示例说明：

<jsp:include page=""></jsp:include> 这是动态包含
page 属性是指定你要包含的jsp 页面的路径
动态包含也可以像静态包含一样。把被包含的内容执行输出到包含位置
	动态包含的特点：
	1、动态包含会把包含的jsp 页面也翻译成为java 代码

​	2、动态包含底层代码使用如下代码去调用被包含的jsp 页面执行输出。
​		JspRuntimeLibrary.include(request, response, "/include/footer.jsp", out, false);

​	3、动态包含，还可以传递参数

```jsp
<jsp:include page="/include/footer.jsp">
    <jsp:param name="username" value="bbj"/>
    <jsp:param name="password" value="root"/>
</jsp:include>
```

动态包含的底层原理：

![6.png](https://i.loli.net/2021/09/10/96OZqkepSxKrhbu.png)

#### 4.3 jsp 标签-转发

示例说明：

- <jsp:forward page=""></jsp:forward> 是请求转发标签，它的功能就是请求转发
- page 属性设置请求转发的路径

```jsp
<jsp:forward page="/scope2.jsp"></jsp:forward>
```

#### 4.4 jsp 代码示例

九九乘法表：

```jsp
<body>
    <%-- 练习一：在jsp 页面中输出九九乘法口诀表--%>
    <h1 align="center">九九乘法口诀表</h1>
    <table align="center">
    <%-- 外层循环遍历行--%>
    <% for (int i = 1; i <= 9; i++) { %>
        <tr>
        <%-- 内层循环遍历单元格--%>
        <% for (int j = 1; j <= i ; j++) { %>
        	<td><%=j + "x" + i + "=" + (i*j)%></td>
        <% } %>
        </tr>
    <% } %>
    </table>
</body>
```

jsp 请求转发的使用：
![7.png](https://i.loli.net/2021/09/10/PyxsINeo2ib1hjM.png)

### 5. Listener监听器

1、Listener 监听器它是JavaWeb 的三大组件之一。JavaWeb 的三大组件分别是：Servlet 程序、Filter 过滤器、Listener 监
听器。

2、Listener 它是JavaEE 的规范，就是接口

3、监听器的作用是，监听某种事物的变化。然后通过回调函数，反馈给客户（程序）去做一些相应的处理。

#### 5.1 ServletContextListener 监听器

ServletContextListener 它可以监听ServletContext 对象的创建和销毁。

ServletContext 对象在web 工程启动的时候创建，在web 工程停止的时候销毁。

监听到创建和销毁之后都会分别调用ServletContextListener 监听器的方法反馈。

两个方法：

```java
public interface ServletContextListener extends EventListener {
    /**
    * 在ServletContext 对象创建之后马上调用，做初始化
    */
    public void contextInitialized(ServletContextEvent sce);
    /**
    * 在ServletContext 对象销毁之后调用
    */
    public void contextDestroyed(ServletContextEvent sce);
}
```

使用ServletContextListener 监听器监听ServletContext 对象：

​	1、编写一个类去实现ServletContextListener
​	2、实现其两个回调方法
​	3、到web.xml 中去配置监听器

```java
public class MyServletContextListenerImpl implements ServletContextListener {
    @Override
    public void contextInitialized(ServletContextEvent sce) {
    	System.out.println("ServletContext 对象被创建了");
    }
    @Override
    public void contextDestroyed(ServletContextEvent sce) {
    	System.out.println("ServletContext 对象被销毁了");
    }
}
```

web.xml配置：

```xml
<!--配置监听器-->
<listener>
	<listener-class>com.java.listener.MyServletContextListenerImpl</listener-class>
</listener>
```

