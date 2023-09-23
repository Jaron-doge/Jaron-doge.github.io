---
title: 3 Servlet
date: 2021-9-3 00:00:00
tags: 
categories: JAVA EE
---

## Servlet

### 1. Servlet配置

<!-- more -->

**web.xml中的配置**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
version="4.0">
<!-- servlet 标签给Tomcat 配置Servlet 程序-->
<servlet>
<!--servlet-name 标签Servlet 程序起一个别名（一般是类名） -->
<servlet-name>HelloServlet</servlet-name>
<!--servlet-class 是Servlet 程序的全类名-->
<servlet-class>com.atguigu.servlet.HelloServlet</servlet-class>
</servlet>
<!--servlet-mapping 标签给servlet 程序配置访问地址-->
<servlet-mapping>
<!--servlet-name 标签的作用是告诉服务器，我当前配置的地址给哪个Servlet 程序使用-->
<servlet-name>HelloServlet</servlet-name>
<!--url-pattern 标签配置访问地址
/ 斜杠在服务器解析的时候，表示地址为：http://ip:port/工程路径
/hello 表示地址为：http://ip:port/工程路径/hello 
-->
<url-pattern>/hello</url-pattern>
</servlet-mapping>
</web-app>
```

**从Servlet3.0开始，配置Servlet支持注解方式**

@WebServlet注解用于标注在一个继承了HttpServlet类之上，属于类级别的注解。

![img](https://img2018.cnblogs.com/i-beta/1913588/202001/1913588-20200103165022431-1582263216.png)

```servlet
@WebServlet(name = "helloServlet", value = "/hello-servlet")
```

![1.png](https://i.loli.net/2021/09/10/qnEHNAzlDR6wQji.png)

### 2. Servlet的生命周期

1. 执行Servlet 构造器方法
2. 执行init 初始化方法
3. 执行service 方法
4. 执行destroy 销毁方法

- 第一、二步，是在第一次访问，的时候创建Servlet 程序会调用。

- 第三步，每次访问都会调用。

- 第四步，在web 工程停止的时候调用。

### 3. GET和POST请求的处理

**1. 将servletRequest转换成HttpServletRequest并处理**

```java
public class HelloServlet implements Servlet {
    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws
    ServletException, IOException {
        System.out.println("3 service === Hello Servlet 被访问了");
        // 类型转换（因为它有getMethod()方法）
        HttpServletRequest httpServletRequest = (HttpServletRequest) servletRequest;
        // 获取请求的方式
        String method = httpServletRequest.getMethod();
        if ("GET".equals(method)) {
        doGet();
        } else if ("POST".equals(method)) {
        doPost();
        }
    }
}
```

**2. 通过继承HttpServlet类实现Servlet程序**
一般在实际项目开发中，都是使用继承HttpServlet 类的方式去实现Servlet 程序。
1、编写一个类去继承HttpServlet 类
2、根据业务需要重写doGet 或doPost 方法
3、到web.xml 中的配置Servlet 程序的访问地址

```java
public class HelloServlet2 extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
    IOException {
        System.out.println("HelloServlet2 的doGet 方法");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
    IOException {
        System.out.println("HelloServlet2 的doPost 方法");
    }
}
```

### 4. Servlet类的继承体系

![2.png](https://i.loli.net/2021/09/10/sXMGemVBli72vKH.png)

### 5. ServletConfig类

Servlet 程序和ServletConfig 对象都是由Tomcat 负责创建，我们负责使用。
Servlet 程序默认是第一次访问的时候创建，ServletConfig 是每个Servlet 程序创建时，就创建一个对应的ServletConfig 对象。

#### 5.1 ServletConfig类的三大作用

1、可以获取Servlet 程序的别名servlet-name 的值
2、获取初始化参数init-param
3、获取ServletContext 对象

```xml
<servlet>
    <init-param>
    <!--是参数名-->
    <param-name>username</param-name>
    <!--是参数值-->
    <param-value>root</param-value>
    </init-param>
