---
layout: post
title:  " HashSet、HashMap "
subtitle: 
cover-img: 
thumbnail-img:
share-img: 
date: 2022-01-09 22:29:26 +0800
tags: [Java, HashSet, HashMap]
---

#### Set：

- 无序（添加和取出的顺序不一样），没有索引。
- 没有重复元素，所以最多包含一个null
- 和Collection接口一样，

```java
Set set = new HashSet();
set.add("hello");
set.add("world");
set.add("my");
set.add("girl");
// Set接口存放对象是无序的，添加和取出的顺序不一致
// 取出一次后顺序都是一致的，不会一次一变
System.out.println(set);

// 遍历 1.使用迭代器   因为是Collection的接口
Iterator iterator = set.iterator();
while (iterator.hasNext()) {
    Object next = iterator.next();
    System.out.println(next);
}
// 增强for循环：
for (Object o : set){
    System.out.println(o);
}
// Set 对象不能通过索引来获取
```



HashSet ：实现了Set接口：底层实际上是HashMap: 源码如下：

```java
public HashSet() {
    map = new HashMap<>();
}
```

```java
Set hashSet = new HashSet();
hashSet.add(null);
hashSet.add(null);
//只能存放一个null  即元素不能重复
System.out.println(hashSet);// [null]
// 不保证存放元素的顺序和取出的顺序一致。不能有重复的元素
```

再执行add方法后，会返回一个`boolean`，成功返回`true`，失败返回`false`

```java
        HashSet set = new HashSet();
        // HashSet 不能添加相同的元素/对象
        set.add("lucy");
        set.add("lucy");//添加不了
        set.add(new Dog("tom"));
        set.add(new Dog("tom"));//可以添加
        System.out.println(set);
        set.add(new String("belle"));
        set.add(new String("belle"));//添加不了
        System.out.println(set);
    }
}
class Dog{
    private String name;

    public Dog(String name) {
        this.name = name;
    }
    @Override
    public String toString() {
        return name;
    }
}
```



#### HashSet底层机制说明：

**HashMap底层为（数组+链表+红黑树）**：

```java
HashSet hashSet = new HashSet();
hashSet.add("java");
hashSet.add("java");
```
以这个为例：按照步骤进行：

```java
public HashSet() {//构造器
    map = new HashMap<>();
}
```

```java
public boolean add(E e) { //
    return map.put(e, PRESENT)==null;// PRESENT 静态的Object()
}
```

```java
public V put(K key, V value) {//key :java   value: PRESENT
    return putVal(hash(key), key, value, false, true);
}
```

注意：以上的hash(key) 方法会得到key对应的hash值（并不完全等价于`hashcode`）

hash(key) 方法:`(key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);`

```java
final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
               boolean evict) {
    Node<K,V>[] tab; Node<K,V> p; int n, i;//定义了辅助变量
    // table是HashMap的一个数组 类型是Node[]
    if ((tab = table) == null || (n = tab.length) == 0)
        //如果table为null，或者大小为0那么执行resize()
        n = (tab = resize()).length;
    // resize() 执行完毕后，第一次扩容到16个空间
    if ((p = tab[i = (n - 1) & hash]) == null)
    //根据key得到的hash值，去计算该key应该存放到table表的哪个索引位置
    //并把这个位置的对象赋值给p
    //再去判断这个值是是否为null,如果为null，表示没有任何数据，创建一个Node
    //直接放进去  放入table[i]
        tab[i] = newNode(hash, key, value, null);
    //key:java  value=PRESENT
    
    else {
        Node<K,V> e; K k;// 需要的时候再临时创建，辅助变量
        if (p.hash == hash &&
//以下两个满足其一即可进入if：
//如果当前索引位置对应的链表的第一个元素hash值和准备添加的key的hash值一样，
//而且准备加入的key和p指向的的Node节点的key的hash值一样；
//或者::准备加入的key和p指向的的Node节点的key本身是一个对象，
//而且p指向的的Node节点的key和准备加入的key的equals比较后相同。（这是比较对象的）
            ((k = p.key) == key || (key != null && key.equals(k))))
            e = p;
//如果无法满足进入前者，那么我们判断是不是一棵红黑树，
//如果是的话，进入红黑树的比较方法 putTreeVal() 该方法复杂暂不展开
        else if (p instanceof TreeNode)
            e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
        else {
            for (int binCount = 0; ; ++binCount) {//死循环  除非break
        //这是个链表啊，那就执行循环比较，直到有对象和链表里的key相同的话，直接退出
        //执行完毕，若都没有相同的，就将他放在链表最后
                if ((e = p.next) == null) {
                    //若为空即这个节点没有元素，将其放在第一个
                    p.next = newNode(hash, key, value, null);
     //这里显示 执行完毕，若都没有相同的，就将他放在链表最后，立即判断：
                    if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
    //是否大于等于7，即是否为8个元素了，达到的话调用红黑树
    //treeifyBin(tab, hash);将其转为红黑树，在进行树化时
    //如果长度大于8小于64时，还不会立刻树化，先尝试去扩容表来解决
    //如果大于64的话就去变成红黑树
                        treeifyBin(tab, hash);
                    break;
                }
                if (e.hash == hash &&// 这里显示：存在一个相同的，break
                    ((k = e.key) == key || (key != null && key.equals(k))))
                    break;
                p = e;
//我们判断e和我们要添加的不相同，就将e赋值给p,让e=p.next,p的下一个是否为空，
//如果不为空，我们判断他和我们的是否相同，不相同继续循环，一直到：
//e成为了最后一个，那么e赋值给p,p的下一个没有了，为空，则终于进入if语句，
//新建一个 并且赋值给p.next，结束。
            }
        }
        if (e != null) { // existing mapping for key
            V oldValue = e.value;
            if (!onlyIfAbsent || oldValue == null)
                e.value = value;
            afterNodeAccess(e);
            return oldValue;
        }
    }
    ++modCount;
    if (++size > threshold)//threshold=12
        //添加一个完毕后，判断这个数字是否大于12，如果大于12，进行扩容
        resize();
    afterNodeInsertion(evict);
    return null;
}
```

至此，java添加完毕。
