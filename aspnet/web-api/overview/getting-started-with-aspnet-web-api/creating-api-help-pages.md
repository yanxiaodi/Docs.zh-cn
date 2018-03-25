---
uid: web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
title: "创建用于 ASP.NET Web API 的帮助页 |Microsoft 文档"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2013
ms.topic: article
ms.assetid: 0150e67b-c50d-4613-83ea-7b4ef8cacc5a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
msc.type: authoredcontent
ms.openlocfilehash: 37fd26ebaea192cb540c443eff8a07343ab8c15b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="creating-help-pages-for-aspnet-web-api"></a>创建用于 ASP.NET Web API 的帮助页
====================
通过[Mike Wasson](https://github.com/MikeWasson)

当创建 web API 时，通常很有用创建帮助页中，以便其他开发人员知道如何调用你的 API。 你可以手动创建所有文档，但最好自动生成尽可能多地。

为了简化此任务，ASP.NET Web API 提供一个库用于自动生成的帮助页在运行时。

![](creating-api-help-pages/_static/image1.png)

## <a name="creating-api-help-pages"></a>创建 API 帮助页

安装[ASP.NET 和 Web 工具 2012.2 更新](https://go.microsoft.com/fwlink/?LinkId=282650)。 此更新将集成到 Web API 项目模板的帮助页。

接下来，创建新的 ASP.NET MVC 4 项目并选择 Web API 项目模板。 项目模板创建名为示例 API 控制器`ValuesController`。 此模板还创建 API 帮助页。 所有的帮助页的代码文件放置在项目的区域文件夹中。

![](creating-api-help-pages/_static/image2.png)

运行应用程序时，主页页面包含的 API 帮助页的链接。 从主页页面的相对路径是 /Help。

![](creating-api-help-pages/_static/image3.png)

此链接将你带到 API 摘要页。

![](creating-api-help-pages/_static/image4.png)

为此页的 MVC 视图 Areas/HelpPage/Views/Help/Index.cshtml 中定义。 你可以编辑此页后，可以修改布局、 简介、 标题、 样式和等。

页的主要部分是 Api，由控制器分组的表。 表条目动态生成的使用**IApiExplorer**接口。 （我将详细讨论关于此界面更高版本。）如果你添加一个新的 API 控制器，在运行时自动更新表。

"API"列列出的 HTTP 方法和相对 URI。 "说明"列包含每个 API 的文档。 最初，文档并不仅仅是占位符文本。 在下一步的部分中，我将向你演示如何从 XML 注释中添加文档。

每个 API 提供到具有更多详细信息，包括示例请求和响应正文页的链接。

![](creating-api-help-pages/_static/image5.png)

## <a name="adding-help-pages-to-an-existing-project"></a>向现有项目添加帮助页

可以使用 NuGet 包管理器将帮助页面添加到现有的 Web API 项目中。 此选项可从比"Web API"模板不同的项目模板开始。

从**工具**菜单上，选择**库程序包管理器**，然后选择**程序包管理器控制台**。 在[程序包管理器控制台](http://docs.nuget.org/docs/start-here/using-the-package-manager-console)窗口中，键入以下命令之一：

有关**C#**应用程序：`Install-Package Microsoft.AspNet.WebApi.HelpPage`

有关**Visual Basic**应用程序：`Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`

有两个包，一个用于 C# 和 Visual Basic 的一个。 请确保使用与你的项目匹配的一个。

此命令将安装必需的程序集并添加 （位于的区域/HelpPage 文件夹中） 的帮助页的 MVC 视图。 你将需要手动添加的帮助页的链接。 URI 是 /Help。 若要在 razor 视图中创建一个链接，添加以下代码：

[!code-cshtml[Main](creating-api-help-pages/samples/sample1.cshtml)]

此外，请确保注册区域。 在 Global.asax 文件中，添加以下代码到**应用程序\_启动**方法，如果它尚不存在：

[!code-csharp[Main](creating-api-help-pages/samples/sample2.cs?highlight=4)]

## <a name="adding-api-documentation"></a>添加 API 文档

默认情况下，帮助页具有文档的占位符字符串。 你可以使用[XML 文档注释](https://msdn.microsoft.com/library/b2s063f7.aspx)创建文档。 若要启用此功能，打开文件应用程序区域/HelpPage\_Start/HelpPageConfig.cs 并取消注释以下行：

[!code-csharp[Main](creating-api-help-pages/samples/sample3.cs)]

现在启用 XML 文档。 在解决方案资源管理器，右键单击该项目并选择**属性**。 选择**生成**页。

![](creating-api-help-pages/_static/image6.png)

下**输出**，检查**XML 文档文件**。 在编辑框中，键入"应用\_Data/XmlDocument.xml"。

![](creating-api-help-pages/_static/image7.png)

接下来，打开的代码`ValuesController`/Controllers/ValuesControler.cs 中定义的 API 控制器。 将文档注释添加到控制器方法。 例如:

[!code-csharp[Main](creating-api-help-pages/samples/sample4.cs)]

> [!NOTE]
> 提示： 如果你将插入符号放置在上方方法的行，然后键入三个正斜杠，Visual Studio 自动插入的 XML 元素。 然后你可以填写中的空格。


现在生成和运行一次，应用程序并导航到的帮助页。 文档字符串应出现在 API 表中。

![](creating-api-help-pages/_static/image8.png)

帮助页读取 XML 文件在运行时的字符串。 （在部署应用程序时，请确保部署的 XML 文件。）

## <a name="under-the-hood"></a>揭秘

帮助页的顶部生成**ApiExplorer**类，该类是 Web API framework 的一部分。 **ApiExplorer**类提供了原材料创建帮助页。 对于每个 API， **ApiExplorer**包含**ApiDescription**介绍 API。 为此目的，"API"被指 HTTP 方法和相对 URI 的组合。 例如，下面是一些不同的 Api:

- 获取 /api/Products
- 获取 /api/产品 / {id}
- 发布/api/产品

如果控制器操作支持多个 HTTP 方法， **ApiExplorer**每种方法将其视为不同的 API。

若要隐藏的 API **ApiExplorer**，添加**ApiExplorerSettings**属性设为操作和组*IgnoreApi*为 true。

[!code-csharp[Main](creating-api-help-pages/samples/sample5.cs)]

你还可以向控制器，以排除整个控制器中添加此属性。

ApiExplorer 类获取从文档字符串**IDocumentationProvider**接口。 如前面看到的帮助页库提供了**IDocumentationProvider** ，从 XML 文档字符串获取文档。 代码位于 /Areas/HelpPage/XmlDocumentationProvider.cs。 你可以获取文档从其他源通过编写您自己**IDocumentationProvider**。 若要连接它，调用**SetDocumentationProvider**中定义的扩展方法**HelpPageConfigurationExtensions**

**ApiExplorer**自动调入**IDocumentationProvider**接口可以获取每个 API 文档字符串。 它将它们存储在**文档**属性**ApiDescription**和**ApiParameterDescription**对象。

## <a name="next-steps"></a>后续步骤

你不限于使用此处显示的帮助页。 事实上， **ApiExplorer**并不局限于创建帮助页。 钥 Huang 链接具有写入一些出色的博客文章可帮助你想在初始状态：

- [将一个简单的测试客户端添加到 ASP.NET Web API 帮助页](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/02/adding-a-simple-test-client-to-asp-net-web-api-help-page.aspx)
- [使 ASP.NET Web API 帮助页在自承载服务上工作](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/20/making-asp-net-web-api-help-page-work-on-self-hosted-services.aspx)
- [设计时生成的 ASP.NET Web API 的帮助页 （或客户端）](https://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)
- [高级帮助页自定义项](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/10/asp-net-web-api-help-page-part-3-advanced-help-page-customizations.aspx)
