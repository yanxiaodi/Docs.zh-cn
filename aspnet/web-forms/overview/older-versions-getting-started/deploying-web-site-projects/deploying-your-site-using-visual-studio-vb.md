---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-visual-studio-vb
title: "部署你的网站使用 Visual Studio (VB) |Microsoft 文档"
author: rick-anderson
description: "Visual Studio 包含用于部署网站工具。 在本教程中了解有关这些工具的详细信息。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: 977105f3-7987-4e50-8be7-afb53b4ca28a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-visual-studio-vb
msc.type: authoredcontent
ms.openlocfilehash: 362f8391f3352b3abf00045bca0c212cd850b17f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="deploying-your-site-using-visual-studio-vb"></a>部署你的网站使用 Visual Studio (VB)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载代码](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_04_VB.zip)或[下载 PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial04_DeployingViaVS_vb.pdf)

> Visual Studio 包含用于部署网站工具。 在本教程中了解有关这些工具的详细信息。


## <a name="introduction"></a>介绍

前面的教程介绍了如何部署到 web 主机提供商的简单 ASP.NET web 应用。 具体而言，本教程说明了如何使用 FileZilla 等 FTP 客户端将从开发环境的所需的文件传输到生产环境。 Visual Studio 还提供内置的工具可简化部署到 web 主机提供商。 本教程检查两个这些工具: 复制网站工具，其中你可以将文件移到和从远程 web 服务器使用 FTP 或 FrontPage 服务器扩展;和发布工具，将整个网站复制到指定位置。


