---
title: "使用Hugo建立網站(MAC) (III)"
date: 2020-04-17T14:00:00+08:00
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
- mac
- github.io
- website
series:
- hugo_on_mac
categories:
- hugo
image: images/feature2/bam.png
---

### 前言
上一篇連結在此
[使用Hugo建立網站(MAC) (II)](https://stanley98745.github.io/posts/hugo_on_mac2/)

本文參考 [在github部署hugo靜態網站](https://medium.com/@chswei/%E5%9C%A8-github-%E9%83%A8%E7%BD%B2-hugo-%E9%9D%9C%E6%85%8B%E7%B6%B2%E7%AB%99-9c40682dfe40)

### 註冊github

首先先到[github](https://github.com/) 註冊帳號。  

你的username就是你未來的github.io前面的名稱。
如果你的username叫做ccc，那你之後的網址就會叫做
> https://ccc.github.io

### 在github上建立兩個repositories

在github上建立兩個repositories，_website-hugo_ 以及 **_username_**.github.io

_website-hugo_ 這個repository隨便取就好，你要叫blog, new-site, sofarsogood 都可以。

但是 **_username_**.github.io一定要跟自己的帳號一樣。
像我是stanley98745，建立時就要打上stanley98745.github.io，因為我之前犯蠢，想說可以改別的名字。結果不行!

> 對了別忘記在名稱那邊要打上github.io。

#### 建立public資料夾並連結到username.github.io
在blog資料夾底下，我們使用hugo指令來創建public的資料夾。
(就是一開始hugo new site _newsite_，你的 _newsite_ 資料夾底下)

```bash
hugo
```

對沒錯，就只有 hugo

> 以下帳號的地方使用username來替代，記得修改成你的帳號哦！

```bash
cd public
git init
git remote add origin https://github.com/username/username.github.io.git
git add .
git commit -m "Initial commit"
git push -u origin master
```

#### 將blog資料夾連結到github上的website-hugo

基本上只有修改成website-hugo，其他都大同小異。

```bash
cd ..
git init
git remote add origin https://github.com/username/website-hugo.github.io.git
git add .
git commit -m "Initial commit"
git push -u origin master
```
> 萬一在 ```git add .``` 出現warning，不要怕，別理他。

然後等待個幾分鐘，你的 **_https://username.github.io_** 就出現囉!

### 更新網站
假設你有新的貼文想要上傳，那進到你的 _newsite_ 資料夾底下，在終端機執行以下指令

```bash
hugo
cd public
git add .
git commit -m "NEW POST"
git push origin master
```

那麼這樣就可以發表新的文章囉!

使用Hugo建立網站(MAC)的系列文就到這邊啦!

Linux的架站方式應該跟MAC差不多，所以未來吃飽沒事有機會再使用windows來試試看囉!
