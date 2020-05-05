---
title: "CS231_n Image Classification - part1"
date: 2020-05-05T12:00:00+08:00
description: "LeetCode Solving"
draft: false
hideToc: false
enableToc: true
enableTocContent: false
author: Stanley
authorEmoji: 🏫
tags:
- cs231n
- Image Classification
- KNN
- python
- Deep learning
series:
- cs231_n
categories:
- cs231_n
- CNN
- DeepLearning
image: images/feature2/mathbook.png
---

### 前言

影像分類(Image Classification)主要的作法是將輸入的影像做辨識並且分門別類。這對於電腦視覺的領域來說，是重要且核心的議題之一。比如說我們想在一張圖片上分辨人、貓、車子，或是在一堆車子的影像中分辨出不同品牌或是不同種類的車子。

**那怎麼去做影像分類呢? 首先我們要先了解影像的結構。**

我們知道光的三原色是 _R G B_ ，在電腦硬體上採取每一個像素以24位元的表示方式，每個位元以二進位方式儲存，那光的三原色就各佔了1/3，就是8bits。所以每一種原色的強度就會依照8bits的最高值2<sup>8</sup>分成256個值，表示範圍從0~255。

假設一張 800 x 600 的彩色圖片，裡面所包含的資訊就會有  
> 800 x 600 x 3 = 1440000

假設解析度更好，比如說是4K解析度，那資訊量基本上是非常龐大的。
<br/>
![img1](/images/CS231_n/img_classification/img1.png)
<br/><br/>

### Challenges

根據拍攝的場景與情形，其實會造成很多在實際上應用的難處。我們這邊複製cs231n的筆記內容來簡單講述一下會遇到的問題。
<br/><br/>
![challenges](/images/CS231_n/img_classification/challenges.jpeg)
<br/><br/>
* **Viewpoint variation.** A single instance of an object can be oriented in many ways with respect to the camera.

* **Scale variation.** Visual classes often exhibit variation in their size (size in the real world, not only in terms of their extent in the image).

* **Deformation.** Many objects of interest are not rigid bodies and can be deformed in extreme ways.

* **Occlusion.** The objects of interest can be occluded. Sometimes only a small portion of an object (as little as few pixels) could be visible.

* **Illumination conditions.** The effects of illumination are drastic on the pixel level.

* **Background clutter.** The objects of interest may blend into their environment, making them hard to identify.

* **Intra-class variation.** The classes of interest can often be relatively broad, such as chair. There are many different types of these objects, each with their own appearance.
<br/><br/>

#### Viewpoint variation
針對同一個物體來說，假設我們今天從不同角度拍攝他，可能因為周遭的光線或是有遮蔽物等等的因素，會造成pixel的變化。pixel一但有變化，有可能會造成辨別的困難增加。
<br/><br/>

![img2_viewpoint](/images/CS231_n/img_classification/img2_viewpoint.PNG)
<br/><br/>

#### Scale variation

辨識的物件相對於其他物件的對比大小，包含在影像裡與現實生活中。
<br/><br/>

#### Illumination conditions
剛剛有提到電腦硬體中的三原色儲存方式，0~255表示該原色的亮度。你應該看過有些照片在逆光下，人的臉孔根本完全看不清楚。所以環境的明暗程度，將會影響到辨識的正確率。
<br/><br/>

![img2_illumination](/images/CS231_n/img_classification/img2_Illumination.PNG)

<br/><br/>

#### Deformation
有看到我們可愛的貓貓嗎? 俗話說貓貓就跟液體一樣會變形，變形之後怎麼有辦法讓電腦辨識得出來呢? 現實生活中生物都是非剛體的，所以變形的情況也是需要去克服的。
<br/><br/>

![img2_deformation](/images/CS231_n/img_classification/img2_deformation.PNG)

<br/>

> 這裡的 Deformation 指的是體態的不同，並不是像前面viewpoint中，在不同角度看同一個物件的形狀不同。

<br/><br/>

#### Occlusion
中文翻譯叫做咬合，實際上的意思就是說，有興趣的物件被部分遮蔽了。我們可能看到貓尾巴，就知道那邊有一隻貓，但是電腦卻分類不出來。
<br/><br/>

![img2_occlusion](/images/CS231_n/img_classification/img2_occlusion.PNG)

