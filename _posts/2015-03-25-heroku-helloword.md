---
layout: post
title: "试玩heroku之nodejs app"
date: 2015-03-25 17:28:00
description: 在heroku上试着部署了一下nodejs app
headline:
category: deployment
tags: [ heroku nodejs ]
comments: true
mathjax:
---
###试玩heroku

一直看美国同事在heroku上部署部分项目, 今天下午正好有点空, 就注册了个账号玩了一下.


官方提供了一个非常易于上手的getting started:

####1.安装heroku tool
这个很简单就是下载对应系统的安装包咯.


####2.login
在命令行中执行: 

	heroku login

登陆完成后会提示:

	Authentication successful.

期间遇到几次失败, 估计国内连国外,你懂的. 多试了好几次才成功.


####3.下载官方提供的一个sample app
如果你自己有nodejs app直接用也可以, 我只是先试试手, 所以就用官方的啦, 毕竟很小,push神马的速度很快.

	git clone https://github.com/heroku/node-js-getting-started.git
	cd node-js-getting-started

####4.部署项目
在上一步进入到项目根目录后,执行:
	
	heroku create

这个命令是创建一个叫做heroku的remote git, 并和local git关联起来. 并会生成一个名字类似"calm-spire-3034"这样的随机app名称. 如果想换自己想要的名称可以执行

	heroku apps:rename ****

接下来就是上传当地项目到服务器上, 执行:

	git push heroku master

可以看到这些操作跟普通的github项目差不多, 唯一不同的是push到heroku的master上.到此,简单的部署已经完成了.

####5.确认是否成功
执行:

	 heroku ps:scale web=1

来查看刚才的实例有没有运行成功. 另外可以直接访问对应的url来确认是否运行成功,有个快捷命令是:

	heroku open

这个命令会帮你自动打开浏览器访问刚才部署好的web.

####6.查看log
	
	heroku logs --tail

####7.Procfile 配置文件
在项目根目录中创建一个名字叫"Procfile"的文件, 里面写上项目执行的命令, 比如"web: node index.js" 这样的话push了代码后项目就会自动运行啦.


--------------------------------------------------------------------------------------


是不是很简单?


另外heroku还提供了一个叫做foreman的工具可以用于本地调试程序哦. 


执行:

	foreman start web

就可以在本地启动啦~ 调试完了再提交到heroku服务器上.修改代码后提交, 基本和github的操作模式一样哦. 另外heroku还提供使用第三方云服务的插件来用哦,比如使用postgresql等更多的内容可以从heroku的document里面学到哦, 等有空再深入的玩一下吧.


