---
layout: post
title: 给Hexo添加Valine评论
comments: true
tags:
  - 教程
  - Hexo
date: 2020-01-13 21:03:28
---
博客之前一直没有评论系统，因为刚建博客的时候，市面上还能用的只有畅言和Disqus，而Disqus需要魔法，畅言需要备案。
<!--more-->
后来出了个Gitment，是基于GitHub的，但也需要有账号才能评论，所以一直没去搞，直到前几天发现一个新的评论系统[Valine](https://valine.js.org/)，它不需要后端，而且支持Emoji，马上搞起。

>Valine 诞生于2017年8月7日，是一款基于LeanCloud的快速、简洁且高效的无后端评论系统。

>理论上支持但不限于静态博客，目前已有Hexo、Jekyll、Typecho、Hugo、Ghost 等博客程序在使用Valine。

我的博客基于Hexo，以下教程针对Hexo博客，参考[yilia大佬的教程](https://github.com/litten/hexo-theme-yilia/pull/646)。

因为Valine是基于[LeanCloud](https://www.leancloud.cn/)的，所以我们需要先在官网上注册一个账号，如果只是想用Valine的评论功能，那这个LeanCloud可以简单看作为一个存储评论的仓库。

1.注册成功后，进入控制台，随便创建一个应用；
![](\assets\images/200113_1.jpg)
2.进入设置，点击左侧应用Keys，记录一下AppID和AppKey；
![](\assets\images/200113_2.jpg)
3.点击左侧安全中心，因为只是用来存储评论，所以关掉其他功能；
![](\assets\images/200113_3.jpg)
4.在下方Web安全域名处添加你的博客域名，防止记录到别的网站评论；
![](\assets\images/200113_4.jpg)
5.在Hexo博客主题的配置文件_config.yml中添加如下代码；
``` bash
valine: 
 appid: '步骤2记录的AppID'
 appkey: '步骤2记录的AppKey'
 verify: false
 notify: false
 avatar: mm
 placeholder: '评论框占位符'
```
6.在主题文件layout/_partial/article.ejs中添加如下代码；

``` bash
  <% if (theme.valine && theme.valine.appid && theme.valine.appkey){ %>
    <section id="comments" style="margin:10px;padding:10px;background:#fff;">
      <%- partial('post/valine', {
        key: post.slug,
        title: post.title,
        url: config.url+url_for(post.path)
        }) %>
    </section>
  <% } %>
<% } %>
```

7.在主题文件layout/_partial/post/内新建valine.ejs文件，代码如下：
``` bash
<div id="vcomment" class="comment"></div> 
<script src="//cdn1.lncld.net/static/js/3.0.4/av-min.js"></script>
<script src="//unpkg.com/valine/dist/Valine.min.js"></script>
<script>
   var notify = '<%= theme.valine.notify %>' == true ? true : false;
   var verify = '<%= theme.valine.verify %>' == true ? true : false;
    window.onload = function() {
        new Valine({
            el: '.comment',
            notify: notify,
            verify: verify,
            app_id: "<%= theme.valine.appid %>",
            app_key: "<%= theme.valine.appkey %>",
            placeholder: "<%= theme.valine.placeholder %>",
            avatar:"<%= theme.valine.avatar %>"
        });
    }
</script>
```

8.重新编译下Hexo，评论系统就完成了。

``` bash
hexo g
hexo d
```

目前评论系统的头像是基于[Gravatar](http://cn.gravatar.com/)的，如果想使用自己的头像，需要自行注册，在评论的时候填写注册邮箱即可。

默认的匿名头像由如下7种，可自行修改步骤5中的avatar: mm；
![](\assets\images/200113_5.jpg)