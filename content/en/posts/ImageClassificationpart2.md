---
title: "CS231_n Image Classification - part2"
date: 2020-05-16T15:00:00+08:00
description: "CNN Learning"
draft: false
hideToc: false
enableToc: true
enableTocContent: false
author: Stanley
authorEmoji: 🏫
tags:
- cs231n
- Image Classification
- Linear Classification
- python
- Deep learning
series:
- cs231_n
- Deep Learning
categories:
- cs231_n
- CNN
- Deep Learning
libraries:
- katex
image: images/feature2/mathbook.png
---

由前一篇文章[**Part1**](https://stanley98745.github.io/posts/imageclassification-part1/)，我們發現**KNN**並無法有效地處理影像的問題。主要有兩個問題：

- classifier 必須_儲存_所有 training data，所以需要大量的儲存空間。
- 分類 test image 時必須與所有training images 做比對。

<br/>

本篇將介紹 **Linear Classification**，可以說是最基本的Neural Networks處理方法。**Linear Classification**主要包含兩大內容，**score function** 與 **loss function**。

<br/><br/>

### Score Function

**Score function** 主要是將 raw data 轉換為 **類別** 的分數。比如說我們有一張 32 x 32 的彩色影像，raw data 的資訊就會是 32 x 32 x 3 = 3072 pixels，而假設總共有十種類別(貓、車、飛機等等)。那Score function 就是把3072的維度映射到10維上 $f: R^{3072} \mapsto R^{10}$，比較簡單的映射方式就是線性映射：

$$ f(x_i, W, b) =  W x_i + b $$

- $x_i$： training image, [3072 x 1]
- $W$： weights，[10 x 3072]
- $b$： bias vector, [10 x 1]

我們可以透過調整 $ W $ , $ b $ 讓計算出來的分數能夠匹配ground truth label，直觀的想法就是正確的類別得分會比錯誤類別的得分還高。除此之外，訓練完模型之後，我們只要儲存 $ W $ , $ b $，就可以快速判別test image是否正確。簡單歸納其優點：

- 線性函數，能夠快速訓練模型。
- 測試集的data不需一一與訓練集進行比較，節省時間。

<br/>

> 所謂的 ground truth 指的就是有效的答案或是標準值。

<br/>

這個線性函數的capacity(_意指模型的fitting能力_)，主要是依照影像的 R G B色彩來分類，比如說船的影像，由於大部分的情形都在海上或有天空的背景，藍色的比例就會比較多。所以如果要判斷船的影像，**藍色的權重** 就必須比較大，紅與綠的權重就要比較小。以下是來自CS231_n的範例。

<br/>

<img src="/images/CS231_n/img_classification2/imagemap.jpg" class="center">

<br/>

這個範例總共分成3個class，red (cat)、 green (dog)、 blue (ship)，而這個weights表現得不好，因為將貓分類成狗了。

<br/><br/>

### 不同觀點來理解Score Function

其實根據以上的線性函數，就很好理解了。但是CS231_n課程為我們展示了三種不同觀點來理解Score Function。

- 代數觀點 (Algebraic Viewpoint)
- 視覺觀點 (Visual Viewpoint)
- 幾何觀點 (Geometric Viewpoint)

<br/><br/>

#### 代數觀點 (Algebraic Viewpoint)

<img src="/images/CS231_n/img_classification2/Algebraic.jpg" width="70%" class="center">

<br/>

每一個影像都是由不同class所組合而成的，權重的每一個row都是一種類別，我們將pixel矩陣對於權重做 inner product，得到的值進行比較，就可以分類出該影像了。

<br/>

#### 視覺觀點 (Visual Viewpoint)

<img src="/images/CS231_n/img_classification2/Visual.jpg" width="70%" class="center">

<br/>

影像的分類，其實就是看看這個影像跟某一個類別的影像是不是比較吻合，其實就是上一篇講的 **Nearest Neighbor** 的概念。看這個影像跟哪一個類別比較靠近。差別在於 **Nearest Neighbor** 是與每一個training image做比較，而 **Linear Classification** 中，我們只要跟該類別的一個影像做比對就可以了。

<br/>

#### 幾何觀點 (Geometric Viewpoint)

32 x 32 x 3 = 3072 pixels，表示輸入的每一張影像都會有3072維度，而如果有10個類別，也就是把3072維度映射至10維，但這樣難以理解。我們假設有一種方法可以讓3072維度的影像映射到2維平面上。

<img src="/images/CS231_n/img_classification2/Geometric.jpg" width="70%" class="center">

<br/>

所以$ W $ 、 $ b $可以想像成是每一個類別(對應到row)在二維平面上的直線，weight表示直線的傾斜程度，bias表示直線與原點的距離。如此一來就可以分割出許多範圍，用以區分不同類別的影像。

<br/>

### Linear Classifier 困難點

對於某些case來說，Linear Classifier並無法有效地去分類，像是Non-Linear或是inseparable。

<img src="/images/CS231_n/img_classification2/HardCase.jpg" width="70%" class="center">

<br/>

有些問題可以將data先映射到新的平面上，轉換成可分類的問題。
不過有些難以處理的，可能就要使用別的model來訓練了。

<br/><br/>

### 結尾

對於weight與bias，我們該如何去判斷其好壞的程度。再者，如何去調整出適當的weight與bias。這些問題，可以透過Loss Function來取決，並且藉由最小化Loss來得到適當的weight與bias。
