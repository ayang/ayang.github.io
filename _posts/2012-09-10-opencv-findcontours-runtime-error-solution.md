---
layout: post
title: Opencv findContours 函数运行错误解决办法
date: 2012-09-10
tags: Opencv, Win32, C++
---

在使用vc++2010运行一个使用opencv的findContour函数时遇到错误，在stackoverflow上找到了相同的问题：

[http://stackoverflow.com/questions/11677118/opencv-example-code-for-find-contours-vector-deallocation-issue](http://stackoverflow.com/questions/11677118/opencv-example-code-for-find-contours-vector-deallocation-issue "OpenCV example code for find contours: vector deallocation issue")

![](http://pic.yupoo.com/sudomv_v/CfWiXBhs/U8dC9.png)

按照so上给出的答案，修改了当前程序的vc运行库配置，问题解决。具体方法是：项目-属性-配置属性-C/C++-代码生成-运行库，将其改为“多线程调试(/MTd)”。