---
layout: post
title:  "成员方法传参机制、递归调用"
subtitle: 
cover-img: 
thumbnail-img:
share-img: 
date: 2021-12-16 12:26:26 +0800
tags: [Java, 递归, 算法]
---

#### 成员方法传参机制：

结论：基本数据类型，传递的是值（值拷贝），形参的任何改变不影响实参。

```java
class MyTools{
    public void swap(int a,int b){//形参
        int temp = a;
        a = b;
        b = temp;  
    }//void返回值
}
public class Test{
    public static void main(String[] args){
        int a = 10;
        int b = 20;//实参
        Tools pop = new Tools();
        pop.swap(a,b);
        System.out.println("a="+a+"\t b="+b);   
    }//这个输出时不会改变a和b的值，所以输出是 a = 10;b = 20;
}

```

#### 引用传参机制：※

结论：引用类型传递的是地址（传递的也是值，但是值是地址），**可以通过形参影响实参。**

```java
public class Test{
    public static void main(String[] args){
        MyTools Mydata = new MyTools();
        int[] arr = {1,2,3};
        Mydata.TestArray(arr);
        for(int i = 0;i < arr.length;i++){
            System.out.print(arr[i]+" ");
        }//由于类MyTools改变了数组arr的数据，导致我们在main类中arr也改变
    }//这是由于数组不是值拷贝，而是引用类型，所以类MyTools改变时，主方法也改变
}//所以这里输出的结果是200，2，3
class MyTools{
    public void TestArray(int[] arr){
        arr[0] = 200;
        for(int i = 0;i < arr.length;i++){
            System.out.print(arr[i]+" ");
        }//这里的arr指向的是一个地址，我们在方法中将arr[0]修改为了200；
    }//导致整个数组arr改变为了int[] arr={200,2,3};
}
```

#### 克隆对象：

```java
class Person{
    String name;
    int age;
}
class MyTools{
    public Person CopyPerson(Person p){
        Person p2 = new Person();
        p2.name = p.name;
        p2.age = p.age;
        return p2;
    }
}
public class Test{ 
    public static void main(String[] args){
        Person p = new Person();
        p.age = 15;
        p.name = "belle";
        MyTools tools = new MyTools();
        Person p2 = tools.CopyPerson(p);
        System.out.println("p age is"+p.age+"p name is"+p.name);
        System.out.println("p2 age is"+p2.age+"p2 name is"+p2.name);
    }
}
```

#### 方法递归（Recursion）调用：

递归就是方法自己调用自己，每次调用时传入不同的变量，递归有助于我们解决复杂问题，使代码简洁。

```java
public class Recursion{
	public static void main(String[] args){
		Test t1 = new Test();
		t1.test(4);
	}//后进的先行输出
}
class Test{
	public void test(int n){
		if(n > 2){
			test(n - 1);
		}
		System.out.println("n = "+ n);
	}
}//输出结果为 n = 2,n = 3,n = 4;
```

阶乘递归：

```java
public class Recursion{
	public static void main(String[] args){
		Test t1 = new Test();
		int MyData = t1.factorial(4);
		System.out.println("MyData = "+MyData);
	}
}
class Test{
	public int factorial(int n){
		if(n == 1){
			return 1;
		}else{
			return factorial(n-1)*n;
		}
	}
}
```

**递归的主要原则：**

1. 执行一个方法时，就创建一个新的受保护的独立空间（栈空间）；
2. 方法的局部变量时独立的，不会互相影响（举例n变量）；
3. 如果方法中使用的是引用类型变量（数组、对象）就会出现共享该引用类型的数据；
4. 递归必须向退出递归的条件逼近，否则就是无限递归，出现`StackOverflowError`；
5. 当一个方法执行完毕，或者遇到`return`，就会返回，遵守**谁调用，返给谁**，当方法执行完毕或者返回时，该方法也就执行完毕。

#### 练习：（斐波那契数列）

```java
public class Recursion{
	public static void main(String[] args){
		MyTools t1 = new MyTools();
		System.out.println(t1.fibonacci(7));
	}
}
class MyTools{
	public int fibonacci(int n){
		if(n >= 1){
			if(n == 1 || n==2){
				return 1;
			}else{
				return fibonacci(n-1) +fibonacci(n-2);
			}
		}
		else{
			System.out.println("Error ! ");
			return -1;
		}		
	}
}
```

#### 练习：猴子吃桃子：

```java
public class Recursion{
	public static void main(String[] args){
		MyTools t1 = new MyTools();
		System.out.println(t1.peach(1));
	}
}
class MyTools{
	public int peach(int day){
		if(day == 10){
			return 1;
		}else if(day >= 1 && day <= 9){
			return (peach(day + 1) + 1) * 2;
		}else{
			System.out.println("Day Error ! ");
			return -1;
		}		
	}
}
```

