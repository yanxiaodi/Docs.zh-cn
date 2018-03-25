---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
title: "引入了 ASP.NET Web Pages-入门教程 |Microsoft 文档"
author: tfitzmac
description: "WebMatrix 是不再建议将其作为集成的开发环境的 ASP.NET Web 页。 使用 Visual Studio 或 Visual Studio 代码。 本指南..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: a36d3bdf-ef1b-47a4-b932-3a0cf4cad716
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
msc.type: authoredcontent
ms.openlocfilehash: a6789ee75b4ca6e9443681cc7ec0bd3ab94cedcd
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="introducing-aspnet-web-pages---getting-started"></a>引入了 ASP.NET Web 页-入门
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> > [!NOTE] 
> > 
> > WebMatrix 是不再建议将其作为集成的开发环境的 ASP.NET Web 页。 使用[Visual Studio](../program-asp-net-web-pages-in-visual-studio.md)或[Visual Studio Code](https://code.visualstudio.com/)。
> 
> 
> 此指南并应用程序使您的概览的 ASP.NET Web Pages （2 或更高版本） 和 Razor 语法，一个轻型框架，用于创建动态网站。 它还引入了 WebMatrix 中，一个用于创建网页和网站工具。
> 
> **级别**： 新的 ASP.NET Web 页面。  
> **假定的技能**: HTML，基本的级联样式表 (CSS)。
> 
> 你将要掌握的内容集的第一个教程中：
> 
> - 哪种 ASP.NET Web Pages 技术和它的适用。
> - WebMatrix 是什么。
> - 如何安装的所有内容。
> - 如何使用 WebMatrix 创建网站。
>   
> 
> 功能/技术讨论：
> 
> - Microsoft Web 平台安装程序。
> - WebMatrix。
> - *.cshtml*页
>   
> 
> Mike Pope 最初已写入本教程。 Tom FitzMacken 为 Microsoft WebMatrix 3 更新它。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教程中使用的软件版本
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 3


## <a name="what-should-you-know"></a>你应该知道哪些内容？

我们假定你熟悉：

- **HTML**。 没有深入专业知识是必需的。 我们不会介绍 HTML，但我们也不使用任何复杂。 我们将提供到 HTML 教程，我们认为它们是有用的链接。
- **级联样式表 (CSS)**。 与相同 HTML。
- **基本数据库想法**。 如果你已对数据使用电子表格和排序和筛选的数据，是专业技术级别我们通常假定此教程的集。

我们还假定你感兴趣学习基本编程。 ASP.NET 网页使用名为 C# 的编程语言。 你无需具有在编程中，只需有兴趣了解它的任何背景。 如果您曾经编写了任何 JavaScript 网页中之前，你具有充足的背景。

请注意，是否你熟悉编程，你可能会发现时我们将新程序员掌握，最初设置了本教程将慢慢地移动。 我们共同点的第一个的几个教程，不过，将会不太基本的编程，以解释和操作将移动更快的剪辑。

## <a name="what-do-you-need"></a>你需要做什么？

你将需要以下项目：

- 运行 Windows 8、 Windows 7、 Windows Server 2008 或 Windows Server 2012 的计算机。
- 实时的 internet 连接。
- （安装过程所需） 具有管理员特权。

## <a name="what-is-aspnet-web-pages"></a>什么是 ASP.NET Web 页？

ASP.NET Web 页是一个框架，可用于创建动态网页。 简单的 HTML 网页是静态的;位于页中的固定 HTML 标记取决于其内容。 如你创建的 ASP.NET Web Pages 使用的动态页面，您可以动态，通过使用代码创建的页面内容。

动态页面，您可以执行所有类型的操作。 你可以要求用户进行输入使用窗体，，然后更改页的显示或它的外观。 你可以从用户获取信息，将其保存在数据库中，然后将其列更高版本。 你可以从您的网站发送电子邮件。 你可以与 web （例如，映射服务） 上的其他服务进行交互，并生成集成来自这些源的信息的页。

## <a name="what-is-webmatrix"></a>WebMatrix 是什么？

WebMatrix 是集成的网页编辑器、 一个数据库实用程序、 用于测试页面和用于将你的网站发布到 Internet 的功能的 web 服务器的工具。 WebMatrix 是免费的而且很容易安装且易于使用。 （它还适用仅纯 HTML 页面，以及其他技术，如 PHP。）

实际不*具有*要 WebMatrix 用于工作与 ASP.NET Web 页。 你可以创建页面，通过使用文本编辑器中，例如，使用和测试页有权访问的 web 服务器。 但是，WebMatrix 更加所有非常方便，因此这些教程将使用在整个 WebMatrix。

## <a name="about-these-tutorials"></a>有关这些教程

此教程集是如何使用 ASP.NET Web 页的简介。 在此介绍性教程集中共有 9 教程。 它的一系列教程的集，使您从 ASP.NET Web Pages 初级用户转至创建真实的专业的网站的一部分。

此第一个教程设置侧重点向您展示如何处理与 ASP.NET Web 页的基础知识。 完成后，你可以使用选取此结束，其中，更深入探索 Web 页的其他教程集。

我们有意继续轻松深入说明。 然后我们显示内容，只要此教程集我们始终选择我们认为的方法是最简单的方法了解。 更高版本的教程集进入详细深度，并显示效率更高或更灵活方法 （也更有趣）。 但这些教程要求你首先了解基础知识。

您刚才已启动的教程集涵盖这些功能的 ASP.NET Web 页：

- 简介和获取安装的所有内容。 （这是你正在阅读本教程中值。）
- ASP.NET Web Pages 编程基础知识。
- 创建数据库。
- 创建和处理用户输入窗体。
- 添加、 更新和删除数据库中的数据。

## <a name="what-will-you-create"></a>你将创建什么？

本教程设置和后续的围绕的网站，你可以在其中列出您喜欢的电影。 你将能够输入电影、 编辑它们，并将其列。 以下是几个你将创建此教程集中的页。 第一个显示影片列出你将创建的页：

![创建显示的影片列表完成影片应用程序](getting-started/_static/image1.png)

而且，此处是可将新的影片信息添加到您的网站的页面：

![创建显示将添加影片页完成的影片应用程序](getting-started/_static/image2.png)

后续教程集生成对此设置，并添加更多的功能，如上载图片、 允许登录的人员、 发送电子邮件和与社交媒体集成。

## <a name="see-this-app-running-on-azure"></a>请参阅在 Azure 上运行此应用程序

若要查看与实时 web 应用运行的已完成的站点吗？ 只需单击下面的按钮，可以将应用程序的完整版本部署到你的 Azure 帐户。

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebPagesMovies)

