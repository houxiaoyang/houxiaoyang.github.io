---
layout: post
title:  " 数据结构与算法"
subtitle: 
cover-img: 
thumbnail-img:
share-img: 
date: 2022-04-11 20:22:26 +0800
tags: [Java, DataStructures]
---

# 数据结构

线性结构包括：数组、队列、链表、栈

非线性结构包括：二维数组、多维数组、广义表、**树结构**、**图结构**

## 线性结构：数组

稀疏数组：当一个数组中的大部分元素为0或同一个值的数组时，可以使用稀疏数组来保存该数组。 

​		具体做法：首先记录该数组一共有几行几列，有多少个不同的值，再把不同的值的元素的行列和值记录到一个更小规模的数组中，从而缩小程序的规模。

代码实现：

```java
public class SparseArray {
    public static void main(String[] args) {
        int chessArr1[][] = new int[11][11];
        chessArr1[1][2] = 1;
        chessArr1[2][3] = 2;
        for (int[] ints : chessArr1) {
            for (int anInt : ints) {
                System.out.print(anInt);
            }
            System.out.println();
        }
        //    转换为稀疏数组
        int sum = 0;
        for (int i = 0; i < 11; i++) {
            for (int j = 0; j < 11; j++) {
                if(chessArr1[i][j]!= 0){
                    sum++;
                }
            }
        }
        int sparseArr[][] = new int[sum+1][3];
        sparseArr[0][0] = 11;
        sparseArr[0][1] = 11;
        sparseArr[0][2] = sum;

        int count = 0;
        for (int i = 0; i < 11; i++) {
            for (int j = 0; j < 11; j++) {
                if(chessArr1[i][j]!= 0){
                    count++;
                    sparseArr[count][0] = i;
                    sparseArr[count][1] = j;
                    sparseArr[count][2] = chessArr1[i][j];

                }
            }
        }
        System.out.println("稀疏数组如下：");

        for (int i = 0; i < sparseArr.length; i++) {
            System.out.printf("%d\t%d\t%d\t\n",sparseArr[i][0],
                    sparseArr[i][1],sparseArr[i][2]);
        }
        System.out.println();
        System.out.println("恢复为二维数组");

        int chessArr2[][] = new int[sparseArr[0][0]][sparseArr[0][1]];
        for (int i = 1; i < sparseArr.length; i++) {
            chessArr2[sparseArr[i][0]][sparseArr[i][1]] = sparseArr[i][2];
        }
        for (int[] ints : chessArr2) {
            for (int anInt : ints) {
                System.out.printf("%d\t",anInt);
            }
            System.out.println();
        }
    }
}

```

## 线性结构：队列

队列是一个有序列表，可以由数组或者链表来实现  **列表遵循先入先出的原则**

数组模拟环形数组，代码如下：

