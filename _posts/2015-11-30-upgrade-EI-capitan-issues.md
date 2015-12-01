---
layout: post
title: "Macbook升级到EI Capitan系统后产生的一些问题"
date: 2015-11-30 17:00:00
description: 升级到EI Capitan系统后开发环境产生了一些问题，在此做个记录
headline:
category: others
tags: [EI_Capitan MacOS]
comments: true
mathjax:
---
###关于升级系统后出现的一些问题纪录

今天手贱升级了一下macbookpro的系统到EI Capitan 10.11，结果各种问题出现，只好在此记录一下。正好前段时间忙一直没有写blog，就来写一下吧。


#### 1.更新完后发现转接网线口的usb hub没反应了：

这个问题经过google查证，是usb驱动要重新下载， 原来这个hub是免驱动的，但是EI Capitan系统更新后有些驱动貌似不支持了。在购买记录里查到hub的芯片是Realtek RTL8152的，在realtek官网下载对应的驱动，重启后就可以正常使用了。


#### 2.homebrew问题：

homebrew有个doctor功能，运行“brew doctor”会检查哪里有问题，并显示解决方案，很方便。


首先需要我们执行“sudo xcodebuild －license” （更新系统后又更新了xcode7）， homebrew貌似用到xcode command ine tool所以这里要批准使用新的xcode license。


这里需要注意，输入上面的命令后，会提示按enter键查看license内容，然后不要按q退出了。要到最后会提示3个选项［agree， print， cancel］，这里输入agree就可以啦。


再次尝试“brew update”功能， 系统会提示出错， 提示“The /usr/local directory is not writable.” 这个就是EI Capitan系统的其中一个叫做System Integrity Protection的新feature造成的。新系统把/usr/local 改成了只读权限，后面几个问题基本上都是由于这个新feature造成的。虽然可以通过重启后，按住command＋R启动，在里面的命令行下输入“csrutil disable”来关闭，但是谁又能保证不会影响以后的更新功能呢，所以能不关闭就不关吧。


那么，我们再次使用brew doctor功能， 系统会提示， 要修改一些权限来修正brew。

	sudo chown -R $(whoami) /usr/local
	sudo chown -R $(whoami):admin /usr/local

通过上面2个命令，就可以修正brew的问题啦， 现在运行brew update已经可以正常更新了。


#### 3.Ruby的gem问题：

类似上面同样的问题，gem使用时也出现了问题，在google之后决定用这个样的方式解决：

	在~/.gemrc文件(如果没有请自己创建)中加入：
		gem: -n /usr/local/bin

很简单， 这样gem也可以用啦， 小弟因为前端用到sass，所以gem已经可以安装sass啦。


#### 4.celery的问题：

项目用的是django框架，里面用到django－celery， 在启动的时候这个库又问题， google了一下，除了关闭SIP以外，貌似还没发现什么很好的解决办法。 brew安装的python听说不稳定。所以还是用virtualenv好了，反正本来也有用这个来切换旧版本的django环境，用了这个果然就好了。不过后面还会继续研究怎么不用virtualenv解决，就先到这吧，有了方案再来补上。

#### 5.3指点击查询翻译的手势不能用了:

升级后系统默认关掉了这个功能, 要在设置里重新打开. 这个功能感觉很好用啊, 不明白为什么新系统要默认关闭. 总之新系统升级后并没有感觉比yosemite 好用,有时候感觉变卡了. 希望苹果尽快优化优化, 现在已经有很多人吐槽苹果现在软件质量越来越不行了.