---
title: "Precision and Recall"
date: 2020-05-09T18:00:00+08:00
description: "Deep Learning"
draft: false
hideToc: false
enableToc: true
enableTocContent: false
author: Stanley
authorEmoji: 🏫
tags:
- Deep learning
series:
- Deep Learning
categories:
- Deep Learning
libraries:
- katex
image: images/feature2/mathbook.png
---

### 前言

當我們訓練完模型，並且想要針對結果來鑑別模型的好壞，這時候我們可以透過一些指標來判定。而在深度學習中，最有名的就是 **Precision** 跟 **Recall** ，中文翻譯分別為精確率與召回率。

我們來講一下準確率(Accuracy)會遇到的盲點。假設你早就知道垃圾郵件佔的比例大概為 **4%** ，那你只要判斷垃圾郵件的模型設置為，_把全部郵件判斷為正常郵件_ ，準確率就會高達 **96%** 。但對於實務上來說，這個模型一點用處都沒有。

所以說準確率會遇到一些限制，這時候我們就要考慮 **Precision** 跟 **Recall** 。有一個常用的指標稱做是 **Confusion Matrix**，中文翻譯為混淆矩陣。

<br/><br/>

### 混淆矩陣

<br/>

||實際 (O)|實際 (X)|
|:---:|:---:|:---:|
|預測 (O)| True Positive | False Positive |
|預測 (X)| False Negative | True Negative |

<br/>

- True：預測與實際上**相符** 。
- False：預測與實際上**不相符** 。

- Positive：預測**正確**的情況。
- Negative：預測**不正確**的情況

<br/>

簡單來區分就是
**True/False** 表示預測正確與否。
**Positive/Negative** 不論預測結果是否正確，只是表示預測的情況，這邊我們用正向與反向來表示。

<br/><br/>

#### Precision & Recall

**Precision**就是求在預測(O)的情況下，實際也是(O)的比例，而**Recall**就是在實際(O)的情況下，預測為(O)的比例。

$$Precision = \frac {TP} {TP + FP}$$

$$Recall = \frac {TP} {TP + FN}$$

<br/>

舉個例子來說，假設你家門口安裝了智慧門鎖，需要用指紋才能進入。此時使用你的指紋進家門，就會是 **True Positive**。而你的好朋友小偉按壓他的指紋想要進你家，發現進不去，這個情況就會是 **True Negative**。

可是人算不如天算，你發現另一個好朋友小志，居然用他的指紋通過了你家的智慧門鎖，這類情況我們稱作是 **False Positive**，預測是YES但實際上是NO，又稱做是 **Type I Error**。

再來的一段時日，你交了個女朋友--小芳，你把小芳的指紋輸入了系統，這樣她來你家的時候就可以直接進來。結果她第一次使用智慧門鎖就被卡在外面，因為系統判定她不是能夠進入的人。這類情況稱作是 **False Negative**，預測是NO但實際上是YES，又稱做是 **Type II Error**。

<br/>

基本上我們希望模型能夠預測得越準越好，但事實就是不盡人意。所以我們需要根據應用場景，來取決哪一種Error是需要避免的。比如說廣告投放，我們希望寧可錯殺也不可以放過，此時就可以容許**Type I Error**比較高，**Type II Error**則需要比較低。而對於以上的門鎖狀況，則會是希望**Type II Error**比較高，**Type I Error**則比較低。

<br/>

所以**Precision**跟**Recall**差別只在分母，那麼實際上的意義代表什麼呢? **Precision**就是在預測正向的情況下，實際上的準確度是多少。而**Recall**表示的是在實際為正向的情況下，能夠存在、召回多少預測與實際相符的答案。

比如說以上的廣告投放，我們希望的是**Type II Error**比較低，也就是**False Negative**的數據要比較少，所以就是**Recall**要高。而**Type I Error** 高不高都沒有關係，也就是**Precision**對這個case來說不重要。

而在門鎖系統中**Type I Error**則希望比較低，也就是**False Positive**要低，所以**Precision**就很重要。而**Type II Error** 不太重要，也就是我們不太在意**Recall**。

