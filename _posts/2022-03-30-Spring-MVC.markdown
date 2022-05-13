---
layout: post
title:  "SpringMVC数据响应"
subtitle: 
cover-img: 
thumbnail-img:
share-img: 
date: 2022-03-30 10:23:01 +0800
tags: [Java, SpringMVC]
---

## SpringMVC的数据响应方式

### 1. 页面跳转

#### 1.1直接返回字符串

直接返回字符串：此种方式会将返回的字符串与视图解析器的前后缀拼接后跳转。

以下为UserController代码：

```java
@RequestMapping(value = "/quick" ,method = RequestMethod.GET)
public String hi(){
    System.out.println("controller hi running...");
    // 此处添加了内部资源视图解析器
    return "success";
}
```

以下为spring-mvc.xml的配置

```xml
<!--    配置内部资源视图解析器-->
<bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <property name="prefix" value="/WEB-INF/jsp/"/>
    <property name="suffix" value=".jsp"/>
</bean>
```

二者结合：**转发**资源地址为：`/WEB-INF/jsp/success.jsp`  

**重定向**：`redirect:/success.jsp`

#### 1.2  通过ModelAndView对象返回

```java
@RequestMapping(value = "/hello")
public ModelAndView hello(){
    ModelAndView modelAndView = new ModelAndView();
    /*   Model模型：作用是封装数据
         View视图：作用展示数据    */
    // 设置视图名称
    modelAndView.setViewName("index");
    // 设置模型数据
    modelAndView.addObject("username","belle");
    return modelAndView;
}
```

或者如下：

```java
@RequestMapping(value = "/hi")
public ModelAndView hi(ModelAndView modelAndView){
    modelAndView.setViewName("index");
    modelAndView.addObject("username","clyde");
    return modelAndView;
}
```



我们在jsp页面中通过`${}` 方式获取username对应的值

```jsp
<html>
  <head>
    <title>$Title$</title>
  </head>
  <body>
    <h1>Hello,world</h1>
    <h4>From web/jsp/index.jsp</h4>
    <h1>${username}</h1>
  </body>
</html>
```



### 2.回写数据

#### 2.1直接返回字符串

客户端访问服务器端，如果想直接回写字符串作为响应体返回的话，只需要使用

`response.getWriter().print(“hello world”)` 即可，在Controller中采取如下办法

- 通过SpringMVC框架注入的response对象，使用response.getWriter().print(“hello world”) 回写数据，此时不需要

  视图跳转，业务方法返回值为void。

  ```java
  @RequestMapping(value = "/five")
  public void five(HttpServletResponse response) throws IOException {
      response.getWriter().println("hello,world");
  }
  ```

- 将需要回写的字符串直接返回，但此时需要通过`@ResponseBody`注解告知SpringMVC框架，方法返回的字符

  串**不是跳转**是直接在http响应体中返回。**（重点！！！）**

  ```java
  @RequestMapping(value = "/seven")
  @ResponseBody  // 告知SpringMVC不进行视图跳转，直接进行数据响应
  public String seven() throws IOException {
      return "hello,Spring";
  }
  ```


#### 2.2返回对象或集合

  在异步项目中，客户端与服务器端往往要进行json格式字符串交互，此时我们可以手动拼接json字符串返回。

  ```java
  @RequestMapping(value = "/eight")
  @ResponseBody
  public String eight() throws IOException {
  return "{\"username\":\"belle\",\"age\":18}";
  }
  ```

  以上的办法，在数据量大时，拼接效率及其低下，我们采用第三方工具：

  ```xml
  <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-core</artifactId>
      <version>2.9.0</version>
  </dependency>
  <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-databind</artifactId>
      <version>2.9.0</version>
  </dependency>
  <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-annotations</artifactId>
      <version>2.9.0</version>
  </dependency>
  ```

  ```java
  @RequestMapping(value = "/nine")
  @ResponseBody
  public String nine() throws IOException {
      User user = new User();
      user.setUsername("clyde");
      user.setAge(18);
      // 使用json转换工具，将对象转换为json格式的字符串在返回
      ObjectMapper objectMapper = new ObjectMapper();
      String json = objectMapper.writeValueAsString(user);
      return json;
  }
  ```

  **MVC注解驱动**

  以上的办法需要引入json转换工具，我们可以让SpringMVC来做这些工作

  在方法上添加@ResponseBody就可以返回json格式的字符串，但是这样配置比较麻烦，配置

  代码比较多，因此，我们可以使用mvc的注解驱动代替上述配置。

  ```xml
  <!--    mvc注解驱动-->
  <mvc:annotation-driven/>
  ```

  

