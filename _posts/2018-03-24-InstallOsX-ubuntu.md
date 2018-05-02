---
title: ubuntu 16.04 + vmware 14.0 + osx10.12 sierra 搭建 OSX 环境
tags: ubuntu OSX
---

> ubuntu的另一种打开方式 ：D 


<!--more-->
卖了一个肾以后，是这个样子：  
![](/assets/img/blog/2018/2018-03-24-osxInUbuntu.png)  

## 1. 工具  

你需要准备至少10G的硬盘空间，以及，一个8G以上的内存，否则.......  
- [mac os x 10.12 sierra(5.7G)](https://drive.google.com/drive/folders/0B2BGAPbTu7vhM1M1THdhX01Jejg)
- [vmware tools](https://drive.google.com/file/d/0B2BGAPbTu7vhT3UxbER4UjNMZXc/view)
- [注册一个苹果开发者账号](https://idmsa.apple.com/IDMSWebAuth/login?appIdKey=891bd3417a7776362562d2197f89480a8547b108fd934911bcbea0110d07f757&path=%2Faccount%2F&rv=1)

## 2. 安装 VMware workstation 14.0
vmware workstation 12 在 ubuntu 16.04 下有一个内核错误，修复很麻烦，14 版中解决了这个冲突。

### 卸载旧版本
如果以前安装过其它版本的的VMware Workstation 或 Player  

```bash
 // Uninstall vmware workstation: 
sudo  vmware-installer -u vmware-workstation

 // uninstall vmware player: 
sudo  vmware-installer -u vmware-player
```

### 安装基本开发工具
```bash
sudo apt install build-essential
```

### 下载 VMware workstation 14.0 并安装

下载地址： [https://www.vmware.com/products/workstation-pro/workstation-pro-evaluation.html](https://www.vmware.com/products/workstation-pro/workstation-pro-evaluation.html)  

```bash
chmod +x VMware-Workstation-Full-14.1.1-7528167.x86_64.bundle
sudo ./VMware-Workstation-Full-14.1.1-7528167.x86_64.bundle
```
然后下一步下一步安装即可

## 3. 提升 Vmware 安装 osx 的权限
下载最新版 unlocker  

```bash
https://github.com/DrDonk/unlocker.git
cd ~/Downloads/unlocker
sudo ./lnx-install.sh 
```
执行后当前目录下出现 `backup` 和 `tools` root 权限文件夹  

## 4. 开始安装
### 1. 新建 osx 虚拟机
File ---> new Virtual Machine ---> Typical(典型安装) ---> I will install the operating system later ---> Apple OSX(版本OSX 10.11 图片1)  
 ---> Location（选择虚拟机安装路径）---> Disk Size（图片2）--->  Customize Hardware ---> finish  

![](/assets/img/blog/2018/2018-03-24-os1.png)  
![](/assets/img/blog/2018/2018-03-24-osx2.png)  

### 2. 配置虚拟机

解压`mac os x 10.12 sierra(5.7G)`，  
解压后是一个 8.9 G 的虚拟机硬盘文件（.vmdk）  
```bash
unrar mac os x 10.12 sierra.rar
```  

这时候安装的虚拟机是一个空壳子，下面所要配置的，是将此 vmdk 文件加载到虚拟机中  

内存 4 G  
处理器设为 4 核  
删除原来的硬盘  
添加一块SATA 硬盘 ---> use an existing virtual disk ---> 选择vmdk路径   
弹出询问：是否将此虚拟机硬盘转为新格式？选择 Convert  
保存更改  

### 3. 安装 osx
> 开启虚拟机安装 osx

![](/assets/img/blog/2018/2018-03-24-os3.png)  
选择时区（HongKong-China）---> 键盘（Pinyin）---> Don't transfer any information now ---> continue,continue  

### 4. 安装 vmware tools
打开虚拟机设置，CD/DVD 中加载 iso 镜像， 选择 vmware Tools.iso 的路径  

搭建完成后，到[这里](https://developer.apple.com/download/more/)下载xcode==> app store 不支持 sierra 中安装 xcode
## 参考
[https://www.youtube.com/watch?v=7QAsYobLJtE](https://www.youtube.com/watch?v=7QAsYobLJtE)  

卒。
