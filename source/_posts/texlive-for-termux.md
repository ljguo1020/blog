---
title: 在安卓手机上安装 LaTeX
date: 2021-12-28 16:50:13
tags:
     - LaTeX
     - 安装
categories:
           - LaTeX
cover: /img/12.jpg
---
本文介绍如何在安卓手机上（本地）部署 LaTeX

---
写在前面：本文默认读者已经熟悉一些简单的 Linux 命令行操作，以及知道如何使用命令行来编译 LaTeX. (不会也没关系，花个10来分钟学一下)

---
个人本是极力反对在手机上**编程**的，但是有时候出门在外，身边没有电脑，又需要做一些代码的简单修改，调试. 这个东西倒也还是不错.

当然，我们有一些在线的编译网站，比较出名的 “overleaf” “texpage” etc . 但是对网络有一定的要求，总觉得还是不太好.下面就来介绍如何在安卓手机端安装一个 **texlive** (LaTeX 的一个发行版)：

## 安装 Termux 
### 介绍（废话）
Termux 是一款运行于 Android 系统的开源终端模拟器. 该软件提供了 Linux 环境，即使设备不具备 root 权限也可使用. 通过自带的包管理器（pkg、 apt），Termux 可以安装许多现代化的开发和系统维护工具，例如 zsh、Python、Ruby、NodeJS、MySQL 等软件.
### 如何安装
如果条件允许，可以使用 **谷歌商店** 或者 **F-Droid** 来安装它.当然也可以使用下面的百度云链接来下载安装

链接：https://pan.baidu.com/s/1b_lrPvcLXq9fIdUDAjs8MQ 
提取码：1234

安装好了直接打开，出现如下界面

![Image](https://pic4.zhimg.com/80/v2-b3b73f6d6043012a0db5300db661b76d.jpg)

#### 换源
复制以下内容，输入在这个黑窗口（以下我们称**终端**）里面

```bash
sed -i 's@^\(deb.*stable main\)$@#\1\ndeb https://mirrors.tuna.tsinghua.edu.cn/termux/termux-packages-24 stable main@' $PREFIX/etc/apt/sources.list
sed -i 's@^\(deb.*games stable\)$@#\1\ndeb https://mirrors.tuna.tsinghua.edu.cn/termux/game-packages-24 games stable@' $PREFIX/etc/apt/sources.list.d/game.list
sed -i 's@^\(deb.*science stable\)$@#\1\ndeb https://mirrors.tuna.tsinghua.edu.cn/termux/science-packages-24 science stable@' $PREFIX/etc/apt/sources.list.d/science.list
pkg update
```
然后回车，遇到不动了，直接按回车. 直到换源结束. 如下图：

![Image](https://pic4.zhimg.com/80/v2-5f02c4b4a770c079e18ecbccc1ce4ed5.jpg)

#### 美化一下终端

先安装一些基础工具，终端输入
```bash
pkg update
pkg install vim curl wget git tree -y
```
然后回车，直到安装成功，随后执行
```bash
sh -c "$(curl -fsSL https://github.com/Cabbagec/termux-ohmyzsh/raw/master/install.sh)"  
```
如果请求访问权限，请允许.随后会出现让你选择配色和字体，随便选择吧，后面可以改.

注意：这里访问 Github 可能会比较慢，甚至失败. 可以使用科学上网，或者换成以下命令
```bash
sh -c "$(curl -fsSL https://html.sqlsec.com/termux-install.sh)"  
```
然后重启终端，你会发现它变样了.
![Image](https://pic4.zhimg.com/80/v2-33310d7ef96dccf214bff5f7d20b8c18.jpg)

美化到此结束吧！

## 安装 texlive
终端执行
```bash
pkg install texlive-full
```
然后回车，等待下载安装结束. 大概又接近 4 个G吧，请保证手机内存充足.

安装完成后如下图：
![Image](https://pic4.zhimg.com/80/v2-1e6910f73ab3fe7e604382527022c42e.png)
这时候还没完成，我们依据提示需要执行：
```bash
cd $PREFIX/etc/profile.d
```
然后执行

```bash
. ./texlive.sh
```
![Image](https://pic4.zhimg.com/80/v2-bef6e16eac5cd24fdc1031aab4656fdd.jpg)

然后执行以下命令检查是否安装成功
```bash
tex -v
```
如果出现如下信息，则大功告成了. 
![Image](https://pic4.zhimg.com/80/v2-8342a9c17b0552c281b5368b7527eced.jpg)

## 编译第一份文档
### 编辑器的选择
先重启一下Termux吧.

编辑器呢，很多，这里推荐 **vim** 或者 **nano** , 分别执行以下命令安装
```bash
pkg install vim
pkg install nano
```
不会 vim 的朋友可以学习一下，下面用 nano 演示一下   
直接执行
```bash
nano main.tex
```
来创建一个 tex 文件, 然后将以下内容复制进去
```tex
\documentclass{ctexart}
\usepackage{amsmath}
\title{Welcome to \LaTeX}
\author{ljguo}
\date{\today}
\begin{document}
\maketitle

hello  \LaTeX{}!
你好  \LaTeX{}!

这是勾股定理
\[
   a^{2}+b^{2}=c^{2}    
\]
\end{document}
```
复制好后如下
![Image](https://pic4.zhimg.com/80/v2-bfc01cf26171ecaeeec135d9505a2dd5.png)

按 ```CTRL```+```O```来保存文件，随后按回车确认保存，注意文件名，不要被改了

![Image](https://pic4.zhimg.com/80/v2-767d69fc5d69fd0796a0d051ffd6115c.png)

最后 ```CTRL```+```X```,来退出编辑器，回到终端界面   
用 ```ls``` 命令查看文件是否存在
![Image](https://pic4.zhimg.com/80/v2-aa98093cfd57bb1bfba2f7a75c5281f8.png)

如果存在就可以编译了，执行命令
```bash
xelatex main.tex
```
开始编译，随后出现如下信息表示编译成功
![Image](https://pic4.zhimg.com/80/v2-4f381c509aa9ff053e0518d31ed2ca40.png)
然后用 ```ls``` 命令查看
![Image](https://pic4.zhimg.com/80/v2-d6aa8fb82e7f98eb2ee6e999326a4704.png)
多出了几个以 main 开头的文件，我们需要这个 pdf 文件，执行
```bash
termux-open ./main.pdf
```
来打开这个 pdf 文件，随后就会自动跳转到打开 pdf 了
![Image](https://pic4.zhimg.com/80/v2-d9f5e05993e7d095cfb158e9f35e6b01.jpg)

## 一些补充
使用
```bash
cd /sdcard
```
可以进入你的手机目录下，然后可以建一个文件夹来专门写 LaTeX, 同时也方便传输.

---

先到这里吧，有问题评论区指出.

## 参考
Termux 高级终端安装使用配置教程
<https://www.sqlsec.com/2018/05/termux.html>