</servlet>
```

```java
// 1、可以获取Servlet 程序的别名servlet-name 的值
System.out.println("HelloServlet 程序的别名是:" + servletConfig.getServletName());
// 2、获取初始化参数init-param
System.out.println("初始化参数username 的值是;" + servletConfig.getInitParameter("username"));
System.out.println("初始化参数url 的值是;" + servletConfig.getInitParameter("url"));
// 3、获取ServletContext 对象
System.out.println(servletConfig.getServletContext());
```

### 6. ServletContext 类

1、ServletContext 是一个接口，它表示Servlet 上下文对象
2、一个web 工程，只有一个ServletContext 对象实例。
3、ServletContext 对象是一个域对象。
4、ServletContext 是在web 工程部署启动的时候创建。在web 工程停止的时候销毁。

#### 6.1 ServletContext 类的四个作用

1、获取web.xml 中配置的上下文参数context-param
2、获取当前的工程路径，格式: /工程路径
3、获取工程部署后在服务器硬盘上的绝对路径
4、像Map 一样存取数据

```xml
<!--context-param 是上下文参数(它属于整个web 工程)-->
<context-param>
    <param-name>username</param-name>
    <param-value>context</param-value>
</context-param>
<!--context-param 是上下文参数(它属于整个web 工程)-->
<context-param>
    <param-name>password</param-name>
    <param-value>root</param-value>
</context-param>
```

```java
// 1、获取web.xml 中配置的上下文参数context-param
ServletContext context = getServletConfig().getServletContext();
String username = context.getInitParameter("username");

// 2、获取当前的工程路径，格式: /工程路径
System.out.println( "当前工程路径:" + context.getContextPath() );

// 3、获取工程部署后在服务器硬盘上的绝对路径
System.out.println("工程部署的路径是:" + context.getRealPath("/"));
System.out.println("工程下css 目录的绝对路径是:" + context.getRealPath("/css"));
System.out.println("工程下imgs 目录1.jpg 的绝对路径是:" + context.getRealPath("/imgs/1.jpg"));
```

#### 6.2 ServletContext 像Map 一样存取数据

可以在两个Servlet间传递数据

```java
// Servlet1
// 获取ServletContext 对象
ServletContext context = getServletContext();
context.setAttribute("key1", "value1");
System.out.println("Context1 中获取域数据key1 的值是:"+ context.getAttribute("key1"));
```

```java
// Servlet2
ServletContext context = getServletContext();
System.out.println("Context2 中获取域数据key1 的值是:"+ context.getAttribute("key1"));
```

### 7. HTTP 协议

#### 7.1 GET请求

**请求行**

(1) 请求的方式GET
(2) 请求的资源路径[+?+请求参数]
(3) 请求的协议的版本号HTTP/1.1

**请求头**

key : value 组成不同的键值对，表示不同的含义。

![3.png](https://i.loli.net/2021/09/10/vJIQiENUH74SmyR.png)

#### 7.2 POST请求

**请求行**

(1) 请求的方式GET
(2) 请求的资源路径[+?+请求参数]
(3) 请求的协议的版本号HTTP/1.1

**请求头**

1) key : value 不同的请求头，有不同的含义
空行

- 常用请求头的说明
  - Accept: 表示客户端可以接收的数据类型
  - Accpet-Languege: 表示客户端可以接收的语言类型
  - User-Agent: 表示客户端浏览器的信息
  - Host： 表示请求时的服务器ip 和端口号

**请求体**

​	发送给服务器的数据

![4.png](https://i.loli.net/2021/09/10/W89cSpw1yNVnhmM.png)

#### 7.3 GET请求和POST请求的方式

**GET请求**

1、form 标签method=get
2、a 标签
3、link 标签引入css
4、Script 标签引入js 文件
5、img 标签引入图片
6、iframe 引入html 页面
7、在浏览器地址栏中输入地址后敲回车

**POST请求**

8、form 标签method=post

#### 7.4 Get 和 Post的区别

1、Get 是用来从服务器上获得数据，而 Post 是用来向服务器上传递数据。

2、Get 将表单中数据的按照 variable=value 的形式，添加到 action 所指向的 URL 后面，并且两者使用“?”连接，而各个变量之间使用“&”连接；Post 是将表单中的数据放在 form 的数据体中，按照变量和值相对应的方式，传递到 action 所指向 URL。

3、Get 是不安全的，因为在传输过程，数据被放在请求的 URL 中，而如今现有的很多服务器、代理服务器或者用户代理都会将请求URL记录到日志文件中，然后放在某个地方，这样就可能会有一些隐私的信息被第三方看到。另外，用户也可以在浏览器上直接看到提交的数据，一些系统内部消息将会一同显示在用户面前。Post 的所有操作对用户来说都是不可见的。

4、Get 传输的数据量小，这主要是因为受 URL 长度限制；而 Post 可以传输大量的数据，所以在上传文件只能使用 Post（当然还有一个原因，将在后面的提到）。

5、Get 限制 Form 表单的数据集的值必须为 ASCII 字符；而 Post 支持整个 ISO10646 字符集。

6、Get 是 Form 的默认方法。

#### 7.5 响应的HTTP协议格式

**响应行**

(1) 响应的协议和版本号
(2) 响应状态码
(3) 响应状态描述符

**响应头**

(1) key : value 不同的响应头，有其不同含义
空行

**响应体**

​	是回传给客户端的数据

![5.png](https://i.loli.net/2021/09/10/PCEh8pByo5agbDW.png)

#### 7.5.1 常用的响应码

200 	表示请求成功
302 	表示请求重定向
404 	表示请求服务器已经收到了，但是你要的数据不存在（请求地址错误）
500 	表示服务器已经收到请求，但是服务器内部错误（代码错误）

### 8. HttpServletRequest 类

每次只要有请求进入Tomcat 服务器，Tomcat 服务器就会把请求过来的HTTP 协议信息解析好封装到Request 对象中。然后传递到service 方法（doGet 和doPost）中给我们使用。我们可以通过HttpServletRequest 对象，获取到所有请求的信息。

#### 8.1 HttpServletRequest 类常用方法

![6.png](https://i.loli.net/2021/09/10/GzduneiY7jEQ2cp.png)

**request.setAttribute()**

有效范围是一个请求范围，不发送请求的界面无法获取到value的值，jsp界面获取使用EL表达式${num}；
只能在一个request内有效，如果重定向客户端，将取不到值。

request在当次的请求的URL之间有效，比如，你在请求某个servlet，那么你提交的信息，可以使用request.getAttribute()方式获得，而当你再次跳转之后，这些信息将不存在。

**request.getSession().setAttribute(“num”,value)；**

有效范围是一个session周期，在session过期之前或者用户关闭页面之前是有效的，jsp界面获取使用EL表达式${num}；

可以通过sessionID得到自己的session，将参数存储在session中，即使重定向客户端也没事，这个值可以在多个页面上使用。

比如访问一个网站，登录后用户信息被保存到session中，在session过期之前或者用户关闭页面之前，用户信息可以通过request.getSession().getAttribute()方式获得。

#### 8.2 获取请求参数

表单

```jsp
<body>
    <form action="http://localhost:8080/07_servlet/parameterServlet" method="get">
        用户名：<input type="text" name="username"><br/>
        密码：<input type="password" name="password"><br/>
        兴趣爱好：<input type="checkbox" name="hobby" value="cpp">C++
        <input type="checkbox" name="hobby" value="java">Java
        <input type="checkbox" name="hobby" value="js">JavaScript<br/>
        <input type="submit">
    </form>
