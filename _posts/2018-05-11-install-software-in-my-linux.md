---
title: Linux 下软件安装管理
tags: Linux
---

> 然后, 我就用五六种方法装一个软件, 可把我牛B坏了

<!--more-->
## 一. 软件包介绍
>软件包就两种, **源码包**和**二进制包**(rpm, yum,apt)

### 1. 源码包管理 - tar.gz

优点:  
- 开源, 可以修改源代码
- 可以自由选择所需功能
- 软件是编译安装, 更加适合自己的系统, 更加稳定
- 卸载方便(只要卸载安装目录即可)

缺点:  
- 安装过程步骤较多, 容易出现拼写错误 -- 尤其是大软件集合时(LAMP)
- **编译**时间较长, 安装比二进制安装时间长

**为了解决源码包安装的缺点, 引入了二进制包**

### 2. 二进制包(rpm)
优点:  
- 包管理系统简单, 通过几个命令就可以实现安装/升级/查询/卸载
- 安装速度比源码包安装快的多

缺点:  
- 经过编译, 不再可以看到源码
- 功能选择不如源码包灵活
- 依赖性

二进制包是编译后的源码包,安装方便, 就像 windows 下的`.exe`文件, 但是二进制包有一个非常麻烦的地方, 在于需要手动解决依赖问题(树形依赖, 环形依赖, 自依赖), 有时候为了安装一个软件, 需要提前安装十几二十个依赖, 就很....   
**为了解决二进制包安装的依赖性槽点, 引入了 yum 在线安装包(自动寻找相应依赖并安装)**  
### 3. 脚本安装包
把复杂的软件包安装过程写成了`shell脚本`,实际安装的还是源码包和二进制包.  

yum 已经可以实现软件的简易安装, 但是当需要搭建环境(比如:lnmp)时,   
这个环境里有十几二十个软件, `shell脚本`将所有软件打包到一起,只需运行start,就可以自动安装.

## 二. rpm 命令管理
### 1. rpm 命名规则
rpm 是centos 中的包, 存在于**/mnt/cdrom/Packages**中,存在于光盘中  
`httpd-2.2.15-5.el6.centos.1.i686.rpm`  
- httpd 软件包名
- 2.2.15 软件版本
- 15 软件发布的次数
- el6.centos 适合的linux 平台
- i686 适合的硬件平台

### 2. rpm 包依赖
- 树形依赖  a -> b -> c
- 环形依赖  a -> b -> c -> a
- 模块依赖  
.so 文件是一个依赖文件, 存在于 rpm 包中, 通过这个网站[http://rpmfind.net/](http://rpmfind.net/)来找.so文件对应的rpm包是哪个.  

### 3. yum 的出现拯救了rpm包依赖
> yum 和 rpm 命令装的都是 rpm 软件包, yum 只是 rpm 包的在线安装方法

如果没有yum, rpm 就要手动解决依赖问题, 很要命,  

yum在线安装虽然可以帮助我们解决依赖问题, 但是 **yum 包没有查询和校验功能**.  

### 2. 常用命令

```bash
# 安装命令
 rpm -ivh 包全名
# 升级与卸载
rpm -Uvh 包全名
rpm -e 包名
# rpm 包查询
 # 查询软件包是否安装
rpm -qa | grep httpd
 # 查询软件包详细信息
rpm -qi 包名
 # 查询包中的文件将安装在哪
rpm -ql 包名
 # 查询系统文件属于哪个软件包
rpm -qf 系统文件名
# rpm 包校验 - 判断包文件是否和官方文件相同
rpm -V 包名
# rpm 包中文件提取
rpm2cpio 包全名 | cpio -idv ./文件绝对路径
``` 
安装过的包包名存在于`/var/lib/rpm`中的数据库

## 三. yum 在线管理
### 1. yum 源文件
yum 地址:  
`vim /etc/yum.repos.d/CentOS-Base.repo`  

换源就是更改 mirrorlist 的地址.

### 2. 光盘搭建 yum 源
如果电脑无法联网, 可以挂载本地光盘, 使用本地 yum 安装  
```bash
# 建立挂载点
mkdir /mnt/cdrom
# 挂载光盘
mount /dev/cdrom /mnt/cdrom
# 使在线yum 失效
cd /etc/yum.repos.d
mv CentOS-Base.repo CentOS-Base.repo.bak
# 使本地 yum 生效 -- baseurl 改为自己光盘挂载地址 / 注释不存在的baseurl地址/enable=1 生效
cd /etc/yum.repos.d
vim CentOS-Media.repo

baseurl=file:///mnt/cdrom
enable=1

# 验证本地光盘设置是否成功 -- 出现yum名(c6-media)
yum list
```
### 3. yum 命令
**服务器使用最小化安装, 用什么软件装什么, 尽量不卸载**


```bash
# 1.查找包
yum search 包名
# 2. 安装(不需要使用包全名,包名即可)
yum -y install 包名
# 3. 升级
yum -y update 包名
# 4. 卸载
yum -y remove 包名
```
## 四. 源码包安装
rpm 包安装位置由rpm 包作者指定, 源码包安装位置由使用者指定.  
源代码保存位置:  **/usr/local/src**  
软件安装位置:  **/usr/local**  
**源码包一定要指定安装位置**

所有 rpm 源码包都是用 C 语言写的, 所以必须安装 C 语言编译器: gcc

### 1. 软件配置与检查
- 定义需要的功能选项(把需要的功能写入 Makefile 文件,用于后续编辑) 
- 检查当前环境 

```bash
# 指定安装位置
./configure --prefix=/usr/local/apache2
```

### 2. make 编译
把C语言编译成机器语言, 耗时
```bash
make
```
如果编译报错失败, 使用 `make clean` 清除所有已编译的源码,重新开始
### 3. 编译安装
```bash
make install
```
源码包下有**INSTALL**文件, 源码包的作者写的安装说明,软件启动方法

## 五. 脚本安装包
[lnmp 一键安装脚本](https://lnmp.org/)

## 参考
- [Linux 软件安装管理](https://www.imooc.com/learn/447)  

卒.


