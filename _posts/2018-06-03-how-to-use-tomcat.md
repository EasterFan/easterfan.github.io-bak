---
title: tomcat 发布项目小结
tags: JavaWeb Tomcat
---

> **直接拷贝(常用)**或**配置虚拟路径**

<!--more-->
## 一. Tomcat 介绍
Tomcat 是开源的 web 服务器, **用来处理客户端发来的请求, 并返回服务器端的资源**  

### 1. tomcat 安装

[Tomcat 的安装和配置关联 idea](http://easterpark.me/2018/05/14/build-javaweb-environment.html)  

### 2. tomcat 目录介绍
![](/assets/img/blog/2018/2018-06-03-tomcat-index.png)  
tomcat 将webapps 目录下的每个文件夹看做一个独立项目:  
```bash
http://localhost:8080 ---> webapps/ROOT
http://localhost:8080/xml/ ---> webapps/xml文件夹
```

## 二. Tomcat 使用
> 在 Tomcat 发布 JavaWeb 项目, 让其它电脑能访问本电脑的资源

### 1. 直接拷贝
将整个项目文件夹拷贝到 `webapps` 目录下, 假设项目文件夹为 `ttest`, 启动 tomcat 后, 在浏览器中访问 `localhost:8080/ttest/index.html`  

### 2. 配置虚拟路径 -- server.xml
> 使用虚拟路径, 就不用拷贝项目文件夹到 webapps 目录

**Step 1.** 启动 tomcat , 在浏览器访问: `
http://localhost:8080/docs/config/context.html` 查看文档`Defining a context`部分  

**Step 2.** 在conf/server.xml 的 host元素节点下, 添加以下内容  

```xml
<!-- docBase ：  项目的路径地址
path : 对应的虚拟路径 一定要以/打头。
-->
<Host name="localhost"  appBase="webapps"
            unpackWARs="true" autoDeploy="true">

            <Context docBase="/home/easter4/Desktop/ttest" path="/a"></Context>
 </Host>
```

**Step 3.** 打开浏览器访问 `http://localhost:8080/a/index.html `

### 3. 配置虚拟路径 -- 单独文件

**Step 1.** 在tomcat/conf/catalina/localhost/ 文件夹下新建一个xml文件(如: test.xml)

**Step 2.** 在这个文件里面写入以下内容  

```xml
<?xml version='1.0' encoding='utf-8'?>
<Context docBase="/home/easter4/Desktop/ttest"></Context>
```
**Step 3.** 在浏览器访问 `http://localhost:8080/test/index.html`

卒.
