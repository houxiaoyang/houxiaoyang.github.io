---
layout: post
title:  " 匿名内部类（Anonymous Inner Class） "
subtitle: 
cover-img: 
thumbnail-img:
share-img: 
date: 2021-12-30 23:23:26 +0800
tags: [Java, 匿名内部类, 静态内部类]
---

#### 匿名内部类：定义在外部类的局部位置，比如方法中，没有类名

**1. 本质是类  2. 内部类   3. 该类没有可见的名字  4. 同时还是一个对象** 

1.基本语法：

```java
new 类或接口(参数列表){
    类体
}
```

举例基于接口：编译类型`IA`  运行类型就是匿名内部类**`Outer$1`**  我们只需要用一次之后就不再使用，简化开发

```java
interface IA{
    void Cry();
}
class Outer{
    private int n1 = 10;
    public void Method(){
        IA tiger = new IA(){
            @Override
            public void Cry() {
                System.out.println("老虎叫~~~");
            }
        };
        tiger.Cry();
    }
}
```

 `IA tiger = new IA()`  我们新建一个接口，后跟一个方法体`{}`，直接将接口内的方法重写该方法。

JDK在底层创建匿名内部类，立即创建该实例，并且把地址返回给tiger。匿名内部类使用一次就没有了（类没有了）但是对象可以随便调用。        `tiger.Cry();` 可以随时调用

举例基于类：Father 不是抽象类，所以可以匿名实现，如果是基于抽象类的匿名内部类，则必重写其内部的抽象方法。

```java
class Outer{
    private int n1 = 10;
    public void Method(){
        Father father = new Father("clyde"){
            @Override
            public void test() {
                System.out.println("重写了test");
            }
        };
        father.test();
    }
}
class Father{
    public Father(String name){
    }
    public void test(){
    }
}
```

#### 细节分析：

1.除了前述方法调用外，还可以直接调用：

```java
class Outer{
    public void Method(){
        new Person("belle"){
            @Override
            public void test() {
                System.out.println("重写调用了test方法");
            }
        }.test();
    }
}
```

2.可以直接访问外部类的所有成员，包括私有的

3.不能添加访问修饰符，因为它的低位就是局部变量。

4.作用域：仅仅在定义它的方法或代码块中使用。

5.如果外部类和局部内部类的成员重名是，遵循就近原则;

如果想要访问外部类的成员，使用 `外部类名.this.成员` 去访问 举例如下:

```java
class Outer {
    private int age = 100;

    public void Method() {
        new Person("belle") {
            private int age = 99;

            @Override
            public void test() {
                System.out.println("内部的age= " + age);
                System.out.println("外部类age= " + Outer.this.age);
            }
        }.test();
    }
}
```



实践：

直接当做参数传递；

```java
public class TestExercise {
    public static void main(String[] args) {
        Test(new IA() {
            @Override
            public void Show() {
                System.out.println(" 实参");
            }
        });
    }
    public static void Test(IA ia){
        ia.Show();
    }
}
interface IA {
    void Show();
}
```



#### 成员内部类：定义在外部类的成员位置上：

```java
public class TestExercise {
    public static void main(String[] args) {
        Outer outer = new Outer();
        outer.test();
    }
}

class Outer {//外部类
    private int age = 10;
    public String name = "belle";

    class Inner {//成员内部类
        public void say() {
            System.out.println(" age= " + age + " name= " + name);
        }
    }

    public void test() {//外部类的一个成员
        Inner inner = new Inner();
        inner.say();
    }
}
```



 `class Inner`  可以添加任意修饰符

在成员外部类创建成员内部类对象，直接调用内部类的方法

#### 外部其他类，使用成员内部类的两种方式：

1.`outer.new Inner()` 语法中，`new Inner()` 是我们创建的`outer`的成员

```java
public class TestExercise {
    public static void main(String[] args) {
        Outer outer = new Outer();
        Outer.Inner inner = outer.new Inner();
        inner.say();
    }
}
```



```java
class Outer {
    private int age = 10;
    public String name = "belle";

     class Inner {
        public void say() {
            System.out.println(" age= " + age + " name= " + name);
        }
    }
}
```

2.第二种，直接在外部类中编写一个方法，返回Inner对象：

```java
class Outer {
    private int age = 10;
    public String name = "belle";

     class Inner {
        public void say() {
            System.out.println(" age= " + age + " name= " + name);
        }
    }//如下所示：
    public Inner getInnerInstance(){
         return new Inner();
    }
}
```

调用该方法即可：

```java
        Outer.Inner innerInstance = new Outer().getInnerInstance();
        innerInstance.say();
```



#### 静态内部类：

1.放在外部类的成员位置  2.用static修饰  3.可以直接访问外部类的所有静态成员，不能直接访问非静态成员

```java
class Outer {
    private  int age = 10;//非静态成员，报错！！
    public static String name = "belle";//可以访问

     static class Inner {
        public void say() {
            System.out.println(" age= " + age + " name= " + name);
        }
    }
}
```

3.可以添加任何访问修饰符，因为他是一个成员

4.作用域为整个类体

```java
    public void hi(){
        Inner inner = new Inner();
        inner.say();
    }
```



5.外部类访问内部类，需要先创建对象 

  5.1 因为静态内部类，可以通过类名直接访问，前提是有访问权限

```java
public class TestExercise {
    public static void main(String[] args) {
        Outer outer = new Outer();
        Outer.Inner inner = new Outer.Inner();
        inner.say();
    }
}
```

5.2 编写一个方法返回静态内部类的实例：

```java
    public Inner getInnerInstance(){
         return new Inner();
    }
```



