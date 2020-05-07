---
title: "使用Hugo建立網站(MAC) (I)"
date: 2020-04-16T12:00:00+08:00
description: "Hugo建立網站教學"
draft: false
hideToc: false
enableToc: true
enableTocContent: false
author: Stanley
authorEmoji: 🎅
tags:
- hugo
- osx
series:
- hugo_on_mac
categories:
- hugo
image: images/feature2/bam.png
---

_OS: Mac OSX_

嘗試使用window10，但不知道為什麼localhost:1313就是不給看。
一氣之下拿起我的陳年Mac來架站。

### Homebrew & Hugo安裝 ：

若沒有安裝homebrew, 請先安裝homebrew  
升級homebrew，以及安裝git (optional)與hugo
```Bash
$ brew update & upgrade
$ brew install git
$ brew install hugo
```  


### 建立網站：

首先開啟終端機，並移動到目標路徑，然後利用hugo指令建立出一個叫 _blog_ 的資料夾，你也可以替換你自己想要的名稱。再來部落格主題，我這邊採用zzo的主題。如果你想要別的主題，可以到 [hugo themes](https://themes.gohugo.io/) 搜尋你想要的主題，但由於每個作者習慣不同，所以在後面設定config.toml時會有點不同。


```Bash
$ hugo new site blog
$ cd blog/themes
$ git clone themes website
$ git clone https://github.com/zzossig/hugo-theme-zzo.git
$ cp -r zzo/exampleSite/** ../
$ cd ..
```

> 以上如果你採用的是其他主題，那在 **_cp_** 那行，zzo就應該改成你主題的名字。

如果你是想要替換主題，就將 **_git clone_** 那行改成以下形式。
```bash
git submodule add https://github.com/zzossig/hugo-theme-zzo.git
```  

通常在發布網站之前，我們會使用localhost:1313來看自己做的網站有沒有問題。

```bash
hugo serve
```

然後在網頁上面打上 **_localhost:1313_** ，就可以看到你的成品了。

### 修改樣板：  

一般來說複製 **_exampleSite_** 資料夾中的所有檔案到blog資料夾底下就ok了。同時 **_config.toml_** 就是我們要修改的對象。但是zzo的主題，**_config.toml_** 是放在 **config** 資料夾底下，所以我們要進去 **config/_default** 資料夾底下修改 **_config.toml_**

因此我把 _zzo theme_ 的 **_config.toml_** 修改放在下一篇，這樣文章內容會比較一致。

其他主題則需要看一下作者github的 **_README_** ，千萬要多小心哦。

### 發表文章
在 **blog/content** 資料夾底下應該可以找到post資料夾，有的還會放在更深的一層，像是zzo的主題就放在 **content/en** 資料夾底下。如果想要發表文章，只需要

```bash
hugo new posts/your-post.md
```

大致上就是這樣子囉！


**下一篇連結在此**
[使用Hugo建立網站(MAC) (II)](https://stanley98745.github.io/posts/hugo_on_mac2/)
