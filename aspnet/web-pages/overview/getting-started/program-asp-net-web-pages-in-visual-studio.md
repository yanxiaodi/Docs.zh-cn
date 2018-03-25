---
uid: aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
title: "编程 ASP.NET 网页 (Razor) 使用 Visual Studio |Microsoft 文档"
author: tfitzmac
description: "本附录说明如何使用 Visual Studio 2010 或 Visual Web Developer 2010 Express 给程序的 ASP.NET Web Pages 使用 Razor 语法。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/13/2014
ms.topic: article
ms.assetid: 0acfec5a-48f2-4766-a801-a0f426966f0a
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: 5cfeda206eda8fb3fd769d34fb40bae2c3b65093
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="programming-aspnet-web-pages-razor-using-visual-studio"></a>使用 Visual Studio 编程 ASP.NET Web 页 (Razor)
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> 此文章介绍了如何使用 Visual Studio 或 Visual Web Developer Express 给程序所需的 ASP.NET Web Pages (Razor) 网站。
> 
> 学习内容
> 
> - 需要安装 （如果有的话） 你的 Visual Studio 版本中的工作与 ASP.NET Web 页。
> - 如何将支持为 ASP.NET 网页添加到 Visual Web Developer 2010 Express。
> - 如何使用 Visual Studio 中的功能用于 ASP.NET Razor 页，包括 IntelliSense 和调试程序。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教程中使用的软件版本
> 
> 
> - ASP.NET 网页 (Razor) 3
> - Visual Studio 2013
> - WebMatrix 3
>   
> 
> 本教程还适用于 ASP.NET Web Pages 2、 Visual Studio 2012、 Visual Studio 2010 中，和 WebMatrix 2。


使用 Razor 语法使用 WebMatrix 或许多其他代码编辑器，可以 ASP.NET Web 页进行编程。 你还可以使用 Microsoft Visual Studio 这是提供一套强大的工具创建的应用程序 （而不仅仅是网站） 的许多类型的全功能集成的开发环境 (IDE)。 若要使用 ASP.NET Razor 页，你可以使用 Visual Studio 的完整版本之一或者免费[Visual Studio Express for Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express)版本。

Visual Studio 提供了用于与 ASP.NET Razor 网页编程的两个特别有用功能是：

- *IntelliSense*。 内置于 Visual Studio 中的 IntelliSense 功能是比在 WebMatrix 中的 IntelliSense 更全面。
- *调试器*。 调试器可让您通过停止程序，它是运行、 检查变量和单步执行代码行的行时诊断你的代码。

## <a name="using-visual-studio-with-different-versions-of-aspnet-web-pages"></a>Visual Studio 中使用不同版本的 ASP.NET 网页

Visual Studio 2012 和 Visual Studio 2013 包括支持的 ASP.NET Web 页。 （为支持的 ASP.NET Web Pages 必需的包安装的安装 Visual Studio 时。）

Visual Studio 2010 不包括支持默认情况下为 ASP.NET 网页。 若要使用 Visual Studio 2010 ASP.NET 网页，你必须安装 ASP.NET MVC 包。 若要获取 ASP.NET 网页 2，你需要安装 ASP.NET MVC 4。

下表总结了不同版本的 Visual Studio 中的支持为 ASP.NET 网页。

|  | Visual Studio 2010 | Visual Studio 2012 | Visual Studio 2013 |
| --- | --- | --- | --- |
| **ASP.NET 网页 2** | 安装 ASP.NET MVC 4 | （包括） | （包括） |
| **ASP.NET Web Pages 3** |  | 更新到 ASP.NET Web 页 3 通过 NuGet | （包括） |

