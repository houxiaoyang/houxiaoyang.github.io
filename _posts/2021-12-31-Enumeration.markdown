---
layout: post
title:  " 枚举 Enumeration "
subtitle: 
cover-img: 
thumbnail-img:
share-img: 
date: 2021-12-31 22:45:26 +0800
tags: [Java, 枚举]
---

#### 枚举 Enumeration：

枚举属于一个特殊的类，里边包含一组特殊的具体的对象

```java
class Season {
    private String name;
    private String desc;

    //1.将构造器私有化，防止new
    //2.去掉setXXX相关方法，防止属性被修改
    //3.在Season内部直接创建固定的对象,
    public static Season SPRING = new Season("春天", "温暖");

    private Season(String name, String desc) {
        this.name = name;
        this.desc = desc;
    }
    public String getName() {
        return name;
    }
    public String getDesc() {
        return desc;
    }
    @Override
    public String toString() {
        return " name= " + name +
                " desc= " + desc;
    }
}
```

 `public static Season SPRING = new Season("春天", "温暖");` 该对象记得需要public

#### 采用枚举方法来实现：

1.使用`enum`关键字替代`class`

```java
enum Season {   //来替换 class Season
```

2.采用`SPRING("春天", "温暖")` 来替换 `public static Season SPRING = new Season("春天", "温暖");`

3.多个常量(对象)，用, 间隔

```java
SPRING("春天", "温暖"),WINTER("冬天","寒冷");
```

4.如果采用enum实现枚举，需要将定义常量对象写在前边

```java
public class EnumExercise {
    public static void main(String[] args) {
        System.out.println(Season.SPRING);
        System.out.println(Season.WINTER);
    }
}

enum Season {
    SPRING("春天", "温暖"),WINTER("冬天","寒冷");
    private String name;
    private String desc;
    private Season(String name, String desc) {
        this.name = name;
        this.desc = desc;
    }
    public String getName() {
        return name;
    }
    public String getDesc() {
        return desc;
    }
    @Override
    public String toString() {
        return " name= " + name +
                " desc= " + desc;
    }
}
```

#### 注意事项：

1. 当我们使用enum关键字开发一个枚举类时，**`会默认继承Enum类`**，返回的就是`return name`

2. 采用`SPRING("春天", "温暖")` 来替换 `public static Season SPRING = new Season("春天", "温暖");` 时必须知道它调用的是哪个构造器。

3. 如果使用**无参构造器** 创建枚举对象，则实参列表和小括号都可以省略：

   ```java
   What();//或者 What  最后一个带分号
   ```

4. 枚举对象必须放在枚举行首。

练习：以下语法正确，默认时存在无参构造器，`BOY` 和`GIRL`都是在调用无参构造器

```java
enum Gender(){
    BOY,GIRL;
}
```



#### enum常用方法举例：

**不能继承其他类，因为已经默认继承了Enum类，但是可以实现接口：举例如下：**

```java
public class Testclasswork {
    public static void main(String[] args) {
        Music.CLASSICMUSIC.playing();
    }
}

interface IPlay {
    public void playing();
}

enum Music implements IPlay {
    CLASSICMUSIC;
    @Override
    public void playing() {
        System.out.println("播放音乐");
    }
}
```



1. toString：Enum类已经重写，返回的是当前对象名，子类可以重写该方法，用于返回对象的属性信息：

2. name:返回当前对象名（常量名），子类中不能重写：

   ```java
   //输出枚举对象的名字：
           System.out.println(summer.name());
   ```

3. ordinal:返回当前位置号，按照顺序，默认从0开始；

4. values：返回当前枚举中所有的常量

   ```
           //从反编译可以看到values方法返回一个数组，该数组含有所有枚举对象
           Season[] values = Season.values();
   ```

   增强for循环：从`values`中拿出一个值赋给前边的`season` 直到`values`中没有值，结束循环

   ```java
           Season[] values = Season.values();
           for (Season season:values) {//增强for循环
               System.out.println(season);
           }
   ```

5. valueOf：将字符串转换成枚举对象，要求字符串必须为已有的常量名，否则报错

   ```java
           //先根据你输入的WINTER就去Season中寻找枚举对象，找到就输出，找不到就报错
           Season winter = Season.valueOf("WINTER");
           System.out.println(winter);
   
   ```

6. compareTo：比较两个枚举常量，比较的就是编号`Season.AUTUMN.compareTo(Season.WINTER)`

   ```java
           //怎么比较：前者编号，减去后者的编号
           System.out.println(Season.AUTUMN.compareTo(Season.WINTER));
   ```

   

