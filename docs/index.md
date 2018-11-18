# 技术开放
### 印象比较深或者比较好的地方
TODO
### 平时看什么书籍

### 今后的计划、规划

### 如何定位一个线上问题

### CPU高负载，OOM排查

---

# 数据结构
### 集合
[图解集合 6 : LinkedHashMap](http://www.importnew.com/25103.html)；  

### HashMap原理，不安全性

### ConcurrentHashMap实现原理

---

# 算法
### LRUCache算法
使用 LinkedHashMap实现LRUCache（最近最少使用）：[图解集合 6 : LinkedHashMap](http://www.importnew.com/25103.html)     
两个特性：
1. <K,V> : 缓存应该使用Map来快速查询键值；
2. 能够按照访问顺序进行排序；

### 基本算法
#### Base64、MD5等

### Hash一致性算法的原理和实现
将数据Hash之后，落到一个`0~2^32-1`构成的环上...

---
# Java
## Java、spring语法，技术点
### AOP的实现原理
简答：动态代理，JDK自身（基于接口实现）和Cglib（基于代理类实现）
TODO  各自优缺点


## JVM
### 内存模型，分别存储什么，线程安全与否
TODO
### 类加载机制，双亲委派模型，哪些违反了双亲委派模型，为什么？
TODO
### Happen Before的理解


## 多线程 
### 几个重要参数

### 线程间通信的多种方式，优缺点

### 锁
#### synchronize和lock接口，优缺点比较

#### ReentrantLock/读写锁等

#### 重点： 分布式锁

### long类型是否为原子操作
 不是
 
### volitile关键字的作用和原理？happen before？
可见性、一致性

---

# 数据库
### 分库分表设计，垂直拆分，水平拆分

### 业务ID生成规则，有哪些方式

### Sql调优？平时使用的注意点

### 慢启动如何优化

---
## 软技能、广度等
### Http协议的理解

### TCP的理解，三次握手，滑动窗口

--- 
## 缓存
### Guava Cache，实现原理

---

## Netty
### Netty的理解

### Netty的线程模型

---
## 微服务、分布式
### 微服务的理解，好处和弊端？

### 分布式缓存的设计？

### 热点缓存？

### 限流算法、单机限流、分布式限流

### 微服务框架的选型和差异

### SpringCloud、Dubbo、Thrift的差异，优缺点等

### zookeeper的理解
分布式协调器...

---
# 运维
## LVS、Nginx、Haproxy、Keepalived实现原理
四层负载均衡、七层负载均衡

## Linux启动流程

## 数据库Innodb死锁如何定位
```
show engine innodb status
```

## mysql主从复制延时


---
# MQ
### 如何处理MQ的重复消费
业务幂等处理...

### 客户端负载均衡算法
轮询、随机、一致性hash、故障转移、LRU等...

---
## 设计模式

---
# 笔试















