---
layout: post
title:  " main方法、代码块 "
subtitle: 
cover-img: 
thumbnail-img:
share-img: 
date: 2021-12-28 15:01:26 +0800
tags: [Java, main, 代码块]
---

#### 深入理解main方法：

main方法的形式：

```java
public static void main(String[] args) {}
```

1. main方法是虚拟机调用。< 深入理解Java虚拟机 >
2. JVM需要调用类的main方法，所以该方法访问权限必须是`public` ；
3. JVM在执行main()方法时不必创建对象，所以该方法必须是`static`；
4. 该方法接收到String类型的数组参数，该数组中保存执行java命令时传递给所运行的类参数；
5. Java执行程序 参数1 参数2 参数3 ；
6. `void`  不需要返回值；

注意：在main()方法中可以直接调用main方法所在类的静态属性。但是**不能直接访问该类的非静态成员**，我们需要创建一个实例（对象）才可以调用非静态成员。



#### 代码块：

代码块有称为**初始化块**，属于类中的成员，即是类的一部分，类似与方法，将逻辑语句封装在方法体中，通过{}方式包围起来。

但和方法不同，没有方法名，没有返回，没有参数，只有方法体，而且不用通过对象或类显式调用，而是加载类时或者创建对象隐式调用。（只有方法体的方法）

基本语法：

```java
[修饰符]{
    代码
};
```

注意：

- 修饰符可选，如果选择写只能写`static`
- 代码块分为两类，使用`static`修饰的叫做静态代码块，没有`static`修饰的叫做普通代码块。
- 逻辑语句可以为任何逻辑语句（输入输出方法调用、循环判断等）
- `；`可以写也可省略

优点：相当于是**对构造器的补充机制**，可以做初始化的操作，代码块的调用优先于构造器。

无论调用哪个构造器创建对象，都会优先调用代码块，在调用构造器。

#### 代码块细节分析：

1. static代码块也叫做静态代码块，作对就是对类进行初始化，而且随着类的加载而执行，**并且只会执行一次**，如果是普通代码块，则每创建一个对象就执行一次。

2. **※※类什么时候被加载：1.创建对象实例时（new）**。

   **2.创建子类对象实例，父类也会被加载，父类优先于子类先加载代码块。**

   **3.使用类的静态成员时，静态成员包括静态属性和静态方法。**

3. **普通代码块在创建对象实例时，会被隐式的调用**。被创建一次，就会调用一次。如果只是使用类的静态成员时，普通代码块并不会执行。

**static代码块时类加载时执行，只会执行一次，普通代码块时创建对象时调用，每创建一个调用一次。**

4. 创建一个对象时，在一个类调用顺序是：

   1.调用静态代码块和静态属性初始化（注意：静态代码块和静态属性初始化调用的优先级一样，如果有多个静态代码块和多个静态变量初始化，则按照他们定义的顺序调用）（先后顺序）

   ```java
   class A{
       private static int n1 = getN1();
       static {
           System.out.println("A 静态代码块被调用");
       }
       public static int getN1(){
           System.out.println("getN1被调用");
           return 100;
       }
   }
   //A a = new A();首先输出 getN1被调用  再输出 A 静态代码块被调用
   ```

   

   2.调用普通代码块和普通属性的初始化（注意：普通代码块和普通属性初始化调用的优先级一样，如果有多个普通代码块和多个普通属性初始化，则按照他们定义的顺序调用）

   ```JAVA
   class A{
       private static int n1 = getN1();
       {
           System.out.println("A 普通代码块被调用");
       }
       private int n2 = getN2();
   
       static {
           System.out.println("A 静态代码块被调用");
       }
       public static int getN1(){
           System.out.println("getN1被调用");
           return 100;
       }
       public int getN2(){
           System.out.println("getN2被调用");
           return 200;
       }
       public A(){
           System.out.println("A() 构造器被调用");
       }
   
   }//输出顺序：
   getN1被调用  A 静态代码块被调用 A 普通代码块被调用 getN2被调用  A() 构造器被调用
   ```

   3.调用构造方法。

5.  构造器之前存在super() 和调用普通代码块。

```java
class A{
    public A(){
        //1.首先调用super()。。需要完成父类的super()、普通代码块
        //2.调用本类普通代码块
         System.out.println("hello");
    }
}
```

6.创建一个子类对象时（继承关系），他们的静态代码块，静态属性初始化，普通代码块，普通属性初始化，构造方法的调用顺序如下：

> 1.父类的静态代码块和静态属性（优先级一样，按照定义顺序执行）
>
> 2.子类的静态代码块和静态属性（优先级一样，按照定义顺序执行）
>
> 3.父类的普通代码块和普通属性初始化（优先级一样，按照定义顺序执行）
>
> 4.父类构造方法
>
> 5.子类的普通代码块和普通属性初始化（优先级一样，按照定义顺序执行）
>
> 6.子类的构造方法

7.静态代码块只能直接调用静态成员，普通代码块可以调用任意成员。