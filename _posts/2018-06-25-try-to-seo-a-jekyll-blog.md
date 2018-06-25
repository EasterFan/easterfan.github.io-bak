---
title: 对个人静态博客的谷歌 SEO 优化
tags: SEO 课设 xwan
---
> 《网站推广与搜索引擎优化》，以前课程中期尝试用 wordpress 做了一个简单的优化， 主要只是针对网站标签 、 关键字、 404 页面、外链做一些站内优化，乱搞一气后，并没有什么效果；  
后期引入搜索引擎（Google WebMaster） + 网站分析（Google Analytics），顺便更换长尾词，从外部优化控制， 瞎搞了小半个月，终于把这个只有两位数访问量的小博客做到了谷歌关键字的首页，  
谷歌搜索关键字：温和的倔强派，前两条记录就是我：P

![2018-06-25-seo-finally-display](/assets/img/blog/2018/2018-06-25-seo-finally-display.png)
<!--more-->

## 一.网站基本信息
http://easterpark.me/  
这是一个采用 github+GFM+jekyll 方式构建的静态博客站点。采用highlight.js进行代码高亮，MathJax提供LaTex公式支持。完成部署后，可以在在线撰写博客，也可以本地离线撰写，然后同步到GitHub.  

## 二.去除弊端
这个静态博客中没有使用 Ajax / Iframe / 隐藏层等技术, 对搜索引擎较为友好

## 三.网站结构
![2018-06-25-web-structure](/assets/img/blog/2018/2018-06-25-web-structure.png)

## 四.关键字策略
### 1.向搜索引擎提交网址:
http://www.google.com/addurl/?hl=zh-CN

![2018-06-25-submit-url-to-google](/assets/img/blog/2018/2018-06-25-submit-url-to-google.png)

提交了 http://easterpark.me/ 和 http://easterpark.me/about.html 两个页面

### 2.寻找关键字 -- 谷歌AdWords关键字工具
推荐的15个关键字  
![2018-06-25-get-google-keywords](/assets/img/blog/2018/2018-06-25-get-google-keywords.png)  
不过Google Adwords 推荐的 15 个关键字，确实和我的网站内容相距甚远。。。

### 3.测试网页抓取速度
博客首页网站抓取时间为: 0.247 秒  
![2018-06-25-test-index-speed](/assets/img/blog/2018/2018-06-25-test-index-speed.png)  

### 4. Google Page Speed优化速度
https://developers.google.com/speed/pagespeed/insights/?url
三条优化建议(以后再改：)  
![2018-06-25-three-upspeed-tips](/assets/img/blog/2018/2018-06-25-three-upspeed-tips.png)

## 五.页面索引
### 1. 生成 Sitemap.xml 并提交到谷歌站长工具
![2018-06-25-generate-sitemap-and-submit-to-google](/assets/img/blog/2018/2018-06-25-generate-sitemap-and-submit-to-google.png)
## 六.外部链接关系建立

### 1. 使用Fast Directory Submitter
软件有150条记录, 但是只成功了40多条, 很多报错”Cannot reach remote host”有可能是很多收录的网站已经关闭了.  
![2018-06-25-use-tools-to-submit-batch](/assets/img/blog/2018/2018-06-25-use-tools-to-submit-batch.png)

### 2.提交网址 - submit-url
https://www.google.com/webmasters/tools/submit-url   
![2018-06-25-add-submit-url-to-google](/assets/img/blog/2018/2018-06-25-add-submit-url-to-google.png)

### 3.外链查询

雅虎站长工具外联查询服务关闭了, 用 bing站长工具 和 谷歌站长工具查询外链, 该网站没有外链  
![2018-06-25-find-outer-link](/assets/img/blog/2018/2018-06-25-find-outer-link.png)  

## 七.网站维护
### 1.Google Analytics
![2018-06-25-add-google-analytics](/assets/img/blog/2018/2018-06-25-add-google-analytics.png)

Google analytics 本来需要将跟踪代码添加到网站
首页中, 但是 jekyll 中有谷歌分析的插件, 和跟踪代码的写法有些不同

#### 1.1 在jekyll配置文件 `_config.yml` 中添加Google analytics ID
#### 1.2 在 `_includes/footer.html` 中引入jekyll 版的谷歌分析追踪代码

![2018-06-25-add-ga-to-jekyll](/assets/img/blog/2018/2018-06-25-add-ga-to-jekyll.png)

### 2.Google webmaster检测robots.txt文件
![2018-06-25-test-robots](/assets/img/blog/2018/2018-06-25-test-robots.png)  

卒。  
