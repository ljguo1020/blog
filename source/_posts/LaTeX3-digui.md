---
title: LaTeX3实例--函数递归
date: 2021-12-28 16:43:07
tags:
     - LaTeX3
     - 实例
categories:
           - LaTeX
cover: /img/2.jpg
---
使用LaTeX3实现简单的函数递归，并计算正整数的阶乘和斐波那契数列   

核心代码
```tex
\cs_set:Npn \froac:n #1{
\int_compare:nNnTF{#1}={0}{1}
{
  \fp_eval:n{(#1)*\froac:n{#1-1}}
}
}
\cs_set:Npn \fibon:n #1{
\int_compare:nNnTF{#1}={0}{0}
{
\int_compare:nNnTF{#1}={1}{1}
{
  \fp_eval:n{\fibon:n{#1-1}+\fibon:n{#1-2}}
}
}
}
```
输出接口
```tex
\NewDocumentCommand{\fact}{m}{
#1!~=~\froac:n{#1}\par
}
\NewDocumentCommand{\fibon}{m}{
F(#1)~=~\fibon:n{#1}\par
}
```
完整代码
```tex
\documentclass{ctexart}
\usepackage{mathtools,amsmath}
\usepackage[paperwidth=18cm,margin=3cm]{geometry}
\ExplSyntaxOn
\cs_set:Npn \froac:n #1{
\int_compare:nNnTF{#1}={0}{1}
{
  \fp_eval:n{(#1)*\froac:n{#1-1}}
}
}
\cs_set:Npn \fibon:n #1{
\int_compare:nNnTF{#1}={0}{0}
{
\int_compare:nNnTF{#1}={1}{1}
{
  \fp_eval:n{\fibon:n{#1-1}+\fibon:n{#1-2}}
}
}
}
\NewDocumentCommand{\fact}{m}{
#1!~=~\froac:n{#1}\par
}
\NewDocumentCommand{\fibon}{m}{
F(#1)~=~\fibon:n{#1}\par
}
\ExplSyntaxOff
\begin{document}
\title{\LaTeX3---使用递归}
\author{ljguo1020@gmail.com}
\maketitle
\ExplSyntaxOn
计算~$n!$~
\[
  n!= 
    \begin{dcases}
        1,&n=0\\
        n(n-1)!,&n\geq ~ 1
    \end{dcases}
\]\par
\int_step_function:nN{20}\fact
\newpage
计算斐波那契数列
\[
F(n)=
\begin{dcases}
    0,&n=0\\
    1,&n=1\\
    F(n-1)+F(n-2),&n\geq 2
\end{dcases}
\]\par
\int_step_function:nN{20}\fibon
\ExplSyntaxOff
\end{document}
```
输出
![Image](https://pic4.zhimg.com/80/v2-19c642f79782c8692c2672ea292ae4ae.png)
![Image](https://pic4.zhimg.com/80/v2-f11eb0d1d7f506ec54ef4c34cb05389c.png)

---

关注，点赞都是我的动力鸭~~~