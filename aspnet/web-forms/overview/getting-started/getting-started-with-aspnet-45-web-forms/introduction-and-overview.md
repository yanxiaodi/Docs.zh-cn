---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: "Getting Started with ASP.NET 4.5 Web 窗体和 Visual Studio 2013 |Microsoft 文档"
author: Erikre
description: "此分步教程系列将教您构建使用 ASP.NET 4.5 和 Microsoft Visual Studio 截止的 ASP.NET Web 窗体应用程序的基础知识..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: 6ae398f94c0ac3872601bdc8fd935f6d285793db
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2013"></a>ASP.NET 4.5 Web 窗体和 Visual Studio 2013 入门
====================
通过[艾力克 Reitan](https://github.com/Erikre)

[下载 Wingtip Toys 示例项目 (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)或[下载电子书 (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> 此分步教程系列将教您构建使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for Web 的 ASP.NET Web 窗体应用程序的基础知识。 [ASP.NET Web 窗体测验](http://quizapp.cloudapp.net/?quiz=ASP.NET)  
> 测试您的知识，并通过采用 ASP.NET Web 窗体 Quiz 强调关键概念。 此测验专门从包含在此教程系列中的内容。 测验中的每个问题提供的附加指导的链接以及解释。


## <a name="introduction"></a>介绍

这一系列的教程将指导您完成创建针对 Web 和 ASP.NET 4.5 使用 Visual Studio Express 2013 的 ASP.NET Web 窗体应用程序所需的步骤。

应用程序将创建名为**Wingtip Toys**。 它是销售项在线应用商店前端网站的简化的示例。 本系列教程重点介绍 ASP.NET 4.5 中提供的新功能。

注释是欢迎使用和我们将使努力地更新本系列教程基于你的建议。

### <a name="download-completed-project"></a>已完成的下载项目

你可以下载的 C# 项目中包含已完成的教程。

- [Getting Started with ASP.NET 4.5 Web 窗体和 Visual Studio 2013-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)

### <a name="review-the-content-by-taking-the-related-aspnet-web-forms-quiz"></a>通过执行相关的 ASP.NET Web 窗体测验查看的内容

完成本教程后，测试您的知识，并通过采用强调关键概念[ASP.NET Web 窗体 Quiz](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001)。 此测验专门从包含在此教程系列中的内容。 测验中的每个问题提供的附加指导的链接以及解释。

- [ASP.NET Web 窗体测验](http://quizapp.cloudapp.net/?quiz=ASP.NET)

### <a name="audience"></a>读者

本系列教程的目标的受众是经验丰富的开发人员不熟悉 ASP.NET Web 窗体。 开发人员感兴趣本系列教程应具有以下几个方面：

- 熟悉对象面向编程 (OOP) 语言
- 熟悉 Web 开发概念 (HTML、 CSS、 JavaScript)
- 熟悉关系数据库概念
- 熟悉 n 层体系结构概念

如果你有兴趣查看上面列出的区域，请考虑查看以下内容：

- [Visual C# 入门](https://msdn.microsoft.com/library/a72418yk.aspx)
- [Web 开发](https://msdn.microsoft.com/beginner/bb308760.aspx)， [HTML、 CSS、 JavaScript、 SQL、 PHP、 JQuery](http://w3schools.com/)
- [关系数据库](http://en.wikipedia.org/wiki/Relational_database)
- [多层体系结构](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a>应用程序功能

显示此序列中的 ASP.NET Web 窗体功能包括：

- Web 应用程序项目 （而不是网站项目）
- Web Forms — Web 窗体
- 主控页配置
- Bootstrap
- 实体框架代码优先，LocalDB
- 请求验证
- 强类型数据控件模型绑定，数据注释，并且值提供程序
- SSL 和 OAuth
- ASP.NET 标识、 配置和授权
- 非介入式验证
- 路由
- ASP.NET 错误处理

### <a name="application-scenarios-and-tasks"></a>应用程序方案和任务

演示本系列中的任务包括：

- 创建、 查看和运行新的项目
- 创建数据库结构
- 初始化和数据库进行种子设定
- 自定义 UI 使用样式、 图形和母版页
- 添加页面和导航
- 显示菜单的详细信息和产品数据
- 创建购物车
- 添加 SSL 和 OAuth 支持
- 添加付款方式
- 包括的管理员角色和用户访问应用程序
- 限制对特定页和文件夹的访问
- 将文件上载到 web 应用程序
- 实现输入的验证
- 注册 web 应用程序的路由
- 实现错误处理和错误日志记录

## <a name="overview"></a>概述

如果你不熟悉 ASP.NET Web 窗体，但同时具备编程概念的熟悉，你可以右教程。 如果你已熟悉 ASP.NET Web 窗体，可通过 ASP.NET 4.5 中提供的新功能获益从本教程系列。 如果你熟悉编程概念和 ASP.NET Web 窗体，请参阅 Web 窗体中提供的其他教程[入门](../../../index.md)ASP.NET Web 站点上的部分。

特定于**最新**ASP.NET 4.5 功能在此 Web 窗体中的系列教程包括以下：

- 用于创建一个简单的 UI 项目提供[支持多个 ASP.NET 框架](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add)（Web 窗体、 MVC 和 Web API）。
- [Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap)，布局和主题的框架，可提供响应式设计和主题功能。
- [ASP.NET 标识](../../../../identity/index.md)，新的 ASP.NET 成员资格系统中所有的 ASP.NET 框架和工作原理相同适用于 web 托管非 IIS 的软件。
- [实体框架 6](https://msdn.microsoft.com/data/ef.aspx)，更新到实体框架，使你检索和操作数据作为强类型化对象，访问数据以异步方式处理暂时性连接故障，以及记录 SQL 语句。

ASP.NET 4.5 功能的完整列表，请参阅[ASP.NET 和 Web Tools for Visual Studio 2013 发行说明](../../../../visual-studio/overview/2013/release-notes.md)。

### <a name="the-wingtip-toys-sample-application"></a>Wingtip Toys 示例应用程序

以下屏幕截图提供将在本系列教程中创建 ASP.NET Web 窗体应用程序的快速视图。 从 Visual Studio Express 2013 for Web 中运行应用程序时，你将看到以下 web 主页。

![Wingtip Toys 的默认页](introduction-and-overview/_static/image1.png)

你可以注册为新用户，或现有的用户身份登录。 导航通过从数据库检索可用的产品提供顶部每个产品类别。

通过选择的产品链接，你将能够查看所有可用产品的列表。

![Wingtip Toys-产品](introduction-and-overview/_static/image2.png)

你还可以通过选择列出的产品中的任何查看各个产品详细信息。

![Wingtip Toys 的产品详细信息](introduction-and-overview/_static/image3.png)

作为用户，可以注册和登录使用 Web 窗体模板的默认功能。 本教程还介绍了如何使用现有 Gmail 帐户登录。 此外，你可以登录以管理员身份添加和从数据库中删除产品。

![Wingtip Toys-登录](introduction-and-overview/_static/image4.png)

以用户身份登录，你可以将产品添加到购物车和与 PayPal 签出。 请注意此示例应用程序旨在 PayPal 的开发人员沙盒起作用。 没有实际 money 事务会发生。

![Wingtip Toys 的购物车](introduction-and-overview/_static/image5.png)

PayPal 将确认你的帐户、 顺序和付款信息。

![Wingtip Toys-PayPal](introduction-and-overview/_static/image6.png)

返回从 PayPal 后, 可以查看并完成你的订单。

![Wingtip Toys-订单审核](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a>系统必备

在开始之前，请确保你已在计算机上安装以下软件：

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs)或[Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web)。 自动安装.NET Framework。

本系列教程使用 Microsoft Visual Studio Express 2013 for Web。 可以使用 Microsoft Visual Studio Express 2013 for Web，或者 Microsoft Visual Studio 2013 来完成本教程系列。

> [!NOTE] 
> 
> Microsoft Visual Studio 2013 和 Microsoft Visual Studio Express 2013 for Web 将通常被称为 Visual Studio 在本教程系列整个。


如果你已安装了 Visual Studio 版本，则安装过程将安装 Visual Studio 2013 或 Microsoft Visual Studio Express 2013 for Web 旁边的现有版本。 在早期版本中创建的网站中可以在 Visual Studio 2013 中打开，并继续在以前版本中打开。

> [!NOTE] 
> 
> 本演练假定你所选*Web 开发*设置的集合，首次启动 Visual Studio。 有关详细信息，请参阅[如何： 选择 Web 开发环境设置](https://msdn.microsoft.com/library/ff521558.aspx)。


## <a name="download-the-sample-application"></a>下载示例应用程序

安装必备项之后, 你就可以开始创建新的 Web 项目显示在本教程系列。 如果你想要进行**（可选)**运行本系列教程创建示例应用程序，你可以从网站下载它 MSDN 示例。 此下载包含以下项目：

- 中的示例应用程序*WingtipToys*文件夹。
- 用于创建示例应用程序中的资源*WingtipToys 资产*文件夹中的*WingtipToys*文件夹。

#### <a name="download-the-file-from-msdn-samples-site"></a>从 MSDN 示例站点下载文件：

[Getting Started with ASP.NET 4.5 Web 窗体和 Visual Studio 2013-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) 

下载*.zip*文件。 若要查看已完成本教程系列创建的项目中，找到并选择*C#*文件夹中的*.zip*文件。 保存*C#* folderto 使用以使用 Visual Studio 2013 项目的文件夹。 默认情况下，Visual Studio 2013 项目文件夹如下所示：

**C:\Users\*****&lt;username&gt;*****\Documents\Visual Studio 2013\Projects**

重命名***C#***文件夹***WingtipToys***。

> [!NOTE]
> 如果你已有一个名为文件夹*WingtipToys*在项目文件夹中，暂时重命名该现有文件夹在重命名之前*C#*文件夹*WingtipToys*。


若要运行已完成的项目，打开*WingtipToys*文件夹并双击*WingtipToys.sln*文件。 Visual Studio 2013 将打开的项目。 接下来，右键单击*Default.aspx*文件位于解决方案资源管理器窗口，并单击右键单击菜单中浏览器中查看。

### <a name="tutorial-support-and-comments"></a>教程支持和注释

使用随附的问题解答部分[Getting Started with ASP.NET 4.5 Web 窗体和 Visual Studio 2013-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) 的任何问题或注释的示例。

对本系列教程的注释都欢迎，并更新本系列教程时将保持尽一切可能要考虑到帐户更正或建议的教程的注释中提供的改进。

在开发期间，发生错误时，或者如果网站无法正常运行，错误消息可能的问题来源授予复杂线索，或可能不说明如何修复此错误。 若要帮助你解决问题的一些常见方案，你可以使用[ASP.NET 论坛](https://forums.asp.net/)或附带的问题解答节[Getting Started with ASP.NET 4.5 Web 窗体和 Visual Studio 2013-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) 示例。 如果你收到如下错误消息，或当你完成本教程的内容不起作用，请务必检查上述位置。

>[!div class="step-by-step"]
[下一篇](create-the-project.md)
