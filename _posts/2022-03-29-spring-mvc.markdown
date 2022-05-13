---
layout: post
title:  "SpringMVC"
subtitle: 
cover-img: 
thumbnail-img:
share-img: 
date: 2022-03-29 10:23:01 +0800
tags: [Java, SpringMVC]
---

## SpringMVC快速入门

SpringMVC是一种基于Java 的实现MVC 设计模型的请求驱动类型的轻量级Web 框架，属于SpringFrameWork的后续

产品，已经融合在Spring Web Flow 中。

SpringMVC已经成为目前最主流的MVC框架之一，并且随着Spring3.0 的发布，全面超越Struts2，成为最优秀的MVC

框架。它通过一套注解，让一个简单的Java 类成为处理请求的控制器，而无须实现任何接口。同时它还支RESTful

编程风格的请求。

### 开发步骤：

1. 导入SpringMVC相关坐标：此处需要版本为5.0.5   TomCat版本为9

   ```xml
   <!--Spring坐标-->
   <dependency>
       <groupId>org.springframework</groupId>
       <artifactId>spring-context</artifactId>			   
       <version>5.0.5.RELEASE</version>
   </dependency>
   <!--SpringMVC坐标-->
   <dependency>
       <groupId>org.springframework</groupId>
       <artifactId>spring-webmvc</artifactId>
       <version>5.0.5.RELEASE</version>
   </dependency>
   <!--Servlet坐标-->
   <dependency>
       <groupId>javax.servlet</groupId>
       <artifactId>servlet-api</artifactId>
       <version>2.5</version>
   </dependency>
   <!--Jsp坐标-->
   <dependency>
       <groupId>javax.servlet.jsp</groupId>
       <artifactId>jsp-api</artifactId>
       <version>2.0</version>
   </dependency>
   ```

   

2. 配置SpringMVC核心控制器DispathcerServlet

   ```xml
   <servlet>
       <servlet-name>DispatcherServlet</servlet-name>
       <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
       <init-param>
           <param-name>contextConfigLocation</param-name>
           <param-value>classpath:spring-mvc.xml</param-value>
       </init-param>
       <load-on-startup>1</load-on-startup>
   </servlet>
   <servlet-mapping>
       <servlet-name>DispatcherServlet</servlet-name>
       <url-pattern>/</url-pattern>
   </servlet-mapping>

3. 创建Controller类和视图页面

   ```java
   @Controller
   public class UserController {
   
       @RequestMapping(value = "/quick" ,method = RequestMethod.GET)
       public String hi(){
           System.out.println("controller hi running...");
           // 此处添加了内部资源视图解析器
           return "success";
       }
   ```

   

4. 使用注解配置Controller类中业务方法的映射地址

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>Title</title>
   </head>
   <body>
       <h1>success!!!!</h1>
   </body>
   </html>
   ```

   

5. 配置SpringMVC核心文件spring-mvc.xml

   ```xml
       <!--    Controller组件扫描-->
       <context:component-scan base-package="com.belle.controller">
           <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
       </context:component-scan>
   ```

   


### SpringMVC的执行流程

1. 用户发送请求至前端控制器DispatcherServlet。
2. DispatcherServlet收到请求调用HandlerMapping处理器映射器。
3. 处理器映射器找到具体的处理器(可以根据xml配置、注解进行查找)，生成处理器对象及处理器拦截器(如果有则生成)一并返回给DispatcherServlet。
4. DispatcherServlet调用HandlerAdapter处理器适配器。
5. HandlerAdapter经过适配调用具体的处理器(Controller，也叫后端控制器)。
6. Controller执行完成返回ModelAndView。
7. HandlerAdapter将controller执行结果ModelAndView返回给DispatcherServlet。
8. DispatcherServlet将ModelAndView传给ViewReslover视图解析器。
9. ViewReslover解析后返回具体View。
10. DispatcherServlet根据View进行渲染视图（即将模型数据填充至视图中）。DispatcherServlet响应用户。



### SpringMVC组件解析

1. **前端控制器：DispatcherServlet**
用户请求到达前端控制器，它就相当于MVC 模式中的C，DispatcherServlet是整个流程控制的中心，由
它调用其它组件处理用户的请求，DispatcherServlet的存在降低了组件之间的耦合性。
2. **处理器映射器：HandlerMapping**
HandlerMapping负责根据用户请求找到Handler 即处理器，SpringMVC提供了不同的映射器实现不同的
映射方式，例如：配置文件方式，实现接口方式，注解方式等。
3. **处理器适配器：HandlerAdapter**
通过HandlerAdapter对处理器进行执行，这是适配器模式的应用，通过扩展适配器可以对更多类型的处理
器进行执行。
4. **处理器：Handler**
  它就是我们开发中要编写的具体业务控制器。由DispatcherServlet把用户请求转发到Handler。由
  Handler 对具体的用户请求进行处理。
5. **视图解析器：View Resolver**
View Resolver 负责将处理结果生成View 视图，View Resolver 首先根据逻辑视图名解析成物理视图名，即具体的页面地址，再生成View 视图对象，最后对View 进行渲染将处理结果通过页面展示给用户。
6. **视图：View**
SpringMVC框架提供了很多的View 视图类型的支持，包括：jstlView、freemarkerView、pdfView等。最常用的视图就是jsp。一般情况下需要通过页面标签或页面模版技术将模型数据通过页面展示给用户，需要由程序员根据业务需求开发具体的页面

### SpringMVC注解解析



`@RequestMapping`
作用：用于建立请求URL 和处理请求方法之间的对应关系

位置：

- 类上，请求URL 的第一级访问目录。此处不写的话，就相当于应用的根目录

- 方法上，请求URL 的第二级访问目录，与类上的使用@ReqquestMapping标注的一级目录一起组成访问虚拟路径属性：

  - value：用于指定请求的URL。它和path属性的作用是一样的

  - method：用于指定请求的方式

  - params：用于指定限制请求参数的条件。它支持简单的表达式。要求请求参数的key和value必须和配置

    一模一样例如：

    - params= {"accountName"}，表示请求参数必须有accountName
    - params= {"moeny!100"}，表示请求参数中money不能是100



### mvc命名空间引入

命名空间：xmlns:context="http://www.springframework.org/schema/context"

xmlns:mvc="http://www.springframework.org/schema/mvc"

约束地址：http://www.springframework.org/schema/context

http://www.springframework.org/schema/context/spring-context.xsd

http://www.springframework.org/schema/mvc

http://www.springframework.org/schema/mvc/spring-mvc.xsd

### SpringMVC的相关组件

- 前端控制器：DispatcherServlet
- 处理器映射器：HandlerMapping
- 处理器适配器：HandlerAdapter
- 处理器：Handler
- 视图解析器：View Resolver
- 视图：View

### SpringMVC的注解和配置

- 请求映射注解：@RequestMapping

- 视图解析器配置：

  REDIRECT_URL_PREFIX = "redirect:"

  FORWARD_URL_PREFIX = "forward:"

  prefix = "";

  suffix = "";



