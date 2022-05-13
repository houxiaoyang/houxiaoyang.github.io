---
layout: post
title:  " Spring "
subtitle: 
cover-img: 
thumbnail-img:
share-img: 
date: 2022-03-27 22:56:26 +0800
tags: [Java, Spring]
---

## Spring开发步骤

1. 导入Spring 开发的基本包坐标
2. 编写Dao 接口和实现类
3. 创建Spring 核心配置文件
4. 在Spring 配置文件中配置UserDaoImpl
5. 使用Spring 的API 获得Bean 实例

## 1.1  Bean标签范围配置

### 1）当scope的取值为singleton时

Bean的实例化个数：1个

Bean的实例化时机：**当Spring核心文件被加载时，实例化配置的Bean实例**

Bean的生命周期：

- 对象创建：当应用加载，创建容器时，对象就被创建了
- 对象运行：只要容器在，对象一直活着
- 对象销毁：当应用卸载，销毁容器时，对象就被销毁了

### 2）当scope的取值为prototype时

Bean的实例化个数：多个

Bean的实例化时机：**当调用getBean()方法时实例化Bean**

- 对象创建：当使用对象时，创建新的对象实例
- 对象运行：只要对象在使用中，就一直活着
- 对象销毁：当对象长时间不用时，被Java 的垃圾回收器回收了

## 1.2 Bean实例化三种方式

### 1）使用无参构造方法实例化

它会根据默认无参构造方法来创建类对象，如果bean中没有默认无参构造函数，将会创建失败

```xml
<bean id="userDao" class="com.belle.dao.impl.UserDaoImpl"/>
```



### 2）工厂静态方法实例化

工厂的静态方法返回Bean实例

```java
public class StaticFactoryBean {
    public static UserDao createUserDao(){
        return new UserDaoImpl();
    }
}
```

```xml
<bean id="userDao" class="com.belle.factory.StaticFactoryBean" 
      factory-method="createUserDao" />
```



### 3）工厂实例方法实例化

工厂的非静态方法返回Bean实例

```java
public class DynamicFactoryBean {
    public UserDao createUserDao(){
        return new UserDaoImpl();
    }
}
```

```xml
<bean id="factoryBean" class="com.belle.factory.DynamicFactoryBean"/>
<bean id="userDao" factory-bean="factoryBean" factory-method="createUserDao"/>
```



## 1.3 Bean的依赖注入

​		依赖注入（Dependency Injection）：它是Spring 框架核心IOC 的具体实现。在编写程序时，通过控制反转，把对象的创建交给了Spring，但是代码中不可能出现没有依赖的情况。

**IOC 解耦只是降低他们的依赖关系，但不会消除**。例如：业务层仍会调用持久层的方法。

简单的说，就是坐等框架把持久层对象传入业务层，而不用我们自己去获取。

1. 创建UserService，UserService 内部在调用UserDao的save() 方法

   ```java
   public class UserServiceImpl implements UserService {
       @Override
       public void save() {
           ApplicationContext applicationContext = new ClassPathXmlApplicationContext(
               "applicationContext.xml");
           UserDao userDao = (UserDao) applicationContext.getBean("userDao");
           userDao.save();
       }
   }
   ```

2. 将UserServiceImpl 的创建权交给Spring

   ```xml
   <bean id="userService" class="com.belle.service.impl.UserServiceImpl"/>
   ```

3. 从Spring 容器中获得UserService 进行操作

   ```java
   ApplicationContextapplicationContext= new ClassPathXmlApplicationContext(
       "applicationContext.xml");
   UserServiceuserService= (UserService) applicationContext.getBean("userService");
   userService.save();
   ```

**怎么将UserDao怎样注入到UserService内部呢？**

- **构造方法**

1. 创建有参构造

   ```java
   public class UserServiceImpl implements UserService {
       @Override
       public void save() {
           ApplicationContext applicationContext = new
   ClassPathXmlApplicationContext("applicationContext.xml");
           UserDao userDao = (UserDao) applicationContext.getBean("userDao");
           userDao.save();
       }
   }
   ```

2. 配置Spring容器调用有参构造时进行注入

   ```xml
   <bean id="userDao" class="com.belle.dao.impl.UserDaoImpl"/>
   <bean id="userService" class="com.belle.service.impl.UserServiceImpl">
       <constructor-arg name="userDao" ref="userDao"></constructor-arg>
   </bean>
   ```

- **set方法**

1. 在UserServiceImpl中添加setUserDao方法

   ```xml
   <bean id="userDao" class="com.belle.dao.impl.UserDaoImpl"/>
   <bean id="userService" class="com.belle.service.impl.UserServiceImpl">
       <property name="userDao" ref="userDao"/>
   </bean>
   ```

2. **P命名空间注入**本质也是set方法注入，但比起上述的set方法注入更加方便，主要体现在配置文件中，如下：

   首先，需要引入P命名空间： 其次修改注入方式：

   ```xml
   xmlns:p="http://www.springframework.org/schema/p"
   ```

   ```xml
   <bean id="userService" class="com.belle.service.impl.UserServiceImpl" p:userDao-
   ref="userDao"/>
   ```



## 1.4 注入数据的三种数据类型

