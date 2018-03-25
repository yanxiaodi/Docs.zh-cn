---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-an-ftp-client-vb
title: "部署你的网站使用 FTP 客户端 (VB) |Microsoft 文档"
author: rick-anderson
description: "部署 ASP.NET 应用程序的最简单方法是手动将从开发环境所需的文件复制到生产环境。 此应用..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: 09279194-bcf9-4b59-a09d-c68e5926a758
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-an-ftp-client-vb
msc.type: authoredcontent
ms.openlocfilehash: 7792891aed6f0c5e952018dacb36a1d267cb6ae0
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
<a name="deploying-your-site-using-an-ftp-client-vb"></a>部署你的网站使用 FTP 客户端 (VB)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载代码](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_03_VB.zip)或[下载 PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial03_DeployingViaFTP_vb.pdf)

> 部署 ASP.NET 应用程序的最简单方法是手动将从开发环境所需的文件复制到生产环境。 本教程演示如何使用 FTP 客户端到 web 宿主提供程序从你的桌面获取文件。


## <a name="introduction"></a>介绍

前面的教程引入组成少量的 ASP.NET 页、 母版页、 自定义基本的简单簿评审 ASP.NET web 应用`Page`类，大量的映像，以及三个 CSS 样式表。 现在，我们已准备好部署到 web 主机提供商，此时，应用程序将使用连接到 Internet 的任何人可以访问此应用程序 ！


