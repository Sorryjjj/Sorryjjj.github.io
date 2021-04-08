---
title: Java
date: 2021-01-10 20:09:24
tags:
categories: Java
---

# Hello world

Java程序的开发过程

- 利用 JDK （调用 Java API）编写出 Java 源代码，存储于 `.java` 文件中
- JDK 中的编译器 javac 将 Java 源代码编译成 Java 字节码，存储于 `.class` 文件中
- JRE 加载、验证、执行 Java 字节码
- JVM 将字节码解析为机器码并映射到 CPU 指令集或 OS 的系统调用。

```java
/*
Hello.java // 新建.java文件
javac Hello.java // 编译生成Hello.class
java Hello // 运行
*/
class Hello {
	public static void main(String[] args) {
		System.out.println("Hello,World");		
	 } 
}
```

# 文档注释

可以被javadoc解析，以网页形式生成说明文档

```java
/**
  @author
  @version
*/
```

# Java API

# JDK、 JRE、JVM

## JDK

Java Development Kit， 是整个JAVA的核心，包括了 Java 运行时的环境（JRE）、解释器（Java）、编译器（javac）、Java 归档（jar ——一种软件包文件格式）、文档生成器（Javadoc）等工具。

## JRE

Java Runtime Environment，Java运行环境，包含JVM标准实现及Java核心类库。提供 Java 应用程序执行时所需的环境，由 Java 虚拟机（JVM）、核心类、支持文件组成。

## JVM

Java Virtual Machine，即java虚拟机，JVM 有针对不同系统的特定实现（Windows，Linux，macOS），目的是使用相同的字节码，它们都会给出相同的结果。Java虚拟机在执行字节码时，把字节码解释成具体平台上的机器指令执行。这就是Java的能够“一次编译，到处运行”的原因。

# 数据类型

## 基础数据类型

- 数值型：整型（byte, short,int,long）、浮点型（float, double）
- 字符型：char
- 布尔型：boolean

| 基本类型 | 位数 | 字节 | 默认值  |
| -------- | ---- | ---- | ------- |
| int      | 32   | 4    | 0       |
| short    | 16   | 2    | 0       |
| long     | 64   | 8    | 0L      |
| byte     | 8    | 1    | 0       |
| char     | 16   | 2    | 'u0000' |
| float    | 32   | 4    | 0f      |
| double   | 64   | 8    | 0d      |
| boolean  | 1    |      | false   |

对于 boolean，官方文档未明确定义，它依赖于 JVM 厂商的具体实现。逻辑上理解是占用 1 位，但是实际中会考虑计算机高效存储因素。

## 引用数据类型

- 类：class
- 接口：interface
- 数组： array

## 自动类型提升

当容量小的数据类型的变量与容量大的数据类型的变量进行运算时，结果自动提升为容量大的数据类型

（容量指表示数的范围大小）

注意，当byte、short、char三种类型进行运算时，结果为int型

Byte,char,short  -> int -> long  ->  float -> double

## 强制类型转换

需要使用强转符： ()

可能导致精度损失