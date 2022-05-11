---
layout: post
title:  "Springmvc"
author: "Clyde"
tags: springmvc
excerpt_separator: <!--more-->
---

Springmvc is a request-driven lightweight Web framework based on Java that implements the MVC design model. It is a follow-up product of SpringFrameWork and has been integrated into Spring Web Flow.<!--more-->SpringMVC has become one of the most mainstream MVC frameworks at present, and with the release of Spring3.0, it has surpassed Struts2 in an all-round way and become the best MVC framework. It uses a set of annotations to make a simple Java class a controller for handling requests without having to implement any interface. It also supports restful programming style requests.

### Development steps:

1. Import SpringMVC-related coordinates:   Notice:**TomCat version 9**

   ```xml
   <!--Spring-->
   <dependency>
       <groupId>org.springframework</groupId>
       <artifactId>spring-context</artifactId>			   
       <version>5.0.5.RELEASE</version>
   </dependency>
   <!--SpringMVC-->
   <dependency>
       <groupId>org.springframework</groupId>
       <artifactId>spring-webmvc</artifactId>
       <version>5.0.5.RELEASE</version>
   </dependency>
   <!--Servlet-->
   <dependency>
       <groupId>javax.servlet</groupId>
       <artifactId>servlet-api</artifactId>
       <version>2.5</version>
   </dependency>
   <!--Jsp-->
   <dependency>
       <groupId>javax.servlet.jsp</groupId>
       <artifactId>jsp-api</artifactId>
       <version>2.0</version>
   </dependency>
   ```

2. Configure the SpringMVC core controller **DispathcerServlet**:

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

3. Create Controller class and view page:

   ```java
   @Controller
   public class UserController {
   
       @RequestMapping(value = "/quick" ,method = RequestMethod.GET)
       public String hi(){
           System.out.println("controller hi running...");
           // Added internal resource view resolver here
           return "success";
       }
   ```

4. Use annotations to configure the mapping address of business methods in the Controller class:

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

5. Configure the SpringMVC core file spring-mvc.xml:

   ```xml
   <!--Controller component scan-->
   <context:component-scan base-package="com.belle.controller">
       <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
   </context:component-scan>
   ```



### The execution flow of SpringMVC:

1. The user *sends a request* to the front controller DispatcherServlet.
2. The DispatcherServlet *receives the request* and calls the HandlerMapping handler mapper.
3. The processor mapper finds a specific processor (which can be searched according to xml configuration and annotations), *generates a processor object* and a processor interceptor (if any) and *returns it to DispatcherServlet*.
4. DispatcherServlet *calls* HandlerAdapter handler adapter.
5. HandlerAdapter is adapted to *call a specific handler* (Controller, also called a back-end controller).
6. Controller execution completes and *returns to* ModelAndView.
7. HandlerAdapter *returns* the controller execution result ModelAndView to DispatcherServlet.
8. DispatcherServlet *passes ModelAndView to* ViewReslover view resolver.
9. ViewReslover returns a specific View after parsing.
10. DispatcherServlet *renders the view according* to the View (that is, fills the model data into the view). The DispatcherServlet responds to the user.

### SpringMVC component analysis

1. **DispatcherServlet**:When the user request arrives at the front-end controller, it is equivalent to C in the MVC pattern. DispatcherServlet is the center of the entire process control. It calls other components to process user requests, and the existence of DispatcherServlet reduces the coupling between components.
2. **HandlerMapping**:HandlerMapping is responsible for finding the Handler according to the user's request, that is, the processor. SpringMVC provides different mappers to implement different Mapping method, such as: configuration file method, implementation interface method, annotation method, etc.
3. **HandlerAdapter**:The processor is executed through the HandlerAdapter, which is the application of the adapter mode. More types of processing can be performed by extending the adapter.device to execute.
4. **Handler**:It is the specific business controller to be written in our development. The user request is forwarded to the Handler by the DispatcherServlet. Depend on Handler handles specific user requests.
5. **View Resolver**:The View Resolver is responsible for generating the View view from the processing result. The View Resolver first resolves the logical view name into the physical view name, that is, the specific page address, then generates the View view object, and finally renders the View to display the processing result to the user through the page.
6. **View**:The SpringMVC framework provides support for many View view types, including: jstlView, freemarkerView, pdfView, etc. The most commonly used view is jsp. In general, model data needs to be displayed to users through page tags or page template technology, and specific pages need to be developed by programmers according to business needs.



