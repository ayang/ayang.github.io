---
layout: post
title: 极端条件下修复Yosemite系统的方法
date: 2014-10-29
tags: osx, mac
---

2012年的MBP，上次换SSD把恢复分区给删掉了，这次升级Yosemite后还是没看到恢复分区，就想用[Recovery Partition Creator 3.8
](http://musings.silvertooth.us/2014/07/recovery-partition-creator-3-8/)这个工具安装一个恢复分区，结果是进不了系统，主分区遭到破坏，恢复分区也没装上。

只有重装一条路可选，但重装有两个办法，一个是系统盘，我没有；二是网络恢复，但试了一下，我的2012年的本网络恢复会下载Moutain Lion版本，然后升级，而且下载这个包非常慢，估计需要6小时左右。我现在手头只有三个东西，一台Ubuntu台式机，一个移动硬盘，一个4G U盘。从网上下载了Yosemite安装包后，尝试了各种在Linux下制作OS X系统盘的方法都失败了。妈的，不让人活了！

经过几个小时的探索，终于发现了一条小路可以通往重装系统。中间种种失败就不表了。

## 解决方案

1. 用U盘制作Ubuntu安装盘，启动Mbp,进入系统后发现Mbp的主分区其实没坏，只是启动不了。看了看文件都在，把需要备份的文件拷贝到移动硬盘。

2. 重新启动Mbp, 按`Command+Opt+R`进入网络恢复，这次运气还行，8分钟就下载完网络恢复包进入基础系统了。

3. 我的移动硬盘是Ntfs格式，在Ubuntu下将Yosemite的安装包dmg文件放到移动硬盘。拔下，插到Mbp。

4. 在Mbp进入磁盘工具，挂载Yosemite安装包。这一步应该也可以用命令行方式实现，但我当时还没想到，我是这样操作的：1)随便点击一个分区，再点恢复标签，在源磁盘的地方可以选择一个映像，点击选择Yosemite安装包的dmg映像，此时左侧应该能看到这个映像已经挂载，如果没有挂载，随便选一个分区作为目标，再点恢复应该就能挂载。此时挂载的是一块磁盘，在左侧双击该磁盘就会挂载分区。由于我这次想格盘重装，我把磁盘格为了一个分区。如果手头有一个8G的U盘，下一步就简单了，如果没有，你需要：硬盘划分为两个分区，后面一个大小是8G，两个分区的卷标分别是`Macitosh SSD`和`SSD8G`。

5. 退出磁盘工具，进入命令行工具，使用`ls /Volumes`应该已经能看到一个磁盘分区名字叫`Install Yosemite`。用命令行工具制作启动盘。如果你有8GU盘，SSD8G改为你的U盘。详细的安装盘制作方法参考[http://www.iplaysoft.com/osx-yosemite-usb-install-drive.html](http://www.iplaysoft.com/osx-yosemite-usb-install-drive.html)。

	sudo /Volumes/<移动硬盘>/Install\ OS\ X\ Yosemite.app/Contents/Resources/createinstallmedia --volume /Volumes/SSD8G --applicationpath /Volumes/<移动硬盘>/Install\ OS\ X\ Yosemite.app --nointeraction

6. 重新开机按Opt键，用制作好的安装盘启动。用正常方法安装系统直到Yosemite可以正常运行。因为我的都在SSD上，所以此过程很快。

7. 如果你用了U盘做安装盘，此步忽略。重启进入恢复分区（Yosemite将恢复分区隐藏了，用Opt启动是看不到的，此处被捉弄了，需要按`Command+R`），删掉`SSD8G`这个分区，此处`disk utility`还是比较聪明，主分区自动合并了删掉后的空闲分区。重启。终于完了。

## 心得

1. Mac确实是一个很封闭的系统，基本无法使用其他操作系统制作一个OSX的启动盘或者安装盘。不要用Linux工具去改Mac的分区表，经常会导致一些奇怪的问题。

2. Recovery Partition Creator 3.8官方博客上说还不支持Yosemite，我竟然没看到，才导致一系列事情发生。多看官方文档没坏处。

3. 对操作系统动手之前要做好充分准备，应对你想不到的事情。如果有Time Machine就简单多了。
