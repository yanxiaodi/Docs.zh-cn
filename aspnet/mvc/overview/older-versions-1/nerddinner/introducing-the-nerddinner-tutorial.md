---
uid: mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
title: "引入 NerdDinner 教程 |Microsoft 文档"
author: shanselman
description: "若要了解新框架的最佳方法是生成一些。 本教程将指导完成如何构建使用 ASP.NE 的较小，但完成后，应用程序..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 397522d5-0402-4b94-b810-a2fb564f869d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
msc.type: authoredcontent
ms.openlocfilehash: 57eedb224e26867c78cc399b89f91b95f722074d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="introducing-the-nerddinner-tutorial"></a>引入 NerdDinner 教程
====================
通过[Scott Hanselman](https://github.com/shanselman)

[下载 PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> 若要了解新框架的最佳方法是生成一些。 本教程将指导完成如何生成一个较小，但完成，使用 ASP.NET MVC 1，应用程序，并介绍了一些背后的核心概念。
> 
> 如果你使用的 ASP.NET MVC 3，我们建议你遵循[获取启动使用 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或[MVC 音乐商店](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教程。


## <a name="nerddinner-tutorial"></a>NerdDinner 教程

若要了解新框架的最佳方法是生成一些。 本教程将指导完成如何生成一个较小，但完成，使用 ASP.NET MVC 应用程序，并介绍了一些背后的核心概念。

我们将生成应用程序称为"NerdDinner"。 NerdDinner 可以轻松地让人有机会查找和组织晚餐联机：

![](introducing-the-nerddinner-tutorial/_static/image1.png)

NerdDinner 使已注册的用户能够创建、 编辑和删除晚餐。 它会强制实施跨应用程序的一组一致的验证规则和业务规则：

![](introducing-the-nerddinner-tutorial/_static/image2.png)

访问者可以使用基于 AJAX 的映射来搜索即将到来晚餐正在挂起附近它们：

![](introducing-the-nerddinner-tutorial/_static/image3.png)

单击 dinner 会将他们到详细信息页，以了解更多相关信息：

![](introducing-the-nerddinner-tutorial/_static/image4.png)

如果他们感兴趣参加 dinner 他们可以登录，或在站点上注册：

![](introducing-the-nerddinner-tutorial/_static/image5.png)

然后，他们可以单击基于 AJAX 的回复链接参加会议事件：

![](introducing-the-nerddinner-tutorial/_static/image6.png)

![](introducing-the-nerddinner-tutorial/_static/image7.png)

### <a name="implementing-nerddinner"></a>实现 NerdDinner

我们将首先我们 NerdDinner 的应用程序使用的文件-&gt;Visual Studio 中的新项目命令以创建新的 ASP.NET MVC 项目。 然后，我们将以增量方式添加功能和功能。 此过程中，我们将介绍：

1. [如何创建新的 ASP.NET MVC 项目](# "创建新的 ASP.NET MVC 项目")
2. [如何创建数据库](# "创建数据库")
3. [如何生成一个具有业务规则验证模型](# "生成一个具有业务规则验证模型")
4. [如何使用控制器和视图来实现列表/详细 UI](# "使用控制器和视图，以实现详细信息列表/用户界面")
5. [如何提供 CRUD （创建、 读取、 更新、 删除） 数据窗体条目支持](# "提供 CRUD （创建、 读取、 更新、 删除） 数据窗体条目支持")
6. [如何使用 ViewData 和实现 ViewModel 类](# "使用 ViewData 和实现 ViewModel 类")
7. [如何重新使用 UI 使用母版页和它们](# "重用 UI 使用母版页和它们")
8. [如何实现高效数据分页](# "实现高效数据分页")
9. [如何保护应用程序使用身份验证和授权](# "安全应用程序使用身份验证和授权")
10. [如何使用 AJAX 提供动态更新](# "到提供动态更新使用 AJAX")
11. [如何使用 AJAX 实现映射方案](# "到实现映射情况下使用 AJAX")
12. [如何启用自动的单元测试](# "启用自动进行单元测试")

你可以构建你自己的 NerdDinner 副本从头通过完成每个步骤我们的本章中的演练。 或者，你可以下载完整的版本的源代码此处： [GitHub 上的 NerdDinner](https://github.com/AspNetMVPSamples/NerdDinner)。 你还可以选择还可以[下载本教程的免费 PDF 版](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)如果你想要阅读本教程脱机。

可以使用 Visual Studio 2008 或可用 Visual Web Developer 2008 速成版生成该应用程序。 可以为数据库使用 SQL Server 或可用的 SQL Server Express。

你可以安装 ASP.NET MVC、 Visual Web Developer 2008 速成版，和 SQL Server Express （所有免费） 使用的 V2 [Microsoft Web 平台安装程序](https://www.microsoft.com/web/downloads/platform.aspx)

### <a name="now-lets-get-started"></a>现在让我们开始吧...

现在，我们已介绍了 NerdDinner 是什么，让我们汇总我们袖子并编写一些代码。

我们将首先通过使用文件-&gt;Visual Studio 来创建 NerdDinner 应用程序中的新项目。

>[!div class="step-by-step"]
[下一篇](create-a-new-aspnet-mvc-project.md)