## SpringMVC获得请求数据

客户端请求参数的格式是：`name=value&name=value… …`

服务器端要获得请求的参数，有时还需要进行数据的封装，SpringMVC可以接收如下类型的参数：

- 基本类型参数
- POJO类型参数
- 数组类型参数
- 集合类型参数

### 1.获得基本类型参数

Controller中的业务方法的参数名称要与请求参数的name一致，参数值会自动映射匹配。

```java
@RequestMapping(value = "/eleven")
@ResponseBody
public void eleven(String username, int age) throws IOException {
    System.out.println(username);
    System.out.println(age);
}
```

浏览器输入：`http://localhost:8080/eleven?username=belle&age=18`

控制台会输出 `belle  18`

### 2.获得POJO类型参数

Controller中的业务方法的POJO参数的属性名与请求参数的name一致，参数值会自动映射匹配。

```java
@RequestMapping(value = "/twelve")
@ResponseBody
public void twelve(User user) throws IOException {
    System.out.println(user);
}
```

浏览器输入：`http://localhost:8080/twelve?username=belle&age=18`

控制台输出：`User{username='belle', age=18}`

### 3.获得数组类型参数

Controller中的业务方法数组名称与请求参数的name一致，参数值会自动映射匹配。

```java
@RequestMapping(value = "/thirteen")
@ResponseBody
public void thirteen(String[] strings) throws IOException {
    System.out.println(Arrays.asList(strings));
}
```

浏览器输入：`http://localhost:8080/thirteen?strings=111&strings=222&strings=333`

控制台输出：`[111, 222, 333]`

### 4.获得集合类型参数

获得集合参数时，要将集合参数包装到一个POJO中才可以。

```java
@RequestMapping(value = "/fourteen")
@ResponseBody
public void fourteen(VO vo) throws IOException {
    System.out.println(vo);
}
```

以上的VO对象为：

```java
public class VO {
    private List<User> userList;
```

```jsp
<body>
    <form action="${pageContext.request.contextPath}/fourteen" method="post">
        <%--  表明时第几个User对象的username age--%>
        <input type="text" name="userList[0].username"><br/>
        <input type="text" name="userList[0].age"><br/>
        <input type="text" name="userList[1].username"><br/>
        <input type="text" name="userList[1].age"><br/>
        <input type="submit" value="提交">
    </form>
</body>
```

以上列表输入后：控制台打印：

`VO{userList=[User{username='belle', age=12}, User{username='clyde', age=55}]}`

当使用ajax提交时，可以指定contentType为json形式，那么在方法参数位置使用@RequestBody可以直接接收集合

数据而无需使用POJO进行包装。

```jsp
<title>Title</title>
<script src="${pageContext.request.contextPath}/js/jquery-3.6.0.js"></script>
<script>
    var userList = new Array();
    userList.push({username:"belle",age:18});
    userList.push({username:"clyde",age:19});

    $.ajax({
        type:"POST",
        url:"${pageContext.request.contextPath}/fiveteen",
        data:JSON.stringify(userList),
        contentType:"application/json;charset=utf-8"
    })
</script>
```

```java
@RequestMapping(value = "/fiveteen")
@ResponseBody
public void fiveteen(@RequestBody List<User>userList) throws IOException {
    System.out.println(userList);
}
```

控制台输出：`[User{username='belle', age=18}, User{username='clyde', age=19}]`

此处存在jquery文件无法找到的错误，在springmvc.xml中添加如下代码：

