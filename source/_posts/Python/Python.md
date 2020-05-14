---
toc: true
title: Python
categories: 
 - 笔记
tags:
 - Python
---

### PIL&Pillow
<!-- more -->
Python图像库PIL(Python Image Library)是python的第三方图像处理库，但是由于其强大的功能与众多的使用人数，几乎已经被认为是python官方图像处理库了。其官方主页为:PIL。 PIL历史悠久，原来是只支持python2.x的版本的，后来出现了移植到python3的库pillow,pillow号称是friendly fork for PIL,其功能和PIL差不多，但是支持python3。本文主要介绍PIL那些最常用的特性与用法,主要参考自:http://www.effbot.org/imagingbook
PIL可以做很多和图像处理相关的事情:

+ 图像归档(Image Archives)。PIL非常适合于图像归档以及图像的批处理任务。你可以使用PIL创建缩略图，转换图像格式，打印图像等等。
+ 图像展示(Image Display)。PIL较新的版本支持包括Tk PhotoImage，BitmapImage还有Windows DIB等接口。PIL支持众多的GUI框架接口，可以用于图像展示。
+ 图像处理(Image Processing)。PIL包括了基础的图像处理函数，包括对点的处理，使用众多的卷积核(convolution kernels)做过滤(filter),还有颜色空间的转换。PIL库同样支持图像的大小转换，图像旋转，以及任意的仿射变换。PIL还有一些直方图的方法，允许你展示图像的一些统计特性。这个可以用来实现图像的自动对比度增强，还有全局的统计分析等。

#### Image Module

+ open

  ```python
  from PIL import Image             ##调用库，包含图像类
  im = Image.open("3d.jpg")  ##文件存在的路径，如果没有路径就是当前目录下文件
  im.show()
  ```

  