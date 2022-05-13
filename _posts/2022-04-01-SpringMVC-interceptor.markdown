---
layout: post
title:  " SpringMVC interceptor "
subtitle: 
cover-img: 
thumbnail-img:
share-img: 
date: 2022-04-01 22:56:26 +0800
tags: [Java, Spring interceptor]
---



# SpringMVC 拦截器

Spring MVC 的拦截器类似于Servlet 开发中的过滤器Filter，用于对处理器进行**预处理**和**后处理**。

将拦截器按一定的顺序联结成一条链，这条链称为**拦截器链（Interceptor Chain）**。在访问被拦截的方法或字段

时，拦截器链中的拦截器就会按其之前定义的顺序被调用。拦截器也是AOP思想的具体实现。



拦截器快速入门:

1. 创建拦截器类实现HandlerInterceptor接口

   ```java
   public class MyHandlerInterceptor1 implements HandlerInterceptor {
       public boolean preHandle(HttpServletRequest request, HttpServletResponse
   response, Object handler) {
           System.out.println("preHandle running...");
           return true;
       }
       public void postHandle(HttpServletRequest request, HttpServletResponse
   response, Object handler, ModelAndView modelAndView) {
           System.out.println("postHandle running...");
       }
       public void afterCompletion(HttpServletRequest request, HttpServletResponse
   response, Object handler, Exception ex) {
           System.out.println("afterCompletion running...");
       }
   }
   ```

   

2. 配置拦截器

   ```xml
   <!--配置拦截器-->
   <mvc:interceptors>
       <mvc:interceptor>
           <mvc:mapping path="/**"/>
           <bean class="com.belle.interceptor.MyHandlerInterceptor"/>
       </mvc:interceptor>
   </mvc:interceptors>
   ```

   

3. 测试拦截器的拦截效果

   ```java
   @RequestMapping("/belle")
   @ResponseBody
   public ModelAndViewquickMethod23() throws IOException, ParseException{
       System.out.println("目标方法执行....");
       ModelAndViewmodelAndView= new ModelAndView();
       modelAndView.addObject("name","itcast");
       modelAndView.setViewName("index");
       return modelAndView;
   }
   ```









