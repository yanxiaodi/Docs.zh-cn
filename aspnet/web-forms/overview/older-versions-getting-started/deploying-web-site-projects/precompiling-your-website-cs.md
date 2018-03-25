---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/precompiling-your-website-cs
title: "预编译你的网站 (C#) |Microsoft 文档"
author: rick-anderson
description: "Visual Studio 为 ASP.NET 开发人员提供了两种类型的项目： Web 应用程序项目 (Wap) 和网站项目 (WSPs)。 一个主要区别 betwe..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: ecd5a4de-beb7-4d1d-bbbb-e31003633267
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/precompiling-your-website-cs
msc.type: authoredcontent
ms.openlocfilehash: f31f470b4d2b6736b98c0b7d88ea7a53ad1438b9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="precompiling-your-website-c"></a>预编译你的网站 (C#)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载代码](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_15_CS.zip)或[下载 PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial15_Precompiling_cs.pdf)

> Visual Studio 为 ASP.NET 开发人员提供了两种类型的项目： Web 应用程序项目 (Wap) 和网站项目 (WSPs)。 两种项目类型之间的主要区别之一是 Wap 必须显式在部署之前进行编译而 WSP 中的代码都可以在 web 服务器上会自动编译的代码。 但是，很可能在部署之前 WSP 预编译。 本教程介绍预编译的好处，并演示如何预编译网站从 Visual Studio 中和从命令行。


## <a name="introduction"></a>介绍

Visual Studio 为 ASP.NET 开发人员提供了两种不同的项目类型： Web 应用程序项目 (WAP) 和网站项目 (WSP)。 这些项目类型之间的主要区别之一是 Wap 需要*显式编译*而 WSPs 使用*自动编译*，默认情况下。 与 Wap，web 应用程序的代码编译为一个程序集，在网站的创建`Bin`文件夹。 部署需要复制标记内容 ( `.aspx.ascx`，和`.master`文件) 在项目中中的程序集以及`Bin`文件夹; 代码隐藏类文件本身不需要部署。 另一方面，通过将标记页和其相应的代码隐藏类复制到生产环境部署 WSPs。 按需 web 服务器上进行编译的代码隐藏类。

> [!NOTE]
> 引用的"显式编译 Versus 自动编译"一节中[*确定什么文件需要部署到*教程](determining-what-files-need-to-be-deployed-cs.md)有关更多背景项目之间的差异模型、 显式和自动编译和编译模型如何影响部署。


自动编译选项是易于使用。 没有显式编译步，仅将文件已修改需要若要部署，而显式编译要求部署的更改的标记页面，并且只编译的程序集。 但是，自动部署具有两个潜在缺点：

- 因为必须自动编译页面，当它们首次访问时，可能会有短暂而明显延迟在部署后首次请求 ASP.NET 页时。
- 自动编译要求这两种声明性标记和源代码是在 web 服务器上存在。 如果你计划在销售给客户将在其 web 服务器安装 web 应用程序，这可以是一个毫无吸引力的选项。

如果两个以上不足之处是交易断路器，可以使用任一切换到 WAP 模型或*预编译*部署之前 WSP。 本教程检查最适合用于托管网站的预编译选项，并将指导完成的预编译过程和预编译网站的部署。

## <a name="an-overview-of-aspnet-code-generation-and-compilation"></a>ASP.NET 代码生成和编译的概述

我们查看可用的预编译选项之前，让我们先介绍一下的代码生成和编译的 ASP.NET 页请求第一次，因为已创建或上次更新时间时发生。 如您所知，将 ASP.NET 页组成两个部分： 中的声明性标记`.aspx`文件; 以及源代码部分，通常在一个单独的代码隐藏类文件 (`.aspx.cs`)。 当请求 ASP.NET 页时由运行时执行的步骤取决于应用程序的编译模型。

与 Wap，页的源的代码必须显式编译到然后部署到单个程序集。 在部署期间，此程序集和各种标记页面将复制到生产环境。 当请求到达时到 ASP.NET 页的 web 服务器时，运行时创建的页面的代码隐藏类的实例并调用其`ProcessRequest`方法，它启动的页面生命周期，并从根本上讲，生成页面内容，就会归还请求者。 运行时可以使用 ASP.NET 页的代码隐藏类，因为到部署之前的程序集中已编译的代码隐藏类。

使用 WSPs 和自动编译，则在部署之前没有显式编译步骤。 相反，部署涉及到将声明性和源代码内容复制到生产环境。 当请求到达时到 ASP.NET 页的 web 服务器第一次由于已创建或上次更新时间页上时，运行时必须先编译为程序集的代码隐藏类。 此编译的程序集保存在的文件夹`%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files`，但可以通过自定义此文件夹的位置`<compilation tempDirectory="" />`元素`<system.web>`，通常在`Web.config`。 由于程序集保存到磁盘，它不需要在同一页的后续请求重新编译。

> [!NOTE]
> 在使用自动编译，因为要花片刻时间来编译页的代码并将保存到生成的程序集的服务器的站点中的第一次 （或第一次，因为它已更改） 请求页面如你所料，是连接期间的轻微延迟磁盘。


简单地说，与显式编译需要编译网站的源代码之前部署，无需执行此步骤中保存运行时。 与自动编译运行时处理第一个访问此页编译页的代码的源代码，但使用略初始化成本，因为它已创建或上次更新时间。

但什么有关 ASP.NET 页的声明性一部分 (`.aspx`文件)？ 很明显没有之间的关系`.aspx`文件和它们的代码隐藏类，在声明性的标记中定义的 Web 控件中的代码，可在代码中访问。 它也是很明显，中的内容`.aspx`文件可能严重影响生成的页面的呈现的标记。 如何运行时的工作原理与文本、 HTML 和 Web 控件语法定义中`.aspx`要生成请求的页面文件的呈现内容？

我不想获取太搁置上低级别的实现详细信息，其中 Wap 和 WSPs 之间的不同，但简而言之由运行时自动生成的类文件，其中包含作为受保护的成员和方法的各种 Web 控件。 此生成的文件作为实现*分部类*到相应的代码隐藏类。 ([分部类](http://www.dotnet-guide.com/partialclasses.html)允许单个类的内容，以分散在多个文件。)因此，在两个位置定义的代码隐藏类： 在`.aspx.cs`创建和运行时创建此自动生成的类中的文件。 此自动生成的类存储在`%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files`文件夹。

重要占用此处是 ASP.NET 页呈现由运行时，这两个其声明性和源代码部分必须编译到程序集。 与 Wap，源代码显式编译成程序集之前部署，但声明性标记必须仍转换为代码和通过在 web 服务器上运行时编译。 使用 WSPs 使用自动编译，源代码和声明性标记需要编译的 web 服务器。

很可能使用 WSP 模型使用显式编译。 可以显式类似的源的代码部分，编译使用 WAP 模型。 更重要的是，您还可以编译的声明性的标记。

## <a name="precompilation-options"></a>预编译选项

.NET Framework 附带[ASP.NET 编译工具 (`aspnet_compiler.exe`)](https://msdn.microsoft.com/library/ms229863.aspx) ，使您能够编译的 ASP.NET 应用程序使用 WSP 模型生成的源代码 （和甚至内容）。 此工具随.NET Framework 2.0 版发布，且位于`%WINDIR%\Microsoft.NET\Framework\v2.0.50727`文件夹; 它可以从命令行使用或在 Visual Studio 中从启动通过生成菜单的发布网站选项。

编译工具提供了两种常规形式的编译： 就地预编译和部署的预编译。 在运行就地预编译`aspnet_compiler.exe`从命令行工具，然后指定您的计算机上的虚拟目录或所在的网站的物理路径的路径。 编译工具进行编译在项目中，存储的已编译的版本中的每个 ASP.NET 页`%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files`一样如果具有每个已访问的网页首次从浏览器的文件夹。 就地预编译可以加快对你的站点上的新部署 ASP.NET 页，因为它减少了从无需执行此步骤运行时的第一个请求。 但是，因为它需要能够运行程序从 web 服务器的命令行的就地预编译不用于托管网站的大部分。 在共享宿主环境中不允许此访问级别。

> [!NOTE]
> 有关就地预编译的详细信息，请参阅[How To： 预编译的 ASP.NET Web 站点](https://msdn.microsoft.com/library/ms227972.aspx)和[ASP.NET 2.0 中的预编译](http://www.odetocode.com/Articles/417.aspx)。


而不是编译到的网站中的页`Temporary ASP.NET Files`文件夹中，为部署的预编译编译到的目录的选择以及一种格式，可以部署到生产环境中的页。

有两种风格的预编译为我们在本教程中浏览的部署： 预编译可更新用户界面，并利用非可更新用户界面的预编译。 可更新用户界面的预编译离开中的声明性标记`.aspx`， `.ascx`，和`.master`文件，从而允许开发人员可以查看和，如果需要，修改在生产服务器上的声明性标记。 与非可更新用户界面的预编译生成`.aspx`已失效的任何内容并删除的页`.ascx`和`.master`文件，从而隐藏的声明性的标记和更改从禁止开发人员生产环境。

### <a name="precompiling-for-deployment-with-an-updatable-user-interface"></a>预编译为可更新用户界面的部署

了解部署预编译的最佳方法是查看示例，请参见操作。 让我们将使用一种可更新用户界面部署簿评审 WSP 预编译。 可以从 Visual Studio 生成菜单或从命令行调用 ASP.NET 编译工具。 本节讨论中使用工具从 Visual Studio;从命令行运行编译器工具考察"预编译从命令行"部分。

在 Visual Studio 中打开簿评审 WSP，转到生成菜单中，并选择发布网站菜单选项。 这将启动发布网站对话框中 (请参阅**图 1**)，你可以在其中指定的目标位置、 是否可更新的、 预编译的站点的用户界面和其他编译器工具选项。 目标位置可以是远程 web 服务器或 FTP 服务器，但现在选择你的计算机的硬盘驱动器上的文件夹。 因为我们想要预编译具有可更新用户界面的站点，将"允许此预编译的网站为可更新"复选框已选中，然后单击确定。

[![](precompiling-your-website-cs/_static/image2.png)](precompiling-your-website-cs/_static/image1.png)

**图 1**: ASP.NET 编译工具将预编译你的网站指定的目标位置  
 ([单击以查看实际尺寸的图像](precompiling-your-website-cs/_static/image3.png))

> [!NOTE]
> 生成菜单中的发布网站选项不可用在 Visual Web Developer。 如果你使用 Visual Web Developer 需要使用"预编译从命令行"部分将介绍在 ASP.NET 编译工具的命令行版本。


在预编译网站后, 导航到你在发布网站对话框中输入的目标位置。 花些时间比较你的网站的内容的此文件夹的内容。 **图 2**显示图书评论网站文件夹。 请注意，它包含`.aspx`和`.aspx.cs`文件。 另请注意，`Bin`目录都包含一个文件， `Elmah.dll`，我们在添加[前面教程](logging-error-details-with-elmah-cs.md)

[![](precompiling-your-website-cs/_static/image5.png)](precompiling-your-website-cs/_static/image4.png)

**图 2**： 项目目录包含`.aspx`和`.aspx.cs`文件;`Bin`文件夹只包含`Elmah.dll`  
 ([单击以查看实际尺寸的图像](precompiling-your-website-cs/_static/image6.png))

**图 3**显示其内容由 ASP.NET 编译工具创建的目标位置文件夹。 此文件夹不包含任何代码隐藏文件。 此外，此文件夹`Bin`目录中包括多个程序集和第二个`.compiled`文件除了`Elmah.dll`程序集。

[![](precompiling-your-website-cs/_static/image8.png)](precompiling-your-website-cs/_static/image7.png)

**图 3**： 目标位置文件夹包括有关部署的文件  
 ([单击以查看实际尺寸的图像](precompiling-your-website-cs/_static/image9.png))

与 Wap 中的显式编译，部署过程预编译不会创建一个程序集的整个站点。 相反，它批处理一起几个页面到每个程序集。 它还会将编译`Global.asax`到其自己的程序集，以及在任何类文件 （如果存在）`App_Code`文件夹。 为 ASP.NET 保存声明性的标记文件 web 页、 用户控件和母版页 (`.aspx`， `.ascx`，和`.master`文件，分别) 作为复制的目标位置目录是。 同样，`Web.config`文件将直接通过复制以及任何静态文件，如图像、 CSS 类和 PDF 文件。 有关的更正式说明编译工具如何处理各种文件类型，请参阅[ASP.NET 预编译期间文件处理](https://msdn.microsoft.com/library/e22s60h9.aspx)。

> [!NOTE]
> 你可以指示编译工具以通过检查从发布网站对话框中的"使用固定命名和单页程序集"复选框创建 ASP.NET 页、 用户控件或母版页每个程序集。 拥有编译到其自己的程序集中每个 ASP.NET 页使部署更灵活地细化控制。 例如，如果你已更新单个 ASP.NET 网页，且需要来部署所做的更改，你需要仅在部署该页面的`.aspx`文件和关联的程序集到生产环境。 请查阅[How To： 使用 ASP.NET 编译工具生成的固定名称](https://msdn.microsoft.com/library/ms228040.aspx)有关详细信息。


目标位置目录还包含一个文件，即未预编译的 web 项目的一部分`PrecompiledApp.config`。 此文件通知应用程序进行预编译的 ASP.NET 运行以及是否它已预编译使用可更新或中午可更新 UI。

最后，花些时间打开之一`.aspx`中使用 Visual Studio 或首选文本编辑器的目标位置的文件。 当预编译为可更新用户界面的部署，目标位置目录中的 ASP.NET 页包含作为网站中的相应文件的完全相同标记。

### <a name="precompiling-for-deployment-with-a-non-updatable-user-interface"></a>预编译为使用的非可更新用户界面的部署

ASP.NET 编译器工具还可以使用预编译网站具有非可更新 UI 的部署。 预编译具有非可更新 UI 的站点即可像预编译可更新的用户界面，主要区别在于 ASP.NET 页、 用户控件和目标目录中的主控页被剥离其标记的工作。 预编译具有非可更新 UI 的部署的网站，请从生成菜单中，选择发布 Web 站点选项，但请取消选中"允许此预编译的网站为可更新"选项 (请参阅**图 4**)。

[![](precompiling-your-website-cs/_static/image11.png)](precompiling-your-website-cs/_static/image10.png)

**图 4**： 取消选中"允许此预编译的网站为可更新"选项对预编译与非可更新 UI  
 ([单击以查看实际尺寸的图像](precompiling-your-website-cs/_static/image12.png))

**图 5**在与非可更新用户界面预编译后将显示目标位置文件夹。

[![](precompiling-your-website-cs/_static/image14.png)](precompiling-your-website-cs/_static/image13.png)

**图 5**： 具有非可更新 UI 的部署的目标位置文件夹  
 ([单击以查看实际尺寸的图像](precompiling-your-website-cs/_static/image15.png))

比较**图 3**到**图 5**。 虽然两个文件夹可能看起来相同，但请注意非可更新的 UI 文件夹缺少主页上， `Site.master`。 和而**图 5**包括各种的 ASP.NET 页中，如果你查看这些文件，你将看到它们已被其声明性的标记已删除和替换为占位符文本的内容:"这是由生成的标记文件预编译的工具，并且不应删除 ！"

[![](precompiling-your-website-cs/_static/image17.png)](precompiling-your-website-cs/_static/image16.png)

**图 5**： 已从 ASP.NET 页中删除的声明性的标记

`Bin`中的文件夹**图 3**和**5**更大不相同。 程序集，除了`Bin`文件夹中的**图 5**包括`.compiled`每个 ASP.NET 页、 用户控件和母版页文件。

预编译的非可更新 UI 的站点是在你不希望 ASP.NET 页的内容的个人或公司用户安装或管理在生产环境中的网站的用户要修改的情况下有用。 如果生成销售给客户以便在其自己的 web 服务器上安装 ASP.NET web 应用程序时，你可能想要确保，它们不修改你的站点的外观和感觉通过直接编辑`.aspx`将它们装运的页。 通过预编译你的网站具有非可更新 UI，你将发运占位符`.aspx`作为安装过程中，从而阻止你检查或修改其内容的客户的一部分的页。

### <a name="precompiling-from-the-command-line"></a>从命令行预编译

在后台，Visual Studio 的发布网站对话框时，将调用 ASP.NET 编译工具 (`aspnet_compiler.exe`) 预编译网站。 或者，你可以调用此工具从命令行。 事实上，如果你使用 Visual Web Developer 然后你将需要编译器工具从命令行运行，因为 Visual Web Developer 的生成菜单不包括发布 Web 站点选项。

若要使用命令行中的编译器工具，启动到命令行删除，然后导航到框架目录中，通过`%WINDIR%\Microsoft.NET\Framework\v2.0.50727`。 接下来，在命令行中输入以下语句：

`aspnet_compiler -p "physical_path_to_app" -v / -f -u "target_location_folder"`

上面的命令将启动 ASP.NET 编译器工具 (`aspnet_compiler.exe`) 和通过`-p`切换，请指示它预编译网站位于*物理\_路径\_到\_应用*;此值将为类似`C:\MySites\BookReviews`，并应由引号分隔。

`-v`开关指定的虚拟目录的站点。 如果你的站点注册为 IIS 元数据库中的默认网站，则可以省略`-p`开关，并只需指定的虚拟目录的应用程序。 如果你使用`-p`切换，值继续`-v`交换机指示网站的根和用于解决应用程序根目录的引用。 例如，如果指定的值`-v /MySite`然后在应用程序中引用`~/path/file`将解析为`~/MySite/path/file`。 因为图书评论站点位于在我的 web 托管公司的根目录处我已经使用了该交换机`-v /`。

`-f`开关，如果存在，将指示编译工具覆盖*目标\_位置\_文件夹*目录已存在的情况。 如果省略`-f`交换机和目标位置文件夹已存在，编译工具将退出并显示错误:"错误 ASPRUNTIME： 目标目录不为空。 请手动将其删除或选择其他目标。"

`-u`开关，如果存在，会通知该工具以创建可更新的用户界面。 省略此开关预编译站点使用的非可更新用户界面。

最后，*目标\_位置\_文件夹*是目标位置目录; 的物理路径此值将为类似`C:\MySites\Output\BookReviews`，并应由引号分隔。

## <a name="deploying-the-precompiled-website"></a>将预编译的网站部署

此时，我们已了解如何使用 ASP.NET 编译工具预编译网站使用这两个的可更新而非可更新用户界面选项。 但是，我们的示例为止具有预编译网站到本地文件夹，而不适用于生产环境。 好消息是部署预编译的网站轻松，并可以通过 Visual Studio 或通过某些其他文件复制机制，例如从独立的 FTP 客户端。

发布网站对话框中 (在第一次显示**图 1**) 具有一个目标位置选项，指示预编译的网站文件复制到的位置。 此位置可以是远程 web 服务器或 FTP 服务器。 此文本框中输入远程服务器对预编译，并且将网站部署到在一个步骤中指定的服务器。 或者，可以预编译网站到本地文件夹，然后手动将该文件夹的内容复制到生产环境中通过 FTP 或一些其他方法。

具有自动部署为通过 Visual Studio 的发布网站对话框的预编译的网站非常简单的站点在开发和生产环境之间没有配置差别。 但是，如中另有说明[*常见配置差异之间开发和生产*教程](common-configuration-differences-between-development-and-production-cs.md)是很常见的存在这种差异。 例如，图书评论 web 应用程序在开发环境中比在生产环境中使用不同的数据库。 当 Visual Studio 将网站发布到盲目地复制查找配置文件信息是否包含在开发环境中的远程服务器。

为站点配置开发和生产环境之间的差异可能是最好预编译站点到本地目录，复制的特定于生产的配置文件，然后将复制到的预编译输出的内容生产。

有关从开发环境的文件复制到生产环境中使用刷新程序参考[*部署您的网站使用 FTP 客户端*](deploying-your-site-using-an-ftp-client-cs.md)和[ *部署你的网站使用 Visual Studio* ](determining-what-files-need-to-be-deployed-cs.md)教程。

## <a name="summary"></a>摘要

ASP.NET 支持两种编译模式： 自动和显式。 如前面的教程中所述，Web 应用程序项目 (Wap) 将使用显式编译，而网站项目 (WSPs) 默认情况下使用自动编译。 但是，很可能以显式编译，方法是使用 ASP.NET 编译工具部署之前 WSP。

本教程侧重于部署支持的编译工具的预编译。 当部署，预编译，编译工具可创建的目标位置文件夹、 编译指定的 web 应用程序的源代码，并将复制这些编译到的目标位置文件夹的程序集和内容的文件。 可以配置编译工具来创建可更新或非可更新用户界面。 当与非可更新用户界面选项预编译，将删除内容文件中的声明性标记。 简而言之，预编译允许你部署基于网站项目的应用程序，而不包括任何源代码文件和使用声明性的标记中删除，如果需要。

尽情享受编程 ！

### <a name="further-reading"></a>其他阅读材料

在本教程中讨论的主题的详细信息，请参阅以下资源：

- [ASP.NET Web 站点预编译](https://msdn.microsoft.com/library/ms228015.aspx)
- [代码隐藏和 ASP.NET 2.0 中的编译](https://msdn.microsoft.com/magazine/cc163675.aspx)
- [在 ASP.NET 中的预编译](http://www.odetocode.com/Articles/417.aspx)
- [在 ASP.NET 中的预编译的站点选项](http://www.dotnetperls.com/precompiled)

>[!div class="step-by-step"]
[上一页](logging-error-details-with-elmah-cs.md)
[下一页](users-and-roles-on-the-production-website-cs.md)
