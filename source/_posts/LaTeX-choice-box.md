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

typora-copy-images-to: upload
---

{% aplayer "演员" "薛之谦" "https://sharefs.ali.kugou.com/202202192103/eaf8f3fca3058aa07450e2065cd7690a/KGTX/CLTX001/219dcc6167ebf64eaa187414ca58e836.mp3 " "https://p3fx.kgimg.com/stdmusic/240/20160907/20160907192633928797.jpg"  %}

## 想法来源

今天下午在配置我的博客时，看到了这样一种效果

![](https://ljguo-1308058910.cos.ap-nanjing.myqcloud.com//img/LaTeX_choice_box1.png)

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
  -  style = ,  设置当前框样式

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

![](https://ljguo-1308058910.cos.ap-nanjing.myqcloud.com//img/LaTeX_choice_box2.png)

## 未完结

还有些功能没有实现，后面再更新吧 ～ 现在是凌晨 02:55 先睡了～

---

## 继续

我来继续更新啦~

{% note info simple %}

我打算用 ```l3coffin``` 来实现了，最近看了一下这个模块，真的是非常厉害！

{% endnote %}

- 首先我们需要对绘制的两种框代码做一点点修改

1. ```\__item_true_label:```  {% checkbox blue checked , 选择样式%}

```tex
\cs_new:Npn \__item_true_label:
{
  \draw_begin:
  \draw_baseline:n{0.5ex}
  \draw_path_rectangle:nn{0,0}{0.4cm,0.4cm}
  \color_fill:x{\__color_tl}
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
  \color_select:x{\__color_tl}
  \draw_path_use:n{stroke}
  \draw_end:
}
```

- 定义两个函数来输出 ```coffin``` 

1. ```\__item_label: ``` 

```tex
\cs_new_protected:Npn \__item_label: 
{
  \par
  \int_compare:nF{\__item_int = \c_zero_int}
  {
	\__item_content:
  }
  \int_incr:N \__item_int
  \vcoffin_set:Nnx \__label_coffin {1cm}{\__item_tl}
  \vcoffin_set:Nnw \__content_coffin {\textwidth - \__parsep_dim - 2cm}
}
```

2. ```\__item_content:``` 

```tex
\cs_new_protected:Npn \__item_content:
{
  \vcoffin_set_end:
  \coffin_join:NnnNnnnn \__label_coffin{T}{r}\__content_coffin{T}{l}{-10pt}{0pt}
  \coffin_typeset:Nnnnn \__label_coffin{B}{l}{\__parsep_dim}{\__vsep_dim}
  \par 
}
```

- 定义键值

```tex
\keys_define:nn{test}
{
  colorlist.clist_set:N = \__color_clist, % 用于设置颜色列表
  vsep.dim_set:N = \__vsep_dim, % 控制每个条目的垂直间距
  vsep.initial:n = 1pt, % 设置初始值
  parsep.dim_set:N = \__parsep_dim, % 控制每个条目整体水平的偏移量
  parsep.initial:n = \__parindent_dim, % 设置初始值
  color.str_set:N = \__color_str, % 用于设置单个颜色
  color.initial:n = black % 设置初始值
}
```

- 提前申明变量和定义函数变体

```tex
\coffin_new:N \__label_coffin
\coffin_new:N \__output_coffin
\dim_zero_new:N \__vsep_dim
\dim_zero_new:N \__parsep_dim
\dim_zero_new:N \__parindent_dim
\dim_set_eq:NN \__parindent_dim \parindent
\tl_new:N \__item_tl
\cs_generate_variant:Nn \vcoffin_set:Nnn{Nnx}
\int_zero_new:N \__item_int
\cs_generate_variant:Nn\color_fill:n{x}
\cs_generate_variant:Nn\color_select:n{x}
```

- ```test``` 环境

```tex
\NewDocumentEnvironment{test}{O{}}
{
  \group_begin: 
  \dim_set:Nn \parindent{0pt}
  \int_zero:N \__item_int        
  \keys_set:nn{test}{#1}
  \seq_set_from_clist:NN \__color_seq \__color_clist  
  \clist_if_empty:NTF \__color_clist
  {
	\tl_set:Nn\__color_tl
    {\__color_str}
  }
  {
	\tl_set:Nn\__color_tl {\clist_item:Nn\__color_clist{\__item_int}}
  }     
  \RenewDocumentCommand{\item}{s}
  {           
    \IfBooleanTF{##1}
    {
      \tl_gset:Nn \__item_tl
      {\__item_true_label:}
	}
	{
	  \tl_gset:Nn \__item_tl
	  {
		\__item_false_label:
	  } 
	}
	\__item_label:					
  }          
}
{
  \__item_output:
  \group_end:
}
```

这次就到这儿叭，后面在更新~

## 完整源码 

```tex
\documentclass{ctexart}
\usepackage{l3draw,expl3,xparse}
\ExplSyntaxOn

\coffin_new:N \__label_coffin
\coffin_new:N \__content_coffin
\dim_zero_new:N \__vsep_dim
\dim_zero_new:N \__parsep_dim
\dim_zero_new:N \__parindent_dim
\dim_set_eq:NN \__parindent_dim \parindent
\tl_new:N \__item_tl
\cs_generate_variant:Nn \vcoffin_set:Nnn{Nnx}
\int_zero_new:N \__item_int
\cs_generate_variant:Nn\color_fill:n{x}
\cs_generate_variant:Nn\color_select:n{x}

\cs_new:Npn \__item_true_label:
{
  \draw_begin:
  \draw_baseline:n{0.5ex}
  \draw_path_rectangle:nn{0,0}{0.4cm,0.4cm}
  \color_fill:x{\__color_tl}
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
  \color_select:x{\__color_tl}
  \draw_path_use:n{stroke}
  \draw_end:
}

\cs_new_protected:Npn \__item_label: 
{
  \par
  \int_compare:nF{\__item_int = \c_zero_int}
  {
	\__item_content:
  }
  \int_incr:N \__item_int
  \vcoffin_set:Nnx \__label_coffin {1cm}{\__item_tl}
  \vcoffin_set:Nnw \__content_coffin {\textwidth - \__parsep_dim - 2cm}
}

\cs_new_protected:Npn \__item_content:
{
  \vcoffin_set_end:
  \coffin_join:NnnNnnnn \__label_coffin{T}{r}\__content_coffin{T}{l}{-10pt}{0pt}
  \coffin_typeset:Nnnnn \__label_coffin{B}{l}{\__parsep_dim}{\__vsep_dim}
  \par 
}

\keys_define:nn{test}
{
  colorlist.clist_set:N = \__color_clist, % 用于设置颜色列表
  vsep.dim_set:N = \__vsep_dim, % 控制每个条目的垂直间距
  vsep.initial:n = 1pt, % 设置初始值
  parsep.dim_set:N = \__parsep_dim, % 控制每个条目整体水平的偏移量
  parsep.initial:n = \__parindent_dim, % 设置初始值
  color.str_set:N = \__color_str, % 用于设置单个颜色
  color.initial:n = black % 设置初始值
}

\NewDocumentEnvironment{test}{O{}}
{
  \group_begin: 
  \dim_set:Nn \parindent{0pt}
  \int_zero:N \__item_int        
  \keys_set:nn{test}{#1}
  \seq_set_from_clist:NN \__color_seq \__color_clist  
  \clist_if_empty:NTF \__color_clist
  {
	\tl_set:Nn\__color_tl
    {\__color_str}
  }
  {
	\tl_set:Nn\__color_tl {\clist_item:Nn\__color_clist{\__item_int}}
  }     
  \RenewDocumentCommand{\item}{sO{}}
  {           
    \IfBooleanTF{##1}
    {
      \tl_gset:Nn \__item_tl
      {\__item_true_label:}
	  }
	  {
	    \tl_gset:Nn \__item_tl
	    {
		  \__item_false_label:
	    } 
	  }	
	\__item_label:					
  }          
}
{
  \__item_content:
  \group_end:
}
\ExplSyntaxOff

\begin{document}
我喜欢？
\begin{test}[colorlist ={ blue,red,yellow,cyan,black},]
	\item 抽烟
	\item* 喝酒
	\item  蹦迪
	\item  赌博
	\item* 美女
\end{test}
   
\end{document}
```

## 测试

{% folding cyan, 测试一 %}

```tex
我喜欢？
\begin{test}[colorlist ={ blue,red,yellow,cyan,black},vsep = 10pt]
	\item 抽烟
	\item* 喝酒
	\item  蹦迪
	\item  赌博
	\item* 美女
\end{test}
```

![](https://ljguo-1308058910.cos.ap-nanjing.myqcloud.com//img/LaTeX_choice_box3.png)

{% endfolding %}

{% folding red, 测试二 %}

```tex
我喜欢？
\begin{test}[color = blue]
	\item 吃饭
	\item 睡觉
	\item* 玩手机
	\item* \LaTeX 
	\item* 编程
	\item* 算法
\end{test}
```

![](https://ljguo-1308058910.cos.ap-nanjing.myqcloud.com//img/LaTeX_choice_box4.png)

{% endfolding %}

---

{% tip bolt %}

如果懒得复制源码，可以到我的 GitHub 下载

{% link choic-box, https://github.com/ljguo1020/TeX/tree/main/choice-box %}

{% endtip %}

下次再见~
