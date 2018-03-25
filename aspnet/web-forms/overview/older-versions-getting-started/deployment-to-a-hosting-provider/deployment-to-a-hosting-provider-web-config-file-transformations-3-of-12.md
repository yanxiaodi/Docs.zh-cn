---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12
title: "部署具有 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 的 ASP.NET Web 应用程序： Web.Config 文件转换的 3 12 |Microsoft 文档"
author: tdykstra
description: "这一系列的教程演示如何部署 （发布） ASP.NET web 应用程序项目，它通过使用 Visual Stu 包含 SQL Server Compact 数据库..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 2b0df3d9-450b-4ea6-b315-4c9650722cad
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12
msc.type: authoredcontent
ms.openlocfilehash: ed78b55d2b0315cf428f137c56ad85b29a95e1c5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-webconfig-file-transformations---3-of-12"></a>部署具有 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 的 ASP.NET Web 应用程序： Web.Config 文件转换的 3 12
====================
通过[Tom Dykstra](https://github.com/tdykstra)

[下载初学者项目](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> 这一系列的教程演示如何部署 （发布） ASP.NET web 应用程序项目，它通过使用 Visual Studio 2012 RC 或 Visual Studio Express 2012 RC for Web 中包含 SQL Server Compact 数据库。 如果你安装 Web 发布更新，也可以使用 Visual Studio 2010。 有关序列的简介，请参阅[序列中的第一个教程](deployment-to-a-hosting-provider-introduction-1-of-12.md)。
> 
> 显示部署功能后，将 Visual Studio 2012 RC 版本中推出，演示如何部署 SQL Server Compact 以外的 SQL Server 版本并演示如何将部署到 Azure App Service Web Apps 的教程，请参阅[的 ASP.NET Web 部署使用 Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md)。


## <a name="overview"></a>概述

本教程演示如何自动执行更改的过程*Web.config*文件时将其部署到不同的目标环境。 大多数应用程序具有设置*Web.config*时部署应用程序必须是不同的文件。 自动化进行这些更改的过程，可以防止你无需手动执行它们，每次部署，这会非常单调乏味，而且容易出错。

提示： 如果你收到如下错误消息，或当你完成本教程的内容不起作用，请务必检查[故障排除页](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)。

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>Web.config 转换与 Web 部署参数

有两种方法来自动执行更改的过程*Web.config*文件设置： [Web.config 转换](https://msdn.microsoft.com/library/dd465326.aspx)和[Web 部署参数](https://msdn.microsoft.com/library/ff398068.aspx)。 A *Web.config*转换文件包含指定如何更改的 XML 标记*Web.config*文件部署时。 你可以指定不同的更改特定生成配置和特定发布配置文件。 默认值生成配置调试和发布，而且你可以创建自定义生成配置。 发布配置文件通常对应于目标环境中。 (有关详细信息发布中的配置文件，您将学习[作为测试环境部署到 IIS](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)教程。)

可以使用 web 部署参数来指定许多不同类型的设置必须在部署，包括在中找到的设置过程中配置*Web.config*文件。 用于指定当*Web.config*文件更改时，Web 部署参数是更复杂的设置，但不是知道要部署之前设置的值时，它们会很有用。 例如，在企业环境中，你可能会造成*部署包*并将其提供给的人员在 IT 部门要将安装在生产环境，并该用户有能够输入连接字符串或不希望这样做的密码知道。

对于本教程介绍如何方案，你知道的所有内容都进行到*Web.config*文件，因此不需要使用 Web 部署参数。 你将配置某些转换，具体取决于生成配置使用，存在不同，并且一些与不同，具体取决于使用的发布配置文件。

## <a name="creating-transformation-files-for-publish-profiles"></a>为发布配置文件创建转换文件

在**解决方案资源管理器**，展开*Web.config*若要查看*Web.Debug.config*和*Web.Release.config*转换文件默认情况下，对两个默认的生成配置创建。

![Web.config_transform_files](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image1.png)

你可以通过右键单击 Web.config 文件并选择创建的自定义生成配置的转换文件**添加配置转换**从上下文菜单中，但对于本教程不需要执行此操作。

你需要以下两个转换文件，用于配置部署目标，而不是生成配置相关的更改。 这种类型的一个典型示例是设置的不同的测试中和生产的 WCF 终结点。 在更高版本的教程，您将创建发布配置文件，名为测试和生产环境，因此你需要*Web.Test.config*文件和一个*Web.Production.config*文件。

必须手动创建发布配置文件相关联的转换文件。 在**解决方案资源管理器**，右键单击 ContosoUniversity 项目并选择**在 Windows 资源管理器中打开文件夹**。

![Open_folder_in_Windows_Explorer](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image2.png)

在**Windows 资源管理器**，选择*Web.Release.config*文件，复制文件，然后将粘贴它的两个副本。 重命名这些副本*Web.Production.config*和*Web.Test.config*，然后关闭**Windows 资源管理器**。

在**解决方案资源管理器**，单击**刷新**以查看新的文件。

选择新文件，右键单击，然后单击**包括在项目**上下文菜单中。

![在项目中包括测试和生产配置文件](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image3.png)

若要阻止这些文件部署，可以选择在**解决方案资源管理器**，然后在**属性**窗口更改**生成操作**属性从**内容**到**无**。 （基于的生成配置的转换文件将自动阻止部署。）

你现在就可以输入*Web.config*转换*Web.config*转换文件。

## <a name="limiting-error-log-access-to-administrators"></a>限制对管理员的错误日志访问

如果没有错误，应用程序运行时，应用程序将显示常规错误页面代替系统生成的错误页，并且它使用[Elmah NuGet 包](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx)错误日志记录和报表。 `customErrors`中的元素*Web.config*文件指定的错误页：

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample1.xml)]

若要查看错误页，暂时更改`mode`属性`customErrors`元素从"RemoteOnly"到"On"和运行 Visual Studio 中的应用程序。 导致错误通过请求 URL 无效，如*Studentsxxx.aspx*。 而不是 IIS 生成"找不到页面"错误页，你看到*GenericErrorPage.aspx*页。

[![Error_page](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image5.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image4.png)

若要查看错误日志，替换 URL 中的所有内容后使用的端口号*elmah.axd* (例如在屏幕截图中， `http://localhost:51130/elmah.axd`)，然后按 Enter:

[![Elmah_log_page](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image7.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image6.png)

不要忘记设置`customErrors`回"RemoteOnly"模式完成后的元素。

你的开发计算机上很方便地允许免费访问错误日志页中，但在生产环境中将会带来安全风险。 对于生产站点，你可以将添加通过配置中的某个转换只对管理员限制错误日志访问的授权规则*Web.Production.config*文件。

打开*Web.Production.config*和添加新`location`紧跟在打开之后元素`configuration`标记，如下所示。 (请确保仅添加`location`元素，并不显示在此处只是为了提供一些上下文周围标记。)

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample2.xml?highlight=2-9)]

`Transform`属性值为"插入"，则这`location`元素要作为同级添加到任何现有`location`中的元素*Web.config*文件。 (已存在一个`location`元素，它指定授权规则的**更新信用额度**页。)当你在部署后测试生产站点时，你将测试以验证此授权规则有效。

无需限制错误日志访问在测试环境中，因此无需添加此代码与*Web.Test.config*文件。

> [!NOTE] 
> 
> **安全说明**永远不会显示错误详细信息为公共在生产应用程序，或将该信息存储在公共位置。 攻击者可以使用错误信息发现站点中的漏洞。 如果你在自己的应用程序中使用 ELMAH，一定要调查在其中可以配置 ELMAH 为安全风险降至最低的方式。 本教程中的 ELMAH 示例不应视为是建议的配置。 它是一个示例： 所选为了演示如何处理应用程序必须能够在其中创建文件的文件夹。


## <a name="setting-an-environment-indicator"></a>设置环境指示器

一种常见方案是让*Web.config*文件必须将部署到每个环境中不同的设置。 例如，调用 WCF 服务的应用程序可能需要在测试和生产环境的不同终结点。 Contoso 大学应用程序还包括这种类型的设置。 此设置控制可见的指示符站点的页上，告诉你要在中，如开发、 测试或生产的环境。 设置值确定是否应用程序都将追加"（开发）"或"（测试）"中的主要标题*Site.Master*母版页：

[![Environment_indicator](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image9.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image8.png)

在生产中运行应用程序时，将省略环境指示器。

Contoso 大学 web 页读取在中设置一个值`appSettings`中*Web.config*以确定哪些环境中运行该应用程序的文件：

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample3.xml)]

