---
layout: post
title:  " LinkedHashSet、Map"
subtitle: 
cover-img: 
thumbnail-img:
share-img: 
date: 2022-01-10 22:56:26 +0800
tags: [Java, LinkedHashSet, Map]
---

#### LinkedHashSet ：优点：存在顺序

1. LinkedHashSet 是HashSet的子类：底层是一个LinkedHashMap，底层维护了一个数组+双向链表（before ,after）
2. LinkedHashSet 根据元素的HashCode值来决定元素的储存位置，同时使用**链表维护元素的次序**，使得元素看起来是以插入顺序保存的。
3. LinkedHashSet 不允许添加重复元素

举例说明：

```java
Set set = new LinkedHashSet();
set.add(new String("AA"));
set.add(456);
```

   添加第一次时，直接将数组table扩容到16，存放类型节点为 `LinkedHashMap$Entry`

其数组为`HashMap$Node[]`  存放的元素/数据为`LinkedHashMap$Entry` 类型，该类型继承了前者，添加了before和after，可以实现双向链表的功能。

#### Map接口和常用方法：

- Map和Collection并列存在，用于保存具有映射关系的数据，Key-Value

- Map中的key和value可以时人设引用类型的数据，会封装到HashMap$Node对象中

- Map中的key不允许重复，当有相同的key就相当于替换

- Map的value可以重复

- Map的key可以为null，value也可以为null，注意key为null，只能有一个；value为null，可以很多个

- 常用String类作为Map的key

- key和value之间存在**单向一对一关系**，即通过指定的key 总能找到对应的value

  ```java
  System.out.println(map.get("no1"));
  ```

- Map存放数据的key-value，一对k-v是放在一个Node中的，因为Node实现了Entry接口。

#### Map遍历方式:

举例：

```java
Map map = new HashMap();
map.put("hello","world");
map.put("belle","beauty");
map.put("girl","beauty");
```

1. ##### 采用取出所有的key值，通过map.get(key)获取对应的value

   - 增强for循环：

     ```java
     Set keySet = map.keySet();
     for (Object key: keySet) {
         System.out.println(map.get(key));
     }
     ```

   - 迭代器：

     ```java
     Set keySet = map.keySet();
     Iterator iterator = keySet.iterator();
     while (iterator.hasNext()) {
         Object key = iterator.next();
         System.out.println(map.get(key));
     }
     ```
   
2. ##### 直接取出value:  这个方式无法取得key值：

   - 增强for循环：

     ```java
     Collection values = map.values();
     for (Object value:values) {
         System.out.println(value);
     }
     ```

   - 迭代器：

     ```java
     Collection values = map.values();
     Iterator iterator = values.iterator();
     while (iterator.hasNext()) {
         Object value = iterator.next();
         System.out.println(value);
     }
     ```

3. ##### EntrySet方式：

   - 增强for循环：m就是entry的集合，包含K-V，提供了两个方法获得K和V
   
     ```java
     Set entryset = map.entrySet();
     for (Object entry:entryset) {
         Map.Entry m = (Map.Entry) entry;
         System.out.println(m.getKey()+"-"+m.getValue());
     }
     ```
   
   - 迭代器:这里的entry为Node类型，实现了Entry方法，向下转型成Entry，以便使用Entry的方法。
   
     ```java
     Set entryset = map.entrySet();
     Iterator iterator = entryset.iterator();
     while (iterator.hasNext()) {
         Object entry = iterator.next();
         Map.Entry m = (Map.Entry) entry;
         System.out.println(m.getKey()+"-"+m.getValue());
     }
     ```
   
     
   
   

































