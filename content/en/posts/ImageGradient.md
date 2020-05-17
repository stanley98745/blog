---
title: "Image Gradient"
date: 2020-05-15T18:00:00+08:00
description: "Image Processing"
draft: false
hideToc: false
enableToc: true
enableTocContent: false
author: Stanley
authorEmoji: 🏫
tags:
- Image Processing
series:
- Image Processing
categories:
- Image Processing
libraries:
- katex
image: images/feature2/mathbook.png
---

Gradient 翻譯為梯度，也就是函數在某個方向上的變化率。 而Image Gradient指的就是影像的顏色或是強度，沿著某個方向的變化程度。 透過Image Gradient我們能夠對影像進行一些處理，像是判別顏色漸變的地方，或是**edge detection**。

<br/><br/>

### 數學型式

Image Gradient 是對著x方向與y方向微分，所以對於一個target pixel，最簡單的Image Gradient就是左右(x方向)與上下(y方向)的微分了。因此影像的梯度就會是一個vector(通常以矩陣型式來表示)：

$$\nabla f(x,y) = \begin{pmatrix} g_x \\\\ g_y \end{pmatrix} = \begin{pmatrix} \frac{\partial f}{\partial x} \\\\[8pt] \frac{\partial f}{\partial y} \end{pmatrix} = \begin{pmatrix} f(x+1, y) - f(x-1, y) \\\\[8pt] f(x, y+1) - f(x, y-1) \end{pmatrix}$$

<br/>

對於梯度來說，最重要的兩個參數就是大小(Magnitude)與方向(Direction)。

- **Magnitude**: $g = \sqrt{ g_x^2 + g_y^2 }$
- **Direction**: $\theta = \arctan{(g_y / g_x)}$

<br/>

舉個簡單的例子

<img src="/images/ImageGradient/1_TargetPixel.png" width="50%" class="center">

<br/><br/>

Image Gradient Vector就會是

$$\nabla f = \begin{pmatrix}f(x+1, y) - f(x-1, y) \\\\[8pt] f(x, y+1) - f(x, y-1)\end{pmatrix} = \begin{pmatrix} 90-180 \\\\ 70-40 \end{pmatrix} = \begin{pmatrix} -90\\\\ 30 \end{pmatrix}$$

- **Magnitude**: $$g = \sqrt{ (-90)^2 + 30^2 } = 94.868$$
- **Direction**: $$\theta = \arctan{(30 / -90)} = -18.435^{\circ}$$

<br/><br/>

以上的過程都是線性運算。另外對於一張影像來講，通常都是幾十萬到幾百萬個像素，而我們的Operator只有前後或是上下，所以是一個 1x3 或是 3x1 的矩陣。也就是我們不需要global的資訊，只需要local的資訊。對於一個線性運算以及只需要local資訊的做法，我們有一個很實用的運算方式，那就是**Convolution**。

不過使用Convolution需要注意一下，以上的Image Gradient Vector的計算方式，用矩陣來看的話會是

<img src="/images/ImageGradient/2_wrongConv.png" width="50%" class="center">

<br/><br/>

不過Convolution的計算方式是會前後相反的，計算方式會是

<img src="/images/ImageGradient/3_correctConv.png" width="50%" class="center">

<br/><br/>

所以在使用Operator的時候，要注意一下kernel的問題。

