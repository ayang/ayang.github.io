---
title: Gmail中的图片和背景都看不到了
date: 2012-04-03 23:52
tags: gmail
---

打开Gmail，发现整个网页一片黑，所有的主题背景和图片、图标都不见了，研究了一下，是ssl.gstatic.com这个域名被强奸了，于是乎，咱们老办法：修改hosts文件，加入：

    74.125.227.111 ssl.gstatic.com

刷新，亮了。