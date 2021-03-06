---
layout: post
title:  " Transaction "
subtitle: 
cover-img: 
thumbnail-img:
share-img: 
date: 2022-04-05 22:56:26 +0800
tags: [Java, Transaction]
---



# 1. 编程式事务控制相关对象

## 1.1 PlatformTransactionManager

`PlatformTransactionManager` 接口是spring 的事务管理器，它里面提供了我们常用的操作事务的方法。

-  `TransactionStatus getTransaction(TransactionDefination defination)`  获取事务的状态信息
- `void commit(TransactionStatus status)`   提交事务
- `void rollback(TransactionStatus status)`   回滚事务

PlatformTransactionManager 是接口类型，不同的Dao 层技术则有不同的实现类.

## 1.2.TransactionDefinition

`TransactionDefinition` 是事务的定义信息对象，里面有如下方法：

- `int getIsolationLevel()`   获得事务的隔离级别
- `int getPropogationBehavior()`         获得事务的传播行为
- `int getTimeout()`               获得超时时间
- `boolean isReadOnly()`             是否只读

###    1.事务隔离级别

设置隔离级别，**可以解决事务并发产生的问题**，如脏读、不可重复读和幻读。

- ISOLATION_DEFAULT       默认
- ISOLATION_READ_UNCOMMITTED      读未提交
- ISOLATION_READ_COMMITTED       读已提交（解决脏读）
- ISOLATION_REPEATABLE_READ    重复读 （解决不可重复读）
- ISOLATION_SERIALIZABLE    串行化（全部解决，但性能较低）

### 2.事务传播行为

解决业务方法在调用业务方法时，事务间统一性的问题。

- **REQUIRED：如果当前没有事务，就新建一个事务，如果已经存在一个事务中，加入到这个事务中。一般的选择（默认值）**
- **SUPPORTS：支持当前事务，如果当前没有事务，就以非事务方式执行（没有事务）**
- MANDATORY：使用当前的事务，如果当前没有事务，就抛出异常
- REQUERS_NEW：新建事务，如果当前在事务中，把当前事务挂起。
- NOT_SUPPORTED：以非事务方式执行操作，如果当前存在事务，就把当前事务挂起
- NEVER：以非事务方式运行，如果当前存在事务，抛出异常
- NESTED：如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则执行REQUIRED 类似的操作
- 超时时间：默认值是-1，没有超时限制。如果有，以秒为单位进行设置
- 是否只读：建议查询时设置为只读

### 3.TransactionStatus

`TransactionStatus` 接口提供的是事务具体的运行状态，方法介绍如下：

- `boolean hasSavepoint()`      是否存储回滚点

- `boolean isCompleted()`      事务是否完成
- `boolean isNewTransaction()`       是否是新事务
- `boolean isRollbackOnly()`          事务是否回滚

### 4 知识要点：

编程式事务控制三大对象

- PlatformTransactionManager

- TransactionDefinition

- TransactionStatus

## 2. 基于XML的声明式事务控制

Spring 的声明式事务顾名思义就是采用声明的方式来处理事务。这里所说的声明，就是指在配置文件中声明，用在Spring 配置文件中声明式的处理事务来代替代码式的处理事务。

**声明式事务处理的作用：**

- **事务管理不侵入开发的组件**。具体来说，业务逻辑对象就不会意识到正在事务管理之中，事实上也应该如此，因为事务管理是属于系统层面的服务，而不是业务逻辑的一部分，如果想要改变事务管理策划的话，也只需要在定义文件中重新配置即可
- 在不需要事务管理的时候，只要在设定文件上修改一下，即可移去事务管理服务，无需改变代码重新编译，这样维护起来极其方便

**注意：Spring 声明式事务控制底层就是AOP。**

### 声明式事务控制的实现

