---
layout: post
title:  " 正则表达式 "
subtitle: 
cover-img: 
thumbnail-img:
share-img: 
date: 2022-02-17 22:56:26 +0800
tags: [Java,regexp]
---

正则表达式：对字符串执行模式匹配的技术

```java
 String content = "1998年12月8日，第二代Java平台的企业版J2EE发布。1999年6月";

        // 匹配所有四个数字
        // 1.\\d  表示任意的数字
        String regStr= "\\d\\d\\d\\d";

        // 2.创建模式对象
        Pattern pattern = Pattern.compile(regStr);

        // 3.创建匹配器
        Matcher matcher = pattern.matcher(content);
       
		// 4.输出结果
		while (matcher.find()){
            System.out.println(matcher.group(0));
        }

```

### 源码分析：

1.  **matcher.find()**  分析

       * 1.根据规则定位满足规则的字符串（举例1998）
       * 2.找好后将 子字符串的开始的索引记录到 mather对象的属性 int[] groups;
       *   groups[0] = 0, 把该子字符串的结束的索引 +1 的值记录到 groups[1] = 4
       * 3.同时记录oldLast 的值为 子字符串的结束的索引 +1 的值  即为 4
       *   下次执行 find() 方法时，从 oldLast 位置开始匹配

2.   **matcher.group(0)**  分析

    ```java
    public String group(int group) {
                    if (first < 0)
                        throw new IllegalStateException("No match found");
                    if (group < 0 || group > groupCount())
                        throw new IndexOutOfBoundsException("No group " + group);
                    if ((groups[group*2] == -1) || (groups[group*2+1] == -1))
                        return null;
                    return getSubSequence(groups[group * 2], 
                                          groups[group * 2 + 1]).toString();
                }
    ```

    1. 根据 groups[0]= 0 和 groups[1]=4 的记录的位置，从 content 开始截取子字符串并返回  [0,4)
    
    

### 分组分析：

1. **matcher.find()**  分析

      * 1.根据规则定位满足规则的字符串（举例1998）

      * `(\\d\\d)(\\d\\d)` 表示分组，

      * 2.找好后将 子字符串的开始的索引记录到 mather对象的属性 int[] groups;

        - 2.1  groups[0] = 0, 把该子字符串的结束的索引 +1 的值记录到 groups[1] = 4

        - 2.2  记录1组（）匹配到的字符串groups[2] = 0   ,   groups[3] = 2  结束位置加1
        - 2.3  记录2组（）匹配到的字符串groups[4] = 2   ,   groups[5] = 4  结束位置加1
        - 同时记录oldLast 的值为 子字符串的结束的索引 +1 的值  即为 4

      * 下次执行 find() 方法时，从 oldLast 位置开始匹配

小结：

1. 如果正则表达式有分组，那么取出的规则如下：
   - groups[0]   表示匹配到的子字符串
   - groups[1] 表示匹配到的子字符串的第一组字串
   - groups[2]  表示匹配到的子字符串的第二组字串
   - .......
   - 但是分组不能越界





## 正则表达式的语法：

1. 限定符
2. 选择匹配符
3. 分组组合和反向引用符
4. 特殊字符
5. 字符匹配符
6. 定位符

### 元字符（Metacharacter）-

#### 转义号  `\\`

`\\` 符号：在我们使用正则表达式去检索某些特殊字符的时候，需要用到转义符号，否则检索不到结果，甚至会报错，

在此提示：在Java的正则表达式中 ，两个`\\`表示其他语言中的一个   `\`

```java
public static void main(String[] args) {
    String content = "abc&$(ABC(!@#(";
    // 匹配（
    String regStr = "\\("; // 此处需要转义符  寻找（  本身
    Pattern compile = Pattern.compile(regStr);
    Matcher matcher = compile.matcher(content);

    while (matcher.find()){
        System.out.println(matcher.group(0));
    }
}
```

需要用到转义符号有以下：`.`  `_`  `*`  `+`  `(`   `)`  `$`  `/`  `\`   `?`  `[`   `]`  `^`  `{`   `}`



#### 字符匹配符 

符号     `[   ]`    表示可接收的字符列表  [ asjdfh ] 其中的字符都可以接收  

符号     `[ ^ ]`    表示不可接收的字符  [ ^abc]  除了abc之外的字符

符号    `-`     连字符         表示   A-Z    的任意一个大写字母

符号        `.`    匹配除了 \n  以外的任何字符

符号      `\\d`        匹配单个数字字符   ,相当于`[ 0-9]`           `\\d {3}`  三个数字

符号     `\\D`            匹配单个非数字字符，相当于`[ ^0-9]`    `\\D (\\d)*` 非数字开头，后跟多个数字

符号    `\\w`         匹配单个数字、大小写字母字符，  相当于`[0-9a-zA-Z]`

符号     `\\W`         匹配单个非数字、大小写字母字符，  相当于`[^0-9a-zA-Z]`

符号  `\\s`            匹配任何空白字符

符号  `\\S`            匹配任何非空白字符



如何不区分大小写：

```java
        String regStr = "[a-z]";//匹配a-z 之间任意一个字符
        String regStr = "[A-Z]";//匹配A-Z 之间任意一个字符
        String regStr = "abc";//匹配abc 字符串[默认区分大小写]
        String regStr = "(?i)abc";//匹配abc 字符串[不区分大小写]
