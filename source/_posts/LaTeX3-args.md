---
title: 在LaTeX中定义多参数命令
date: 2022-01-30 08:50:13
tags:
     - LaTeX3
     - 实例
categories:
           - LaTeX
cover: /img/1.jpg
---
> 本文将要利用 **xparse** 宏包和 **LaTeX3** 的 **l3keys** 模块来构建一个简单的多参数命令   


## 提出问题

在 **LaTeX** 中，一个命令最多接受 9 个参数，虽然在日常使用中远远够了，但还是有一些用户有这方面的需求，我们下面来举一个简单的例子

```tex
\newcommand{\rectangle}[4]
{
  \begin{tikzpicture}
    \draw[line width=#3,draw=#4](0,0) rectangle (#1,#2);
  \end{tikzpicture}
}
```

创建了一个画矩形的命令 ```\rectangle``` , 它接受 4 个参数分别是 ```\rectangle{<width>}{<height>}{<line width>}{<color>}``` 也即是 **宽、高、线宽、颜色** ，这样虽然是合法的，但是难免显得有些臃肿，如果参数再多几个，久而久之，参数顺序估计都忘记了.

## 讨论问题

首先，我们应该放弃前面的思维方式，然后使用一种更好的方法来解决它，观察如下代码

```tex
\rectangle
{
  width=2cm,
  height=3cm,
  linewidth=1pt,
  color=red,
  ...
}
```

这样，这个命令仅接受一个参数，但效果却比上面的好得多，我们不用去记住参数顺序，而且可以为每个参数设置一个默认值，当缺省时，就使用默认值. 以上就是所谓的 ```key = val ``` 键值对.

## 解决问题

### xparse 宏包简介

**xparse** 宏包是基于 **LaTeX3** 编写的一个更加强大的创建命令或环境的宏包，它提供了```\NewDocumentCommand``` 用于取代原 **LaTeX2e** 中的 ```\newcommand``` ,例如

```tex
\NewDocumentCommand{\foo}{m}{my name is #1.}
\foo{ljguo}
```

#### 部分参数介绍

* **m** : 标准必选参数

* **o**  : 标准可选参数 , 如果缺省，会返回一个 ```-NoValue-``` 标记
* **O**{default} : 同 **o** ,但可以带一个默认值
*  **s** :  带星号命令 

*  **v** : 抄录参数

#### 几个命令介绍

* ```\IfNoValueTF``` , ```\IfNoValueT``` , ```\IfNoValueF``` 用于判断所接受参数是否为  ```-NoValue-``` 标记

* ```\IfBooleanTF``` , ```\IfBooleanT``` , ```\IfBooleanF``` 用于判断所接受参数的逻辑值，主要用于 **s** 类参数

#### 使用样例

> ```tex
> \NewDocumentCommand{\foo}{m}{my name is #1.}
> \foo{ljguo}
> ```
>
> ```tex
> my name is ljguo.
> ```

> ```tex
> \NewDocumentCommand{\foo}{o}{my name is #1.}
> \foo
> \foo[ljguo]
> ```
>
> ```tex
> my name is -NoValue-.
> my name is ljguo.
> ```

> ```tex
> \NewDocumentCommand{\foo}{O{ljguo}}{my name is #1.}
> \foo
> \foo[latexer]
> ```
>
> ```tex
> my name is ljguo.
> my name is latexer.
> ```

> ```tex
> \NewDocumentCommand{\foo}{s}
> {
>   \IfBooleanTF{#1}
>   {i have star.}
>   {i dont have star.}
> }
> \foo
> \foo*
> ```
>
> ```tex
> i dont have star.
> i have star.
> ```

### l3keys 模块简介

主要用到两个函数

* ```\keys_define:nn``` : 用于设置键值对
* ```\keys_set:nn``` : 用于为键值对赋值
* 这两个函数的第一个参数接受模块名

注意：使用 **LaTeX3** 语法应当将该部分代码放在 ```\ExplSyntaxOn``` 和 ```\ExplSyntaxOff``` 之间

> ```tex
> \ExplSyntaxOn 
> \keys_define:nn{example} %为 example 模块设置键值对
> {
>   name.tl_set:N  = \l_name_tl,    %将 name 保存到一个 tl 变量中
>   name.default:n = ljguo,         %为 name 设置默认值
>   name.initial:n = ljguo,         %为 name 设置初始值
>   age.int_set:N  = \l_age_int,    %将 age 保存到一个 int 变量中
>   age.default:n  = 21,            %为 age 设置默认值
>   age.initial:n  = 21             %为 age 设置初始值
> }
> \NewDocumentCommand{\showinfo}{} % 构建一个命令，用于输出个人信息
> {
>   hello,~my~name~is~\tl_use:c {l_name_tl},~and~i~am~\int_use:c {l_age_int} ~ years~old.  
> }
> \showinfo
> \keys_set:nn{example} %为 example 模块的键值赋值
> {
>   name = latexer,
>   age  = 50
> }
> \showinfo
> \ExplSyntaxOff
> ```
>
> ```tex
> hello, my name is ljguo, and i am 21 years old.
> hello, my name is latexer, and i am 50 years old.
> ```

## 回到原问题

现在我们再来定义一个```\rectangle``` 命令

首先设置键值对，需要四个键值

> ```tex
> \ExplSyntaxOn
> \keys_define:nn{rectangle}
> {
>   width.dim_set:N        = \l_width_dim,
>   width.default:n        = 2cm,
>   width.initial:n        = 2cm,
>   height.dim_set:N       = \l_height_dim,
>   height.default:n       = 3cm,
>   height.initial:n       = 3cm,
>   linewidth.dim_set:N    = \l_linewidth_dim,
>   linewidth.default:n    = 1pt,
>   linewidth.initial:n    = 1pt,
>   color.tl_set:N         = \l_color_tl,
>   color.default:n        = cyan,
>   color.initial:n        =cyan
> }
> \NewDocumentCommand{\rectangle}{O{}}
> {
>   \group_begin: 
>   \keys_set:nn{rectangle}{#1}
>   \begin{tikzpicture}
>     \draw[line~width = \dim_use:N \l_linewidth_dim,draw = \tl_use:N     \l_color_tl](0,0)rectangle(\dim_use:N \l_width_dim,\dim_use:N \l_height_dim);
>   \end{tikzpicture}
>   \group_end:
> }
> \rectangle\par
> \rectangle
> [
>   width = 3cm,
>   height = 2cm,
>   color = red
> ]
> \ExplSyntaxOff
> ```
>
> ![结果](/home/ljguo/Pictures/Screenshots/截图_选择区域_20220130101538.png)

## 总结

**xparse** 宏包是一个非常优秀的宏包，值得大家去学习，另外，作者水平有限，如有错误或者疑问，欢迎联系我

* **QQ** : 1123203930
* **Email** : 1123203930@qq.com