1. 引入tx命名空间

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:aop="http://www.springframework.org/schema/aop"
          xmlns:tx="http://www.springframework.org/schema/tx"
          xsi:schemaLocation="
          http://www.springframework.org/schema/beans 								
          http://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/aop
          http://www.springframework.org/schema/aop/spring-aop.xsd
          http://www.springframework.org/schema/tx
          http://www.springframework.org/schema/tx/spring-tx.xsd">
   ```

2. 配置事务增强

   ```xml
   <!--目标对象  内部的方法就是切点-->
   <bean id="accountService" class="com.belle.service.impl.AccountServiceImpl">
       <property name="accountDao" ref="accountDao"/>
   </bean>
   
   <!--配置平台事务管理器-->
   <bean id="transactionManager" 
         class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
       <property name="dataSource" ref="dataSource"/>
   </bean>
   
   <!-- 通知 事务的增强 -->
   <tx:advice id="txAdvice" transaction-manager="transactionManager">
       <!-- 设置事务的属性信息 -->
       <tx:attributes>
           <!-- 那些方法被增强 -->
           <tx:method name="transfer" isolation="REPEATABLE_READ" 
                      propagation="REQUIRED" read-only="false"/>
           <tx:method name="*"/>
       </tx:attributes>
   </tx:advice>
   ```

3. 配置事务AOP 织入

   ```xml
   <!--     配戏事务AOP的织入 -->
   <aop:config>
       <aop:advisor advice-ref="txAdvice" 
                    pointcut="execution(* com.belle.service.impl.*.*(..))"/>
   </aop:config>
   ```



### 切点方法的事务参数的配置

```xml
<tx:advice id="txAdvice" transaction-manager="transactionManager">
    <!-- 设置事务的属性信息 -->
    <tx:attributes>
        <!-- 那些方法被增强 -->
        <tx:method name="transfer" isolation="REPEATABLE_READ" 
                   propagation="REQUIRED" read-only="false"/>
        <tx:method name="*"/>
    </tx:attributes>
</tx:advice>
```

其中，<`tx:method`> 代表切点方法的事务参数的配置，例如：
`<tx:method name="transfer" isolation="REPEATABLE_READ" propagation="REQUIRED" timeout="-1" read-only="false"/>`

- name：切点方法名称
- isolation:事务的隔离级别
- propogation：事务的传播行为
- timeout：超时时间
- read-only：是否只读



## 基于注解的声明式事务控制

1. 编写AccoutDao

   ```java
   @Repository("accountDao")
   public class AccountDaoImpl implements AccountDao {
   
       @Autowired
       private JdbcTemplate jdbcTemplate;
   
       public void out(String outMan, double money) {
           jdbcTemplate.update(
               "update account set money=money-? where name=?",money,outMan);
       }
   
       public void in(String inMan, double money) {
           jdbcTemplate.update(
               "update account set money=money+? where name=?",money,inMan);
       }
   }
   ```

2. 编写AccoutService

   ```java
   @Service("accountService")
   public class AccountServiceImpl implements AccountService {
   
       @Autowired
       private AccountDao accountDao;
   
       @Transactional(isolation = Isolation.READ_COMMITTED,
                      propagation = Propagation.REQUIRED)
       public void transfer(String outMan, String inMan, double money) {
           // 开启事务
           accountDao.out(outMan,money);
           //int i = 1/0;
           accountDao.in(inMan,money);
           // 提交事务
       }
   }
   
   ```

3. 编写applicationContext.xml 配置文件

   ```xml
   <!--配置平台事务管理器-->
   <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
       <property name="dataSource" ref="dataSource"/>
   </bean>
   <!-- 事务的注解驱动 -->
   <tx:annotation-driven transaction-manager="transactionManager"/>
   ```

### 注解配置声明式事务控制解析

1. 使用`@Transactional` 在需要进行事务控制的类或是方法上修饰，注解可用的属性同xml 配置方式，例如隔离级别、传播行为等。
2. 注解使用在类上，那么该类下的所有方法都使用同一套注解参数配置。
3. 使用在方法上，不同的方法可以采用不同的事务参数配置。
4. Xml配置文件中要开启事务的注解驱动`<tx:annotation-driven />`





