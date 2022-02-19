---
title: LaTeX3---prop (属性列表)
date: 2022-01-13 01:21:43
tags:
     - LaTeX3
     - 教程
categories:
           - LaTeX
cover: /img/5.jpg
---

>本文介绍LaTeX3的 prop 模块
> 简介：主要用来设置一些键值(keyval)，但是与l3keys模块又有区别



## 创建和初始化属性列表


### 新建一个属性列表

```tex
\prop_new:N \l_my_prop
\prop_new:c {l_my_prop}
```

### 初始化(清空内容)

```tex
\prop_clear:N \l_my_prop
\prop_clear:c ...
\prop_gclear:N ...
\prop_gclear:c ...
```

检测是否存在，如果存在就清空.如果不存在就创建，然后清空

```tex
\prop_clear_new:N \l_my_prop
\prop_clear_new:c 
\prop_gclear_new:N
\prop_gclear_new:c
```

## 赋值

### 直接赋值

```tex
\prop_set_from_keyval:Nn \l_my_prop
{
	key1 = val1,
	key2 = val2,
	key3 = val3,
	...
}
\prop_set_from_keyval:cn
\prop_gset_from_keyval:Nn
\prop_gset_from_keyval:cn
```

### 创建副本

这让 ```\l_myb_prop``` 和```\l_mya_prop``` 相等

```tex
\prop_set_eq:NN \l_mab_prop \l_mya_prop
```

### 常量

```tex
\prop_const_from_keyval:Nn\l_my_prop
{
	key1 = val1,
	key2 = val2,
	key3 = val3,
	...
}
```

### 追加(put)

直接追加

```tex
\prop_put:Nnn \l_my_prop {key4}{val4}
```

先检测键值是否存在，如果存在，则什么也不做，不存在就为其追加. 区别于前者的是，如果存在，前者会覆盖掉

```tex
\prop_put_if_new:Nnn \l_my_prop {key4}{val4}
```

### 结合

将后面两个属性列表组合起来，给第一个列表

```tex
\prop_concat:NNN \l_mya_prop \l_myb_prop \l_myc_prop
```

### 以键值形式追加

```tex
\prop_put_from_keyval:Nn \l_my_prop
{
⟨key1⟩ = ⟨value1⟩ ,
⟨key2⟩ = ⟨value2⟩ ,
...
}
```

## 使用

### 传递给一个变量

```tex
\prop_get:NnN \l_my_prop {key1} \l_my_tl
```

此时，```\l_my_tl```中保存的值就是```val1``` 

### 直接使用

```tex
\prop_item:Nn \l_my_prop {key1} % ---> val1
```

## 计算属性列表数量

```tex
\prop_count:N \l_my_prop % 返回一个整数
```

## 移除

``` tex
\prop_remove:Nn \l_my_prop {key2}
```

## 条件判断

### 检测是否存在

```tex
\prop_if_exist_p:N \l_my_prop
\prop_if_exist:NTF \l_my_prop {true code}{false code}
```

### 检测是否为空

```tex
\prop_if_empty_p:N \l_my_prop
\prop_if_empty:NTF \l_my_prop {true code}{false code}
```

### 检测某键值是否存在

```tex
\prop_if_in:NnTF \l_my_prop{key4}{true code}{false code}
```

## 循环

### 遍历

将```keys```和```vals```通过参数传递给函数,所以函数应当接受两个参数

```tex
\prop_map_function:NN \l_my_prop \my_cmd:nn
```

example

```tex
\cs_new:Nn \my_cmd:nn {#1~:~i~love~#2.\par}
\prop_new:N \l_my_prop
\prop_set_from_keyval:Nn \l_my_prop
{
   a = 杨幂,
   b = 迪丽热巴,
   c = 周冬雨
}
\prop_map_function:NN \l_my_prop \my_cmd:nn
```

这会得到

```
a : i love 杨幂.
b : i love 迪丽热巴.
c : i love 周冬雨.
```

```tex
\prop_map_inline:Nn \l_my_prop {code}
```

```code```中可以带两个参数，用于代表```key```和```val```

example

```tex
\prop_new:N \l_my_prop
\prop_set_from_keyval:Nn \l_my_prop
{
   a = 杨幂,
   b = 迪丽热巴,
   c = 周冬雨
}
\prop_map_inline:Nn \l_my_prop{#1~:~i~love~#2.\par} 
```

同样也得到上面的结果

### break

用于终止循环

```tex
\prop_map_break:
```

下面这个函数会在终止循环后再执行一些东西

```tex
\prop_map_break:n {code}
```

example

```tex
\prop_new:N \l_my_prop
\prop_set_from_keyval:Nn \l_my_prop
{
   a = 杨幂,
   b = 迪丽热巴,
   c = 周冬雨
}
\prop_map_inline:Nn \l_my_prop
{
 	\str_if_eq:nnTF{#1}{c}
 	{
 		\prop_map_break:
 	}
 	{
 		#1~:~i~love~#2.\par
 	}
} 
```

这将只得到

```tex
a : i love 杨幂.
b : i love 迪丽热巴.
```

---

该模块暂时更新到这里吧