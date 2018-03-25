---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/determining-what-files-need-to-be-deployed-cs
title: "确定文件需要是部署 (C#) |Microsoft 文档"
author: rick-anderson
description: "需要从开发环境部署到生产环境的文件部分取决于 ASP.NET 应用程序是否生成我们..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: f8d78a88-cc91-40d8-bce3-3d7954f6033b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/determining-what-files-need-to-be-deployed-cs
msc.type: authoredcontent
ms.openlocfilehash: d58956323275a46b44b36d4f19db4d2f607e3916
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="determining-what-files-need-to-be-deployed-c"></a>确定文件需要被部署 (C#)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载代码](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_02_CS.zip)或[下载 PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial02_FilesToDeploy_cs.pdf)

> 需要从开发环境部署到生产环境的文件部分取决于是否使用网站模型或 Web 应用程序模型生成 ASP.NET 应用程序。 了解有关这两个项目模型以及项目模型如何影响部署的详细信息。


## <a name="introduction"></a>介绍

部署 ASP.NET web 应用程序时，需要将从开发环境的与 ASP.NET 相关的文件复制到生产环境。 与 ASP.NET 相关的文件包含 ASP.NET web 页标记和代码和客户端和服务器端支持文件。 客户端支持文件是由 web 页引用和直接发送到浏览器的图像、 css 和 JavaScript 文件，例如这些文件。 服务器端支持文件包括用于在服务器端处理某个请求。 这包括到 SQL 文件，以及其他配置文件、 web 服务、 类文件、 类型化数据集和 LINQ。

一般情况下，客户端支持的所有文件应都复制从开发环境到生产环境中，但服务器端支持文件会都复制取决于是否显式为程序集 (编译服务器端代码`.dll`文件) 或如果你有自动生成这些程序集。 突出显示了本教程，需要显式将代码编译成程序集而不是具有自动发生此编译步骤操作时要部署的文件。

## <a name="explicit-compilation-versus-automatic-compilation"></a>显式编译，而不是自动的编译

ASP.NET web pages 划分为标记和源的声明性代码。 声明性标记部分包括 HTML、 Web 控件和数据绑定语法;中的代码部分包含在 Visual Basic 或 C# 代码编写的事件处理程序。 标记和代码部分通常分为不同的文件：`WebPage.aspx`包含时的声明性标记`WebPage.aspx.cs`托管代码。

请考虑名为 Clock.aspx 包含标签控件的文本属性设置为当前日期和时间加载页面时，将 ASP.NET 页。 声明性标记部分 (在`Clock.aspx`) 将包含标签 Web 控件的标记`<asp:Label runat="server" id="TimeLabel" />`-时的代码部分 (在`Clock.aspx.cs`) 都`Page_Load`事件处理程序替换为以下代码：

[!code-csharp[Main](determining-what-files-need-to-be-deployed-cs/samples/sample1.cs)]

要为此页上，该页面的代码部分为请求提供服务的 ASP.NET 引擎顺序 (`WebPage.aspx.cs`文件) 必须首先进行编译。 显式或自动，可能会发生此编译。

如果编译显式发生，则整个应用程序的源代码编译到一个或多个程序集 (`.dll`文件) 位于应用程序的`Bin`目录。 如果编译会自动发生，则生成自动生成程序集，则默认情况下，放置在`Temporary ASP.NET`Files 文件夹，它可以找到在`%WINDOWS%\Microsoft.NET\Framework\` *&lt;版本&gt;*，虽然此位置是可通过配置[`<compilation>`元素](https://msdn.microsoft.com/library/s10awwz0.aspx)中`Web.config`。 与显式编译，你必须采取一些措施来将 ASP.NET 应用程序的代码编译为一个程序集，并在部署之前时会出现此步骤。 与自动编译的编译过程时发生的 web 服务器上首次访问资源。

无论何种编译模型中，你将使用、 所有 ASP.NET 页面的标记部分 (`WebPage.aspx`文件) 需要复制到生产环境。 你需要在程序集将复制与显式编译`Bin`文件夹中，但你不需要向上 ASP.NET 页的代码部分将复制 (`WebPage.aspx.cs`文件)。 与自动编译，你需要为复制代码部分文件，以便将存在并访问页时可以自动编译代码。 每个 ASP.NET web 页的标记部分包括`@Page`属性指示是否已显式编译页面的关联的代码或是否需要会自动编译的指令。 因此，生产环境可以无缝地使用任一编译模型并不需要应用以便指示显式或自动编译所用的任何特殊配置设置。

表 1 总结了要部署使用显式编译，而不是自动的编译时，不同的文件。 请注意，无论编译模型使用你应始终将部署中的程序集`Bin`文件夹中，如果该文件夹存在。 `Bin`文件夹包含程序集特定于 web 应用程序，其中包括已编译的源代码，使用显式编译模型时。 `Bin`目录还包含其他项目中的程序集和您可能正在使用，任何开放源代码或第三方程序集和这些需要生产服务器上。 因此，作为一个常规的经验法则，复制`Bin`到生产部署时的文件夹。 (如果你使用自动编译模型，并且不使用任何外部程序集则不会有`Bin`目录的确定 ！)

| **编译模型** | **部署标记部分文件？** | **部署源代码文件？** | **部署中的程序集`Bin`目录？** |
| --- | --- | --- | --- |
| 显式编译 | 是 | No | 是 |
| 自动编译 | 是 | 是 | 是 （如果存在） |

**表 1:**部署哪些文件取决于所使用的编译模型。

## <a name="taking-a-trip-down-memory-lane"></a>拍摄内存 Lane 下行程

使用编译使用哪种方法取决于，情况情况下，如何在 Visual Studio 中管理 ASP.NET 应用程序。 由于。在 2000 年发生了四个不同版本的 Visual Studio 的 Visual Studio.NET 2002年、 Visual Studio.NET 2003年、 Visual Studio 2005 中，和 Visual Studio 2008 中的 NET 的开始。 Visual Studio.NET 2002年和 2003年管理 ASP.NET 应用程序使用*Web 应用程序项目模型*。 Web 应用程序项目模型的关键功能包括：

- 构成项目在单个项目文件中定义的文件。 未在项目文件中定义的任何文件由 Visual Studio 不考虑 web 应用程序的一部分。
- 使用显式编译。 生成项目将项目中的代码文件编译到单个程序集，则位于`Bin`文件夹。

当 Microsoft 发布了 Visual Studio 2005 时它们将放弃对 Web 应用程序项目模型的支持，它替换为网站项目模型。 通过以下方式中，网站项目模型区分本身从 Web 应用程序项目模型：

- 而不是使明确指出项目的文件的单个项目文件，会改为使用文件系统。 简单地说，web 应用程序文件夹 （或子文件夹） 中的任何文件被视为项目的一部分。
- 生成 Visual Studio 中的项目不会创建中的程序集`Bin`目录。 相反，生成网站项目报告任何编译时错误。
- 自动编译支持。 尽管代码可以是预编译 （显式编译），通过将标记和源代码复制到生产环境中，通常部署的网站项目。

它释放 Visual Studio 2005 Service Pack 1 时，Microsoft 将恢复 Web 应用程序项目模型。 但是，Visual Web Developer 不断仅支持网站项目模型。 好消息是与 Visual Web Developer 2008 Service Pack 1 已丢弃此限制。 现在，你可以在 Visual Studio （和 Visual Web Developer），它使用 Web 应用程序项目模型或网站项目模型来创建 ASP.NET 应用程序。 这两个模型都具有其优点和缺点。 请参阅[简介 Web 应用程序项目： 比较网站项目和 Web 应用程序项目](https://msdn.microsoft.com/library/aa730880.aspx#wapp_topic5)有关比较两个模型并帮助确定哪些项目模型适合你的情况。

## <a name="exploring-the-sample-web-application"></a>浏览示例 Web 应用程序

本教程中下载内容还包括调用图书评论的 ASP.NET 应用程序。 网站模仿有人可能会创建一个爱好网站以共享其簿查看与在线社区。 此 ASP.NET web 应用程序非常简单，由以下资源组成：

- `Web.config`应用程序的配置文件。
- 母版页 (`Site.master`)。
- 七个不同的 ASP.NET 页： 

    - ~`/Default.aspx`-站点的主页。
    - ~`/About.aspx`-"有关站点"页。
    - ~`/Fiction/Default.aspx`-列出已经过检查的虚构书籍的页。 

        - ~`/Fiction/Blaze.aspx`-Richard Bachman novel 评审*Blaze*。
    - ~/`Tech/Default.aspx`-列出已经过检查的技术书籍的页。 

        - ~/`Tech/CYOW.aspx`-检查*创建您自己的网站*。
        - ~/`Tech/TYASP35.aspx`-检查*教授自己 ASP.NET 3.5 24 小时内*。
- 样式文件夹中的三个不同 CSS 文件。
- 四个图像文件-通过 ASP.NET 徽标和图像的三个审阅丛书在后台提供支持的所有位于`Images`文件夹。
- A`Web.sitemap`文件，也不能定义站点图用于显示中的菜单`Default.aspx`的根目录中的页和`Fiction`和`Tech`文件夹。
- 名为的类文件`BasePage.cs`，它定义一个基`Page`类。 此类扩展的功能`Page`类通过自动设置`Title`属性根据站点映射中的页面的位置。 简而言之，任何 ASP.NET 代码隐藏类的扩展`BasePage`(而不是`System.Web.UI.Page`) 将具有其标题设置为根据站点映射中其位置的值。 例如，查看时 ~ /`Tech/CYOW.aspx`页上，标题设置为"主页:: 技术:: 创建您自己的网站"。

图 1 显示图书评论网站通过浏览器查看时的屏幕截图。 在这里看到页 ~ /`Tech/TYASP35.aspx`，其中评审书*教授自己 ASP.NET 3.5 24 小时内*。 跨越了页面和左侧列中的菜单的顶部的痕迹基于站点地图结构中定义`Web.sitemap`。 右下角的映像是一个映像位于封面`Images`文件夹。 网站的外观和感觉定义通过级联样式表规则拼由 CSS 文件中样式文件夹中，在主页上，定义总体的页面布局时`Site.master`。


[![簿评审网站提供了多种类型的标题上评审](determining-what-files-need-to-be-deployed-cs/_static/image2.png)](determining-what-files-need-to-be-deployed-cs/_static/image1.png)

**图 1:**簿评审网站提供了多种类型的标题上评审 ([单击以查看实际尺寸的图像](determining-what-files-need-to-be-deployed-cs/_static/image3.png))


此应用程序不使用站点数据库。查看每项作为单独的网页，并在应用程序中实现。 本教程 （和下一步的几个教程） 演练将部署的 web 应用程序不具有数据库。 但是，在将来的教程中我们将增强此应用程序可以存储评审、 读取器注释和其他信息在数据库中，并将浏览步骤需要执行正确部署数据驱动的 web 应用程序。

> [!NOTE]
> 这些教程专注于承载 ASP.NET 应用程序与 web 宿主提供程序和不浏览如 ASP 辅助主题。NET 的站点映射系统或使用基`Page`类。 有关这些技术的详细信息以及有关在整个教程涵盖其他主题的更多背景，请参阅其他阅读材料部分的每个教程结尾处。


本教程的下载完成的 web 应用程序的两个副本，每个不同的 Visual Studio 项目类型作为实现： BookReviewsWAP、 Web 应用程序项目和 BookReviewsWSP，网站项目。 这两个项目与 Visual Web Developer 2008 SP1 结合创建和使用 ASP.NET 3.5 SP1。 若要使用这些项目，启动通过解压缩到你的桌面的内容。 若要打开 Web 应用程序项目 (BookReviewsWAP)，导航到 BookReviewsWAP 文件夹并双击该解决方案文件， `BookReviewsWAP.sln`。 若要打开网站项目 (BookReviewsWSP)，启动 Visual Studio，然后，从文件菜单中，选择打开网站选项，浏览到`BookReviewsWSP`文件夹在你的桌面上，单击确定。


此教程看文件将需要部署应用程序时将复制到生产环境中的剩余的两个部分。 下面的两个教程- *[部署你的站点使用 FTP](deploying-your-site-using-an-ftp-client-cs.md)* 和*[部署您站点使用 Visual Studio](deploying-your-site-using-visual-studio-cs.md)*  -显示几种不同方法将这些文件复制到 web 宿主提供程序。

## <a name="determining-the-files-to-deploy-for-the-web-application-project"></a>确定要为 Web 应用程序项目部署的文件

Web 应用程序项目模型使用显式编译-项目的源代码编译到单个程序集生成应用程序每次。 此编译包括 ASP.NET 页的代码隐藏文件 (~ /`Default.aspx.cs`，~ /`About.aspx.cs`，依次类推)，以及`BasePage.cs`类。 生成的程序集名为 BookReviewsWAP.dll，位于应用程序的`Bin`目录。

图 2 显示构成该书籍评审 Web 应用程序项目的文件。


[![解决方案资源管理器列出构成 Web 应用程序项目的文件](determining-what-files-need-to-be-deployed-cs/_static/image5.png)](determining-what-files-need-to-be-deployed-cs/_static/image4.png)

**图 2**: 解决方案资源管理器列出构成 Web 应用程序项目的文件


部署 ASP.NET 应用程序开发通过 Web 应用程序项目模型开始生成应用程序以便显式编译为程序集的最新的源代码。 接下来，将以下文件复制到生产环境中：

- 文件包含的声明性标记每个 asp.net 页上，例如 ~ /`Default.aspx`，~ /`About.aspx`，依次类推。 此外，副本向上任何主页面和用户控件的声明性标记。
- 程序集 (`.dll`文件) 中`Bin`文件夹。 你不需要进行复制的程序数据库文件 (`.pdb`) 或您可能会发现任何 XML 文件`Bin`目录。

不需要将 ASP.NET 页的源代码文件复制到生产环境中，也需要将复制`BasePage.cs`类文件。

> [!NOTE]
> 如图 2 所示，`BasePage`类实现为在项目中，放在名为的文件夹中的类文件`HelperClasses`。 当编译项目中的代码`BasePage.cs`文件一起 ASP.NET 页的代码隐藏类编译到单个程序集中， `BookReviewsWAP.dll.` ASP.NET 有一个名为的特殊文件夹`App_Code`，旨在 for Web 中保存类文件网站项目。 中的代码`App_Code`文件夹会自动编译，因此应不能用于 Web 应用程序项目。 相反，应将你的应用程序的类文件放在名为的正常文件夹`HelperClasses`，或`Classes`，或类似的内容。 或者，你可以将类文件放在单独的类库项目。


除了将复制与 ASP.NET 相关的标记文件和中的程序集`Bin`文件夹中，你还需要将客户端支持文件的映像和 CSS 文件的复制的其他服务器端支持文件，以及`Web.config`和`Web.sitemap`。 这些客户端和服务器端需要支持文件复制到生产环境，而不考虑是否使用显式或自动编译。

## <a name="determining-the-files-to-deploy-for-the-web-site-project-files"></a>确定要部署的网站项目文件的文件

网站项目模型支持自动编译，在使用 Web 应用程序项目模型时不可用的功能。 与显式编译必须将你的项目的源代码编译到程序集，并将该程序集复制到生产环境。 另一方面，与自动编译你只需将源代码复制到生产环境，并且根据需要由运行时按需编译。

Visual Studio 中的生成菜单选项中不存在 Web 应用程序项目和网站项目。 构建 Web 应用程序项目将项目的源代码编译到单个程序集位于`Bin`目录; 构建网站项目中检查任何编译时错误，但不会创建任何程序集。 部署 ASP.NET 应用程序使用的网站项目模型所有需要执行开发是复制到生产环境中，相应的文件，但我会建议你第一次生成项目以确保没有编译时错误。

图 3 显示构成该书籍评审网站项目的文件。


 [![解决方案资源管理器列出构成网站项目的文件](determining-what-files-need-to-be-deployed-cs/_static/image7.png)](determining-what-files-need-to-be-deployed-cs/_static/image6.png) 

**图 3**: 解决方案资源管理器列出构成网站项目的文件


部署网站项目涉及到将所有与 ASP.NET 相关的文件复制到生产环境-包含 ASP.NET 页、 母版页和用户控件的标记页及其代码文件。 你还需要进行任何类文件，如 BasePage.cs 复制。 请注意，`BasePage.cs`文件位于`App_Code`文件夹中，这是一个类文件在网站项目中使用的特殊 ASP.NET 文件夹。 特殊文件夹需要作为中的类文件创建在生产环境，以及，`App_Code`开发环境上的文件夹必须复制到`App_Code`上生产文件夹。

除了复制 ASP.NET 标记和源代码文件，你还需要将客户端支持文件的映像和 CSS 文件的复制的其他服务器端支持文件，以及`Web.config`和`Web.sitemap`。

> [!NOTE]
> 网站项目还可以使用显式编译。 将来的教程将说明如何显式编译网站项目。


## <a name="summary"></a>摘要

部署 ASP.NET 应用程序时，需要将从开发环境的所需的文件复制到生产环境。 精确的需要进行同步的文件集取决于是否 ASP.NET 应用程序的显式或自动编译代码。 是否配置 Visual Studio 会影响所使用的编译策略来管理使用 Web 应用程序项目模型或网站项目模型的 ASP.NET 应用程序。

Web 应用程序项目模型使用显式编译，并将项目的代码编译成一个程序集在`Bin`文件夹。 部署应用程序、 ASP.NET 页的标记部分的内容时`Bin`文件夹必须推送到生产环境; 不需要的代码文件和代码隐藏类，例如-应用程序中的源代码要复制到生产环境。

尽管可以显式编译网站项目中，我们将会在将来看到教程，网站项目模型默认情况下，使用自动编译。 部署使用自动编译的 ASP.NET 应用程序要求的标记部分*和*源代码必须复制到生产环境。 第一次请求时，代码会自动编译对生产环境。

现在，我们检查完文件需要开发和生产环境之间同步我们已准备好部署到 web 主机提供商簿评审应用程序。

尽情享受编程 ！

### <a name="further-reading"></a>其他阅读材料

在本教程中讨论的主题的详细信息，请参阅以下资源：

- [ASP.NET 编译概述](https://msdn.microsoft.com/library/ms178466.aspx)
- [ASP.NET 用户控件](https://msdn.microsoft.com/library/y6wb1a0e.aspx)
- [正在检查 ASP。NET 的网站的导航](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Web 应用程序项目的简介](https://msdn.microsoft.com/library/aa730880.aspx)
- [Master 页教程](../master-pages/creating-a-site-wide-layout-using-master-pages-cs.md)
- [页间共享代码](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/pages/code.aspx)
- [使用适用于 ASP.NET 页的代码隐藏类的自定义的基类](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)
- [Visual Studio 2005 的网站项目系统： 它是什么，为什么这样做它？](https://weblogs.asp.net/scottgu/archive/2005/08/21/423201.aspx)
- [演练： 将网站项目转换为 Visual Studio 中的 Web 应用程序项目](https://msdn.microsoft.com/library/aa983476.aspx)

>[!div class="step-by-step"]
[上一页](asp-net-hosting-options-cs.md)
[下一页](deploying-your-site-using-an-ftp-client-cs.md)
