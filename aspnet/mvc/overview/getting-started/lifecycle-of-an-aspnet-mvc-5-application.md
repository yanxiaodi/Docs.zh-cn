---
uid: mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
title: "ASP.NET MVC 5 应用程序生命周期 |Microsoft 文档"
author: cephalin
description: "下载 PDF 文档绘制图表的 ASP.NET MVC 5 应用程序生命周期。 此生命周期文档提供的 MVC 生命周期的高级视图..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/28/2014
ms.topic: article
ms.assetid: 9c1e3a75-b644-4480-8326-11300b1ec4b3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
msc.type: authoredcontent
ms.openlocfilehash: 50d58d10c11677fa72ede6a03e686cbde4cbae1d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="lifecycle-of-an-aspnet-mvc-5-application"></a>ASP.NET MVC 5 应用程序生命周期
====================
通过[Cephas Lin](https://github.com/cephalin)

[下载 PDF 文档](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf)

此处你可以下载的图表的生命周期的每个 ASP.NET MVC 5 应用程序，接收 HTTP 请求发送的 HTTP 响应回客户端的 PDF 文档。 它旨在为那些不熟悉 ASP.NET MVC 的教学工具，而且还作为那些需要深入了解应用程序的特定方面的参考。 PDF 文档具有以下功能：

- 相关[HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication.aspx)阶段以帮助你了解 MVC 集成到[ASP.NET 应用程序生命周期](https://msdn.microsoft.com/library/bb470252.aspx)。
- MVC 应用程序生命周期，，您可以从中了解每个 MVC 应用程序通过在请求处理管道中的主要阶段的高级视图。  
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image1.jpg)
- 显示向下钻取，到请求处理管道的详细信息的详细信息视图。 你可以比较高级视图和详细信息视图以查看如何生命周期的详细信息将收集到的各个阶段。 [下载 PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf)若要查看更大的视图。
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image2.jpg)
- 放置和用途的所有可重写方法[控制器](https://msdn.microsoft.com/library/system.web.mvc.controller.aspx)请求处理管道中的对象。 你可能或可能不需要重写任何一种方法，但务必要了解应用程序生命周期中的其角色，以便你可以在适当的生命周期阶段的预期的效果编写代码。
- 断了向上关系图显示如何调用每个筛选器类型 （身份验证、 授权、 操作和结果）。
- 链接到有用的文章或博客与感兴趣的详细信息视图的每个点。


## <a name="next-steps"></a>后续步骤

本文档是否满足您的需求？ 我们会十分感激你的反馈。 如果你有任何问题的 ASP.NET MVC 生命周期中你的应用程序， [Stackoverflow](http://stackoverflow.com/help)和[ASP.NET MVC 论坛](https://forums.asp.net/1146.aspx)是可以提出的极佳位置。 请按照[我](https://twitter.com/Cephas_MSFT)以便您可以获取我的最新教程上的更新在 twitter 上。