</body>
```

```java
public class ParameterServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
    IOException {
        // 防止中文乱码
     	response.setContentType("text/html; charset=utf-8");   
        
    // 获取请求参数
        String username = req.getParameter("username");
        String password = req.getParameter("password");
        String[] hobby = req.getParameterValues("hobby");
        
        System.out.println("用户名：" + username);
        System.out.println("密码：" + password);
        System.out.println("兴趣爱好：" + Arrays.asList(hobby));
    }
}
```

#### 8.3 乱码问题

**doGet 请求的中文乱码解决**

```java
// 获取请求参数
String username = req.getParameter("username");

//1 先以iso8859-1 进行编码
//2 再以utf-8 进行解码
username = new String(username.getBytes("iso-8859-1"), "UTF-8");
```

**POST 请求的中文乱码解决**

```java
// 设置请求体的字符集为UTF-8，从而解决post 请求的中文乱码问题
req.setCharacterEncoding("UTF-8");
```

### 9. 请求转发

请求转发 是指服务器收到请求后，从一次资源跳转到另一个资源的操作。

![7.png](https://i.loli.net/2021/09/10/y73DuIRfL8mFM9K.png)

Servlet1代码：

```java
// 获取请求的参数（办事的材料）查看
String username = req.getParameter("username");
System.out.println("在Servlet1（柜台1）中查看参数（材料）：" + username);
// 给材料盖一个章，并传递到Servlet2（柜台2）去查看
req.setAttribute("key1","柜台1 的章");
// 问路：Servlet2（柜台2）怎么走
/*** 请求转发必须要以斜杠打头，/ 斜杠表示地址为：http://ip:port/工程名/ , 映射到IDEA 代码的web 目录**/
RequestDispatcher requestDispatcher = req.getRequestDispatcher("/servlet2");
// RequestDispatcher requestDispatcher = req.getRequestDispatcher("http://www.baidu.com");
// 走向Sevlet2（柜台2）
requestDispatcher.forward(req,resp);
```

Servlet2代码：

```java
// 获取请求的参数（办事的材料）查看
String username = req.getParameter("username");
System.out.println("在Servlet2（柜台2）中查看参数（材料）：" + username);
// 查看柜台1 是否有盖章
Object key1 = req.getAttribute("key1");
System.out.println("柜台1 是否有章：" + key1);
// 处理自己的业务
System.out.println("Servlet2 处理自己的业务");
```

**base标签**

![8.png](https://i.loli.net/2021/09/10/iHQR4qzOXCB5dM6.png)

```html
<!--base 标签设置页面相对路径工作时参照的地址 href 属性就是参数的地址值-->
<base href="http://localhost:8080/07_servlet/a/b/">
```

### 10. / 的意义

在web 中/ 斜杠是一种绝对路径。

/ 斜杠如果被浏览器解析，得到的地址是：http://ip:port/

```html
<a href="/">斜杠</a>
```

/ 斜杠如果被服务器解析，得到的地址是：http://ip:port/工程路径

```java
1、servletContext.getRealPath(“/”);
2、request.getRequestDispatcher(“/”);
```

特殊情况

把斜杠发给浏览器解析，得到http://ip:port/

```java
response.sendRedirect("/")
```

### 11. HttpServletResponse 类

HttpServletResponse 类和HttpServletRequest 类一样。每次请求进来，Tomcat 服务器都会创建一个Response 对象传递给Servlet 程序去使用。HttpServletRequest 表示请求过来的信息，HttpServletResponse 表示所有响应的信息，我们如果需要设置返回给客户端的信息，都可以通过HttpServletResponse 对象来进行设置

#### 11.1 两个输出流

![9.png](https://i.loli.net/2021/09/10/UruGaK384NPDJsb.png)

两个流同时只能使用一个

#### 11.2 向客户端回传数据

```java
PrintWriter writer = resp.getWriter();
writer.write("response's content!!!");
```

#### 11.3 解决响应中文乱码

```java
// 它会同时设置服务器和客户端都使用UTF-8 字符集，还设置了响应头
// 此方法一定要在获取流对象之前调用才有效
resp.setContentType("text/html; charset=UTF-8");
```

### 12. 请求重定向

请求重定向，是指客户端给服务器发请求，然后服务器告诉客户端说。我给你一些地址。你去新地址访问。叫请求重定向（因为之前的地址可能已经被废弃）。

![10.png](https://i.loli.net/2021/09/10/eOliZKmQuk5HJpt.png)

#### 12.1 第一种方案

```java
// 设置响应状态码302 ，表示重定向，（已搬迁）
resp.setStatus(302);
// 设置响应头，说明新的地址在哪里
resp.setHeader("Location", "http://localhost:8080");
```

#### 12.2 第二种方案（推荐）

```java
resp.sendRedirect("http://localhost:8080");
```

#### 12.3 转发和重定向的区别

1）使用相对路径在重定向和转发中没有区别
2）重定向和请求转发使用绝对路径时，根/路径代表了不同含义
**重定向**response.sendRedirect("xxx")是服务器向客户端发送一个请求头信息，由客户端再请求一次服务器。/指的Tomcat的根目录,写绝对路径应该写成"/当前Web程序根名称/资源名" 。如"/WebModule/login.jsp","/bbs/servlet/LoginServlet"

**转发**是在服务器内部进行的，写绝对路径/开头指的是当前的Web应用程序。绝对路径写法就是"/login.jsp"或"/servlet/LoginServlet"。

**总结**：以上要注意是区分是从服务器外的请求，还在是内部转发，从服务器外的请求，从Tomcat根写起(就是要包括当前Web的根)；是服务器内部的转发，很简单了，因为在当前服务器内，/写起指的就是当前Web的根目录。

**注意**

假如一开始访问到的是http://localhost:8080/myProject/index.html , 然后该页面通过GET请求访问到对应的Servlet, 此时重定向的相对路径不是相对于Servlet而言的，而是相对于原先请求路径而言（或者说相对于用户浏览器上面显示的页面地址而言），即相对于：
http://localhost:8080/myProject/index.html 而言。

