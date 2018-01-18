---
title: mysql子查询语句绕行查询
tags: mysql 数据库 子查询
---

> 类比线程, 当查询表和修改表是同一张表时,对这张表进行增删改操作, 子查询需建立临时表绕行

<!--more-->

# 一个问题
mysql 5.6, 删除用户表(myuser)中所有与用户 eee 同性别的职员  

```bash
+------+----------+--------+--------+
| name | password | gender | salary |
+------+----------+--------+--------+
| FDF  | 123456   | W      | 5000   |
| dd   | ff       | M      | 6000   |
| SS   | DD       | M      | NULL   |
| MING | 1524     | M      | NULL   |
| MING | 15524    | M      | NULL   |
| aaa  | aa       | W      | 1200   |
| bbb  | bbb      | W      | 5620   |
| ccc  | ccc      | W      | 1254   |
| ddd  | ddd      | W      | 1452   |
| eee  | eee      | M      | 5213   |
+------+----------+--------+--------+
```

这样删,报错:  

```sql
DELETE FROM myuser
WHERE gender = (
              SELECT gender 
              FROM myuser 
              WHERE name = 'eee'
              );
    
ERROR 1093 (HY000): You can't specify target table 'myuser' for update in FROM clause

```
这样删,通过:  

```sql
DELETE FROM myuser
WHERE gender = (
                SELECT gender FROM(
                      SELECT gender 
                      FROM myuser
                      WHERE name = 'eee'
                                    ) AS temp 
              );
```

# 为什么

## 其他尝试

子查询在 DDL 创建表
```sql
CREATE TABLE myuser2
AS
SELECT name,password,gender,salary
FROM myuser;
```

子查询在 DQL 查找  
```sql
SELECT name 
FROM myuser2
WHERE salary > (SELECT salary 
                FROM myuser2 
                WHERE name = 'ddd');
```

子查询语句在建表, 查表,可以直接使用,  
但是当查询表和修改表是同一张表时,   
子查询语句报错.


## 一个猜测
mysql 子查询语句在 DDL 和 DQL 语句中可以直接使用, 但在 DML 语句中需要建临时表绕行,   

可能是因为当查询表和修改表是同一张表时,对这张表进行增删改操作, 子查询需建立临时表绕行

## 也许因为

如果把 myuser 表看作对象, 父查询和子查询语句看成两个线程,  

java 通过锁定对象,来确保每个线程可以单独占有对象,  

mysql 通过新建一张临时表的方法,将子进程对对象的修改单独存到一张临时表中 , 通过存储临时表, 来确保每个线程单独占有修改对象.  

显然 mysql 建立临时表的做法, 比 Java 锁定对象的做法 ,更占用资源 , 体贴的揣测一下这样绕行设计的用意    

类比 Java 线程 , 它们为了达到一个目的:  

**保证数据的完整性**


# 参考
[子查询在 UPDATE 语句中也需要临时表绕行](http://www.jb51.net/article/102784.htm)  
[猜测可能是因为嵌套递归的原因](http://www.cnblogs.com/nick-huang/p/4412818.html)  

卒 