> [!NOTE]
> 由 Visual Studio 提供其他与部署相关的工具包括[Web 安装程序项目](https://msdn.microsoft.com/library/wx3b589t.aspx)和[Web 部署项目](https://www.microsoft.com/downloads/details.aspx?FamilyId=0AA30AE8-C73B-4BDD-BB1B-FE697256C459&amp;displaylang=en)外接程序。 Web 安装程序项目包网站的内容和配置信息导出成单一的 MSI 文件。 此选项将在 intranet 内部署的网站或公司的销售的预打包的 web 应用程序在其自己的 web 服务器上安装客户最为有用。 Web 部署项目外接程序是 Visual Studio 外接程序，它方便了之间的指定配置差异生成用于开发环境和生产环境。 本教程系列; 不讨论 web 安装程序项目Web 部署项目中总结了[*常见配置差异之间开发和生产*](common-configuration-differences-between-development-and-production-vb.md)教程。


## <a name="deploying-your-site-using-the-copy-web-site-tool"></a>部署站点上使用复制网站工具

Visual Studio 的复制网站工具是在功能上类似于独立的 FTP 客户端。 简而言之，复制网站工具，可连接到远程网站通过 FTP 或 FrontPage 服务器扩展。 复制网站用户界面包含类似于 FileZilla 的用户界面，两个窗格： 左窗格中列出的本地文件，而右窗格中列出这些文件在目标服务器上的。

> [!NOTE]
> 复制网站工具仅适用于网站项目。 当您正在使用 Web 应用程序项目，visual Studio 确实提供此工具。


让我们看看使用复制网站工具发布到生产环境簿查看应用程序。 复制网站工具仅适用于使用的网站项目模型的项目，因为我们仅可以检查与 BookReviewsWSP 项目中使用此工具。 打开该项目。

通过单击复制网站图标 （此图标圆形图 1 中） 的解决方案资源管理器; 启动复制网站工具项目或者，你可以从网站菜单上选择复制网站选项。 这两种方法启动显示在图 1 中; 的复制网站用户界面图 1 中的，左窗格中仅被填充，因为我们尚未连接到远程服务器。


[![复制网站工具的用户界面是划分到两个窗格](deploying-your-site-using-visual-studio-vb/_static/image2.png)](deploying-your-site-using-visual-studio-vb/_static/image1.png)

**图 1**: 复制网站工具的用户界面是划分到两个窗格 ([单击以查看实际尺寸的图像](deploying-your-site-using-visual-studio-vb/_static/image3.png))


若要部署我们的站点中，我们需要首先连接到 web 宿主提供程序。 单击顶部的复制网站用户界面连接按钮。 此时将显示打开网站对话框，在图 2 所示。

你可以通过从左侧选择四个选项之一来连接到目标网站：

- **文件系统**-选择此选项可从你的计算机将您的网站部署到可访问的文件夹或网络共享。
- **本地 IIS** -使用此选项将站点部署到您的计算机上安装的 IIS web 服务器。
- **FTP 站点**-连接到远程网站使用 FTP。
- **远程站点**-连接到远程网站使用 FrontPage 服务器扩展。

大多数 web 宿主提供程序支持 FTP，但更少提供 FrontPage 服务器扩展支持。 为此，我已选择了 FTP 站点的选项，然后输入连接信息，如图 2 中所示。


[![指定目标网站](deploying-your-site-using-visual-studio-vb/_static/image5.png)](deploying-your-site-using-visual-studio-vb/_static/image4.png)

**图 2**： 指定目标网站 ([单击以查看实际尺寸的图像](deploying-your-site-using-visual-studio-vb/_static/image6.png))


连接后，复制网站工具加载在右窗格中的远程站点文件，并指示每个文件的状态： 新的、 已删除、 已更改或未更改。 你可以将文件从本地站点复制到远程站点或进行相反的操作。

让我们添加一个新页到 BookReviewsWSP 项目，然后将其部署，以便我们可以看到操作中的复制网站工具。 Visual Studio 中名为的根目录中创建新的 ASP.NET 页`Privacy.aspx`。 使用母版页页面`Site.master`并将站点的隐私策略添加到此页。 创建此页后，图 3 显示 Visual Studio。


[![添加新的页名为&lt;代码&gt;Privacy.aspx&lt;/c o&gt;到网站的根文件夹](deploying-your-site-using-visual-studio-vb/_static/image8.png)](deploying-your-site-using-visual-studio-vb/_static/image7.png)

**图 3**： 添加新的页名为`Privacy.aspx`到网站的根文件夹 ([单击以查看实际尺寸的图像](deploying-your-site-using-visual-studio-vb/_static/image9.png))


接下来，返回到复制网站用户界面。 如图 4 所示，左窗格中现在包含新的文件-`Policy.aspx`和`Policy.aspx.vb`。 更重要的是，这些文件都标有一个箭头图标和一个状态的新的该值指示它们存在本地站点上但不是能在远程站点。


[![复制网站工具包括新建&lt;代码&gt;Privacy.aspx&lt;/c o&gt;在其左侧窗格中的页](deploying-your-site-using-visual-studio-vb/_static/image11.png)](deploying-your-site-using-visual-studio-vb/_static/image10.png)

**图 4**： 复制网站工具包括新建`Privacy.aspx`在其左侧窗格中的页 ([单击以查看实际尺寸的图像](deploying-your-site-using-visual-studio-vb/_static/image12.png))


若要部署新文件选择它们，然后单击箭头图标，以将它们传输到远程站点。 传输完成后`Policy.aspx`和`Policy.aspx.vb`文件存在于状态未更改与本地和远程站点上。

列出新的文件，以及复制网站工具突出显示了与不同，在本地和远程站点之间的任何文件。 若要在操作中查看，请返回到`Privacy.aspx`页上，并将几个更多字添加到的隐私策略。 保存页，然后返回到复制网站工具。 如图 5 所示，`Privacy.aspx`页的左窗格中的状态为已更改，该值指示它为与远程站点不同步。


[![复制网站工具指示&lt;代码&gt;Privacy.aspx&lt;/c o&gt;页已更改](deploying-your-site-using-visual-studio-vb/_static/image14.png)](deploying-your-site-using-visual-studio-vb/_static/image13.png)

**图 5**： 复制网站工具指示`Privacy.aspx`页已更改 ([单击以查看实际尺寸的图像](deploying-your-site-using-visual-studio-vb/_static/image15.png))


复制网站工具还指示自上次复制操作以来是否已删除文件。 删除`Privacy.aspx`从本地项目并刷新复制网站工具。 `Privacy.aspx`和`Privacy.aspx.vb`文件保持在左窗格中，列出，但具有，该值指示是否已将其删除自上次复制操作的状态为已删除。

## <a name="publishing-a-web-application"></a>发布 Web 应用程序

部署 web 应用程序从 Visual Studio 中的另一种方法是使用发布选项，这是可通过生成菜单访问。 发布选项显式编译应用程序，然后复制到指定的远程站点所需的文件的所有。 正如我们很快将看到的则发布选项是更钝比复制网站工具。 复制网站工具，可以检查本地和远程站点上的文件，并允许您上载或下载单个文件，根据需要而发布选项部署整个 web 应用程序。


除了将所有所需的文件复制到指定的远程站点，发布选项还可以显式编译应用程序。 假设 Web 应用程序项目只需要显式编译它应该为不足为奇了发布选项适用于 Web 应用程序项目。 新增功能可能有点令人惊讶是发布选项也是适用于网站项目。 中所述[*确定什么文件需要部署到*](determining-what-files-need-to-be-deployed-vb.md)教程中，网站项目显式编译完成的过程称为*预编译*。 本教程侧重于与 Web 应用程序项目; 使用发布选项将来的教程将探索如何预编译，此时我们将返回以查看使用的网站项目的发布选项。

> [!NOTE]
> 对于网站项目和 Web 应用程序项目的 Visual Studio 中提供的发布选项时，Visual Web Developer 仅提供 Web 应用程序项目的发布选项。


让我们看一下部署簿评审应用程序使用发布选项。 首先在 Visual Studio 中打开 BookReviewsWAP （Web 应用程序项目）。 从发布菜单上选择生成 BookReviewsWAP 项目。 这会打开一个对话框，提示输入目标位置，在其他配置选项中 （请参阅图 6）。 更像使用复制网站工具可以输入指向本地文件夹，在 IIS 上的本地网站，支持的 FrontPage 服务器扩展或 FTP 服务器地址的远程网站的位置。 你可以选择是否要替换的已部署的文件的远程 web 服务器上的文件，或删除所有在发布前远程站点上的内容。 你还可以指定是否将复制：

- 仅在项目中运行应用程序，它将省略不需要的源代码和与项目相关的文件所需文件。
- 所有项目文件，其中包括源代码文件和 Visual Studio 项目文件，都如解决方案文件。
- 在源项目文件夹中，将所有文件都复制而不考虑是否要包括在项目中的源项目文件夹中的所有文件。

此外还有一个选项以将内容上载`App_Data`文件夹。


[![指定目标网站](deploying-your-site-using-visual-studio-vb/_static/image17.png)](deploying-your-site-using-visual-studio-vb/_static/image16.png)

**图 6**： 指定目标网站 ([单击以查看实际尺寸的图像](deploying-your-site-using-visual-studio-vb/_static/image18.png))


对于簿查看应用程序远程站点包含复制通过复制网站工具 BookReviewsWSP 项目时，部署的文件。 因此，让我们首先删除所有现有内容的发布选项。 此外，让我们只复制所需的文件，而不是使用不需要的代码和项目源文件塞满生产环境。 指定这些选项后, 单击发布按钮。 接下来的几秒内通过 Visual Studio 会将部署所需的文件到目标站点中，在输出窗口中显示其进度。

发布操作完成后，图 7 显示 FTP 站点上的文件。 请注意，已上载仅的标记页面，并且需要服务器端和客户端的支持文件。


[![仅将所需的文件发布到生产环境](deploying-your-site-using-visual-studio-vb/_static/image20.png)](deploying-your-site-using-visual-studio-vb/_static/image19.png)

**图 7**： 仅所需文件发布到生产环境 ([单击以查看实际尺寸的图像](deploying-your-site-using-visual-studio-vb/_static/image21.png))


发布选项是比复制网站工具的较低一些略有区别的工具。 复制网站工具允许你检查本地和远程站点上的文件并查看它们有何不同，而发布选项提供了任何此类接口。 此外，该复制网站工具使您能够一次性更改，上载或删除单个文件。 发布选项不允许此类细化的控制;相反，它将发布*整个*应用程序。 此行为有其优点和缺点。 在加上一侧，你了解何时使用你不会忘记上载重要文件发布选项。 但考虑如果进行少量更改到非常大的网站-使用发布选项无法更新该页面或两个已修改，但必须等待尽管 Visual Studio 部署的整个站点，而是会发生什么情况。

并不少见，才会生产和开发环境之间不同，其内容的某些文件。 重要的例子就是应用程序的配置文件， `Web.config`。 因为发布选项会盲目地将复制的 web 应用程序文件时，它将与在开发环境中的版本覆盖生产环境中的自定义的配置文件。 后续教程探讨此主题会进一步，并提供用于部署的 web 应用，这种差异存在时的提示。

## <a name="summary"></a>摘要

将网站部署涉及将从开发环境的所需的文件复制到生产环境。 前面的教程介绍了如何使用 FileZilla 等 FTP 客户端传输文件。 本教程检查 Visual Studio 中的两个部署工具: 复制网站工具和发布选项。 它列出本地计算机和可以轻松地上载或下载两台计算机之间的文件的指定远程计算机上的文件的双窗格界面，该复制网站工具是类似于 FTP 客户端。 发布选项是一个更钝工具显式将编译该项目，然后再部署到指定目标整个应用程序。

尽情享受编程 ！

### <a name="further-reading"></a>其他阅读材料

在本教程中讨论的主题的详细信息，请参阅以下资源：

- [复制使用复制网站工具创建网站](https://msdn.microsoft.com/library/1cc82atw.aspx)
- [I： 如何部署使用复制网站工具网站](../../../videos/how-do-i/how-do-i-deploy-a-web-site-using-the-copy-web-site-tool.md)（视频）
- [如何： 发布 Web 应用程序项目](https://msdn.microsoft.com/library/aa983453.aspx)
- [如何： 发布网站](https://msdn.microsoft.com/library/20yh9f1b.aspx)
- [安装程序和 Visual Studio 中的部署项目](https://msdn.microsoft.com/library/wx3b589t.aspx)

>[!div class="step-by-step"]
[上一页](deploying-your-site-using-an-ftp-client-vb.md)
[下一页](common-configuration-differences-between-development-and-production-vb.md)
