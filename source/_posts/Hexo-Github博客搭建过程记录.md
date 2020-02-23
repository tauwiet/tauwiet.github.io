---
layout: post
title: Hexo&Github博客搭建过程记录
date: 2017-07-11 10:36:03
comments: true
tags:
- 教程
- Hexo
---
早之前就有兴趣搭建博客，不为输出思想，只为好奇折腾，但因种种原因一直没有实际行动。
近期枯燥无味的实验后，略感无趣，索性尝试下搭建博客，特此记录一下搭建过程。
手上有一台VPS，平时负责跑SS和EFB，本想使用公认最牛的Wordpress，直接将博客搭建在VPS内，由于VPS内存太小，不会优化，如果搞坏了，EFB我也不会独自搭建，此方法只能作罢。
网上较多推荐的，不需要使用VPS的博客搭建平台，如Hexo，台湾籍大神开发，中文支持肯定没问题，依托于Github，依赖少等原因，最终选定Hexo来管理博客。
<!--more--> 
# 前期准备
---
**安装Node.js**<br>

>Node.js是一个基于Chrome V8引擎的JavaScript运行环境。 
 下载地址[Node.js](https://nodejs.org/en/)
>通过命令行查看安装版本node --version

<br>**安装Git**<br>

>Git是开源的分布式版本控制系统，用于敏捷高效地处理项目。我们的网站在本地搭建好了，需要使用Git同步到GitHub上。
 下载地址[Git](https://git-scm.com/)
>通过命令行查看安装版本git

<br>**安装Hexo**<br>

>Hexo就是我们的个人博客网站的框架，此时需要创建一个文件夹，任意命名。
 命令行下  npm install hexo-cli -g
>通过命令行查看版本hexo --version

<br>**创建Github账号**<br>
 >Hexo是一款基于Node.js的静态博客框架，依赖少易于安装使用，可以方便的生成静态网页托管在GitHub上，故需要一个Github账号。
 同时在github里生成一个库，名字就叫[username].github.io
 这个是固定的。

# 博客搭建 
---
**Hexo初始化**<br>

 文件夹下Git Bash Here，执行hexo init
 安装必要组件npm install
 
  hexo简单指令：
``` bash
   hexo n "我的博客" == hexo new "我的博客" #新建文章 
   hexo g == hexo generate #生成 
   hexo s == hexo server #启动服务预览 
   hexo d == hexo deploy #部署
```
 通过浏览器打开 http://localhost:4000/ 即可预览博客。

**关联到Guihub**

 打开Git Bash
``` bash
   git config --global user.name "你的GitHub用户名"
   git config --global user.email "你的GitHub注册邮箱"
```
 生成ssh密钥文件
``` bash
   ssh-keygen -t rsa -C "你的GitHub注册邮箱"
```
 找到生成的.ssh的文件夹中的id_rsa.pub密钥，将内容全部复制到[GitHub_Settings_keys](https://github.com/settings/keys) 页面中，新建new SSH Key下，最后点击Add SSH key。
 在Git Bash中检测GitHub公钥设置是否成功，通过:
``` bash
   ssh git@github.com 
```
 在根目录里修改_config.yml配置文件
``` bash
 deploy:
   type: git
   repo: github.com/[username]/[username].github.io 
```

 最后安装Git部署插件，输入命令：
``` bash
   npm install hexo-deployer-git --save
```
 执行hexo d，将可在[username].github.io 网页下查看自己的博客


# 主题（以Yilia主题为例）
---
**安装**<br>

``` bash
   git clone https://github.com/litten/hexo-theme-yilia.git themes/yilia
```

<br>**配置**<br>

  修改hexo根目录下的 _config.yml下的theme: yilia
  同时\themes\yilia下的 _config.yml也可以按需要修改  

**更新**<br>

``` bash
  cd themes/yilia
  git pull
```

# 绑定域名
---
**购买域名**<br>

以阿里云为例，按个人喜好自行购买域名

<br>**域名解析**<br>

购买域名后，进入阿里云控制台，域名列表查看已购买的域名。
点击解析，并添加：
> CNAME---@---[username].github.io
> CNAME--www--[username].github.io
完成个人域名向博客的映射

<br>**博客映射域名**<br>

进入Github博客的仓库，添加文件
>文件名为CNAME(注意：没有扩展名)，文件内容为个人域名(注意：没有http，没有www)。

本地根目录下的source文件夹中添加如上CNAME文件，完成添加后重新部署，即可通过个人域名访问博客。

# 换电脑后的部署
---
 依照上述步骤重新部署，并替换以下文件即可
>配置文件: _config.yml
 主题文件: theme/
 博客文件: source/
 文章模板: scaffolds/
 使用包说明: package.json（经测试非必要）
>限定忽略文件: .gitignore（经测试非必要）

# 其他
---
至此，整个博客搭建完成，耗时半天吧。
既然搭建成功，那就尽量写下去吧，哪怕一周一次，一月一次，一年一次，
反正这博客地址知道的人也少。
理工科宅男，打小就没什么写作能力，希望这玩意能慢慢影响到我吧。

 