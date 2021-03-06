目的

在这篇教程中，你将会学到：

* 使用OpenCV中的cv::split函数来将图片分成几块。
* 使用OpenCV重点的cv::calcHist函数来计算图像中直方柱的个数。
* 使用cv::normalize函数对图像的直方数组进行归一化处理。

> 注意
> 
> 在上一篇教程中（直方图方程）我们讨论了直方图的一种形式叫做图片直方图。现在我们考虑一下他更广泛的概念。继续阅读一下。

什么叫直方图？

* 直方图是将数据进行统计并放置到预定义的二值柱中。
* 我们所说的数据，并不是仅限制在强度值中（正如我们之前的教程直方图方程中看到的那样）。数据可以是任何你认为可以很好描述图片的内容。
* 让我们来看看一个例子。想象一下这样一个包含图片信息的矩阵（比如强度值在0~255之间）：

![](https://docs.opencv.org/Histogram_Calculation_Theory_Hist0.jpg)

* 如果我们将数据进行整合统计会有什么情况呢？既然在这个例子当中，我们的数据会有大概256种值，我们可以将这个范围细分一下（叫做小集合），就比如：

![](http://latex.codecogs.com/gif.latex?%5Cbegin%7Barray%7D%7Bl%7D%20%5B0%2C%20255%5D%20%3D%20%7B%20%5B0%2C%2015%5D%20%5Ccup%20%5B16%2C%2031%5D%20%5Ccup%20....%5Ccup%20%5B240%2C255%5D%20%7D%20%5C%5C%20range%20%3D%20%7B%20bin_%7B1%7D%20%5Ccup%20bin_%7B2%7D%20%5Ccup%20....%5Ccup%20bin_%7Bn%20%3D%2015%7D%20%7D%20%5Cend%7Barray%7D)

并且，我们会计算在预定义子集(![](http://latex.codecogs.com/gif.latex? (![](http://latex.codecogs.com/gif.latex?)中每一个像素点的数量。对上述图片应用我们介绍的方法，我们就能得到下面这张图片（x轴是子集，y轴是子集的数量）。

![](https://docs.opencv.org/Histogram_Calculation_Theory_Hist1.jpg)

* 这就是一个简单的例子，他告诉直方图的使用和它的重要性。直方图不仅保留了颜色强度值，还显示了我们想分析的图像信息（比如，径向、方向等）。
* 让我们来看看直方图的每一个部分：

    1.dims：你想将你收集的数据分成几个区。例如，在我们给出的例子当中，dims=1表示我们只是想计算每个像素的强度值（在一个灰阶图中）。
    2.bins：分区中子集的个数。在我们给出的例子中，bins=16。
    3.range：值的范围。以这个例子为例：range=[0,255]

* 那么如果你想分析两个特征呢？在这个例子中你可以将直方图画成3D的样式（x和y代表的是(![](http://latex.codecogs.com/gif.latex?bin_{x})和(![](http://latex.codecogs.com/gif.latex?bin_{y})，而z则是各个特征的数量(![](http://latex.codecogs.com/gif.latex?%28bin_%7Bx%7D%2C%20bin_%7By%7D%29)）。如果你需要更多的特征，那么就使用相同的方法（当然要更有技巧性）。

OpenCV提供的功能

简单而言，OpenCV实现了cv::calcHist函数，它能够计算一组数组的直方图（通常是图像或图像的一部分）。它可以分析32个维度。我们将在下面的代码中知道这个函数的用法。