---
layout: post
title:  "转义字符、注释"
subtitle: 
cover-img: 
thumbnail-img:
share-img: 
date: 2021-11-12 13:24:26 +0800
tags: [Java, 代码规范]
---
`\t`  一个制表位，实现对齐的功能

`\n` 换行符

`\\`在一个名称中间输出一个斜杠<!--(如果要输出两个斜杠`\`，那么我们需要写四个斜杠)-->

`\"`  输出一个“，同理`\'`表示输出一个'

`\r` 表示一个回车 ;回车加换行`\r\n`


```
public class ChangeChar {
	public static void main(String[] args) {
		System.out.println("name\tauthor\tprice\tamount\r\nTk\tLUO\t120\t1000");
	}
}
```

输出结果如下：
```
name    author  price   amount
Tk      LUO     120     1000
```

**一个合格的程序员应该有良好的编程习惯**

// 单行注释

/*  多行注释*/  ~~多行注释里不允许嵌套多行注释~~

文档注释：

```
/**
 *@author
 *@version 
 */
```

代码规范：

1. 类、方法的注释要以`javadoc`的方式来写；
2. 非`javadoc`的注释写给维护者查看；
3. 全选 `Tab` 整体向后移动，向左移动`shift+Tab`；
4. 源文件使用UTF-8的格式；
5. 行宽不要超过80字符；
6. 代码编写**次行风格**和**行尾风格**。（推荐行尾风格）

