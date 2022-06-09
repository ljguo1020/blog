---
title: LaTeX3--整数数组 
date: 2021-12-28 16:32:25
tags: 
     - LaTeX3
     - 教程
categories:
           - LaTeX
cover: /img/4.jpg
---
本文仅介绍 LaTeX3 整数数组模块

---
区别于其它语言的一点：数组首个元素编号是 1， 而不是 0.
## 构建数组
构建一个长度为 5 的数组 ```\l_array_int```
```tex
\intarray_new:Nn \l_array_int {5}
```
## 数组赋值
给数组第 3 个元素赋值为 5.
```tex
\intarray_gset:Nnn \l_array_int {3}{5}
```
## 整体赋值
可以不用提前声明数组长度
```tex
\intarray_const_from_clist:Nn \l_array_int {2,5,4,1,6}
```
## 获取数组长度
```tex
\intarray_const_from_clist:Nn \l_array_int {2,5,4,1,6,10,5}
\intarray_count:N \l_array_int %---> 7
```
## 使用数组
```tex
\intarray_item:Nn \l_array_int {3} %---> 4
```
```tex
\intarray_rand_item:N \l_array_int %随机输出一个元素
```
## 初始化数组
```tex
\intarray_gzero:N \l_array_int
```
## 其它
```tex
\intarray_show:N ...
\intarray_log:N ...
```

---

点赞，收藏都是我的动力鸭~~~