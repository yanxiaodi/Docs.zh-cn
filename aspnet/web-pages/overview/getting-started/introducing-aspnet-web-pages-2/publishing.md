---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
title: "引入的 ASP.NET Web Pages-通过使用 WebMatrix 发布的站点 |Microsoft 文档"
author: tfitzmac
description: "本教程是文章中介绍的 ASP.NET Web Pages 和 Microsoft WebMatrix 的教程集最后一部分。 还会讨论如何以发布站点 t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 7e85c70e-1a88-4408-8b3d-29611c7713ed
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
msc.type: authoredcontent
ms.openlocfilehash: 1e718c92a2f94df50fcf7af6859917746a4982ac
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="introducing-aspnet-web-pages---publishing-a-site-by-using-webmatrix"></a>引入了 ASP.NET Web 页-通过使用 WebMatrix 发布站点
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> 本教程是文章中介绍的 ASP.NET Web Pages 和 Microsoft WebMatrix 的教程集最后一部分。 它讨论如何将你的站点发布到 Internet，以便其他人可以使用它。 它假定你已完成通过系列[为 ASP.NET 网页站点创建一致的查看](https://go.microsoft.com/fwlink/?LinkId=251585)。
> 
> 你将了解如何以发布你的站点使用：
> 
> - Microsoft Azure
> - Web 托管公司


## <a name="about-publishing-your-site"></a>有关发布你的站点

到目前为止，则你已完成你的本地计算机上，包括测试页面的所有工作。 若要运行你*.cshtml*页，你已使用内置到 WebMatrix 中，即 IIS Express web 服务器。 但这是当然没有人可以看到已创建不同的站点。 若要让其他人使用你的站点，你必须将其发布到 Internet。

除非你已具有对公共 web 服务器的访问，发布意味着你必须有一个用于帐户*云平台*或*托管提供商*。 云平台，如 Microsoft Azure 为你的应用程序提供按需基础结构。 托管提供程序是的公司拥有可公开访问的 web 服务器和，将出租你空间为您的网站。 托管计划运行从每月只需几美元 （或甚至免费） 对于小型网站到数以百计的大量商业网站的每月的资金。

> [!NOTE]
> 你可能有权通过 internet 服务提供商 (ISP) 用于获取在家里 internet 服务的公共 web 服务器。 但是，你托管提供商必须支持 ASP.NET 网页。 许多 Isp 不这样做，但最好始终检查。


在本教程中，我们将向你提供如何将发布的概述。 因为流程有点不同于每个宿主提供程序，则它是不可行涵盖所有情况，提供准确的详细信息。 但，你将获取进程的工作原理的一个好主意。

本教程包含四个部分：

1. [设置默认页](#defaultpage)
2. 发布 （选择以下项之一）  
 a. [你的站点发布到 Microsoft Azure](#azure)  
 b. [将你的站点发布到 Web 托管公司](#host)
3. [更新实时站点： 重新发布](#update)

<a id="defaultpage"></a>
## <a name="setting-up-the-default-page"></a>设置默认页

当用户导航到您的网站的基址时，向用户显示你的站点的默认页面。 例如，如果在 www.contoso.com 情况下，Default.htm 设为该站点的默认页，然后导航到**www.contoso.com**等同于导航到**www.contoso.com/Default.htm**。

目前，您的网站使用**Default.cshtml**作为默认页。 此页是相当不错的默认页，但在本教程中你未添加任何内容到该页面以便它将显示一个空白页。 打开 Default.cshtml 并将内容替换为下面的代码。

[!code-cshtml[Main](publishing/samples/sample1.cshtml)]

现在你的站点是准备好进行发布。 首先，你将看到如何将站点部署到 Azure，然后如何将其部署到 web 托管公司。 任一选项适用于您的网站，并且你只需遵循的部署选项之一。

<a id="azure"></a>
## <a name="publishing-your-site-to-microsoft-azure"></a>你的站点发布到 Microsoft Azure

本教程将首先显示您如何将您的网站部署到 Microsoft Azure。 通过使用 Microsoft 帐户登录，你可以在 Azure 上创建最多 10 个免费站点。 这些免费站点提供一种简便方式测试你的站点。 你始终可以删除此站点示例更高版本以避免使用所有可用站点。 可以在几分钟内创建一个免费试用帐户。 有关详细信息，请参阅[Azure 免费试用版](https://azure.microsoft.com/free/?WT.mc_id=A443DD604)。

在 WebMatrix 功能区中，单击**发布**按钮。

![WebMatrix 功能区中的发布按钮](publishing/_static/image1.png)

**发布您网站**对话框随即显示。 如果你不具有到你的 Microsoft 帐户身份登录的则此对话框将包含**开始使用 Azure**链接。 单击此链接。

![发布你的站点](publishing/_static/image2.png)

如果你不具有到 Microsoft 帐户身份登录的则您再次有机会登录。 你必须登录到 Microsoft 帐户来发布站点在 Azure 上。

![登录](publishing/_static/image3.png)

在登录到你的 Microsoft 帐户后, 对话框包含指向在 Azure 上创建新站点或连接到你在 Azure 上的现有站点之一的链接。

![创建新站点](publishing/_static/image4.png)

选择**创建新站点**。

如果名为你的项目**WebPagesMovies**，你的站点的默认名称将为**webpagesmovies.azurewebsites.net**。 此默认名称是最有可能不可用，由红色感叹号。

![默认网站名称](publishing/_static/image5.png)

将站点名称更改为可可用，并选择靠近你所在位置的位置。

![更改的站点名称](publishing/_static/image6.png)

单击“确定”。

WebMatrix performss 测试，确定是否符合你的站点服务器。

![兼容性测试](publishing/_static/image7.png)

选择**继续**。

显示兼容性测试的结果。

![兼容性结果](publishing/_static/image8.png)

选择**继续**。

WebMatrix 显示的文件和将发布到站点的数据库。 由于这是你要将站点发布的第一个时间，所有文件均已列出。 你可以取消选中未准备好发布的文件。 在后续的发布中，将显示仅将已更改的文件。 请参阅[更新实时站点： 重新发布](#update)。

![发布预览](publishing/_static/image9.png)

选择**继续**。

站点部署到 Azure 后，将会显示一条消息，指示部署已完成。

![发布完成](publishing/_static/image10.png)

你的站点和数据库已发布到 Azure，和现在向公众提供。 单击消息，指示发布完成后，并且现在，你将看到你已部署的站点中的链接。 也可以访问 Internet 的任何人都可以添加或修改数据库中的记录。

![](publishing/_static/image11.png)

<a id="host"></a>
## <a name="publishing-your-site-to-a-web-hosting-company"></a>将你的站点发布到 Web 托管公司

如果你决定不将发布到 Azure，你可以改为将站点发布到 web 托管公司。

单击**查找 web 托管**链接。

![在发布设置对话框中的查找 web 宿主按钮](publishing/_static/image12.png)

你转到页列出了宿主提供程序支持 ASP.NET 的 Microsoft 站点上。

![列出托管提供商的 Microsoft 站点上的页](publishing/_static/image13.png)

显然，它可能很难知道现在完全长远来看可能需要哪些宿主功能。 下面是几个需要考虑的事项：

- 对于 WebPagesMovies 站点，你无需具有单独的外接程序对于 SQL Server，通常的额外成本。 在你站点中，你正在使用 SQL Server Compact Edition，这自包含。 但是，你可能需要为你执行一些将来网站工作的 SQL Server 访问。 如果你认为你可能会请确保您可以稍后添加 SQL Server 功能。
- 检查宿主提供程序是否支持 Web 部署发布协议。 可以通过使用 FTP 协议，发布，但更方便地使用 Web 部署。

某些站点还会提供免费试用期。 免费试用版是一种好的方法尝试发布和承载时你正在仍试验 WebMatrix 和 ASP.NET Web 页。

选择你喜欢的其中一个。 对于本教程中，我们选择 DiscountASP.NET，因为我们已创建本教程，该公司还有使用户可以拥有一个对几个月免费站点的站点升级。

> [!NOTE]
> 我们选择的用于本教程的宿主提供程序不应被视为认可该公司通过任何其他。 但我们不得不选取一个图中，而 DiscountASP.NET 是一个支持 ASP.NET 网页和 Web 部署协议用于发布的很多公司。


通常情况下，使用托管提供商注册完后，公司向你发送包含用户名和密码，为 web 服务器上，依次类推 URL 的电子邮件。 如果托管公司支持 Web 部署协议，他们可能发送你的文件，包含发布设置，或允许你在下载一个。 发布设置文件简化了为你的过程。

已注册并已准备好发布，请单击**发布**WebMatrix 功能区中的按钮。 **发布设置**对话框随即显示。

如果托管提供商发送一个发布设置文件，单击**导入发布设置**链接和导入文件。 如果你没有发布设置文件，填写字段使用托管公司向你发送电子邮件中的值。 下面是什么**发布设置**对话框可能如下所示完成后：

![在发布设置对话框中填写发布设置](publishing/_static/image14.png)

单击**验证连接**。 如果所有内容是确定，对话框中报告**已成功连接**，这意味着它可以与托管提供商的服务器通信。

![成功消息如果发布设置正确无误](publishing/_static/image15.png)

如果没有问题，WebMatrix 将会尽全力告诉你的问题是：

![如果没有问题的错误消息发布设置](publishing/_static/image16.png)

单击**保存**以保存设置。 WebMatrix 提供执行测试以确保，它可以正确与宿主站点通信：

![产品以执行发布过程的测试的消息](publishing/_static/image17.png)

单击 **“是”**。 WebMatrix 将某些示例文件上载到托管提供商。 如果兼容性测试已完成，WebMatrix 报告的结果：

![发布测试结果](publishing/_static/image18.png)

如果你已准备就绪，请继续并单击**继续**有关实际启动发布过程。 WebMatrix 计算出什么文件位于你的站点和包已在主机服务器 （现在，无） 上以及为你提供的发布过程预览：

![发布过程将上载文件的预览](publishing/_static/image19.png)

要发布的文件列表包括已创建类似的 web 页*Movies.cshtml*。 列表还包括有关帮助程序，您已安装，为您的数据库，运行 SQL Server Compact Edition 等的文件的文件。 因此，初始发布过程会很大。

单击 **“继续”**。 WebMatrix 将你的文件复制到托管提供商的服务器。 完成后，状态栏中报告结果：

![状态栏消息发布过程成功完成](publishing/_static/image20.png)

若要查看你的实时网站，单击状态栏中的链接。 添加*电影*到 URL，并且您将看到*Movies.cshtml*你创建的文件：

![显示电影页实时站点](publishing/_static/image21.png)

<a id="update"></a>
## <a name="updating-the-live-site-republishing"></a>更新实时站点： 重新发布

一旦你已将站点发布 （到 Azure 或 web 托管公司） 中，有两份&mdash;上您的计算机和服务提供商上的版本的版本。 你可能需要在继续开发站点 (如果没有任何其他，作为下一步教程集的一部分)。 执行操作时，你必须重新发布你的站点，以将更改从您的计算机复制到服务提供程序。 在 WebMatrix 中发布过程可以确定在你的站点上，文件已更改内容并发布仅对这些文件。

若要查看重新发布的工作原理，请打开*Movies.cshtml*站点，进行一些小更改，然后保存该文件。 例如，将标题更改为`Movies - Updated`。

单击**发布**中功能区按钮。 WebMatrix 确定更改的内容，并将发布的文件的位置处显示。

![发布对话框中显示已更改文件可供重新发布](publishing/_static/image22.png)

> [!IMPORTANT] 
> 
> 默认情况下，WebMatrix 发布你的数据库 (*.sdf*文件) 仅你发布站点，第一次。 一旦发布你的站点和人员交互与网站，实时站点上的数据库通常具有站点的真实数据。 您必须非常小心，不要覆盖实时数据库与*.sdf*体现在你的计算机，这通常只包含测试数据的文件。 这就是为什么你看到警告**发布将覆盖任何远程数据库**，为什么的复选框和*WebPagesMovies.sdf*默认未选中状态。


单击 **“继续”**。 WebMatrix 发布更改的文件，并显示一条成功消息，像你发布的第一个时间。

转到实时网站 （如果它仍显示可以单击成功消息中的链接），并验证已发布所做的更改。

> [!TIP] 
> 
> **远程编辑文件**
> 
> 作为更改你的站点，然后重新发布的替代方法，你可以编辑直接在 WebMatrix 中的远程文件。 在此方案中，打开文件上的服务提供商和 WebMatrix 下载一份它进行编辑。 每次保存该文件，WebMatrix 将所做的更改发送到站点。
> 
> 远程编辑是对你的实时网站进行更改的简单办法。 但是，这种方式的更改未同步与您的本地站点中的文件。 若要同步的本地文件与远程站点，你可以下载远程文件。 此过程的工作非常类似于发布，除按相反的顺序。
> 
> 我们不会更描述有关远程编辑和远程下载设施的 WebMatrix 此处。 它们非常有用，如果多个用户需要在不同的计算机上的相同站点上工作。 有关详细信息，请参阅[发布并编辑远程站点使用 WebMatrix 2 Beta](https://go.microsoft.com/fwlink/?LinkId=251591)。


## <a name="additional-resources"></a>其他资源

- [ASP.NET WebMatrix 的 ASP.NET Web Pages 论坛](https://forums.asp.net/1224.aspx/1?WebMatrix+and+ASP+NET+Web+Pages)、 文章的绝佳问题和 get 解答。

>[!div class="step-by-step"]
[上一篇](layouts.md)
