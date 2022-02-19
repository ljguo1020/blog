---
title: Butterfly 主题的使用（自用）
date: 2022-02-18 16:31:23
tags:
    - 使用教程
categories:
           - 教程
copyright_url: https://ljguo1020.github.io/2022/02/18/test/
---

## 行内文本样式 text

```markdown
1. 带 {% u 下划线 %} 的文本
2. 带 {% emp 着重号 %} 的文本
3. 带 {% wavy 波浪线 %} 的文本
4. 带 {% del 删除线 %} 的文本
5. 键盘样式的文本 {% kbd command %} + {% kbd D %}
6. 密码样式的文本：{% psw 这里没有验证码 %}
```

得到：

1. 带 {% u 下划线 %} 的文本
2. 带 {% emp 着重号 %} 的文本
3. 带 {% wavy 波浪线 %} 的文本
4. 带 {% del 删除线 %} 的文本
5. 键盘样式的文本 {% kbd command %} + {% kbd D %}
6. 密码样式的文本：{% psw 这里没有验证码 %}

## 行内文本 span

```markdown
- 彩色文字
在一段话中方便插入各种颜色的标签，包括：{% span red, 红色 %}、{% span yellow, 黄色 %}、{% span green, 绿色 %}、{% span cyan, 青色 %}、{% span blue, 蓝色 %}、{% span gray, 灰色 %}。
- 超大号文字
文档「开始」页面中的标题部分就是超大号文字。
{% span center logo large, Volantis %}
{% span center small, A Wonderful Theme for Hexo %}
```

得到：

- 彩色文字
在一段话中方便插入各种颜色的标签，包括：{% span red, 红色 %}、{% span yellow, 黄色 %}、{% span green, 绿色 %}、{% span cyan, 青色 %}、{% span blue, 蓝色 %}、{% span gray, 灰色 %}。
- 超大号文字
文档「开始」页面中的标题部分就是超大号文字。
{% span center logo large, Volantis %}
{% span center small, A Wonderful Theme for Hexo %}

## 段落文本p

```markdown
- 彩色文字
在一段话中方便插入各种颜色的标签，包括：{% p red, 红色 %}、{% p yellow, 黄色 %}、{% p green, 绿色 %}、{% p cyan, 青色 %}、{% p blue, 蓝色 %}、{% p gray, 灰色 %}。
- 超大号文字
文档「开始」页面中的标题部分就是超大号文字。
{% p center logo large, Volantis %}
{% p center small, A Wonderful Theme for Hexo %}
```

得到：

- 彩色文字
  在一段话中方便插入各种颜色的标签，包括：{% p red, 红色 %}、{% p yellow, 黄色 %}、{% p green, 绿色 %}、{% p cyan, 青色 %}、{% p blue, 蓝色 %}、{% p gray, 灰色 %}。
- 超大号文字
  文档「开始」页面中的标题部分就是超大号文字。
  {% p center logo large, Volantis %}
  {% p center small, A Wonderful Theme for Hexo %}

## 引用 note

### 自带

#### simple

```markdown
{% note simple %}默认 提示块标签{% endnote %}

{% note default simple %}default 提示块标签{% endnote %}

{% note primary simple %}primary 提示块标签{% endnote %}

{% note success simple %}success 提示块标签{% endnote %}

{% note info simple %}info 提示块标签{% endnote %}

{% note warning simple %}warning 提示块标签{% endnote %}

{% note danger simple %}danger 提示块标签{% endnote %}
```

{% note simple %}默认 提示块标签{% endnote %}

{% note default simple %}default 提示块标签{% endnote %}

{% note primary simple %}primary 提示块标签{% endnote %}

{% note success simple %}success 提示块标签{% endnote %}

{% note info simple %}info 提示块标签{% endnote %}

{% note warning simple %}warning 提示块标签{% endnote %}

{% note danger simple %}danger 提示块标签{% endnote %}

#### modern

