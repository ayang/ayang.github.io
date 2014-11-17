---
layout: post
title: 让Vim在不同模式下自动切换Fcitx状态
date: 2014-11-17
tags: vim, fcitx
---

[Mind-Flex](http://mind-flex.appspot.com/)在[Fcitx for vim](http://mind-flex.appspot.com/fcitx_for_vim.html)文中介绍了配置vim使其在退出插入模式时自动禁用输入法，再进入插入模式时能够恢复以前的输入法状态的方法，但我再Fcitx 4.2下貌似不能用，后来发现是fcitx-remote命令的返回值有变化，导致Mind-Flex的脚本没有正确识别当前输入法状态。稍加修改即可使用，上代码：

<script src="https://gist.github.com/ayang/656b0fa1f4eeef254a51.js"></script>

