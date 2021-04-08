---
title: Java 多线程
date: 2021-01-29 14:36:24
tags:
categories: Java
---

# 程序、进程、线程

# 线程的创建和启动

## 继承Thread类形式

1. 创建一个继承于Thread类的子类
2. 重写Thread类的run()，将此线程执行的操作声明在run方法中
3. 创建Thread类子类对象
4. 调用此对象的start()：启动当前线程，且调用当前线程的run()

```java
class MyThread extends Thread{
    .....
    @Override
    public void run(){

    }
}

MyThread mt=new MyThread();
mt.start();
```

## 实现Runnable接口形式

1. 创建一个实现了Runnable接口的类
2. 实现类去实现Runnable中的抽象方法run()
3. 创建实现类的对象
4. 将此对象作为参数传到Thread类的构造器中，创建Thread类的对象
5. 通过Thread类的对象调用start()

```java
class MyThread implements Runnable{
    ....
    @Override
    public void run(){

    }
}

MyThread mt=new MyThread();
Thread td=new Thread(mt);
sd.start();
```

# 线程的属性

## id、name

通过Thread(String name)构造方法或者void setName(String name)设置名字

## 优先级

线程的优先级从1到10，默认为5，因程序实际运行的操作系统不同，优先级会被映射到操作系统中的取值

## 状态

- New：线程实例化后，还没有开始运行
- Runnable：一旦调用start方法，线程就处于可运行状态，正在运行或者准备运行
- Blocked：阻塞状态，等待其他线程释放锁
- Waiting：等待
- Timed waiting：计时等待
- Terminated：结束

使用getState()可以确定线程的当前状态

# 线程的生命周期

![image-20210129153140366](https://tva1.sinaimg.cn/large/008eGmZEgy1gn4lnmkosdj31f40kgdq8.jpg)

# 线程的同步

多个线程执行的不确定性引起执行结果的不稳定：当有多个线程同时操作同一个对象时，可能出现竞态条件（race condition），无法预期最终执行结果

线程同步可以解决线程的安全问题，操作同步代码时，只能有一个线程参与，其他线程等待，相当于是一个单线程的过程，但效率较低

## 同步代码块

```java
synchronized(obj)
{
    //需要被同步的代码块
}
```

obj称为同步监视器，即锁

当线程开始执行同步代码块前，必须先获得对同步代码块的锁定。并且任何时刻只能有一个线程可以获得对同步监视器的锁定，当同步代码块执行完成后，该线程会释放对该同步监视器的锁定

## 同步方法

如果操作共享数据的代码完整的声明在一个方法中，可以将此方法声明为同步方法，不需要再指定同步监视器同步方法的同步监视器就是this，也就是调用该方法的对象