你需要将此解决方案部署到 Azure 的 Azure 帐户。 如果你还没有帐户，可以使用以下选项：

- [免费建立一个 Azure 帐户](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604)-获取信用额度来试用付费版 Azure 服务，你可以使用和甚至在用完后，最多可以保留帐户和使用免费的 Azure 服务。
- [激活 MSDN 订户权益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604)-MSDN 订阅为你的信用额度可以用于付费版 Azure 服务的每个月。

## <a name="installing-everything"></a>安装的所有内容

你可以通过从 Microsoft Web 平台安装程序安装的所有内容。 实际上，你安装安装程序，然后使用它来安装其他任何内容。

若要使用 Web 页，你必须至少具有 sp3 安装，Windows XP 或 Windows Server 2008 或更高版本。

上[网页页](../../../index.md)ASP.NET 网站中，单击**安装**。

![ASP.NET Web 站点显示&quot;安装 WebMatrix&quot;按钮](getting-started/_static/image3.png)

系统会要求你接受许可条款和隐私声明，然后安装 WebMatrix。

![接受条款以开始安装](getting-started/_static/image4.png)

单击**运行**开始安装。 (如果你想要保存安装程序，请单击**保存**，然后从保存位置的文件夹运行安装程序。)

![](getting-started/_static/image5.png)

Web 平台安装程序中出现，请准备好安装 WebMatrix。 单击“安装” 。

![](getting-started/_static/image6.png)

安装过程指出它已在你的计算机上安装并启动该过程。 具体取决于完全什么已安装，过程可能需要任何位置从几分钟到几分钟的时间。 选择**我接受**以接受许可条款。

## <a name="hello-webmatrix"></a>Hello WebMatrix

完成后，安装过程可以自动启动 WebMatrix。 如果它不存在，在 Windows 中，从**启动**菜单，启动**Microsoft WebMatrix**。