```java
public class CricleArrayQueueDemo {
    public static void main(String[] args) {
        CricleArrayQueue arrayQueue = 
            new CricleArrayQueue(3);
        char key = ' '; // 接收用户输入
        Scanner scanner = new Scanner(System.in);
        boolean loop = true;
        while (loop) {
            System.out.println("s(show):显示队列");
            System.out.println("e(exit):退出程序");
            System.out.println("a(add):添加数据到队列");
            System.out.println("g(get):从队列取出数据");
            System.out.println("h(head):查看队列头的数据");
            key = scanner.next().charAt(0);
            switch (key) {
                case 's':
                    arrayQueue.showQueue();
                    break;
                case 'a':
                    System.out.println("输入一个数字");
                    int value = scanner.nextInt();
                    arrayQueue.addQueue(value);
                    break;
                case 'e':
                    scanner.close();
                    loop = false;
                    break;
                case 'g':
                    try {
                        int queue = 
                            arrayQueue.getQueue();
                        System.out.println(
                            "取出的数据为：" + queue);
                    } catch (Exception e) {
                        System.out.println(e.getMessage());
                    }
                    break;
                case 'h':
                    try {
                        int headQueue = 
                            arrayQueue.headQueue();
                        System.out.printf(
                            "队列头的数据时%d\n", headQueue);
                    } catch (Exception e) {
                        System.out.println(e.getMessage());
                    }
                    break;
                default:
                    break;
            }
        }
    }
}

class CricleArrayQueue {
    private int maxSize;
    private int front;
    private int rear;
    private int[] arr;


    public CricleArrayQueue(int arrMaxSize) {
        maxSize = arrMaxSize;
        arr = new int[maxSize];
        front = 0;  // 指向队列头部 队列头的前一个位置
        rear = 0;  // 指向队列尾部的数据，包含该数据
    }

    public boolean isFull() {
        return (rear + 1) % maxSize == front;
    }

    public boolean isEmpty() {
        return rear == front;
    }

    public void addQueue(int n) {
        if (isFull()) {
            System.out.println("队列已满");
            return;
        }
        arr[rear] = n;
        rear = (rear + 1) % maxSize;
    }

    public int getQueue() {
        if (isEmpty()) {
            throw new RuntimeException("队列为空，不能取数据");
        }
        int temp = arr[front];
        front = (front + 1) % maxSize;
        return temp;
    }
    public void showQueue() {
        if (isEmpty()) {
            System.out.println("没有数据，无法显示");
            return;
        }
        for (int i = front; i < front + size(); i++) {
            System.out.printf("arr[%d]=%d\n",
                              i % maxSize, arr[i % maxSize]);
        }

    }
    // 求出当前队列的有效数据的个数
    public int size() {
        return (rear + maxSize - front) % maxSize;
    }
    public int headQueue() {
        if (isEmpty()) {
            throw new RuntimeException("无数据");
        }
        return arr[front];
    }
}
```



## 线性结构：链表

带头节点的链表，插入有顺序的链表，首先创建一个节点类：

```java
class HeroNode {
    public int no;
    public String name;
    public String nickname;
    public HeroNode next;// 指向下一个节点

    public HeroNode(int no, String name, String nickname) {
        this.no = no;
        this.name = name;
        this.nickname = nickname;
    }

    @Override
    public String toString() {
        return "HeroNode{" +
                "no=" + no +
                ", name='" + name + '\'' +
                ", nickname='" + nickname + '\'' + '}';
    }
}
```

无序的插入方法如下：

```java
public void add(HeroNode heroNode) {
    HeroNode temp = head;
    while (true) {
        if (temp.next == null) {
            break;
        }
        temp = temp.next;
    }
    temp.next = heroNode;
}
```

有序的插入方法如下：

首先我们需要一个变量来临时接收新加入的节点，将**新节点的next**指向temp下一个节点，将temp的下一个节点，指向新节点，这里的顺序不能改变。

```java
public void addByOrder(HeroNode heroNode) {
    HeroNode temp = head;
    boolean flag = false;
    while (true) {
        if (temp.next == null ) {
            break;
        }
        if(temp.next.no>heroNode.no){
            break;
        }else if ((temp.next.no == heroNode.no)){
            flag = true;
            break;
        }
        temp = temp.next;
    }
    if(flag){
        System.out.println("要插入的英雄的编号已经存在");
    }else {
        heroNode.next = temp.next;
        temp.next = heroNode;
    }
}
```

链表的删除操作：

我们找到链表的下一个节点与我们要删除的节点编号一致，将这个节点next指向下下一个节点，垃圾回收机制会直接删除中间的这个节点；

```java
 public void del(int no){
       HeroNode temp = head;
       boolean flag = false;
       while (true){
           if(temp.next == null){
               break;
           }
           if(temp.next.no == no){
               flag = true;
               break;
           }
           temp = temp.next;
       }
       if (flag){
           temp.next = temp.next.next;
       }else {
           System.out.println("没有找到");
       }
    }
```







