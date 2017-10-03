# 这个博客

功能
- [订阅/邮箱/二维码](README.md#rss)
- [云标签](README.md#)
- [系列文章](README.md#)
- [首页文章缩略图](README.md#)
- [评论disqus](README.md#disqus)
- [](README.md#)
- [](README.md#)
- [](README.md#)


样式
- [地球访问量](README.md#earth)
- [人体时钟](README.md#)
- [EasterPark Banner](README.md#)
- [头像翻页](README.md#)
- [](README.md#)
- [](README.md#)
- [](README.md#)

<h2 id="rss">订阅/邮箱/二维码</h2>


<h2 id="disqus">评论disqus</h2>
- [disqus官网](https://disqus.com/)注册一个账号，并设置短域名为easterpark

- 修改_config.xml
```bash
disqus: 'easterpark'

```
- 修改`blog/_includes/comments.html`
```bash
dsq.src = '//easterpark.disqus.com/embed.js';
```

<h2 id="earth">地球访问量</h2>
一句话样式，见[revolvermaps官网](https://www.revolvermaps.com/)  
然后将复制的js脚本放到index.html的`<section>`标签内。
