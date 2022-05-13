---
layout: post
title:  " HttpServletResponse、HttpServletRequest "
subtitle: 
cover-img: 
thumbnail-img:
share-img: 
date: 2022-03-12 22:56:26 +0800
tags: [Javaweb, HttpServletResponse]
---

web服务器接收到客户端的http请求，针对这个请求，分别创建一个代表请求的HttpServletRequest对象，代表响应的一个HttpServletResponse对象：

- 如果需要获取客户端请求过来的参数：找HttpServletRequest
- 如果要给客户端响应一些信息：找HttpServletResponse

## 负责向浏览器发送数据的方法

- ```java
  ServletOutputStream getOutputStream() throws IOException;
  PrintWriter getWriter() throws IOException;
  ```

响应状态码常量：

```java
    int SC_CONTINUE = 100;
    int SC_SWITCHING_PROTOCOLS = 101;
    int SC_OK = 200;
    int SC_CREATED = 201;
    int SC_ACCEPTED = 202;
    int SC_NON_AUTHORITATIVE_INFORMATION = 203;
    int SC_NO_CONTENT = 204;
    int SC_RESET_CONTENT = 205;
    int SC_PARTIAL_CONTENT = 206;
    int SC_MULTIPLE_CHOICES = 300;
    int SC_MOVED_PERMANENTLY = 301;
    int SC_MOVED_TEMPORARILY = 302;
    int SC_FOUND = 302;
    int SC_SEE_OTHER = 303;
    int SC_NOT_MODIFIED = 304;
    int SC_USE_PROXY = 305;
    int SC_TEMPORARY_REDIRECT = 307;
    int SC_BAD_REQUEST = 400;
    int SC_UNAUTHORIZED = 401;
    int SC_PAYMENT_REQUIRED = 402;
    int SC_FORBIDDEN = 403;
    int SC_NOT_FOUND = 404;
    int SC_METHOD_NOT_ALLOWED = 405;
    int SC_NOT_ACCEPTABLE = 406;
    int SC_PROXY_AUTHENTICATION_REQUIRED = 407;
    int SC_REQUEST_TIMEOUT = 408;
    int SC_CONFLICT = 409;
    int SC_GONE = 410;
    int SC_LENGTH_REQUIRED = 411;
    int SC_PRECONDITION_FAILED = 412;
    int SC_REQUEST_ENTITY_TOO_LARGE = 413;
    int SC_REQUEST_URI_TOO_LONG = 414;
    int SC_UNSUPPORTED_MEDIA_TYPE = 415;
    int SC_REQUESTED_RANGE_NOT_SATISFIABLE = 416;
    int SC_EXPECTATION_FAILED = 417;
    int SC_INTERNAL_SERVER_ERROR = 500;
    int SC_NOT_IMPLEMENTED = 501;
    int SC_BAD_GATEWAY = 502;
    int SC_SERVICE_UNAVAILABLE = 503;
    int SC_GATEWAY_TIMEOUT = 504;
    int SC_HTTP_VERSION_NOT_SUPPORTED = 505;
```



#### 2.常见应用：向浏览器输出信息、下载文件

下载文件：

```java
public class FileServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 1.获取下载文件的路径
        String realPath = "C:\\Users\\cuilo\\IdeaProjects\\javaweb-02-servlet\\Response\\target\\classes\\1.jpg";
        System.out.println(realPath);

        // 2.下载文件名获取
        System.out.println(realPath);
        String filename = realPath.substring(realPath.lastIndexOf("\\") + 1);

        // 3.使浏览器支持（Content-Disposition）下载我们需要的东西,显示为中文的情况更改编码为utf-8
        resp.setHeader("Content-Disposition","attachment;filename="+ URLEncoder.encode(filename,"UTF-8"));

        // 4.获取下载文件的输入流
        FileInputStream in = new FileInputStream(realPath);

        // 5.创建缓冲区
        int len = 0;
        byte[] buffer = new byte[1024];

        // 6.获取OutputStream对象
        ServletOutputStream out = resp.getOutputStream();

        // 7.将FileoutputStream流写入到buffer缓冲区
        while ((len = in.read(buffer))>0){
            out.write(buffer,0,len);
        }
        in.close();
        out.close();
    }
```



### 实现重定向：

一个web资源收到客户端请求后，他会通知客户端去访问另外一个web资源，这个过程叫做**重定向**

```java
void sendRedirect(String var1) throws IOException;
```

```java
@Override
protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    // * resp.setHeader("Location","/r/img");
    // * resp.setStatus(302)

    resp.sendRedirect("/img"); // 重定向
}
```

面试题：请你谈谈重定向和转发的区别：

相同点：页面都会发生跳转

不同点：请求转发的时候url不会发生变化；重定向的时候，url地址栏会发生变化











