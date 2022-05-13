---
layout: post
title:  " 节点流、包装流、Properties类"
subtitle: 
cover-img: 
thumbnail-img:
share-img: 
date: 2022-01-18 22:40:26 +0800
tags: [Java,节点流, 包装流, Properties类]
---

1. 节点流可以从一个**特定的数据源**读写数据，如FileReader、FileWriter，只能对一个特定的
2. 处理流（也叫**包装流**）是"连接"已存在的流（节点流或处理流）之上，为程序提供更为强大的读写功能、也更加灵活，比如**BufferedReader**、**BufferedWriter**，因为其包含一个其父类的属性，所以其父类的子类都可以封装

**BufferedReader ：：可以封装一个Reader节点流，该节点流可以是任意的，但必须是Reader的子类**！

**BufferedWriter：：可以封装任意一个Writer节点流，该节点流可以是任意的，但必须是Writer的子类！**

## 节点流和处理流的区别和联系：

1. 节点流是底层流/低级流，可以直接跟数据源相接
2. 处理流包装节点流，既可以消除不同节点流的实现差异，也可以提供更方便的方法来完成输入输出。
3. 处理流对节点流进行包装，使用了修饰器设计模式，不会直接与数据源相连

### 处理流的功能主要体现在以下两个方面：

1. 性能的提高：主要以增加缓冲的方式来提高输入输出效率。
2. 操作的便捷：处理流可能提供了一系列便捷的方法来一次输入输出大批量的数据，使用更加灵活方便

```java
public static void main(String[] args) throws IOException {
    String filePath = "d:\\a.java";
    BufferedReader bufferedReader = new BufferedReader(new FileReader(filePath));
    String line;
    //可以直接读一行的内容，如果读完就返回null
    while((line = bufferedReader.readLine())!=null){
        System.out.println(line);
    }
    //我们秩序关闭bufferedReader即可，系统会自动关闭节点流
    bufferedReader.close();
}
```

#### 案例分析：使用BufferedWriter将"hello,world"，写入到文件中

```java
String filePath = "d:\\ok.txt";
// 采用追加的方式，在FileWriter 内写入true
BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(filePath,true));
bufferedWriter.write("hello,world!");
//建议插入一个换行,这个换行符和系统相关
bufferedWriter.newLine();
//关闭
bufferedWriter.close();
```

### 处理流：BufferedInputStream 和BufferedOutputStream

BufferedInputStream是字节流，在创建BufferedInputStream时，会创建一个内部缓冲区数组

### 对象流：ObjectInputStream和ObjectOutputStream

案例要求：

1. 将int num = 100这个int数据保存到文件中，不是数字100，而是 **int 100** ，并且能够从文件中直接恢复int 100
2. 将Dog dog = new Dog("小黄",3) 这个dog对象保存到文件中，并且能够从文件中恢复
3. 上述要求，就是能够将基本数据类型或者对象进行**序列化**和**反序列化**操作

#### 序列化和反序列化：

1. **序列化就时在保存数据的时，保存数据的值和数据类型** 保存数据，OutputStream

2. 反序列化就是在恢复数据时，恢复数据的值和数据类型，恢复数据，InputStream

3. 需要让某个对象支持序列化机制，则必须让其类是可序列化的，为了让某个类是可序列化的，该类必须实现如下两个接口：

    `serializable`  // 这是一个标记接口  ； `Externalizable` //该接口由方法需要实现，一般不使用这个

**ObjectInputStream和ObjectOutputStream 两者属于处理流**

ObjectInputStream：提供反序列化功能，

ObjectOutputStream：提供序列化功能

案例分析：以下提供序列化：write