- 普通数据类型

  ```java
  public class UserDaoImpl implements UserDao {
      private String company;
      private int age;
      public void setCompany(String company) {
          this.company = company;
      }
      public void setAge(int age) {
          this.age = age;
      }
      public void save() {
          System.out.println(company+"==="+age);
          System.out.println("UserDao save method running....");
      }
  }
  ```

  ```xml
  <bean id="userDao" class="com.belle.dao.impl.UserDaoImpl">
      <property name="company" value="belle"></property>
      <property name="age" value="15"></property>
  </bean>
  ```

  

  

- 集合数据类型（List<String>）的注入

  ```java
  public class UserDaoImpl implements UserDao {
      private List<String> strList;
      public void setStrList(List<String> strList) {
          this.strList = strList;
      }
      public void save() {
          System.out.println(strList);
          System.out.println("UserDao save method running....");
      }
  }
  ```

  ```xml
  <bean id="userDao" class="com.belle.dao.impl.UserDaoImpl">
      <property name="strList">
          <list>
              <value>aaa</value>
              <value>bbb</value>
              <value>ccc</value>
          </list>
      </property>
  </bean>
  ```

  

- 集合数据类型（List<User>）的注入

  ```java
  public class UserDaoImpl implements UserDao {
      private List<User> userList;
      public void setUserList(List<User> userList) {
          this.userList = userList;
      }
      public void save() {
          System.out.println(userList);
          System.out.println("UserDao save method running....");
      }
  }
  ```

  ```xml
  <bean id="u1" class="com.belle.domain.User"/>
  <bean id="u2" class="com.belle.domain.User"/>
  <bean id="userDao" class="com.belle.dao.impl.UserDaoImpl">
      <property name="userList">
          <list>
              <bean class="com.belle.domain.User"/>
              <bean class="com.belle.domain.User"/>
              <ref bean="u1"/>
              <ref bean="u2"/>
          </list>
      </property>
  </bean>
  ```



- 集合数据类型（Map<String,User>）的注入

  ```java
  public class UserDaoImpl implements UserDao {
      private Map<String,User> userMap;
      public void setUserMap(Map<String, User> userMap) {
          this.userMap = userMap;
      }
      public void save() {
          System.out.println(userMap);
          System.out.println("UserDao save method running....");
      }
  }
  ```

  ```xml
  <bean id="u1" class="com.belle.domain.User"/>
  <bean id="u2" class="com.belle.domain.User"/>
  <bean id="userDao" class="com.belle.dao.impl.UserDaoImpl">
      <property name="userMap">
          <map>
              <entry key="user1" value-ref="u1"/>
              <entry key="user2" value-ref="u2"/>
          </map>
      </property>
  </bean>
  ```

  

- 集合数据类型（Properties）的注入

  ```java
  public class UserDaoImpl implements UserDao {
      private Properties properties;
      public void setProperties(Properties properties) {
          this.properties = properties;
      }
      public void save() {
          System.out.println(properties);
          System.out.println("UserDao save method running....");
      }
  }
  ```

  ```xml
  <bean id="userDao" class="com.belle.dao.impl.UserDaoImpl">
      <property name="properties">
          <props>
              <prop key="p1">aaa</prop>
              <prop key="p2">bbb</prop>
              <prop key="p3">ccc</prop>
          </props>
      </property>
  </bean>
  ```



## 1.5 引入其他配置文件（分模块开发）

实际开发中，Spring的配置内容非常多，这就导致Spring配置很繁杂且体积很大，所以，可以将部分配置拆解到其

他配置文件中，而在Spring主配置文件通过import标签进行加载

```xml
<import resource="applicationContext-xxx.xml"/>
```



### Spring的重点配置

```xml
<bean>标签
	id属性:在容器中Bean实例的唯一标识，不允许重复
	class属性:要实例化的Bean的全限定名
	scope属性:Bean的作用范围，常用是Singleton(默认)和prototype
<property>标签：属性注入
	name属性：属性名称
	value属性：注入的普通属性值
	ref属性：注入的对象引用值
	<list>标签
	<map>标签
	<properties>标签
<constructor-arg>标签
<import>标签:导入其他的Spring的分文件
```



## 1.6 ApplicationContext的实现类

#### **1）ClassPathXmlApplicationContext**

它是从类的根路径下加载配置文件推荐使用这种

#### 2）FileSystemXmlApplicationContext

它是从磁盘路径上加载配置文件，配置文件可以在磁盘的任意位置。

#### 3）AnnotationConfigApplicationContext

当使用注解配置容器对象时，需要使用此类来创建spring 容器。它用来读取注解。



- #### getBean()方法使用

```java
public Object getBean(String name) throws BeansException {
    assertBeanFactoryActive();
    return getBeanFactory().getBean(name);
}
public <T> T getBean(Class<T> requiredType) throws BeansException {
    assertBeanFactoryActive();
    return getBeanFactory().getBean(requiredType);
}
```



其中，当参数的数据类型是字符串时，表示根据Bean的id从容器中获得Bean实例，返回是Object，需要强转。

当参数的数据类型是Class类型时，表示根据类型从容器中匹配Bean实例，当容器中相同类型的Bean有多个时，则

此方法会报错。

```java
ApplicationContext applicationContext = new
ClassPathXmlApplicationContext("applicationContext.xml");
UserService userService1 = (UserService) applicationContext.getBean("userService");
UserService userService2 = applicationContext.getBean(UserService.class);
```



#### Spring的重点API

```java
ApplicationContext app = newClasspathXmlApplicationContext("xml文件");
app.getBean("id");
app.getBean(Class);
```



