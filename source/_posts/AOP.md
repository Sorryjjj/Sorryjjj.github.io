---
title: AOP
date: 2021-03-20 13:49:51
tags: [AOP, Java]
categories: Java
---

# 代理模式

- 静态代理
- 动态代理

## 静态代理

角色分析：

- 抽象角色：一般会使用接口或者抽象类来解决
- 真实角色：被代理的角色
- 代理角色：代理真实角色，做一些附属操作
- 客户：访问代理对象的人

优点：

- 可以使真实角色的操作更加纯粹，不用去关注一些公共的业务
- 公共业务交给代理角色，实现业务分工
- 便于扩展，可以做到不修改目标对象的功能前提下,对目标功能扩展

缺点：

- 因为代理对象需要与目标对象实现一样的接口,所以会有很多代理类,类太多.
- 同时,一旦接口增加方法,目标对象与代理对象都要维护



代码实现：

```java
//抽象接口
public interface Rent {
	public void rent();
}

//真实角色
public class Host implements Rent {
  public void rent() {
    
  }
}

//代理角色
public class Proxy implements Rent {
  Host host;
  public Proxy(Host host) {
    this.host = host
  }
  public void rent() {
    work();
    host.rent();
  }
  
  //其他业务
  public void work(){
    
  }
}

```

# 动态代理

本质为使用反射机制

- 代理对象,不需要实现接口
- 代理对象的生成,是利用JDK的API,动态的在内存中构建代理对象(需要我们指定创建代理对象/目标对象实现的接口的类型)
- 动态代理分为两大类：
  - 基于接口：JDK动态代理
  - 基于类：cglib
  - 基于Java字节码：javasist

## Proxy

**JDK中生成代理对象的API**

代理类所在包:java.lang.reflect.Proxy

```java
static Object newProxyInstance(ClassLoader loader, Class<?>[] interfaces,InvocationHandler h )
```

```java
public class ProxyFactory{

    //维护一个目标对象
    private Object target;
    public ProxyFactory(Object target){
        this.target=target;
    }

   //给目标对象生成代理对象
    public Object getProxyInstance(){
        return Proxy.newProxyInstance(
                target.getClass().getClassLoader(),
                target.getClass().getInterfaces(),
                new InvocationHandler() {
                    @Override
                    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                        System.out.println("开始事务2");
                        //执行目标对象方法
                        Object returnValue = method.invoke(target, args);
                        System.out.println("提交事务2");
                        return returnValue;
                    }
                }
        );
    }

}
```

# AOP

面向切面编程

AOP能够将那些与业务无关，**却为业务模块所共同调用的逻辑或责任（例如事务处理、日志管理、权限控制等）封装起来**，便于**减少系统的重复代码**，**降低模块间的耦合度**，并**有利于未来的可拓展性和可维护性**。

## 使用注解

1. 选择连接点：**选择哪一个类的哪一方法用以增强功能**
2. 创建切面：Spring 中只要使用 `@Aspect` 注解一个类，那么 Spring IoC 容器就会认为这是一个切面了
3. 定义切点：注解中定义 execution 的正则表达式，Spring 通过这个正则表达式判断具体要拦截的是哪一个类的哪一个方法

```java
//1
    ....
    public void service() {
        // 仅仅只是实现了核心的业务功能
        System.out.println("签合同");
        System.out.println("收房租");
    }
    ....
```



```java
//2
package aspect;

import org.aspectj.lang.annotation.After;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.springframework.stereotype.Component;

@Component
@Aspect
class Broker {

    @Before("execution(* pojo.Landlord.service())")
    public void before(){
        System.out.println("带租客看房");
        System.out.println("谈价格");
    }

    @After("execution(* pojo.Landlord.service())")
    public void after(){
        System.out.println("交钥匙");
    }
}
```

```java
//3
//execution：代表执行方法的时候会触发
//* ：代表任意返回类型的方法
//pojo.Landlord：代表类的全限定名
//service()：被拦截的方法名称*/
execution(* pojo.Landlord.service())

```



![image-20210320171440171](https://tva1.sinaimg.cn/large/008eGmZEly1goqhm29kgrj311e0gi0vh.jpg)



## 使用 XML 配置

```xml
<!-- 装配 Bean-->
<bean name="landlord" class="pojo.Landlord"/>
<bean id="broker" class="aspect.Broker"/>

<!-- 配置AOP -->
<aop:config>
    <!-- where：在哪些地方（包.类.方法）做增加 -->
    <aop:pointcut id="landlordPoint"
                  expression="execution(* pojo.Landlord.service())"/>
    <!-- what:做什么增强 -->
    <aop:aspect id="logAspect" ref="broker">
        <!-- when:在什么时机（方法前/后/前后） -->
        <aop:around pointcut-ref="landlordPoint" method="around"/>
    </aop:aspect>
</aop:config>
```

