---
layout: post
title: 更换Hexo博客部署方式
comments: true
tags:
  - 教程
  - Hexo
date: 2020-06-29 21:57:19
---
半年多没更新博客，突然忘记如何推送文本，太可怕了，特意找到之前教程，重新记录一下。原作者是[CrazyMilk](https://www.zhihu.com/question/21193762/answer/79109280)。
<!--more-->

### 部署
1. 在GitHub上创建一个用来存放博客的仓库；
2. 创建分支，使其拥有两个分支：master 和 Hexo；
3. 设置Hexo为默认分支，
4. 使用git clone拷贝仓库到本地；
5. 在拷贝到本地的仓库文件夹内运行Git bash;
6. 依次执行（Hexo分支）：
	>npm install hexo  
	>hexo init  
	>npm install  
	>npm install hexo-deployer-git
7. 修改_config.yml中的deploy参数，分支改为master；
8. 依次执行：
	>git add .
	>git commit -m "..."
	>git push origin hexo
9. 执行hexo g -d生成网站并部署到GitHub上。

### 发布
1. 在仓库文件夹内运行Git bash;
2. 执行Hexo n "文章名"；
3. 依次执行（此步在Hexo分支建立网页文件）：
	>git add .
	>git commit -m "..."
	>git push origin hexo
4. 执行hexo g -d生成网页并部署到GitHub上（此步生成网页存放在master）。

### 迁移
1. 使用git clone拷贝仓库到本地（默认Hexo分支）;
2. 在拷贝到本地的文件夹内运行Git bash;
3. 依次运行命令：
	>npm install hexo
	>npm install
	>npm install hexo-deployer-git