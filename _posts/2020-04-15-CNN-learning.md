---
layout:     post
title:      "神经网络之CNN"
subtitle:   " \"广泛应用的卷积神经网络\""
date:       2020-04-15 12:00:00
author:     "LiYanXin"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - VHDL
---

# 卷积神经网络的学习

## CNN的优点

CNN可以说是应用最火的神经网络之一，特别是在模式分类领域，由于该网络避免了对图像的复杂前期预处理，可以直接输入原始图像，因而得到了更为广泛的应用。 

下面简单记录一下卷积神经网络的学习。

## 卷积层  
  卷积运算就是将原始图片的与特定的Feature Detector(filter)做卷积运算(符号⊗)，卷积运算就是将下图两个3x3的矩阵作相乘后再相加，以下图为例0 *0 + 0*0 + 0*1+ 0*1 + 1 *0 + 0*0 + 0*0 + 0*1 + 0*1 =0

  CNN 的第一层通常是卷积层（Convolutional Layer）。首先需要了解卷积层的输入内容是什么。
  
  * 过滤器(filter)  
  
  <img width="50%" height="50%" src="/img/in-post/conv_layer.png"/>
  
  如上所述，输入内容为一个 7 x 7 的像素值数组。现在，解释卷积层的最佳方法是想象有一束手电筒光正从图像的左上角照过。
  假设手电筒光可以覆盖 3 x 3 的区域，想象一下手电筒光照过输入图像的所有区域。在机器学习术语中，这束手电筒被叫做过滤器（filter，有时候也被称为神经元（neuron）或核（kernel）），被照过的区域被称为感受野（receptive field）。过滤器同样也是一个数组（其中的数字被称作权重或参数）。
  重点在于过滤器的深度必须与输入内容的深度相同（这样才能确保可以进行数学运算），因此过滤器大小为 3 x 3。
  现在，以过滤器所处在的第一个位置为例，即图像的左上角。当筛选值在图像上滑动（卷积运算）时，过滤器中的值会与图像中的原始像素值相乘（又称为计算点积）。
  这些乘积被加在一起（从数学上来说，一共会有 25 个乘积）。现在你得到了一个数字。切记，该数字只是表示过滤器位于图片左上角的情况。  
  
  <img width="50%" height="50%" src="/img/in-post/conv_layer1.png"/>
  
  我们在输入内容上的每一位置重复该过程。（下一步将是将过滤器右移 1 单元，接着再右移 1 单元，以此类推。）输入内容上的每一特定位置都会产生一个数字。
  过滤器滑过所有位置后将得到一个 5 x 5 的数组，我们称之为激活映射（activation map）或特征映射（feature map）。  
  
  很容易得出的一个问题就是，这个过滤器是怎么工作的呢？它的意义是什么呢？我联想到之前上过的一门图象处理课，老师曾教过拉普拉斯算子，Sobel算子，现在想来
  filter应该和它们的意义相近，它们可以忽略图象中多余杂乱的信息，将图象的边界萃取出来，当然我们可能不光关心图象的边界，还有其他的一些细节，都可以采用不同的filter实现我们的目的。  
  
  <img width="50%" height="50%" src="/img/in-post/relu.png"/>
  
  <img width="50%" height="50%" src="/img/in-post/relu1.png"/>
  
  <img width="50%" height="50%" src="/img/in-post/relu2.png"/>
  
  最常见的应该就是relu函数，可以去掉图象中的负值，得到清晰的形状。

	* 步长(stride)   
  
  Stride是每次卷积滤波器移动的步长。步幅大小通常为1，意味着滤镜逐个像素地滑动。但往往我们的一副图象会有很多像素值，这时我们需要调整步长，让处理时间和资源在可以接受的范围内。
  通过增加步幅大小，您的滤波器在输入上滑动的间隔更大，因此单元之间的重叠更少。  
	下面的动画显示步幅大小为1。 
  <img width="50%" height="50%" src="/img/in-post/stride.gif"/>
  
  * 填充  

	添加一层零值像素以使用零环绕输入，这样我们的要素图就不会缩小。除了在执行卷积后保持空间大小不变，填充还可以提高性能并确保内核和步幅大小适合输入。  

	可视化卷积层的一种好方法如下所示，最后我们以一张动图解释下卷积层到底做了什么。  

	<img width="50%" height="50%" src="/img/in-post/summary_conv.png"/>

    
 ## 总结
　　现在我们基本掌握了建立时间和保持时间的概念，下一篇可以开始讨论跨时钟域的问题了！
    
    
    
    
    
    