```

或者：

```java
//1. 当创建Pattern 对象时，指定Pattern.CASE_INSENSITIVE, 表示匹配是不区分字母大小写.
Pattern pattern = Pattern.compile(regStr, Pattern.CASE_INSENSITIVE);
```



#### 选择匹配符  `|`

在匹配某个字符串的时候式选择性的，即：既可以匹配这个，又可以匹配哪个，这是需要用到选择匹配符 `|`

符号   `|`    ab | cd   ab或者cd



限定符  用于指定其前边的字符和组合项出现多次

(abc)*     仅包含任意个abc的字符串，等效于\W*     指定字符重复0次或者n次，    abc,abcabcabc

m+(abc)*   以至少一个m开头，后接任意个abc的字符串    指定字符重复1次到多次，最少一次 m,mabc,mabcabc

m+abc?    以至少一个m开头，后接ab或abc的字符串    mab,mabc,mmmab,mmabc

[abcd]{3}     由abcd中字母组成的任意长度为3的字符串   abc,dbc,adc 

{n,}   表示指定至少n个匹配，    [abcd]{3,}    由abcd中字母组成的任意长度不小于3的字符串

{n,m}  指定至少n个但不多于m个匹配    [abcd]{3,5}  abcd中的字母组成的任意长度不小于3，不大于5的字符串



#### 限定符

```java
String content = "a211111aaaaaahello";
//a{3},1{4},\\d{2}
//String regStr = "a{3}";// 表示匹配aaa
//String regStr = "1{4}";// 表示匹配1111
//String regStr = "\\d{2}";// 表示匹配两位的任意数字字符
//a{3,4},1{4,5},\\d{2,5}
//细节：java 匹配默认贪婪匹配，即尽可能匹配多的
//String regStr = "a{3,4}"; //表示匹配aaa 或者aaaa
//String regStr = "1{4,5}"; //表示匹配1111 或者11111
//String regStr = "\\d{2,5}"; //匹配2 位数或者3,4,5

//1+
//String regStr = "1+"; //匹配一个1 或者多个1
//String regStr = "\\d+"; //匹配一个数字或者多个数字

//1*
//String regStr = "1*"; //匹配0 个1 或者多个1
//演示?的使用, 遵守贪婪匹配
String regStr = "a1?"; //匹配a 或者a1
Pattern pattern = Pattern.compile(regStr/*, Pattern.CASE_INSENSITIVE*/);
Matcher matcher = pattern.matcher(content);
while (matcher.find()) {
	System.out.println("找到" + matcher.group(0));
}
```



#### 定位符

```java
String content = "hanshunping sphan nnhan";
//String content = "123-abc";
//以至少1 个数字开头，后接任意个小写字母的字符串
//String regStr = "^[0-9]+[a-z]*";
//以至少1 个数字开头, 必须以至少一个小写字母结束
//String regStr = "^[0-9]+\\-[a-z]+$";
//表示匹配边界的han[这里的边界是指：被匹配的字符串最后,
// 也可以是空格的子字符串的后面]
//String regStr = "han\\b";
//和\\b 的含义刚刚相反
String regStr = "han\\B";

Pattern pattern = Pattern.compile(regStr);
Matcher matcher = pattern.matcher(content);
while (matcher.find()) {
	System.out.println("找到=" + matcher.group(0));
}
```



## 分组

-  `(pattern)`   非命名捕获，捕获匹配的字符串，编号为0的第一个捕获是由整个正则表达式模式匹配的文本，其他捕获结果则根据左括号的顺序从1开始自动编号
- `(?<name>pattern)`    命名捕获，将匹配的字符串捕获到一个组名称或者编号名称中。**用于name的字符串不能包含任何标点符号，并且不能以数字开头**，可以使用单引号替代尖括号  例如`(?name)`

```java
// 1. matcher.group(0) 得到匹配到的字符串
// 2. matcher.group(1) 得到匹配到的字符串的第1 个分组内容
// 3. matcher.group(2) 得到匹配到的字符串的第2 个分组内容
```

#### 命名分组：

```java
String content = "cuilongyang  s7789 nn1189cui";
String regStr = "(?<g1>\\d\\d)(?<g2>\\d\\d)";//匹配4 个数字的字符串