```markdown
{% note modern %}默认 提示块标签{% endnote %}

{% note default modern %}default 提示块标签{% endnote %}

{% note primary modern %}primary 提示块标签{% endnote %}

{% note success modern %}success 提示块标签{% endnote %}

{% note info modern %}info 提示块标签{% endnote %}

{% note warning modern %}warning 提示块标签{% endnote %}

{% note danger modern %}danger 提示块标签{% endnote %}
```

{% note modern %}默认 提示块标签{% endnote %}

{% note default modern %}default 提示块标签{% endnote %}

{% note primary modern %}primary 提示块标签{% endnote %}

{% note success modern %}success 提示块标签{% endnote %}

{% note info modern %}info 提示块标签{% endnote %}

{% note warning modern %}warning 提示块标签{% endnote %}

{% note danger modern %}danger 提示块标签{% endnote %}

#### flat

```markdown
{% note flat %}默认 提示块标签{% endnote %}

{% note default flat %}default 提示块标签{% endnote %}

{% note primary flat %}primary 提示块标签{% endnote %}

{% note success flat %}success 提示块标签{% endnote %}

{% note info flat %}info 提示块标签{% endnote %}

{% note warning flat %}warning 提示块标签{% endnote %}

{% note danger flat %}danger 提示块标签{% endnote %}
```

{% note flat %}默认 提示块标签{% endnote %}

{% note default flat %}default 提示块标签{% endnote %}

{% note primary flat %}primary 提示块标签{% endnote %}

{% note success flat %}success 提示块标签{% endnote %}

{% note info flat %}info 提示块标签{% endnote %}

{% note warning flat %}warning 提示块标签{% endnote %}

{% note danger flat %}danger 提示块标签{% endnote %}

#### disabled

```markdown
{% note disabled %}默认 提示块标签{% endnote %}

{% note default disabled %}default 提示块标签{% endnote %}

{% note primary disabled %}primary 提示块标签{% endnote %}

{% note success disabled %}success 提示块标签{% endnote %}

{% note info disabled %}info 提示块标签{% endnote %}

{% note warning disabled %}warning 提示块标签{% endnote %}

{% note danger disabled %}danger 提示块标签{% endnote %}
```

{% note disabled %}默认 提示块标签{% endnote %}

{% note default disabled %}default 提示块标签{% endnote %}

{% note primary disabled %}primary 提示块标签{% endnote %}

{% note success disabled %}success 提示块标签{% endnote %}

{% note info disabled %}info 提示块标签{% endnote %}

{% note warning disabled %}warning 提示块标签{% endnote %}

{% note danger disabled %}danger 提示块标签{% endnote %}

#### no-icon

```markdown
{% note no-icon %}默认 提示块标签{% endnote %}

{% note default no-icon %}default 提示块标签{% endnote %}

{% note primary no-icon %}primary 提示块标签{% endnote %}

{% note success no-icon %}success 提示块标签{% endnote %}

{% note info no-icon %}info 提示块标签{% endnote %}

{% note warning no-icon %}warning 提示块标签{% endnote %}

{% note danger no-icon %}danger 提示块标签{% endnote %}
```

{% note no-icon %}默认 提示块标签{% endnote %}

{% note default no-icon %}default 提示块标签{% endnote %}

{% note primary no-icon %}primary 提示块标签{% endnote %}

{% note success no-icon %}success 提示块标签{% endnote %}

{% note info no-icon %}info 提示块标签{% endnote %}

{% note warning no-icon %}warning 提示块标签{% endnote %}

{% note danger no-icon %}danger 提示块标签{% endnote %}

## 上标标签 tip

```markdown
{% tip %}默认情况{% endtip %}
{% tip success %}success{% endtip %}
{% tip error %}error{% endtip %}
{% tip warning %}warning{% endtip %}
{% tip bolt %}bolt{% endtip %}
{% tip ban %}ban{% endtip %}
{% tip home %}home{% endtip %}
{% tip sync %}sync{% endtip %}
{% tip cogs %}cogs{% endtip %}
{% tip key %}key{% endtip %}
{% tip bell %}bell{% endtip %}
{% tip fa-atom %}自定义font awesome图标{% endtip %}
```

