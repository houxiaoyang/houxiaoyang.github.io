---
layout: post
title:  "嵌套循环实例（空心菱形）"
subtitle: ""
cover-img: 
thumbnail-img:
share-img: 
date: 2021-12-09 18:24:00 +0800
tags: [Java, 空心菱形]
---

代码如下：

```java
import java.util.Scanner;
public class pyramid{
	public static void main(String[] args){
		Scanner myScanner = new Scanner(System.in);
		System.out.println("what's level?");
		int total = myScanner.nextInt();
		for(int i = 1;i <= total;i++){
			for(int k = 1;k <= total - i;k++){
				System.out.print(" ");
			}//定义空格部分，每行比前一行多一个
            //此处相当于填空了前边的倒三角外部
			for(int j =1;j<=2*i -1;j++){
				if(j == 1||j==2*i-1){
				    System.out.print("*");
			    }else{//如果在开始或者结尾，那么就填上*
				    System.out.print(" ");
                }//其他部分填上空格
			}
		    System.out.println(" ");
		}//执行完毕后换行
		for(int i = 1;i <= total-1;i++){
			for(int k =1;k <= i;k++){
				System.out.print(" ");
			}//下半部分比上半部分少一行，所以-1
			for(int j = 1;j <= 2*(total-i)-1;j++){
				if(j == 1||j==2*(total-i)-1){
					System.out.print("*");
	            }else{//如果在开始或者结尾，那么就填上*
	             	System.out.print(" ");
	            }//否则就填上空格（此处相当于填空实心菱形内部）
	        }
			System.out.println(" ");	
		}
	}
}
```