```java
String filePath = "e:\\test.txt";
ObjectOutputStream objectOutputStream = new ObjectOutputStream(new FileOutputStream(filePath));
objectOutputStream.writeInt(100);//int -->Integer 
objectOutputStream.writeBoolean(true);//Boolean 均实现了Serializable接口
objectOutputStream.writeChar('a');//该包装类实现了 Serializable 接口
objectOutputStream.writeUTF("hello,world");
objectOutputStream.writeObject(new Dog("旺财",12));
objectOutputStream.close();

class Dog implements Serializable{
    private String name;
    private int age;

    public Dog(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

以下提供反序列化：read

```java
String filePath = "d:\\data.dat";
ObjectInputStream ois = new ObjectInputStream(new FileInputStream(filePath));
System.out.println(ois.readInt());
System.out.println(ois.readBoolean());
System.out.println(ois.readChar());
System.out.println(ois.readDouble());
System.out.println(ois.readUTF());
System.out.println(ois.readObject());//此处需要向下转型
```

注意：如果我们需要调用Dog类的方法，需要向下转型，将Dog类的定义，拷贝到可以引用的位置

#### 注意细节说明：

1. 读写顺序要一致
2. 要求实现序列化或反序列化对象，需要实现Serializable接口
3. 序列化的类中建议添加**SerialVersionUID**，（序列化版本号）为了提高版本的兼容性
4. 序列化对象时，默认将里面的所有属性都进行序列化，但除了**static**和**transient**修饰的成员
5. 序列化对象时，要求里面属性的类型也需要实现序列化接口
6. 序列化具备可继承性，也就是如果某类已经实现了可序列化，则它的所有子类也默认实现了序列化

#### 标准输入输出流：

- System.in 标准输入    InputStream    默认设备：键盘

  编译类型：InputStream    运行类型：BufferedInputStream

- System.out  标准输出    PrintStream  默认设备：显示器

  编译类型为:PrintStream   运行类型为： PrintStream 

#### 转换流：InputStreamReader和OutputStreamWriter:

InputStreamReader 可以传入一个InputStream对象，而且指定编码方式

1. InputStreamReader：Reader的子类，可以将InputStream字节流包装成（转换）Reader(字符流)

2. OutputStreamWriter：Writer的子类，可以将OutputStream字节流包装成（转换）Writer(字符流)

3. 当处理纯文本数据时，如果使用字符交流效率更高，并且可以有效解决中文问题，建议将字节流转换成字符流

4. 可以在使用时指定编码格式（UTF-8，GBK，GB2312等等）

   ```java
   String filePath = "d:\\a.txt";
   // 把FileInputStream 转换成 InputStreamReader，指定编码gbk
   InputStreamReader isr = new InputStreamReader(new FileInputStream(filePath), "gbk");
   // 把InputStreamReader传入BufferedReader进行处理
   BufferedReader bufferedReader = new BufferedReader(isr);
   // 读取
   String str = bufferedReader.readLine();
   System.out.println(str);
   //关闭外层流
   bufferedReader.close();
   ```

**打印流只有输出流，没有输入流，字节流PrintStream，字符流PrintWriter**

```java
PrintStream out = System.out;
// 在默认情况下，输出位置是显示器
out.print("belle,hello");
// print底层调用的是write，我们可以调用write进行打印
out.write("hello".getBytes());
out.close();
```

```java
// 可以修改打印流输出的位置
System.setOut(new PrintStream("d:\\f1.txt"));
System.out.println("hello,cuilongyang");
```

## 配置文件：Properties类

Properties属于Hashtable的子类，专门用于读写配置文件的集合类

配置文件格式：键=值;   键=值;

注意：键值对不需要由空格，值不需要用引号引起来，默认类型为String 

### 基本介绍：Properties类常见方法

- load : 加载配置文件的键值对到Properties对象
- list：将数据显示到指定设备/流对
- **getProperty(key)**：根据键获取值
- **setProperty(key,value)**：设置键值对到Properties对象
- store：将Properties中的键值对存储到配置文件，在idea中，保存信息到配置文件，如果含有中文，会存储为Unicode码

#### 获取配置文件，读取配置文件：

```java
// 创建Properties对象
Properties properties = new Properties();
// 加载指定配置文件
properties.load(new FileReader("src\\mysql.properties"));
// 获取用户名
String user = properties.getProperty("user");
// 显示k-v到控制台
properties.list(System.out);
```

#### 使用Properties类来创建 配置文件，修改配置文件内容：

```java
// 创建Properties对象
Properties properties = new Properties();
// 创建，如果该文件没有key就是创建，有的话就相当于替换修改
properties.setProperty("charset","utf-8");
properties.setProperty("user","belle");
properties.setProperty("pwd","123456");

// 将k-v存储文件中即可
properties.store(new FileOutputStream("src\\mysql.properties"),null);
System.out.println("保存配置文件成功");
```

上述的**null**代表的是注释。

