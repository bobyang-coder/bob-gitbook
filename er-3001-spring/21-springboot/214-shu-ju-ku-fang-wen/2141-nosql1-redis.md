Spring Boot中除了对常用的关系型数据库提供了优秀的自动化支持之外，对于很多NoSQL数据库一样提供了自动化配置的支持，包括：Redis, MongoDB, Elasticsearch, Solr和Cassandra。

## 使用Redis {#使用Redis}

Redis是一个开源的使用ANSI C语言编写、支持网络、可基于内存亦可持久化的日志型、Key-Value数据库。

* [Redis官网](http://redis.io/)
* [Redis中文社区](http://www.redis.cn/)

#### 引入依赖 {#引入依赖}

Spring Boot提供的数据访问框架Spring Data Redis基于Jedis。可以通过引入`spring-boot-starter-redis`来配置依赖关系。

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-redis</artifactId>
</dependency>
```

#### 参数配置

按照惯例在`application.properties`中加入Redis服务端的相关配置，具体说明如下：

```

```

**其中spring.redis.database的配置通常使用0即可，Redis在配置的时候可以设置数据库数量，默认为16，可以理解为数据库的schema**