在我们讨论从[*确定什么文件需要部署到*](determining-what-files-need-to-be-deployed-vb.md)教程，我们知道需要复制到 web 宿主提供程序的文件。 （回想一下，将复制哪些文件取决于是否你编译应用程序显式或自动。）但是如何我们获得文件从开发环境 （我们桌面） 到生产环境 （由 web 宿主提供程序的 web 服务器）？ [ **F** ile **T** ransfer **P** rotocol (FTP)](http://en.wikipedia.org/wiki/File_Transfer_Protocol)是用于将文件从一台计算机复制到另一个，通过网络的常用的协议。 另一个选项是 FrontPage 服务器扩展 (FPSE)。 本教程重点介绍使用独立的 FTP 客户端软件部署到生产环境中开发环境的必要文件。

> [!NOTE]
> Visual Studio 包含通过 FTP; 发布的网站的工具下一教程中介绍了这些工具，以及一看使用 FPSE 的工具。


若要使用的 FTP，我们需要复制这些文件*FTP 客户端*在开发环境。 FTP 客户端是应用程序设计为将文件复制到正在运行的计算机安装的计算机从*FTP 服务器*。 （如果你的 web 宿主提供程序支持文件传输，通过 FTP，大多数一样，则 FTP 服务器在其 web 服务器上运行。）有大量的 FTP 客户端应用程序。 Web 浏览器可以为 FTP 客户端甚至 double。 我最喜欢的 FTP 客户端，我将用于本教程中，一个是[FileZilla](http://filezilla-project.org/)，免费、 开源的 FTP 客户端，用于 Windows、 Linux 和 Mac。 任何 FTP 客户端起作用，不过，因此请使用在最熟悉的任何客户端。

如果你采用了沿你将需要使用 web 宿主提供程序之前创建一个帐户可以完成本教程或后续的。 如前面的教程中所述，有的 web 宿主提供程序公司匆忙与各种的价格、 功能和服务质量。 我将使用为本教程系列[折扣 ASP.NET](http://discountasp.net)为我的 web 宿主提供程序，但你可以遵循任何 web 宿主提供程序，只要它们支持您的网站开发中的 ASP.NET 版本。 （这些教程使用创建 ASP.NET 3.5。）此外，由于我们会将文件复制到 web 宿主提供程序使用在本教程中以及在将来发生 FTP 它是命令性 web 宿主提供程序支持对 web 服务器的 FTP 访问。 几乎所有的 web 主机提供商提供此功能，但在注册之前，您应仔细检查。

## <a name="deploying-the-book-review-web-application-project"></a>部署该书籍查看 Web 应用程序项目

回想一下，有两个版本的书籍查看 web 应用程序： 一个实现使用 Web 应用程序项目模型 (BookReviewsWAP) 和另一个使用网站项目模型 (BookReviewsWSP)。 项目类型会影响是否自动或显式编译为站点和该编译模型指示需要部署的文件。 因此，我们将查看单独，部署在 BookReviewsWAP 和 BookReviewsWSP 项目开头 BookReviewsWAP。 花些时间下载这些两个 ASP.NET 应用程序，如果你不这样做已。

通过导航到启动 BookReviewsWAP 项目`BookReviewsWAP`文件夹，然后双击`BookReviewsWAP.sln`文件。 在部署项目之前很重要来生成它以确保对源代码的任何更改都包括在编译的程序集。 若要生成项目转到生成菜单，然后选择生成 BookReviewsWAP 菜单选项。 这将在项目中的源代码编译到单个程序集， `BookReviewsWAP.dll`，这将置于`Bin`文件夹。

现在，我们已准备好部署所需的文件 ！ 启动 FTP 客户端并连接到 web 宿主提供程序上的 web 服务器。 （与 web 托管公司的名义注册时它们将通过电子邮件发送你如何连接到 FTP 服务器的信息; 这包括 FTP 服务器，以及用户名和密码的地址。）

从你的桌面的以下文件复制到 web 宿主提供程序的根网站文件夹中。 当您使用到 web 服务器的 FTP 的 web 宿主提供程序时很可能在网站根目录。 但是，某些 web 宿主提供程序具有一个名为子`www`或`wwwroot`用作你的网站文件的根文件夹。 最后，当 FTPing 文件时你可能需要在生产环境中的上创建相应的文件夹结构`Bin`文件夹，`Fiction`文件夹，`Images`文件夹中，依次类推。

- `~/Default.aspx`
- `~/About.aspx`
- `~/Site.master`
- `~/Web.config`
- `~/Web.sitemap`
- 完整内容`Styles`文件夹
- 完整内容`Images`文件夹 (和子文件夹， `BookCovers`)
- `~/Fiction/Default.aspx`
- `~/Fiction/Blaze.aspx`
- `~/Tech/Default.aspx`
- `~/Tech/CYOW.aspx`
- `~/Tech/TYASP35.aspx`
- `~/Bin/BookReviewsWAP.dll`

图 1 显示 FileZilla 后已复制所需的文件。 FileZilla 显示在左侧和右侧的远程计算机上的文件的本地计算机上的文件。 如图 1 所示，ASP.NET 源代码文件，如`About.aspx.vb`，位于本地计算机 （的开发环境中），但未被其因为无需在使用时部署代码文件复制到 web 宿主提供程序 （的生产环境中）显式编译。

> [!NOTE]
> 没有任何影响具有源代码文件在生产服务器上，它们将被忽略。 ASP.NET，以便即使生产服务器上存在的源代码文件将无法访问它们到你的网站的访问者，默认情况下禁止 HTTP 请求路由到源代码文件。 (即，如果用户尝试访问`http://www.yoursite.com/Default.aspx.vb`他们将得到一个错误页面，说明，这些类型的文件-`.vb`以及禁止的文件-。)


[![使用 FTP 客户端将从你的桌面的所需的文件复制到位于 Web 宿主提供程序的 Web 服务器。](deploying-your-site-using-an-ftp-client-vb/_static/image2.png)](deploying-your-site-using-an-ftp-client-vb/_static/image1.png)

**图 1**： 使用 FTP 客户端将从你的桌面的所需的文件复制到位于 Web 宿主提供程序的 Web 服务器 ([单击以查看实际尺寸的图像](deploying-your-site-using-an-ftp-client-vb/_static/image3.png))


在部署你的站点后花些时间测试的站点。 如果你已购买的域名并配置 DNS 设置正确，可以通过输入你的域名访问你的站点。 或者，你的 web 主机提供商应提供你指向你的站点的 url 的外观如下所示*accountname*。*webhostprovider*.com 或*webhostprovider*.com /*accountname*。 例如上折扣 ASP.NET, 我帐户的 URL 是： `http://httpruntime.web703.discountasp.net`。

图 2 显示了已部署的图书评论站点。 请注意，我将在折扣 ASP 上查看它。NET 的服务器，在`http://httpruntime.web703.discountasp.net`。 在此时间点到 Internet 的连接的任何人都无法查看我的网站 ！ 如我们所料，站点的外观和行为就像它未在开发环境中测试它时。

> [!NOTE]
> 如果你收到错误时查看你的应用程序花一些时间，请确保你部署文件的正确的集合。 接下来，检查该错误消息来查看是否它将显示任何有关问题的线索。 接下来，你可以转到 web 主机公司的支持人员或在适当的论坛中发布你的问题[ASP.NET 论坛](https://forums.asp.net/)。


[![簿评审站点是现在可访问任何有 Internet 连接的人。](deploying-your-site-using-an-ftp-client-vb/_static/image5.png)](deploying-your-site-using-an-ftp-client-vb/_static/image4.png)

**图 2**: 簿评审站点是现在可访问任何有 Internet 连接的人 ([单击以查看实际尺寸的图像](deploying-your-site-using-an-ftp-client-vb/_static/image6.png))


## <a name="deploying-the-book-review-web-site-project"></a>部署该书籍查看网站项目

部署时的 ASP.NET 应用程序使用自动编译，如 BookReviewsWSP 网站项目，不会在没有编译的程序集`Bin`文件夹。 因此，必须将 web 应用程序的源代码文件部署到生产环境。 让我们逐步完成此过程。

因为 Web 应用程序项目是明智之举第一次生成应用程序，然后将其部署。 虽然生成网站项目不会创建程序集，它会检查页中的任何编译时错误。 更好的要立即查找这些错误，而不是具有到你的站点访问者为你发现这些 ！

一旦成功生成项目后，使用 FTP 客户端将以下文件复制到 web 宿主提供程序的根网站文件夹。 你可能需要在生产环境中创建相应的文件夹结构。

> [!NOTE]
> 如果你已部署项目，但同时仍想要尝试部署 BookReviewsWSP 项目的 BookReviewsWAP，首先删除所有部署 BookReviewsWAP 时上载 web 服务器上的文件，然后将文件部署为 BookReviewsWSP。


- `~/Default.aspx`
- `~/Default.aspx.vb`
- `~/About.aspx`
- `~/About.aspx.vb`
- `~/Site.master`
- `~/Site.master.vb`
- `~/Web.config`
- `~/Web.sitemap`
- 完整内容`Styles`文件夹
- 完整内容`Images`文件夹 (和子文件夹， `BookCovers`)
- `~/App_Code/BasePage.vb`
- `~/Fiction/Default.aspx`
- `~/Fiction/Default.aspx.vb`
- `~/Fiction/Blaze.aspx`
- `~/Fiction/Blaze.aspx.vb`
- `~/Tech/Default.aspx`
- `~/Tech/Default.aspx.vb`
- `~/Tech/CYOW.aspx`
- `~/Tech/CYOW.aspx.vb`
- `~/Tech/TYASP35.aspx`
- `~/Tech/TYASP35.aspx.vb`

图 3 显示了所需的文件复制后 FileZilla。 如你所见，ASP.NET 源代码文件，如`About.aspx.vb`，一些在本地计算机 （的开发环境中） 和 web 宿主提供程序 （的生产环境中） 上出现的原因代码文件需要时使用自动部署编译。


[![使用 FTP 客户端将从你的桌面的所需的文件复制到位于 Web 宿主提供程序的 Web 服务器](deploying-your-site-using-an-ftp-client-vb/_static/image8.png)](deploying-your-site-using-an-ftp-client-vb/_static/image7.png)

**图 3**： 使用 FTP 客户端将从你的桌面的所需的文件复制到位于 Web 宿主提供程序的 Web 服务器 ([单击以查看实际尺寸的图像](deploying-your-site-using-an-ftp-client-vb/_static/image9.png))


应用程序的编译模型不影响用户体验。 相同的 ASP.NET 页访问并且它们的外观和是否已创建网站，使用 Web 应用程序项目模型或网站项目模型的行为相同。

## <a name="updating-a-web-application-on-production"></a>更新生产中的 Web 应用程序

Web 应用程序开发和部署不是一次性的过程。 例如，创建簿评审网站时我生成的各个页面，并编写了我的个人计算机 （的开发环境中） 中的随附的代码。 达到稳定状态后, 我部署我的应用程序，以便其他人无法访问站点，查看我评论。 但部署将此站点上我不断发展的末尾不标记。 可能添加更多的图书评论或实现新功能，例如允许速率丛书我访问者或保留其自己的注释。 此类增强功能将在开发环境上进行开发，并且完成后，将需要部署。 开发和部署，因此，将循环。 你开发应用程序，并随后部署它。 当站点处于活动状态并在生产环境中时，添加新功能，并随着时间推移，这使得需要重新部署应用程序修复了 bug。 依此类推，依此类推。

如你所料，重新部署你只需将新功能和更改的文件复制的 web 应用程序时。 无需重新部署不变的页或服务器或客户端支持文件 （尽管不没有这样做任何影响）。

> [!NOTE]
> 需要使用显式编译是无论何时向项目添加新的 ASP.NET 页或进行任何代码相关的更改，你需要重新生成项目，这将更新中的程序集时需要牢记的一点`Bin`文件夹。 因此，你将需要更新 web 应用程序在生产 （以及其他新的和更新内容） 时将此更新的程序集复制到生产环境。


此外了解到的任何更改`Web.config`或中的文件`Bin`目录会停止并重启网站的应用程序池。 如果您的会话状态存储使用`InProc`模式 （默认值），然后将你站点的访问者将丢失其会话状态，只要这些密钥的文件进行修改。 若要避免此缺陷，请考虑将存储会话使用`StateServer`或`SQLServer`模式。 有关本主题的详细信息，请阅读[会话状态模式](https://msdn.microsoft.com/library/ms178586.aspx)。

最后，请记住，重新部署应用程序可以花几秒钟到几分钟时间，具体取决于的数量和复制到生产环境所需要的文件的大小。 在此期间用户访问您的网站可能会遇到错误或异常行为。 你可以"关闭"将整个应用程序通过添加一个名为页`App_Offline.htm`到说明你的用户的应用程序的根目录下的站点维护 （或任何） 已关闭，并且将备份功能很快即可。 当`App_Offline.htm`文件存在，ASP.NET 运行时将所有传入请求重定向到该页面。

## <a name="summary"></a>摘要

部署 web 应用程序时，需要将从开发环境的所需的文件复制到生产环境。 依据通过网络传输文件的最常见方式是文件传输协议 (FTP) 和大多数 web 宿主提供程序支持对 web 服务器的 FTP 访问。 在本教程中我们已了解如何使用 FTP 客户端将所需的文件部署到 web 服务器。 部署后，该网站可访问的任何人都使用连接到 Internet ！

尽情享受编程 ！

### <a name="further-reading"></a>其他阅读材料

在本教程中讨论的主题的详细信息，请参阅以下资源：

- [应用\_Offline.htm 和解决"IE 友好错误"功能](https://weblogs.asp.net/scottgu/App_5F00_Offline.htm-and-working-around-the-_2200_IE-Friendly-Errors_2200_-feature)
- [会话状态模式](https://msdn.microsoft.com/library/ms178586.aspx)

>[!div class="step-by-step"]
[上一页](determining-what-files-need-to-be-deployed-vb.md)
[下一页](deploying-your-site-using-visual-studio-vb.md)