如果不熟悉Convolution的話，可以看一下 [**Convolution**](http://www.songho.ca/dsp/convolution/convolution.html)

<br/><br/>

### 實作Image Gradient

```python
"""
Image Gradient Vector
"""

import numpy as np
from scipy import signal as sig

data = np.array([[0, 70, 0],
                 [180, 255, 90],
                 [0, 40, 0]])

G_x = sig.convolve2d(data, np.array([[0, 0, 0],
                                     [1, 0, -1],
                                     [0, 0, 0]]), mode='same')

G_y = sig.convolve2d(data, np.array([[0, -1, 0],
                                     [0, 0, 0],
                                     [0, 1, 0]]), mode='same')

print(f'G_x is \n{G_x}')
print(f'G_y is \n{G_y}')
```

<br/><br/>

### 常見的Kernels

[Prewitt](https://en.wikipedia.org/wiki/Prewitt_operator)：相較於前述的Image Gradient只針對Target pixel上下左右的像素做計算，Prewitt Operator使用了Target pixel的8個周遭像素來做計算。

$$\mathbf{G}_x = \begin{pmatrix}-1 & 0 & +1 \\\\[8pt]-1 & 0 & +1 \\\\[8pt]-1 & 0 & +1\end{pmatrix} \ast \mathbf{A} \text{ , } \mathbf{G}_y = \begin{pmatrix}+1 & +1 & +1 \\\\[8pt]0 & 0 & 0 \\\\[8pt]-1 & -1 & -1\end{pmatrix} \ast \mathbf{A}$$

<br/>

[Sobel](https://en.wikipedia.org/wiki/Sobel_operator)：對於Target pixel的上下左右像素更為重視，提高了這些像素的權重。

$$\mathbf{G}_x = \begin{pmatrix}-1 & 0 & +1 \\\\[8pt]-2 & 0 & +2 \\\\[8pt]-1 & 0 & +1\end{pmatrix} \ast \mathbf{A} \text{ , } \mathbf{G}_y = \begin{pmatrix}+1 & +2 & +1 \\\\[8pt]0 & 0 & 0 \\\\[8pt]-1 & -2 & -1\end{pmatrix} \ast \mathbf{A}$$

<br/><br/>

#### Sobel 應用範例

首先我們隨意找一張照片，並把這張照片轉換成灰階，可以透過opencv來完成。並且我們定義Sobel Operator Kernel，最後使用convolve2d來完成。

![kitties](/images/ImageGradient/3_kitties.jpg)

```python
import numpy as np
import cv2
import scipy.signal as sig
import matplotlib.pyplot as plt

# Read image and convert it to grayscale
img = cv2.imread("kitties.jpg", cv2.IMREAD_GRAYSCALE)

# Define Sobel operator kernels
kernel_x = np.array([[-1, 0, 1],
                     [-2, 0, 2],
                     [-1, 0, 1]])

kernel_y = np.array([[-1, -2, -1],
                     [0, 0, 0],
                     [1, 2, 1]])

sobelG_x = sig.convolve2d(img, kernel_x, mode='same')
sobelG_y = sig.convolve2d(img, kernel_y, mode='same')

# Plot the result
fig = plt.figure()
ax1 = fig.add_subplot(121)
ax2 = fig.add_subplot(122)
ax1.imshow(sobelG_x, cmap='gray')
ax2.imshow(sobelG_y, cmap='gray')
ax1.set_xlabel("sobelGx")
ax2.set_xlabel("sobelGy")
plt.show()
```

<br/>

![EdgeDetection](/images/ImageGradient/EdgeDetection.png)

<br/><br/>

除了自定義Kernel之外，opencv也提供了[Sobel Operator](https://docs.opencv.org/3.4/d4/d86/group__imgproc__filter.html#gacea54f142e81b6758cb6f375ce782c8d)給使用者。

> dst = cv.Sobel(src, ddepth, dx, dy[, dst[, ksize[, scale[, delta[, borderType]]]]])

<br/>

參數說明 _粗體字為必選_

- **src**： 輸入影像的陣列。
- **ddepth**： 輸出影像的深度，表示電腦可分辨的色彩數目，越多代表越細膩。[參考](https://docs.opencv.org/3.4/d4/d86/group__imgproc__filter.html#filter_depths)
- **dx, dy**： 求導階數，0 表示不求導數。
- dst：輸出影像的陣列。
- ksize：Sobel Kernel的大小，必須為1, 3, 5, 7。 (n x n 的方陣)
- scale：可以縮放導數的比例，像是 y = a x + b 的 a。
- delta：對輸出陣列增減一個bias，像是 y = a x + b 的 b。
- borderType：convolution在邊界上的處理方式。

<br/>

```python
import cv2

# Read image and convert it to grayscale
img = cv2.imread("kitty.jpg", cv2.IMREAD_GRAYSCALE)

# Sobel Operator in x, y direction
Sobel_x = cv2.Sobel(img, cv2.CV_64F, dx=1, dy=0, ksize=3)
Sobel_y = cv2.Sobel(img, cv2.CV_64F, dx=0, dy=1, ksize=3)

absX = cv2.convertScaleAbs(Sobel_x)
absY = cv2.convertScaleAbs(Sobel_y)
output = cv2.addWeighted(absX, 0.5, absY, 0.5, 0)

# Resize images for stitching images
img = cv2.resize(img, (640, 360))
absX = cv2.resize(absX, (640, 360))
absY = cv2.resize(absY, (640, 360))
output = cv2.resize(output, (640, 360))

result = cv2.vconcat([cv2.hconcat([img, absX]), cv2.hconcat([absY, output])])

font = cv2.FONT_HERSHEY_SIMPLEX

result = cv2.putText(result, 'original', (50, 300), font, 1, (255, 255, 255), 1)
result = cv2.putText(result, 'absX', (690, 300), font, 1, (255, 255, 255), 1)
result = cv2.putText(result, 'absY', (50, 660), font, 1, (255, 255, 255), 1)
result = cv2.putText(result, 'Both', (690, 660), font, 1, (255, 255, 255), 1)

cv2.imshow("Result", result)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

<br/>

<img src="/images/ImageGradient/result.jpg" width="85%" class="center">

<br/><br/>

以上就是Image Gradient的相關介紹囉! 如果有任何問題的話，都歡迎留言哦!

ref:
[1] [WIKI - Image Gradient](https://en.wikipedia.org/wiki/Image_gradient)
[2] [Object Detection for Dummies](https://lilianweng.github.io/lil-log/2017/10/29/object-recognition-for-dummies-part-1.html)
[3] [Convloution](http://www.songho.ca/dsp/convolution/convolution.html)
