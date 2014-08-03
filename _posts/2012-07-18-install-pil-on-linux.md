---
layout: post
title: Linux下Python模块PIL的安装
date: 2012-07-18 18:05:26.641000
slug: install-pil-on-linux
tags: Python, PIL
---

安装Django应用发现图片上传老出问题，发现时默认pip安装的PIL没有链接libjpeg等库，导致不能支持JPEG图片。总结，安装完服务器应该安装的模块有

    sudo apt-get install zlib1g-dev liblcms1-dev python-dev
    sudo apt-get install libjpeg8 libjpeg8-dev libpng12-0 libpng12-dev libfreetype6 libfreetype6-dev zlib1g-dev

然后

    sudo pip install PIL

或者直接

    sudo apt-get install python-imaging

还发现如果通过pip安装MySQL-python需要安装一下模块

    sudo apt-get install python-dev libmysqlclient-dev

本文测试环境为ubuntu server 12.04