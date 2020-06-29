---
layout: post
title: 一些常用MatLab画图指令
comments: true
date: 2018-02-11 16:38:29
tags:
- 教程
- MatLab
---
![](/assets/images/180211_1.jpg)
最近用到一些MATLAB画图命令，每次都查还是有些麻烦，
于是将用过的程序总结一下，记录于此。
<!--more-->
以一个简单的正弦波为例，画出它的时域波形和频域波形，
```
clear all;                  %清除记录
N=2000;                     %获得信号的点数
fs=1000;                    %抽样频率，要高于信号最高频率的2倍
t=(0:N-1)/fs;               %信号时域横轴向量
f=(0:N-1)*fs/N;             %信号频域横轴向量
x=2*sin(10*pi*2*t);
figure('name','时域图');     %出图
plot(t,x);
figure('name','频域图');     %出图
y=abs(fft(x));
plot(f,y);
```
结果如图：
![](/assets/images/180211_2.jpg)
![](/assets/images/180211_3.jpg)

因为频率图横坐标太宽，所以啥都看不出来，
如果要调整一下曲线，只需在plot后面加上其他语句就行。

限制下频率的横纵坐标，前两个数是横坐标起止，后两个数是纵坐标起止；
```
axis([0 20 0 2000]);
```

给时域横纵坐标都加个标签，前一个是横坐标标签，后一个是纵坐标标签；
```
xlabel('时间/s');ylabel('幅度');
```

给图加一个标题；
```
title('时域');
```

给曲线加上网格线；
```
grid on;
```

曲线加颜色并加粗，LineWidth后的数字代表线条粗细；
```
plot(t,x,'r','LineWidth',2);
plot(f,y,'g','LineWidth',2);
```

颜色对应：
'r' 红色 'm' 粉红 'g' 绿色 'c' 青色
'b' 兰色 'w' 白色 'y' 黄色 'k' 黑色

结果如图：
![](/assets/images/180211_4.jpg)
![](/assets/images/180211_5.jpg)

将两种波形显示在一张图上subplot(行数，列数，序号数)；
```
subplot(2,1,1)   
plot(t,x,'r','LineWidth',2);
subplot(2,1,1)   
plot(f,y,'g','LineWidth',2);
```

![](/assets/images/180211_6.jpg)

将两种波形显示在一个坐标系内，只需在plot后加俩单词就行。
```
hold on;
```

先记到这，后面用到再补充。