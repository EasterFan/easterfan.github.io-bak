---
title: ubuntu安装Android Studio
tags: Android 
---

Android studio是一款基于JBrain风格的IDE，具有强大的填词补全功能。

<!--more-->

> 首先保证安装了JDK环境  

# 下载Android Studio
Android Studio下载地址：http://www.android-studio.org/index.php/download  
【问题1】  
解压完后会自动连接谷歌，显示无法连接  
【解决1】  
```bash
vim {Install Diretory}android-studio/bin/idea.properties
```
在最后一行添加  
```bash
disable.android.first.run=true
```  
这样可以启动不连接谷歌，但SDK也没有被下载了，所以我们需要手动下载SDK

# 安装SDK
## 1.下载SDK  
SDK下载地址：http://dl.google.com/android/android-sdk_r24.1.2-linux.tgz  

## 2.解压SDK  
注意：SDK不能解压到Android Studio的安装目录中  

## 3.配置SDK MANAGER
> Configure--Project Defaults--Project Structure--选择SDK目录  


# 安装Gradle
## 1.获取Gradle下载地址  
先创建一个项目，配置完成后让他加载项目组建  

```vim
easter@easter-QTK5:~/Downloads/android-studio/bin$ ./studio.sh 
Looking in classpath from com.intellij.util.lang.UrlClassLoader@6d5380c2 for /com/sun/jna/linux-x86-64/libjnidispatch.so
Found library resource at jar:file:/home/easter/Downloads/android-studio/lib/jna.jar!/com/sun/jna/linux-x86-64/libjnidispatch.so
Trying /home/easter/.AndroidStudio2.3/system/tmp/jna1649764388883264918.tmp
Found jnidispatch at /home/easter/.AndroidStudio2.3/system/tmp/jna1649764388883264918.tmp
[  20386]   WARN - dea.updater.SdkComponentSource - File /home/easter/.android/repositories.cfg could not be loaded. 
Downloading https://services.gradle.org/distributions/gradle-3.3-all.zip
.......................................
```
由上面的信息，可知gradle的下载地址为：`https://services.gradle.org/distributions/gradle-3.3-all.zip`  
然后到浏览器中下载下来，保存到我的地址：`https://d10.baidupcs.com/file/355f61e9c5d092d49577765ab3712dc0?bkt=p3-000021b50b520d458f494310fe895db57848&xcode=0d933459df312859b865e619e53a401113384f57fb8ac25dfc7e6775e72092ed&fid=3631315857-250528-807293517770493&time=1504751850&sign=FDTAXGERLQBHSK-DCb740ccc5511e5e8fedcff06b081203-AuJpYZ1jGCEkyqaIFgoxL%2BAzaL8%3D&to=d10&size=91388147&sta_dx=91388147&sta_cs=11509&sta_ft=zip&sta_ct=0&sta_mt=0&fm2=MH,Yangquan,Netizen-anywhere,,jiangsu,cmnet&newver=1&newfm=1&secfm=1&flow_ver=3&pkey=000021b50b520d458f494310fe895db57848&sl=73793615&expires=8h&rt=pr&r=942751906&mlogid=5785280715373073361&vuk=3631315857&vbdid=2031750456&fin=gradle-3.3-all.zip&fn=gradle-3.3-all.zip&rtype=1&iv=1&dp-logid=5785280715373073361&dp-callid=0.1.1&hps=1&tsl=441&csl=441&csign=efQOH5jkH1ON6boXl7ggQb1jZXU%3D&so=0&ut=5&uter=4&serv=0&uc=3867216583&ic=4039370844&ti=91499ea5817d80289199230296f48c987bc7874590516aea&by=themis`  

## 2.更改Gradle下载地址  
在你的项目文件夹中找到`gradle-wrapper.properties`文件，把原来的下载地址提换成我的地址。重启Android Studio。  
【附：ask-android论坛上一位大拿的话】  
```
不建议针对 Android Studio 的 Gradle 进行环境变量配置以及命令行的命令编译。

Android Studio 自带了项目编译所需的 Maven 依赖，通过 Android Studio 的 Gradle 选项卡，可以进行 Gradle 的命令操作。

如果你使用命令行的实行使用 Gradle 命令，它会从网络上下载依赖。而在中国，想成功下载这些依赖正常情况下是不可能的。所以，建议不要使用命令行的方式。 
```

# 设置快捷方式
在Android Studio安装目录下新建一个`Studio.desktop`文件，用vim编辑  
```vim
[Desktop Entry]
Name=AndroidStudio
Type=Application
Icon=你的AndroidStudio解压目录的绝对路径/android-studio/bin/studio.png
Exec=sh 你的AndroidStudio解压目录的绝对路径/android-studio/bin/studio.sh

```
保存退出（复制的时候注意每一行后面都不能有空格）  
然后右键该文件>属性>权限>选择允许作为程序执行文件  
ctrl键鼠标拖到启动栏。  

# 关于软件更新
参考这位大牛http://www.cnblogs.com/jecyhw/p/4467625.html，关于FQ获得更新信息，但不FQ下载，主要是这个网站http://mirrors.neusoft.edu.cn/android/repository/，这个网址是对google的Andriod的一个镜像。所有Android更新相关的东西在上面都有。  
goagent在SDK Manager中设置代理获取更新信息。  
卒。

