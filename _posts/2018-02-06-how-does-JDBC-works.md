---
title: JDBC 基本工作原理
tags: JavaWeb JDBC
---

> JDBC 是 **java 程序**和**数据库**之间的桥梁

<!--more-->

## 一. JDBC 是什么
> 希望用相同的方式访问不同的数据库，以实现与具体数据库无关的java操作界面。JDBC 消除了数据库的差异，JDBC 底层是 socket 通信。

1. JDBC 定一套标准**接口**及访问数据库的通用api，不同的数据库厂商根据各自数据库的特点去**实现**这些接口。

2. JDBC 只是一个接口，具体工作的是JDBC的**实现类**。在 JDBC 中这个实现类，叫做 **JDBC Driver**，这个实现类由数据库厂商实现，去数据库厂商官网下载，我们不需要自己写。

## 二. JDBC 组成
> 一个入口 DriverManager + 三个接口 Connection Statement ResultSet = JDBC

Step 1. DriverManager  装载驱动类 -- JDBC 的入口

```java
// 下载 jar 包，执行该代码，驱动类通过 static 实现在 DriverManager 中的自动注册
Class.forName("oracle.jdbc.driver.OracleDriver");
```

Step 2. Conncection 接口 -- 连接  
DatabaseMetaData

```java
// 根据传入的用户名密码建立连接 - 不同数据库连接名不同
Connection conn = DriverManager.getConnection(
"jdbc:oracle:thin:@192.168.0.26:3389","Administrator","123456"
);
```

Step 3. Statement 语句对象接口 -- 传输 SQL  
PreparedStatment  
CallableStatement

```java
// 1. 创建查询对象
Statement stmt = conn.createStatement();

// 2. 执行 Statement（三种方式）
// true 表示有结果集， false 表示无结果集， 创建失败抛异常
boolean flag = stmt.execute(sql-DDL);

// 代表二维查询结果，使用 for 遍历处理，抛出异常则失败
ResultSet rs = stmt.executeQuery(sql-DQL);

// 返回数字，表示更新行的数量，抛出异常则失败
int flag = stmt.executeUpdate(sql-DML);
```

Step 4. ResultSet 结果集接口 -- 传输结果  
ResultSetMetaData  

```java
// rs 结果集中包含了一个游标，游标默认在结果集的第一行之前
ResultSet rs = stmt.executeQuery(dql);

// ResultSet 和 while 是固定搭配
		while(rs.next()){
			int id = rs.getInt("id");
			String name = rs.getString(id);
			System.out.println(id + name + "/n");
		}
```

## 三. JDBC 工作原理
> JDBC 定义接口 -- 数据库厂商实现接口 -- 程序员调用接口（实际调用的是数据库厂商实现的接口）

![](/assets/img/blog/2018/2018-02-06-jdbcFunction.jpg)    

整个工作过程：  
1. DriverManager 加载驱动，建立连接
2. Statement 创建语句对象，将 SQL 语句发送给数据库
3. 数据库执行 SQL 语句，返回一个结果集对象
4. 客户端得到结果集对象ResultSet，从中获取数据
5. 关闭连接  

卒.
