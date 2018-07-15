---
title: 【日记0710】flyway安装配置
tags: flayway 数据库 qd
---

了解配置 flyway - 一个数据库版本控制利器。

<!--more-->
## 一. 介绍
Flyway 是数据库的版本控制，目的在于保证**所有环境（开发/测试/线上）数据库的一致**，  
和git很类似，git管理的是程序，Flyway管理的是数据库。

## 二. 安装
在配置文件`build.gradle`中，通过 Gradle 添加依赖：  
```bash
dependencies {
	compile('org.flywaydb:flyway-core')
}
```

## 三. 更改flyway配置文件
`applications.properties`添加配置文件  
```bash
# flyway
flyway.url=jdbc:mysql://localhost:3306/Default
# 指定 sql 路径
flyway.locations=filesystem:src/main/resources/db/migraty
# 如果数据库不为空，设置成 true
flyway.baseline-on-migrate=true
# 数据库用户名密码
flyway.user=root
flyway.password=123456
```

## 四. 踩坑点
```bash
Caused by: org.flywaydb.core.api.FlywayException: Validate failed: Detected failed migration to version 2 (insert some data)
```
由于sql语句错误等原因，导致sql执行失败后，flyway 执行失败，更正后，flyway 也不可以识别sql语句的变化。  

解决方法：  
删除数据库中`schema_version`表中对应记录（不是删除整张表），重新运行。

## 五. 一个问题
一套代码有本地/测试/线上环境，怎样在不同的环境中加载不同的配置？  
Spring Boot中有一种隔离应用配置文件的方式，让特定的配置在特定的环境中生效（profiles），  
一些普通的配置信息可以直接写在.properties里，比如数据库连接，但是一些敏感信息不能写在配置文件里，推到 codebase 中，（万一github密码泄漏，敏感信息也泄漏了）  

所以，敏感信息存放在哪？  
两种解决方法：  
1. 敏感信息写在环境变量里，然后用shell脚本读出来
2. 使用第三方存储，比如config server等，提供完整全面的密码保护服务。

## 07.08
上午主要研究flyway的用法（在gradle中引入和配置参数），参照官方文档和一个demo跑了一遍，遇到了几个问题，中途参加，不，围观了各位前辈们关于前后端交互的过程采用什么技术的讨论，全程十几个术语乱飞，看着各位前辈激烈的讨论，一脑懵逼，问吧，显然要耽误会议的整体进度，还会暴露我的愚蠢，不问吧，我又不会，纠结 :P 。  

整个会议主要讨论前后端交互过程，cookie，session等，最后只记住了**JWT + token**，用redis里的键值对存储的 key-value 代替前端传来的敏感信息，传输的是字符串格式的token，原来登陆第一次用户名和密码是明文传输，存储到redis缓存后，以后访问用的是密文传输。    

下午看了java stream这个api，用于简便的操作集合，但是没用上，今天的目标是写flyway的实现类，这星期的目标是写一个邮件登陆。  

自己会的东西交给谷歌，即用即搜，将注意力集中在未知的部分。  

## 参考
- [官网](https://flywaydb.org/)
- [一个flyway demo](https://github.com/callicoder/spring-boot-flyway-example)
