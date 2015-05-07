---
layout: post
title: 在react native项目里添加facebook login功能时遇到的问题
date: 2015-05-06 16:32:51
description: react native项目里添加facebook login功能时遇到的问题备忘
headline:
category: react-native
tags: [react-native ios facebook]
comments: true
mathjax:
---
###问题

今天在自己的react－native项目里面添加facebook帐号登录功能，在按照facebook的getting started步骤里有一步是在Info.plist里面添加3个key－value，其中有一个key叫做"URL types",有个child叫做“URL Schemes”,但是所有步骤做完后，运行代码点击login按钮，发现一直报错:

	"fb********** is not registered as a URL scheme. 
	Please add it in your Info.plist"


###解决方案

不要再Xcode里面直接填写这些key－value,而是用sublime等文本编辑器打开Info.plist文件,在里面填入以下内容:

	<key>CFBundleURLTypes</key>
	<array>
		<dict>
			<key>CFBundleURLSchemes</key>
			<array>
				<string>fb***************</string>
			</array>
		</dict>
	</array>


看到没，实际的key并不是"URL types",而是"CFBundleURLTypes". 


改完保存后,在Xcode ui里面查看key显示为"URL types".坑爹吧~


现在重新build一下，就可以正常login啦。




