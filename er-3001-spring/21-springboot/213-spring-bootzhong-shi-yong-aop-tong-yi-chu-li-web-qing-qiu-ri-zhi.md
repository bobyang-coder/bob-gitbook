# Spring Boot中使用AOP统一处理Web请求日志

#### AOP相关概念

> AOP为Aspect Oriented Programming的缩写，意为：面向切面编程，通过预编译方式和运行期动态代理实现程序功能的统一维护的一种技术。AOP是Spring框架中的一个重要内容，它通过对既有程序定义一个切入点，然后在其前后切入不同的执行内容，比如常见的有：打开数据库连接/关闭数据库连接、打开事务/关闭事务、记录日志等。基于AOP不会破坏原来程序逻辑，因此它可以很好的对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率。
>
> ##### **常用术语介绍**
>
> 让我们先了解一些AOP的概念和定义。这些术语并不是Spring特有。不幸的是，AOP术语并不显得直观。如果Spring使用它自己的术语，那么就更显得混淆。
>
> 1. Aspect（切面）：一种跨多对象的横向模块化关系。J2EE应用程序中的事务管理就是一个很好的横切关系的例子。切面作为顾问和拦截应用于Spring中。
> 2. Joinpoint（接合点）：程序执行过程中的点，就好象方法中的一个调用，或者一个特别的被抛出的异常。在Spring AOP中，一个接合点通常是方法调用。Spring并不明显地使用"接合点"作为术语；接合点信息可以通过MethodInvocation的参数穿过拦截器访问，相当于实现org.springframework.aop.Poinkcut接口。
> 3. Advice（通知）：被AOP框架使用的特定接合点。不同的通知类型包括："around"，"before"和"throws"通知。通知类型在下面介绍。许多AOP框架，包括Spring，通知模块就如同一个拦截器，维持一系列拦截器围绕着接合点。
> 4. Pointcut（切入点）：当一个通知将被激活的时候，会指定一些结合点。一个AOP框架必须允许开发者指定切入点：例如使用正则表达式。
> 5. Introduction（引入）：向一个通知类中添加方法或成员变量。Spring允许你向任何通知类中引入新接口。例如，你可能会使用一个引入，以使得任何实现IsModified接口的对象简单的缓存。
> 6. Target object（标签对象）：包含接合点的对象。也被通知对象或者代理对象所引用。
> 7. AOP proxy（AOP代理）：被AOP框架创建的对象，包括通知。在Spring中，一个AOP代理将会是一个JDK动态代理或者一个CGLIB代理。
> 8. Weaving（编织）：聚合切面以生成一个通知对象。这可以在编译时（比如使用AspectJ编译器）或在运行时完成。Spring，如同其它纯Java AOP框架一样，在运行时完成编织。
>
> ##### 通知类型介绍：
>
> 1. ##### **前置通知\[Before advice\]**：在连接点前面执行，前置通知不会影响连接点的执行，除非此处抛出异常。
> 2. ##### **正常返回通知\[After returning advice\]**：在连接点正常执行完成后执行，如果连接点抛出异常，则不会执行。
> 3. ##### **异常通知\[Throws advice\]**：如果一个方法抛出异常，通知将会被执行。Spring提供了健壮的类型以抛出通知，所以你可以按自己的喜好编写代码捕捉异常（和子类），而不需要通过Throwable或Exception。
> 4. ##### **返回通知\[After \(finally\) advice\]**：在连接点执行完成后执行，不管是正常执行完成，还是抛出异常，都会执行返回通知中的内容。
> 5. ##### **环绕通知\[Around advice\]**：环绕通知围绕在连接点前后，比如一个方法调用的前后。这是最强大的通知类型，能在方法调用前后自定义一些操作。环绕通知还需要负责决定是继续处理join point\(调用ProceedingJoinPoint的proceed方法\)还是中断执行。

---

> _**下面主要讲两个内容**_
>
> 1. _**如何在Spring Boot中引入Aop功能**_
> 2. _**如何使用Aop做切面去统一处理Web请求的日志。**_

---

#### **引入AOP依赖**

