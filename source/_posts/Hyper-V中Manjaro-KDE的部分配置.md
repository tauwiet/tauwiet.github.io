---
layout: post
title: Hyper-V中Manjaro-KDE的部分配置
comments: true
tags:
  - 教程
  - Manjaro
date: 2020-12-18 22:13:49
---
Hyper V中安装Manjaro的过程中，遇到了一些问题，在此记录。
<!--more--> 
1 安装过程卡到引导界面，可能是由于sddm或lightdm启动失败。
依次执行以下命令：
```bash
$ su
$ pacman –Sy 
$ pacman –S xf86-video-fbdev
$ systemctl restart sddm
```

2 安装xrdp远程桌面服务
依次执行以下命令：
```bash
$ sudo pacman –Sy
$ sudo pacman –S git
$ cd Downloads (or wherever you want to download it to)
$ git clone https://github.com/Microsoft/linux-vm-tools.git ./linux-vm-tools
$ cd linux-vm-tools/arch
```

修改./makepkg.sh文件，将xrdp改成xrdp-git，因为默认版本的xrdp不支持linux-vm-tools。
```bash
$ ./makepkg.sh
$ ./install-config.sh
```

编辑.xinitrc文件，删除下面一行中的“--exit-with-session”
local dbus_args=(--sh-syntax --exit-with-session)

在windows的shell下键入：
```bash
$ set-vm -vmname <vmname> -EnhancedSessionTransportType HvSocket
```

3 更换中国源
```bash
$ sudo pacman-mirrors -i -c China -m rank
$ sudo pacman –Syy
```

4 安装vim
```bash
$ sudo pacman –S vim
```

5 安装google输入法
```bash
$ sudo pacman -S fcitx-configtool
$ sudo pacman -S fcitx-googlepinyin
$ sudo vim ~/.xprofile
```

在文件中添加如下内容
```bash
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"
```

重启系统

6 安装Pulseaudio启动声音
```bash
$ cd Downloads
$ git clone https://aur.archlinux.org/pulseaudio-module-xrdp-git.git
$ cd pulseaudio-module-xrdp-git
$ makepkg –sri
$ pulseaudio -D
```
将编译好的pkg/pulseaudio-module-xrdp-git/路径下的文件，参照目录，放到相应位置，包含module-xrdp-sink.so和module-xrdp-source.so

```bash
$ pulseaudio --kill
$ pulseaudio --start
```

有个问题是，每次开机都需要通过pulseaudio -D启动声音，没找到解决办法，尝试过将这一行命令做成脚本开机自启，也没有成功，可能自启脚本配置的有问题。

7 安装electron-ssr
在AUR库中下载libappindicator-sharp，进入目录执行：
```bash
$ makepkg -sri
```
在[对应页面](https://github.com/qingshuisiyuan/electron-ssr-backup/releases)下载适应的软件包。
```bash
$ Sudo pacman –U 软件包文件名
$ sudo pacman -S gconf
$ export http_proxy="http://127.0.0.1:12333"
```

配置浏览器代理，Firefox直接在代理中进行相关配置，chrome需要额外插件。


