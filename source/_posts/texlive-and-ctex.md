---
title: Windows 系统下texlive与CTeX套装共存 
katex: true
date: 2022-06-09 23:03:17
tags:
cover: /img/9.jpg
---

## 介绍

texlive想必大家都已经很熟悉了，这里不做介绍；CTeX套装我不熟悉，也不做过多介绍

CTeX套装已经10年左右未更新了，CTeX套装的存在是为了解决正文在 LaTeX 中的排版，现在已经有了更好的替代品 **ctex宏集** ,原则上来说，它已经完成自己的使命，应该退出历史的舞台了.

但是由于国内有些中文期刊仍然在使用 CTeX 套装，所以要想投稿这些期刊，不得不使用它，一些大佬建议不要在一台机器上装两个发行版，原因应该是当你使用编译引擎编译文档时，新手无法确定使用的是哪个发行版提供的，本文旨在简单介绍如何通过修改环境变量，使得可以控制使用哪一个发行版

## 安装

点击下面链接，下载CTeX套装，如何无脑下一步安装就行，建议安装位置不要放在C盘（我的安装位置是E:\CTEX）

[CTeX下载地址](https://mirrors.tuna.tsinghua.edu.cn/ctex/legacy/2.9/CTeX_2.9.2.164_Full.exe) 

安装好之后，打开环境变量，具体方法是 

右键此电脑 ---> 属性 --->  高级系统设置 ---> 环境变量 

找到系统变量中的 Path 变量栏，然后打开![](https://raw.githubusercontent.com/ljguo1020/Images/main/img/061901.png)

![](https://raw.githubusercontent.com/ljguo1020/Images/main/img/061902.png)

红框的都是 CTeX 的环境变量，篮框的是 texlive 的环境变量，原则上编译的时候都是在命令行调用可执行程序来编译. 所以它会优先选择环境变量 **靠前** 的路径来执行.

如上图，在 ```E:\CTEX\MiKTeX\miktex\bin``` 和 ```D:\texlive\2022\bin\win32``` 两个路径下都有 ```tex.exe``` 这个可执行文件，但是由于 CTeX 的环境变量靠前，所以会优先选择 CTeX, 如下图

![](https://raw.githubusercontent.com/ljguo1020/Images/main/img/061903.png)

当我将 texlive 的环境变量移到前面后，如下

![](https://raw.githubusercontent.com/ljguo1020/Images/main/img/061904.png)

![](https://raw.githubusercontent.com/ljguo1020/Images/main/img/061905.png)

至此，大概明白了如何选择需要的发行版来编译了

## 其它

其它的，比如编辑器的使用，我就不做解释了