<br/>

> **True Negative** 為何不考慮?

<br/>

因為在正常的命題之下，實際與預測正向的結果通常比負向的還少。可以想成你有100萬張圖片，要來判定是不是貓咪，那你一定會蒐集一大堆不是貓咪的照片，所以實際不是貓而且預測也不是貓的結果，就會非常非常多。因此**True Negative**的量絕對是最多的，而且用處也不大。

<br/><br/>

#### F Measure

如果我們現在覺得**Precision**跟**Recall**都很重要，該怎麼樣去統合這個標準呢?

<br/>

> 採用 **F Measure**

<br/>

而 **F Measure** 的公式為

$$F_\beta = (1+\beta ^2) \times \frac {Precision \times Recall} {(\beta ^2 \times Precision) + Recall}$$

<br/>

當 $\beta = 0$，就是 **Precision**

當 $\beta = \infty$，就是 **Recall**

如果今天希望**Precision**比重比較高，則 beta 要小一點。 希望**Recall**比重比較高，則 beta 要大一點。

<br/>

當 $\beta = 1$，就叫作 **F<sub>1</sub> Score**，表示**Precision**跟**Recall**重要程度相當。

<br/>

而**F<sub>1</sub> Score**所使用的平均方法為**調和平均**，大家可能會對這個平均方法有點疑惑，一般常用的不就是算術平均嗎? 為何現在要使用調和平均?

<br/><br/>

#### 調和平均

調和平均(Harmonic mean)，最經典的用法就是計算平均速率。**調和平均數是將所有數值取了倒數之後，計算其算術平均數，再將得到的數字取倒數。**

<br/>

相關應用有什麼呢?

還記得國高中學過的電阻並聯、縮減質量吧?

當兩個電阻 R<sub>1</sub> R<sub>2</sub> 並聯時，等效電阻就會是

$$\frac{1} {\frac{1} {R_1} + \frac{1} {R_2}}$$

<br/>

而縮減質量(Reduced Mass) 就是雙星運動的有效慣性質量，方程式為

$$\frac{1} {\frac{1} {m_1} + \frac{1} {m_2}}$$

<br/>

以上的並聯等效電阻以及縮減質量，恰好都是調和平均數的 $\frac {1} {n}$。

平均速率、並聯等效電阻和縮減質量等相關應用，數據都是呈現倒數關係，因此使用調和平均來作相關計算。

而由於上述提到的**Precision**跟**Recall**，其分子是相同的，唯一的差別在於分母，也就是呈現所謂的倒數關係。所以這裡我們在F-Measure使用調和平均數。

<br/><br/>

### 結尾

除了**Precision**跟**Recall**之外，在醫學方面也利用混淆矩陣建立的不少指標，像是Prevalence、Specificity等等，但這些與機器學習的關係比較遠，如果對於這些指標有興趣的人，也可以透過WIKI來了解更多。那有關**Precision**跟**Recall**的介紹就到這邊囉!

<br/><br/>

ref[1]：[如何辨別機器學習模型的好壞？秒懂Confusion Matrix](https://www.ycc.idv.tw/confusion-matrix.html)
ref[2]：[WIKI -- Confusion Matrix](https://en.wikipedia.org/wiki/Confusion_matrix)
ref[3]：[Precision, Recall, F1-score簡單介紹](https://medium.com/nlp-tsupei/precision-recall-f1-score%E7%B0%A1%E5%96%AE%E4%BB%8B%E7%B4%B9-f87baa82a47)
ref[4]：[心理學和機器學習中的 Accuracy、Precision、Recall Rate 和 Confusion Matrix](https://medium.com/@ChingTien/%E5%BF%83%E7%90%86%E5%AD%B8%E5%92%8C%E6%A9%9F%E5%99%A8%E5%AD%B8%E7%BF%92%E4%B8%AD%E7%9A%84-accuracy-precision-recall-rate-%E5%92%8C-confusion-matrix-529d18abc3a)
ref[5]：[WIKI -- Harmonic mean](https://en.wikipedia.org/wiki/Harmonic_mean)
