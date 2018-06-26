---
title: sql查询语句的执行顺序
tags: mysql 数据库
---
> FROM > WHERE > GROUP BY > 聚合函数 > HAVING ====>  聚合函数不能**直接**作为 WHERE 的过滤条件

<!--more-->

# 一个问题
在学生成绩表中,查询学生的平均分大于80的专业  

```sql
SELECT AVG(IFNULL(score,0)),major
FROM myuser
WHERE AVG(IFNULL(score,0)) > 80
GROUP BY major;
```

报错:  
```bash
ERROR 1111 (HY000): Invalid use of group function
# 无效的分组方法
```

换个语句:  
```sql
SELECT AVG(IFNULL(score,0)),major
FROM myuser
GROUP BY major
HAVING AVG(IFNULL(score,0)) > 80;
```

```bash
+-----------------------+--------+
| AVG(IFNULL(score,0))  | major  |
+-----------------------+--------+
|                88     | MIS    |
+-----------------------+--------+
```

# 为什么
`无效的分组方法`,是在 WHERE 语句报错,有一种说法, WHERE 字句不能以聚合函数作为过滤条件,因为过滤的顺序不对  

## 1. WHERE执行顺序
WHERE 在检索数据表时,只过滤一遍,然后筛选符合过滤条件的记录, 放到结果集中   
WHERE 只检索一遍, 第一遍检索的时候,并不知道最大值是多少.

## 2. 聚合函数执行顺序

使用聚合函数的结果作为过滤条件,   

那么一定是数据从表中查询完毕(WHERE 在查询过程中发挥作用),得到结果集, 并且分组完毕, 才进行聚合函数统计结果.  

由此可见,聚合函数的过滤时机是在 WHERE 之后进行的.


## 3. HAVING 的执行顺序
HAVING 是为了解决这一矛盾的,它会在聚合函数发挥作用后,再进行分组过滤

## 4. WHERE 和 HAVING 的取舍
有时候, WHERE 和 HAVING 的过滤效果是一样的, 当过滤条件是聚合函数时 , 只能用 HAVING, 其他情况下, 能用 WHERE 过滤掉的, 优先用 WHERE  

为什么? 因为 WHERE 执行结束的时候, 结果集已经出来了, 然后GROUP BY分组, 聚合函数统计 , HAVING 过滤, 我们尽量在分组前将不需要的数据过滤掉, 尽早减少多余数据, 让查询速度更快  


## 因为
WHERE 语句在查询过程中发挥作用,   
聚合函数在查询结束,且分组结束之后发挥作用 ,   

把聚合函数作为 WHERE 的过滤条件, 矛盾在于:  
WHERE 执行的时候, 聚合函数还没有执行, 这个过滤条件是一个不存在的值,   

所以报错:无效的分组方法


# 整个查询执行顺序
> FROM > WHERE > GROUP BY > 聚合函数 > HAVING

## 1.一个特例
由 sql 语句的执行顺序, 我们可以说 :   
**聚合函数不能作为 WHERE 的过滤条件**,   
但是,    
今天突然发现一种**子查询**写法, 意外地将聚合函数变成了 WHERE 的过滤条件...  
更新一下  


**查询薪水高于平均水平的员工的姓名**  
```bash
+------+--------+--------+
| name | gender | salary |
+------+--------+--------+
| SS   | M      | 4000   |
| MING | M      | 4231   |
| eee  | M      | 5213   |
| dd   | M      | 6000   |
| FDF  | W      | 5000   |
| bbb  | W      | 5620   |
+------+--------+--------+

```

此时,把聚合函数放在子查询里, 用子查询结果当作 WHERE 的过滤条件,不报错  

猜测一下,   

可能是子查询语句比主查询语句先执行了...

```sql
SELECT name,gender,salary
FROM myuser
WHERE salary > (SELECT AVG(salary) FROM myuser)
ORDER BY gender,salary
```


## 2.查询中还能做的一些优化:  
![](/assets/img/blog/2018/2018-01-16-cleansql.jpeg)


# 参考
[完整的11步查询](https://yq.aliyun.com/articles/39391)  

卒
