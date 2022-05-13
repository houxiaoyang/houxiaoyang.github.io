---
layout: post
title:  " Hashtable、Properties、Collections"
subtitle: 
cover-img: 
thumbnail-img:
share-img: 
date: 2022-01-11 15:35:22 +0800
tags: [Java, Hashtable, Properties, Collections]
---

#### Hashtable:

1. 存放的元素是键值对：K-V
2. **Hashtable的键和值都不能为null，否则就会抛出**
3. Hashtable的使用方法和HashMap一样
4. Hashtable是线程安全的，HashMap是线程不安全的

Hashtable的扩容：

底层有数组`Hashtable$Entry[]` 初始化大小为11；

临界值`threshold` 为8 (11*0.75)

扩容后由初始化的11变为23： 原来的值乘以2在加1

```java
 int newCapacity = (oldCapacity << 1) + 1;
```

#### Map接口实现类：Properties

1. `Properties类继承自Hashtable类`，并且实现了Map接口，也是一种键值对的形式来保存数据。
2. 它的使用特点和Hashtable类似。
3. Properties还可以用于从xxx.properties 文件中，加载数据到Properties类对象，并进行读取和修改

```java
Properties properties = new Properties();
properties.put("belle",100);
properties.put("belle",99);// 会被替换为99
```

可以通过key值获取value.   `System.out.println(properties.get("belle"));`

#### 选择集合实现类：

1. 判断存储类型，是一组对象（单列），还是一组键值对（双列）
   - 一组对象单列就选择Collection类中的：
   - 允许重复：**List:**
     -  增删多就选择**LinkedList**（底层为双向链表）
     - 改查多就选择**ArrayList** （底层维护一个Object类型的可变数组）
   - 不允许重复： **Set：**
     - 无序就选择**HashSet**（底层为**HashMap**,数组+链表+红黑树）
     - 排序就选择**TreeSet**
     - 插入和取出的顺序一致：**LinkedHashSet（LinkedHashMap）**，维护数组+双向链表
2. 一组键值对就选择**Map:**
   - 键值无序：**HashMap**
   - 键值排序：**TreeMap**
   - 键值插入和取出顺序一致：**LinkedHashMap**
   - 需要读取文件：**Properties**

#### TreeSet：可以排序，区分与HashSet的特点

使用无参构造器创建TreeSet时仍然是无序的：**底层还是TreeMap**

可以使用TreeSet提供的一个构造器，传入一个比较器（匿名内部类），指定排序规则。

```java
TreeSet treeSet = new TreeSet(new Comparator() {
    @Override
    public int compare(Object o1, Object o2) {
        return ((String)o2).compareTo((String)o1);
    }
});
```

```java
public TreeMap(Comparator<? super K> comparator) {
    this.comparator = comparator;
}
```

```java
 if (cpr != null) {
    do {
        parent = t;
        cmp = cpr.compare(key, t.key);
        //动态绑定调用重写的compare方法
        if (cmp < 0)
            t = t.left;
        else if (cmp > 0)
            t = t.right;
        else {
            if (t.value == null) {
                t.value = callMappingFunctionWithCheck(key, mappingFunction);
            }
            return t.value;
        }
    } while (t != null);
```

#### TreeMap:实现了Map接口

```java
TreeMap treeMap = new TreeMap(new Comparator() {
    @Override
    public int compare(Object o1, Object o2) {
        // 按照K长度大小进行比较：导致相同长度的无法添加，不是替换
        return ((String)o1).length()-((String)o2).length();
    }
});
```

#### Collections工具类：

```java
List list = new ArrayList();
list.add("tom");
list.add("jack");
list.add("belle");
list.add("clyde");
System.out.println(list);
```

- `reverse(list) :` 反转list中的元素：

  ```java
  Collections.reverse(list);
  ```

- `shuffle(list)`：对list集合元素进行随机排列：

  ```java
  Collections.shuffle(list);
  ```

- `sort(list)`：根据元素的自然顺序对指定的list集合元素按照升序排列

  ```java
  Collections.sort(list);
  ```

- `sort(List,Comparator)`：根据指定的Comparator产生的顺序(可以自己重写，我们举例按照长度)

  ```java
  Collections.sort(list, new Comparator() {
      @Override
      public int compare(Object o1, Object o2) {
          return ((String)o1).length()-((String)o2).length();
      }
  });
  ```

- `swap(List,int,int)`：将指定list集合中的`i`处元素和`j`处的元素进行交换：

  ```java
  Collections.swap(list,0,1);
  ```

- `Object max(Collections)`:根据元素自然顺序，返回给定集合中的最大元素(最小同理)

  ```java
  System.out.println(Collections.max(list));
  ```

- `Object max(Collections,Comparator):`根据Comparator顺序，返回给定集合中的最大元素

  ```java
  Collections.max(list, new Comparator() {
      @Override
      public int compare(Object o1, Object o2) {
          return ((String)o1).length()-((String)o2).length();
      }
  });
  ```

- `int frequency(Collection,Object)`:返回指定集合中指定元素出现的次数

  ```java
  System.out.println(Collections.frequency(list,"belle"));
  ```

- `void copy(List dest,List src)`：将src中的内容复制到dest中：

  ```java
  List dest = new ArrayList();
  //先给dest赋值，大小和list.size()大小相等
  for (int i = 0; i < list.size(); i++) {
      dest.add("");
  }
  Collections.copy(dest,list);
  ```

- `boolean replaceAll(List list,Object oldVal,Object newVal)`:使用新值替换LIst对象的所有旧值：

  ```java
  Collections.replaceAll(list,"tom","汤姆");
  System.out.println(list);
  ```