值应为"Test"的测试环境中和"Prod"中的生产环境中。

打开*Web.Production.config*并添加`appSettings`元素的开始标记前紧跟`location`前面添加的元素：

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample4.xml)]

`xdt:Transform`属性的值"SetAttributes"指示此转换旨在更改中的现有元素的属性值*Web.config*文件。 `xdt:Locator`属性的值"Match(key)"指示要修改的元素是一个其`key`属性匹配`key`此处指定的属性。 仅另一个属性的`add`元素是`value`，这也将更改的内容中部署*Web.config*文件。 此代码会导致`value`属性`Environment``appSettings`元素设置为"Prod" *Web.config*部署到生产环境的文件。

接下来，将应用到相同的更改*Web.Test.config*文件中的，除集`value`向"测试"而不是"Prod"。 完成后，`appSettings`主题中*Web.Test.config*将类似于以下示例：

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample5.xml)]

## <a name="disabling-debug-mode"></a>禁用调试模式下

发行版本中，你不希望启用调试而不考虑的环境部署到。 默认情况下*Web.Release.config*自动创建转换文件中删除的代码使用`debug`属性从`compilation`元素：

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample6.xml)]

`Transform`属性原因`debug`属性省略从已部署*Web.config*文件每当部署发布版本时。

