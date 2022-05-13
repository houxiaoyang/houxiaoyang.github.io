---
layout: post
title:  "构造器、Java this "
subtitle: 
cover-img: 
thumbnail-img:
share-img: 
date: 2021-12-17 20:56:26 +0800
tags: [Java, 构造器, java this]
---

#### 构造器（Constructor）：

构造器主要作用是完成对**新对象的初始化**，（并不是去创建对象）基本语法：

```java
[修饰符] 方法名 (形参列表){
    方法体;
}
```

- 构造器的修饰符可以默认；
- **构造器没有返回值；※**
- 方法名和类名字必须一样；
- 参数列表和成员方法一样的规则；
- 构造器的调用系统完成。

```java
class Person{
	String name;
	int age;
	public Person(String pName,int pAge){
		name = pName;
		age = pAge;
	}
}
```

**构造器名称和类名称一样，都是`Person`**

没有返回值**（不要写`void`）**；`String pName,int pAge`是构造器形参列表。

当我们创建对象时，直接用构造器来指定人的名字：

`Person p1 = new Person("belle",27);`

构造器可以构造器的重载。（可以只指定名字而不指定年龄，其他都保持一致）；

构造器注意细节：

- **如果程序员没有定义构造器，系统会自动给类生成一个默认无参构造方法（默认构造方法）。**

​       **举例：`Person(){}`，** **`javap`反编译**

我们经常使用的`Person p1 = new Person()` 这里的`Person()`  就是在调用默认无参构造器

`Person p1 = new Person("belle",27);` 这是我们已经自己定义的构造器。

- **一旦定义了一个构造器，默认构造器就被覆盖了，就不能再使用默认的无参构造器**；

```java
class Person{
	int age = 90;
	String name;
	public Person(String n,int a){
		name = n;
		age = a;
	}
}
Person p = new Person("belle",20);
```

**流程分析：**

1. 加载Person类信息（`Person.class`），只加载一次；

2. 在堆中分配空间（地址）；

3. 完成对象的初始化：{1.默认初始化`age = 0,name = null;`

   ​                                    2.显示初始化：`age = 90,name = null;`

   ​                                    3.构造器初始化：`age = 20,name = "belle"`}

4. 在对象在堆中的地址返回给`p`   (`p`为对象名，或对象的引用)

#### java this关键字：

java虚拟机给每个对象分配this，代表当前对象。

```java
class Person{
	int age = 90;
	String name;
	public Person(String name,int age){
		this.name = name;
		this.age = age;
	}
}
Person p = new Person("belle",20);
```

`this.name = name;`  传进来的`name`赋值给当前构造器的`this.name`，**谁调用，this就代表谁**

**this注意事项、使用细节：**

1. this关键词可以用来访问本类的属性、方法、构造器；

2. **this用于区分当前类的属性和局部变量；**

3. 访问成员方法的语法：`this.方法名(参数列表)；`

4. **※访问构造器语法**：`this(参数列表)`；**注意只能在构造器中使用**，**且这条语句必须置于第一条语句**

  ```java
  class Tools{
      public Tools(){
          this("jack",100);
      }
      public Tools(String name,int age){
          System.out.println("hello")
      }
  }
  ```

5. this不能再类定义的外部使用，只能在类定义的方法中使用。

#### 练习：

```java
public class TestPerson{
	public static void main(String[] args){
		Person p1 = new Person("marry",20);
		Person p2 = new Person("belle",30);
		System.out.println("whether p1 and p2 are the same ?"+p1.compareto(p2));
	}
}
class Person{
	int age ;
	String name;
	public Person(String name,int age){
		this.name = name;
		this.age = age;
	}
	public boolean CompareTo(Person p){
			return this.name.equals(p.name) && this.age == p.age;
	}
}
```

`p1.CompareTo(p2)`  解释如下：

将`p2`作为实参传入`public boolean CompareTo(Person p){` 将和`p1` 传入的进行对比，如果相同，那么输出为`true`，不相同输出输出为`false` 

`return this.name.equals(p.name) && this.age == p.age;` 解释如下：

`this name`指的是`p1.CompareTo()` 中的`p1` ，括号里的`p.name` 是我们传入的实参，即上段中的`p2` 。

`名称.equals()`判断两个字符串是否相同，`&&`表示两者必须全部相同（短路与）才能输出`true`.

（`boolean`默认输出的为`false`）
