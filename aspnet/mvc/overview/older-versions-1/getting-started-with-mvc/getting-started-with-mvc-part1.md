---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
title: "ASP.NET MVC 简介 |Microsoft 文档"
author: shanselman
description: "这是初学者本教程介绍 ASP.NET MVC 的基础知识。 创建一个简单的 web 应用程序读取和写入数据库中。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: bf4a1c19-0a94-4208-b268-a96ddcf26946
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
msc.type: authoredcontent
ms.openlocfilehash: 08c30f4aab77bff64ed3ab874d13cc5dc863fc99
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
<a name="intro-to-aspnet-mvc"></a>ASP.NET MVC 简介
====================
通过[Scott Hanselman](https://github.com/shanselman)

> > [!NOTE]
> > 本教程中是否可用的更新的版本[此处](../../getting-started/introduction/getting-started.md)使用[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)。 新的教程使用 ASP.NET MVC 5，通过本教程提供了许多改进。
> 
> 
> 这是初学者本教程介绍 ASP.NET MVC 的基础知识。 将创建一个简单的 web 应用程序读取和写入数据库中。 请访问[ASP.NET MVC 学习中心](../../../index.md)若要查找其他 ASP.NET MVC 教程和示例。


让我们来制作我们第一个 ASP.NET MVC Web 应用程序使用[Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/)。 我们将使少量影片列表应用程序，让我们将创建并列出电影。

## <a name="what-youll-build"></a>你将生成

以下是你将生成的应用程序的两个屏幕快照。 你将有一个简单表的影片的各个列。

[![影片列表-Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)

然后就可以创建的窗体以便我们可以将电影添加到列表。

[![创建电影-Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)

## <a name="skills-youll-learn"></a>你将学习的技能

本教程将教您生成使用 Visual Studio ASP.NET MVC Web 应用程序的基础知识。 你将学习：

- 如何创建新的 ASP.NET MVC 项目
- 如何使用 SQL Server 创建新数据库
- 如何创建 ASP.NET MVC 控制器和视图
- 如何检索和显示数据
- 如何编辑数据并启用数据验证
- 如何更新数据库架构

## <a name="get-started"></a>开始操作

通过从开始屏幕中运行 Visual Web Developer 2010 Express （我将称它"VWD"从现在起），然后选择新项目启动。

Visual Web Developer 是一个 IDE 或集成开发环境。 就像使用 Microsoft Word 编写文档，你将使用 IDE 来创建应用程序。 没有到你，以及你也可能使用了到选择的文件的菜单显示各个可用的选项顶部工具栏 |新项目。

[![Microsoft Visual Web Developer 2010 速成版](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)

## <a name="creating-your-first-application"></a>创建第一个应用程序

你可以创建使用 Visual Basic 或 Visual C# 应用程序。 现在，选择 Visual C# 在左侧，然后选择"ASP.NET MVC 2 Web 应用程序"。 将你的项目"电影"，然后单击确定。

[![新项目](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)

在右侧是应用程序中显示所有文件和文件夹解决方案资源管理器。 在中间的大窗口是你在其中编辑你的代码并花费大部分时间。 因此，必须运行的应用程序现在不执行任何操作的情况下，visual Studio 刚创建的 ASP.NET MVC 项目使用默认模板 ！ 这是简单的"Hello World ！ 项目，然后它是为我们的应用程序启动的好时机。

[![Microsoft Visual Web Developer 2010 速成版](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)

选择到工具栏的"播放"按钮。

![开始调试](getting-started-with-mvc-part1/_static/image11.png)

它是指向将编译你的程序并在 web 浏览器中启动你的应用程序的权限的绿色箭头。

*注意： 你可以改为按你键盘上的 f5 键或选择调试-&gt;从"调试"菜单开始调试。*

这将导致 Visual Web Developer 启动开发 web 服务器并运行 web 应用程序 （没有任何配置或手动启用此功能所需的步骤）。 然后，它将启动浏览器，并将其配置为浏览应用程序的主页。 注意下面，浏览器地址栏将显示为"localhost"，并不类似 example.com。这是因为 localhost 始终指向你自己的本地计算机-在这种情况下运行我们刚生成的应用程序。

[![主页](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)

现成的此默认模板提供了您这两页访问和基本的登录页。 让我们更改此应用程序的工作原理，并了解得有点 ASP.NET MVC 在进程中。 关闭浏览器，并允许更改某些代码。

>[!div class="step-by-step"]
[下一篇](getting-started-with-mvc-part2.md)
