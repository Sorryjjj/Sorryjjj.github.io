---
title: Java 网络编程
date: 2021-03-06 15:32:32
tags:
categories: Java
---

# 网络编程

## 概念

TCP/IP  C/S

## 网络通信

- ip
- 端口号
- 通信协议

## IP

- 唯一定位一台网络上计算机
- 127.0.0.1：localhost
- IP地址分类
  - IPv4/IPv6
  - 公网（互联网）/私网（局域网）
    - ABCD类
    - 192.168.xx.xx
- 域名

## 端口

端口表示计算机上一个程序的进程

- 不同的进程有不同的端口号用来区分软件
- 被规定为0～65536：TCP、UDP各65536
- 端口分类
  - 公有端口0～1023
    - HTTP：80
    - HTTPS：443
    - FTP：21
  - 程序注册端口：1024～49151，分配用户或者程序
    - Tomcat
    - mysql
    - oracle
  - 动态、私有



## 通信协议

## TCP/UDP

## Socket

Socket是一个抽象概念，一个应用程序通过一个Socket来建立一个远程连接，而Socket内部通过TCP/IP协议把数据传输到网络

一个Socket就是由IP地址和端口号（范围是0～65535）组成，可以把Socket简单理解为IP地址加端口号

Socket连接成功地在服务器端和客户端之间建立后：

- 对服务器端来说，它的Socket是指定的IP地址和指定的端口号；
- 对客户端来说，它的Socket是它所在计算机的IP地址和一个由操作系统分配的随机端口号。

Java标准库提供了`ServerSocket`来实现对指定IP和指定端口的监听

```java
public class Server {
    public static void main(String[] args) throws IOException {
        ServerSocket ss = new ServerSocket(6666); 
        System.out.println("server is running...");
        for (;;) {
            Socket sock = ss.accept();
            System.out.println("connected from " + sock.getRemoteSocketAddress());
            Thread t = new Handler(sock);
            t.start();
        }
    }
}
```

TCP是一种基于流的协议，因此，Java标准库使用`InputStream`和`OutputStream`来封装Socket的数据流

## HTTP/HTTPS

## Tomcat

免费的开放源代码的Servlet容器，核心组件为

- Connector：负责接收和反馈外部请求的连接器
- Container：负责处理请求的容器

连接器和容器相辅相成，一起构成了基本的 web 服务 Service

![preview](https://tva1.sinaimg.cn/large/008eGmZEgy1gobrgplkv9j30ql056q2y.jpg)

