---
title: LaTeX3----函数篇
date: 2021-12-28 12:32:07
tags:
     - LaTeX3
     - 教程
categories:
           - LaTeX
cover: /img/one.jpg
---
本文只记录函数，也即控制序列(control sequences)的相关用法
## 与 LaTeX2e 的一些比较  
在原始的 LaTeX2e 中 并没有严格的区分变量和函数，或许在命名上有些区分，例如 ```\mycmd``` ```\myvar```,它们的定义方法都是一样的
```tex
\newcommand{\mycmd}[2]{#1 love #2}%\mycmd{I}{you}-->I love you
\newcommand{\myvar}{I love \LaTeX}%\myvar -->I love LaTeX 
```
都是用 ```\newcommand``` 来定义的，函数无非是多了几个参数，这意味着函数是可以接受参数的，而变量是不需要的.
更加古老的 ```\def``` 也是如此

## LaTeX3
在LaTeX3中创建一个函数的语法如下：
```tex
\cs_new:Npn \l_my_cmd:nn #1#2 {#1 ~ love ~ #2} 
\l_my_cmd:nn {I}{you} %-->I love you
```
这是在全局范围内创建一个函数，相当于 ```\newcommand``` ,所有如果这个函数已经被定义，将会报错，当然还有其它语法
```tex
\cs_set:Npn \l_my_cmd:nn #1#2 {#1 ~ love ~ #2} %局部创建函数
\cs_gset:Npn \l_my_cmd:nn #1#2 {#1 ~ love ~ #2} %全局创建函数
```
这两个函数也可以用于创建函数，但是区别于前面的是，如果这个函数存在，它会重新定义该函数，而不会报错，如果不存在，就创建.而这里的 ```set``` 和 ```gset``` 通过上面的注释，大概知道了，前者用于局部创建，后者用于全局.至于这里的 **Npn** **nn**，可以通过上一篇文章了解一下.

前面三个，还有一些情况，待我一一道来
```tex
\cs_set_nopar:Npn \l_my_cmda:nn #1#2{#1 ~ Do~not ~love ~ #2}
```
顾名思义 ```nopar``` 就是参数中不能加 ```\par``` 这个命令，也就是不能分段啦。
```tex
\cs_set_protected:Npn \l_my_cmd:nn #1#2 {#1 ~ love ~ #2}
```
同样，根据名字 ```protected``` 我们可以猜到，这是一个受保护的函数，也就是它不能被 **x** 型展开.
```tex
\cs_set_protected_nopar:Npn \l_my_cmd:nn #1#2 {#1 ~ love ~ #2}
```
这个，自己猜一下吧。

另外，以上只列举了 ```set``` ,当然 ```new```,```gset``` 也都有这些对应的情况，并且它们默认各自还提供了几种不同的<font color=magenta>**变体**</font>供大家使用，当然也可以自己创建一些变体，我们后面再讲。

大家再来看看
```tex
\cs_new:Nn \l_my_cmd:nn {#1 ~ love ~ #2 }
```
对比一下，是不是有点不一样 ```Npn``` $\to$ ```Nn```,少了个屁 (p),读过前面的文章应该知道，这个 p 其实就是 parameters (参数), 创建函数时可以将其省略 ，对应的花括号外部的 ```#1#2``` 也一并省去，系统会自动检测参数数量.

```tex
\cs_set_eq:NN \l_my_cmda:nn \l_my_cmdb:nn
```
这个函数的作用是复制函数定义，复制第二个函数的定义给第一个函数，在底层 TeX 中有 ```\let\cmda\cmdb``` 与之有类似或者一模一样(?)的功能, ```eq``` $\to$ ```equal``` ,当然 ```new``` , ```gset```
也有对应的函数.

```tex
\cs_undefine:N \l_my_cmd:nn
\cs_undefine:c {l_my_cmd:nn}
```
这个函数 ```undefine``` 顾名思义，就是删掉一个函数的定义。例如，我们前文定义了函数 ```\l_my_cmd:nn``` 现在我们使用以上其中一个命令 (两个命令是等效的，待会就会讲到), 然后再用该函数的话，系统就会报错 
```tex
! Undefined control sequence.
\l_my_cmd:nn
```
结果很显然了.

**注** . 这里我们第一次遭遇 ```c``` 变体, ```N``` 变体接受一个 **cs** (控制序列)，也就是一个带反斜杠的命令(?), 而 ```c``` 接受一个 (token list) 也就是一个带花括号的内容，然后会为花括号里面的内容自动加上一个 ```\``` , 对比上面的例子，不难理解.

还有一些函数如下，我将它们一起列举出来 (偷个懒)：
```tex
\cs_meaning:N \l_my_cmd:nn
\cs_meaning:c {l_my_cmd:nn}
\cs_show:N  ...
\cs_show:c {...}
\cs_log:N  ...
\cs_log:c {...}
```
这三个函数都是用于查看函数定义的，区别在于第一个是直接在 PDF 里面显示，第二个是在终端显示 (会停止运行) ，第三个是在日志中显示.

```tex
%first
\use:c {l_my_cmd:nn}
%second
\cs:w
l_my_cmd:nn
\cs_end
```
上面两个作用是一样的，前者将内容放在花括号里面，后者放在环境之间.都等价于直接使用 ```\l_my_cmd:nn``` .

```tex
\cs_to_str:N \l_my_cmd:nn
```
这个函数是将函数转化为字符串 (str) 输出是 ```˙my˙cmd:nn```，好吧，我也不知道它干嘛用的，希望有大佬能为我解释一下.

```tex
\use:n {abc}
\use:nn {abc}{{def}}
\use_i:nn {abc}{def}
```
上面三个函数得到的结果依次是：```abc```, ```abc{def}```, ```abc``` ,这个函数的作用就是直接将花括号里面的内容输出，应该注意第二个，你在 PDF 里面看到的应该是 ```abcdef``` 这是因为 ```{}``` 在PDF 里面是不显示的，当然我们可以做一个简单的测试来验证它
```tex
$a^\use:n{abc}$ 
$a^\use:n{{abc}}$
```
输出依次是 $a^{a}bc$, $a^{abc}$.
```tex
\cs_if_eq_p:NN \l_my_cmda:nn \l_my_cmdb:nn
\cs_if_eq:NNTF \l_my_cmda:nn \l_my_cmdb:nn {yse}{no}
```
第一个函数用于测试两个函数的定义是否相同，在 PDF 中的显示是 $\Delta$,$\Gamma$ ,这其实就是两个布尔值, 前者为真，后者为假. 第二个函数同样的作用，只不过跟上了 ```T```, ```F``` 变体，也就是说条件为真，执行 ```T```,条件为假，执行 ```F```.

```tex
\cs_if_exist_p:N \l_my_cmd:nn
\cs_if_exist_p:c {...}
\cs_if_exist:NTF ...
\cs_if_exist:cTF {...}
```
通过名字，可以知道，这是用来测试这个函数是否存在的，类比一下上面就知道了.(懒得敲了)

----
 函数篇大概就写这么多了吧，还有些函数我没看懂，或者暂时不知道它有啥用的，就先不写了,以后再更吧.

