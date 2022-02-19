---
title: texlive-for-linux
date: 2021-12-28 16:49:19
tags: 
     - LaTeX
     - 安装
categories:
           - LaTeX
cover: /img/8.jpg
---
我们采用镜像下载安装
# 下载texlive镜像文件
这里给出清华大学的镜像源
# https://mirrors.tuna.tsinghua.edu.cn/CTAN/systems/texlive/Images/texlive2021-20210325.iso
下载到一个自己能够找得到的地方，这里默认你已经熟悉一些基本的命令行操作
### 挂载镜像文件
这里假设你的.iso文件路径为/path/iso/textive2021-texlive20210325.iso ,在命令行窗口通过 cd 命令进入该文件夹，如下图，这是我的.iso文件所在路径
![文件路径](https://pic4.zhimg.com/80/v2-410ed05383391d060d87efd7baf41000.png)
然后我们就可以开始进行挂载镜像了
>mkdir ~/iso

在命令行执行以上命令，这个命令是在 “~” 目录，也即是家(home)目录下创建一个名叫“iso”的目录，用于挂载镜像文件。这里不理解的话，其实镜像文件也相当于一个压缩包，我们创建这个文件夹就是用来将.iso文件解压在其中
![创建挂载目录](https://pic4.zhimg.com/80/v2-8fd10bb4528202442809fc82cf1f29bb.png)
>sudo mount texlive2021-20210325.iso ~/iso -o loop

这条命令用于挂载镜像
![挂载镜像](https://pic4.zhimg.com/80/v2-09efa3cf85368e97ed6b64c5eab0b2e3.png)

随后进入刚刚创建的文件夹，执行
>cd ~/iso

![进入挂载目录](https://pic4.zhimg.com/80/v2-0caf895b241de2f5139d0067d6bd5c3c.png)
然后用 ls 命令可以看到里面有很多文件，其中有个install-tl就是我们的安装脚本

![查看内部文件](https://pic4.zhimg.com/80/v2-04e66fae15176a6ba11007a3e8779d9e.png)
# 开始安装
然后执行

>sudo ./install-tl

![执行脚本，安装texlive](https://pic4.zhimg.com/80/v2-4789ed193fa5c8c2324bad0f87c42f8b.png)

你将会看到：

![选择界面](https://pic4.zhimg.com/80/v2-289a9b3d4f0b35236a2f67da9aca1963.png)


输入 i 然后回车，等待安装，界面如下

![安装界面](https://pic4.zhimg.com/80/v2-70e4283c185a885d3889eebc8a0df929.png)

一直等待，大概在20~30分钟，也说不准，直到出现 
>欢迎进入 TeX Live 的世界

或者是

>welcome to texlive

等文字信息，即表示安装成功.

你以为你真的安装成功了吗？？？，nonono，接下来我们还需要做一些配置：

# 添加环境变量
执行
> sudo gedit ~/.bashrc

![打开 .bashrc](https://pic4.zhimg.com/80/v2-7430a4e6f884da9e41244fba0c23cbb6.png)
此命令表示使用 Gedit 编辑器打开 .bashrc 这个配置文件，如果你是WSL用户,没有自带该编辑器，可将 gedit 换成 vi 或者 vim ，打开之后将以下内容添加到里面（如果你用的zsh，可以将 .bashrc 换成 .zshrc ）

注意：如果用 vim 请先了解一下 vim 编辑器的使用方法

>export PATH=/usr/local/texlive/2021/bin/x86_64-linux:$PATH
>
>export MANPATH=/usr/local/texlive/2021/texmf/doc/man:$MANPATH
>
>export INFOPATH=/usr/local/texlive/2021/texmf/doc/info:$INFOPATH

![配置 .bashrc](https://pic4.zhimg.com/80/v2-fa1d39b316a0907ccf3e1fec315fd677.png)
保存，然后命令行继续执行

>sudo gedit /etc/manpath.config

同样，也是打开一个配置文件，在里面相对应的位置加入以下内容，同样也可用 vim 编辑器打开

>MANPATH_MAP  /usr/local/texlive/2021/bin/x86_64-linux  /usr/local/texlive/2021/texmf/doc/man

![配置 manpath.config](https://pic4.zhimg.com/80/v2-6ae8ad7d40b3bf73d849ba1c2214f83e.png)

保存退出，重新打开终端，用以刷新配置文件，然后在新的终端里面执行

>tex -v

得到如下界面，则表示安装成功

![终于成功啦](https://pic4.zhimg.com/80/v2-ffd58c5da049e4b6db2ac6e1815e0e25.png)

接下来你就可以开始写 LaTeX 代码了.

# 开始第一份文档

终端执行
>touch main.tex

新建一个 .tex 文件，用编辑器打开这个文件，在里面写入代码

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

![写入 .tex 文件内容](https://pic4.zhimg.com/80/v2-1225d5c19522e319747215754fda6077.png)

保存，然后命令行执行

>xelatex main.tex

![编译文件](https://pic4.zhimg.com/80/v2-857992e8c281cb5a6a2572f93a01b24d.png)

你将会看到如下内容

![开始编译了](https://pic4.zhimg.com/80/v2-a76aea94d409b224a70bfd6b28f92f8d.png)

直到......

![编译成功](https://pic4.zhimg.com/80/v2-41772d1185c7d129a3b1fb839f0d1095.png)

这个过程不过1~2秒，表示编译成功了，随即使用 ls 命令来查看里面的东西

![pdf文件](https://pic4.zhimg.com/80/v2-7ce214cb238cee5d20dfdcab3d1ec701.png)

多了几个main开头的文件（请自动忽略我的其他文件）然而我们关系的就是这个pdf文件，请到相应的文件加打开它，如果不知道你自己所处的文件夹位置，可以使用 pwd 命令来查看.

![查看所在目录](https://pic4.zhimg.com/80/v2-aa2b430ef39759011067f3da2c9aad0d.png)

然后去打开它吧.

![pdf文件显示](https://pic4.zhimg.com/80/v2-50b1f9eb90425e81bc502f5a6c3e5eb3.png)

这时候，你应该算是大功告成了.别急着乐，在LaTeX这条路上，你只是跨入了大门.不过，嘿嘿，万事开头难嘛.加油！

# 其他

请记住 texdoc 命令，用于打开宏包帮助文档，使用方法如下

>texdoc 宏包/文件名

例如， texdoc amsmath 查看amsmath 宏包的说明文档
另外，推荐入门必读的手册

>lshort-zh-zn
>
>install-latex-guide-zh-cn

第一份文档，入门使用，看完它之后，你的一些日常写作应当没有任何问题，强烈推荐

第二份文档，解决你安装过程中遇到的 99% 的问题，请按需仔细阅读

调出它们的方法很简单，只需要命令行执行

>texdoc lshort-zh-zn
>
>texdoc install-latex-guide-zh-cn

好了，先写到这里吧.......