`<mvc:resources mapping="/js/**" location="/js/"/>`或者：

`<mvc:default-servlet-handler/>` 推荐！

### 5.请求数据乱码问题

当post请求时，数据会出现乱码，我们可以设置一个过滤器来进行编码的过滤。

```xml
<!--    配置全局过滤-->
<filter>
    <filter-name>characterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>characterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

### 6.参数绑定注解@requestParam

当请求的参数名称与Controller的业务方法参数名称不一致时，就需要通过@RequestParam注解显示的绑定

```java
@RequestMapping(value = "/sixteen")
@ResponseBody
public void sixteen(@RequestParam(value = "name") String username) throws IOException {
    System.out.println(username);
}
```

注解@RequestParam还有如下参数可以使用：

- value：与请求参数名称
- required：此在指定的请求参数是否必须包括，默认是true，提交时如果没有此参数则报错
- defaultValue：当没有指定请求参数时，则使用指定的默认值赋值

```java
@RequestMapping(value = "/sixteen")
@ResponseBody
public void sixteen(@RequestParam(value = "name",required = false,defaultValue = "default") String username) throws IOException {
    System.out.println(username);
}
```



### 7.获得Restful风格的参数

Restful是一种软件**架构风格、设计风格**，而不是标准，只是提供了一组设计原则和约束条件。主要用于客户端和

服务器交互类的软件，基于这个风格设计的软件可以更简洁，更有层次，更易于实现缓存机制等。

Restful风格的请求是使用“url+请求方式”表示一次请求目的的，HTTP 协议里面四个表示操作方式的动词如下

- GET：用于获取资源
- POST：用于新建资源
- PUT：用于更新资源
- DELETE：用于删除资源

通过这种方法可以获取地址内部的参数：`@PathVariable`

```java
// localhost:8080/seventeen/zhangsan
@RequestMapping(value = "/seventeen/{username}")
@ResponseBody
public void seventeen(@PathVariable(value = "username") String username) throws IOException {
    System.out.println(username);
}
```

控制台输出：`zhangsan`   

### 8.自定义类型转换器

- SpringMVC默认已经提供了一些常用的类型转换器，例如客户端提交的字符串转换成int型进行参数设置。
- 但是不是所有的数据类型都提供了转换器，没有提供的就需要自定义转换器，例如：日期类型的数据就需要自定义转换器。

#### 自定义类型转换器的开发步骤：

1. 定义转换器类实现Converter接口
2. 在配置文件中声明转换器
3. 在`<annotation-driven>`中引用转换器



```java
public class DataConverter implements Converter<String,Date> {
    public Date convert(String dateStr)  {
        // 将日期的字符串转换为真正的对象 并返回
        SimpleDateFormat format = new SimpleDateFormat("yyyy-MM-dd");
        Date date = null;
        try {
            date = format.parse(dateStr);
        } catch (ParseException e) {
            e.printStackTrace();
        }
        return date;
    }
```

进行声明转换器设置：

```xml
<!--    声明转换器-->
<bean id="conversionService" class="org.springframework.context.support.ConversionServiceFactoryBean">
    <property name="converters">
        <list>
            <bean class="com.belle.converter.DataConverter"/>
        </list>
    </property>
</bean>
```

```xml
<!--    mvc注解驱动-->
<mvc:annotation-driven conversion-service="conversionService"/>
```



### 9.获得Servlet相关API

SpringMVC支持使用原始ServletAPI对象作为控制器方法的参数进行注入，常用的对象如下：

- HttpServletRequest
- HttpServletResponse
- HttpSession

```java
@RequestMapping(value = "/nineteen")
@ResponseBody
public void nineteen(HttpServletRequest request, HttpServletResponse response, HttpSession session) throws IOException {
    System.out.println(request);
    System.out.println(response);
    System.out.println(session);
}
```

控制台输出：

`org.apache.catalina.connector.RequestFacade@2395b94c`
`org.apache.catalina.connector.ResponseFacade@4db3c169`
`org.apache.catalina.session.StandardSessionFacade@58cdedc7`

### 10.获得请求头

#### 1. @RequestHeader

使用@RequestHeader可以获得请求头信息，相当于web阶段学习的**request.getHeader(name)**

@RequestHeader注解的属性如下：

- value：请求头的名称
- required：是否必须携带此请求头



```java
@RequestMapping(value = "/twenty")
@ResponseBody
public void twenty(@RequestHeader(value = "User-Agent", required = false) String user_agent) throws IOException {
    System.out.println(user_agent);
}
```

控制台输出：

`Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/100.0.4896.60 Safari/537.36`

#### 2.@CookieValue

使用@CookieValue可以获得指定Cookie的值

@CookieValue注解的属性如下：

- value：指定cookie的名称
- required：是否必须携带此cookie

```java
@RequestMapping(value = "/twenty")
@ResponseBody
public void twenty(@CookieValue(value ="JSESSIONID" ) String jsessionId) throws IOException {
    System.out.println(jsessionId);
}
```

控制台输出：     `2B286AE5D3F023BAC5B6419FB99D35B3`

### 11.文件上传

#### 1.文件上传客户端三要素

- 表单项**type=“file”**
- 表单的提交方式是**post**
- 表单的enctype属性是多部分表单形式，及**enctype=“multipart/form-data”**

```jsp
<body>
    <form action="${pageContext.request.contextPath}/twentyone" method="post" enctype="multipart/form-data">
        名称：<input type="text" name="username">
        文件：<input type="file" name="upload">
        <input type="submit" value="提交">
    </form>
</body>
```



#### 2.文件上传原理

- 当form表单修改为多部分表单时，request.getParameter()将失效。

- enctype=“application/x-www-form-urlencoded”时，form表单的正文内容格式是

  **key=value&key=value&key=value**

- 当form表单的enctype取值为Mutilpart/form-data时，请求正文内容就变成多部分形式：获取全部的数据，包括

  姓名和上传文件的全部内容

### 12.单文件上传步骤



1. 导入fileupload和io坐标

   ```xml
   <dependency>
       <groupId>commons-fileupload</groupId>
       <artifactId>commons-fileupload</artifactId>
       <version>1.2.2</version>
   </dependency>
   <dependency>
       <groupId>commons-io</groupId>
       <artifactId>commons-io</artifactId>
       <version>2.4</version>
   </dependency>
   ```

   

2. 配置文件上传解析器

   ```xml
   <!--    配置文件上传解析器-->
   <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
       <!--上传文件总大小-->
       <property name="maxUploadSize" value="5242800"/>
       <!--上传单个文件的大小-->
       <property name="maxUploadSizePerFile" value="5242800"/>
       <!--上传文件的编码类型-->
       <property name="defaultEncoding" value="UTF-8"/>
   </bean>
   ```

   

3. 编写文件上传代码

   ```java
   @RequestMapping(value = "/twentyone")
   @ResponseBody
   public void twentyone(String username, MultipartFile uploadFile) throws IOException {
       System.out.println(username);
       // 获得上传文件的名称
       String originalFilename = uploadFile.getOriginalFilename();
       uploadFile.transferTo(new File("E:\\project\\"+originalFilename));
   }
   ```

### 13.多文件上传实现

多文件上传，将页面修改为多个文件上传项，将方法参数`MultipartFile`类型修改为`MultipartFile[]`即可

```jsp
<body>
    <form action="${pageContext.request.contextPath}/twentyone" method="post" enctype="multipart/form-data">
        名称：<input type="text" name="username">
        文件1：<input type="file" name="uploadFiles">
        文件2：<input type="file" name="uploadFiles">
        <input type="submit" value="提交">
    </form>
</body>
```

```java
@RequestMapping(value = "/twentyone")
@ResponseBody
public void twentyone(String username, MultipartFile[] uploadFiles) throws IOException {
    System.out.println(username);
    // 获得上传文件的名称
    for ( MultipartFile uploadFile:uploadFiles) {
        String originalFilename = uploadFile.getOriginalFilename();
        uploadFile.transferTo(new File("E:\\project\\"+originalFilename));
    }
}
```





