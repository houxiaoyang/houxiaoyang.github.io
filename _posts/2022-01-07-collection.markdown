---
layout: post
title:  " 集合、迭代器 Iterator"
subtitle: 
cover-img: 
thumbnail-img:
share-img: 
date: 2022-01-07 22:05:26 +0800
tags: [Java, 集合, 迭代器]
---

#### 集合框架体系：

可以动态保存任意多个对象，使用比较方便。

集合分为单列集合，双列集合：Collection 有两个重要的子接口：List Set  ，实现的子类都是单列集合

Map接口实现子类是双列集合，存放的是k-v（键值对）

![](\assets\img\blogpic\collection.png)

![](\assets\img\blogpic\Map.png)

#### Collection接口实现类的特点：

```java
public interface Collection<E> extends Iterable<E>
```

1. Collection实现子类可以存放多个元素，每个元素可以是Object
2. 有些Collection的实现类，可以存放重复的元素，有些不可以
3. 有些Collection的实现类，有些是有序的（List），有些不是有序（Set）
4. Collection接口没有直接实现子类，是通过它的子接口Set和List来实现的

接口常用方，以实现子类：`Collection col = new ArrayList();`

- add ： 添加单个元素

- remove : 删除指定元素

- contains：查找元素是否存在

- size：获取元素个数

- isEmpty：判断是否为空

- clear：清空

- addAll：添加多个元素

- containsAll：查找多个元素是否都存在

- removeAll：删除多个元素

  ```java
  List col = new ArrayList();
  col.add("belle");
  col.add(100);// 相当于存入Integer 100
  col.add(true);
  System.out.println(col);
  col.remove(0);
  System.out.println(col);
  col.contains("true");// boolean
  col.size();// 返回一共有多少个元素
  List list = new ArrayList();
  list.add("红楼梦");
  list.add("三国");
  col.addAll(list);
  System.out.println(col);
  ```

  #### Iterator迭代器：快捷键 `itit`    查询所有快捷键  `Ctrl + j`

  1. Iterator对象被称为迭代器，主要用于遍历Collection集合中的元素。
  2. 所有实现了Collection接口的集合类都有一个iterator() 方法，用以返回一个实现了Iterator接口的对象，即可以返回一个迭代器。
  3. Iterator
  4. Iterator 仅用于遍历集合，Iterator本身并不存放对象。

```java
Iterator iterator = coll.iterator();
while(iterator.hasNext()){
    //next();//指针下移，将下移以后集合位置上的元素返回
    System.out.println(iterator.next());
}
```

`hasNext()` 判断是否还有下一个元素，如果有没有就返回一个`false`

在调用`iterator.next()` 之前必须调用 `iterator.hasNext()` 进行检测，如果不检测。且下一条记录无效，则会直接调用`iterator.next()`  抛出 `NoSuchElementException` 异常；实例如下：

```java
Collection coll = new ArrayList();
coll.add("belle");
coll.add("love");
coll.add("me");
System.out.println(coll);

Iterator iterator = coll.iterator();
while(iterator.hasNext()){
    Object obj = iterator.next();
    System.out.println(obj);
}
```

如果想要再次遍历，需要重置迭代器： 

```java
iterator = coll.iterator();
```

增强for循环：底层仍然是迭代器，将`col`的元素挨个放入`str`

```java
Collection col = new ArrayList();
col.add("belle");
col.add("love");
col.add("me");
System.out.println(col);
for(Object str:col){
    System.out.println(str);
}
```



