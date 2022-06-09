---
title: 实现一个控制答案显示的功能
date: 2022-02-22 03:08:41
tags:
     - LaTeX3
     - 实例
categories:
           - LaTeX
cover: /img/11.jpg
---

{% note info simple %}

有很多老师想要实现一个{% emp  师生两版 %}的效果。这是一个什么样的效果呢，其实就是教师版的需要显示答案、提示、标注、等等，学生版则不需要。有些老师可能会选择{% u 注释法%}，也即是先把答案等注释掉，生成学生版，然后再来取消注释，生成教师版，但这样未免太麻烦了，其实这个很简单，我们可以使用通过设置一个布尔值来控制答案显示与否

{% endnote %}

## 实现细节

{% tabs 实现细节 %}

<!-- tab 直接写在源文件中(不推荐)  @fab fa-apple-pay -->

- 先定义两个变量

```tex
\tl_clear_new:N \__answer_tl % 用于保存答案
\bool_new:N \__answer_bool % bool变量，用于控制答案显示与否
```

- 声明两个环境

```tex
\NewDocumentEnvironment{exercise}{+b}
{
  \noindent\textbf{\textcolor{magenta}{题目：}}\par 
 #1
}{}
\NewDocumentEnvironment{answer}{+b}
{
  \tl_set:Nn \__answer_tl
  {
    \noindent\textbf{\textcolor{blue}{答案：}}\par
    #1
  }
}
{
  \par 
  \bool_if:NT\__answer_bool{\tl_use:N \__answer_tl}
}
```

 其中```exercise``` ```answer``` 两个环境分别用于输出题目和答案，在上面，可以发现这样一行代码

 ```tex
\tl_set:Nn \__answer_tl
 {
   \noindent\textbf{\textcolor{blue}{答案：}}\par
   #1
 }
 ```

这是将答案的内容写进 ```\__answer_tl``` 这个变量里面，而

```tex
\bool_if:NT\__answer_bool{\tl_use:N \__answer_tl}
```

这行代码中 ```\bool_if:NT``` 函数用于判断 ```\__answer_bool``` 这个 bool 变量的真假，如果为真则执行

```tex
\tl_use:N \__answer_tl
```

上面这行代码的作用是使 ```\__answer_tl``` 变量的值显示出来，也就是把我们保存的答案显示出来

可见这里决定答案显示与否的就是这个 bool 变量的真假了，我们通过

```tex
\bool_set_true:N \__answer_bool
\bool_set_false:N \__answer_bool
```

可以将该 bool 变量的设置为{% emp 逻辑真 %}和{% emp 逻辑假  %}，我们来看一下完整源码

## 完整源码1

```tex
\documentclass{ctexart}
\usepackage{xcolor}
\ExplSyntaxOn
\tl_clear_new:N \__answer_tl % 用于保存答案
\bool_new:N \__answer_bool % bool变量，用于控制答案显示与否
\bool_set_true:N \__answer_bool % 将 \__answer_bool 设置为真，则显示答案，如果注释掉，则不显示
\NewDocumentEnvironment{exercise}{+b}
{
  \noindent\textbf{\textcolor{magenta}{题目：}}\par 
 #1
}{}
\NewDocumentEnvironment{answer}{+b}
{
  \tl_set:Nn \__answer_tl
  {
    \noindent\textbf{\textcolor{blue}{答案：}}\par
    #1
  }
}
{
  \par 
  \bool_if:NT\__answer_bool{\tl_use:N \__answer_tl}
}
\ExplSyntaxOff
\begin{document}
\begin{exercise}
这是题目.....
\begin{answer}
这是答案...
\end{answer}
\end{exercise}

\begin{exercise}
  这是题目.....
\begin{answer}
  这是答案...
\end{answer}
\end{exercise}
\end{document}
```

## 结果

![](https://ljguo-1308058910.cos.ap-nanjing.myqcloud.com//img/LaTeX_show_answer1.png)

如果将第六行代码注释则不会显示答案

<!-- endtab -->

<!-- tab 写进cls(推荐) @fas fa-bomb -->
这里我需要新建一个文件，命名为 ```mycls.cls``` ,内容如下

```tex
\NeedsTeXFormat{LaTeX2e}
\LoadClass{ctexart}
\ProvidesClass{mycls}
\RequirePackage{xcolor}
\ExplSyntaxOn
\tl_clear_new:N \__answer_tl
\bool_new:N \__answer_bool
\keys_define:nn{answer}
{
  show-answer.code:n = {\bool_set_true:N \__answer_bool}
}
\ProcessKeysOptions{answer}
\NewDocumentEnvironment{exercise}{+b}
{
  \noindent\textbf{\textcolor{magenta}{题目：}}\par 
 #1
}{}
\NewDocumentEnvironment{answer}{+b}
{
  \tl_set:Nn \__answer_tl
  {
    \noindent\textbf{\textcolor{blue}{答案：}}\par
    #1
  }
}
{
  \par 
  \bool_if:NT\__answer_bool{\tl_use:N \__answer_tl}
}
\ExplSyntaxOff
\endinput
```

只对部分代码进行解读，其它的参看隔壁

```tex
\keys_define:nn{answer}
{
  show-answer.code:n = {\bool_set_true:N \__answer_bool}
}
```

这里使用 ```l3keys``` 包提供的键值设置，定义了一个 ```show-answer``` 键，他的内容是将 bool 变量设置为逻辑真

```tex
\ProcessKeysOptions{answer}
```

这里使用 ```l3keys2e``` 包提供的 ```\ProcessKeysOptions``` 命令，它可以将所接受到的模块内的键值设置传递给我们的模板 ```class``` ,这两个包均已内嵌在 LaTeX 内核，无需显示加载，这样我们只需在源文件中

```tex
\documentclass[show-answer]{mycls}
%如果加了show-answer可选项，则显示答案，反之则不显示
\begin{document}
\begin{exercise}
这是题目.....
\begin{answer}
这是答案...
\end{answer}
\end{exercise}

\begin{exercise}
  这是题目.....
\begin{answer}
  这是答案...
\end{answer}
\end{exercise}
\end{document}
```

这里输出和前面一样

<!-- endtab -->
{% endtabs %}

至此，大功告成，现在是 4:10 该睡觉了~

如果有问题，欢迎下方评论~

{% note success simple %}

如果本文对您有帮助，您可以请我喝杯奶茶~

{% endnote %}

