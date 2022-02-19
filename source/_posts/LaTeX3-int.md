---
title: LaTeX3--整数
date: 2021-12-28 15:43:43
tags: 
     - LaTeX3
     - 教程
categories:
           - LaTeX
cover: /img/3.jpg
---
本文仅仅介绍 LaTeX3 的整数模块.

----
## 构建赋值、运算
首先，利用以下函数创建一个整数变量
```tex
\int_new:N \l_my_int
\int_new:c {l_my_int}
```
这样就创建了一个整数变量,下面我们来为其赋值：
```tex
\int_set:Nn \l_my_int {10} % --->10
\int_set:Nn \l_my_int {2+2*3} % ---->8
\int_gset:Nn \l_my_int {2+2*3+(2+1)*3} % --->17
```
```tex
\int_set_eq:NN \l_my_int_b \l_my_int_a
\int_gset_eq:NN \l_my_int_b \l_my_int_a
```
使用整数
```tex
\int_use:N \l_my_int
\int_use:c {...}
```
加(减)一个数
```tex
 % \int_set:Nn \l_my_int {3}
\int_add:Nn \l_my_int {10} 
\int_use:N \l_my_int % --->13
\int_sub:Nn \l_my_int {1+9}
\int_use:c {l_my_int} %--->3
```
自加(减) 1
```tex
\int_incr:N \l_my_int
\int_decr:N \l_my_int
```
运算
```tex
\int_eval:n {10+2*3+5} % --->21
```
一些数学函数
```tex
\int_sign:n {100} % --->1 ,符号函数
\int_abs:n {-10} % --->10 ,绝对值
\int_div_round:nn {3}{1+1} % --->2 ,除法，四舍五入
\int_div_truncate:nn {3}{2} % --->1,除法，向零取整
\int_max:nn {100}{45} % --->100,取较大的一个
\int_min:nn {100}{45} % --->45
\int_mod:nn {9}{5} % --->4 ,取余数
\int_zero:N \l_my_int % ---> ,将变量清零
```
## 测试、比较
测试变量是否存在
```tex
\int_if_exist_p:N \l_my_int %--->输出布尔值 
\int_if_exist:NTF \l_my_int {yes} {no} %--->条件语句 
```
比较
```tex
\int_compare_p:nNn {100} > {102} 
\int_compare:nNnTF {100} > {102} {yes} {no} % ---> no 
\int_compare_p:n {1<2<3} 
\int_compare:nTF {1<2<3} {yes} {no}
```
以上除了 ```>```, ```<```, 还有 ```=```, ```!=```, ```>=``` and ```<=``` .

case
```tex
\int_case:nn{10}
{
    {2+4}{aaa}
    {3+7}{bbb}
} % ---> bbb
\int_case:nnTF ...
```
测试奇偶
```tex 
\int_if_even_p:n {...}
\int_if_even:nTF {...} {...} {...}
\int_if_odd_p:n {...}
\int_if_odd:nTF {...} {...} {...}
```
## 循环
### do-until 循环
```tex
\int_do_until:nNnn {<intexpr1>} <relation> {<intexpr2>} {<code>}
```
这是 ```do-until``` 循环， 它会先执行一次 ```<code>``` 中的内容，然后循环，直到条件为真.

示例
```tex
\int_new:N \l_mya_int
\int_new:N \l_myb_int
\int_set:Nn \l_mya_int {1}
\int_set:Nn \l_myb_int {10}
\int_do_until:nNnn {\l_mya_int}>{\l_myb_int} 
{
    this~is~\int_use:c{l_mya_int}.
    \int_incr:N \l_mya_int \par
}
```
这将会输出  

