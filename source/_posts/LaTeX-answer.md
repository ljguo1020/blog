---
title: 使用 LaTeX3 在试卷中做一个答案统计
date: 2022-02-17 08:42:23
update: 2022-02-17 13:50:13
tags:
     - LaTeX3
     - 实例
categories:
           - LaTeX
copyright_url: https://ljguo1020.github.io/2022/02/17/LaTeX-answer/
cover: /img/main.jpg
---

## 问题的提出  

在群里有老师想要实现如下的一个答案统计效果

![](https://ljguo-1308058910.cos.ap-nanjing.myqcloud.com//img/QQ截图20220217123653.png)

这本来是 ```exam``` 文档类提供的一个功能，奈何有些老师不想使用这个文类，我们只好造一个轮子

## 分析问题

我们需要注意到：

- 这个**答案列表**应该是在排版选择题的时候就输入，而不是最后来输入
- 也就是我们需要先将前面的答案暂存起来，最后再来排版出来
- **计数**功能，我们还需要去检测答案列表中各个选项的数量

## 解决问题 

首先我们需要一个排版选择题答案的命令

有不少老师在使用 ```\xx{}{}{}{}```这个命令，如果你没有类似的命令，我仿造他们的思路造了一个这样的命令```\choices{}{}{}{}```，源码如下，这里参考了公众号[**不会功夫的熊猫**](https://mp.weixin.qq.com/s/nq6Qx_yCX5Vt4Acl--RX6w)的方法

> 这里排版选择题我特别推荐[xkwxdyy](https://github.com/xkwxdyy)的 ```xchoices``` 宏包，这个宏包功能十分强大，对选择题可以很灵活的排版，项目地址 https://github.com/xkwxdyy/xchoices

```tex
\ExplSyntaxOn
\cs_new_protected:Npn \g_choices: #1#2#3#4 {
  \par\noindent
\box_clear_new:N \l_choiceA_box
\box_clear_new:N \l_choiceB_box
\box_clear_new:N \l_choiceC_box
\box_clear_new:N \l_choiceD_box

\hbox_set:Nn \l_choiceA_box{A.#1~~}
\hbox_set:Nn \l_choiceB_box{B.#2~~}
\hbox_set:Nn \l_choiceC_box{C.#3~~}
\hbox_set:Nn \l_choiceD_box{D.#4~~}

\dim_zero_new:N \l_choice_wd_max_AB
\dim_zero_new:N \l_choice_wd_max_CD 
\dim_zero_new:N \l_choice_wd_max
 \dim_set:Nn \l_choice_wd_max_AB {\dim_max:nn{\box_wd:N \l_choiceA_box}{\box_wd:N \l_choiceB_box}}
 \dim_set:Nn \l_choice_wd_max_CD {\dim_max:nn{\box_wd:N \l_choiceC_box}{\box_wd:N \l_choiceD_box}}
 \dim_set:Nn \l_choice_wd_max {\dim_max:nn{\dim_use:N \l_choice_wd_max_AB}{\dim_use:N \l_choice_wd_max_CD}}

\dim_compare:nNnTF{\dim_use:N \l_choice_wd_max} < {0.25\linewidth}
{
\box_set_wd:Nn \l_choiceA_box {0.25\linewidth}
\box_set_wd:Nn \l_choiceB_box {0.25\linewidth}
\box_set_wd:Nn \l_choiceC_box {0.25\linewidth}
\box_set_wd:Nn \l_choiceD_box {0.25\linewidth}

\box_use:N \l_choiceA_box
\box_use:N \l_choiceB_box
\box_use:N \l_choiceC_box
\box_use:N \l_choiceD_box
}
{
\dim_compare:nNnTF{\dim_use:N \l_choice_wd_max} < {0.5\linewidth}
{
\box_set_wd:Nn \l_choiceA_box {0.45\linewidth}
\box_set_wd:Nn \l_choiceB_box {0.45\linewidth}
\box_set_wd:Nn \l_choiceC_box {0.45\linewidth}
\box_set_wd:Nn \l_choiceD_box {0.45\linewidth}

\box_use:N \l_choiceA_box
\box_use:N \l_choiceB_box
\par\vspace*{0.5em}\hspace{2em}
\box_use:N \l_choiceC_box
\box_use:N \l_choiceD_box
}
{
\box_use:N \l_choiceA_box
\par\vspace*{0.5em}\hspace{2em}
\box_use:N \l_choiceB_box
\par\vspace*{0.5em}\hspace{2em}
\box_use:N \l_choiceC_box
\par\vspace*{0.5em}\hspace{2em}
\box_use:N \l_choiceD_box   
}
}
}
\cs_set_eq:NN \choices \g_choices:
\ExplSyntaxOff
```

有了个命令，就比较方便了，我们只需要在原有的命令上面加上一个参数来接受正确答案的选项就好了

```tex
\NewDocumentCommand{\choices}{ommmm}
{
  \IfNoValueTF{#1}
    {\seq_put_right:Nn\__answer_list_seq{\space}}
    {\seq_put_right:Nn\__answer_list_seq{#1}}
  \choice{#2}{#3}{#4}{#5}
}
```

这里重新定义了一个```\choices[]{}{}{}{}``` 命令，第一个可选参数接受正确答案

观察到上面有三行代码

```tex 
\IfNoValueTF{#1}
    {\seq_put_right:Nn\__answer_list_seq{\space}}
    {\seq_put_right:Nn\__answer_list_seq{#1}}
```

这行代码是用于检测第一个可选参数，如果有可选参数，则将接受到的参数向右追加到 ```\__answer_list_seq``` 这个序列里面，如果没有可选参数，追加一个 ```\space``` 标记

下面我们需要定义和初始化一些变量

```tex
\seq_clear_new:N \__answer_list_seq % 用于保存答案列表
\int_zero_new:N \__choice_amount_int % 用于获取各个选项数量
```

我们来解释一些上面的 ```\seq_put_right:Nn``` 函数，例如现在我们有一个空序列变量 ```\__answer_list_seq``` 我们使用如下命令后

```tex
\choices[A]{A}{B}{C}{D}
\choices[B]{A}{B}{C}{D}
\choices[C]{A}{B}{C}{D}
\choices[A]{A}{B}{C}{D}
\choices[C]{A}{B}{C}{D}
```

此时这个```\__answer_list_seq``` 的值就是 ```ABCAC``` 了

使用如下命令可以获取到 ```\__answer_list_seq```  这个序列某项的值

```tex
\seq_item:Nn\__answer_list_seq{2} % ---> B
```

那么我们来定义一个命令来显示答案列表，```\seq_count:N``` 命令用于获取序列的长度，```\int_step_inline:nnn``` 用于创建一个整数循环

```tex
\NewDocumentCommand{\answerlists}{}
{
答案列表:~\int_step_inline:nnn{1}{\seq_count:N \__answer_list_seq}
{##1.\seq_item:Nn\__answer_list_seq{##1}~}
}
```

此时再使用 ```\answerlists``` 命令就可以得到

>答案列表: 1.A 2.B 3.C 4.A 5.C

下面的问题就是来解决这个选项计数的功能

例如现在我们的 ```\__answer_list_seq``` 中保存的值为 ```ABCAC``` ，我们如何去获取各个选项的值呢？

当然这里有一个思路，可以用 ```\seq_map_inline:Nn ``` ，然后使用一个加法计数器来得到各选项的值，但是感觉比较麻烦，所以这里我将使用正则表达式来获取匹配到的数量

```tex
\regex_count:nnN{A}{ABCDAA}\l_tmpa_int % ---> \l_tmpa_int = 3
```

这个 ```\regex_count:nnN``` 函数接受三个参数，第一个接受一个正则表达式，第二个接受待匹配内容，第三个接受一个整数变量，用于保存匹配到的次数的整数值

我们尝试 

```tex
regex_count:nnN{A}{\__answer_list_seq}\__choice_amount_int
```

这样是不行的，因为第二个参数的 ```n``` 变体，无法为我们展开 ```\__answer_list_seq``` ，我们需要定义一个 ```V``` 变体

```tex
\cs_generate_variant:Nn\regex_count:nnN{nVN}
```

这样我们就可以使用

```tex
regex_count:nVN{A} \__answer_list_seq \__choice_amount_int
```

来得到选项 ```A``` 的数量了

现在来定义一个选项计数的命令

```tex
\NewDocumentCommand{\choicescount}{}
{
  选项计数: ~A:\regex_count:nVN{A}\__answer_list_seq\__choice_amount_int
  \int_use:N \__choice_amount_int 
      \quad B:\regex_count:nVN{B}\__answer_list_seq\__choice_amount_int
  \int_use:N \__choice_amount_int 
      \quad C:\regex_count:nVN{C}\__answer_list_seq\__choice_amount_int
  \int_use:N \__choice_amount_int 
      \quad D:\regex_count:nVN{D}\__answer_list_seq\__choice_amount_int
  \int_use:N \__choice_amount_int
}
```

至此，这个问题已经被解决了

## 完整源码

```tex
\documentclass{ctexart}
\usepackage{geometry}
\geometry{margin=2cm}
\begin{document}
%%%%%%%%%%%%%%%%%% 定义\choice{}{}{}{}命令，如果你有，可以注释它 %%%%%%%%%%%%%%%%%%%%%
\input{choice.tex} %% 这部分代码在上文中
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
\ExplSyntaxOn
\seq_clear_new:N \__answer_list_seq
\int_zero_new:N \__choice_amount_int
\cs_generate_variant:Nn\regex_count:nnN{nVN}
\NewDocumentCommand{\choice}{ommmm}
{
  \IfNoValueTF{#1}
    {\seq_put_right:Nn\__answer_list_seq{\space
    {\seq_put_right:Nn\__answer_list_seq{#1}}
  \choice{#2}{#3}{#4}{#5}
}
\NewDocumentCommand{\answerlists}{}
{
  答案列表:~\int_step_inline:nnn{1}{\seq_count:N \__answer_list_seq}
         {##1.\seq_item:Nn\__answer_list_seq{##1}~}
}
\NewDocumentCommand{\choicescount}{}
{
  选项计数: ~A:\regex_count:nVN{A}\__answer_list_seq\__choice_amount_int
  \int_use:N \__choice_amount_int 
      \quad B:\regex_count:nVN{B}\__answer_list_seq\__choice_amount_int
  \int_use:N \__choice_amount_int 
      \quad C:\regex_count:nVN{C}\__answer_list_seq\__choice_amount_int
  \int_use:N \__choice_amount_int 
      \quad D:\regex_count:nVN{D}\__answer_list_seq\__choice_amount_int
  \int_use:N \__choice_amount_int
}
\choice[A]{A}{B}{C}{D}
\choice[B]{A}{B}{C}{D}
\choice[C]{A}{B}{C}{D}
\choice[A]{A}{B}{C}{D}
\choice[C]{A}{B}{C}{D}
\choice[D]{A}{B}{C}{D}
\choice[B]{A}{B}{C}{D}
\choice[A]{A}{B}{C}{D}
\choice[D]{A}{B}{C}{D}
\choice[B]{A}{B}{C}{D}
\par 
\vspace*{2cm}
\answerlists
\par 
\choicescount
\ExplSyntaxOff
\end{document}
```

## 输出结果

![](https://ljguo-1308058910.cos.ap-nanjing.myqcloud.com//img/QQ截图20220217130337.png)

## 样式加强

我们还可以修改一下输出样式

效果图如下

![](https://ljguo-1308058910.cos.ap-nanjing.myqcloud.com//img/QQ截图20220217132609.png)

我们修改一下 ```\answerlists``` 命令

```tex
\NewDocumentCommand{\answerlists}{s}
{
  \IfBooleanTF{#1}
  {
    \int_step_inline:nn{\seq_count:N \__answer_list_seq}
    {\seq_put_right:Nn \__answer_amount_seq{##1}}
    \begin{tabularx}{\seq_count:N \__answer_list_seq cm}
      {|*{\seq_count:N \__answer_list_seq}{>{\centering\arraybackslash}X|}}
      \hline
      \seq_use:Nn \__answer_amount_seq{&}\\ \hline
      \seq_use:Nn \__answer_list_seq{&}\\ \hline
    \end{tabularx}
  }
  {
    答案列表:~\int_step_inline:nnn{1}{\seq_count:N \__answer_list_seq}
    {##1.\seq_item:Nn\__answer_list_seq{##1}~}
  }
}
```

可以发现增加了

```tex
\int_step_inline:nn{\seq_count:N \__answer_list_seq}
{\seq_put_right:Nn \__answer_amount_seq{##1}}
\begin{tabularx}{\seq_count:N \__answer_list_seq cm}
  {|*{\seq_count:N \__answer_list_seq}{>{\centering\arraybackslash}X|}}
  \hline
  \seq_use:Nn \__answer_amount_seq{&}\\ \hline
  \seq_use:Nn \__answer_list_seq{&}  \\ \hline
\end{tabularx}
```

观察前两行代码，这里是通过 ```\int_step_inline:nn``` 构建了一个序列，这个序列```\__answer_amount_seq``` 的值为```1234 ... \seq_count:N \__answer_list_seq``` ，随后使用 ```tabularx``` 环境来排版这个表格，同时需要 ```\usepackage{tabularx}``` 宏包的支持

然后就是 ```\seq_use:Nn``` 这个函数，是用来将序列的值通过某个 **token** 连接起来，例如，此时 ```\__answer_list_seq``` 的值为 ```ABCDAC``` ，那么使用 ```\seq_use:Nn \__answer_list_seq{&}``` 后，将被扩展为

> A & B & C & D & A & C   

同时，我们也修改 ```\choicescount``` 命令

```tex
\NewDocumentCommand{\choicescount}{s}
{
  \IfBooleanTF{#1}
  {
    \begin{tabular}{|c|c|c|c|c|}
      \hline
      选项 & A & B & C & D \\\hline
      计数 
      & \regex_count:nVN{A}\__answer_list_seq\__choice_amount_int
        \int_use:N \__choice_amount_int 
      & \regex_count:nVN{B}\__answer_list_seq\__choice_amount_int
        \int_use:N \__choice_amount_int
      & \regex_count:nVN{C}\__answer_list_seq\__choice_amount_int
        \int_use:N \__choice_amount_int
      & \regex_count:nVN{D}\__answer_list_seq\__choice_amount_int
        \int_use:N \__choice_amount_int \\ \hline
    \end{tabular}
  }
  {
    选项计数: ~A:\regex_count:nVN{A}\__answer_list_seq\__choice_amount_int
  \int_use:N \__choice_amount_int 
      \quad B:\regex_count:nVN{B}\__answer_list_seq\__choice_amount_int
  \int_use:N \__choice_amount_int 
      \quad C:\regex_count:nVN{C}\__answer_list_seq\__choice_amount_int
  \int_use:N \__choice_amount_int 
      \quad D:\regex_count:nVN{D}\__answer_list_seq\__choice_amount_int
  \int_use:N \__choice_amount_int
  }
}
```

关于 ```\NewDocumentCommand``` 中的参数规范 ```s``` 可自行查阅 ```xparse``` 宏包用户说明文档

通过前面的工作我们构建了四个命令，分别是

- \answerlists

- \answerlists*

- \choicescount

- \choicescount*

带星号的命令用于输出表格形式

## 加强完整源码

```tex
\documentclass{ctexart}
\usepackage{geometry,tabularx}
\geometry{margin=2cm}
\begin{document}
\input{choice.tex}
\ExplSyntaxOn
\seq_clear_new:N \__answer_list_seq
\seq_clear_new:N \__answer_amount_seq
\int_zero_new:N \__choice_amount_int
\cs_generate_variant:Nn\regex_count:nnN{nVN}

\NewDocumentCommand{\choices}{ommmm}
{
  \IfNoValueTF{#1}{\seq_put_right:Nn\__answer_list_seq{\space}}{\seq_put_right:Nn\__answer_list_seq{#1}}
  \choice{#2}{#3}{#4}{#5}
}
\NewDocumentCommand{\answerlist}{s}
{
  \IfBooleanTF{#1}
  {
    \int_step_inline:nn{\seq_count:N \__answer_list_seq}
    {\seq_put_right:Nn \__answer_amount_seq{##1}}
    \begin{tabularx}{\seq_count:N \__answer_list_seq cm}
      {|*{\seq_count:N \__answer_list_seq}{>{\centering\arraybackslash}X|}}
      \hline
      \seq_use:Nn \__answer_amount_seq{&}\\ \hline
      \seq_use:Nn \__answer_list_seq{&}\\ \hline
    \end{tabularx}
  }
  {
    答案列表:~\int_step_inline:nnn{1}{\seq_count:N \__answer_list_seq}
    {##1.\seq_item:Nn\__answer_list_seq{##1}~}
  }
}
\NewDocumentCommand{\choicecount}{s}
{
  \IfBooleanTF{#1}
  {
    \begin{tabular}{|c|c|c|c|c|}
      \hline
      选项 & A & B & C & D \\\hline
      计数 
      & \regex_count:nVN{A}\__answer_list_seq\__choice_amount_int
        \int_use:N \__choice_amount_int 
      & \regex_count:nVN{B}\__answer_list_seq\__choice_amount_int
        \int_use:N \__choice_amount_int
      & \regex_count:nVN{C}\__answer_list_seq\__choice_amount_int
        \int_use:N \__choice_amount_int
      & \regex_count:nVN{D}\__answer_list_seq\__choice_amount_int
        \int_use:N \__choice_amount_int \\ \hline
    \end{tabular}
  }
  {
    选项计数: ~A:\regex_count:nVN{A}\__answer_list_seq\__choice_amount_int
  \int_use:N \__choice_amount_int 
      \quad B:\regex_count:nVN{B}\__answer_list_seq\__choice_amount_int
  \int_use:N \__choice_amount_int 
      \quad C:\regex_count:nVN{C}\__answer_list_seq\__choice_amount_int
  \int_use:N \__choice_amount_int 
      \quad D:\regex_count:nVN{D}\__answer_list_seq\__choice_amount_int
  \int_use:N \__choice_amount_int
  }
}

\choices[A]{A}{B}{C}{D}
\choices[B]{A}{B}{C}{D}
\choices[C]{A}{B}{C}{D}
\choices[A]{A}{B}{C}{D}
\choices[C]{A}{B}{C}{D}
\choices[D]{A}{B}{C}{D}
\choices[B]{A}{B}{C}{D}
\choices[A]{A}{B}{C}{D}
\choices[D]{A}{B}{C}{D}
\choices[B]{A}{B}{C}{D}
\par 
\vspace*{2cm}
\centering
\answerlist*
\par 
\choicecount*
\ExplSyntaxOff

\end{document}
```

## 输出结果

![](https://raw.githubusercontent.com/ljguo1020/Images/main/img/QQ%E6%88%AA%E5%9B%BE20220217132609.png)

---

好了，就到这儿叭，我们下期再见啦 ~