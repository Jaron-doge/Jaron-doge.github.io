---
title: 5 EL 表达式 JSTL 标签库
date: 2021-9-5 00:00:00
tags: 
categories: JAVA EE 
---

## EL 表达式 & JSTL 标签库

### 1. EL 表达式

<!-- more -->

EL 表达式的全称是：Expression Language。是表达式语言。

EL 表达式的什么作用：EL 表达式主要是代替jsp 页面中的表达式脚本在jsp 页面中进行数据的输出。因为EL 表达式在输出数据的时候，要比jsp 的表达式脚本要简洁很多。

```jsp
<body>
    <%
    request.setAttribute("key","值");
    %>
    表达式脚本输出key 的值是：
 <%=request.getAttribute("key1")==null?"":request.getAttribute("key1")%><br/>
    EL 表达式输出key 的值是：${key1}
</body>
```

EL 表达式的格式是：${表达式}
EL 表达式在输出null 值的时候，输出的是空串。jsp 表达式脚本输出null 值的时候，输出的是null 字符串。

#### 1.1 EL 表达式搜索域数据的顺序

EL 表达式主要是在jsp 页面中输出数据。
主要是输出域对象中的数据。

当四个域中都有相同的key 的数据的时候，EL 表达式会按照四个域的从小到大的顺序去进行搜索，找到就输出。

```jsp
<body>
    <%
    //往四个域中都保存了相同的key 的数据。
    request.setAttribute("key", "request");
    session.setAttribute("key", "session");
    application.setAttribute("key", "application");
    pageContext.setAttribute("key", "pageContext");
    %>
    ${ key }
</body>
```

#### 1.2 EL 表达式输出类的属性

​	EL会找get方法进行调用

```jsp
<body>
    输出Person：${ p }<br/>
    输出Person 的name 属性：${p.name} <br>
    输出Person 的pnones 数组属性值：${p.phones[2]} <br>
    输出Person 的cities 集合中的元素值：${p.cities} <br>
    输出Person 的List 集合中个别元素值：${p.cities[2]} <br>
    输出Person 的Map 集合: ${p.map} <br>
    输出Person 的Map 集合中某个key 的值: ${p.map.key3} <br>
    输出Person 的age 属性：${p.age} <br>
</body>
```

#### 1.3 EL 表达式运算

语法：${ 运算表达式} ， EL 表达式支持如下运算符：

**1. 关系运算**