> 在Spring Boot中引入AOP就跟引入其他模块一样，非常简单，只需要在`pom.xml`中加入如下依赖：在完成了引入AOP依赖包后，一般来说并不需要去做其他配置。也许在Spring中使用过注解配置方式的人会问是否需要在程序主类中增加`@EnableAspectJAutoProxy`来启用，实际并不需要。
>
> ```java
> <!--aop-->
> <dependency>
>     <groupId>org.springframework.boot</groupId>
>     <artifactId>spring-boot-starter-aop</artifactId>
> </dependency>
> ```
>
> 可以看下面关于AOP的默认配置属性，其中`spring.aop.auto`属性默认是开启的，也就是说只要引入了AOP依赖后，默认已经增加了`@EnableAspectJAutoProxy`。
>
> ```asciidoc
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
>   * 使用`@Before`在切入点开始处切入内容
>   * 使用`@After`在切入点结尾处切入内容
>   * 使用`@AfterReturning`在切入点return内容之后切入内容（可以用来对处理返回值做一些加工处理）
>   * 使用`@Around`在切入点前后切入内容，并自己控制何时执行切入点自身的内容
>   * 使用`@AfterThrowing`用来处理当切入内容部分抛出异常之后的处理逻辑
>
> ```java
> package com.bob.stu.aspect;
>
> import org.aspectj.lang.JoinPoint;
> import org.aspectj.lang.annotation.*;
> import org.slf4j.Logger;
> import org.slf4j.LoggerFactory;
> import org.springframework.stereotype.Component;
> import org.springframework.web.context.request.RequestContextHolder;
> import org.springframework.web.context.request.ServletRequestAttributes;
>
> import javax.servlet.http.HttpServletRequest;
>
> /**
>  * http请求日志切面
>  *
>  * @author bob   <bobyang_coder@163.com>
>  * @since 2017/5/14
>  */
> @Aspect() //切面注解
> @Component
> public class HttpRequestAspect {
>     private static final Logger logger = LoggerFactory.getLogger(HttpRequestAspect.class);
>     ThreadLocal<Long> startTime = new ThreadLocal<>();
>
>     /**
>      * 定义切入点
>      */
>     @Pointcut("execution(public * com.bob.stu.controller.*.*(..)))")
>     public void logging() {
>     }
>
>     /**
>      * 在方法之前打印日志
>      * 日志内容：1.访问的url；2.访问的method；3.访问者ip；4.请求参数
>      */
>     @Before("logging()")
>     public void doBefore(JoinPoint joinPoint) {
>         logger.info("----------loggingBefore----------------");
>         ServletRequestAttributes requestAttributes = (ServletRequestAttributes) RequestContextHolder.getRequestAttributes();
>         HttpServletRequest request = requestAttributes.getRequest();
>         logger.info("url={}", request.getRequestURL());
>         logger.info("http_method={}", request.getMethod());
>         logger.info("visitor_ip={}", request.getRemoteAddr());
>         logger.info("class_method={}.{}", joinPoint.getSignature().getDeclaringTypeName(), joinPoint.getSignature().getName());
>         logger.info("method_args={}", joinPoint.getArgs());
>         startTime.set(System.currentTimeMillis());//设置开始时间
>     }
>
>     /**
>      * 在方法之后打印日志
>      */
>     @After("logging()")
>     public void doAfter() {
>         logger.info("----------loggingAfter----------------");
>     }
>
>     @AfterReturning(returning = "object", pointcut = "logging()")
>     public void doAfterReturning(Object object) {
>         logger.info("response={}", object.toString());
>         logger.info("spend time : {} ", System.currentTimeMillis() - startTime.get());//计算程序执行时间
>     }
> }
> ```
>
> 可以看上面的例子，通过`@Pointcut`定义的切入点为`com.bob.stu.controller` 包下的所有函数（对web层所有请求处理做切入点），然后通过`@Before`实现，对请求内容的日志记录（本文只是说明过程，可以根据需要调整内容），最后通过`@AfterReturning`记录请求返回的对象。
>
> 通过运行程序并访问：`http://localhost:8080/girl/1`，可以获得下面的日志输出
>
> ```
> 2017-05-14 22:39:25.451  INFO 16734 --- [nio-8080-exec-3] com.bob.stu.aspect.HttpRequestAspect     : ----------loggingBefore----------------
> 2017-05-14 22:39:25.452  INFO 16734 --- [nio-8080-exec-3] com.bob.stu.aspect.HttpRequestAspect     : url=http://127.0.0.1:8080/girl2/1
> 2017-05-14 22:39:25.453  INFO 16734 --- [nio-8080-exec-3] com.bob.stu.aspect.HttpRequestAspect     : http_method=GET
> 2017-05-14 22:39:25.453  INFO 16734 --- [nio-8080-exec-3] com.bob.stu.aspect.HttpRequestAspect     : visitor_ip=127.0.0.1
> 2017-05-14 22:39:25.453  INFO 16734 --- [nio-8080-exec-3] com.bob.stu.aspect.HttpRequestAspect     : class_method=com.bob.stu.controller.Girl2Controller.get
> 2017-05-14 22:39:25.453  INFO 16734 --- [nio-8080-exec-3] com.bob.stu.aspect.HttpRequestAspect     : method_args=1
> 2017-05-14 22:39:25.453  INFO 16734 --- [nio-8080-exec-3] com.bob.stu.controller.Girl2Controller   : ==========method:get========
> Hibernate: select girl0_.id as id1_0_0_, girl0_.age as age2_0_0_, girl0_.name as name3_0_0_ from girl girl0_ where girl0_.id=?
> 2017-05-14 22:39:25.467  INFO 16734 --- [nio-8080-exec-3] com.bob.stu.aspect.HttpRequestAspect     : ----------loggingAfter----------------
> 2017-05-14 22:39:25.468  INFO 16734 --- [nio-8080-exec-3] com.bob.stu.aspect.HttpRequestAspect     : response=Girl{id=1, name='bob', age=12}
> 2017-05-14 22:39:25.468  INFO 16734 --- [nio-8080-exec-3] com.bob.stu.aspect.HttpRequestAspect     : spend time : 15
> ```

