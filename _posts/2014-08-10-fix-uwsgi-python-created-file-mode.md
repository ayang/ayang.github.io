---
layout: post
title: 修复uwsgi下python进程创建文件夹的权限为777的问题
date: 2014-08-10
tags: uwsgi, umask
---

总是发现服务器 django 进程生成的图片文件夹的权限不正常，所有文件夹都是777，查看 uwsgi 进程的 umask 终于找出了原因，umask=0, My God.
关于 umask 的解释请看这里 http://man7.org/linux/man-pages/man2/umask.2.html
## 解决方法

啥也不说了，在 uwsgi.ini 里加上一句：

	umask = 022

问题瞬间解决。

顺便说一下查看进程 umask 的方法，需要先安装 gdb.

	ps aux | grep uwsgi  # 找到 uwsgi 进程的 pid, 比如2333
	gdb --pid=2333  # gdb 调试进程，下面的命令在 gdb 内运行
	(gdb) call umask(0)  ＃ 这句将进程的当前 umask 设置为0并返回原 umask, 即我们需要找到的结果
	(gdb) call umask(18)  # 将当前 umask 设置为022，022是八进制，转换为十进制就是18
