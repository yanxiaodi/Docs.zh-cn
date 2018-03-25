---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-the-production-web-application-to-use-the-production-database-cs
title: 配置用于生产数据库 (C#) 的生产 Web 应用程序 |Microsoft 文档
author: rick-anderson
description: 如前面的教程中所述，是很常见的开发和生产环境之间不同的配置信息。 这是 es...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/23/2009
ms.topic: article
ms.assetid: 0177dabd-d888-449f-91b2-24190cf5e842
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-the-production-web-application-to-use-the-production-database-cs
msc.type: authoredcontent
ms.openlocfilehash: 21eac6a4d829795f02eeeca5f9870b1ab8132d08
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
<a name="configuring-the-production-web-application-to-use-the-production-database-c"></a>配置用于生产数据库 (C#) 的生产 Web 应用程序
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载代码](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_08_CS.zip)或[下载 PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial08_DBConfig_cs.pdf)

> 如前面的教程中所述，是很常见的开发和生产环境之间不同的配置信息。 这是对于数据驱动的 web 应用程序，尤其如此，因为数据库连接字符串的开发和生产环境之间存在差异。 本教程将探讨方法来配置要在更多详细信息中包括适当的连接字符串的生产环境。


## <a name="introduction"></a>介绍

数据驱动的 web 应用程序通常在生产中使用不同的数据库时比开发中。 对于 web 主机提供商托管的和本地开发的应用程序，开发数据库通常驻留在开发人员的计算机时生产数据库托管在 web 托管公司的设施数据库服务器上。 部署数据驱动的 web 应用程序时，需要将开发数据库复制到生产数据库服务器。 在以前的教程，我们看方法完成此步骤。

Web 应用程序使用中的信息*连接字符串*以与数据库建立连接。 连接字符串，通常存储在`Web.config`，指定数据库服务器名称、 数据库、 安全上下文中和其他信息的名称。 因为 web 应用程序使用的数据库依赖于是否在开发或生产环境中运行 web 应用程序，连接字符串必须与不同的两种环境之间。

它是很常见的开发和生产环境之间不同的配置信息。 *常见配置差异之间开发和生产*教程讨论上维护单独的配置信息之间这两种环境，以及简要介绍的技术数据库连接字符串。 本教程将探讨方法来配置要在更多详细信息中包括适当的连接字符串的生产环境。

## <a name="examining-the-connection-string-information"></a>检查连接字符串信息

所用的图书评论 web 应用程序的连接字符串存储在应用程序的配置文件， `Web.config`。 `Web.config` 包含一个特殊的节，用于存储连接字符串，命名恰当[ &lt;connectionStrings&gt;](https://msdn.microsoft.com/library/bf7sd233.aspx)。 `Web.config`文件图书评论网站具有一个名为此节中定义的连接字符串`ReviewsConnectionString`:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample1.xml)]

连接字符串的数据源 =。 \SQLEXPRESS;AttachDbFilename = |DataDirectory|\Reviews.mdf;Integrated 安全 = True;用户实例 = True-并且组成大量的选项和值，由分号和每个选项分隔的选项/值对，并且由一个等号分隔的值。 在此连接字符串中使用的四个选项如下：

- `Data Source` -指定的数据库服务器和数据库服务器实例名称的位置 （如果有）。 值， `.\SQLEXPRESS`，是一个示例： 有数据库服务器和实例名称。 期指定数据库服务器位于同一台计算机作为该应用程序;实例名称是`SQLEXPRESS`。
- `AttachDbFilename` -指定数据库文件的位置。 值包含占位符`|DataDirectory|`，即解析为应用程序 s 的完整路径`App_Data`在运行时的文件夹。
- `Integrated Security` -一个布尔值，该值指示是否在连接到数据库 (false) 或当前的 Windows 帐户凭据 (true) 时使用指定的用户名/密码。
- `User Instance` -特定于 SQL Server Express 版本，该值指示是否允许本地计算机上的非管理用户附加并连接到 SQL Server Express Edition 数据库的配置选项。 请参阅[SQL Server Express 用户实例](https://msdn.microsoft.com/library/ms254504.aspx)有关对此设置的详细信息。
  

允许的连接字符串选项取决于你连接到的数据库和正在使用的 ADO.NET 数据库提供程序。 例如，用于连接到 Microsoft SQL Server 数据库不同于用来连接到 Oracle 数据库连接字符串。 同样，连接到 Microsoft SQL Server 数据库使用 SqlClient 提供程序使用不同的连接字符串比时使用的 OLE DB 访问接口。

你可通过使用如站点的现有生成的数据库连接字符串[ConnectionStrings.com](http://www.connectionstrings.com/)为那些选项可以使用的资源。 但是，较简单的方法是将数据库添加到 Visual Studio 中的服务器资源管理器，然后抓住属性窗口中的连接字符串。 让我们来使用此后一种方法来进行构造生产数据库服务器的连接字符串。

打开 Visual Studio，然后导航到服务器资源管理器窗口 （在 Visual Web Developer 中，此窗口调用数据库资源管理器）。 右键单击数据连接选项，然后从上下文菜单中选择添加连接选项。 这将显示向导中图 1 所示。 选择适当的数据源并单击继续。


[![选择将新的数据库添加到服务器资源管理器](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image2.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image1.jpg) 

**图 1**： 选择将新的数据库添加到服务器资源管理器 ([单击以查看实际尺寸的图像](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image3.jpg))


接下来，指定各种数据库连接信息 （请参见图 2）。 当您使用您的 web 托管公司注册它们应如何连接到数据库的数据库服务器名称、 数据库名称、 用户名和密码用于连接到数据库，依次类推提供了信息。 输入此信息后，单击确定以完成此向导并将数据库添加到服务器资源管理器。


[![指定的数据库连接信息](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image5.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image4.jpg) 

**图 2**： 指定数据库连接信息 ([单击以查看实际尺寸的图像](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image6.jpg))


现在，生产环境数据库应列在服务器资源管理器。 从服务器资源管理器中选择数据库并转到属性窗口。 您将找到名为使用数据库 s 的连接字符串的连接字符串的属性。 假定你在生产和 SqlClient 提供程序上使用 Microsoft SQL Server 数据库连接字符串应类似于以下：

**数据源=*serverName*; 初始目录=*databaseName*; 持久性安全信息=True; 用户 ID=*用户名*; 密码=*密码**\*

其中*serverName*， *databaseName*，*用户名*，和*密码*与数据库服务器名称，数据库的值名称和用户名和密码由你的 web 主机公司提供给你。

## <a name="deploying-the-book-reviews-web-application"></a>部署簿评审 Web 应用程序

前面的教程进行单步遍历将开发数据库复制到生产环境中，但未将不介绍将数据驱动应用程序部署。 此时生产环境中包含的数据库，但与静态评审使用簿评审应用程序的版本。 我们需要新的、 数据驱动应用程序部署到生产服务器以及更新的配置信息。

需要一段时间来部署数据驱动应用程序从开发环境到生产环境。 此过程已在前面的教程中详细介绍。 如果你需要使用刷新程序，请参阅*部署您的网站使用 FTP 客户端*或*部署您的网站使用 Visual Studio*教程。 你将需要确保生产数据库连接字符串是一个用于生产环境，这意味着备用`Web.config`文件必须部署。 具体而言，此修改`Web.config`文件的`<connectionStrings>`元素需要包含生产数据库连接字符串和外观应与以下类似：

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample2.xml)]

请注意，该连接字符串中`<connectionStrings>`元素名为相同 (`ReviewsConnectionString`)，但现在却包含生产数据库连接字符串，而不是开发数据库连接字符串。

除非你有更正式的部署工作流，请手动修改`Web.config`文件部署 （请记住要恢复为使用开发数据库连接字符串之前，使用生产数据库连接字符串之后） 或维护一个单独`Web.config`获取作为部署过程的一部分上载到生产环境的生产环境配置信息的文件。

> [!NOTE]
> 如果你意外地部署`Web.config`包含开发数据库连接字符串，然后对生产应用程序尝试连接到数据库时将会出错的文件。 此错误表现为`SqlException`使用报表服务器找不到或无法访问的消息。


站点部署到生产环境后，请访问你在浏览器将生产站点。 你应查看和在本地运行数据驱动应用程序时享受与相同的用户体验。 这是当然的时你访问的网站上生产站点是由提供支持生产数据库服务器上，而访问在开发环境中的网站开发中使用数据库。 图 3 显示*教授自己 ASP.NET 3.5 24 小时内*查看从生产环境 （请注意浏览器的地址栏中的 URL） 中的网站的页。


[![数据驱动应用程序是现在可在生产 ！](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image8.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image7.jpg) 

**图 3**: The Data-Driven 应用程序是现在可在生产 ！ ([单击以查看实际尺寸的图像](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image9.jpg))


### <a name="storing-connection-strings-in-a-separate-configuration-file"></a>将连接字符串存储在单独的配置文件

维护上开发和生产环境的单独的配置信息的常用的方法是让两个版本的`Web.config`： 一个用于开发环境，一个用于生产。 在部署时相应`Web.config`版本可以复制到生产环境。 理想情况下，此过程将自动部署工作流的一部分。

而不是维护两个单独`Web.config`文件 （可选） 可以提供更精细的差异。 构成元素`Web.config`文件可以在然后中引用外部配置文件中定义`Web.config`文件。 简而言之，你可以有一个`Web.config`文件引用一个 databaseConnectionStrings.config 文件，其中将包含连接字符串这两个环境由应用程序，并且将是唯一的每个环境。 找到分隔到单独的文件不同的配置信息提供整洁简单`Web.config`文件以及更多清楚地概述了开发和生产环境之间的配置区别。

若要使用此方法，请首先在名为 web 应用程序中创建一个新文件夹`ConfigSections`。 接下来，将两个文件添加到名为 databaseConnectionStrings.dev.config 和 databaseConnectionStrings.production.config 此新文件夹。接下来，将复制`<connectionStrings>`元素从`Web.config`到 databaseConnectionStrings.dev.config 和 databaseConnectionStrings.production.config 的文件，然后修改中的连接字符串databaseConnectionStrings.production.config 文件，以便它指定生产数据库连接字符串。 例如，databaseConnectionStrings.dev.config 文件应包含仅`<connectionStrings>`元素引用开发数据库的连接字符串：

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample3.xml)]

同样，databaseConnectionStrings.production.config 文件应只包含`<connectionStrings>`元素，但其中一个具有的生产数据库连接字符串。

请 databaseConnectionStrings.dev.config 文件的副本并将其命名 databaseConnectionStrings.config。

> [!NOTE]
> 你可以将配置文件名称以外的 databaseConnectionStrings.config，如果你 d 喜欢，如`connectionStrings.config`或`dbInfo.config`。 但是，请务必将与该文件`.config`扩展作为`.config`文件，默认情况下，不提供由 ASP.NET 引擎。 如果您其他命名该文件，例如`connectionStrings.txt`，用户无法其浏览器指向[www.yoursite.com/ConfigSettings/connectionStrings.txt](http://www.yoursite.com/ConfigSettings/connectionStrings.txt)和查看文件的内容 ！


此时`ConfigSections`文件夹应包含三个文件 （请参见图 4）。 DatabaseConnectionStrings.dev.config 和 databaseConnectionStrings.production.config 文件分别包含开发和生产环境中，连接字符串。 DatabaseConnectionStrings.config 文件包含 web 应用程序在运行时将使用的连接字符串信息。 因此，databaseConnectionStrings.config 文件应该是相同的 databaseConnectionStrings.dev.config 文件在开发环境中，而在生产 databaseConnectionStrings.config 文件应该相同databaseConnectionStrings.production.config。


[![ConfigSections](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image11.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image10.jpg) 

**图 4**: ConfigSections ([单击以查看实际尺寸的图像](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image12.jpg))


现在，我们需要指示`Web.config`将 databaseConnectionStrings.config 文件用于其连接字符串存储区。 打开 `Web.config` 并将现有 `<connectionStrings>` 元素替换为以下内容：

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample4.xml)]

`configSource`属性指定相对于的物理路径`Web.config`文件。 如果外部`.config`文件位于与相同的目录`Web.config`然后将此属性设置为的文件名称`.config`文件。 如果它在一个子目录，s 原样 databaseConnectionStrings.config，这种情况指定使用反斜杠来分隔类似 ConfigSections\databaseConnectionStrings.config 文件夹和文件名称的子文件夹。

经过此修改后，开发和生产环境包含相同`Web.config`文件。 现在，唯一的区别是 databaseConnectionStrings.config 文件。 将 databaseConnectionStrings.production.config 文件复制到生产环境，并将它重命名为 databaseConnectionStrings.config。如果在将来，有向生产数据库的连接字符串的更改将需要使它们到 databaseConnectionStrings.production.config 文件，然后将该文件上载到生产环境，重命名 databaseConnectionStrings.config。

> [!NOTE]
> 你可以指定任何信息`Web.config`中单独的文件并使用元素`configSource`属性中引用该文件`Web.config`。


## <a name="summary"></a>摘要

数据驱动应用程序通常在开发和生产环境中使用不同的数据库。 因此，存储在 web 应用程序的配置的数据库连接字符串必须是唯一每个环境。 在本教程中我们介绍了如何确定了生产数据库连接字符串以及要维护的两种环境中的唯一连接字符串信息的方式。

尽情享受编程 ！

#### <a name="further-reading"></a>其他阅读材料

在本教程中讨论的主题的详细信息，请参阅以下资源：

- [连接字符串和配置文件](https://msdn.microsoft.com/library/ms254494.aspx)
- [数据库配置字符串信息 @ ConnectionStrings.com](http://www.connectionstrings.com/)
- [移出 Web.config 文件的设置](http://www.asp101.com/tips/index.asp?id=154)
- [技术文档&lt;connectionStrings&gt;元素](https://msdn.microsoft.com/library/bf7sd233.aspx)

>[!div class="step-by-step"]
[上一页](deploying-a-database-cs.md)
[下一页](configuring-a-website-that-uses-application-services-cs.md)
