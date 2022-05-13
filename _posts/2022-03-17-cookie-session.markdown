---
layout: post
title:  " Cookie、Session、JSP "
subtitle: 
cover-img: 
thumbnail-img:
share-img: 
date: 2022-03-17 22:56:26 +0800
tags: [Javaweb, Cookie, Session,JSP]
---

#### 保存会话的两种技术：cookie 、session (我们可以将信息或者数据保存在Session中)

```java
@Override
protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    // 服务器告知访问的时间，将这个时间封装为一个信件
    PrintWriter out = resp.getWriter();

    //Cookie:服务器端从客户端获取
    Cookie[] cookies = req.getCookies();
    // cookie 可能存在多个

    // 判断cookie是否存在
    if(cookies!=null) {
        // 如果存在怎么办
        out.write("你上一次访问的时间为：");
        for (int i = 0; i < cookies.length; i++) {
            Cookie cookie = cookies[i];
            if (cookie.getName().equals("lastLoginTime")) {
                // 获取cookie中的值
                long lastLoginTime = Long.parseLong(cookie.getValue());
                Date date = new Date(lastLoginTime);
                out.write(date.toLocaleString());
            }
        }
    }else {
        out.write("这是你第一次访问本站~");
    }

    // 服务器给客户端响应一个cookie
    Cookie cookie = new Cookie("lastLoginTime", System.currentTimeMillis() + "");
    resp.addCookie(cookie);

    // 设置cookie有效期
    cookie.setMaxAge(24*60*60);

    // 解决中文乱码问题
    req.setCharacterEncoding("UTF-8");
    resp.setCharacterEncoding("UTF-8");
    resp.setHeader("Content-type","text/html;charset=UTF-8");
}
```

cookie：一般会保存在本地的用户目录下 appdata;

#### 一个网站cookie是否存在上限

-  一个cookie只能保存一个信息
- 一个web站点可以给浏览器发送多个cookie，最多存放20个cookie
- cookie大小限制为4kb
- 300个cookie浏览器上限

#### 删除cookie:

- 不设置有效期，关闭浏览器，自动失效
- 设置有效期时间为0



## Session（重点）

- 服务器会给每个用户（浏览器）创建一个session对象
- 一个Session独占一个浏览器，只有浏览器没有关闭，这个session就存在
- 用户登录之后，整个网站都可以访问

#### Session和Cookie的区别：

- Cookie是把用户的数据写给用户的浏览器，浏览器保存（可以保存多个）
- Session把用户的数据写到用户独占的Session中，服务器端保存（保存重要的信息，减少服务器资源的浪费）
- Session对象由服务创建

#### 使用场景：

- 保存一个登录的信息
- 购物车信息
- 在整个网站中会经常使用的数据，我们将他保存在Session中

#### 会话自动过期：

```java
<session-config>
    <session-timeout>15</session-timeout>
</session-config>
```



## JSP:Java Server Pages

Java服务器端页面，也和Servlet一样，用于动态web技术

最大的特点：

-  写JSP就像在写HTML
  - HTML只给用户提供静态的数据
  - JSP页面中可以嵌入JAVA代码，为用户提供动态数据

JSP本质上就是一个Servlet:

```java
public void _jspInit() {
}// 初始化

public void _jspDestroy() {
}// 销毁

public void _jspService(final javax.servlet.http.HttpServletRequest request, final javax.servlet.http.HttpServletResponse response)
    throws java.io.IOException, javax.servlet.ServletException {// JSPService
```

1. 判断请求

2. 内置一些对象

   ```java
   final javax.servlet.jsp.PageContext pageContext;  // 页面上下文
   javax.servlet.http.HttpSession session = null; // Session
   final javax.servlet.ServletContext application;// applicationContext
   final javax.servlet.ServletConfig config;// config
   javax.servlet.jsp.JspWriter out = null;// out
   final java.lang.Object page = this; // page：当前页
   final javax.servlet.http.HttpServletRequest request  // 请求
   final javax.servlet.http.HttpServletResponse response // 响应
   ```

3. 输出页面前增加的代码

   ```java
   response.setContentType("text/html"); // 设置响应的页面类型
   pageContext = _jspxFactory.getPageContext(this, request, response,
                                             null, true, 8192, true);
   _jspx_page_context = pageContext;
   application = pageContext.getServletContext();
   config = pageContext.getServletConfig();
   session = pageContext.getSession();
   out = pageContext.getOut();
   _jspx_out = out;
   ```

4. 以上的这些对象我们可以在JSP的页面中直接使用



### JSP的基础语法：

JSP作为java技术的一种应用，Java的所有语法都支持

#### JSP表达式：

```jsp
<%--  JSP表达式作用是将程序的输出到客户端--%>
<%=new java.util.Date()%>
```

#### JSP脚本片段：

```jsp
<%
int sum = 0;
for (int i = 0; i <= 100; i++) {
    sum += i;
}
out.println("<h1>SUM="+sum+"</h1>");
%>
```



#### 脚本片段的再实现：

```jsp
<%--  在代码中嵌入HTML元素--%>
  <%
    for (int i = 0; i < 5; i++) {
  %>
      <h1>Hello,world ,number is <%=i%></h1>
  <%
      }
  %>
```



#### JSP声明：

```jsp
<%!
    static {
    System.out.println("loading Servlet!");
}
private int globalVar = 0;
public void belle(){
    System.out.println("进入方法belle");
}
%>
```

JSP声明会被编译到JSP生成的Java类种，其他的则会被生成到JSPService方法中





### JSP指令：

