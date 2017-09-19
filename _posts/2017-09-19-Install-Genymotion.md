---
title: 安装Genymotion模拟器
tags: Android 
---

Android Studio自带的模拟器太慢了，Genymotion 是一套完整的工具,它提供了 Android 虚拟环境,自 Android 开发以
来一直被称为是“史上最快的模拟器”。它运行在Virtualbox环境中。  

# 安装Virtualbox
使用apt源安装  
```bash
# 1.打开源文件
sudo gedit /etc/apt/sources.list
# 2.在打开文件末尾加入下面内容：
deb http://download.virtualbox.org/virtualbox/debian trusty contrib

# 3.更新apt源：

sudo apt-get update

# 4.安装dkms
sudo apt-get install dkms

# 5.安装公钥
wget http://download.virtualbox.org/virtualbox/debian/oracle_vbox.asc
sudo apt-key add oracle_vbox.asc

# 5.安装VirtualBox 5.0：
sudo apt-get update
sudo apt-get install virtualbox-5.0

```
##解决依赖问题
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
> 包依赖问题托了3天，期间尝试Wily/Vivid，改变源以后libvpx1和libvpx2总有一个无法安装，最后选了trusty，libvpx1不能安装，后来在[这个网站](ftp.cn.debian.org)误打误撞找到了libvpx1的包，特地下载安装，包依赖问题解决。
# 安装Genymotion
## 下载
[官网](http://www.genymotion.net/)需要先注册  
## 安装
```bash
# 给权限
sudo chmod +x genymotion-2.10.0-linux_x64.bin 
# 安装genymotion
./genymotion-2.10.0-linux_x64.bin 
```
## 在board中打开genymotion
![](/assets/img/blog/Android/2017-09-17-genymotion.png)
卒。

