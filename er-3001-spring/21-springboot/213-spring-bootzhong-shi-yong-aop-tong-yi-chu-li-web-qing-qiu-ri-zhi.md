# Spring Boot中使用AOP统一处理Web请求日志

> AOP为Aspect Oriented Programming的缩写，意为：面向切面编程，通过预编译方式和运行期动态代理实现程序功能的统一维护的一种技术。AOP是Spring框架中的一个重要内容，它通过对既有程序定义一个切入点，然后在其前后切入不同的执行内容，比如常见的有：打开数据库连接/关闭数据库连接、打开事务/关闭事务、记录日志等。基于AOP不会破坏原来程序逻辑，因此它可以很好的对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率。
>
> **下面主要讲两个内容，一个是如何在Spring Boot中引入Aop功能，二是如何使用Aop做切面去统一处理Web请求的日志。**

#### **引入AOP依赖**

> 在Spring Boot中引入AOP就跟引入其他模块一样，非常简单，只需要在`pom.xml`中加入如下依赖：

```java
<!--aop-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
```

> 在完成了引入AOP依赖包后，一般来说并不需要去做其他配置。也许在Spring中使用过注解配置方式的人会问是否需要在程序主类中增加`@EnableAspectJAutoProxy`来启用，实际并不需要。
>
> 可以看下面关于AOP的默认配置属性，其中`spring.aop.auto`属性默认是开启的，也就是说只要引入了AOP依赖后，默认已经增加了`@EnableAspectJAutoProxy`。
>
> ```java
> {
>   "name": "spring.aop.auto",
>   "type": "java.lang.Boolean",
>   "description": "Add @EnableAspectJAutoProxy.",
>   "defaultValue": true
> },
> {
>   "name": "spring.aop.proxy-target-class",
>   "type": "java.lang.Boolean",
>   "description": "Whether subclass-based (CGLIB) proxies are to be created (true) as opposed to standard Java interface-based proxies (false).",
>   "defaultValue": false
> },
> ```
>
> 而当我们需要使用CGLIB来实现AOP的时候，需要配置`spring.aop.proxy-target-class=true`，不然默认使用的是标准Java的实现。

#### 实现Web层的日志切面

> 实现AOP的切面主要有以下几个要素：
>
> * 使用`@Aspect`注解将一个java类定义为切面类
> * 使用`@Pointcut`定义一个切入点，可以是一个规则表达式，比如下例中某个package下的所有函数，也可以是一个注解等。
> * 根据需要在切入点不同位置的切入内容

* 使用
  `@Before`
  在切入点开始处切入内容
* 使用
  `@After`
  在切入点结尾处切入内容
* 使用
  `@AfterReturning`
  在切入点return内容之后切入内容（可以用来对处理返回值做一些加工处理）
* 使用
  `@Around`
  在切入点前后切入内容，并自己控制何时执行切入点自身的内容
* 使用
  `@AfterThrowing`
  用来处理当切入内容部分抛出异常之后的处理逻辑



