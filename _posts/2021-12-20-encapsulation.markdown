---
layout: post
title:  " 包、访问修饰符、封装 "
subtitle: 
cover-img: 
thumbnail-img:
share-img: 
date: 2021-12-20 13:05:26 +0800
tags: [Java, 封装, 包]
---

#### IDEA：

快捷键：

- 删除当前行，**`ctrl + Y`**   (自行设置)
- 复制当前行 **`ctrl + D`**
- 补全代码：**`alt + /`**
- 添加注释 **`ctrl + /`**
- 快速导入import包，使用**`alt+enter`**
- 快速格式化代码：**`ctrl + alt + L`**
- 快速运行程序：**`alt+R`**
- 生成构造器：**`alt + insert`**
- 查看类的继承关系：**`ctrl + H`**
- 将光标放在一个方法上，输入**`ctrl +B`** 可以定位到方法
- 自动分配变量名，在后边加 **`.var`**



#### 包的三大作用：

区分相同名字的类；管理类；控制访问范围。

包的本质就是创建不痛的文件夹/目录，来保存类文件。

###### 包的命名规则：

只能包含数字、字母、下划线、小圆点、不能数字开头，不能是关键字和保留字。

###### 包的命名规范：

一般情况下：**com.公司名.项目名.业务模块名**

常用的包：

**java.lang.  :**默认包，直接使用不需要引用。

**java.util.**  ：工具包，系统提供的工具包，工具类

**java.net.**  :  网络包，网络开发

**java.awt.**  : Java界面开发，GUI

需要包里的那个类我们就导入。如果java.util.*;不建议全部导入

#### 访问修饰符：

用于控制方法和属性（成员变量）的访问权限（范围）：

1. 公开级别：**public**修饰，对外公开

2. 受保护级别：**protected**修饰，对子类和同一个包中的类公开

3. 默认级别：没有修饰符，向同一个包的类公开；

4. 私有级别：**private**修饰，只有类本身可以访问，不对外公开。

   | 访问级别 | 访问控制修饰符 | 本类 | 本包 | 子类 | 不同包 |
   | :------: | :------------: | :--: | :--: | :--: | :----: |
   |   公开   |     public     | `√`  | `√`  | `√`  |  `√`   |
   |  受保护  |   protected    | `√`  | `√`  | `√`  |  `×`   |
   |   默认   |   没有修饰符   | `√`  | `√`  | `×`  |  `×`   |
   |   私有   |    private     | `√`  | `×`  | `×`  |  `×`   |



#### **面向对象的三大特征：封装，继承，多态。**

**封装（encapsulation）**就是把抽象出的数据[属性]和堆数据的操作[方法]封装在一起，数据被保护在内部，程序的其他部分只有通过被授权的操作（方法）才能对数据进行操作。

封装的好处，可以对数据进行验证，保证安全合理。

封装的实现步骤：

1.将属性进行私有化private（导致不能直接修改属性）

2.提供一个公共的（public）方法，用于对属性的判断并赋值

```java
public void setXxx(类型 参数名){
    //加入数据验证的业务逻辑
    属性 = 参数名;
}
```

3.提供一个公共的(public)get方法，用于获取属性的值

```java
public XX getXxx(){//权限判断
    return xx;
}
```



将构造器与setXXX结合:

```java
public Person(String name, int age, double salary) {
    this.setName(name);
    this.setAge(age);
    this.setSalary(salary);
}
```



代码如下：

```java
package com.cuilo.encap;
public class Accout {
    private String name;
    private double money;
    private String passwd;

    public Accout() {
    }

    public Accout(String name, double money, String passwd) {
        this.setName(name);
        this.setMoney(money);
        this.setPasswd(passwd);
    }//在构造器里采用set方法

    public String getName() {
        return name;
    }

    public void setName(String name) {
        if (name.length() >= 2 && name.length() <= 4) {
            this.name = name;
        } else {
            System.out.println("名字长度需在2~4位之间，已修改位默认值");
            this.name = "吴彦祖";
        }

    }

    public double getMoney() {
        return money;
    }

    public void setMoney(double money) {
        if (money > 20) {
            this.money = money;
        } else {
            System.out.println("余额最低为20元，已修改为默认");
        }

    }

    public String getPasswd() {
        return passwd;
    }

    public void setPasswd(String passwd) {
        if (passwd.length() ==6) {
            this.passwd = passwd;
        } else {
            System.out.println("密码长度需6位，已修改位默认000000");
            this.passwd = "000000";
        }
    }
    public void showinfo() {
        //可以增加一个权限的校验
        System.out.println("账号信息为 姓名 = " + name + " 余额 = " + money + " 密码 = " + passwd);
    }
}
```







