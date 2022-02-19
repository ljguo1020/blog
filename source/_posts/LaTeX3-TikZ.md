---
title: LaTeX3 和 TikZ 结合绘图
date: 2021-12-28 16:46:25
tags: 
     - LaTeX3
     - 实例
categories:
           - LaTeX
cover: /img/7.jpg
---
这是LaTeX3的使用样例

---
## 定义颜色
colors.tex
```tex
\definecolor{1}{RGB}{255,255,255}
\definecolor{2}{RGB}{255,20,147}
\definecolor{3}{RGB}{0,255,255}
\definecolor{4}{RGB}{255,20,147}
\definecolor{5}{RGB}{225,0,0}
\definecolor{6}{RGB}{225,255,0}
\definecolor{7}{RGB}{47,79,79}
\definecolor{8}{RGB}{25,25,112}
\definecolor{9}{RGB}{0,200,0}
\definecolor{10}{RGB}{124,252,0}
\definecolor{11}{RGB}{139,69,19}
\definecolor{12}{RGB}{250,128,114}
\definecolor{13}{RGB}{255,69,0}
\definecolor{14}{RGB}{153,50,204}
\definecolor{15}{RGB}{139,121,94}
\definecolor{16}{RGB}{205,193,197}
\definecolor{17}{RGB}{205,16,118}
\definecolor{18}{RGB}{238,154,0}
\definecolor{19}{RGB}{205,133,63}
\definecolor{20}{RGB}{139,0,139}
```
## 主文件
main.tex
```tex
\documentclass[margin=5cm]{standalone}
\usepackage{tikz}
\pagecolor{black}
\input{colors.tex}
\ExplSyntaxOn
\NewDocumentCommand{\helix}{ m m m}
{
    \draw[line~width=2pt,#2] #1+(0 \c_colon_str #3)--+(60 \c_colon_str #3)--+(120 \c_colon_str #3)--+(180 \c_colon_str #3)--+(240 \c_colon_str #3)--+(300 \c_colon_str #3)--cycle;
    \draw[line~width=2pt,#2] #1+(10 \c_colon_str #3)--+(70 \c_colon_str #3)--+(130 \c_colon_str #3)--+(190 \c_colon_str #3)--+(250 \c_colon_str #3)--+(310 \c_colon_str #3)--cycle;
    \draw[line~width=2pt,#2] #1+(20 \c_colon_str #3)--+(80 \c_colon_str #3)--+(140 \c_colon_str #3)--+(200 \c_colon_str #3)--+(260 \c_colon_str #3)--+(320 \c_colon_str #3)--cycle;
    \draw[line~width=2pt,#2] #1+(30 \c_colon_str #3)--+(90 \c_colon_str #3)--+(150 \c_colon_str #3)--+(210 \c_colon_str #3)--+(270 \c_colon_str #3)--+(330 \c_colon_str #3)--cycle;   
}
\ExplSyntaxOff
\begin{document}
\ExplSyntaxOn
\begin{tikzpicture}[x=0.1cm,y=0.1cm]  
    \int_step_variable:nnnNn{0}{10}{1080} \i 
        {   
            \helix{(\i \c_colon_str 2+1*\i)}{\int_rand:nn{1}{20}}{0.15*\i}        
        }
\end{tikzpicture}
\ExplSyntaxOff
\end{document}
```
输出
![Image](https://pic4.zhimg.com/80/v2-13e03941cfa2da229449c2128c569c25.jpg)
将函数解析式稍加修改
```tex
\documentclass[margin=10cm]{standalone}
\usepackage{tikz}
\pagecolor{black}
\input{colors.tex}
\ExplSyntaxOn
\NewDocumentCommand{\helix}{ m m m}
{
    \draw[line~width=2pt,#2] #1+(0 \c_colon_str #3)--+(60 \c_colon_str #3)--+(120 \c_colon_str #3)--+(180 \c_colon_str #3)--+(240 \c_colon_str #3)--+(300 \c_colon_str #3)--cycle;
    \draw[line~width=2pt,#2] #1+(10 \c_colon_str #3)--+(70 \c_colon_str #3)--+(130 \c_colon_str #3)--+(190 \c_colon_str #3)--+(250 \c_colon_str #3)--+(310 \c_colon_str #3)--cycle;
    \draw[line~width=2pt,#2] #1+(20 \c_colon_str #3)--+(80 \c_colon_str #3)--+(140 \c_colon_str #3)--+(200 \c_colon_str #3)--+(260 \c_colon_str #3)--+(320 \c_colon_str #3)--cycle;
    \draw[line~width=2pt,#2] #1+(30 \c_colon_str #3)--+(90 \c_colon_str #3)--+(150 \c_colon_str #3)--+(210 \c_colon_str #3)--+(270 \c_colon_str #3)--+(330 \c_colon_str #3)--cycle;   
}
\ExplSyntaxOff
\begin{document}
\ExplSyntaxOn
\begin{tikzpicture}[x=20cm,y=20cm]
    \int_step_variable:nnnNn{0}{10}{360} \i 
     {   
         \helix{(\i \c_colon_str {1-sin(\i)})}{\int_rand:nn{1}{20}}{0.15}        
     } 
\end{tikzpicture}
\ExplSyntaxOff
\end{document}
```
得到
![Image](https://pic4.zhimg.com/80/v2-bc27ca6062fe5896b4a2df6a671916df.jpg)

---

点赞，关注都是我的动力鸭~~~