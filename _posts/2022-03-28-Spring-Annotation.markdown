---
layout: post
title:  "Spring Annotation"
subtitle: 
cover-img: 
thumbnail-img:
share-img: 
date: 2022-03-28 10:23:01 +0800
tags: [Java, Spring Annotation]
---

## Spring配置数据源

- 数据源(连接池)是提高程序性能如出现的
- 事先实例化数据源，初始化部分连接资源
- 使用连接资源时从数据源中获取
- 使用完毕后将连接资源归还给数据源
- 常见的数据源(连接池)：DBCP、C3P0、BoneCP、Druid等

### 1.1 数据源的开发步骤

1. 导入数据源的坐标和数据库驱动坐标
2. 创建数据源对象
3. 设置数据源的基本连接数据
4. 使用数据源获取连接资源和归还连接资源

## Spring注解开发：

### 1.1 原始注解

Spring是轻代码而重配置的框架，配置比较繁重，影响开发效率，所以注解开发是一种趋势，**注解代替xml配置**文

件可以简化配置，提高开发效率。

**Spring原始注解主要是替代`<Bean>`的配置**

- **@Component**              使用在**类**上用于实例化Bean  
- **@Controller**                使用在web层类上用于实例化Bean
- **@Service**                     使用在service层类上用于实例化Bean
- **@Repository**               使用在dao层类上用于实例化Bean
- **@Autowired**                使用在字段上用于根据类型依赖注入
- **@Qualifier**                   结合@Autowired一起使用用于根据名称进行依赖注入
- **@Resource**                  相当于@Autowired+@Qualifier，按照名称进行注入
- **@Value**                         注入普通属性
- **@Scope**                        标注Bean的作用范围
- @PostConstruct           使用在方法上标注该方法是Bean的初始化方法
- @PreDestroy                使用在方法上标注该方法是Bean的销毁方法



### 1.2 Spring新注解

原始注解对于非自定义的Bean就无法进行注解，我们无法在jar包上进行注解，所以需要新注解来解决这些问题

- 非自定义的Bean的配置：`<bean>`
- 加载properties文件的配置：`<context:property-placeholder>`
- 组件扫描的配置：`<context:component-scan>`
- 引入其他文件：`<import>`

新注解：

- @Configuration         用于指定当前类是一个Spring 配置类，当创建容器时会从该类上加载注解

- @ComponentScan    用于指定Spring 在初始化容器时要扫描的包。

   作用和在Spring 的xml 配置文件中的`<context:component-scan base-package="com.belle"/>`一样

- @Bean                        使用在方法上，**标注将该方法的返回值存储到Spring 容器中**

- @PropertySource       用于加载.properties 文件中的配置

- @Import                      用于导入其他配置类



```java
@Configuration
@ComponentScan("com.belle")
@Import({DataSourceConfiguration.class})
public class SpringConfiguration {
    
}
```



```java
@PropertySource("classpath:jdbc.properties")
public class DataSourceConfiguration {
    @Value("${jdbc.driver}")
    private String driver;
    @Value("${jdbc.url}")
    private String url;
    @Value("${jdbc.username}")
    private String username;
    @Value("${jdbc.password}")
    private String password;
}
@Bean(name="dataSource")
public DataSource getDataSource() throws PropertyVetoException {
    ComboPooledDataSource dataSource = new ComboPooledDataSource();
    dataSource.setDriverClass(driver);
    dataSource.setJdbcUrl(url);
    dataSource.setUser(username);
    dataSource.setPassword(password);
    return dataSource;
}
```



#### 测试加载核心配置类创建Spring容器

```java
@Test
public void testAnnoConfiguration() throws Exception {
    ApplicationContext applicationContext = newAnnotationConfigApplicationContext(
        SpringConfiguration.class);
    UserService userService = (UserService)applicationContext.getBean(
        "userService");
    userService.save();
    DataSource dataSource = (DataSource)applicationContext.getBean(
        "dataSource");
    Connection connection = dataSource.getConnection();
    System.out.println(connection);
}
```

## Spring集成Junit

#### 1.1原始Junit测试Spring的问题

在测试类中，每个测试方法都有以下两行代码：

```java
ApplicationContext ac = newClassPathXmlApplicationContext("bean.xml");
IAccountService as = ac.getBean("accountService",IAccountService.class);
```

这两行代码的作用是获取容器，如果不写的话，直接会提示空指针异常。所以又不能轻易删掉。

#### 1.2 Spring集成Junit步骤

- 让SpringJunit负责创建Spring容器，但是需要将配置文件的名称告诉它

- 将需要进行测试Bean直接在测试类中进行注入

1. 导入spring集成Junit的坐标

   ```xml
   <!--此处需要注意的是，spring5 及以上版本要求junit 的版本必须是4.12 及以上-->
   <dependency>
       <groupId>org.springframework</groupId>
       <artifactId>spring-test</artifactId>
       <version>5.0.2.RELEASE</version>
   </dependency>
   <dependency>
       <groupId>junit</groupId>
       <artifactId>junit</artifactId>
       <version>4.12</version>
       <scope>test</scope>
   </dependency>
   ```

   

2. 使用@Runwith注解替换原来的运行期

   ```java
   @RunWith(SpringJUnit4ClassRunner.class)
   public class SpringJunitTest {}
   ```

   

3. 使用@ContextConfiguration指定配置文件或配置类

   ```java
   @RunWith(SpringJUnit4ClassRunner.class)
   //加载spring核心配置文件
   //@ContextConfiguration(value = {"classpath:applicationContext.xml"})
   //加载spring核心配置类
   @ContextConfiguration(classes = {SpringConfiguration.class})
   public class SpringJunitTest {}
   ```

   

4. 使用@Autowired注入需要测试的对象

   ```java
   @RunWith(SpringJUnit4ClassRunner.class)
   @ContextConfiguration(classes = {SpringConfiguration.class})
   public class SpringJunitTest {
       @Autowiredprivate 
       UserService userService;
   }
   ```

   

5. 创建测试方法进行测试

   ```java
   @RunWith(SpringJUnit4ClassRunner.class)
   @ContextConfiguration(classes = {SpringConfiguration.class})
   public class SpringJunitTest {
       @Autowired
       private UserService userService;
       @Test
       public void testUserService(){
           userService.save();
       }
   }
   ```

   

















