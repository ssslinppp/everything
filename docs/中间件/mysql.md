# Mysql

---
# msyql
博客：何登成 mysql

## 事务的隔离级别

## 事务的传播属性

## 索引（聚簇索引等）
#### 最左前缀原则

## 数据库调优参数

查询缓存
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


## 数据库慢查询

