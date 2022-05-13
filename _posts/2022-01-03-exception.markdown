---
layout: post
title:  " 异常 Exception  "
subtitle: 
cover-img: 
thumbnail-img:
share-img: 
date: 2022-01-03 21:05:26 +0800
tags: [Java, 异常]
---

#### 异常 Exception ：

将程序执行过程中发生的不正常情况称为异常。（程序逻辑错误不是异常）

```java
public class Exercise {
    public static void main(String[] args) {
        int num1 = 10;
        int num2 = 0;
        int result = num1 / num2;//ArithmeticException
        //异常处理机制类解决这个问题
        System.out.println("程序继续执行。。");
    }
}
```

因为`num2= 0`，导致系统崩溃，系统的健壮性太低，Java设计者提供了一个可以解决异常的处理机制。

首先选中该代码块，快捷键`alt + ctrl + t`  选择`try-catch`

```java
        try {
            int result = num1 / num2;
        } catch (Exception e) {
            System.out.println(e.getMessage());
            //e.printStackTrace();
        }
```

如果这个代码块出现问题，那么就捕获该问题，程序同时继续往下进行。

两大异常：

- Error: java虚拟机无法解决的严重问题JVM内部错误，资源耗尽。比如栈溢出 。严重会导致崩溃。
- Exception：其他因编程错误或偶然的外在因随导致的一般性问题，可以使用针对性的代码进行处理。例如空指针访问，试图读取不存在的文件，网络连接中断，Exception分为两类：运行时异常和编译时异常。

#### 异常体系图：

编译异常：Java源程序，编译时就会出现的异常。必须处理

运行异常：在内存中加载、运行类时存在的异常。可不处理

#### 常见五大运行时异常：

1.`NullPointerException` 空指针异常：当应用程序试图在需要对象的地方使用null时，抛出该异常：

```java
        String name = null;
        System.out.println(name.length());
```

2.`ArithmeticException` 数学运算异常

3.`ArrayIndexOutOfBoundsException` 数组下标越界异常

4.`ClassCastException` 类型转换异常

5.`NumberFormatException` 数字格式不正确异常

#### finally:

```java
        String name = null;
        try {
            System.out.println(name.length());
        } catch (Exception e) {
            //如果没有发生异常，此流程不会执行
            e.printStackTrace();
        }finally {
            //不管代码块是否有异常发生，始终要执行finally
        }

```

`finally` 不管代码块是否有异常发生，始终要执行`finally`  可以放入释放资源的代码块

#### throws: 如果没有添加try catch。默认采用throws

JVM---->调用main方法-------->调用f1方法------->调用f2方法：

**如果f2发生异常，可以直接throws抛给 f1，f1也可以再把异常抛给main，main也可以在throws抛给JVM，**

**每个人都可以自身try-catch-finally，或者抛出去throws。最终throws到JVM，JVM直接退出，报错**

扔给调用者处理，

```java
            String str = "123";
            int a = Integer.parseInt(str);
            System.out.println(" 数字形式："+a);
```

将字符串形式转换为int类型，

练习题：输入一个整数，错误就一直让输入，直到你输出的是一个整数：

```java
import java.util.Scanner;
public class Test {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        while (true) {
            System.out.println("请输入一个整数：");
            String inputStr = scanner.next();
            try {
                int num = Integer.parseInt(inputStr);
                break;
            } catch (NumberFormatException e) {
                System.out.println("你输入的不是整数");
            }
        }
        System.out.println("输入完毕");
    }
}
```

#### throws注意细节：

1，对于编译异常，程序中必须处理，比如try-catch 或者throws  **（二选一）**

2，对于运行异常，程序中如果没有处理，默认就是throws方法处理。

3，子类重写父类的方法时，对抛出异常的规定：子类重写的方法，所抛出的异常类型要么为父类抛出异常子类，要么和父类抛出的异常一致。

4，在throws过程中，如果有方法try-catch，就相当于异常处理们就不必throws 

#### 自定义异常：

步骤：定义类，继承`Exception`，或者`RuntimeException`

一般情况下时继承`RuntimeException` 做成运行时异常，好处是可以使用默认的处理机制

```java
public class Test {
    public static void main(String[] args) {
        int age = 10;
        if (!(age >= 18 && age <= 120)) {
            throw new AgeException("年龄需要在18~120之间");
        }
        System.out.println("输入正确");
    }
}

//自定义一个异常
class AgeException extends RuntimeException {
    public AgeException(String message) {
        super(message);
    }
}
```

|           | 意义                         | 位置       | 后边跟的东西 |
| --------- | ---------------------------- | ---------- | ------------ |
| throws    | 异常处理的一种方式           | 方法声明处 | 异常类型     |
| **throw** | **手动生成异常对象的关键字** | 方法体中   | 异常对象     |

