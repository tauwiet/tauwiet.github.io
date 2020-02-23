---
layout: post
title: 修改SSH登陆端口及密码
comments: true
tags:
  - 教程
  - VPS
date: 2019-03-29 15:06:57
---
![](\assets\images/190329_1.jpg)
新购买的VPS服务器SSH登陆端口和密码通常是系统默认的，IP不太会变，为了安全考虑，其他东西最好自己换一下。
<!--more-->
## 修改SSH默认端口

1 编辑SSH配置文件
```bash
	vi /etc/ssh/sshd_config
```

2 去掉Port 22前的注释#，添加一行预设置的端口
```bash
	Port 22
	Port 222
```

3 重启SSH
``` bash
	systemctl restart sshd.service
```

4 查看防火墙配置信息
```bash
	iptables -L -n  
```

如果出现如下结果，说明防火墙形允许所有请求。
![](\assets\images/190329_2.jpg)  

注意：后续防火墙的设置针对SSH端口是有保护性的，但我在配置完成后Shadowsocks无法正常使用，翻了一下午博客，暂时没找到行之有效的解决方案。

5 防火墙允许通过新端口  
```bash
	iptables -D INPUT -m state --state NEW -m tcp -p tcp --dport 222 -j DROP
	iptables -I INPUT -m state --state NEW -m tcp -p tcp --dport 222 -j ACCEPT
```

6 保存iptables 配置
```bash 
	iptables-save > /etc/iptables.up.rules
	echo -e '#!/bin/bash\n/sbin/iptables-restore < /etc/iptables.up.rules' > /etc/network/if-pre-up.d/iptables
	chmod +x /etc/network/if-pre-up.d/iptables
```

7 恢复第2步端口22的注释#，禁止端口22通过防火墙
```bash 
	iptables -I INPUT -m state --state NEW -m tcp -p tcp --dport 22 -j DROP
```

8 参照第6步，保存配置

## 修改SSH登陆密码

1 登陆SSH

2 输入passwd

3 输入两次新密码后修改成功