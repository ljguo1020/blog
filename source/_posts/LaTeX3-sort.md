---
title: LaTeX3实例--冒泡排序
date: 2021-12-28 16:34:32
tags: 
     - LaTeX3
     - 实例
categories:
           - LaTeX
cover: /img/6.jpg
---
本文是 LaTeX3 的一个简单使用样例
---

根据 C 语言的冒泡排序，改写成 LaTeX3， 如下：

C (部分代码)

```c
for(i=0;i<4;i++)
{
    for(j=0;j<4-i;j++)
    {
        if(a[j]>a[j+1])
        {
            t=a[j];
            a[j]=a[j+1];
            a[j+1]=t;
        }
    }
}
```
LaTeX3

```tex
\documentclass[UTF8]{ctexart}
\ExplSyntaxOn
\int_new:N \l_t_int
\NewDocumentCommand{\sort}{m}
{

\intarray_const_from_clist:Nn \l_array_int {#1}                 
\int_step_variable:nNn{\intarray_count:N \l_array_int} \i 
{
    \int_step_variable:nNn{\intarray_count:N \l_array_int-\i} \j 
    {        
        \int_compare:nT {\intarray_item:Nn \l_array_int{\j} > \intarray_item:Nn\l_array_int{\j+1}}
        {  
        \int_set_eq:NN \l_t_int \intarray_item:Nn \l_array_int{\j}   
        \intarray_gset:Nnn  \l_array_int {\j}{\intarray_item:Nn \l_array_int{\j+1}}
        \intarray_gset:Nnn  \l_array_int {\j+1}{\l_t_int}  
        }
    }
}
这些整数的由小到大的排序为：\int_step_inline:nn{\intarray_count:N \l_array_int}{\intarray_item:Nn\l_array_int {##1}~}
\cs_undefine:N \l_array_int
}
\ExplSyntaxOff 
\begin{document}
\sort{15,4,12,5,45,5,6,15}\par 
\sort{12,5,4,5,6,78,100,105}\par 
\sort{42,0,-3,5,25,68}
\end{document}
```
输出
![Image](https://pic4.zhimg.com/80/v2-1f8c26052785eef792f0226c6c362c1b.png)
---
关注，点赞都是我的动力鸭~~~