> 物件被部分遮蔽了，顯露出來的地方只有幾個像素的大小。

<br/><br/>

#### Background clutter
白貓在雪地，黑貓站在黑布上，橘貓在枯黃的草地上享受夕陽，如果連我們都不仔細看，就會遺漏啊!更何況是電腦了，根本辨識不出來。
<br/><br/>

![img2_backgroundclutter](/images/CS231_n/img_classification/img2_backgroundclutter.PNG)
<br/><br/>
> 簡單來講就是，環境保護色。

<br/><br/>

#### Intraclass variation
相同名稱但是卻不同形狀、顏色，像是圖片上的貓咪顏色不同，或是椅子有不同的形狀。該怎麼去分門別類，就要針對實際應用囉!
<br/><br/>

![img2_intraclass](/images/CS231_n/img_classification/img2_intraclass.PNG)

<br/><br/>

### 資料驅動的處理方式 (Data-driven Approach)

一般我們在寫排序相關的演算法，都是在程式裡面指定說我們想用什麼方式來做。但對於處理影像辨識的問題，因為辨識的情況依照各自領域的不同，演算法並不是那麼統一，因此大多採用資料驅動的處理方式 _(Data-driven Approach)_ 。

提供大量不同類別的examples，並發展一個「學習演算法」來瀏覽這些examples，並學習這些類別的 **「視覺外觀」** 。學習的資料集我們稱做 **traning set**，測試完之後要驗證正確性的資料集稱做 **testing set**。其實這種學習的方法，我們就稱做是 _機器學習_ 。

那麼整個機器學習的流程是什麼呢?

> 1. 收集影像 _(images)_ 與標籤 _(label)_ 的資料集
> 2. 利用機器學習的方法來訓練分類器 _(classifier)_
> 3. 在新的影像上評估這個分類器是否有效

以上的label就是類別一樣，比如說標記成狗、貓、飛機之類的。

以CIFAR10為例
<br/>

![img3_classifier](/images/CS231_n/img_classification/img3_classifier.png)
<br/><br/>
這邊附上 CS231_n 的 image classification pipeline原文

* **Input:** Our input consists of a set of N images, each labeled with one of K different classes. We refer to this data as the training set.

* **Learning:** Our task is to use the training set to learn what every one of the classes looks like. We refer to this step as training a classifier, or learning a model.

* **Evaluation:** In the end, we evaluate the quality of the classifier by asking it to predict labels for a new set of images that it has never seen before. We will then compare the true labels of these images to the ones predicted by the classifier. Intuitively, we’re hoping that a lot of the predictions match up with the true answers (which we call the ground truth).

<br/><br/>

### Classification
#### Nearest Neighbor Classifier
影像辨識常會用到 _Convolutional Neural Networks (CNN)_ ，但是這個Nearest Neighbor Classifier卻跟CNN毫無相關，充其量只是讓我們了解機器學習的一種處理方式而已。

CS231_n 筆記裡面提到 nearest neighbor classifier的處理方式:
> The nearest neighbor classifier will take a test image, compare it to every single one of the training images, and predict the label of the closest training image.

有兩個重點

1. 找test image跟"每一個" train image做比較。
2. 判斷此test image跟label的關係遠近。

第一點很好理解，就是把某張測試集的圖片，對訓練集的每一張圖片進行比對，然後用分數，也就是一個函數來評估是否屬於這個類別。

第二點，也就是關於這個函數，該用什麼函數來表示之間關係的遠近程度? 絕對值就可以嗎? 兩點距離公式嗎? 這個問題就是 **Metric** 或稱作是 **distance function** ，詳細過程不在這邊敘述，可以參考以下資料。

