---
layout: post
title: Web页面加载远程Silverlight app遇到的问题  
date: 2015-02-05 16:32:51
description: Web页面加载远程Silverlight app遇到的问题备忘
headline:
category: deployment
tags: [silverlight crossdomain ]
comments: true
mathjax:
---
###概述

今天在编写完公司项目的one key start部署脚本后，有个新需求修改，这个需求是将原本项目里继承的一个silverlight app从本地静态文件目录里移到另外的静态文件服务器上。本以为没什么问题，因为之前已经解决过silverlight的跨域访问api的问题。但实际运行中发生了这样的情况：


###问题描述

silverlight app在html页面中无法正常显示， 显示成一片空白，但是鼠标右键点击silverlight app的区域可以显示silverlight的环境时正常的。


在google chrome的dev tools里无法捕捉到get silverlight .xap包的请求， 换成ie debug看到请求状态显示已中断。


###解决方案

经过一番google，找到了一些资料， 但是大多数都是在说跨域访问的问题， 而不是跨域加载造成的问题。 后来在经过一些无效的尝试后， 终于找到了解决办法。


#####1.修改silverlight app的AppManifest.xml，将"ExternalCallersFromCrossDomain"设置为"ScriptableOnly":

	<Deployment xmlns="http://schemas.microsoft.com/client/2007/deployment"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            ExternalCallersFromCrossDomain="ScriptableOnly">
    	<Deployment.Parts>
    	</Deployment.Parts>
	</Deployment>


这里是设置silverlight app的访问级别，ExternalCallersFromCrossDomain属性是用来限制跨域silverlight的访问级别。（在同域的情况下无效）


详情参考：[msdn doc 关于 ExternalCallersFromCrossDomain](https://msdn.microsoft.com/en-us/library/system.windows.deployment.externalcallersfromcrossdomain(v=vs.95).aspx?cs-save-lang=1&cs-lang=csharp#code-snippet-1)

#####2.修改html页面中加载xap包的部分， 增加enableHtmlAccess参数的配置:

	<param name="enableHtmlAccess" value="true" />


这个属性在同域的情况下默认是true，但是在跨域的情况下默认为false。


详情参考：[msnd doc 关于 enableHtmlAccess](https://msdn.microsoft.com/en-us/library/cc838264(VS.95).aspx)


#####3.在nginx的配置文件中增加对应的mime type的配置:

	types{
		application/x-silverlight-app xap;
	}


这一步非常关键，在用ie debug的时候可以看到请求xap包返回的contentType是application／octet-stream二进制流，这显然是不对的。后来google发现silverlight app每次会检测这个header，必须是application/x-silverlight-app才能正常显示。修改完后reload和restart nginx，silverlight app就可以正常显示了。


当然，要确定完全没问题的话， 还是重新测试一下整个silverlight app里的流程吧。

