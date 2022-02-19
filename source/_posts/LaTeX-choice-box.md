---
title: 实现一个复选列表效果
date: 2022-02-19 01:41:07
tags:
     - LaTeX3
     - 实例
categories:
           - LaTeX
copyright_url: https://ljguo1020.github.io/2022/02/19/LaTeX-choice-box/
cover: /img/16.jpg
---

## 想法来源

今天下午在配置我的博客时，看到了这样一种效果

<img src="https://raw.githubusercontent.com/ljguo1020/Images/main/img/LaTeX_choice_box1.png" style="zoom:67%;" />

于是想着用 LaTeX 来实现一下

### 需要实现的功能

- 定义一个环境，命名为 ```test ``` ,并接受一些键值
  - colorlist = {color1,color2,color3, ... }, 用于设置各个选择框的颜色
  - color = colorname, 用于接受一个颜色，指定下列所有选择框为同一颜色
  - scale =  fp ,用于对框大小进行缩放
  - style = ,用于设置框的样式
- (在```test```环境中)重新定义 ```\item``` 命令 和 ```\item*``` 命令，分别用于制定空框和选择框
- ```\item``` 可以接受一个可选参数，即 ```\item[]``` ```\item*[]``` ,这个可选参数可以设置键值
  -  {% del color = , 设置当前框颜色%}
  - style = ,  设置当前框样式

## 开始动手 

### 预定义变量

```tex
\int_zero_new:N \__item_int % 用于为 \item 命令设置计数器
```

### 使用 ```l3draw``` 来绘制两种框

1. ```\__item_true_label:```  {% checkbox blue checked , 选择样式%}

```tex
\cs_new:Npn \__item_true_label:
{
    \draw_begin:
    \draw_baseline:n{0.5ex}
    \draw_path_rectangle:nn{0,0}{0.4cm,0.4cm}
    \color_fill:x{\clist_item:Nn\__color_clist{\__item_int}}
    \draw_path_use:n{fill}
    \draw_scope_begin:
    \draw_cap_round:
    \draw_join_round:
    \color_select:n{white}
    \draw_path_moveto:n{0.1cm,0.21cm}
    \draw_path_lineto:n{0.18cm,0.08cm}
    \draw_path_lineto:n{0.32cm,0.32cm}
    \draw_linewidth:n{1pt}
    \draw_path_use:n{stroke}
    \draw_scope_end:
    \draw_end:
}
```

2. ```\__item_false_label:```  {% checkbox blue  , 未选择样式%}

```tex
\cs_new:Npn \__item_false_label:
{
    \draw_begin:
    \draw_baseline:n{0.5ex}
    \draw_linewidth:n{0.8pt}
    \draw_path_rectangle:nn{0,0}{0.4cm,0.4cm}
    \color_select:x{\clist_item:Nn\__color_clist{\__item_int}}
    \draw_path_use:n{stroke}
    \draw_end:
}
```

注意上面代码中有两个函数

```tex
\color_select:x
\color_fill:x
```

**expl3**  宏包为我们提供的是 ```n``` 变体，这里我们需要对它所接受的参数进行完全展开，于是我们定义一个 ```x``` 变体

```tex
\cs_generate_variant:Nn\color_fill:n{x}
\cs_generate_variant:Nn\color_select:n{x}
```

### 定义键值

```tex
\keys_define:nn{test}
{
  colorlist.clist_set:N = \__color_clist,   
}
```

### 定义 ```test``` 环境

```tex
\NewDocumentEnvironment{test}{m}
{
  \int_zero:N \__item_int        
  \keys_set:nn{test}{#1}
  \seq_set_from_clist:NN \__color_seq \__color_clist       
  \RenewDocumentCommand{\item}{sm}
  {
    \int_incr:N \__item_int
    \IfBooleanTF{##1}
    {
      \__item_true_label: \space ##2 \par
    }
    {
      \__item_false_label: \space ##2 \par
    }
    }        
}
```

至此，暂时告一段落～

## 完整源码

```tex
\documentclass{ctexart}
\usepackage{l3draw}
\begin{document}
    \ExplSyntaxOn
    \int_zero_new:N \__item_int
    \cs_generate_variant:Nn\color_fill:n{x}
    \cs_generate_variant:Nn\color_select:n{x}
    \cs_new:Npn \__item_true_label:
    {
      \draw_begin:
      \draw_baseline:n{0.5ex}
      \draw_path_rectangle:nn{0,0}{0.4cm,0.4cm}
      \color_fill:x{\clist_item:Nn\__color_clist{\__item_int}}
      \draw_path_use:n{fill}
      \draw_scope_begin:
      \draw_cap_round:
      \draw_join_round:
      \color_select:n{white}
      \draw_path_moveto:n{0.1cm,0.21cm}
      \draw_path_lineto:n{0.18cm,0.08cm}
      \draw_path_lineto:n{0.32cm,0.32cm}
      \draw_linewidth:n{1pt}
      \draw_path_use:n{stroke}
      \draw_scope_end:
      \draw_end:
    }
    \cs_new:Npn \__item_false_label:
    {
      \draw_begin:
      \draw_baseline:n{0.5ex}
      \draw_linewidth:n{0.8pt}
      \draw_path_rectangle:nn{0,0}{0.4cm,0.4cm}
      \color_select:x{\clist_item:Nn\__color_clist{\__item_int}}
      \draw_path_use:n{stroke}
      \draw_end:
    }
    \keys_define:nn{test}
    {
      colorlist.clist_set:N = \__color_clist,   
    }
    \NewDocumentEnvironment{test}{m}
    {
      \int_zero:N \__item_int        
      \keys_set:nn{test}{#1}
      \seq_set_from_clist:NN \__color_seq \__color_clist       
      \RenewDocumentCommand{\item}{sm}
      {
        \int_incr:N \__item_int
        \IfBooleanTF{##1}
        {
          \__item_true_label: \space ##2 \par
        }
        {
          \__item_false_label: \space ##2 \par
        }
     }        
    }
    {\vspace*{0.5cm}}
    \ExplSyntaxOff
{\LARGE\bfseries 我喜欢？}\par 
\begin{test}{colorlist={green,cyan,red,blue,red,yellow,red}}
  \item*{熬夜} 
  \item{玩游戏} 
  \item*{打羽毛球} 
  \item*{LaTeX}
  \item*{听歌}
  \item{学习}
  \item*{熬夜玩游戏}
\end{test} 
\end{document}
```

## 输出结果

![](https://raw.githubusercontent.com/ljguo1020/Images/main/img/LaTeX_choice_box2.png)

## 未完结

还有些功能没有实现，后面再更新吧 ～ 现在是凌晨 02:55 先睡了～
