---
title: ubuntu 16.04 + idea + maven 下的项目管理
tags: Maven 项目管理 java
---
> 我的电脑上能运行, 你的电脑上不能运行 _?? ==> 管理你所有第三方 jar 包,
更是一个自动化构建工具, 覆盖编译/测试/清理/打包和部署整个项目生命周期,
<!--more-->

# 一. maven 介绍
> **Maven 是**基于项目对象模型(POM), 可以通过一小段描述信息来管理项目的构建/报告和文档的**项目管理工具**.

## 1. 目录介绍
```bash
- bin (mvn 的运行脚本)
- boot (类加载框架, maven 使用该框架加载自己的类库)
- conf (配置文件)
- lib (maven 执行时所用到的类库 -- 自身的和第三方的)
```

## 2. maven 环境(ubuntu 16.04 + idea)
[http://maven.apache.org/](http://maven.apache.org/)下载, maven 需要JDK环境
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

## 3. 跑 hello world
### 1. hello 项目目录
```bash
-src
  -main
    -java
      -package
  -test
    -java
      -package
  -resources
  -target
  -pom.xml
```

HelloWorld.java  
```java
package com.imooc.maven01.model;

public class HelloWord {
    public static String sayHello(){
        return "hello world";
    }
}
```

HelloWorldTest.java
```java
package com.imooc.maven01.model;
import org.junit.*;

public class HelloWorldTest {
    @Test
    public void testHello(){
        Assert.assertEquals("hello world",HelloWord.sayHello());
    }
}
```

pom.xml 引入junit测试  
```xml
<dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.10</version>
        </dependency>
</dependencies>
```

### 2. 运行三个命令  
```bash
mvn compile -- 编译项目, 并在target 目录下生成编译后的.class文件
mvn test -- 跑测试, 在target目录下生成编译后的.class测试文件和测试报告
mvn package -- 打包项目
```
# 二. maven 核心知识
## 1. 常用命令
```bash
# 安装jar包到本地仓库 -- 供其它项目使用
mvn install
# 清除target
mvn clean
# 编译
mvn compile
# 打包
mvn package
# 跳过单元测试 -- 一些单元测试要用到数据库中的数据,如果数据库还没有生成这些数据,就会报错
mvn clean package -D maven.test.skip=true
```

## 2. Maven 构建包的流程
一个项目中会使用很多第三方jar包, maven 对 jar 包的管理是:  
1. 加载 pom.xml , 找到所有jar包的坐标`GroupID`
2. 根据`GroupID`先在本地仓库中寻找 jar 包
3. 如果找到了, 将此 jar 包加入到本地仓库的`classpath`中; 如果没找到, 就到中央仓库查找,然后下载到本地仓库, 供本地仓库使用
4. 加载完所有 jar 包后, `mvn compile`编译项目

## 3. archetype 自动创建 maven 目录
maven创建目录的两种方式 -- **也可以直接用 ide 创建**

1. archetype:generate 按照提示进行选择
2. archetype:generate -DgourpId=组织名，公司网址的反写+项目名称 -DartifactId=项目名-模块名 -Dversion=版本号 -Dpackage=代码所在的包名  
`mvn archetype:generate -DgroupId=com.imooc.maven04 -DartifactId=maven04-demo -Dversion=1.0.0-SNAPSHOT -Dpackage=com.imooc.maven04-demo`

## 4. maven 仓库
### 1. 更换远程中央仓库地址
maven 中央仓库地址:  
`vim {maven-install}/lib/maven-model-builder-3.5.3.jar(解压)/org/apache/maven/model/pom-4.0.0.xml`  

此处pom.xml 可以看做是所有xml文件的父类, 在`<repository>`下找到中央仓库`https://repo.maven.apache.org/maven2`,也可以直接搜索: `search.maven.org`

更换中央仓库地址:  
```bash
# 打开 maven 配置文件
vim {maven-install}/conf/settings.xml

# <mirrors> 标签下添加新 <mirror>
  <mirror>
      <id>maven.net.cn</id>
      <mirrorOf>central</mirrorOf>
      <name>central mirror in china</name>
      <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
  </mirror>
```

### 2. 更换本地仓库地址
```bash
# 默认本地仓库地址(以maven01为例):  
cd ~/.m2/repository/com/imooc/maven01/model/maven01-model/

# 打开 maven 配置文件
vim {maven-install}/conf/settings.xml

# <localRepository> 标签下添加新路径
<localRepository>/path/to/local/repo</localRepository>
```

## 5. maven 的生命周期
完整的项目构建过程包括:  
清理 -> 编译 -> 测试 -> 打包 -> 集成测试 -> 验证 -> 部署

而 maven 的三个相互独立的生命周期:

```bash
-clean --> 清理项目  
  -pre-clean --> 执行清理前的工作
  -clean  --> 清理上一次构建生成的所有文件
  -post-clean  --> 执行清理后的工作

-default --> 构建项目
  -compile
  -test
  -package
  -install

-site --> 根据pom信息, 自动生成项目站点
  -pre-site --> 生成项目站点前预准备工作
  -site   --> 生成项目的站点文档
  -post-site  --> 生成站点后的善后工作
  -site-deploy  --> 发布生成的站点到服务器上
```

## 6. pom.xml常用标签
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">

    <!--指定当前 pom 版本,固定-->
    <modelVersion>4.0.0</modelVersion>
    <!--反写的公司网址 + 项目名-->
    <groupId>com.imooc.maven01.model</groupId>
    <!--项目名 + 模块名-->
    <artifactId>maven01-model</artifactId>
    <!--snapshot 快照
    alpha 内测
    beta  公测
    release 稳定
    GA 正式发布
    -->
    <version>0.0.1-SNAPSHOT</version>


    <!--  maven 打包方式, 默认是 jar, 还可以打包成 war zip pom   -->
    <packaging></packaging>
    <!--项目描述名, 产生项目文档时使用-->
    <name></name>
    <!--项目地址-->
    <url></url>
    <!--项目描述-->
    <description></description>
    <!--开发人员列表-->
    <developers></developers>
    <!--许可证信息-->
    <licenses></licenses>
    <!--组织信息-->
    <organization></organization>

    <!--依赖项-->
    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.10</version>
            <!--依赖范围-->
            <scope>test</scope>
            <!--设置依赖是否可选,默认 false-->
            <optional>false</optional>
            <!--排除依赖传递列表-->
            <exclusions>
                <exclusion>

                </exclusion>

            </exclusions>
        </dependency>
    </dependencies>

    <!--依赖的管理, 定义在父模块,供子模块使用-->
    <dependencyManagement>
        <dependencies>
            <dependency>

            </dependency>
        </dependencies>

    </dependencyManagement>

    <!--构建行为管理-->
    <build>
        <!--插件列表-->
        <plugins>
            <plugin></plugin>
        </plugins>
    </build>

    <!--子模块中对父模块的继承-->
    <parent></parent>
    <!--聚合多个模块, 同时编译多个模块-->
    <modules></modules>
</project>
```

## 7. Maven 依赖
### 1. 依赖范围
我们引入依赖,是通过将依赖引入到 `classpath` 中, 为本地项目提供支持.  
Maven 中有三个 classpath: **编译, 测试, 运行,**  
每个依赖对项目的作用范围, 是上面三个 classpath 中的一个,    
所以, 在引入依赖时, 我们就可以指定该依赖作用的范围.  

比如 `junit` 依赖, 为项目提供测试功能, 它的作用范围是**测试阶段**, 所以我们需要将它的作用范围限定在测试阶段 --> 即把 junit 引入到测试的`classpath`中, 只需要在 pom.xml 中写入:  

```xml
<dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.10</version>
            <!--依赖范围-->
            <scope>test</scope>
        </dependency>
</dependencies>
```
maven 范围有**6**个可选参数:  
`compile` -- 默认选项, 在3种 classpath 中都有效.  
`provided` -- 编译测试(servlet -- 容器在运行时提供了 servlet, 如果项目本身额外再加载依赖,会产生冲突)  
`runtime`  -- 测试和运行时有效(jdbc驱动,已经编译好)  
`test`  -- 只在测试范围有效(junit)  
`system` -- 编译测试(与本机系统相关联,可移植性差, 比如读取本机javaweb目录)  
`import` -- 只使用在 dependencyManagement 中, 表示从其它 pom 中导入 dependency 配置  
[官网 dependencyScope 的介绍](http://maven.apache.org/guides/introduction/introduction-to-dependency-mechanism.html#Dependency_Scope)
### 2. 依赖传递
有三个 maven 项目　ABC, B　依赖　A, C 依赖 B, 那么, C 的 Maven Dependency 中就会出现 A ==> C 依赖 A.  

C 对 A 的依赖, 就是传递依赖.  

如果 C 只需要用到 B , 不想要引入 A, 可以在 B 的依赖中, 使用 `<exclusions>`标签,排除 A.

C 的 pom.xml  
```xml
<dependencies>
        <dependency>
            <groupId>com.easter</groupId>
            <artifactId>B</artifactId>
            <version>0.1</version>
            <!--排除依赖传递列表-->
            <exclusions>
                <exclusion>
                  <groupId>com.easter</groupId>
                  <artifactId>A</artifactId>
                  <version>0.1</version>
                </exclusion>
            </exclusions>
        </dependency>
    </dependencies>
```
### 3. 依赖冲突
依赖冲突, 是指 A 和 B 依赖了同一构件的不同版本.  

冲突解决:  
- 短路优先  
若两条依赖:  
A --> B --> C --> X(jar)  
A --> D --> X(jar)  
此时优先使用下面相对较短的依赖引用.  

- 路径长度相同, 先声明的优先  
即在 pom.xml 中从上而下解析, 先解析的优先.  

### 4. Maven 的聚合和继承
**聚合:**  
将多个 Maven 项目组合到一个项目中, 同时进行 `mvn install`,安装到本地仓库中  
组合后的父pom 文件只需要加入`<packaging>`和`<modules>`

```xml
    <groupId>com.imooc.maven03.model</groupId>
    <artifactId>maven03-model</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>pom</packaging>

    <modules>
        <module>../maven01model</module>
        <module>../maven02model</module>
    </modules>
```
**继承:**  
如果项目中多个模块频繁的使用到同一个依赖(比如: junit), 可以定义一个**父pom**文件, 将公共依赖提取出来.  
组合后的父pom 文件只需要加入`<packaging>`和`<dependencyManagement>` , 子pom 需要在`<parent>`标签中引入父pom 坐标.  

```xml
    <groupId>com.imooc.maven04.model</groupId>
    <artifactId>maven04-model</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>pom</packaging>

    <name>all-maven-parent</name>

    <properties>
        <junit.verion>3.8.1</junit.verion>
    </properties>

    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>junit</groupId>
                <artifactId>junit</artifactId>
                <version>${junit.verion}</version>
                <scope>test</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
```

# 三. 快速开始
> maven 创建 web 项目, 并发布到 jetty 中

```xml
<plugins>
      <plugin>
        <groupId>org.eclipse.jetty</groupId>
        <artifactId>jetty-maven-plugin</artifactId>
        <version>9.4.10.v20180503</version>
        <executions>
              <!--打包成功后,使用 jetty run 来运行 jetty 服务-->
              <execution>
                  <phase>package</phase>
                  <goals>
                      <goal>run</goal>
                  </goals>
              </execution>
          </executions>
      </plugin>
</plugins>
```
先通过pom.xml 安装 jetty, 然后在 maven 的 plugin 中运行 `jetty run`  

# 参考
- [项目管理利器——maven](https://www.imooc.com/learn/443)
