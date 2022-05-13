---
layout: post
title:  " SpringBoot总结 " 
date: 2022-04-16 20:22:26 +0800
tags: [Java, SpringBoot, 热部署]
---

# 热部署

修改完程序后，效果立刻发生变化，这种情况叫做热部署。

自动添加热部署：IDEA失去焦点5秒钟后启动热部署

1. **点击`Setting`==>`Build`==>`Compiler`==>勾选`Build project automatically`** 

2. **点击`Setting`==>`Advanced Settings` ==> **

   **勾选 `Allow auto-make to start even if developed application is currently running`** 

热部署范围配置：系统存在默认热部署目录，可以修改该目录：自定义不参与热部署：

```yml
spring:
  devtools:
    restart:
      exclude: static/**,public/**
```

关闭热部署（开发环境）：

```yml
spring:
  devtools:
    restart:
      exclude: static/**,public/**
      enabled: false
```

# 第三方Bean属性绑定

配置Lombok: 注解`@Data`  设置getter  setter toString方法

```xml
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
</dependency>
```

使用该类，初始化的时候加载配置文件的数据，使用：

`@ConfigurationProperties(prefix = "servers")`   servers为配置文件的前缀，

```java
@ConfigurationProperties(prefix = "servers")
public class ServerConfig {
    private String ipAddress;
    private int port;
    private long timeout;
}
```

该类的成员属性与配置文件中的名称一一对应：

```yml
servers:
  ipAdress: 192.168.0.1
  port: 9999
  timeout: -1
```

测试该方法：

```java
@SpringBootApplication
public class SpringConfigurationApplication {
    public static void main(String[] args) {
        ConfigurableApplicationContext ctx = 
            SpringApplication.run(SpringConfigurationApplication.class, args);
        ServerConfig bean = ctx.getBean(ServerConfig.class);
        System.out.println(bean);
    }
```

输出结果为：`ServerConfig(ipAddress=null, port=8000, timeout=-1)`



