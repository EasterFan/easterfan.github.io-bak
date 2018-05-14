---
title: ubuntu 16.04 + jdk1.8 + tomcat8 + maven3.3 + mysql5.5 = javaWeb
tags: JavaWeb ubuntu
---

> 春天啊, 可是感冒堵住了鼻孔, 闻不到花香, O

<!--more-->

# 零. 所有软件下载地址
先下载下来, 再一个一个装  
- [oracle jdk1.8](http://www.oracle.com/technetwork/java/javase/downloads/java-archive-javase8-2177648.html)  
- [tomcat8](https://tomcat.apache.org/download-80.cgi)  
- [maven3.3](http://maven.apache.org/)  
- [navicat for mysql](https://www.navicat.com.cn/products) --- 超级慢  

# 一. 安装 oracle jdk
如果先前安装 open jdk 也不用卸载, 保留两个版本jdk, 将 oracle设置为默认 jdk 即可.  
```bash
# 1. 解压
tar zxvf jdk-8u162-linux-x64.tar.gz
mv jdk1.8.0_162 /usr/lib/jvm

# 2. 环境变量
sudo vim ~/.bashrc

export JAVA_HOME=/usr/lib/jvm/jdk1.8.0_162
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
export PATH=${JAVA_HOME}/bin:$PATH

# 3. 配置默认jdk

sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk1.8.0_162/bin/java 300
sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk1.8.0_162/bin/javac 300
sudo update-alternatives --install /usr/bin/jar jar /usr/lib/jvm/jdk1.8.0_162/bin/jar 300
sudo update-alternatives --install /usr/bin/javah javah /usr/lib/jvm/jdk1.8.0_162/bin/javah 300
sudo update-alternatives --install /usr/bin/javap javap /usr/lib/jvm/jdk1.8.0_162/bin/javap 300

# 4. 查看默认jdk
sudo update-alternatives --config java

# 5. 验证
java -version
```
# 二. tomcat

```bash
# 1. 解压
tar zxvf apache-tomcat-8.5.31.tar.gz
sudo mv apache-tomcat-8.5.31/ /usr/local

# 否则 idea 没有权限拷贝操作
chmod -R 755 apache-tomcat-8.5.31/

# 2. 环境变量
vim /etc/profile

export CATALINA_HOME=/opt/apache-tomcat-8.5.31
export PATH=$PATH:$JAVA_HOME/bin:$CATALINA_HOME/bin

# 3. 检验

cd /usr/local/apache-tomcat-8.5.31/bin
./startup.sh
```
浏览器访问 `localhost:8080` 检验汤姆猫.

# 三. maven
[http://maven.apache.org/](http://maven.apache.org/) 下载
```bash
# 解压
tar zxvf apache-maven-3.3.9-bin.tar.gz
# 移动
sudo mv apache-maven-3.3.9/ /opt/apache-maven-3.3.9/
# 创建链接
sudo ln -s /opt/apache-maven-3.3.9/ /opt/maven

# 配置环境变量
sudo vim ~/.bashrc
export M2_HOME=/opt/maven
export M2=$M2_HOME/bin
export PATH=$M2:$PATH

# 刷新生效
source ~/.bashrc
# 测试
mvn -version
```

# 四. mysql + navicat
```bash
# 1. 安装mysql
sudo apt install mysql-server
apt install mysql-client
# 2. navicat for mysql
tar zxvf navicat120_mysql_cs_x64.tar.gz
cd navicat120_mysql_cs_x64/
./start_navicat
```
打开 navicat 后,中文乱码, 需要编辑`start_navicat` 修改第一行的编码,  
将`export LANG="en_US.UTF-8"` 改为 `export LANG="zh_CN.UTF-8"`.

# 五. 一个 Helloworld


## 1. 配置 maven
maven 配置一次即可，所有Maven项目共享。  
![](/assets/img/blog/2018/2018-05-14-maven-config1.png)

## 2. 新建 maven wepapp项目
new --> project --> maven  
web-app 的骨架有两个, `cocoon-22-archetype-webapp`和 `maven-archetype-webapp`,我们选的是后者.  
等待 maven 下载各种依赖, 参考[这里](http://easterpark.me/2018/05/07/come-to-know-maven.html)更改**maven远程仓库**(提高下载速度)和**本地仓库**(多个项目共用同一个本地仓库,避免重复下载大量依赖)  

另外, 新建项目的`WEB-INF`是 java 的 web 应用安全目录 -- 客户端无法访问, 服务端可以访问.  

## 3. 配置 Facets

![](/assets/img/blog/2018/2018-05-14-Facets1.png)  


### 关于更改默认`<web-app>根标签版本`
项目加载完成后, 自动生成的`web.xml`版本是2.3,我们需要将版本升级到 3.1 以获得更好的前端支持效果.  
在web.xml 中复制粘贴下面的根标签,  
可以在 `structure/Facets`中更改, 如[这篇博客解决方法](https://blog.csdn.net/qq_31496897/article/details/77186576),但是我删除原来`web.xml`新建3.1版后, idea 没有生成新的3.1`web.xml`, 无奈, 暂时复制解决.   
```xml
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
         http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         version="3.1" metadata-complete="true">
    <display-name>Archetype Created Web Application</display-name>

    <!--默认访问页面-->
    <welcome-file-list>
        <welcome-file>index.jsp</welcome-file>
        <welcome-file>index.html</welcome-file>
    </welcome-file-list>
</web-app>
```

## 4. 配置 Artifact
![](/assets/img/blog/2018/2018-05-14-artifact1.png)  
## 5. 配置 tomcat

- 添加 tomcat 本地服务器  
![](/assets/img/blog/2018/2018-05-14-tomcat-config1.png)  

- 添加部署方式  
![](/assets/img/blog/2018/2018-05-14-tomcat-config2.png)  
- 热部署  
![](/assets/img/blog/2018/2018-05-14-tomcat-config3.png)  

## 6. 创建 Java 目录
![](/assets/img/blog/2018/2018-05-14-add-java.png)   

最后项目的整体结构:  
![](/assets/img/blog/2018/2018-05-14-all-menu.png)


# 六. 参考
- [Project Structure 讲解](http://www.cnblogs.com/zadomn0920/p/6196962.html)
- [使用IntelliJ IDEA 15和Maven创建Java Web项目](https://blog.csdn.net/myarrow/article/details/50824793)  

卒.
