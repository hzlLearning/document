# 在windows下安装Lnmp开发环境

## 前言
作为一名phper我们深知运行的环境是linux而开发环境是windows，他们
各有所长，那么可不可以结合他们的优点呢？在windows下开发在linux下运行
调试。答案是可以的。当然如果你用的是macbook那么当我没说！

## 名称解释
1. L（Lnmp）: L指的是`linux`操作系统。本文使用的是一款桌面系统`ubuntu`.
我使用的版本是ubuntu麒麟。你可以轻易的在他的[官网](http://www.ubuntukylin.com/downloads/)上找到他的镜像。  
这里你也可以使用其他的linux系统。如`centos`等。`centos`小巧方便，你可以对他进行简易安装。这里笔者将不在详细展开。

2. n（Lnmp）：Nginx 服务器。现在比较流行了两大服务器之一。还有一个是apache。
笔者这里使用的是nginx服务器。

3. m（Lnmp）: Mysql数据库。这个估计不需要我多解释了。轻量级的数据库。mysql是免费的，
所以越来越多的企业开始选择使用他。这里主要是想说明。如果你想使用他，就要为php安装相应的
扩展。所以理论上你可以使用任何一种数据库。

4. p（Lnmp）:PHP。本文使用的php5。有兴趣的朋友可以尝试安装php7,笔者也会补上这一空缺。

## 虚拟机的安装和配置

1. 安装vmware或virtual box。

2. 安装vm tools 设置共享文件夹
> **注意**  
    这里可能出现的问题有两个：  
    1.virtual box安装tools时找不到安装文件。这里如果你已经点击了安装tools的选项看不到是正常的。需要将你刚推入的设备挂载起来。
    使用`mount`命令进行挂载。  
    2.