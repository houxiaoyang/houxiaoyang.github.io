---
layout: post
title:  " 注解 Annotation "
subtitle: 
cover-img: 
thumbnail-img:
share-img: 
date: 2022-01-01 22:05:26 +0800
tags: [Java, 注解]
---

#### 注解 Annotation ：

注解也被称为元数据(Metadata)，用于修饰解释包、类、方法、属性、构造器、局部变量等数据信息。

和注释一样，注解不影响程序逻辑，但注解可以被编译或者运行，相当于嵌入在代码中补充的信息。

在Java SE中，注解的使用目的比较简单，例如标记过时的功能，忽略警告等。在Java EE中注解占据了更重要的角色，例如用来配置应用程序的任何切面，代替Java EE旧版中所遗留的繁冗代码和XML配置等。

#### 注解基本介绍：

使用Annotation时，需要在其前边添加@符号，并把该Annotation当成一个修饰符使用，用于修饰它支持的程序元素。三个基本的Annotation：

- @Override ：限定某个方法，是重写父类方法，该注解只能用于该方法，不能注解类
- @Deprecated：用于表示，某个程序元素（类、方法等）已经过时
- @SuppressWarnings：抑制编译器警告

有这个注解，语法就会进行校验，如果真是重写了该方法，那么就会编译通过，如果没有构成重写，则编译错误

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.SOURCE)
public @interface Override {
}
```

以上为Override的定义：如果我们发现@interface 表示后边的Override是一个注解类

`@Target(ElementType.METHOD)` 说明只能修饰方法；`@Target`  是修饰注解的注解，称为元注解

- @Deprecated：用于表示，某个程序元素（类、方法等）已经过时 不在推荐使用，但仍然可以使用

```java
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(value={CONSTRUCTOR, FIELD, LOCAL_VARIABLE, METHOD, PACKAGE, MODULE, PARAMETER, TYPE})
public @interface Deprecated {..}
```

可以修饰方法、类、字段、包

```java
@SuppressWarnings({"all"})//抑制所有警告
```

```java
@Target({TYPE, FIELD, METHOD, PARAMETER, CONSTRUCTOR, LOCAL_VARIABLE, MODULE})
@Retention(RetentionPolicy.SOURCE)
public @interface SuppressWarnings {
```

通常我们放在main方法，抑制警告的范围就是整个main，通常我们可以放在具体的语句、方法、类

```java
@SuppressWarnings({"unchecked"})//忽略没有检查的警告
@SuppressWarnings({"rawtypes"})//忽略没有指定泛型的警告
@SuppressWarnings({"unused"})//忽略没有使用某个变量的警告错误
```

