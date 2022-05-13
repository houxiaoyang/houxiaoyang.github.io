---
layout: post
title:  " StringBuffer、StringBuilder "
subtitle: 
cover-img: 
thumbnail-img:
share-img: 
date: 2022-01-05 18:52:26 +0800
tags: [Java, StringBuffer, StringBuilder]
---

#### StringBuffer类：

注表示可变的字符序列，可以对字符串内容进行增删。

很多方法与String相同，但StringBuffer是 可变长度的

StringBuffer是一个容器

-  `StringBuffer extends AbstractStringBuilder`  其父类是`AbstractStringBuilder`
- `public final class StringBuffer`  属于final类，不能被继承
- `implements java.io.Serializable`   可以被串行化
- 父类`AbstractStringBuilder` 有属性`char[] value` 不属于`final` ，数组`value`存放字符串内容，在堆中不用每次更换地址（即创建新的对象）

#### String  vs  StringBuffer  :

1. String 保存的是字符串常量，里边的值不能更改，每次String类的更新实际上就是更改地址，效率低。

   `private final char value[]`

2. StringBuffer 保存的是字符串变量，里边的值是可以更改，每次StringBuffer的更新实际上可以更新内容，不用每次更新地址，效率较高：`char[] value`  在堆中

**StringBuffer() 构造器    初始容量为16个字符**

**StringBuffer(100)  指定char[]的大小**

`StringBuffer hello = new StringBuffer("hello");`  

通过给一个String创建StringBuffer,char[]大小就是字符串长度+16

##### 将String 变为 StringBuffer:

```java
String str = "belle";
StringBuffer stringBuffer = new StringBuffer(str);
```

注意：此处的`stringBuffer` 才是我们变成`StringBuffer`的对象，以前的`str`对象没有任何改变。

方法二采用append的方法：

```java
String str = "belle";
StringBuffer stringBuffer = new StringBuffer();
StringBuffer append = stringBuffer.append(str);
```

##### 将StringBuffer 变为 String:

采用StringBuffer提供的toString的方法：

```java
        StringBuffer stringBuffer = new StringBuffer("belle");
        String str = stringBuffer.toString();
```

采用构造器：

```java
        StringBuffer stringBuffer = new StringBuffer("belle");
        String s = new String(stringBuffer);
```

#### StringBuffer练习题：

```java
        String str = null;
        StringBuffer sb = new StringBuffer();
        sb.append(str);
        System.out.println(sb.length());// 第一问
        System.out.println(sb);
        StringBuffer sb1 = new StringBuffer(str);// 第二问
```

第一问：`sb.append(str)`  实际上是调用 `AbstractStringBuilder` 的`appendNull`  我们对照`appendNull`  源码发现如下代码：实际上添加完毕后是在后边加上`null`四个字符串，所以结果其长度就是4 

```java
        if (isLatin1()) {
            val[count++] = 'n';
            val[count++] = 'u';
            val[count++] = 'l';
            val[count++] = 'l';
        }
```

第二问：`StringBuffer(str)` 运行该步骤时，其父类会调用`str.length()` 

 str 为`null`  那么必然会报错空指针异常 `super(str.length()+16)`

```java
AbstractStringBuilder(String str) {
    int length = str.length();
    ...}
```

练习题：给一串数字，每三位添加一个逗号。

```java
public class StringBufferTest {
    public static void main(String[] args) {
        String price = "12343546546563567.89";
        StringBuffer sb = new StringBuffer(price);
//        int i = sb.lastIndexOf(".");
//        sb.insert(i - 3,",");
        for (int i = sb.lastIndexOf(".") - 3; i > 0; i -= 3) {
            sb.insert(i, ",");
        }
        System.out.println(sb);
    }
}
```



#### StringBuilder类：

一个可变的字符序列，此类提供一个与StringBuffer兼容的API，但不保证同步**（StringBuilder不是线程安全）**，该类被设计用作StringBuffer的一个简易替换，用在字符串缓冲区，**被单个线程使用的时候**，如果可能，建议有限使用该类，（在大多数实现中，其速度比StringBuffer块）

在StringBuilder上的主要操作时append 和insert 方法，可重载再写方法，以接受任意类型的数据。

```java
StringBuilder stringBuilder = new StringBuilder();
```

- StringBuilder继承了AbstractStringBuilder  
- StringBuilder实现了Serializable  可以网络传输，可以保存到文件
- final  类 不能被继承
- StringBuilder  仍在父类AbstractStringBuilder 的char[] value  在堆中
- StringBuilder 没有做互斥处理，单线程下处理

##### String 、StringBuffer 和StringBuilder比较：

1. StringBuffer 和StringBuilder 类似，均代表可变的字符序列，而且方法也一样
2. String：不可变字符序列，效率低，但是复用率高
3. StringBuffer：可变字符序列，效率较高（增删），线程安全
4. StringBuilder：可变字符序列，效率最高，线程不安全
5. **String注意说明：如果堆字符串需要做出大量修改，不要使用String**

总之使用原则如下：

1. 如果字符串中存在大量的修改操作，一般使用StringBuffer或者StringBuilder
2. 如果字符串中存在大量的修改操作，在**单线程**情况下，一般使用**StringBuilder**
3. 如果字符串中存在大量的修改操作，在**多线程**情况下，一般使用**StringBuffer**
4. 如果我们字符串很少修改，而且被多个对象引用，使用String，例如配置信息

**StringBuffer和StringBuilder使用方法完全一样。**