![](https://pic4.zhimg.com/80/v2-989dd33fc68601292049d678ad7cd10c.png)

示例
```tex
\int_new:N \l_mya_int
\int_new:N \l_myb_int
\int_set:Nn \l_mya_int {1}
\int_set:Nn \l_myb_int {10}
\int_do_until:nNnn {\l_mya_int}<{\l_myb_int} 
{
    this~is~\int_use:c{l_mya_int}.
    \int_incr:N \l_mya_int \par
}
```
这将只得到 ```this is 1.``` 对比一下.

### do-while 循环
```tex
\int_do_while:nNnn {<intexpr1>} <relation> {<intexpr2>} {<code>}
```
与 ```do-until``` 循环类似，不过它是等到条件为假时结束循环

示例
```tex
\int_new:N \l_mya_int
\int_new:N \l_myb_int
\int_set:Nn \l_mya_int {1}
\int_set:Nn \l_myb_int {10}
\int_do_while:nNnn {\l_mya_int}<{\l_myb_int}
{
     this~is~\int_use:c{l_mya_int}.
     \int_incr:N \l_mya_int \par
}
```
这将会输出

![](https://pic4.zhimg.com/80/v2-987c1955b1c411066a8865d72deb3876.png)

类似的还有 ```until-do``` , ```while-do``` 循环，它们是直接测试条件，不会事先执行一次 ```<code>``` 中的内容.

```tex
\int_until_do:nNnn {⟨intexpr1⟩} ⟨relation⟩ {⟨intexpr2⟩} {⟨code⟩}

\int_while_do:nNnn {⟨intexpr1⟩} ⟨relation⟩ {⟨intexpr2⟩} {⟨code⟩}

\int_do_until:nn {⟨integer relation⟩} {⟨code⟩}

\int_do_while:nn {⟨integer relation⟩} {⟨code⟩}

\int_until_do:nn {⟨integer relation⟩} {⟨code⟩}

\int_while_do:nn {⟨integer relation⟩} {⟨code⟩}
```
自行测试.

### step (按步)循环

```tex
\int_step_function:nN {⟨final value⟩} ⟨function⟩
```
这个函数是接受 ```nN``` 变体，第一个接受一个整数，第二个接受一个函数，也即将整数作为参数传递给该函数.

示例
```tex
\cs_set:Nn \l_my_cmd:n {I~am~#1~years~old.\par }
\int_step_function:nN {6} \l_my_cmd:n
```
这将会得到

![](https://pic4.zhimg.com/80/v2-fe7ab951b6d3fd1e8da1f4f66b6123b6.png)

```tex
\cs_set:Nn \l_my_cmd:n {I~am~#1~years~old.\par }
\int_step_function:nnN {5}{8} \l_my_cmd:n
```
![](https://pic4.zhimg.com/80/v2-1fc200290f1a45abd0bb3d3c1f69305d.png)
```tex
\cs_set:Nn \l_my_cmd:n {I~am~#1~years~old.\par }
\int_step_function:nnnN {1}{2}{9} \l_my_cmd:n
```
![](https://pic4.zhimg.com/80/v2-513cab728a331219cadc428f6286fafb.png)

以上是按函数循环，下面还有两种循环
```tex
\int_step_inline:nn{5}{我有~#1~个苹果 \par}
```
得到

![](https://pic4.zhimg.com/80/v2-d8989096e5f266fc5278140fcdd7a640.png)

这是用 ```#1``` 作为参数传递给后面的内容，另外还有：
```tex
\int_step_inline:nnn{3}{7}{我有~#1~个苹果 \par}

\int_step_inline:nnnn{3}{2}{9}{我有~#1~个苹果 \par}

\int_step_variable:nNn ...
\int_step_variable:nnNn ...
\int_step_variable:nnnNn ...
```
自行测试一下.

## 转换

```tex
\int_to_arabic:n{123} % --->123
\int_to_alph:n{3} % --->c
\int_to_Alph:n{4} % --->D
```
```tex
\cs_new:Npn \int_to_alph:n #1
{
\int_to_symbols:nnn {#1} { 26 }
{
    { 1 } { a }
    { 2 } { b }
        ...
    { 26 } { z }
}
}
```
```tex
\int_to_bin:n{6} % --->110 ,二进制
\int_to_hex:n{78} % --->4e ,十六进制
\int_to_Hex:n{78} % --->4E ,十六进制
\int_to_oct:n{10} % --->12 ,八进制
\int_to_base:nn{9}{3} % --->100 ,三进制
\int_to_base:nn{9}{4} % --->21 ,四进制
\int_to_Base:nn{17}{18} % --->H ,十八进制
```
```tex
\int_to_roman:n {2} % ---> ii 
\int_to_Roman:n {2} % ---> II
```
```tex
\int_from_alph:n {f} % ---> 6
\int_from_bin:n ...
\int_from_hex:n ...
\int_from_oct:n ...
\int_from_roman:n ...
\int_from_base:nn ...
```
## 伪随机数
```tex
\int_rand:n {10} % 1~10 之间产生一个随机整数
\int_rand:nn {5}{8} % 5~8 之间产生一个随机整数
```
## 已定义的常量
```tex
\c_zero_int % --->0
\c_one_int % --->1
\c_max_int % ---> 2147483647 ,可存储的最大整数
\c_max_register_int % --->32767 ,整数寄存器的最大数量
\c_max_char_int % --->255 ,引擎完全支持的最大字符编码。
```

---
就到这儿吧，整数模块就告一段落了.