![1.png](https://i.loli.net/2021/09/10/xiX2FQYyhvDg7Ib.png)

**2. 逻辑运算**

![2.png](https://i.loli.net/2021/09/10/DMxi8g2IXQvfc3E.png)

**3. 算术运算**

![3.png](https://i.loli.net/2021/09/10/7BxNUJsERF6V3kq.png)

**4. empty运算**

empty 运算可以判断一个数据是否为空，如果为空，则输出true,不为空输出false。

以下几种情况为空：
	1、值为null 值的时候，为空
	2、值为空串的时候，为空
	3、值是Object 类型数组，长度为零的时候
	4、list 集合，元素个数为零
	5、map 集合，元素个数为零

```jsp
<body>
    <%
        // 1、值为null 值的时候，为空
        request.setAttribute("emptyNull", null);
    %>
    ${ empty emptyNull } <br/>
</body>
```

**5. “.”点运算和[] 中括号运算符**

.点运算，可以输出Bean 对象中某个属性的值。

[]中括号运算，可以输出map 集合中key 里含有特殊字符的key 的值。

```jsp
<body>
    <%
        Map<String,Object> map = new HashMap<String, Object>();
        map.put("a.a.a", "aaaValue");
        request.setAttribute("map", map);
    %>
    ${ map['a.a.a'] } <br>
</body>
```

#### 1.4 EL 表达式的11个隐含对象

EL 个达式中11 个隐含对象，是EL 表达式中自己定义的，可以直接使用。

![4.png](https://i.loli.net/2021/09/10/2fC3sBOZXVuQtIY.png)

#### 1.4.1 EL 获取四个特定域中的属性

![5.png](https://i.loli.net/2021/09/10/nNQtc4lSB9dHCJ5.png)

```jsp
<body>
    <%
        pageContext.setAttribute("key1", "pageContext1");
        pageContext.setAttribute("key2", "pageContext2");
        request.setAttribute("key2", "request");
        session.setAttribute("key2", "session");
        application.setAttribute("key2", "application");
    %>
    ${ applicationScope.key2 }
</body>
```

#### 1.4.2 pageContext 对象的使用

```jsp
<body>
    <%
		pageContext.setAttribute("req", request);
	%>
    <%=request.getScheme() %> <br>
    1.协议： ${ req.scheme }<br>
    2.服务器ip：${ pageContext.request.serverName }<br>
    3.服务器端口：${ pageContext.request.serverPort }<br>
    4.获取工程路径：${ pageContext.request.contextPath }<br>
    5.获取请求方法：${ pageContext.request.method }<br>
    6.获取客户端ip 地址：${ pageContext.request.remoteHost }<br>
    7.获取会话的id 编号：${ pageContext.session.id }<br>
</body>
```

#### 1.4.3 EL 表达式其他隐含对象的使用

**param、paramValues**

```jsp
输出请求参数username 的值：${ param.username } <br>
输出请求参数password 的值：${ param.password } <br
                                          
输出请求参数username 的值：${ paramValues.username[0]}<br>
输出请求参数hobby 的值：${ paramValues.hobby[0] } <br>
输出请求参数hobby 的值：${ paramValues.hobby[1] } <br>
```

**header、headerValues**

```jsp
输出请求头【User-Agent】的值：${ header['User-Agent'] } <br>
输出请求头【Connection】的值：${ header.Connection } <br>
输出请求头【User-Agent】的值：${ headerValues['User-Agent'][0] } <br>
```

**cookie**

```jsp
获取Cookie 的名称：${ cookie.JSESSIONID.name } <br>
获取Cookie 的值：${ cookie.JSESSIONID.value } <br>
```

**initParam**

```jsp
输出&lt;Context-param&gt;username 的值：${ initParam.username } <br>
输出&lt;Context-param&gt;url 的值：${ initParam.url } <br>
```

web.xml的配置：

```xml
<context-param>
    <param-name>username</param-name>
    <param-value>root</param-value>
</context-param>
<context-param>
    <param-name>url</param-name>
    <param-value>jdbc:mysql:///test</param-value>
</context-param>
```

### 2. JSTL 标签库

JSTL 标签库全称是指JSP Standard Tag Library JSP 标准标签库。是一个不断完善的开放源代码的JSP 标签库。
EL 表达式主要是为了替换jsp 中的表达式脚本，而标签库则是为了替换代码脚本。这样使得整个jsp 页面变得更佳简洁

 #### 2.1 JSTL 介绍

JSTL由五个不同功能的标签库组成。

![6.png](https://i.loli.net/2021/09/10/HjJF74Dez9LOUAo.png)

在jsp标签库中使用taglib 指令引入标签库

```jsp
// CORE 标签库
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
// XML 标签库
<%@ taglib prefix="x" uri="http://java.sun.com/jsp/jstl/xml" %>
// FMT 标签库
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>
// SQL 标签库
<%@ taglib prefix="sql" uri="http://java.sun.com/jsp/jstl/sql" %>
// FUNCTIONS 标签库
<%@ taglib prefix="fn" uri="http://java.sun.com/jsp/jstl/functions" %>
```

#### 2.2 JSTL 标签库的使用步骤

1、先导入jstl 标签库的jar 包。
	taglibs-standard-impl-1.2.1.jar
	taglibs-standard-spec-1.2.1.jar
2、第二步，使用taglib 指令引入标签库。

#### 2.3 core 核心库使用

#### 2.3.1 <c:set />（使用很少）

	作用：set 标签可以往域中保存数据

```jsp
<%--
i.<c:set />
    作用：set 标签可以往域中保存数据
        
    域对象.setAttribute(key,value);
    scope 属性设置保存到哪个域
        page 表示PageContext 域（默认值）
        request 表示Request 域
        session 表示Session 域
        application 表示ServletContext 域
    var 属性设置key 是多少
    value 属性设置值
--%>           
保存之前：${ sessionScope.abc } <br>
<c:set scope="session" var="abc" value="abcValue"/>
保存之后：${ sessionScope.abc } <br>
```

#### 2.3.2 \<c:if />

	if 标签用来做if 判断。

```jsp
<%--
ii.<c:if />
	if 标签用来做if 判断。
	test 属性表示判断的条件（使用EL 表达式输出）
--%>
<c:if test="${ 12 == 12 }">
	<h1>12 等于12</h1>
</c:if>
<c:if test="${ 12 != 12 }">
	<h1>12 不等于12</h1>
</c:if>
```

#### 2.3.3 \<c:choose>\<c:when>\<c:otherwise>标签

	作用：多路判断。跟switch ... case .... default 非常接近

```jsp
<%--
iii.<c:choose> <c:when> <c:otherwise>标签
	作用：多路判断。跟switch ... case .... default 非常接近
    
	choose 标签开始选择判断
	when 标签表示每一种判断情况
		test 属性表示当前这种判断情况的值
	otherwise 标签表示剩下的情况
    
<c:choose> <c:when> <c:otherwise>标签使用时需要注意的点：
	1、标签里不能使用html 注释，要使用jsp 注释
	2、when 标签的父标签一定要是choose 标签
--%>
<%
	request.setAttribute("height", 180);
%>
<c:choose>
    <%-- 这是jsp 注释--%>
    <c:when test="${ requestScope.height > 190 }">
    	<h2>小巨人</h2>
    </c:when>
    <c:when test="${ requestScope.height > 180 }">
    	<h2>很高</h2>
    </c:when>
    <c:when test="${ requestScope.height > 170 }">
    	<h2>还可以</h2>
    </c:when>
    <c:otherwise>
        <c:choose>
            <c:when test="${requestScope.height > 160}">
            	<h3>大于160</h3>
            </c:when>
            <c:when test="${requestScope.height > 150}">
            	<h3>大于150</h3>
            </c:when>
            <c:when test="${requestScope.height > 140}">
            	<h3>大于140</h3>
            </c:when>
            <c:otherwise>
                其他小于140
            </c:otherwise>
    	</c:choose>
    </c:otherwise>
</c:choose>
```

#### 2.3.4 \<c:forEach />

	作用：遍历输出使用。

```jsp
<%-- 1.遍历1 到10，输出
    begin 属性设置开始的索引
    end 属性设置结束的索引
    var 属性表示循环的变量(也是当前正在遍历到的数据)
    for (int i = 1; i < 10; i++)
--%>
<table border="1">
	<c:forEach begin="1" end="10" var="i">
        <tr>
        	<td>第${i}行</td>
        </tr>
    </c:forEach>
</table>
```

```jsp
<%-- 2.遍历Object 数组
    for (Object items : arr)
    items 表示遍历的数据源（遍历的集合）
    var 表示当前遍历到的数据
--%>
<%
	request.setAttribute("arr", new String[]{"18610541354","18688886666","18699998888"});
%>
<c:forEach items="${ requestScope.arr }" var="item">
	${ item } <br>
</c:forEach>
```

```jsp
<%
    Map<String,Object> map = new HashMap<String, Object>();
    map.put("key1", "value1");
    map.put("key2", "value2");
    map.put("key3", "value3");
    // for ( Map.Entry<String,Object> entry : map.entrySet()) {
    // }
    request.setAttribute("map", map);
%>
<c:forEach items="${ requestScope.map }" var="entry">
	<h1>${entry.key} = ${entry.value}</h1>
</c:forEach>	
```

**foreach标签所有属性**

```jsp
<%--
    items 表示遍历的集合
    var 表示遍历到的数据
    begin 表示遍历的开始索引值
    end 表示结束的索引值
    step 属性表示遍历的步长值
    varStatus 属性表示当前遍历到的数据的状态
    for（int i = 1; i < 10; i+=2）
--%>
<c:forEach begin="2" end="7" step="2" varStatus="status" items="${requestScope.stus}" var="stu">
    <tr>
        <td>${stu.id}</td>
        <td>${stu.username}</td>
        <td>${stu.password}</td>
        <td>${stu.age}</td>
        <td>${stu.phone}</td>
        <td>${status.step}</td>
    </tr>
</c:forEach>
```