Pattern pattern = Pattern.compile(regStr);
Matcher matcher = pattern.matcher(content);
while (matcher.find()) {
    System.out.println("找到=" + matcher.group(0));
    System.out.println("第1 个分组内容=" + matcher.group(1));
    System.out.println("第1 个分组内容[通过组名]=" + matcher.group("g1"));
    System.out.println("第2 个分组内容=" + matcher.group(2));
    System.out.println("第2 个分组内容[通过组名]=" + matcher.group("g2"));
}
```



#### 特别分组：

`(?:pattern)`   **匹配pattern但不捕获该匹配的子表达式**，即它是一个非捕获匹配，不存储供以后使用的匹配，对于用“or”字符`（|）` 组合模式部件的情况很有用。例如：`industr(?:y|ies)`  比  `industry|industries` 更好

`(?=pattern)`   它是一个非捕获匹配，例如 `windows(?=95|98|NT|2000)` 匹配windows 2000中的windows 不匹配windows 3.1中的windows

 `(?!pattern)`     该表达式匹配不处于匹配pattern的字符串的起始点的搜索字符串，它是一个非捕获匹配，例如例如 `windows(?!95|98|NT|2000)`    不匹配windows 2000中的windows     匹配windows 3.1中的windows



#### Pattern类

matches 方法，用于整体匹配, 在验证输入的字符串是否满足条件使用，返回一个boolean

```java
boolean matches = Pattern.matches(regStr, content);
```



#### Matcher类

```java
 String content = "hello  jack cuilongyangutom hello cuilongyang hello cuilongyang";
        String regStr = "hello";
        Pattern pattern = Pattern.compile(regStr);
        Matcher matcher = pattern.matcher(content);
        while (matcher.find()) {
            System.out.println(matcher.start());
            System.out.println(matcher.end());
            System.out.println("找到: " +
                               content.substring(matcher.start(), matcher.end()));
        }
        //整体匹配方法，常用于，去校验某个字符串是否满足某个规则
        System.out.println("整体匹配=" + matcher.matches());
        
		//完成如果content 有cuilongyang 替换成崔龙阳
        regStr = "cuilongyang";
        pattern = Pattern.compile(regStr);
        matcher = pattern.matcher(content);
        //注意：返回的字符串才是替换后的字符串原来的content 不变化
        String newContent = matcher.replaceAll("崔龙阳");
        System.out.println("newContent=" + newContent);
        System.out.println("content=" + content);
```





### 分组、捕获、反向引用

- 分组：我们可以用圆括号组成一个比较复杂的匹配模式，那么则个圆括号的部分我们可以看作是一个子表达式（一个分组）

- 捕获：把正则表达式中子表达式（分组）匹配的内容，保存到以数字编号或者显式命名的组里，方便后面引用，从左向右，以分组的左括号为标志，第一个出现的分组的组号为1，第二个为2，以此类推。组0代表的是整个正则式

- 反向引用：圆括号的内容被补货后，可以在这个括号后边被使用，从而写出一个比较实用的匹配模式，这个我们称为**反向引用**，这种引用既可以实在正则表达式内部，也可以在正则表达式外部，**内部方向引用 `\\分组号`**  **外部反向引用 `$分组号`**

  举例：**内部引用**

  - 要匹配两个连续的相同数字：`(\\d)\\1`
  - 要匹配五个连续的相同数字：`(\\d)\\1{4}`
  - 要匹配个位与千位相同，十位与百位相同的数字   `(\\d)(\\d)\\2\\1`    内部引用



#### 语句去重：

```java
String content = "我....我要....学学学学....编程java!";

// 去掉标点...
Pattern pattern = Pattern.compile("\\.");
Matcher matcher = pattern.matcher(content);
content = matcher.replaceAll("");

System.out.println(content);

// 取消重复的字
// 使用 (.)\\1+   匹配到重复的情况
pattern = Pattern.compile("(.)\\1+");// 分组状况被分组到 $1
matcher = pattern.matcher(content);
while (matcher.find()){
    System.out.println(matcher.group(0));
}
// 外部 反向引用   $1 引用分组的结果替换
content = matcher.replaceAll("$1");
System.out.println(content);
```



或者采用一条语句来引用：

```java
content = Pattern.compile("(.)\\1+").matcher(content).replaceAll("$1");
```



#### String 类中使用正则表达式

```java
String content = "2000 年5 月，JDK1.3、JDK1.4 和J2SE1.3 相继发布，几周后其" +
    "获得了Apple 公司Mac OS X 的工业标准的支持。2001 年9 月24 日，J2EE1.3 发" +
    "布。2002 年2 月26 日，J2SE1.4 发布。自此Java 的计算能力有了大幅提升";
//使用正则表达式方式，将JDK1.3 和JDK1.4 替换成JDK
content = content.replaceAll("JDK1\\.3|JDK1\\.4", "JDK");
System.out.println(content);

//要求验证一个手机号， 要求必须是以138 139 开头的
content = "13888889999";
if (content.matches("1(38|39)\\d{8}")) {
    System.out.println("验证成功");
} else {
    System.out.println("验证失败");
}


//要求按照# 或者- 或者~ 或者数字来分割
content = "hello#abc-jack12smith~北京";
String[] split = content.split("#|-|~|\\d+");
for (String s : split) {
    System.out.println(s);
}
```

