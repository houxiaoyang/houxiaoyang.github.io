---
layout: post
title:  "java入门（一）"
subtitle: 
cover-img: 
thumbnail-img:
share-img: 
date: 2021-11-11 18:05:26 +0800
tags: [Java, Record]
---
Hello,world!文件的创建：

`public class Hello {}` 该代码中，表示`Hello`是一个类，而`public`是一个公有的类

`Hello {}` 表示一个类的开始和结束

`public static void main(String[] args)` 表示一个主方法，即我们程序的入口

`main() {}` 表示方法的开始和结束

`System.out.println("hello.world")`  表示输出“hello，world”到屏幕上

`；`表示语句的结束


```
//代码如下:
public class Hello {
	public static void main(String[] args) {
		System.out.println("hello.world");
	}
}
```

在命令行输入`javac Hello.java` (该步骤是将`Hello.java`文件编译为字节码文件)，如果编译成功，就会在该文件夹下生成一个`Hello.class`格式的(字节码文件)文件；

继续在命令行输入`java Hello` 即可运行。**(~~请不要在运行的时候输入`java Hello.class`~~ ,不需要带`class`，系统可能认为你是在运行`class`类，实际上我们只需要运行`Hello`即可)**；

如果修改了源文件，那么需要保存再次`javac`编译，才能进一步运行。

注意：

1. 如果文件中含有中文时，请在文件中->设置文件编码->GBK，保存即可；
2. 如果运行的`Public class` 类别为`Hello`，文件名务必设置为Hello，请注意大小写；
3. java源文件以`.java`为扩展名，源文件的基本组成部分为类（class）；
4. java程序的执行入口是`main()`方法，固定书写格式：`public static void main(String[]{...} args)` ;
5. 一个源文件中只可能包含一个`public`类，其他类的个数不限。