此相同的转换是在测试和生产中转换文件因为复制版本转换文件创建它们。 你不需要它那里重复，因此打开的每个文件，删除**编译**元素，并保存并关闭每个文件。

## <a name="setting-connection-strings"></a>设置连接字符串

在大多数情况下你不必设置连接字符串转换，因为你可以发布配置文件中指定连接字符串。 但有异常时要部署 SQL Server Compact 数据库，并且你正在使用 Entity Framework Code First 迁移更新目标服务器上的数据库。 对于此方案，你必须指定将用于在服务器更新数据库架构的其他连接字符串。 若要设置此转换，将添加 **&lt;connectionStrings&gt;** 紧跟在打开之后元素**&lt;配置&gt;**中同时标记*Web.Test.config*和*Web.Production.config*转换文件：

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample7.xml)]

`Transform`属性指定此连接字符串将添加到*connectionStrings*中部署元素*Web.config*文件。 (发布过程创建此额外的连接字符串自动为你如果不存在，但默认情况下**providerName**属性获取设置为`System.Data.SqlClient`，其中不不适用于 SQL Server Compact 起作用。 通过手动添加的连接字符串，你将在部署过程从创建具有错误的提供程序名称的连接字符串元素。）

你现在具有指定的所有*Web.config* Contoso 大学应用程序来测试和生产部署需要的转换。 在以下教程中，你将负责设置项目属性所需的部署设置任务。

## <a name="more-information"></a>详细信息

有关涵盖的本教程主题的详细信息，请参阅中的 Web.config 转换方案[ASP.NET 部署内容映射](https://msdn.microsoft.com/library/bb386521.aspx)。

>[!div class="step-by-step"]
[上一页](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)
[下一页](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
