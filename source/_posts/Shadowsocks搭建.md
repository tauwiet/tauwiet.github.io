---
layout: post
title: Shadowsocks搭建
comments: true
tags:
  - 教程
  - SS
date: 2019-03-28 14:52:10
---
![](\assets\images/190328_1.jpg)
搬瓦工下架所有OpenVZ的服务，不能再以之前的价格续费，既如此，换个供应商玩玩，记录下重新搭建Shadowsocks过程中出现的一些状况。
<!--more-->
Shadowsocks搭建于[Virmath](https://virmach.com/)上，最便宜的服务器每月仅需1.25美元，
系统信息：Debian 4.9.65-3+deb9u1 (2017-12-23) x86_64

1 首先获取服务器的IP，端口和密码，并登陆SSH；  

2 安装pip  
```bash
apt-get install -y python-pip
```

3 通过pip安装Shadowsocks
```bash
pip install shadowsocks
```

安装成功后显示:
![](\assets\images/190328_2.jpg)

4 新建并配置shadowsocks.json
```bash
vi /etc/shadowsocks.json
```

配置如下：
```bash
{
 "server":"服务器IP",
 "local_address":"127.0.0.1",
 "local_port":1080,
 "port_password":{
 "端口1":"密码1",
 "端口2":"密码2"
 },
 "timeout":300,
 "method":"aes-256-cfb",
 "fast_open": false
}
```

5 开启Shadowsocks服务
```bash
ssserver -c /etc/shadowsocks.json -d start 
```

成功后显示:
![](\assets\images/190328_3.jpg)

后记：
我在开启服务的时候发生了如下的错误，
![](\assets\images/190328_4.jpg)
原因是openssl1.1.0版本中，废弃了EVP_CIPHER_CTX_cleanup函数。
参考一篇[博客](https://blog.csdn.net/blackfrog_unique/article/details/60320737)所述，修改代码后，此问题得以解决。

文件位置：/usr/local/lib/python2.7/dist-packages/shadowsocks/crypto/openssl.py

将第52行的  
```bash
libcrypto.EVP_CIPHER_CTX_cleanup.argtypes = (c_void_p,)
```

改为  
```bash
libcrypto.EVP_CIPHER_CTX_reset.argtypes = (c_void_p,)
```

将第111行的  
```bash
libcrypto.EVP_CIPHER_CTX_cleanup(self._ctx)
```

改为  
```bash
libcrypto.EVP_CIPHER_CTX_reset(self._ctx)
```
