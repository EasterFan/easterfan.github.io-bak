---
title: 【日记0709】如果我有一台 mac
tags: MACOS  qd
---

> 那是做梦都要笑醒的事情

<!--more-->
## 一. 开发环境

### 0. 新机必操作
第一步，更新系统，然后。。。。  

```bash
# 安装 homebrew
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
# 添加环境变量
echo 'export PATH="/usr/local/bin:$PATH"' >> ~/.bash_profile
# 测试
brew doctor
```

### 1. oracle JDK 1.8
下载  http://www.oracle.com/technetwork/java/javase/downloads/index.html

```bash
# 查看 jdk 版本 及安装路径
/usr/libexec/java_home -V

# 设置环境变量
vim ~/.bash_profile
export JAVA_HOME=$(/usr/libexec/java_home)

# 测试
java -version
```

### 2. mysql 5.7
```bash
brew search mysql
brew install mysql@5.7
echo 'export PATH="/usr/local/opt/mysql@5.7/bin:$PATH"' >> ~/.bash_profile
brew services start mysql@5.7
mysql -uroot
```

### 3. nodejs 和 jekyll 环境
> 我的博客环境

#### 下载 nvm  
```bash
git clone https://github.com/creationix/nvm.git ~/.nvm && cd ~/.nvm && git checkout `git describe --abbrev=0 --tags`
```

#### nvm 环境变量  
```bash
# 打开文件
vim ~/.bash_profile
# 加入位置
source ~/.nvm/nvm.sh
export NVM_DIR="/Users/YOURUSERNAME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"  # This loads nvm
# 检验
source ~/.bash_profile
nvm --version
```

#### 安装 node
```bash
# 查找 node 版本
nvm ls-remote
# 安装node版本并设为默认node
nvm install 8.10.0
nvm alias default 8.10.0
# 检验
node --version
npm -v
```

#### 安装jekyll
> 原来 mac 中自带了 ruby 环境， ruby 2.0 以上版本才支持 jekyll

```bash
# 安装 jekyll （30 min）
sudo gem install jekyll
# 验证
jekyll --help
# 安装 jekyll 依赖包bundle（20 min）
sudo gem install bundler
# jekyll 环境安装完毕，进入博客仓库安装依赖
sudo bundle install
# 启动
npm start
```

## 二. 其他软件

- [安装ssr](https://github.com/shadowsocks/ShadowsocksX-NG/releases/)
- [谷歌翻译](https://chrome.google.com/webstore/category/extensions)


## 三. 常用快捷键
待定。。

## 参考资料
- [多jdk版本切换](https://www.cnblogs.com/newstar/p/5512151.html)
- [mac-dev-setup](https://github.com/nicolashery/mac-dev-setup)
- [mac-setup-gitbook版](https://sourabhbajaj.com/mac-setup/Homebrew/README.html)
