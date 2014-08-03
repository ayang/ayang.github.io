---
layout: post
title: Mac 下自动更换DNS服务器脚本
date: 2013-08-28 03:13
tags: Mac, Bash Script
---

经常切换 google dns 和 114 dns，就写了个脚本，放到个人~/bin目录下。

	#!/bin/bash

	if [ $# -ne 2 ] ; then
	    echo Useage: $0 network dnsgroup
	    exit 0
	fi

	network=$1
	case $1 in
	    e ) network=Ethernet ;;
	    w ) network=Wi-Fi ;;
	esac

	dns=$2
	case $2 in
	    114 ) dns='114.114.114.114 114.114.115.115' ;;
	    8 ) dns='8.8.8.8 8.8.4.4' ;;
	esac

	networksetup -setdnsservers $network $dns

需要将此代码保存为文件到~/bin/usedns，并加执行权限。然后运行

	usedns e 114

即可切换有线网络的dns到 114 dns。