---
layout: post
title: cv::Mat 和 gdiplus::Bitmap 行字节对齐问题
date: 2012-09-13 05:45
tags: opencv gdiplus
---

下面是cv::Mat转gdiplus::Bitmap的函数：

	Gdiplus::Bitmap(rgbImage.cols, rgbImage.rows, rgbImage.step,
		PixelFormat24bppRGB, (BYTE*)rgbImage.data)

图像从cv::Mat转gdiplus::Bitmap时某些图转化会出错，原因是行字节对齐问题。cv::Mat默认是行连续的，没有对齐每一行的字节，而gdiplus::Bitmap的每一行都是4字节对齐的，为方便32位系统处理提高性能，而且是强制要求。

通过cv::imread函数和cv系列的其他函数得到的图像都是非字节对齐的，只有某些特殊函数，比如从IplImage得到的函数才是4字节对齐的。这就造成列数非4的整数倍的图像无法直接转化到Bitmap。

**解决方法**我想出3种：

* 在转化以前先对Mat对象进行resize，缩放到列为4的整数倍，避免问题；
* 通过新建一块内存，再按照一定的方法把Mat中的每一行字节复制到新内存中，使用这块内存作为函数的BYTE*变量，第三个参数就不能使用rgbImage.step，需要对对齐字节数量进行调整；
* 对原图clip，裁剪到列为4的整数倍；

目前我使用第一种方法，因为我用到的都是显示缩略图，不影响功能。第二种和第三种都涉及到拷贝内存问题，需要额外时间和空间。第三种找了半天没找到直接使用clip的函数，算了。
