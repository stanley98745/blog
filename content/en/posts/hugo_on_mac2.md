---
title: "使用Hugo建立網站(MAC) (II)"
date: 2020-04-16T14:00:00+08:00
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
[使用Hugo建立網站(MAC) (I)](https://stanley98745.github.io/posts/hugo_on_mac1/)

> _本篇是介紹zzo這個主題_
> [zzo hugo theme](https://themes.gohugo.io/hugo-theme-zzo/)
> [zzo github](https://github.com/zzossig/hugo-theme-zzo)

zzo這個主題我其實蠻喜歡的，簡潔明瞭。不但可以放自己的部落格文章、簡報，此外還可以設置集散區域，來秀你的作品集與程式碼。不過我也還在摸索zzo這個主題，包含googleAnalytics，或是他的[external libraries](https://zzodocs.netlify.app/docs/externallibrary/)。

那廢話不多說，我們馬上開始。

### Config ：

> 注意這邊不是在 _theme/zzo_ 資料夾裡面的 _config_ 資料夾喔!

首先我們到config資料夾底下，可以看到 _config/\_default_ 資料夾底下有五個檔案，分別是：
>config.toml
>languages.toml
>menus.en.toml
>menus.ko.toml
>params.toml

#### config.toml

我們擷取部分可能會修改的程式碼  

```toml
baseURL = "http://example.org"
title = "Hugo Zzo Theme"
theme = "zzo"
```

baseURL：就是你的網址，比如說是https://XXXX.github.io

title：似乎只是描述而已，真正的標籤頁title修改在 **_languages.toml_** 中

theme：基本上就是zzo

```toml
defaultContentLanguage = "en"
defaultContentLanguageInSubdir = true
hasCJKLanguage = true
```

defaultContentLanguage：
設定default的語言，我這邊是以en為主，不過好像可以改成zh

defaultContentLanguageInSubdir：
讓你的網址後面可以是 **_網站/en_**
像是https://XXXX.github.io/en 可以用來變換不同語言的設定。

**_hasCJKLanguage_**:
_Set “true” for Chinese/Japanese/Korean languages._
感覺是設定給中日韓語言架站的人使用的。不過我直接用en也可以打中文，所以不清楚差別到底在哪邊。

```toml
summaryLength = 70
buildFuture = true

copyright = "&copy;{year}, All Rights Reserved"
timeout = 10000
enableEmoji = true
paginate = 13
rssLimit = 100

enableGitInfo = false
googleAnalytics = ""
```  

summaryLength:
_The length of a post description on a list page._
但是我修改值之後卻發現沒什麼變化，可能還要再試試看。

buildFuture:
_if true, we can use future date for talks page_
talks page是用來放links的列表頁面(video, ppt, ...)，蠻像庫存頁面的樣子。當把這個值改成 **false**，就可以變成像是先打好文章，然後時間到他就會顯示了。如果是 **true**，表示可以先看到“未來”的文章。假設你預定明天要發，日期改成明天，那你用localhost來看網站的時候，使用 **false** 是無法看到的哦。

copyright：
也就是一般網頁最下方會出現的授權標語。

enableEmoji：
可以自由決定是否使用表情符號唷！

paginate：
決定首頁出現的文章數量。

googleAnalytics:
這部分還沒測試，但應該是將 **_UA-XXXX_** 貼上應該就可以了吧...？


#### languages.toml：
```toml
[en]
  title = "Hugo Zzo Theme"
  languageName = "English"
  weight = 1

[ko]
  title = "Hugo Zzo Theme"
  languageName = "한국어"
  weight = 2
```

title：出現在網頁標籤頁之後的標題，並不是在網頁內的標題哦！
![1](/images/hugo_on_mac/hugo_on_mac2.1.png)

基本上我是將 [ko] 後面的都刪除了。

#### menus.en.toml

```toml
[[main]]
  identifier = "about"
  name = "about"
  url = "about"
  weight = 1

[[main]]
  identifier = "archive"
  name = "archive"
  url = "archive"
  weight = 2

[[main]]
  identifier = "gallery"
  name = "gallery"
  url = "gallery"
  weight = 3

[[main]]
  parent = "gallery"
  name = "cartoon"
  url = "gallery/cartoon"

[[main]]
  parent = "gallery"
  name = "photo"
  url = "gallery/photo"

[[main]]
  identifier = "posts"
  name = "posts"
  url = "posts"
  weight = 4

[[main]]
  identifier = "notes"
  name = "notes"
  url = "notes"
  weight = 5
```

這個toml代表的是各個頁面的切換
![1](/images/hugo_on_mac/hugo_on_mac2.2.png)

而目前我只留下 **_about, archive, posts_** 而已，並且我修改weight讓他們按照順序。(不過應該是沒有關係啦!)

identifier：
**不要修改，是辨識content資料夾底下的東西。**

name：
可以修改，會顯示在頁面上，比如about可以修改成關於。

url：網址顯示的名稱。

#### params.toml

```toml
logoText = "Zzo" # Logo text that appears in the site navigation bar.
logoType = "short" # long, short -> short: squre shape includes logo text, long: rectangle shape not includes logo text
logo = true # Logo that appears in the site navigation bar.
description = "The Zzo theme for Hugo example site." # for SEO
custom_css = [] # custom_css = ["scss/custom.scss"] and then make file at root/assets/scss/custom.scss
custom_js = [] # custom_js = ["js/custom.js"] and then make file at root/assets/js/custom.js
useFaviconGenerator = false # https://www.favicon-generator.org/

themeOptions = ["dark", "light", "hacker", "solarized", "kimbie"] # select options for site color theme
notAllowedTypesInHome = ["contact", "talks", "about", "showcase"] # not allowed page types in home page. type can be set in front matter or default to folder name.
notAllowedTypesInHomeSidebar = ["about", "archive", "showcase"] # not allowed page types in home page sidebar(recent post titles).
notAllowedTypesInArchive = ["about", "talks", "showcase"] # not allowed page types in archive page

# header
homeHeaderType = "text" # text, img, slide

# navbar
enableThemeChange = true # site color theme

# body
enableBreadcrumb = true # breadcrumb for list, single page
enableSearch = true # site search with Fuse
enableSearchHighlight = true # when true, search keyword will be highlighted
enableGoToTop = true # scroll to top
enableWhoami = true # at the end of single page
summaryShape = "classic" # card, classic, compact
searchResultPosition = "main" # side, main
archiveGroupByDate = "2006" # "2006-01": group by month, "2006": group by year
archivePaginate = 13 # items per page
paginateWindow = 1 # setting it to 1 gives 7 buttons, 2 gives 9, etc. If set 1: [1 ... 4 5 6 ... 356] [1 2 3 4 5 ... 356] etc
talksPaginate = 8 # items per page
talksGroupByDate = "2006" # "2006-01": group by month, "2006": group by year

# whoami: usage - home page sidebar, single page bottom of post. all values can be empty
myname = "zzossig"
email = "zzossig@gmail.com"
whoami = "Web Developer"
bioImageUrl = "" # image url like http//... If not set, we find a avatar image in root/static/images/whoami/avatar.(png|jpg|svg)
useGravatar = false # we use this option highest priority
location = "Seoul, Korea"
organization = "Hugo"
link = "https://github.com/zzossig/hugo-theme-zzo"

# sidebar
enableBio = true # home page sidebar
enableSidebar = true # Set to false to create the full width of the content.
enableSidebarTags = true # if you want to use tags.
enableSidebarSeries = true
enableSidebarCategories = true
enableHomeSidebarTitles = true
enableListSidebarTitles = true
enableToc = true # single page table of contents, you can replace this param to toc(toc = true)
hideToc = false # Hide or Show toc
tocPosition = "inner" # inner, outer
enableTocSwitch = true # single page table of contents visibility switch
itemsPerCategory = 5 # maximum number of posts shown in the sidebar.
sidebarPosition = "right" # bio, profile component layout position

# footer
showPoweredBy = true # show footer text: Powered by Hugo and Zzo theme
showFeedLinks = true # RSS Feed
showSocialLinks = true # email, facebook, twitter ...
enableLangChange = true # show button at bottom left of footer.

# service
baiduAnalytics = "" # alternative of google analytics
enableBusuanzi = false # if set true, total page view, total unique visitors show up in the footer.
busuanziSiteUV = true # unique visitors (total number of visitors)
busuanziSitePV = true # site total page view count
busuanziPagePV = true # post view count

# rss
updatePeriod = "" # Possible values: 'hourly', 'daily', 'weekly', 'monthly', or 'yearly'.
updateFrequency = ""
fullContents = false

# comment
enableComment = true
disqus_shortname = ""
commento = false

[gitment]          # Gitment is a comment system based on GitHub issues. see https://github.com/imsun/gitment
  owner = ""              # Your GitHub ID
  repo = ""               # The repo to store comments
  clientId = ""           # Your client ID
  clientSecret = ""       # Your client secret

[utterances]       # https://utteranc.es/
  owner = ""              # Your GitHub ID
  repo = ""               # The repo to store comments

[gitalk]           # Gitalk is a comment system based on GitHub issues. see https://github.com/gitalk/gitalk
  owner = ""              # Your GitHub ID
  repo = ""               # The repo to store comments
  clientId = ""           # Your client ID
  clientSecret = ""       # Your client secret

# Valine.
# You can get your appid and appkey from https://leancloud.cn
# more info please open https://valine.js.org
[valine]
  enable = false
  appId = '你的appId'
  appKey = '你的appKey'
  notify = false  # mail notifier , https://github.com/xCss/Valine/wiki
  verify = false # Verification code
  avatar = 'mm'
  placeholder = '说点什么吧...'
  visitor = false

[changyan]
  changyanAppid = ""        # Changyan app id             # 畅言
  changyanAppkey = ""       # Changyan app key

[livere]
  livereUID = ""            # LiveRe UID                  # 来必力

# Isso: https://posativ.org/isso/
[isso]
  enable = false
  scriptSrc = "" # "https://isso.example.com/js/embed.min.js"
  dataAttrs = "" # "data-isso='https://isso.example.com' data-isso-require-author='true'"

[socialOptions] # if set, social icons will show up.
  email = "mailto:your@email.com"
  phone = ""
  facebook = "http://example.org/"
  twitter = "http://example.org/"
  github = "https://github.com/zzossig/hugo-theme-zzo"
  stack-overflow = ""
  instagram = ""
  google-plus = ""
  youtube = ""
  medium = ""
  tumblr = ""
  linkedin = ""
  pinterest = ""
  stack-exchange = ""
  telegram = ""
  steam = ""
  weibo = ""
  douban = ""
  csdn = ""
  gitlab = ""
  mastodon = ""
  jianshu = ""
  zhihu = ""
  signal = ""
  whatsapp = ""

[donationOptions]
  enable = false # if set, the donation button will show up on the single page.
  alipay = "" # Alipay QR Code image (example path: images/donation/alipay-qrcode.png) and put your file at root/static/images/donation/
  wechat = "" # Wechat pay QR Code image (example path: same as above)
  paypal = "" # Paypal URL
  patreon = "" # Patreon URL
  bitcoin = "" # example path: images/donation/bitcoin-code-image.png

[copyrightOptions]
  enableCopyrightLink = false # if set, you can add copyright link
  copyrightLink = ""
  copyrightLinkImage = ""
  copyrightLinkText = ""

# possible share name: ["facebook","twitter", "reddit", "linkedin", "tumblr", "weibo", "douban", "line"]
[[share]]
  name = "facebook"
[[share]]
  name = "twitter"
  username = ""
```

有一點多，我只講述我修改的部分。

logoText：
如圖，修改的是這個地方的標題。
![1](/images/hugo_on_mac/hugo_on_mac2.3.png)

description：
SEO的keyword

**接著是修改home page側邊欄的個人資料**
> myname = "zzossig"
> email = "zzossig@gmail.com"
> whoami = "Web Developer"
> bioImageUrl = ""
> useGravatar = false # we use this option highest priority
> location = "Seoul, Korea"
> organization = "Hugo"
> link = "https://github.com/zzossig/hugo-theme-zzo"

其中要比較注意的是 **_bioImageUrl_** ，會讀取 _static/images/whoami/avatar.jpg_ ，要不就是貼圖片網址，要不就是將 _avatar.jpg_ 改成你要的圖片。

enableLangChange：
因為我只用一種語言，所以我把這個取消。假設你有韓文跟英文的網頁，就可以用這個來替換，會出現一個按鈕在最下方的左邊。

enableBusuanzi：
total views與total vistors，會在page最下方顯示。

**[socialOptions]**
總之就是修改你的個人資料，會在側邊欄以及最下方顯示出來，可以放Facebook, Linkedin, Youtube, ...。

### Content

在 **_content_** 資料夾底下預設會有 _en_ 以及 _ko_ 兩個資料夾，我自己是把 _ko_ 資料夾刪除了，而 _en_ 資料夾底下只留了 _about, archive, posts_ ，可以根據你自己的需求，來做選擇。不過要提醒，可能要先理解 **_markdown_** 的語法，再來進行修改哦。

除此之外，還有一個檔案叫做 **_\_index.md_**，主要在修改網頁的主標題、副標題與背景顏色和大小，而我只修改 _title_ 跟 _subtitle_ ，可以看下面的圖來理解。

![1](/images/hugo_on_mac/hugo_on_mac2.4.png)


基本上大致會修改的地方就是這兩個資料夾了。下一篇我們將介紹如何把網站丟到github上並且發布。


下一篇連結在此
[使用Hugo建立網站(MAC) (III)](https://stanley98745.github.io/posts/hugo_on_mac3/)