```jsp
<%@ page errorPage="/error/500.jsp" %>
```

### JSP标签：

```jsp
<jsp:include page="header.jsp" />
<h1>网页主题</h1>
<jsp:include page="footer.jsp" />
```



#### 九大内置对象：

- PageContext   储存东西
- Request   储存东西
- Response
- Session   储存东西
- Application (ServletContext)   存东西
- config (ServletConfig)
- out
- page
- exception

```jsp
<%--内置对象--%>
<%
    pageContext.setAttribute("n1","侯笑洋1"); // 保存的数据只在一个页面中有效
    request.setAttribute("n2","侯笑洋2");// 保存的数据只在一次请求中有效，请求转发会携带这个数据
    session.setAttribute("n3","侯笑洋3");// 保存的数据只在一次会话中有效，从打开到关闭浏览器
    application.setAttribute("n4","侯笑洋4");// 保存的数据只在服务器中有效，从打开到关闭服务器
%>
<%--    使用pageContext取出我们保存的值--%>
<%
    String n1 = (String) pageContext.findAttribute("n1");
    String n2 = (String) pageContext.findAttribute("n2");
    String n3 = (String) pageContext.findAttribute("n3");
    String n4 = (String) pageContext.findAttribute("n4");
%>
<%--使用EL表达式  ${}--%>
<h1>取出的值为：</h1>
<h3>${n1}</h3>
<h3>${n2}</h3>
<h3>${n3}</h3>
<h3>${n4}</h3>
```



request:客户端向服务器发送请求，产生的数据，用户看完就没用了，例如新闻

Session：客户端向服务器发送请求，产生的数据，用户用完还有用，购物车

application:客户端向服务器发送请求，产生的数据，一个用户用完了，其他数据还可能使用



## JSP标签、JSTL标签、EL表达式

```xml
<!-- JSTL表达式的依赖 -->
<dependency>
    <groupId>javax.servlet.jsp.jstl</groupId>
    <artifactId>jstl-api</artifactId>
    <version>1.2</version>
</dependency>
<!-- standard标签库 -->
<dependency>
    <groupId>taglibs</groupId>
    <artifactId>standard</artifactId>
    <version>1.1.2</version>
</dependency>
```

EL表达式：${}

- **获取数据**
- **执行运算**
- **获取web开发的常用对象**
- 调用Java对象

```jsp
<%--    <jsp:include page=""--%>
<jsp:forward page="jsptag2.jsp">
    <jsp:param name="name1" value="value1"/>
    <jsp:param name="name2" value="value2"/>
</jsp:forward>
```

jsptag2.jsp代码：

```jsp
<%--取出参数--%>
 1<%=request.getParameter("name1")%>
 2<%=request.getParameter("name2")%>
```



### JSTL标准标签库：

JSP标准标签库是一个JSP标签集合，它封装了JSP应用的通用核心功能，为了弥补有HTML标签的不足：他自定了许多标签可以供我们使用，标签的功能和Java代码一样。

核心标签：

```jsp
<form action="coreif.jsp" method="get"></form>
    <%--
    EL表达式获取表单中的数据
    ${param.参数名}
    --%>
<input type="text" name="username" value="${param.username}">
<input type="submit" value="登录">
```

步骤：

- 引入对应的taglib
- 使用其中的方法
- 在tomcat中也需要引入jstl的包，否则会报错：jstl解析错误

举例Java代码如下：

```java
<%
    if (request.getParameter("username").equals("admin")) {
        out.println("登录成功");
    }
%>
```

换为JSTL:

```jsp
<form action="coreif.jsp" method="get">
    <%--
    EL表达式获取表单中的数据
    ${param.参数名}
    --%>
<input type="text" name="username" value="${param.username}">
<input type="submit" value="登录">
</form>

<c:if test="${param.username=='admin'}" var="isAdmin">
    <c:out value="管理员欢迎你"/>
</c:if>
<c:out value="${isAdmin}"/>
```



举例：

```jsp
<%
    ArrayList<String> people = new ArrayList<>();
    people.add(0,"张三");
    people.add(1,"李四");
    people.add(2,"王五");
    people.add(3,"赵六");
    people.add(4,"田七");
    people.add(5,"孙八");
    request.setAttribute("list",people);
%>
<%--
var:每次遍历出来的变量
items:要遍历的集合对象
--%>
<c:forEach var="people" items="${list}">
    <c:out value="${people}"/>
</c:forEach>
<hr>
<%--
begin:   从哪里开始
end:  到哪里结束
step: 步长
--%>
<c:forEach var="people" items="${list}" begin="1" end="3" step="2">
    <c:out value="${people}"/> <br>
</c:forEach>
```



## JavaBean

实体类：特定写法：

- 必须有一个无参构造
- 属性必须私有化
- 必须有对应的get/set方法

一般用来和数据库的字段做映射  ORM

ORM:对象关系映射

- 表-->类
- 字段-->属性
- 行记录-->对象

```JSP
<jsp:useBean id="people" class="com.belle.pojo.people" scope="page"/>
<jsp:setProperty name="people" property="address" value="郑州"/>
<jsp:setProperty name="people" property="id" value="1"/>
<jsp:setProperty name="people" property="age" value="22"/>
<jsp:setProperty name="people" property="name" value="侯笑洋"/>


姓名：<jsp:getProperty name="people" property="name"/>
id:<jsp:getProperty name="people" property="id"/>
年龄：<jsp:getProperty name="people" property="age"/>
地址：<jsp:getProperty name="people" property="address"/>
```



