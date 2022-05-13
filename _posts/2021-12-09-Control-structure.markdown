---
layout: post
title:  "程序控制结构"
subtitle: 
cover-img: 
thumbnail-img:
share-img: 
date: 2021-12-09 22:27:26 +0800
tags: [Java, 控制结构]
---

#### 单分支控制结构：

```java
import java.util.Scanner;
public class If01{
    public static void main(String[] args){
        Scanner myScanner = new Scanner(System.in);
        System.out.println("Please enter your age");
        int age = myScanner.nextInt();
        if(age > 18){
            System.out.println("Your age is greater than 18");
        }
        else{
            System.out.println("Your age is less than 18");
        }
    }
}

```

编写一个程序，声明两个double型变量并赋值，判断第一个数大于10.0，第二个数小于20.0，打印两数之和。

```java
public class Ecercise01{
    public static void main(String[] args){
        double n1 = 15.0;
        double n2 = 16.0;
        if(n1 > 10.0 && n2 < 20.0){
                System.out.println(n1 + n2);
        }
    }
}
```

<!--多分支：举例如下：-->

```java
import java.util.Scanner;
public class If01{
    public static void main(String[] args){
        Scanner myScanner = new Scanner(System.in);
        System.out.println("please enter your price");
        int price = myScanner.nextInt();
        if(price >= 1 && price <= 100){
            if(price == 100){
                System.out.println("very good");
            }else if(price > 80 && price <= 99 ){
                System.out.println("excellent");
            }else if (price >= 60 && price <= 80){
                System.out.println("generally");
            }else{
                System.out.println("Unqualified");
            }
        }else{
            System.out.println("Input error");
        }
    }
}
```

嵌套分支（最好不要超过三层）

```java
import java.util.Scanner;
public class If01{
    public static void main(String[] args){
        Scanner myScanner = new Scanner(System.in);
        System.out.println("please enter your score");
        double score = myScanner.nextDouble();
        if(score > 8.0){
            System.out.println("please enter your gender");
            char gender = myScanner.next().charAt(0);
            if(gender == 'm'){
                 System.out.println("Congratulations on entering the Men's Team final");
            }
            else{
                System.out.println("Congratulations on entering the Women's Team final");
            }
        }else{
            System.out.println("You didn't make it to the final");
        }

    }
}
```

 `char gender = myScanner.next().charAt(0);`这里是实现把字符串换成字符，`myScanner.next()`字符串，把他的第一个字符得到`charAt(0)`

#### switch流程：

**穿透：**如果在执行第一个语块后没有`break`，那么就直接执行第二个语块（不会判断是否匹配第二常量），依次执行所有语块，直到`default`语块，结束`switch.`  

```java
import java.util.Scanner;
public class If01{
    public static void main(String[] args){
        Scanner myScanner = new Scanner(System.in);
        System.out.println("please enter (a~c)");
        char c1 = myScanner.next().charAt(0);
        switch(c1){
            case 'a':
                 System.out.println("1");
                 break;
            case 'b':
                 System.out.println("2");
                 break;
            case 'c':
                 System.out.println("3");
                 break;
            default:
                 System.out.println("error");
        }
    }
}
```

**switch细节讨论：**

1. 表达式数据类型，应该和case后的常量类型一致，或者是可以自动转换成可以相互比较的类型，比如输入的是字符，常量是int.

2. switch(表达式)中表达式的返回值必须是（`byte`,`short`,`int`,`char`,`enum(枚举)`，`String`）

   ````java
   double c = 1.1;
   Switch(c){    //double类型不能是返回值  所以是错误的
       case 2.1   //错误
   }
   ````

3. case中的值必须是一个常量（常量表达式也可），不能是变量。

4. default语句是可选的，也可以没有。

5. break语句用来执行完成一个case分支后让程序结束跳出switch语块，没有break的话就会一直执行到结束**（穿透）**。

#### for循环控制：

   ```java
   for(循环变量初始化;循环条件;循环变量迭代){
        循环操作(可以多条语句)
   }
   ```

   ```java
   for(int i = 1;i <= 10;i++){
       System.out.println("hello,world");
   }
   //以下方法同样适用：
   int i = 1
   for( ;i <= 10;){
      System.out.println("hello,world");
       i++;
   }
   ```

 {: .box-note}
**Note:** `for(;;)`表示无限循环。

<!--举例练习题：-->

```java
public class For01{
    public static void main(String[] args){
    	int count = 0;
    	int sum = 0;
		for(int i = 1;i <= 100;i++){
		    if(i % 9 == 0){
		    	count++;
		    	sum += i;
		    }	
		}
		System.out.println("times="+count+","+"sum="+sum);
    }
}
```

