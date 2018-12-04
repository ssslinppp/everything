# Mysql

---
# msyql
博客：何登成 mysql

## 事务的隔离级别

## 事务的传播属性

## 索引（聚簇索引等）
#### 最左前缀原则

## 数据库调优参数

- query_cache_type： ON/OFF    
- query_cache_size    
- max_allowed_packet:     
- max_connections：并发过高时，可以考虑增大；        
- wait_timeout：    
- thread_concurrency：充分利用多核的特性    
- 各种buffer/cache大小等    
- 线程池配置参数    


[MySQL线程池问题个人整理](https://cloud.tencent.com/developer/article/1068832)   

线程池配置参数        
```
MariaDB [none]> show variables like 'thread_pool%';

+---------------------------+-------+
| Variable_name             | Value |
+---------------------------+-------+
| thread_pool_idle_timeout  | 60    |
| thread_pool_max_threads   | 500   |
| thread_pool_oversubscribe | 3     |
| thread_pool_size          | 32    |
| thread_pool_stall_limit   | 500   |
+---------------------------+-------+

```

查询缓存（默认开启）
```
MariaDB [(none)]> show variables like '%query_cache%';
+------------------------------+---------+
| Variable_name                | Value   |
+------------------------------+---------+
| have_query_cache             | YES     |
| query_cache_limit            | 1048576 |
| query_cache_min_res_unit     | 4096    |
| query_cache_size             | 0       | 查询缓存大小
| query_cache_strip_comments   | OFF     |
| query_cache_type             | ON      | 开启查询缓存
| query_cache_wlock_invalidate | OFF     |
+------------------------------+---------+

```


一个请求，最大包的大小
```
// 批量插入性能数据时，包太大
MariaDB [(none)]> show variables like '%pack%';  
+--------------------------+------------+
| Variable_name            | Value      |
+--------------------------+------------+
| max_allowed_packet       | 1048576    |
| slave_max_allowed_packet | 1073741824 |
+--------------------------+------------+
```

设置最大连接数
```
MariaDB [(none)]> show variables like '%max_connections%';  
+-----------------------+-------+
| Variable_name         | Value |
+-----------------------+-------+
| extra_max_connections | 1     |
| max_connections       | 10000 |
+-----------------------+-------+
2 rows in set (0.00 sec)
```


默认的8小时
```
show processlist； 
如果有大量的process处于sleep，可以考虑将wait_timeout设置小点

MariaDB [(none)]> show variables like '%wait_timeout%';  
+--------------------------+----------+
| Variable_name            | Value    |
+--------------------------+----------+
| innodb_lock_wait_timeout | 50       |
| lock_wait_timeout        | 31536000 |
| wait_timeout             | 31536000 |
+--------------------------+----------+

```


thread_concurrency
```
MariaDB [(none)]> show variables like 'thread_concurrency';
+--------------------+-------+
| Variable_name      | Value |
+--------------------+-------+
| thread_concurrency | 10    |
+--------------------+-------+

```

## 数据库慢查询

