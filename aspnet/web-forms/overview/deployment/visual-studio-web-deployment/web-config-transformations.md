---
uid: web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
title: "使用 Visual Studio 的 ASP.NET Web 部署： Web.config 文件转换 |Microsoft 文档"
author: tdykstra
description: "本系列教程演示如何部署 （发布） ASP.NET web 应用程序到 Azure App Service Web Apps 或第三方托管提供程序，使用的..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 5a2a927b-14cb-40bc-867a-f0680f9febd7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
msc.type: authoredcontent
ms.openlocfilehash: a526275d76618c325a6b00f33cc550f28ab0cc00
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-web-deployment-using-visual-studio-webconfig-file-transformations"></a>使用 Visual Studio 的 ASP.NET Web 部署： Web.config 文件转换
====================
通过[Tom Dykstra](https://github.com/tdykstra)

[下载初学者项目](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> 本系列教程演示如何部署 （发布） ASP.NET web 应用程序到 Azure App Service Web Apps 或第三方托管提供程序，通过使用 Visual Studio 2012 或 Visual Studio 2010。 有关序列的信息，请参阅[序列中的第一个教程](introduction.md)。


## <a name="overview"></a>概述

本教程演示如何自动执行更改的过程*Web.config*文件时将其部署到不同的目标环境。 大多数应用程序具有设置*Web.config*时部署应用程序必须是不同的文件。 自动化进行这些更改的过程，可以防止你无需手动执行它们，每次部署，这会非常单调乏味，而且容易出错。

提示： 如果你收到如下错误消息，或当你完成本教程的内容不起作用，请务必检查[故障排除页](troubleshooting.md)。

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>与 Web 部署参数的 Web.config 转换

有两种方法来自动执行更改的过程*Web.config*文件设置： [Web.config 转换](https://msdn.microsoft.com/library/dd465326.aspx)和[Web 部署参数](https://msdn.microsoft.com/library/ff398068.aspx)。 A *Web.config*转换文件包含指定如何更改的 XML 标记*Web.config*文件部署时。 你可以指定不同的更改特定生成配置和特定发布配置文件。 默认值生成配置调试和发布，而且你可以创建自定义生成配置。 发布配置文件通常对应于目标环境中。 (有关详细信息发布中的配置文件，您将学习[作为测试环境部署到 IIS](deploying-to-iis.md)教程。)

可以使用 web 部署参数来指定许多不同类型的设置必须在部署，包括在中找到的设置过程中配置*Web.config*文件。 用于指定当*Web.config*文件更改时，Web 部署参数是更复杂的设置，但不是知道要部署之前设置的值时，它们会很有用。 例如，在企业环境中，你可能会造成*部署包*并将其提供给的人员在 IT 部门要将安装在生产环境，并该用户有能够输入连接字符串或不希望这样做的密码知道。

对于本系列教程涵盖方案，您事先知道所必须采取的措施的一切*Web.config*文件，因此不需要使用 Web 部署参数。 你将配置某些转换，具体取决于生成配置使用，存在不同，并且一些与不同，具体取决于使用的发布配置文件。

<a id="watransforms"></a>

## <a name="specifying-webconfig-settings-in-azure"></a>在 Azure 中的指定 Web.config 设置

如果*Web.config*你想要更改的文件设置位于`<connectionStrings>`或`<appSettings>`元素，以及将部署到 Azure App Service 中 Web Apps，您是否自动执行过程中的更改的另一种方法部署。 你可以输入你想要在 Azure 中生效设置**配置**你的 web 应用的管理门户页面的选项卡 (向下滚动到**应用设置**和**连接字符串**部分)。 部署项目时，Azure 将自动应用所做的更改。 有关详细信息，请参阅[Windows Azure 网站： 应用程序字符串和连接字符串的工作原理](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx)。

## <a name="default-transformation-files"></a>默认转换文件

在**解决方案资源管理器**，展开*Web.config*若要查看*Web.Debug.config*和*Web.Release.config*转换文件默认情况下，对两个默认的生成配置创建。

![Web.config_transform_files](web-config-transformations/_static/image1.png)

你可以通过右键单击 Web.config 文件并选择创建的自定义生成配置的转换文件**添加配置转换**从上下文菜单。 本教程中，不需要为此，请并且禁用菜单选项，因为你尚未创建任何自定义生成配置。

稍后你将创建三个详细转换文件，每个测试，用于暂存，和生产发布配置文件。 处理在发布配置文件转换文件因为它依赖于目标环境的一个典型示例是设置的不同的测试中和生产的 WCF 终结点。 你将创建发布配置文件转换文件中更高版本的教程后创建的发布配置文件，可以随。

## <a name="disable-debug-mode"></a>禁用调试模式

取决于生成配置，而不是目标环境的设置的示例`debug`属性。 为发布版本，则通常想调试禁用无论的环境部署到。 因此，默认情况下，Visual Studio 项目模板创建*Web.Release.config*将带中删除的代码文件转换`debug`属性从`compilation`元素。 下面是默认值*Web.Release.config*： 除了一些注释掉的示例转换代码，它包括中的代码`compilation`中移除的元素`debug`属性：

[!code-xml[Main](web-config-transformations/samples/sample1.xml?highlight=18)]

`xdt:Transform="RemoveAttributes(debug)"`属性指定你想`debug`属性从移除`system.web/compilation`中部署元素*Web.config*文件。 这将进行每次部署发布版本。

## <a name="limit-error-log-access-to-administrators"></a>限制对管理员的错误日志访问

如果没有错误，应用程序运行时，应用程序将显示常规错误页面代替系统生成的错误页，并且它使用[Elmah NuGet 包](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx)错误日志记录和报表。 `customErrors`应用程序中的元素*Web.config*文件指定的错误页：

[!code-xml[Main](web-config-transformations/samples/sample2.xml)]

若要查看错误页，暂时更改`mode`属性`customErrors`元素从"RemoteOnly"到"On"和运行 Visual Studio 中的应用程序。 导致错误通过请求 URL 无效，如*Studentsxxx.aspx*。 而不是 IIS 生成"无法找到资源"错误页上，你看到*GenericErrorPage.aspx*页。

![错误页](web-config-transformations/_static/image2.png)

若要查看错误日志，替换 URL 中的所有内容后使用的端口号*elmah.axd* (例如， `http://localhost:51130/elmah.axd`)，然后按 Enter:

![ELMAH 页](web-config-transformations/_static/image3.png)

不要忘记设置`customErrors`回"RemoteOnly"模式完成后的元素。

你的开发计算机上很方便地允许免费访问错误日志页中，但在生产环境中将会带来安全风险。 对于生产站点中，你想要添加授权规则，将错误日志访问限制为管理员，并确保限制工作你希望在测试和过渡还中。 因此，这是你想要实现，每次部署发布版本，因此在其所属的另一个更改*Web.Release.config*文件。

打开*Web.Release.config*和添加新`location`立即在关闭前的元素`configuration`标记，如下所示。

[!code-xml[Main](web-config-transformations/samples/sample3.xml?highlight=27-34)]

`Transform`属性值为"插入"，则这`location`元素要作为同级添加到任何现有`location`中的元素*Web.config*文件。 (已存在一个`location`元素，它指定授权规则的**更新信用额度**页。)

现在你可以预览转换后，若要确保你正确编码。

在**解决方案资源管理器**，右键单击*Web.Release.config*单击**预览转换**。

![预览转换菜单](web-config-transformations/_static/image4.png)

演示如何开发页面将打开*Web.config*上的左窗格和内容文件已部署*Web.config*文件将如下所示在右侧，突出显示的更改。

![调试转换的预览](web-config-transformations/_static/image5.png)

![位置转换的预览](web-config-transformations/_static/image6.png)

(在预览版中，你可能会发现自己编写一些附加更改转换为： 这些通常涉及不会影响功能的空白区域中删除。)

当你在部署后测试站点时，还将测试来验证授权规则有效。

> [!NOTE] 
> 
> **安全说明**永远不会显示错误详细信息为公共在生产应用程序，或将该信息存储在公共位置。 攻击者可以使用错误信息发现站点中的漏洞。 如果你在自己的应用程序中使用 ELMAH，配置 ELMAH 来尽量降低安全风险。 本教程中的 ELMAH 示例不应视为是建议的配置。 它是一个示例： 所选为了演示如何处理应用程序必须能够在其中创建文件的文件夹。 有关详细信息，请参阅[保护 ELMAH 终结点](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages)。


## <a name="a-setting-that-youll-handle-in-publish-profile-transformation-files"></a>将在中处理的设置发布配置文件转换文件

一种常见方案是让*Web.config*文件必须将部署到每个环境中不同的设置。 例如，调用 WCF 服务的应用程序可能需要在测试和生产环境的不同终结点。 Contoso 大学应用程序还包括这种类型的设置。 此设置控制可见的指示符站点的页上，告诉你要在中，如开发、 测试或生产的环境。 设置值确定是否应用程序都将追加"（开发）"或"（测试）"中的主要标题*Site.Master*母版页：

![环境指示器](web-config-transformations/_static/image7.png)

当应用程序运行在过渡环境还是生产省略环境指示器。

Contoso 大学 web 页读取在中设置一个值`appSettings`中*Web.config*以确定哪些环境中运行该应用程序的文件：

[!code-xml[Main](web-config-transformations/samples/sample4.xml)]

值应在测试环境中，将"Test"和"生产"过渡和生产。

转换文件中的以下代码将实现此转换：

[!code-xml[Main](web-config-transformations/samples/sample5.xml)]

`xdt:Transform`属性的值"SetAttributes"指示此转换旨在更改中的现有元素的属性值*Web.config*文件。 `xdt:Locator`属性的值"Match(key)"指示要修改的元素是一个其`key`属性匹配`key`此处指定的属性。 仅另一个属性的`add`元素是`value`，这也将更改的内容中部署*Web.config*文件。 此处显示的原因代码`value`属性`Environment``appSettings`元素设置为"Test" *Web.config*部署的文件。

此转换属于你尚未创建发布配置文件转换文件。 你将创建并更新转换文件，当你创建的测试、 过渡和生产环境的发布配置文件，则实现此更改。 你将在执行该操作[将部署到 IIS](deploying-to-iis.md)和[部署到生产环境](deploying-to-production.md)教程。

> [!NOTE]
> 因为此设置处于`<appSettings>`元素，必须在要部署到 Azure 应用程序服务，请参阅中的 Web Apps 时指定转换所需的另一种可选[在 Azure 中的指定 Web.config 设置](#watransforms)前面的本主题中。


## <a name="setting-connection-strings"></a>设置连接字符串

虽然默认转换文件包含的示例，演示如何更新连接字符串，但在大多数情况下你不必设置连接字符串转换，因为你可以发布配置文件中指定连接字符串。 你将在执行该操作[将部署到 IIS](deploying-to-iis.md)和[部署到生产环境](deploying-to-production.md)教程。

## <a name="summary"></a>摘要

你现在要做尽可能多可以按照与*Web.config*转换之前创建的发布配置文件中，并已了解的内容将在已部署的 Web.config 文件中预览。

![位置转换的预览](web-config-transformations/_static/image8.png)

在以下教程中，你将负责设置项目属性所需的部署设置任务。

## <a name="more-information"></a>详细信息

有关本教程所涵盖主题的详细信息，请参阅[使用 Web.config 转换在部署过程中更改目标 Web.config 文件或 app.config 文件中设置](https://go.microsoft.com/fwlink/p/?LinkId=282413#transforms)的 Web 部署内容映射中Visual Studio 和 ASP.NET。

>[!div class="step-by-step"]
[上一页](preparing-databases.md)
[下一页](project-properties.md)
