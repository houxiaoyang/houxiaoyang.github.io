---
layout: post
title:  " 局部内部类 "
subtitle: 
cover-img: 
thumbnail-img:
share-img: 
date: 2021-12-30 12:05:26 +0800
tags: [Java, 局部内部类]
---

#### 内部类（Inner Class）:

一个类的内部又完整的嵌套了另一个类结构，被嵌套的类称为内部类，嵌套的其他类称为外部类。

**类的五大成员：属性、方法、构造器、代码块、内部类**

**※※内部类的最大特点就是可以直接访问私有属性，并且可以体现类与类之间的包含关系。※※**

```java
class Outer{//外部类
    class Inner{//内部类
        
    }
}
class Other{//外部其他类
    
}
```

#### 四种内部类：

**定义在外部类局部位置上（比如方法内）**：1. 局部内部类（有类名） `※※※2.匿名内部类（无类名）`

**定义在外部类的成员位置上：** 1.成员内部类（没有static修饰） 2.静态内部类（使用static修饰）

#### 1.局部内部类：局部内部类是定义在外部类的局部位置，例如方法中，有类名[本质是类]

1.1 举例如下：`class Outer 外部类`   `class Inner  内部类` **定义在外部类的m1方法中**

```java
class Outer {
    private int n1 = 99;//私有属性
    private void m2(){//私有方法
        
    }
    public void m1() {//外部类的一个方法
        class Inner {//内部类
            public void f1() {//内部类的方法
                System.out.println("n1 = " + n1);//
                m2();//可以直接调用内部的私有方法
            }
        }
    }
}
```

1.2 不能添加访问修饰符，因为它的地位就是一个局部变量，局部变量不使用修饰符（实际上和局部属性一样，作用范围仅仅在这个外部类中）。但是可以final修饰，（意味着我们不希望这个内部类被继承）。

1.3 作用域仅仅在它定义的方法或者代码块中

1.4局部内部类可以直接访问外部类的成员

1.5外部类访问局部内部类的成员：

只能在外部类`Outer`方法中创建局部内部类`Inner`的对象，调用对象即可（必须在作用域内）

```java
class Outer {
    private int n1 = 99;
    private void m2(){
        System.out.println("私有方法m2 被调用");

    }
    public void m1() {
        class Inner {
            public void f1() {
                System.out.println("n1 = " + n1);
                m2();
            }
        }
        //****我们在外部类中创建内部类的对象，调用其方法****
        Inner inner = new Inner();
        inner.f1();
    }
}
```

运行时我们只需要创建外部类的对象，调用外部类的方法`m1`即可：

```java
public class TestExercise {
    public static void main(String[] args) {
        Outer outer = new Outer();
        outer.m1();
    }
}
```

1.6外部其他类不能访问局部内部类。

1.7 如果外部类和局部内部类的成员重名是，遵循就近原则;

如果想要访问外部类的成员，使用 `外部类名.this.成员` 去访问 举例如下：

```java
class Outer {
    private int n1 = 99;
    public void m1() {
        class Inner {
            private int n1 = 100;
            public void f1() {
                System.out.println("n1 = " + n1);//结果100
                System.out.println("外部类的n1= "+ Outer.this.n1);//99
            }
        }
    }
}
```

 `Outer.this`  就是外部类的对象，谁在调用m1方法，就是它的对象。（就是我们上例创建的outer）











