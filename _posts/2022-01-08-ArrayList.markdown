---
layout: post
title:  " List、ArrayList、Vector、LinkedList "
subtitle: 
cover-img: 
thumbnail-img:
share-img: 
date: 2022-01-08 22:38:26 +0800
tags: [Java, ArrayList, Vector, LinkedList]
---

#### List接口和常用方法：

添加元素：

```java
list.add(1,"hi");// 在index = 1 的位置加上一个"hi"
```

获取指定index位置的元素：

```java
Object result = list.get(2);
```

返回指定元素在集合中首次，最后一次出现的位置：

```java
System.out.println(list.indexOf("hello"));
System.out.println(list.lastIndexOf("hello"));
```

移除指定index位置的元素，并返回此元素：

```java
list.remove(0);
System.out.println(list);
```

设置指定index位置的元素为某个元素，相当于替换：

```java
list.set(1,"hi");
System.out.println(list);
```

返回从`fromIndex` 到`toIndex`位置的子集和： 包含前一个元素，不包含后一个元素：

```java
List list = list.subList(1,3);
System.out.println(list);
```

List的三种遍历：迭代器，增强for，普通for

#### ArrayList底层结构和源码分析※※※：

- permits all elements，including null，ArrayList 可以加入null，并且多个

- ArrayList 是由数组来实现数据存储的，

- Arraylist基本等同于Vector，除了ArrayList是线程不安全（效率高）

  在多线程的情况下不建议使用ArrayList

##### ArrayList的底层操作机制原码分析：

1. ArrayList中维护了一个Object类型的数组elementData，`transient Object[] elementData;`

   `transient`  **表示瞬间短暂，该属性不会被序列化**

2. 当初次创建ArrayList对象时，如果使用的时无参构造器，则初始`elementData`容量为0，加入一个元素，扩容为10，后续按照1.5倍进行扩容。`（10，15，22，33.....)`；**如果时指定大小的构造器，扩容的话直接以1.5倍扩容。**

3. ```java
       ArrayList list = new ArrayList();
       for (int i = 0; i < 10; i++) {
           list.add(i);
       }
       for (int i = 11; i < 15; i++) {
           list.add(i);
       }
   ```

   ```java
   public ArrayList() {
           this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
   ```

   创建了一个无参构造器，`elementData数组 = {}`,然后进行下步骤执行`list.add`：

   ```java
   public boolean add(E e) {
       modCount++;//记录被修改的次数
       add(e, elementData, size);//该步骤源码在下方
       return true;
   }
   ```

   ```java
   private void add(E e, Object[] elementData, int s) {
       if (s == elementData.length)// 此处s = 10,进入扩容
           elementData = grow();//该步骤源码在下方
       elementData[s] = e;
       size = s + 1;
   }
   ```

   先确定是否需要扩容，再执行赋值，不够进行去扩容，以下为扩容机制：

   ```java
   private Object[] grow(int minCapacity) {
       int oldCapacity = elementData.length;
       if (oldCapacity > 0 || elementData != DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
           int newCapacity = ArraysSupport.newLength(oldCapacity,
                   minCapacity - oldCapacity, /* minimum growth */
                   oldCapacity >> 1           /* preferred growth */);
           return elementData = Arrays.copyOf(elementData, newCapacity);
       } else {
           return elementData = new Object[Math.max(DEFAULT_CAPACITY, minCapacity)];
       }
   }
   ```

   `ArraysSupport.newLength(oldCapacity, minCapacity - oldCapacity,  oldCapacity >> 1  );`

   此处的`newLength(int oldLength, int minGrowth, int prefGrowth)`方法返回一个

   `hugeLength(oldLength, minGrowth);` 该方法时判断要么返回一个`SOFT_MAX_ARRAY_LENGTH`  要么返回

   `minLength = oldLength + minGrowth`  谁大返回谁。

   综上：`newLength()`返回了一个旧的值（10）+（1和5中的最大值即5），所以变成了15.

   `oldCapacity >> 1`   变为原来的一半，原来为10，现在为5

   使用扩容机制来确定要扩大到多少，第一次扩容出来为`newCapacity` 为10.第二次及其以后采用1.5倍扩容。

   扩容时采用  `elementData = Arrays.copyOf(elementData, newCapacity);` copyof  可以保留之前的数据

#### Vector：

和ArrayList底层一样，Vector是线程同步的，即线程安全，操作方法都带有`synchronized`，因此导致效率不高。

无参构造器默认10，扩容按照2倍扩容

#### LinkedList： 

底层实现了双向链表和双端队列的特点。可以添加任意元素。线程不安全，没有实现同步。

**底层结构：**

- LinkedList底层维护了一个双向链表。
- 其中两个属性：first和last分别指向首节点和尾节点。
- 其中每个节点（Node对象），里边有维护了prev (指向前一个节点)、next(后一个节点)、item；
- 所以LinkedList元素的添加和删除不是通过数组完成的，相对效率较高

```java
public class ExerciseLinkedList {
    public static void main(String[] args) {
        Node n1 = new Node("belle");
        Node n2 = new Node("clyde");
        Node n3 = new Node("hello");
        // 链接三个节点，形成双向列表
        n1.next = n2;
        n2.next = n3;
        n3.pre = n2;
        n2.pre = n1;
        Node first = n1;// 双向列表首节点
        Node last = n3; // 双向列表尾节点

        // 添加一个数据n1  n2 之间添加 n4
        Node n4 = new Node("love");
        n4.pre = n1;
        n2.pre = n4;
        n1.next = n4;
        n4.next = n2;
        while(true){
            if(first == null){
                break;
            }
            System.out.println(first);
            first = first.next;
        }
    }
}
class Node{
    public Object item;// 存放数据的地方
    public Node next; // 指向下一个节点
    public Node pre; // 指向上一个节点
    public Node(Object name){
        this.item = name;
    }
    public String toString(){
        return "Node name = "+item;
    }
}
```

·创建一个LinkedList后调用无参构造器：    `public LinkedList() {}`

```java
public boolean add(E e) {
    linkLast(e);
    return true;
}
```

```java
void linkLast(E e) {
    final Node<E> l = last;
    final Node<E> newNode = new Node<>(l, e, null);
    last = newNode;
    if (l == null)
        first = newNode;
    else
        l.next = newNode;
    size++;
    modCount++;
```



























































