---
layout: post
title:  " equals、hashCode、finalize  "
subtitle: 
cover-img: 
thumbnail-img:
share-img: 
date: 2021-12-24 18:05:26 +0800
tags: [Java, equals, hashcode]
---

#### equals方法：

 **object类**

equals 在object默认比较两者内存地址是否相同，子类String中往往重写该方法判断内容是否相等，重写如下：

重写Object的 equals 方法：

```java
public boolean equals(Object obj){
    //判断两个是否为同一个对象，是同一个对象那么一定是相同的
    if(this == obj){
        return true;
    }
    //判断是否是相同类型
    if(obj instanceof Person){
        Person p = (Person)obj;//向下转型得到obj的各个属性
        return this.name.equals(p.name) && this.age == p.age;
    }
    //如果不是同类型，直接返回false(人无法和狗相比)
    return false;
}
```



 `equals` 默认 比较两个对象是不是同一个对象，比较的就是内存地址，是同一个对象就返回true，不是同一个对象就返回false。

`==`判断是什么类型？如果是基本数据类型，就是比较二者的值是否相等，如果是引用类型就是判断两者是否内存地址相同（都是new出来的地址必定不相同）

`equals` 判断object是比较两者的内存地址是否相同，数据object类的子类，则会改写（默认完成），子类中都是比较二者的内容那个是否相同。



#### hashCode方法：返回该对象的哈希码值：提高哈希表的性能

1. 提高具有哈希结构的容器的效率
2. 两个引用，如果指向的是同一个对象，则哈希值肯定是一样的。
3. 两个引用，如果指向的是不同对象，则哈希值是不一样的。
4. 哈希值主要根据地址号来写的，不能完全将哈希值等价于地址。
5. 后续hashCode如果需要也会重写。



#### toString ：返回该对象的字符串表示：全类名+@+哈希值的十六进制

```java
public String toString(){
    return getClass().getName()+"@"+Integer.toHexString(hashCode());
}
```

`getClass().getName()`  表示全类名（包名+类名）

`Integer.toHexString(hashCode())`  将对象的哈希值转换成十六进制的字符串

`monster.toString()`   调用toString方法

重写对象的同toString方法，输出对象的属性：`insert + Alt`

当我们直接输出一个对象时，tostring方法会默认调用：

`System.out.println(monster);`    等价于：`monster.toString()`

#### finalize 方法：(实际中多半不用，面试需求)

当某个对象没有被引用时，启用垃圾回收器就会销毁该对象，把堆里的空间释放出来，销毁对象前，会调用finalize方法，我们可以在这个方法里添加业务逻辑代码（数据库链接，关闭文件）等

如果我们不重写该方法，那么object类的finalize默认处理，如果重写那么就可以实现自己的逻辑

垃圾回收机制有自己的算法回收机制，我们可以自己调用运行垃圾回收器。`System.gc()`

#### 断点调试：在运行时，是以对象的运行类型来执行的！！

`F7` 跳入  `F8` 跳过 `shift + F8` (跳出) `F9` （resume 执行到下一个断点）
