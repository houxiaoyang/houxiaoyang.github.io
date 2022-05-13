---
layout: post
title:  "控制流程练习题"
subtitle: 
cover-img: 
thumbnail-img:
share-img: 
date: 2021-12-10 17:42:26 +0800
tags: [Java, 练习题]
---

#### 水仙花数：

判断一个三位数是否是水仙花数，水仙花数即各个位上数字立方和等于其本身：

```java
import java.util.Scanner;
public class ex4{
	public static void main(String[ ] args){
		Scanner myScanner = new Scanner(System.in);
		System.out.println("enter your number:"); 
		int daffodil = myScanner.nextInt();
		if(daffodil > 100){
			int h = (int)(daffodil/100);
			int d = (int)((daffodil-h*100)/10);
			int o = daffodil-h*100-d*10;
        //更好的办法：百位：n/00 ；十位：n%100/10  个位：n%10
			if(h*h*h + d*d*d + o*o*o == daffodil){
        //更好的办法：Math.pow(a，b)  计算a的b次方
			 	System.out.println(+daffodil+"is a daffodil number");
			}
		}else{
			System.out.println(+daffodil+"is not a daffodil number");
		} 
	}
}
```



#### 输出1-100之间的不能被5整除的数，每五个一行：

```java
public class ex6{
	public static void main(String[ ] args){
		int count = 0; 
		for(int i = 1;i <= 100;i++){
			if(i % 5 != 0){
				count++;
				System.out.print(i+"\t");
			}
            if(count % 5 == 0){
            	System.out.println();
            }
		}
	}
}
```



#### 输出小写a-z以及大写的Z-A：

```java
public class ex7{
	public static void main(String[ ] args){    
            for(char c1 = 'a';c1 <= 'z';c1++){
         //字符可以用来比大小，本质是一个数字
                System.out.print(c1+" ");
            }
            System.out.println();
            for(char c2 = 'Z';c2 >= 'A';c2--){
            	System.out.print(c2+" ");
            }
	}
}
```



#### 计算1-(1/2)+(1/3)-(1/4).....(1/100):

```java
public class ex8{
	public static void main(String[ ] args){    
        double sum = 0;
        for(int i = 1;i <= 100;i++){
            if(i % 2 != 0){
                sum += 1.0/i;
             //选择1为1.0 提高精度，或者改为double类型
            }else{
                sum -= 1.0/i;
            }
        }
        System.out.println("sum="+sum);
	}
}
```



#### 求1+(1+2)+(1+2+3)+(1+2+3+4)+...+(1+2+3+4+...+100)：

```java
public class ex9{
	public static void main(String[ ] args){
        int sum = 0;    
        for(int i = 1;i <= 100;i++){
            for(int j = 1;j <= i;j++){                
                sum += j;
            } 
        }           
        System.out.println("sum="+sum); 
	}
}
```

