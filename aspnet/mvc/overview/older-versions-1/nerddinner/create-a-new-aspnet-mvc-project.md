---
uid: mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
title: "创建新的 ASP.NET MVC 项目 |Microsoft 文档"
author: microsoft
description: "步骤 1 演示了如何将基本 NerdDinner 应用程序结构放在位置。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 7e0e9928-8fdc-4b74-9882-55fac0976628
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
msc.type: authoredcontent
ms.openlocfilehash: 4d30a6803b1478014a2afb814ac317df27394446
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
<a name="create-a-new-aspnet-mvc-project"></a>创建新的 ASP.NET MVC 项目
====================
by [Microsoft](https://github.com/microsoft)

[下载 PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> 这是一个免费的第 1 步["NerdDinner"应用程序教程](introducing-the-nerddinner-tutorial.md)，查找步程取如何生成一个较小，但完成，使用 ASP.NET MVC 1 web 应用程序。
> 
> 步骤 1 演示了如何将基本 NerdDinner 应用程序结构放在位置。
> 
> 如果你使用的 ASP.NET MVC 3，我们建议你遵循[获取启动使用 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或[MVC 音乐商店](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教程。


## <a name="nerddinner-step-1-file-gtnew-project"></a>NerdDinner 步骤 1： 文件-&gt;新项目

我们将首先我们 NerdDinner 的应用程序通过选择**文件-&gt;新项目**Visual Studio 2008 或可用 Visual Web Developer 2008 速成版中的菜单项。

这将显示"新建项目"对话框。 若要创建新的 ASP.NET MVC 应用程序，我们选择对话框左侧的"Web"节点，然后选择右侧的"ASP.NET MVC Web 应用程序"项目模板：

![](create-a-new-aspnet-mvc-project/_static/image1.png)

*重要说明： 请确保你已下载并安装 ASP.NET MVC-否则为它不会显示在新项目对话框。你可以使用的 V2 [Microsoft Web 平台安装程序](https://www.microsoft.com/web/downloads/platform.aspx)如果您尚未安装程序 (ASP.NET MVC 内可用时"Web 平台-&gt;框架和运行时"部分)。*

我们将将新的项目，我们将创建"NerdDinner"，然后单击"确定"按钮创建它。

当我们单击"确定"Visual Studio 将显示其他对话框，提示我们可以根据需要创建新应用程序的单元测试项目。 此单元测试项目使我们能够创建验证的功能和行为的我们的应用程序的自动的测试 (我们将介绍的内容如何更高版本在本教程中的待办事项)。

![](create-a-new-aspnet-mvc-project/_static/image2.png)

使用所有可用 ASP.NET MVC 单元测试项目模板在计算机上安装，填充中上述对话框的"测试框架"下拉列表。 可以为 NUnit、 MBUnit 和 XUnit 下载版本。 也支持内置的 Visual Studio 单元测试框架。

*注意： Visual Studio 单元测试框架时才可以使用 Visual Studio 2008 专业版和更高版本。如果你正在使用 VS 2008 标准版或 Visual Web Developer 2008 速成版将需要下载并安装针对 ASP.NET MVC NUnit、 MBUnit 或 XUnit 扩展，此对话框显示的顺序。如果没有安装任何测试框架，将不显示对话框。*

我们将使用我们创建的测试项目的默认"NerdDinner.Tests"名称，然后使用"Visual Studio 单元测试"框架选项。 当我们单击"正常"按钮 Visual Studio 将创建解决方案为我们具有在其中的一个用于我们的 web 应用程序，一个以进行单元测试的两个项目：

![](create-a-new-aspnet-mvc-project/_static/image3.png)

### <a name="examining-the-nerddinner-directory-structure"></a>检查 NerdDinner 目录结构

当使用 Visual Studio 创建新的 ASP.NET MVC 应用程序时，它会自动将大量的文件和目录添加到项目：

![](create-a-new-aspnet-mvc-project/_static/image4.png)

默认情况下的 ASP.NET MVC 项目具有六个顶级目录：

| **Directory** | **目的** |
| --- | --- |
| **/Controllers** | 处理 URL 请求的控制器类的放置位置 |
| **/Models** | 类表示和操作数据的放置位置 |
| **/Views** | 放置负责呈现输出的 UI 模板文件的位置 |
| **/Scripts** | JavaScript 库文件和脚本 (.js) 的放置位置 |
| **/Content** | CSS 和图像文件与其他非-动态/非-JavaScript 内容的放置位置 |
| **/App\_Data** | 存储数据文件在你想要读/写。 |

ASP.NET MVC 不需要此结构。 事实上，开发人员处理大型应用程序将通常对应用程序分区向上跨多个项目，以使其更易于管理 (例如： 数据模型类通常在一个单独的类库项目中从转 web 应用程序)。 默认项目结构，但是，提供我们可用于保持我们的应用程序主要关注干净的很好的默认目录约定。

当我们扩展 /Controllers 目录我们将找到 Visual Studio 添加默认情况下两个控制器类 – HomeController 和 AccountController – 到项目：

![](create-a-new-aspnet-mvc-project/_static/image5.png)

当我们扩展 /Views 目录时，我们将找到三个子目录 – /Home、 /Account /Shared – 以及其中的文件也已按默认添加到项目的多个模板：

![](create-a-new-aspnet-mvc-project/_static/image6.png)

当我们扩展 /Content 和 /Scripts 目录时，我们将找到用于在站点上，设置样式所有 HTML Site.css 文件，以及 JavaScript 库，可以使 ASP.NET AJAX 和 jQuery 支持应用程序中：

![](create-a-new-aspnet-mvc-project/_static/image7.png)

我们展开 NerdDinner.Tests 项目时，我们将会包含我们控制器类的单元测试的两个类：

![](create-a-new-aspnet-mvc-project/_static/image8.png)

为工作应用程序的完成，但主页上，有关页、 帐户注册/注销/登录信息页，和 （所有有线上移和现成的工作） 的未处理的错误页中，由 Visual Studio 添加这些默认文件提供我们的基本结构。

### <a name="running-the-nerddinner-application"></a>运行 NerdDinner 应用程序

我们可以通过选择运行该项目**调试-&gt;启动调试**或**调试-&gt;启动但不调试**菜单项：

![](create-a-new-aspnet-mvc-project/_static/image9.png)

这将启动内置 ASP.NET Web 服务器附带 Visual Studio 中，并运行我们的应用程序：

![](create-a-new-aspnet-mvc-project/_static/image10.png)

下面是我们新的项目的主页 (URL:"/") 运行时：

![](create-a-new-aspnet-mvc-project/_static/image11.png)

单击"关于"选项卡显示有关页面 (URL:"/ Home/有关"):

![](create-a-new-aspnet-mvc-project/_static/image12.png)

单击右上角的"登录"链接，我们将转到登录页 (URL:"/ 帐户/登录")

![](create-a-new-aspnet-mvc-project/_static/image13.png)

如果我们没有登录帐户，我们可以单击注册链接 (URL:"/ 帐户/注册") 创建一个：

![](create-a-new-aspnet-mvc-project/_static/image14.png)

代码以实现更高版本的主页中，有关和注销 / 注册我们创建我们的新项目时，已按默认添加功能。 我们将使用它作为我们的应用程序的起始点。

### <a name="testing-the-nerddinner-application"></a>测试 NerdDinner 应用程序

如果我们使用的专业版或更高版本的 Visual Studio 2008，我们可以使用内置的单元测试在 Visual Studio 中的 IDE 支持测试项目：

![](create-a-new-aspnet-mvc-project/_static/image15.png)

选择上述选项之一，将打开 IDE 中的"测试结果"窗格，并向我们提供的新项目中包括涵盖的内置功能 27 单元测试的通过/失败状态：

![](create-a-new-aspnet-mvc-project/_static/image16.png)

本教程中稍后我们将详细介绍自动测试并添加其他单元测试以涵盖我们实现应用程序功能。

### <a name="next-step"></a>下一步

我们现在就地提供基本应用程序结构。 让我们现在[创建一个数据库来存储我们的应用程序数据](create-a-database.md)。

>[!div class="step-by-step"]
[上一页](introducing-the-nerddinner-tutorial.md)
[下一页](create-a-database.md)
