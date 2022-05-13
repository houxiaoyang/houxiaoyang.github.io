---
layout: post
title:  "递归调用：汉诺塔"
subtitle: 
cover-img: 
thumbnail-img:
share-img: 
date: 2021-12-16 20:59:26 +0800
tags: [Java, 递归, 汉诺塔]
---

#### 汉诺塔：

```java
public class HannoTower{
	public static void main(String[] args){
		Tools test = new Tools();
		test.move(3,'A','B','C');
	}
}
class Tools{
	public void move(int num,char a,char b,char c){
   //以上这个顺序可以理解为，盘个数，开始点，中转点，最终点
		if(num == 1){
        //如果只有一个盘，直接将a移动到c即可
			System.out.println(a+"=>"+c);
		}else{//多个盘的情况
      //看作两个盘，一个组合盘（除了最底盘之外的所有盘）个数为（num-1），一个最底盘
      //将这个组合盘（num-1）从a移到b盘，如下所示：
            move(num-1,a,c,b);
      //再将最底盘，从a盘移动到c盘，如下所示：
			System.out.println(a+"=>"+c);
      //再将组合盘（num-1）从b盘移动到c盘，如下所示：
            move(num-1,b,a,c);      
		}
	}
}
```