#### while循环控制：

```java
循环变量初始化;
while(循环条件){
    循环体(语句);
    循环变量迭代;
}
```

```java
public class While{
    public static void main(String[] args){
        int i = 40;
        while(i <= 200){
            if(i % 2 == 0){
                System.out.println(+i);
            }
            i++;
        }
    }
}
```

#### do..while循环控制：

```java
循环变量初始化;
do{              //先执行，在判断
    循环体(语句);
    循环变量迭代;
}while(循环条件);//  一定会执行一次
```

```java
public class DoWhile{
    public static void main(String[] args){
        int i = 1;
        do{
            System.out.println("hello,world");
            i++;
            }while(i <= 10);//满足这个条件就一直执行
        } 
    }
```

```java
import java.util.Scanner;
public class While1{
    public static void main(String[] args){
        Scanner myScanner = new Scanner(System.in);
        char answer = ' '; // 这是一个空字符
        do{
            System.out.println("give me money?");
            answer = myScanner.next().charAt(0);
            System.out.println("answer="+answer);
            }while(answer != 'y' );
    }
}

```

#### 练习题：

```java
//九九乘法表：
public class mul{
	public static void main(String[] args){
		for(int i = 1;i <=9;i++){
			for(int j = 1;j <= 9 && j <= i;j++){
			    System.out.print(+j+"*"+i+"="+i*j+"\t");
			}//print("")为不换行
			System.out.println("");
		}
	}
}//println("")为换行
```

#### 跳转控制语句 break：

```java
{
....
break;
....
}//只会终止当前循环，不会终止所有程序
```

break可以指定退出哪一层.**（不建议使用。可读性变差）**

```java
label1:{
label2   {
           break label;
  }//指定推出label1循环
}//若没有标签，就近原则，选最近的退出
```

```java
public class test{
	public static void main(String[ ] args){
		int answer = 0;
        //int n = 0;
		for(int i = 0;i <= 100;i++){
			answer += i;
			if(answer > 20){
		        System.out.println(i);
                //n = i; 这里将i赋值给n 
		        break;
			}
		}
    }//System.out.println(i) 这里会报错，注意i的作用域范围
}//若想要在循环体外使用这个值，在循环体内将i赋给一个值
```

###### 三次登陆机会测试：

```java
import java.util.Scanner;
public class test{
	public static void main(String[ ] args){
		Scanner myScanner = new Scanner(System.in);
		String name = "";
		String password = "";
		for(int chance = 1;chance <= 3;chance++){
			System.out.println("Your name please?");
		    name = myScanner.next();
		    System.out.println("Your password please?");
		    password = myScanner.next();
		    if("cuilongyang".equals(name)&&"666".equals(password)){
      //"".equals(name)用于比较这两个值是否相等，若相等输出为True
		    	System.out.println("Congratulations");
		    	break;
		    }else{
		    	System.out.println("You only have "+(3-chance)+" chances");
		    }
		}
	}
}
```

#### 跳转控制语句 continue：

**结束当前循环，直接继续进行下次循环。**

<!--举例如下：-->

```java
public class test{
	public static void main(String[ ] args){
		int i = 1;
		while(i <= 4){
			i++;
			if(i == 2){
				continue;}//跳过i=2，继续进行循环
			System.out.println("i="+i);
		}
	}//输出的结果为：i=3，i=4，1=5
}
```

#### 跳转控制语句 return：

以下为break语句：
```java
public class test{
	public static void main(String[ ] args){
		for(int i = 1;i <= 5;i++){
			if(i==3){
				System.out.println("cuilongyang"+i);
				break;
			}
			System.out.println("hello");
		}
		System.out.println("go on ");
	}//输出hello，hello，cuilongyang3,go on
}
```

以下为continue语句：
```java
public class test{
	public static void main(String[ ] args){
		for(int i = 1;i <= 5;i++){
			if(i==3){
				System.out.println("cuilongyang"+i);
				continue;
			}
			System.out.println("hello"+i);
		}
		System.out.println("go on ");
	}//输出hello1,hello2,cuilongyang3,hello4,hello5,go on
}// 
```

**以下为return语句：**

```java
public class test{
	public static void main(String[ ] args){
		for(int i = 1;i <= 5;i++){
			if(i==3){
				System.out.println("cuilongyang"+i);
				return;
			}//return用在方法时跳出方法，如果用在主方法main，退出程序
			System.out.println("hello"+i);
		}
		System.out.println("go on ");
	}//输出 hello1，hello2，cuilongyang3  后续都不会进行。
}
```

**return用在方法时跳出方法，如果用在主方法main，退出程序...**