{% tip %}默认情况{% endtip %}
{% tip success %}success{% endtip %}
{% tip error %}error{% endtip %}
{% tip warning %}warning{% endtip %}
{% tip bolt %}bolt{% endtip %}
{% tip ban %}ban{% endtip %}
{% tip home %}home{% endtip %}
{% tip sync %}sync{% endtip %}
{% tip cogs %}cogs{% endtip %}
{% tip key %}key{% endtip %}
{% tip bell %}bell{% endtip %}
{% tip fa-atom %}自定义font awesome图标{% endtip %}

## 动态标签 anima

- On DOM load（当页面加载时显示动画）

```markdown
{% tip warning faa-horizontal animated %}warning{% endtip %}
{% tip ban faa-flash animated %}ban{% endtip %}
```

{% tip warning faa-horizontal animated %}warning{% endtip %}
{% tip ban faa-flash animated %}ban{% endtip %}

- 调整动画速度

```markdown
{% tip warning faa-horizontal animated faa-fast %}warning{% endtip %}
{% tip ban faa-flash animated faa-slow %}ban{% endtip %}
```

{% tip warning faa-horizontal animated faa-fast %}warning{% endtip %}
{% tip ban faa-flash animated faa-slow %}ban{% endtip %}

- On hover（当鼠标悬停时显示动画）

```markdown
{% tip warning faa-horizontal animated-hover %}warning{% endtip %}
{% tip ban faa-flash animated-hover %}ban{% endtip %}
```

## 复选列表 checkbox

```markdown
{% checkbox 纯文本测试 %}
{% checkbox checked, 支持简单的 [markdown](https://guides.github.com/features/mastering-markdown/) 语法 %}
{% checkbox red, 支持自定义颜色 %}
{% checkbox green checked, 绿色 + 默认选中 %}
{% checkbox yellow checked, 黄色 + 默认选中 %}
{% checkbox cyan checked, 青色 + 默认选中 %}
{% checkbox blue checked, 蓝色 + 默认选中 %}
{% checkbox plus green checked, 增加 %}
{% checkbox minus yellow checked, 减少 %}
{% checkbox times red checked, 叉 %}
```