若要使用 Visual Studio 2010，请参阅[安装支持 ASP.NET Web 页的 Visual Studio 2010 中](#vs2010support)。

## <a name="launching-visual-studio-from-webmatrix"></a>启动 Visual Studio 中的，从 WebMatrix

如果你已在 WebMatrix 中启动项目，并想要切换到 Visual Studio，WebMatrix 将提供用于轻松地在 Visual Studio 中打开项目的按钮。 你必须安装 Visual Studio 此按钮在计算机上启用。 下图显示在 WebMatrix 中的按钮。

![启动 Visual Studio](program-asp-net-web-pages-in-visual-studio/_static/image1.png)

单击按钮时，在 Visual Studio 中打开该项目。 你可以来回切换 WebMatrix 和 Visual Studio 之间没有任何问题。 如果任何文件中另一个环境中已更改并且需要重新加载以获取最新的更改，你将收到通知。

## <a name="creating-aspnet-razor-site-in-visual-studio"></a>在 Visual Studio 中创建 ASP.NET Razor 站点

在 Visual Studio 中创建 ASP.NET Razor 网站：

1. Visual Studio 或 Visual Web Developer 启动。
2. 在**文件**菜单上，单击**新网站**。

    ![创建新网站](program-asp-net-web-pages-in-visual-studio/_static/image2.png)
3. 在**新网站**对话框框中，选择要使用 （Visual C# 或 Visual Basic） 的语言。
4. 选择**ASP.NET 网站 (Razor)**模板。

    ![razor 站点](program-asp-net-web-pages-in-visual-studio/_static/image3.png)
5. 单击“确定”。

新的项目中存在，且使用某些默认的 web 页面以帮助你开始填充。

### <a name="using-intellisense"></a>Using IntelliSense

现在，你已创建站点，你可以看到在 Visual Studio 中的 IntelliSense 工作原理。

1. 在你刚创建的网站，打开*Default.cshtml*页。
2. 后`<h3>`标记在页中，键入`@ServerInfo.`（包括该点）。 请注意如何 IntelliSense 显示的可用方法`ServerInfo`下拉列表中的帮助器。 

    ![Intellisense](program-asp-net-web-pages-in-visual-studio/_static/image4.png)
3. 选择`GetHtml`方法从列表，然后按 Enter。 IntelliSense 会自动填写方法中。 (如使用 C# 中的任何方法，你必须添加`()`在方法之后的字符。)  
 已完成的代码`GetHtml`方法类似于下面的示例：  

    [!code-cshtml[Main](program-asp-net-web-pages-in-visual-studio/samples/sample1.cshtml)]
4. 按 Ctrl + F5 运行页面。 这是什么时显示在浏览器中网页的外观： 

    ![在浏览器中的默认页](program-asp-net-web-pages-in-visual-studio/_static/image5.png)
5. 关闭浏览器。

### <a name="using-the-debugger"></a>使用调试器

1. 在顶部*Default.cshtml*页上，开头的行之后`Page.Title`，添加以下代码行： 

    [!code-csharp[Main](program-asp-net-web-pages-in-visual-studio/samples/sample2.cs)]
2. 在左侧的代码编辑器的灰色边缘，单击此新行的旁边为了添加*断点*。 断点是一种标记，告知调试器停止在该点运行程序，以便你可以查看发生了什么情况。

    ![设置断点](program-asp-net-web-pages-in-visual-studio/_static/image6.png)
3. 删除对的调用`ServerInfo.GetHtml`方法，并添加对的调用`@myTime`变量在其位置。 此调用将显示由新的代码行的当前时间值。
4. 按 F5 以在调试器中运行页面。 页会在你设置的断点处停止。 下图显示了页面外观在编辑器中使用断点 （用黄色）。 

    ![调试断点](program-asp-net-web-pages-in-visual-studio/_static/image7.png)
5. 在调试工具栏中，单击**单步执行**按钮 （或按 F11） 以运行下一个代码行。 单击此按钮时，每次你向前执行移动到下一行代码。

    ![单步执行按钮](program-asp-net-web-pages-in-visual-studio/_static/image8.png)
6. 检查的值`myTime`变量在其上停留鼠标指针或通过检查中显示的值**局部变量**和**调用堆栈**windows。 Visual Studio 显示变量的值。

    ![显示时间值](program-asp-net-web-pages-in-visual-studio/_static/image9.png)
7. 当完成时检查变量和逐句通过代码时，按 F5 以继续运行而不停止在每个行的页。 在单步执行所有代码完后，浏览器将显示页。

若要了解有关调试器以及如何调试 Visual Studio 中的代码的详细信息，请参阅[演练： 在 Visual Web Developer 调试 Web 页](https://msdn.microsoft.com/library/z9e7w6cs.aspx)。

## <a name="using-razor-in-aspnet-mvc-projects-with-visual-studio"></a>在使用 Visual Studio 的 ASP.NET MVC 项目中使用 Razor

ASP.NET MVC 项目中还进行了广泛使用 Razor 语法。 MVC 是构建动态网站功能强大、 基于模式的方法。 如果你的 ASP.NET Web Pages 站点变得难以维护，你可能想要考虑将其转换为 ASP.NET MVC 应用程序。 有关创建 MVC 应用程序的示例，请参阅[Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md)。

<a id="vs2010support"></a>
## <a name="installing-support-for-aspnet-web-pages-in-visual-studio-2010"></a>在 Visual Studio 2010 安装 ASP.NET 网页的支持

本部分演示如何安装 Visual Web Developer Express 2010 以及 ASP.NET 网页 Tools for Visual Studio。

1. 如果你尚未安装 Web 平台安装程序，请从以下 URL 下载它：

    [https://www.microsoft.com/web/downloads/platform.aspx](https://www.microsoft.com/web/downloads/platform.aspx)
2. 运行 Web 平台安装程序。
3. 单击**产品**选项卡。

    ![WebPI 产品选项卡](program-asp-net-web-pages-in-visual-studio/_static/image10.png)
4. 搜索**ASP.NET MVC 4** （适用于 ASP.NET Web Pages 2)，然后单击**添加**。 这些产品包括 Visual Studio 工具生成 ASP.NET Razor 网站。

    ![WebPi 安装选项](program-asp-net-web-pages-in-visual-studio/_static/image11.png)
5. 单击**安装**以完成安装。
