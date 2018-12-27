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

今天在编写完公司项目的 one key start 部署脚本后，有个新需求修改，这个需求是将原本项目里继承的一个 silverlight app 从本地静态文件目录里移到另外的静态文件服务器上。本以为没什么问题，因为之前已经解决过 silverlight 的跨域访问 api 的问题。但实际运行中发生了这样的情况：

###问题描述

webserver 在服务器 A 上， silverlight app 的 xap 包放在静态文件服务器 B 上。

silverlight app 在 html 页面中无法正常显示， 显示成一片空白，但是鼠标右键点击 silverlight app 的区域可以显示 silverlight 的环境时正常的。

在 google chrome 的 dev tools 里无法捕捉到 get silverlight .xap 包的请求， 换成 ie debug 看到请求状态显示已中断。

###解决方案

经过一番 google，找到了一些资料， 但是大多数都是在说跨域访问的问题， 而不是跨域加载造成的问题。 后来在经过一些无效的尝试后， 终于找到了解决办法。

#####1.修改 silverlight app 的 AppManifest.xml，将"ExternalCallersFromCrossDomain"设置为"ScriptableOnly":

    <Deployment xmlns="https://schemas.microsoft.com/client/2007/deployment"
        xmlns:x="https://schemas.microsoft.com/winfx/2006/xaml"
            ExternalCallersFromCrossDomain="ScriptableOnly">
    	<Deployment.Parts>
    	</Deployment.Parts>
    </Deployment>

这里是设置 silverlight app 的访问级别，ExternalCallersFromCrossDomain 属性是用来限制跨域 silverlight 的访问级别。（在同域的情况下无效）

详情参考：[msdn doc 关于 ExternalCallersFromCrossDomain](<https://msdn.microsoft.com/en-us/library/system.windows.deployment.externalcallersfromcrossdomain(v=vs.95).aspx?cs-save-lang=1&cs-lang=csharp#code-snippet-1>)

#####2.修改 html 页面中加载 xap 包的部分， 增加 enableHtmlAccess 参数的配置:

    <param name="enableHtmlAccess" value="true" />

这个属性在同域的情况下默认是 true，但是在跨域的情况下默认为 false。

详情参考：[msnd doc 关于 enableHtmlAccess](<https://msdn.microsoft.com/en-us/library/cc838264(VS.95).aspx>)

#####3.在 nginx 的配置文件中增加对应的 mime type 的配置:

    types{
    	application/x-silverlight-app xap;
    }

这一步非常关键，在用 ie debug 的时候可以看到请求 xap 包返回的 contentType 是 application／octet-stream 二进制流，这显然是不对的。后来 google 发现 silverlight app 每次会检测这个 header，必须是 application/x-silverlight-app 才能正常显示。修改完后 reload 和 restart nginx，silverlight app 就可以正常显示了。

当然，要确定完全没问题的话， 还是重新测试一下整个 silverlight app 里的流程吧。
