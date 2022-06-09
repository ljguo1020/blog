---
title: 使用 Vscode 配置 LaTeX
date: 2022-02-25 19:55:56
tags:   
           - LaTeX
           - 安装
categories:
           - LaTeX
cover: /img/4.jpg
---

{% tip home %}

Vscode 是一款杰出的代码编辑器，不论从代码高亮，自动补全上来讲，都是相当不错的，本文将要介绍在 Vscode 中配置 LaTeX

{% endtip %}

{% note info no-icon %}

本文默认用户已经安装好了 texlive

{% endnote %}

## 下载 vscode

直接到官网下载最新版的 vscode ，当然官网需要科学上网，不然下载很慢，当然也有其他的法子，自行百度吧

{% note primary flat %}

我们需要记住 vscode 的安装路径，后面需要用到

{% endnote %}

### 安装插件 

- Chinese  , 用于汉化界面
- LaTeX Workshop , 必备插件

安装好插件我们重启一下 vscode ，点击左下角齿轮，打开设置，并打开配置文件 **settings.json** ,按下图操作

![](https://ljguo-1308058910.cos.ap-nanjing.myqcloud.com//img/1.png)

![](https://ljguo-1308058910.cos.ap-nanjing.myqcloud.com//img/2.png)

这样就打开了配置文件，接下来我们来修改配置文件，将下面代码粘贴到里面，如果你是新安装的用户，直接用下面的代码替换你原来的就行了

```json
{
    "latex-workshop.latex.autoBuild.run": "never",
    "latex-workshop.showContextMenu": true,
    "latex-workshop.intellisense.package.enabled": true,
    "latex-workshop.message.error.show": false,
    "latex-workshop.message.warning.show": false,
    "latex-workshop.latex.tools": [
        {
            "name": "xelatex",
            "command": "xelatex",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "%DOCFILE%"
            ]
        },
        {
            "name": "pdflatex",
            "command": "pdflatex",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "%DOCFILE%"
            ]
        },
        {
            "name": "latexmk",
            "command": "latexmk",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "-pdf",
                "-outdir=%OUTDIR%",
                "%DOCFILE%"
            ]
        },
        {
            "name": "bibtex",
            "command": "bibtex",
            "args": [
                "%DOCFILE%"
            ]
        }
    ],
    "latex-workshop.latex.recipes": [
        {
            "name": "XeLaTeX",
            "tools": [
                "xelatex"
            ]
        },
        {
            "name": "PDFLaTeX",
            "tools": [
                "pdflatex"
            ]
        },
        {
            "name": "BibTeX",
            "tools": [
                "bibtex"
            ]
        },
        {
            "name": "LaTeXmk",
            "tools": [
                "latexmk"
            ]
        },
        {
            "name": "xelatex -> bibtex -> xelatex*2",
            "tools": [
                "xelatex",
                "bibtex",
                "xelatex",
                "xelatex"
            ]
        },
        {
            "name": "pdflatex -> bibtex -> pdflatex*2",
            "tools": [
                "pdflatex",
                "bibtex",
                "pdflatex",
                "pdflatex"
            ]
        },
    ],
    "latex-workshop.latex.clean.fileTypes": [
        "*.aux",
        "*.bbl",
        "*.blg",
        "*.idx",
        "*.ind",
        "*.lof",
        "*.lot",
        "*.out",
        "*.toc",
        "*.acn",
        "*.acr",
        "*.alg",
        "*.glg",
        "*.glo",
        "*.gls",
        "*.ist",
        "*.fls",
        "*.log",
        "*.fdb_latexmk"
    ],
    "latex-workshop.latex.autoClean.run": "onFailed",
    "latex-workshop.latex.recipe.default": "lastUsed",
    "latex-workshop.view.pdf.internal.synctex.keybinding": "double-click",
}
```

这段代码已经足够您使用 vscode 来编译 LaTeX 了，下面我们讲一下如何使用外部阅读器来查看编译好的 pdf 文件

## 安装SumatraPdf

这里推荐 SumatraPdf 这个软件，到官网下载安装即可

{% note primary flat %}

同样我们需要记住 SumatraPdf 的安装路径，后面需要用到

{% endnote %}

随后将下面代码放到 **settings.json** 这份配置文件中，注意放的位置在最后一个花括号前面

```json
"latex-workshop.view.pdf.viewer": "external",
"latex-workshop.view.pdf.external.viewer.command":
       "D:/Code/SumatraPDF/SumatraPDF.exe",  //注意修改路径
"latex-workshop.view.pdf.external.viewer.args": [
        "%PDF%"
    ],
```

上面代码中已经标明需要修改路径，这个路径正是我们安装的 SumatraPdf 的路径

这样保存后即可通过外部阅读器来查看pdf了

## 配置反向搜索

反向搜索有利于我们快速定位到文档对应的源码位置，我们需要将下面代码加入到 **settings.json** 里面，同样也是最后一个花括号前面

```json
"latex-workshop.view.pdf.external.synctex.command":
    "D:/Code/SumatraPDF/SumatraPDF.exe",  //注意修改路径
    "latex-workshop.view.pdf.external.synctex.args": [
    "-forward-search",
    "%TEX%",
    "%LINE%",
    "%PDF%",
],
```

然后我们还需要在 SumatraPdf 软件中做一些配置，通过设置打开高级选项，会弹出一个配置文件，我们只需在配置文件最后加上如下代码

```json
InverseSearchCmdLine = "D:\Code\VS code\Microsoft VS Code\Code.exe" "D:\Code\VS code\Microsoft VS Code\resources\app\out\cli.js"  --ms-enable-electron-run-as-node -r -g "%f:%l"
EnableTeXEnhancements = true
```

注意上面代码中 ```D:\Code\VS code``` 是我的 vscode 的安装路径，请换成你的，然后就可以通过在 pdf 文档中双击跳转到 vscode 源码部分了

以上就是在 vscode 中配置 LaTeX 的全部过程，有时间我会将其写得更加详细一点，如果有问题欢迎评论区留言