#### 优化：AOP切面中的同步问题 {#优化：AOP切面中的同步问题}

> 在HttpRequestAspect切面中，分别通过doBefore和doAfterReturning两个独立函数实现了切点头部和切点返回后执行的内容，若我们想统计请求的处理时间，就需要在doBefore处记录时间，并在doAfterReturning处通过当前时间与开始处记录的时间计算得到请求处理的消耗时间。
>
> 那么我们是否可以在HttpRequestAspect切面中定义一个成员变量来给doBefore和doAfterReturning一起访问呢？是否会有同步问题呢？的确，直接在这里定义基本类型会有同步问题，所以我们可以引入ThreadLocal对象，像下面这样进行记录：
>
> ```java
> ThreadLocal<Long> startTime = new ThreadLocal<>();
> ```

#### 优化：AOP切面的优先级 {#优化：AOP切面的优先级}

> 由于通过AOP实现，程序得到了很好的解耦，但是也会带来一些问题，比如：我们可能会对Web层做多个切面，校验用户，校验头信息等等，这个时候经常会碰到切面的处理顺序问题。
>
> 所以，我们需要定义每个切面的优先级，我们需要`@Order(i)`注解来标识切面的优先级。**i的值越小，优先级越高**。假设我们还有一个切面是`CheckNameAspect`用来校验name必须为bob，我们为其设置`@Order(10)`，而上文中WebLogAspect设置为`@Order(5)`，所以HttpRequestAspect有更高的优先级，这个时候执行顺序是这样的：
>
> * 在`@Before`中优先执行`@Order(5)`的内容，再执行`@Order(10)`的内容
> * 在`@After`和`@AfterReturning`中优先执行`@Order(10)`的内容，再执行`@Order(5)`的内容
>
> 所以我们可以这样子总结：
>
> * 在切入点前的操作，按order的值由小到大执行
> * 在切入点后的操作，按order的值由大到小执行

---

创建时间：2017年05月14日22:23:58

参考链接：

1. [http://blog.sina.com.cn/s/blog\_616dbe3d0100eijy.html](http://blog.sina.com.cn/s/blog_616dbe3d0100eijy.html)
2. [http://blog.didispace.com/springbootaoplog/](http://blog.didispace.com/springbootaoplog/)



