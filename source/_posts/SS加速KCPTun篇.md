---
layout: post
title: SS加速KCPTun篇
comments: true
date: 2018-05-04 07:34:32
tags:
- 教程
- SS
---
![](/assets/images/180504_1.jpg)  
迫于最近看有关速度特别差，就想着给我的SS服务加个速，
网上的KCPTun加速写的看不懂，端口什么的叙述的很乱，
花了1天时间终于成功试出来了，在此记录一下步骤。
<!--more-->
总体上需要三块区域的配置（我没有苹果的产品）
1.服务器端的配置；
2.windows端的配置；
3.安卓端的配置；
记得所有KCPTun的版本要保持一致，如后缀都为20170315。

## 服务器端配置
1.创建存放KCPTun的文件夹；
2.进入文件夹；
3.下载linux版KCPTun压缩文件；
4.解压；

``` bash
mkdir ~/kcptun
cd /root/kcptun
wget https://github.com/xtaci/kcptun/releases/download/v20170329/kcptun-linux-amd64-20170315.tar.gz
tar -zxvf kcptun-linux-amd64-20170315.tar.gz
```
解压出来的两个文件，client开头的客户端和server开头的服务端。

5.建立start脚本：
``` bash
vim /root/kcptun/start.sh
```
``` bash
#!/bin/bash
cd /root/kcptun/
./server_linux_386 -c /root/kcptun/server-config.json > kcptun.log 2>&1 &
echo "Kcptun started."
```

6.建立stop脚本：
``` bash
vim /root/kcptun/sto.sh
```
``` bash
#!/bin/bash
echo "Stopping Kcptun..."
PID=`ps -ef | grep server_linux_amd64 | grep -v grep | awk '{print $2}'`
if [ "" !=  "$PID" ]; then
  echo "killing $PID"
  kill -9 $PID
fi
echo "Kcptun stoped."
```

7.建立restart.sh脚本：
``` bash
vim /root/kcptun/restart.sh
```
``` bash
#!/bin/bash
cd /root/kcptun/
sh stop.sh
echo "Restarting Kcptun..."
sh start.sh
```

8.添加脚本执行权限：
``` bash
chmod +x *.sh
```

9.建立KCPTun配置文件(注释不写)：
``` bash
vim /root/kcptun/server-config.json
```
``` bash
{
    "listen": ":29900",              #监听端口，用一个未占用的就行，起名端口1
    "target": "127.0.0.1:12948",     #SS的IP和端口，同机上可以这样，起名端口2
    "key": "https://awy.me",         #密码
    "crypt": "salsa20",              #加密方式
    "mode": "fast2",                 #加速模式
    "mtu": 1350,
    "sndwnd": 1024,
    "rcvwnd": 1024,
    "datashard": 70,
    "parityshard": 30,
    "dscp": 46,
    "nocomp": false,
    "acknodelay": false,
    "nodelay": 0,
    "interval": 40,
    "resend": 0,
    "nc": 0,
    "sockbuf": 4194304,
    "keepalive": 10
}
```

10.启动KCPTun并添加开机运行：
``` bash
sh /root/kcptun/start.sh
chmod +x /etc/rc.local;echo "sh /root/kcptun/start.sh" >> /etc/rc.local
```

服务器端到此就设置完成。

## windows端配置
下载GUI版的KPCTun客户端，长这样：
![](/assets/images/180504_2.jpg)  
下载windows版KCPTun客户端，能解压出client_windows_amd64.exe和kcptun_gclient.exe文件，同linux版差别不大。
将这三个文件放入同一文件夹，因为运行会自动产生额外配置文件。

基础参数中：
本地监听端口随意选一个未占用的，起名端口3；
服务器地址为VPS的IP；
端口为服务器端设置端口，即端口1。

可选参数、传输模式、隐藏参数中：
完全依照服务器端的配置设置就行。

点击启动，出现如下界面就行了。
![](/assets/images/180504_3.jpg) 

打开SS小飞机：
IP地址：127.0.0.1；
端口：本地监听端口，即为端口3；
密码和加密方式不变。

windows端配置到此设置完成。

## 安卓端配置
安卓端除了最新版小飞机外，还需要一个叫kcptun的插件，
插件以软件的形式安装，完成后小飞机最下面插件才能选择，

服务器填写VPS的IP；
远程端口为服务器监听端口，即端口1；
密码和加密方式照旧。

插件配置如下，根据服务器端的填写修改为自己：
``` bash
dscp=46;nodelay=0;parityshard=30;interval=40;rcvwnd=1024;crypt=salsa20;nc=0;acknodelay;resend=2;autoexpire=0;key=kcptun;mode=fast2;mtu=1350;datashard=70;keepalive=10;sndwnd=1024;sockbuf=4194304
```
到此安卓端配置完成。
---

配置下来其实不难，比较混乱的就是端口，一共三个端口，我就能搜出来好几个版本。