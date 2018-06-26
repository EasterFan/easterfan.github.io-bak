---
title: ubuntu安装Genymotion模拟器
tags: Android
---

> Genymotion 是一套完整的工具,它提供了 Android 虚拟环境,被称为是“史上最快的模拟器”。它运行在Virtualbox环境中。ubuntu下安装virtualbox要解决一个依赖问题。  

<!--more-->
# 一. 安装Virtualbox
## 1. 使用apt源安装  
```bash
# 1.打开源文件
sudo gedit /etc/apt/sources.list

# 2.在打开文件末尾添加源：
deb http://download.virtualbox.org/virtualbox/debian xenial contrib

# 3.安装公钥

wget -q https://www.virtualbox.org/download/oracle_vbox_2016.asc -O- | sudo apt-key add -

# 4.安装VirtualBox 5.0：
sudo apt-get update
# 查看virtualbox版本
apt-cache madison virtualbox | grep -iv sources
sudo apt-get install virtualbox-5.1

```

## 2. 可能遇到的问题
1. 安装dkms
```bash
sudo apt-get install dkms
```

2. 添加信任
`sudo apt-get update`时，如果出现报错：  
```bash
The following signatures couldn't be verified because the public key is not available: NO_PUBKEY 40976EAF437D05B5
```
则添加信任  
`sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 40976EAF437D05B5`

3. 解决依赖问题
运行安装virtualbox命令后，会出现一个依赖无法安装  
```bash
virtualbox-5.0 depends on libvpx1 (>= 1.0.0); however:
Package libvpx1 is not installed
```
从下面这个网站下载  
```bash
wget http://ftp.us.debian.org/debian/pool/main/libv/libvpx/libvpx1_1.1.0-1_amd64.deb
sudo dpkg -i libvpx1_1.1.0-1_amd64.deb
sudo apt-get install virtualbox-5.0
# 结束
```

4. 修改配置
在 Oracle Virtual Box 软件中点击设置->系统->处理器,并勾选 PAE/NX 选项，这是跟主系统相关的安全选项。  
包依赖期间尝试Wily/Vivid，改变源以后libvpx1和libvpx2总有一个无法安装，最后选了trusty，libvpx1不能安装，后来在[这个网站](ftp.cn.debian.org)误打误撞找到了libvpx1的包，特地下载安装，包依赖问题解决。  

5. 卸载 virtual box

```bash
sudo apt remove virtualbox
sudo apt autoremove --purge virtualbox*
```
# 二. 安装Genymotion

[官网下载](https://www.genymotion.com/#!/auth/signin)需要先注册  
## 1. 安装
```bash
# 给权限
sudo chmod +x genymotion-2.10.0-linux_x64.bin
# 安装genymotion
./genymotion-2.10.0-linux_x64.bin
```
## 2. 在board中打开genymotion
![](/assets/img/blog/2017/2017-09-19-genymotion.png)  

# 三. Android Studio关联Genymotion
## 1. Android Studio安装genymotion插件  
> file--settings--plugins--install Genymotion--restart  

重启后标签栏出现红色genymotion图标
## 2. 选择genymotion安装目录
![](/assets/img/blog/2017/2017-09-19-genymotion-path.png)
## 3. 解决genymotion插件问题
genymotion单独运行正常，但从Android Studio中启动时，会报错找不到virtualbox，这是AS中genymotion插件的问题  
![](/assets/img/blog/2017/2017-09-19-genymotion-start-error.png)  
解决方法：在Genymotion的安装目录下删除**libssl.so**和**libcrypto.so**这两个文件  
卒。
