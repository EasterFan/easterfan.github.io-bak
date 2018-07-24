---
title: circleci 初体验
tags: ciecleci qd
---

![2018-07-24-circleciprocess](/assets/img/blog/2018/2018-07-24-circleciprocess.png)

<!--more-->
## 一. 是什么
circleci 是一个持续集成和部署工具，我们使用它，是希望每一次改变代码后，circleci在远程自动帮助我们编译测试，然后打包部署到服务器上，部署成功，说明这次代码改动是成功的，可迭代到代码库，部署失败，说明这次代码改动有bug，要检查ci的报错信息，确定是编译/测试/部署哪一具体环节出错，然后修复提交。  

如果 ci 环境和本地环境一致，那么本地测试通过，ci 上也一定通过，  
大大减少了“程序在我机器上能运行，部署环境不能运行”的问题，或者说，一旦出现这种情况，ci 会立刻报错，告诉你是哪次提交引起bug，帮助迅速排错，ci 也保证了持续有效的交付。

## 二. 准备工作
### 1. 账号关联
打开 [https://circleci.com/](https://circleci.com/),用  github 授权登陆。  

### 2. circlecli 本地检查工具
> 一个 yml 配置文件命令行检查工具，依赖 docker

circleci 本地检查工具依赖docker环境，需要先安装docker，mac下安装非常简单，从[docker官网这里](https://docs.docker.com/docker-for-mac/install/)下载`.dmg`文件，拖动到`Application`安装即可.  

docker 安装完成后，可以按照[circleci官方安装方法 ](https://circleci.com/docs/2.0/local-cli/#installing-the-circleci-local-cli-on-macos-and-linux-distros)安装，也可以通过`homebrew`神器下载。  

```bash
brew search circleci
brew install circleci  
# 验证命令
circleci config validate -c .circleci/config.yml
```

## 三. 踩坑指南
### 1. scp 远程登陆绕过指纹验证信息
ci 第一次远程登陆阿里云服务器时，阿里云要求指纹验证，将 ci 服务器的信息 加到信任名单，这个过程需要手动输入 yes 确认(如果不确认，ci 会一直卡在这里)，但我们希望所有过程都是自动执行的，需要加参数跳过这一步：  

```bash
scp -o StrictHostKeyChecking=no -i secret-key.pem username@IP
```
> 因为不熟悉 scp 命令，花了一上午时间测试 scp 上传文件，怎样快速了解命令？ -- 善用 `man 手册`

### 2. 通过杀进程关闭 java 服务，pkill 的返回值问题
jar 包上传到阿里云后，在启动阿里云的java服务之前，需要先关闭原来的 java 服务，直接用pkill命令，在服务器上可以关闭，但一推到ci上就失败，后来排错，发现是因为pkill命令单独执行，成功后返回值是0，但是在命令子句中执行，无论成功失败，返回值都是 255，而在 ci 上，返回值为0，才表示命令执行成功。  

解决方法： 在pkill命令前加 `excu`  

不过最后也是弃用手动关闭开启服务的方法了（手动启动后，会在ci界面疯狂打 log，占用ci），改为将应用注册成服务，交给 Spring 管理  

### 3. 权限问题  

```bash
# 密钥权限不能太 open
chmod 400 xxx.pem

# jar 包权限
chmod 500 jarname.jar

# jar 包的配置文件（启动参数/日志目录等） && 设置仅root可修改
sudo chmod 400 jarname.conf && sudo chown root:root jarname.conf

# 更改 jar 包注册服务权限
chmod 500 /etc/init.d/jarname

# jar 包保护（仅对发布版本）
# 预发布版本
chattr +i jarname.jar
```  

### 4. 将应用注册成服务，文档错误问题
```bash
bootJar {
    launchScript()
}
```
Spring boot 将应用注册成服务，要求应用采用 `bootJar` 方式进行打包，  
但是在启动文件里引入上面配置文件后，应用报错，我首先想到的是文档错误，小纸值突然说可能是文档版本不对。  
查了版本号后，发现是我们的 Spring 版本是个老版本，这个启动方法是新版本的里的写法，所以报错。  

### 5. java 服务启动成功后，几秒后自动关闭 -- 添加配置文件指定启动参数  

报错显示数据库连接出错，是因为我是直接启动程序，没有指定启动参数。  
添加配置文件(在jar同目录下)  
```bash
JAVA_OPTS=-Xmx1024M
LOG_FOLDER=/custom/log/folder
```
## 参考资料
- [一个有趣的circleci系列视频 - 入门](https://www.youtube.com/watch?v=7VxBn_ZgOek)

- [circleci - config.yml 文件介绍视频 - 进阶](https://www.youtube.com/watch?v=xOSHKNUIkjY&t=1194s)

- [circleci 部署指导 - https://circleci.com/docs/2.0/deployment-integrations/](https://circleci.com/docs/2.0/deployment-integrations/)  

- [安装 Spring boot服务 - https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#deployment-install](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#deployment-install)  