{% checkbox 纯文本测试 %}
{% checkbox checked, 支持简单的 [markdown](https://guides.github.com/features/mastering-markdown/) 语法 %}
{% checkbox red, 支持自定义颜色 %}
{% checkbox green checked, 绿色 + 默认选中 %}
{% checkbox yellow checked, 黄色 + 默认选中 %}
{% checkbox cyan checked, 青色 + 默认选中 %}
{% checkbox blue checked, 蓝色 + 默认选中 %}
{% checkbox plus green checked, 增加 %}
{% checkbox minus yellow checked, 减少 %}
{% checkbox times red checked, 叉 %}

## 单选列表 radio

```markdown
{% radio 纯文本测试 %}
{% radio checked, 支持简单的 [markdown](https://guides.github.com/features/mastering-markdown/) 语法 %}
{% radio red, 支持自定义颜色 %}
{% radio green, 绿色 %}
{% radio yellow, 黄色 %}
{% radio cyan, 青色 %}
{% radio blue, 蓝色 %}
```

{% radio 纯文本测试 %}
{% radio checked, 支持简单的 [markdown](https://guides.github.com/features/mastering-markdown/) 语法 %}
{% radio red, 支持自定义颜色 %}
{% radio green, 绿色 %}
{% radio yellow, 黄色 %}
{% radio cyan, 青色 %}
{% radio blue, 蓝色 %}

## 链接卡片 link

```markdown
{% link 芒果不盲的博客, https://ljguo1020.github.io,/img/logo.png %}
```

{% link 芒果不盲的博客,https://ljguo1020.github.io %}

## 音频 audio

```markdown
{% audio https://github.com/volantis-x/volantis-docs/releases/download/assets/Lumia1020.mp3 %}
```

{% audio https://github.com/volantis-x/volantis-docs/releases/download/assets/Lumia1020.mp3 %}

## 折叠框 folding

```markdown
{% folding 查看图片测试 %}

![](https://cdn.jsdelivr.net/gh/volantis-x/cdn-wallpaper/abstract/41F215B9-261F-48B4-80B5-4E86E165259E.jpeg)

{% endfolding %}

{% folding cyan open, 查看默认打开的折叠框 %}

这是一个默认打开的折叠框。

{% endfolding %}

{% folding green, 查看代码测试 %}
假装这里有代码块（代码块没法嵌套代码块）
{% endfolding %}

{% folding yellow, 查看列表测试 %}

- haha
- hehe

{% endfolding %}

{% folding red, 查看嵌套测试 %}

{% folding blue, 查看嵌套测试2 %}

{% folding 查看嵌套测试3 %}

hahaha <span><img src='https://cdn.jsdelivr.net/gh/volantis-x/cdn-emoji/tieba/%E6%BB%91%E7%A8%BD.png' style='height:24px'></span>

{% endfolding %}

{% endfolding %}

{% endfolding %}
```

{% folding 查看图片测试 %}

![](https://cdn.jsdelivr.net/gh/volantis-x/cdn-wallpaper/abstract/41F215B9-261F-48B4-80B5-4E86E165259E.jpeg)

{% endfolding %}

{% folding cyan open, 查看默认打开的折叠框 %}

这是一个默认打开的折叠框。

{% endfolding %}

{% folding green, 查看代码测试 %}
假装这里有代码块（代码块没法嵌套代码块）
{% endfolding %}

{% folding yellow, 查看列表测试 %}

- haha
- hehe

{% endfolding %}

{% folding red, 查看嵌套测试 %}

{% folding blue, 查看嵌套测试2 %}

{% folding 查看嵌套测试3 %}

hahaha <span><img src='https://cdn.jsdelivr.net/gh/volantis-x/cdn-emoji/tieba/%E6%BB%91%E7%A8%BD.png' style='height:24px'></span>

{% endfolding %}

{% endfolding %}

{% endfolding %}

## 分栏 tab

```markdown
{% tabs test1 %}
<!-- tab -->
**This is Tab 1.**
<!-- endtab -->

<!-- tab -->
**This is Tab 2.**
<!-- endtab -->

<!-- tab -->
**This is Tab 3.**
<!-- endtab -->
{% endtabs %}
```

{% tabs test1 %}
<!-- tab -->
**This is Tab 1.**
<!-- endtab -->

<!-- tab -->
**This is Tab 2.**
<!-- endtab -->

<!-- tab -->
**This is Tab 3.**
<!-- endtab -->
{% endtabs %}

```markdown
{% tabs test4 %}
<!-- tab 第一个Tab -->
**tab名字为第一个Tab**
<!-- endtab -->

<!-- tab @fab fa-apple-pay -->
**只有图标 没有Tab名字**
<!-- endtab -->

<!-- tab 炸弹@fas fa-bomb -->
**名字+icon**
<!-- endtab -->
{% endtabs %}
```

{% tabs test4 %}
<!-- tab 第一个Tab -->
**tab名字为第一个Tab**
<!-- endtab -->

<!-- tab @fab fa-apple-pay -->
**只有图标 没有Tab名字**
<!-- endtab -->

<!-- tab 炸弹@fas fa-bomb -->
**名字+icon**
<!-- endtab -->
{% endtabs %}

## 进度条

```markdown
{% progress 10 red 进度条样式预览 %}
{% progress 30 yellow 进度条样式预览 %}
{% progress 50 green 进度条样式预览 %}
{% progress 70 cyan 进度条样式预览 %}
{% progress 90 blue 进度条样式预览 %}
{% progress 100 gray 进度条样式预览 %}
```

{% progress 10 red 进度条样式预览 %}
{% progress 30 yellow 进度条样式预览 %}
{% progress 50 green 进度条样式预览 %}
{% progress 70 cyan 进度条样式预览 %}
{% progress 90 blue 进度条样式预览 %}
{% progress 100 gray 进度条样式预览 %}

## 注释 notation

```markdown
{% nota 把鼠标移动到我上面试试 ,可以看到注解内容出现在顶栏 %}
```

{% nota 把鼠标移动到我上面试试 ,可以看到注解内容出现在顶栏 %}