首次启动 WebMatrix，你可以在有机会使用你的 Microsoft 帐户登录到 Microsoft Azure。 登录，你将收到 10 个免费的 web 应用的整个 Azure。 这些免费的 web 应用程序提供一种简便方式测试你的应用。 如果你还没有 Azure 帐户，但你有 MSDN 订阅，你可以[激活 MSDN 订阅权益](https://www.windowsazure.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604)。 否则，可以在几分钟内创建一个免费试用帐户。 有关详细信息，请参阅[Azure 免费试用版](https://azure.microsoft.com/free/?WT.mc_id=A443DD604)。

无需立即登录以继续本教程。 如果你执行不登录现在，你仍将进行更高版本登录的选项。 最后一个[主题](publishing.md)系列在本教程介绍如何将你的网站部署到 Azure; 因此，你将需要登录以完成该主题。

此时，请登录你的 Microsoft 帐户或选择**现在不**右下角中。

![登录](getting-started/_static/image7.png)

若要开始，将创建空网站，并添加页。 在这一组后面的教程中你将播放的某个内置网站模板。

在开始窗口中，单击**新建**。

![WebMatrix 启动屏幕](getting-started/_static/image8.png)

模板是网站的预构建的文件和不同类型的页。 若要查看所有默认为可用的模板，请选择模板库选项。

![选择模板库](getting-started/_static/image9.png)

在**快速启动**窗口中，选择**空网站**从**ASP.NET**组并命名新站点"WebPagesMovies"。

![WebMatrix 快速启动窗口中与选择的空网站模板](getting-started/_static/image10.png)

单击 **“下一步”**。

如果你已登录到你的 Microsoft 帐户，你将有机会在 Azure 上创建站点。 根据你的站点的默认名称的名称**WebPagesMovies.azurewebsites.net**建议; 但是，感叹号指示此名称不是 Windows Azure 上提供。 为简单起见，选择**跳过**跳过立即在 Azure 上创建网站。 稍后在此系列中，我们将站点发布到 Azure。

![创建 azure 网站](getting-started/_static/image11.png)

WebMatrix 创建和打开站点：

![在 WebMatrix 中打开新 WebPagesMovies 站点](getting-started/_static/image12.png)

在顶部，有了快速访问工具栏和功能区。 在左下角，你看到工作区中选择器其中切换任务 (**站点**，**文件**，**数据库**，**报表**)。 在右侧是内容窗格中，为编辑器和报表。 和跨底部有时您会看到消息通知栏。

你将了解有关 WebMatrix 和其功能在学习这些教程。

## <a name="creating-a-web-page"></a>创建 Web 页

要熟悉 WebMatrix 和 ASP.NET 网页，你将创建一个简单的页面。

在工作区中选择器中，选择**文件**工作区。 此工作区，可以使用文件和文件夹。 左窗格中显示你的站点的文件结构。 功能区更改为显示与文件相关的任务。

![在 WebMatrix 的文件工作区](getting-started/_static/image13.png)

在功能区中，单击下的箭头**新建**，然后单击**新文件**。

![使用&quot;新建&quot;命令在功能区中创建新的文件](getting-started/_static/image14.png)

WebMatrix 显示的文件类型的列表。 选择**CSHTML**，然后在**名称**框中，键入"HelloWorld"。 CSHTML 页是一个 ASP.NET Web Pages 页。

![创建一个新的 CSHTML 页命名 HelloWorld.cshtml](getting-started/_static/image15.png)

单击 **“确定”**。

WebMatrix 创建页面，并在编辑器中打开它。

![在 WebMatrix 编辑器中新的 HelloWorld 页](getting-started/_static/image16.png)

如你所见，页面将包含主要普通 HTML 标记中的，如下所示顶部块除外：

[!code-cshtml[Main](getting-started/samples/sample1.cshtml)]

添加代码，如很快就会看到的。

请注意，页面的不同部分&mdash;元素名称、 属性和文本，以及顶部块 — 所有正在不同的颜色。 这称为*语法突出显示*，并可更轻松地将清除所有内容保持。 它是可以轻松地处理在 WebMatrix 的 web 页面的功能之一。

添加的内容`<head>`和`<body>`在下面的示例之类的元素。 （如果你想，你可以只需复制下面的块然后使用以下代码替换整个现有页面。）

[!code-cshtml[Main](getting-started/samples/sample2.cshtml)]

在快速访问工具栏或在**文件**菜单上，单击**保存**。

![在 WebMatrix 快速访问工具栏保存按钮](getting-started/_static/image17.png)

## <a name="testing-the-page"></a>测试的页

在**文件**工作区中，右键单击*HelloWorld.cshtml*页，然后单击**在浏览器中启动**。

![运行页面 WebMatrix 功能区中使用运行按钮](getting-started/_static/image18.png)

WebMatrix 启动可用于测试你的计算机上的页面的内置 web 服务器 (IIS Express)。 （而无需 IIS Express 在 WebMatrix 中，你将必须发布页的 web 服务器到某个位置之前无法对其进行测试。）在默认浏览器中将显示的页。

![&quot;Hello World&quot;页在浏览器中运行](getting-started/_static/image19.png)

请注意，当你在 WebMatrix 中测试页面，浏览器中的 URL 是类似`http://localhost:33651/HelloWorld.cshtml.`名称*localhost*指的是本地服务器，这意味着，通过在你自己的计算机上的 web 服务器提供的页面。 如前所述，WebMatrix 将包含名为 IIS Express 启动页时运行的 web 服务器程序。

之后的数字*localhost* (例如， *localhost:33651*) 是指*端口号*你计算机上。 这是 IIS Express 用于此特定网站的"通道"的数目。 当你创建你的站点，并为你创建的每个站点不同，端口号是从范围 1024 到 65536 随机进行选择。 （测试自己的网站时，端口号几乎可以肯定将不同数量比 33561。）通过为每个网站使用不同的端口，IIS Express 可以保留直这你对通信的站点。

在发布你的站点到一个公共 web 服务器，不会再看到时，更高版本*localhost*在 URL 中。 此时，你将看到更为典型的 URL，如`http://myhostingsite/mywebsite/HelloWorld.cshtml`或任何页。 你将了解有关稍后在本教程系列中发布站点的详细信息。

## <a name="adding-some-server-side-code"></a>添加一些服务器端代码

关闭浏览器，然后返回到 WebMatrix 中的页。

将行添加到代码块，以便其类似下列代码所示：

[!code-cshtml[Main](getting-started/samples/sample3.cshtml)]

这是很少的 Razor 代码。 获取当前日期和时间，并将该值转换为可能清楚*变量*名为`currentDateTime`。 你将在读取更多有关下一教程中的 Razor 语法。

在页上，正文中后`<p>Hello World!</p>`元素中，添加以下：

[!code-html[Main](getting-started/samples/sample4.html)]

此代码获取的值放入`currentDateTime`顶部变量并将它插入到页的标记。 `@`字符将标记中页的 ASP.NET 网页代码。

运行的页面再次 （WebMatrix 将保存所做的更改为你之前它运行页面）。 此时会显示的日期和时间在页中。

![&quot;Hello World&quot;页具有动态生成的时间显示浏览器中运行](getting-started/_static/image20.png)

请稍等片刻，然后刷新浏览器中的页。 日期和时间显示会更新。

在浏览器中，查看页面源文件。 它类似于以下标记：

[!code-html[Main](getting-started/samples/sample5.html)]

请注意，`@{ }`顶部块不存在。 另请注意，日期和时间屏幕将显示实际字符串的字符 (`1/18/2012 2:49:50 PM`或任何)，而不`@currentDateTime`像中有*.cshtml*页。 发生了什么情况下面是运行时页上，ASP.NET 将处理所有的代码 （几乎在此情况下），标记有`@`。 此代码生成输出，然后该输出已插入到页。

## <a name="this-is-what-aspnet-web-pages-are-about"></a>这是 ASP.NET Web 页即将

当你读取的 ASP.NET Web Pages 生成动态 web 内容时，你已了解是主意。 你刚刚创建的页面包含如上所示的相同的 HTML 标记。 它还可以包含可以执行各种任务的代码。 在此示例中，它未获取当前日期和时间的重要任务。 如你所看到的你可以使用 HTML 中以生成在页中的输出点缀代码。 当有人请求*.cshtml*中浏览器中，ASP.NET 页处理页，同时仍在手中的 web 服务器。 ASP.NET 将插入代码的输出 （如果有） 到页以 HTML 的形式。 完成的代码处理后，ASP.NET 将发送到浏览器所生成的页面。 所有浏览器曾获取为 HTML。 下面是一个关系图：

![如何 ASP.NET 动态生成 HTML 时的概念流](getting-started/_static/image21.png)

其原理很简单，但有许多有趣的任务的代码可以执行，并且有在其中你可以动态 HTML 内容添加到页面的多种有趣方法。 和 ASP.NET *.cshtml*页面，例如从任何 HTML 页中，还可以包括在浏览器本身 （JavaScript 和 jQuery 代码） 中运行的代码。 您将了解所有这些操作在此教程集和后续条件。

## <a name="coming-up-next"></a>接下来

在本系列中下一步教程中，您了解 ASP.NET Web Pages 稍微编程。

## <a name="additional-resources"></a>其他资源

[从头开始创建 ASP.NET 网站](https://www.microsoft.com/web/post/create-an-aspnet-website-from-scratch)。 这是一个教程，具体而言，是有关使用 WebMatrix （不是 ASP.NET Web 页）。 它将进入稍有一些我们将不在此教程集中涉及的 WebMatrix 的其他功能的更多详细信息。

>[!div class="step-by-step"]
[下一篇](intro-to-web-pages-programming.md)
