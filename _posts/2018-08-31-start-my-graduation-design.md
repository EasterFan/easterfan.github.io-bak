---
title: 【毕设】初始化
tags: 毕设 gradle springboot
---

> 这个人终于在开学的前一天开始求生。。。。  
> mac + idea + springboot + gradle

<!--more-->


## 1. 安装 gradle
```bash
# 下载 gradle
brew install gradle
# 检验
gradle -v
# mac 下默认的 gradle home（创建项目时用到）
/usr/local/opt/gradle/libexec
```

## 2. idea 新建项目
File --> new --> project  

![2018-08-31-start-project](/assets/img/blog/2018/2018-08-31-start-project.png)

选择 gradle 构建  
![2018-08-31-start-idea2](/assets/img/blog/2018/2018-08-31-start-idea2.png)

暂时添加 JPA 和 MyBatis
![2018-08-31-start-idea3](/assets/img/blog/2018/2018-08-31-start-idea3.png)

这里如果不能自动读到 gradle 路径，用到上面的地址  
![2018-08-31-start-idea4](/assets/img/blog/2018/2018-08-31-start-idea4.png)

写一个测试类～ hello
![2018-08-31-start-idea5](/assets/img/blog/2018/2018-08-31-start-idea5.png)

在 gradle 中引入 mysql 依赖 `runtime("mysql:mysql-connector-java")`，顺便改启动端口  
![2018-08-31-start-idea6](/assets/img/blog/2018/2018-08-31-start-idea6.png)

浏览器中访问  
![2018-08-31-idea7](/assets/img/blog/2018/2018-08-31-idea7.png)

项目地址：[https://github.com/EasterFan/o2o](https://github.com/EasterFan/o2o)

## 3. 参考资料
- [gradle官网](https://docs.gradle.org/current/userguide/userguide.htm)

- [使用intelliJ创建 spring boot + gradle + mybatis站点 - 简书](https://www.jianshu.com/p/306fa0e5da6b)  

卒。
