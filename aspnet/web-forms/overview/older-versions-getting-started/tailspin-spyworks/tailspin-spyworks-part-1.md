---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
title: "第 1 部分： 文件-> 新建项目 |Microsoft 文档"
author: JoeStagner
description: "本系列教程详细介绍所有生成 Tailspin Spyworks 示例应用程序所采取的步骤。 第 1 部分介绍概述和文件 / 新建项目。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 15d4652b-d5aa-4172-b186-2c7f96ba316d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
msc.type: authoredcontent
ms.openlocfilehash: bd840f9f3f5d723e6bc1bb35955a7770634e9483
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="part-1-file--new-project"></a>第 1 部分： 文件-> 新建项目
====================
通过[Joe stagner 将](https://github.com/JoeStagner)

> Tailspin Spyworks 演示创建在.NET 平台的功能强大、 可扩展应用程序是如何非常简单。 它演示如何使用 ASP.NET 4 中出色的新功能构建在线商店，包括购物、 结帐和管理关闭。
> 
> 本系列教程详细介绍所有生成 Tailspin Spyworks 示例应用程序所采取的步骤。 第 1 部分介绍概述和文件 / 新建项目。


## <a id="_Toc260221666"></a>概述

本教程是 ASP.NET WebForms 的简介。 我们将启动速度慢，因此初学者级别 web 开发体验没什么关系。

我们将构建的应用程序是一个简单的在线商店。

![](tailspin-spyworks-part-1/_static/image1.jpg)


访问者可以浏览按类别排列产品：

![](tailspin-spyworks-part-1/_static/image2.jpg)

它们可以查看单个产品，并将其添加到购物车：

![](tailspin-spyworks-part-1/_static/image3.jpg)

它们可以查看其购物车，删除它们不再需要的任何项：

![](tailspin-spyworks-part-1/_static/image4.jpg)

在继续结帐将提示他们到

![](tailspin-spyworks-part-1/_static/image5.jpg)

![](tailspin-spyworks-part-1/_static/image6.jpg)

排序之后, 他们将看到简单确认屏幕：

![](tailspin-spyworks-part-1/_static/image7.jpg)


我们将通过在 Visual Studio 2010 中，创建新的 ASP.NET WebForms 项目开始，我们将以增量方式添加功能可以创建一个完整的正常运行应用程序。 与此同时，我们将介绍数据库访问权限、 列表和网格视图、 数据更新页、 数据验证、 使用母版页提供一致的页面布局、 AJAX、 验证、 用户成员身份和的详细信息。

你可以遵循步骤，也可以在下载完成的应用程序从[http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)

你可以使用 Visual Studio 2010 或免费的 Visual Web Developer 2010 从[https://www.microsoft.com/express/Web/](https://www.microsoft.com/express/Web/)。 若要生成应用程序，可以使用 SQL Server 或可用 SQL Server Express 到主机数据库。

## <a id="_Toc260221667"></a>文件 / 新项目

我们将开始通过从 Visual Studio 中的文件菜单中选择新项目。 这将显示新项目对话框。

![](tailspin-spyworks-part-1/_static/image8.jpg)

我们将选择的 Visual C# / Web 模板组在左侧，，然后在中间栏中选择"ASP.NET Web 应用程序"模板。 将命名你的项目 TailspinSpyworks 并按确定按钮。

![](tailspin-spyworks-part-1/_static/image9.jpg)

这将创建我们的项目。 让我们看看我们在右侧的解决方案资源管理器的应用程序中包括的文件夹。

![](tailspin-spyworks-part-1/_static/image10.jpg)

空的解决方案不完全为空 – 它将添加基本的文件夹结构：

![](tailspin-spyworks-part-1/_static/image1.png)

请注意实现的 ASP.NET 4 默认项目模板的约定。

- "帐户"文件夹为 ASP 实现基本用户界面。NET 的成员资格子系统。
- "脚本"文件夹用作客户端 JavaScript 文件的存储库并核心 jQuery.js 文件都可用的默认值。
- "样式"文件夹用于组织我们网站的视觉对象 （CSS 样式表）

当我们按下 F5 以运行我们的应用程序和呈现 default.aspx 页时我们看到以下内容。

![](tailspin-spyworks-part-1/_static/image11.jpg)

我们第一个应用程序增强功能就是使用 CSS 类和关联的图像文件将呈现我们希望我们 Tailspin Spyworks 的应用程序 visual asthetics 替换默认 WebForms 模板中的 Style.css 文件。

这样做之后我们 default.aspx 页上呈现如下。

![](tailspin-spyworks-part-1/_static/image12.jpg)

请注意位于顶部的映像链接页以及已添加到母版页的菜单项的权限。 仅"登录"和"帐户"链接指向的存在 （由默认模板生成） 的页面，但我们将实现构建我们的应用程序的页面的其余部分。

我们还将将母版页重新定位到的样式目录。 虽然这是只是首选项它可能简化操作有点如果我们决定在将来执行我们的应用程序"skinable"。

在执行此我们将需要更改母版页后所有的.aspx 文件中的引用默认生成 ASP.NET WebForms 页。

>[!div class="step-by-step"]
[下一篇](tailspin-spyworks-part-2.md)
