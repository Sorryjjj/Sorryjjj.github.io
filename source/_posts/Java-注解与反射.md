---
title: Java 注解与反射
date: 2021-02-26 15:06:24
tags:
categories: Java
---

# 注解

Annotation，JDK5.0引入

作用：不是程序本身，可以对程序进行解释；可以被其他程序（编译器）读取

- 供编译器检查，提早发现错误（如`@Override`)
- 改变编译器的行为,生成代码（如 **Lombok**）
- 代码运行过程中做处理。（如 `@Bean`)

## 内置注解

- @OVerride：限定重写父类方法，若想要重写父类的一个方法时，需要使用该注解告知编译器我们正在重写一个方法。
- @Deprecated：标记已过时，当我们想要让编译器知道一个方法已经被弃用(deprecate)时，应该使用这个注解
- @SuppressWarning：抑制编译器警告，该注解仅仅告知编译器，忽略它们产生了特殊警告

## 元注解

负责注解其他注解，Java定义了四个标准的meta-annotation类型

- @Target：描述注解的使用范围,有一个枚举**ElementType**来指定，

  ```java
  @Target(ElementType.METHOD)
  public @interface Dog {
      
  }
  ```

- @Retention：表述需要在什么级别保存该注释信息，用于描述注解的生命周期（SOURCE < CLASS < RUNTIME）

- @Document：表明该注解需要包含在javadoc中

- @Inherited：表明该注解可以继承父类的注解

## 自定义注解

- 注解的定义通过@interface表示，所有的注解会自动继承java.lang.Annotation接口,且不能再继承别的类或是接口。
- 注解的成员参数只能用public或默认(default) 访问权修饰来进行修饰。
- 成员参数只能使用八种基本类型（byte、short、char、int、long、float、double、boolean）和String、Enum、Class、annotations等数据类型，及其数组。
- 获取类方法和字段的注解信息，只能通过Java的反射技术来获取 Annotation 对象。
- 注解可以没有定义成员，只做标识。

```java
public @interface TestAnnotation {

}
```

# 反射

## Reflection

反射是Java被视为动态语言的关键，反射机制允许程序在运行期间借助于Reflection API取得任何类的内部信息，并能直接操作任意对象的内部属性和方法

任何一个类，在第一次使用时，就会被 JVM 加载到堆内存的方法区中。JVM 加载类成功后，就会在方法区中产生一个对应的 Class 对象（一个类只要一个 Class 对象），这个 Class 对象包含被加载类的全部结构信息。

## 反射提供的功能

在运行时

- 判断任意一个对象所属的类
- 构造任意一个类的对象
- 判断任意一个类所具有的成员变量和方法
- 获取泛型信息
- 调用任意一个对象的成员变量和方法
- 处理注解
- 生成动态代理
- .........

## 优缺点

优点：可以实现动态创建对象和编译，体现出很高的灵活性

缺点：对性能有影响，反射是解释操作，慢于直接执行操作

## Class类

Java.lang.class

## 类的加载过程

当程序主动使用某个类时，如果该类还未被加载到内存中，则系统会通过三个步骤来对该类进行初始化

1. 类的加载：将类的class文件读入内存，并为之创建一个java.lang.Class对象，由类加载器完成
2. 类的链接：将类的二进制数据合并到JRE中
3. 类的初始化：JVM对类进行初始化

## 使用

反射的基本使用包括创建对象，设置属性和调用方法。Class 对象中大多数 get 方法有 Declared 和无 Declared

- 无 Declared：只能获取到 public 修饰的，包括当前类和所有父类。
- 有 Declared：获取到当前类所有的（含有 private），但不包括其父类。