[**Metric**](https://en.wikipedia.org/wiki/Metric_(mathematics))   
[**metric-learn: Metric Learning in Python**](http://contrib.scikit-learn.org/metric-learn/index.html)
<br/><br/>
這邊的distance function是採用下列的方式計算的。每一格的值相減完之後，取完絕對值在將每一格相加起來。
<br/>

![img4_metric](/images/CS231_n/img_classification/img4_metric.PNG)
<br/><br/>
簡單的Nearest Neighbor Classifier實作大概如下。如果要詳細看介紹的話

[**nearest-neighbor-classifier**](https://cs231n.github.io/classification/#nearest-neighbor-classifier)

這邊就不再說明 **nearest-neighbor-classifier** 該如何實作了，因為準確性也不高。

```Python
import numpy as np

class NearestNeighbor(object):
  def __init__(self):
    pass

  def train(self, X, y):
    """ X is N x D where each row is an example. Y is 1-dimension of size N """
    # the nearest neighbor classifier simply remembers all the training data
    self.Xtr = X
    self.ytr = y

  def predict(self, X):
    """ X is N x D where each row is an example we wish to predict label for """
    num_test = X.shape[0]
    # lets make sure that the output type matches the input type
    Ypred = np.zeros(num_test, dtype = self.ytr.dtype)

    # loop over all test rows
    for i in range(num_test):
      # find the nearest training image to the i'th test image
      # using the L1 distance (sum of absolute value differences)
      distances = np.sum(np.abs(self.Xtr - X[i,:]), axis = 1)
      min_index = np.argmin(distances) # get the index with smallest distance
      Ypred[i] = self.ytr[min_index] # predict the label of the nearest example

    return Ypred
```

<br/><br/>

#### k - Nearest Neighbor Classifier (KNN)

[看我如何實作knn (ongoing)]

> 找出k個與目標比較相近的圖片，並讓這k個圖片「投票」選出目標的類別。

所以上述條件在 _k = 1_ 時，就是前一個章節提到的 **Nearest Neighbor Classifier** 。
<br/><br/>

![knn](/images/CS231_n/img_classification/knn.jpeg)
<br/><br/>
所以我們現在的問題是，_k_ 到底該怎麼選擇?

<br/><br/>

#### 超參數調整 (Hyperparameter tuning)

_k_ 如何選擇? 該選擇什麼樣的distance function? 這些選擇我們叫作超參數 _(hyperparameter)_ ，也就是如何設置演算法的參數。那該如何選擇這些參數呢?

> 一個一個調整。

沒錯就是一個一個調整。

**k = 1, performance = ...**
**k = 2, performance = ...**
**k = 3, performance = ...**

並且參數的調整還是 **problem-dependent** ，取決於你想要用在地方，參數調整也會不同。

根據我們上面的討論，我們資料集會分成 __training set__ 與 __testing set__ ，所以我們調整超參數的方式就是 _(假設我們只考慮k的調整好了)_

1. 選擇一個k值，去train。
2. 驗證看看testing set的準確率如何，如果不滿意，在更動k值
3. 回去重新train

至此，恭喜你，當你把train好的模型放到實際應用端時，你會發現你有 __大麻~煩__ 囉!
<br/><br/>
為什麼?
<br/><br/>
因為部署到應用端時，你所接收到的是新的資料集，這些新的資料集不一定能夠完好的符合你的model，也就是不知道這個演算法是不是適合新的data，比較好的作法是

1. 將資料集切成traing, validation, testing
2. 選擇一個k值，去train。
3. 驗證validation set的準確率，如果不滿意，更動k值回去重新train
4. 結果滿意之後，在testing set進行測試。

<br/><br/>

![img5_hyperparameter](/images/CS231_n/img_classification/img5_hyperparameter.PNG)

<br/><br/>

#### Cross-validation

當training data太少，那我們就會使用Cross-validation來作hyperparameter tuning。

比如說我們把training set 切成 n 等分，把其中一個fold當作是validation set，然後進行training。接著把validation set改為下一個fold，再進行training。如此反覆總共作n次，最後將這些performance進行成效比較
<br/><br/>

![img5_crossvalidation](/images/CS231_n/img_classification/img5_crossvalidation.PNG)
<br/><br/>

如果對其他的Cross-validation也有興趣，可以看以下的介紹

[交叉驗證(Cross-validation, CV)](https://medium.com/@chih.sheng.huang821/%E4%BA%A4%E5%8F%89%E9%A9%97%E8%AD%89-cross-validation-cv-3b2c714b18db)

### 總結

KNN的優點在於training的時間很短，因為只有將data作切割與儲存。但是在test time卻很漫長，因為必須去比較 **每一個** training image。但是我們所希望的是在testing時可以馬上就得到yes or no的結果。所以KNN其實不適合用在影像辨識上。

如果你想觀看原文的note可以點選
[cs231_n image classification notes](https://cs231n.github.io/classification/)

想看如何實作KNN [看我如何實作knn (ongoing!!)]

那麼下一篇就是 [Linear Classification]
