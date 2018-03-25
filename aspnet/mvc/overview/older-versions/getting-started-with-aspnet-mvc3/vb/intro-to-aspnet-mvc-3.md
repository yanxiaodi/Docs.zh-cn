---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/intro-to-aspnet-mvc-3
title: "ASP.NET MVC 3 (VB) 简介 |Microsoft 文档"
author: Rick-Anderson
description: "本教程将教您构建使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，这是一个 ASP.NET MVC Web 应用程序的基础知识..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: a1b3d789-93b4-487f-b90d-80c9c9b4f8fa
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: f0717e8d635818322be8b3242efe756a20351340
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="intro-to-aspnet-mvc-3-vb"></a>ASP.NET MVC 3 (VB) 简介
====================
通过[Rick Anderson](https://github.com/Rick-Anderson)

> 本教程将教您构建使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，这是 Microsoft Visual Studio 的免费版本的 ASP.NET MVC Web 应用程序的基础知识。 在开始之前，请确保已安装下面列出的先决条件。 你可以通过单击以下链接安装所有这些： [Web 平台安装程序](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)。 或者，你可以单独安装系统必备组件，使用以下链接：
> 
> - [Visual Studio Web Developer Express SP1 系统必备](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools 更新](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)（运行时 + 工具支持）
> 
> 如果你正在使用 Visual Studio 2010，而不 Visual Web Developer 2010，通过单击以下链接安装必备组件： [Visual Studio 2010 系统必备组件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)。
> 
> Visual Web Developer 项目与 VB.NET 的源代码是可以附带本主题。 [下载 VB.NET 版本](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)。 如果你愿意 C#，切换到[C# 版本](../cs/intro-to-aspnet-mvc-3.md)本教程。


本教程将教您构建使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，这是 Microsoft Visual Studio 的免费版本的 ASP.NET MVC Web 应用程序的基础知识。 在开始之前，请确保已安装下面列出的先决条件。 你可以通过单击以下链接安装所有这些： [Web 平台安装程序](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)。 或者，你可以单独安装系统必备组件，使用以下链接：

- [Visual Studio Web Developer Express SP1 系统必备](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
- [ASP.NET MVC 3 Tools 更新](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)（运行时 + 工具支持）

如果你正在使用 Visual Studio 2010，而不 Visual Web Developer 2010，通过单击以下链接安装必备组件： [Visual Studio 2010 系统必备组件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)。

Visual Web Developer 项目与 VB 的源代码是可以附带本主题。 [下载 VB 版本此处](https://code.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=14824)。 如果你愿意 CSharp，切换到[CSharp 版本](../cs/intro-to-aspnet-mvc-3.md)本教程。

## <a name="what-youll-build"></a>你将生成

你将实现简单的影片列表应用程序支持创建、 编辑和列出数据库中的影片。 以下是你将生成的应用程序的两个屏幕快照。 它包括一个显示从数据库的影片列表页：

[![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image2.png)](intro-to-aspnet-mvc-3/_static/image1.png)

应用程序还可以添加、 编辑和删除电影，以及有关个别权限，请参阅详细信息。 所有数据输入应用场景都包括验证来确保在数据库中存储的数据正确。

[![CreateFormSo](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="skills-youll-learn"></a>你将学习的技能

下面是你将要掌握的内容：

- 如何创建新的 ASP.NET MVC 项目
- 如何创建新的数据库使用实体框架代码的第一个
- 如何创建 ASP.NET MVC 控制器和视图
- 如何检索和显示数据
- 如何编辑数据并启用数据验证

## <a name="getting-started"></a>入门

通过运行 Visual Web Developer 2010 Express (简称的"VWD") 启动，然后选择**新项目**从**启动**页。

Visual Web Developer 是一个 IDE 或集成的开发环境。 就像使用 Microsoft Word 编写文档，你将使用 IDE 来创建应用程序。 在 Visual Web Developer 是向你显示各个可用的选项顶部的工具栏。 此外还有一个菜单，提供另一种方法在 IDE 中执行任务。 (例如，而不是选择**新项目**从**启动**页上，你可以使用菜单并选择**文件** &gt; **新项目**.)

[![](intro-to-aspnet-mvc-3/_static/image6.png)](intro-to-aspnet-mvc-3/_static/image5.png)

## <a name="creating-your-first-application"></a>创建第一个应用程序

你可以创建使用你选择的 Visual Basic 或 Visual C# 作为编程语言的应用程序。 对于本教程中，在左侧，选择 Visual Basic，然后选择**ASP.NET MVC 3 Web 应用程序**。 将你的项目"MvcMovie"，然后单击**确定**。

![1NewMVCproj_sm](intro-to-aspnet-mvc-3/_static/image7.png)

在**新建 ASP.NET MVC 3 项目**对话框中，选择**Internet 应用程序**。 保留**Razor**作为默认视图引擎。

![1InternetAppRazor_SM](intro-to-aspnet-mvc-3/_static/image8.png)

单击“确定”。 因此，必须运行的应用程序现在不执行任何操作的情况下，visual Web Developer 刚创建的 ASP.NET MVC 项目使用默认模板 ！ 这是简单"Hello World ！" 项目和它的启动你的应用程序的好时机。

[![](intro-to-aspnet-mvc-3/_static/image10.png)](intro-to-aspnet-mvc-3/_static/image9.png)

从“调试”菜单中选择“启动调试”。

![](intro-to-aspnet-mvc-3/_static/image11.png)

请注意，键盘快捷方式启动调试 F5。

F5 会导致 Visual Web Developer 启动开发 web 服务器并运行 web 应用程序。 然后，VWD 启动浏览器，并打开应用程序的主页。 请注意，浏览器的地址栏显示`localhost`和不类似于`example.com`。 这是因为`localhost`始终指向你自己本地的计算机，在这种情况下运行你刚才生成应用程序。 当 VWD 运行 web 项目时，随机端口用于项目。 在下图中，随机端口号是 43246。 你的项目将可能使用不同的端口号。

![](intro-to-aspnet-mvc-3/_static/image12.png)

现成的此默认模板提供了您这两页访问和基本的登录页。 让我们更改此应用程序的工作原理，并了解得有点 ASP.NET MVC 在进程中。 关闭你的浏览器并让我们更改某些代码。

>[!div class="step-by-step"]
[下一篇](adding-a-controller.md)
