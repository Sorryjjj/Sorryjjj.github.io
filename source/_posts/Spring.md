---
title: Spring
date: 2021-03-12 13:43:10
tags: Java
categories: Spring
---

# Spring

## 介绍

[Spring Framework中文文档](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/overview.html)

[https://spring.io/](https://spring.io/)

- SSH
- SSM

## 优点

- Spring是一个开源的免费框架
- 是轻量级、非入侵式的框架
- 控制反转（IOC），面向切面编程（AOP）
- 支持事物的处理，对框架整合的支持

## 组成

Spring 框架分为多个模块。应用程序可以选择所需的模块。核心容器的模块是核心，包括配置模型和依赖项注入机制。除此之外，Spring 框架为不同的应用程序体系结构提供了基础支持，包括消息传递，事务性数据和持久性以及 Web。它还包括基于 Servlet 的 Spring MVC Web 框架，以及并行的 Spring WebFlux 反应式 Web 框架。

## 拓展

- Spring Boot
  - 快速开发的脚手架
  - 基于Spring Boot可以快速开发单个微服务
  - 约定大于配置
- Spring Cloud
  - 基于Spring Boot实现

学习Spring Boot的前提是Spring以及Spring MVC

# IOC

**控制反转**（Inversion of Control），是面向对象编程中的一种设计原则，可以用来减低计算机代码之间的耦合度。其中最常见的方式叫做依赖注入（Dependency Injection），还有一种方式叫“依赖查找”（Dependency Lookup）。通过控制反转，对象在被创建的时候，由一个调控系统内所有对象的外界实体将其所依赖的对象的引用传递给它。也可以说，依赖被注入到对象中。（百度百科）

- 控制指的是：**当前对象对内部成员的控制权**。
- 反转指的是：这种控制权**不由当前对象管理**了，由其他(类,第三方容器)来管理。

借助于“第三方”实现具有依赖关系的对象之间的解耦：

![img](https://tva1.sinaimg.cn/large/008eGmZEgy1goh53apdraj30a8051jsj.jpg)



对象由Spring来创建、管理和装配，原理就是通过Java的**反射技术**来实现的，通过反射我们可以获取类的所有信息(成员变量、类名等等等)，再通过配置文件(xml)或者注解来描述类与类之间的关系

官方代码实现：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="..." class="...">
        <!-- collaborators and configuration for this bean go here -->
    </bean>

    <bean id="..." class="...">
        <!-- collaborators and configuration for this bean go here -->
    </bean>

    <!-- more bean definitions go here -->

</beans>
```

```java
// create and configure beans
ApplicationContext context = new ClassPathXmlApplicationContext("services.xml", "daos.xml");

// retrieve configured instance
PetStoreService service = context.getBean("petStore", PetStoreService.class);

// use configured instance
List<String> userList = service.getUsernameList();
```

[Spring IoC有什么好处呢？ - Mingqi的回答 - 知乎](https://www.zhihu.com/question/23277575/answer/169698662)

# Spring配置

## 别名

```xml
<alias name="user" alias="userNew" />
```

## Bean

- id：bean的唯一标识符，相当于对象名
- class：bean对象所对应的全限定名：包名 + 类型
- name：别名
- prooperty：对象属性

```xml
<bean id="user" class="com.jjj.User" name="user2" > 
 	<prooperty name="name" value="jjj"/> 
</bean>
```

## import

将多个配置文件，导入合并为一个，直接使用总的配置

```xml
<beans>
    <import resource="services.xml"/>
    <import resource="resources/messageSource.xml"/>
    <import resource="/resources/themeSource.xml"/>

    <bean id="bean1" class="..."/>
    <bean id="bean2" class="..."/>
</beans>
```

# 依赖注入

## 基于构造函数的依赖关系注入

基于构造函数的 DI 是通过容器调用具有多个参数(每个参数代表一个依赖项)的构造函数来完成的

```java
package x.y;

public class ThingOne {

    public ThingOne(ThingTwo thingTwo, ThingThree thingThree) {
        // ...
    }
}
```

```xml
<beans>
    <bean id="thingOne" class="x.y.ThingOne">
        <constructor-arg ref="thingTwo"/>
        <constructor-arg ref="thingThree"/>
    </bean>

    <bean id="thingTwo" class="x.y.ThingTwo"/>

    <bean id="thingThree" class="x.y.ThingThree"/>
</beans>
```



## 基于 Setter 的依赖项注入

```java
public class ExampleBean {

    private AnotherBean beanOne;

    private YetAnotherBean beanTwo;

    private int i;

    public void setBeanOne(AnotherBean beanOne) {
        this.beanOne = beanOne;
    }

    public void setBeanTwo(YetAnotherBean beanTwo) {
        this.beanTwo = beanTwo;
    }

    public void setIntegerProperty(int i) {
        this.i = i;
    }
}
```

```xml
<bean id="exampleBean" class="examples.ExampleBean">
    <!-- setter injection using the nested ref element -->
    <property name="beanOne">
        <ref bean="anotherExampleBean"/>
    </property>

    <!-- setter injection using the neater ref attribute -->
    <property name="beanTwo" ref="yetAnotherBean"/>
    <property name="integerProperty" value="1"/>
</bean>

<bean id="anotherExampleBean" class="examples.AnotherBean"/>
<bean id="yetAnotherBean" class="examples.YetAnotherBean"/>
```



## p命名空间注入

```xml
<bean id="user" class="com.jjj.User" p:name="user2" > 
</bean>
```

## c命名空间注入

注入构造函数参数

```xml
<bean id="user" class="com.jjj.User" c:name="user2" > 
</bean>
```

## Bean作用域

![image-20210312170103979](https://tva1.sinaimg.cn/large/008eGmZEgy1goh89gbyolj30ky0bnq5j.jpg)

```xml
<bean id="accountService" class="com.something.DefaultAccountService"/>

<!-- the following is equivalent, though redundant (singleton scope is the default) -->
<bean id="accountService" class="com.something.DefaultAccountService" scope="singleton"/>
```

# Bean自动装配

三种装配的方式：

- xml配置
- java显式配置
- 隐士的自动装配

## Spring 容器可以自动装配协作 bean 之间的关系

- 自动装配是Spring满足Bean依赖的一种方式
- Spring会在上下文中自动寻找并给bean自动装配属性

## 自动装配模式：

- byName：会自动再容器上下文中查找，和自己的对象set方法后面的值对应的beanid，需要保证所有bean的id唯一
- byType：会自动在容器上下文中查找，和自己的对象属性类型相同的bean，需要保证bean的class唯一

```xml
<bean class="com.jjj.pojo.cat"/>
<bean class="com.jjj.pojo.dog"/>
<bean id="people" class="com.jjj.pojo.People" autowire="byType"></bean>
```

## 使用注解自动装配

前提：

- 导入约束：context约束
- 配置注解：```<context:annotation-config/>```

**@Autowired**：直接在属性上使用，也可以在set方法上使用

