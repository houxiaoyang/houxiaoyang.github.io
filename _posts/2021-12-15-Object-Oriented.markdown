---
layout: post
title:  "类与对象学习笔记"
subtitle: 
cover-img: 
thumbnail-img:
share-img: 
date: 2021-12-15 15:05:26 +0800
tags: [Java, 面向对象]
---

#### 类与对象（OOP）：

**单独变量：**把信息拆解了，不利于对象的管理。

**数组：**数据类型无法体现；只能通过下标获取信息，造成变量名字和信息对应关系不明确，效率低；不能体现类型的行为。==>引出类与对象。（现有技术无法解决新的需求）

类就是数据类型，比如都属于猫类==>我们自定义的一个数据类型（`int`是Java定义的数据类型）

对象就是一个具体的实例。

**类到对象的几种说法：**1.创建一个对象；2.实例化一个对象；3.把类实例化....

java最大的特点就是面向对象。

```java
class Cat{  //定义猫类
	String name;//定义属性/成员变量/field（字段）
	int age;
	String color;
}//属性也可以是数组 String[]		
public class ObjectTest{
	public static void main(String[] args){
		Cat cat1 = new Cat();
		cat1.name = "Little";
		cat1.age = 3;
		cat1.color = "White";
		Cat cat2 = new Cat();
		cat2.name = "HuaHua";
		cat2.age = 100;
		cat2.color = "Black";
		System.out.println("first cat information :"+cat1.name
			+" "+cat1.age+" "+cat1.color);
		System.out.println("second cat information :"+cat2.name
			+" "+cat2.age+" "+cat2.color);
	}
}
```

- 属性的定义语法和变量相同，示例：`访问修饰符 属性类型 属性名；`   访问修饰符用来控制属性的啊访问范围==>

- **有四种访问修饰符**：`public`、`proctected`、`默认`、`private`
- 属性的类型包括数据类型和引用类型。

- 属性如果不赋值，是有默认值，和数据类型的默认值是相同的。

**创建对象的两个方式：**

- 先声明在创建：  `Cat cat`;   声明创建一个cat

​        `cat = new Cat()`  空间（目前为null）

- 直接创建： `Cat cat = new Cat()`

#### 类和对象的分配机制：

1.栈：存放基本数据类型

2.堆，存放对象（Cat，数组等）

3.方法区：常量池（常量，比如字符串），类加载信息

4.示意图：[(name,age,price)]

```java
Person p = new Person();//类信息只需要加载一次
p.name = "jack";
p.age = 10;
```

1.加载Person类信息，只需要加载一次。

2.在堆中分配空间，进行默认初始化

3.把地址赋给p，p就指向对象。

4.进行指定初始化，`p.name = "jack";`

#### 成员方法：

`public`  表示方法是公开的

`void:` 表示方法没有返回值

`speak()`   表示speak方法名，`()`叫形参列表

`{}`  方法体，可以写我们执行的代码

```java
class Person{
	String name;
	int age;
	public void speak(){
		System.out.println("hello")
	}
    public void cal{
        int sum = 0;
        for(int i = 1;i <= 1000;i++){
           sum += i; 
        }
        System.out.println(sum);
    }
}
```

 方法写好后，不去调用就不会输出：如何调用？创建一个对象，`Person p1 = new Person()` ,`p1.name` 调用。

**创建一个成员方法，该方法可以接收一个数字n，计算1+2+.....+n的值：**

```java
class Person{
    public void cal(int n){
        int sum = 0;
        for(int i = 1;i <= n;i++){
           sum += i; 
        }
        System.out.println(sum);
    }
}
```

调用该方法时：采用`p.cal(5)`  运行上述成员方法，运行1+2+....+5。

**添加成员方法，计算两个数之和：**

```java
class Person{
    public int GetSum(int num1,int num2){
        int sum = num1 +num2;
        return sum;
    }
}
```

 `public int GetSum(int num1,int num2){`   `int` 表示方法执行后，会返回一个`int`值

如何接收数值？`int returnsum = p.GetSum(10,20);`

方法调用小结：

1.当程序执行到方法时，就会开辟新的独立空间（栈空间）执行。

2.方法执行完毕，或者执行到return语句就返回。返回值只有一个。

3.返回到调用方法的地方。

4.返回后，继续执行后边的代码。

```java
class Mytools{
    public void printarray(int[][] map){
    	for(int i = 0;i < map.length;i++){
    		for(int j = 0;j < map[i].length;j++){
    			System.out.print(map[i][j]+" ");
    		}
    		System.out.println();
    	}
    }
}
```

直接调用该方法即可：

`Mytools tools = new Mytools();`
`tools.printarray(map);`

**语法格式：**

```java
public 返回数据类型 方法名 (形参列表){
    语句；
    return 返回值；//若是没有返回值，返回数据类型为void
}//public ==>访问修饰符
```

形参列表的细节：

- 一个方法可以有0个参数，或者多个参数。

- 调用时选择传入相同的类型。

- 调用时传入的参数就是实参，形参和实参类型要一致或兼容，个数、顺序需要一致。

**跨类调用，需要通过对象名调用：需要创建B类的对象，在调用B类的方法（和访问修饰符相关）**

