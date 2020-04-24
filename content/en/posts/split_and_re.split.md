---
title: "Python skill -- split and re.split"
date: 2020-04-20T16:00:00+08:00
description: "Python skill -- split and re.split"
draft: false
hideToc: false
enableToc: true
enableTocContent: false
author: Stanley
authorEmoji: 🧐
tags:
- python
- string
- split
- RegExp
series:
- python skill
categories:
- python
image: images/feature3/code-file.png
---

### split()
<br/>

#### 用法介紹
<br/>

str.split(_sep=None, maxsplit=-1_)

可以指定分隔符號 **sep** ，將字串進行分割。  
**maxsplit** 用來指定分割的次數。所以總共會得到 **maxsplit + 1** 個字串。

* **sep:** 不指定，會用空格來區分。
* **maxsplit:** -1 表示不指定，會將遇到的分隔符號全部分割。
* split()只能分割一種分隔符號。
<br/><br/>

#### 範例
<br/>

```Python
str = "This is a dog, and that is a pig."
print(str.split())
print(str.split('a', 1)) # separate by 'a' at one time
print(str.split('z'))    # there is no z char.
```
<br/><br/>
輸出就會變成
<br/><br/>

```Python
['This', 'is', 'a', 'dog,', 'and', 'that', 'is', 'a', 'pig.']  
['This is ', ' dog, and that is a pig.']  
['This is a dog, and that is a pig.']
```
<br/><br/><br/><br/><br/>

### re.split()
<br/>

re.split() 的好處是可以處理多個分隔符號，並且可以根據正則表示式來作將分隔符號作不同的配對。而在使用之前請先熟悉正則表示式

[Regular Expression Syntax](https://docs.python.org/3/library/re.html#regular-expression-syntax)

#### 用法介紹
<br/>

re.split(pattern, string, maxsplit=0, flags=0)

* **pattern:** 就是split()中的 _sep_，可以為單一字符，也可以為正則表示式。
* **string:** 欲分割的字符串。
* **maxsplit:** 分割的最大次數，與split()中不同。
  > **maxsplit < 0:** 表示不分割。  
  > **maxsplit = 0:** 全部分割。  
  > **maxsplit > 0:** 分割次數限制。
* **flags:** 用來修改pattern表示式的功能。假設今天想要有忽略大小寫的功能，就可以使用flags來修正。 ([re.IGNORECASE](https://docs.python.org/3/library/re.html#re.IGNORECASE))。
<br/><br/>

#### 範例
<br/>

```Python
import re

str = "Words, words, words."

# \W equal to "[^A-Za-z0-9_]"
# + indicates one or more occurrences of the preceding element.

re.split(r'\W+', str)    
re.split(r'\W+', str, 1)
re.split('[a-f]+', '0a3B9', flags=re.IGNORECASE)
```

<br/><br/>
輸出就會變成
<br/><br/>

```Python
['Words', 'words', 'words', '']  
['Words', 'words, words.']
['0', '3', '9']
```

其實re.split() 最重要的是在正則表示式，基本上弄懂正則表示式才能夠掌握re.split()。

文末附上正則表示式的參考資料
<br/>

[Regular_expression wiki](https://en.wikipedia.org/wiki/Regular_expression)  
[python - re](https://docs.python.org/3/library/re.html)  
[陳鍾誠的網站 - 正則表示式](http://ccckmit.wikidot.com/regularexpression)  
