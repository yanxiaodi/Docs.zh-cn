---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
title: "第 1 部分: 概述和文件-> 新建项目 |Microsoft 文档"
author: jongalloway
description: "本系列教程详细介绍所有生成 ASP.NET MVC 音乐商店示例应用程序所采取的步骤。 第 1 部分介绍如何概述和文件-> 新项目。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: bd356ca3-5bdb-4067-9dac-c9e9923a86e8
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
msc.type: authoredcontent
ms.openlocfilehash: 1e3373a21c7d1766cfad390a7ba68b1363d8d895
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="part-1-overview-and-file-new-project"></a>第 1 部分: 概述和文件-> 新建项目
====================
通过[Jon Galloway](https://github.com/jongalloway)

> MVC 音乐商店是，从而引入并说明如何使用 ASP.NET MVC 和 Visual Studio 进行 web 开发分步教程应用程序。  
>   
> MVC 音乐商店是一个轻型示例存储区实现，该类销售音乐集联机，并实现基本的站点管理、 用户登录，和购物车功能。  
>   
> 本系列教程详细介绍所有生成 ASP.NET MVC 音乐商店示例应用程序所采取的步骤。 第 1 部分介绍概述和文件-&gt;新项目。


## <a name="overview"></a>概述

MVC 音乐商店是，从而引入并说明如何使用 ASP.NET MVC 和 Visual Web Developer web 开发的分步教程应用程序。 我们将启动速度慢，因此初学者级别 web 开发体验没什么关系。

我们将构建的应用程序是简单的音乐商店。 有向应用程序的三个主要部分： 购物、 结帐和管理。

![](mvc-music-store-part-1/_static/image1.jpg)

访问者可以浏览按风格的专辑：

![](mvc-music-store-part-1/_static/image2.jpg)

它们可以查看单个唱片集并将其添加到购物车：

![](mvc-music-store-part-1/_static/image3.jpg)

它们可以查看其购物车，删除它们不再需要的任何项：

![](mvc-music-store-part-1/_static/image4.jpg)

在继续结帐将提示他们登录或注册用户帐户。

![](mvc-music-store-part-1/_static/image1.png)

![](mvc-music-store-part-1/_static/image2.png)

创建帐户之后，它们可以通过填写传送和付款信息完成顺序。 若要为简单起见，我们正在运行了令人惊叹的升级： 所有内容都是免费的如果他们输入促销代码"免费"！

![](mvc-music-store-part-1/_static/image5.jpg)

排序之后, 他们将看到简单确认屏幕：

![](mvc-music-store-part-1/_static/image6.jpg)

除了客户 faceing 页中，我们将生成一个管理员部分，其中显示的专辑，管理员可以从中创建、 编辑、 列表，并删除唱片集：

![](mvc-music-store-part-1/_static/image7.jpg)

## <a name="1-file--gt-new-project"></a>1.文件-&gt;新项目

### <a name="installing-the-software"></a>安装软件

本教程将会开始通过创建使用免费 Visual Web Developer 2010 Express （这是可用），为新的 ASP.NET MVC 3 项目，然后我们将以增量方式添加功能可以创建一个完整的正常运行应用程序。 与此同时，我们将介绍数据库访问、 窗体发布方案、 数据验证使用母版页提供一致的页面布局使用 AJAX 页更新和验证、 用户登录名和详细信息。

你可以遵循步骤，也可以在下载完成的应用程序从[MVC 音乐商店](https://github.com/evilDave/MVC-Music-Store)。

可以使用 Visual Studio 2010 SP1 或 Visual Web Developer 2010 Express SP1 （免费版本的 Visual Studio 2010） 生成应用程序。 我们将使用 SQL Server Compact （也可用） 来承载数据库。 在开始之前，请确保已安装下面列出的先决条件。


- [Visual Studio Web Developer Express SP1 系统必备组件]
- [ASP.NET MVC 3 Tools 更新]
- [SQL Server Compact 4.0]-包括运行时和工具支持


### <a name="creating-a-new-aspnet-mvc-3-project"></a>创建新的 ASP.NET MVC 3 项目

我们将开始通过从在 Visual Web Developer 的文件菜单中选择"新建项目"。 这将显示新项目对话框。

![](mvc-music-store-part-1/_static/image5.png)

我们将选择的 Visual C#&gt; Web 模板组在左侧，然后在中间栏中选择"ASP.NET MVC 3 Web 应用程序"模板。 将命名你的项目 MvcMusicStore 并按确定按钮。

![](mvc-music-store-part-1/_static/image8.jpg)

这将显示辅助对话框，我们使我们的项目中的某些 MVC 特定设置。 选择以下项：

项目模板-选择空

查看引擎-选择 Razor

使用 HTML5 语义标记的检查

验证你的设置是如下所示，然后按确定按钮。

![](mvc-music-store-part-1/_static/image9.jpg)

这将创建我们的项目。 让我们看看已添加到我们在右侧的解决方案资源管理器的应用程序的文件夹。

![](mvc-music-store-part-1/_static/image10.jpg)

空的 MVC 3 模板并不完全为空 – 它将添加基本的文件夹结构：

![](mvc-music-store-part-1/_static/image6.png)

ASP.NET MVC 利用的一些基本的命名约定的文件夹名称：

| **文件夹** | **目的** |
| --- | --- |
| **/ 控制器** | 控制器响应输入从浏览器时，决定要处理的问题，并向用户返回响应的内容。 |
| **/ 视图** | 视图保存我们 UI 模板 |
| **/ 模型** | 模型保存和操作数据 |
| **/ 内容** | 此文件夹包含我们的图像、 CSS 和任何其他静态内容 |
| **/ 脚本** | 此文件夹包含我们 JavaScript 文件 |

因为默认情况下的 ASP.NET MVC framework 使用"配置的约定"方法，并使基于文件夹命名约定某些默认假设即使在空的 ASP.NET MVC 应用程序中包含这些文件夹。 例如，控制器看起来的 Views 文件夹中视图的默认情况下您无需显式指定此设置在代码中。 默认约定仍坚持减少的代码需要编写，还可使其便于其他开发人员了解你的项目。 我们将介绍这些约定详细构建我们的应用程序。

>[!div class="step-by-step"]
[下一篇](mvc-music-store-part-2.md)
