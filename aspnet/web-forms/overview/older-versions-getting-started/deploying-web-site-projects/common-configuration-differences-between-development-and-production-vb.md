---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-vb
title: "开发和生产环境 (VB) 之间的常见配置差异 |Microsoft 文档"
author: rick-anderson
description: "在前面的教程，我们通过将所有相关文件从开发环境复制到生产环境中部署了我们的网站。 但是，我..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: 548e75f6-4d6c-4cb4-8da8-417915eb8393
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-vb
msc.type: authoredcontent
ms.openlocfilehash: b53565e4ae09b007029bdab7cc8724e2d7d193fe
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
<a name="common-configuration-differences-between-development-and-production-vb"></a>开发和生产环境 (VB) 之间的常见配置差异
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载 PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial05_ConfigDifferences_vb.pdf)

> 在前面的教程，我们通过将所有相关文件从开发环境复制到生产环境中部署了我们的网站。 但是，不少见，才会配置环境，这使得需要每个环境已具有唯一的 Web.config 文件之间的差异。 本教程检查典型配置差异，并考察维护单独的配置信息的策略。


## <a name="introduction"></a>介绍


最后两个教程在遍历部署简单的 web 应用程序。 [*部署你的网站使用 FTP 客户端*](deploying-your-site-using-an-ftp-client-vb.md)教程介绍了如何使用独立的 FTP 客户端将所需的文件复制到生产开发环境中。 在前面的教程， [*部署您站点使用 Visual Studio*](deploying-your-site-using-visual-studio-vb.md)，查找在部署使用 Visual Studio 的复制网站工具和发布选项。 在这两篇教程在生产环境中的每个文件是在开发环境文件的副本。 但是，不常见的不同于开发环境中的生产环境中的配置文件。 Web 应用程序的配置存储在`Web.config`文件，并通常包括有关外部资源，如数据库、 web 和电子邮件服务器的信息。 它还规定了某些情况下，例如的未经处理的异常发生时要采取的操作过程中的应用程序的行为。

部署 web 应用程序时很重要，正确的配置信息结束，在生产环境中。 在大多数情况下`Web.config`在开发环境中的文件无法被复制到生产环境中作为-是。 相反的自定义的版本`Web.config`需要上载到生产环境。 本教程简要评审的一些较常见的配置差异;它还概述了一些技术进行维护环境之间的不同的配置信息。

## <a name="typical-configuration-differences-between-the-development-and-production-environments"></a>典型配置开发和生产环境之间的差异

`Web.config`文件包括多种类型的 ASP.NET 应用程序的配置信息。 此配置信息的一些无论是相同的环境。 例如，身份验证设置与 URL 授权规则拼中`Web.config`文件的`<authentication>`和`<authorization>`元素通常是相同的而不考虑环境。 但其他配置信息-如有关外部资源的信息通常取决于环境。

数据库连接字符串是基于环境的不同的配置信息的一个极好示例。 当 web 应用程序与数据库服务器通信它必须首先建立连接，并通过做到这一点[连接字符串](http://www.connectionstrings.com/Articles/Show/what-is-a-connection-string)。 尽管可以直接在 web 页或连接到数据库的代码中的数据库连接字符串进行硬编码，最好是将其放`Web.config`的[`<connectionStrings>`元素](https://msdn.microsoft.com/library/bf7sd233.aspx)以便连接字符串信息是在单一的集中式的位置。 通常不同的数据库使用在开发过程中不是在生产; 中使用因此，连接字符串信息必须是唯一的每个环境。

> [!NOTE]
> 将来的教程浏览部署数据驱动的应用程序，此时我们将深入了解如何在配置文件中存储数据库连接字符串的详细信息。


开发和生产环境的预期的行为不同大体上。 在开发环境中的 web 应用程序正在创建，测试和调试通过小的一组开发人员。 在生产环境中多个不同的同时进行用户正在访问同一应用程序。 ASP.NET 包括多种功能，可帮助开发人员在测试和调试应用程序，但应出于性能和安全原因，在生产环境中的禁用了这些功能。 让我们看一下几个此类配置设置。

### <a name="configuration-settings-that-impact-performance"></a>会对性能影响的配置设置

当 ASP.NET 页访问对于第一次 （或后已更改第一次），必须将其声明性标记转换为一个类，并且必须编译此类。 如果 web 应用程序使用自动编译页面的代码隐藏类将需要进行编译，太。 你可以配置多种类型的通过编译选项`Web.config`文件的[`<compilation>`元素](https://msdn.microsoft.com/library/s10awwz0.aspx)。

调试属性是中最重要的特性之一`<compilation>`元素。 如果`debug`属性设置为"true"则在编译的程序集将调试符号，调试 Visual Studio 中的应用程序时需要用到它。 但调试符号增加程序集的大小和运行代码时有更多的内存要求。 此外，当`debug`属性设置为"true"返回任何内容`WebResource.axd`不缓存，这意味着，每次用户访问页它们将需要重新下载返回的静态内容`WebResource.axd`。

> [!NOTE]
> `WebResource.axd`是一个内置的 HTTP 处理程序引入的 ASP.NET 2.0 中，服务器控件用于检索嵌入的资源，例如脚本文件、 图像、 CSS 文件和其他内容。 有关详细信息如何`WebResource.axd`工作原理以及如何使用该嵌入的资源访问来自自定义服务器控件，请参阅[访问嵌入资源通过 URL 使用`WebResource.axd` ](http://aspnet.4guysfromrolla.com/articles/080906-1.aspx)。


`<compilation>`元素的`debug`属性通常设置为"true"，在开发环境中。 事实上，此属性必须设置为"true"若要调试 web 应用程序;如果你尝试调试 ASP.NET 应用程序从 Visual Studio 和`debug`属性设置为"false"，Visual Studio 将显示一个消息，其中说明应用程序不能进行调试直到`debug`属性设置为"true"，将为你进行此更改的优惠。

你应**永远不会**具有`debug`属性设置为"true"，在生产环境中由于其对性能的影响。 有关本主题的更全面讨论，请参阅[Scott Guthrie](https://weblogs.asp.net/scottgu/)的博客文章[不运行生产 ASP.NET 应用程序与`debug="true"`已启用](https://weblogs.asp.net/scottgu/442448)。

### <a name="custom-errors-and-tracing"></a>自定义错误和跟踪

ASP.NET 应用程序中发生未经处理的异常时它将冒泡到运行的位置发生三个操作之一：

- 将显示一般的运行时错误消息。 此页向用户通知存在的运行时错误，但不提供任何有关错误的详细信息。
- 将显示一条异常详细信息消息，其中包括只需引发的异常的信息。
- 显示自定义错误页，这是一个 ASP.NET 页，你创建显示所需的任何消息。

在遇到未经处理的异常时也会发生什么情况取决于`Web.config`文件的[`<customErrors>`部分](https://msdn.microsoft.com/library/h0hfz6fc.aspx)。

在开发和测试应用程序时，它有助于查看浏览器中的任何异常的详细信息。 但是，在生产中的应用程序中显示异常详细信息是潜在的安全风险。 此外，它会 unflattering，会使你查找不专业的网站。 理想情况下，发生未经处理的异常时在开发环境中的 web 应用程序将显示异常的详细信息时相同的应用程序在生产环境中将显示自定义错误页。

> [!NOTE]
> 默认值`<customErrors>`部分设置显示异常详细信息消息仅在页正在访问通过 localhost，且否则显示泛型的运行时错误页时。 这并不理想，但它确保须知的默认行为不显示非本地访问者的异常详细信息。 将来的教程检查`<customErrors>`部分中更多详细信息，并演示如何通过自定义错误页，显示在生产中发生错误时。


跟踪在开发过程中很有用的另一个 ASP.NET 功能。 跟踪，如果启用，记录有关每个传入请求的信息，可提供特殊的网页， `Trace.axd`，查看最近的请求详细信息。 你可以启用和配置跟踪通过[`<trace>`元素](https://msdn.microsoft.com/library/6915t83k.aspx)中`Web.config`。

如果你启用跟踪，请确保它会禁用在生产环境中。 因为跟踪信息包括 cookie、 会话数据和其他潜在的敏感信息，务必禁用在生产环境中的跟踪。 好消息是，默认情况下，禁用跟踪和`Trace.axd`文件才可通过 localhost 访问。 如果您更改这些默认设置在开发过程中确保，它们关闭回在生产环境中。

## <a name="techniques-for-maintaining-separate-configuration-information"></a>用于维护单独的配置信息的技术

在开发和生产环境中具有不同的配置设置将增加复杂性部署过程。 在前面的两个教程中的部署过程涉及复制所有所需的文件从开发到生产环境，但该方法仅适用的配置信息是否这两个环境中相同。 有多种部署具有不同的配置信息的应用程序的技术。 让我们的目录的一些托管的 web 应用程序的这些选项。

### <a name="manually-deploying-the-production-environment-configuration-file"></a>手动部署生产环境配置文件

最简单方法是维护两个版本的`Web.config`文件： 一个用于开发环境，一个用于生产环境。 部署到生产环境的站点需要的所有文件复制到生产服务器在开发环境中*除*为`Web.config`文件。 相反，生产环境而异`Web.config`文件都复制到生产环境。

此方法不是非常复杂，但很容易地实现，因为不经常更改配置信息。 它最适用于应用程序与小型开发团队，托管在单个 web 服务器上，很少更改其配置信息。 手动部署使用独立的 FTP 客户端应用程序文件时，它是最 tenable。 使用 Visual Studio 的复制网站工具或发布选项时，你将需要首先换出特定于部署的`Web.config`之前部署，与生产特定文件，然后在部署完成后重新交换它们。

### <a name="change-the-configuration-during-the-build-or-deployment-process"></a>更改在部署过程的生成配置

到目前为止的讨论具有假定即席生成和部署过程。 许多大型软件项目中有更规范化的开放源代码，自行，使用或第三方工具的进程。 对于所有这些项目可以可能自定义生成或部署过程之前它将推送到生产适当地修改的配置信息。 如果你要构建 web 应用程序使用[MSBuild](http://en.wikipedia.org/wiki/MSBuild)， [NAnt](http://nant.sourceforge.net/)，或某些其他生成工具，可以可能添加生成步骤以修改`Web.config`文件以包括特定于生产的设置。 或部署工作流无法以编程方式连接到源代码管理服务器并检索相应`Web.config`文件。

获取到生产环境的相应配置信息的实际方法广泛根据你的工具和工作流而有所不同。 在这种情况下，我们不将深入探讨此主题会进一步。 如果你使用如 MSBuild 或 NAnt 的常用的生成工具部署文章和教程可以查找特定于这些工具通过 web 搜索。

### <a name="managing-configuration-differences-via-the-web-deployment-project-add-in"></a>通过 Web 部署的管理配置差异外接程序项目

2006 年 Microsoft Web 开发项目外接程序为发布 Visual Studio 2005。 外接程序的 Visual Studio 2008 年发布 2008年。 此外接程序中允许 ASP.NET 开发人员创建一个单独的 Web 部署项目，以及其 web 应用程序项目，生成时，显式编译 web 应用程序并将复制的文件将部署到本地的输出目录。 Web 应用程序项目在幕后使用 MSBuild。

默认情况下，开发环境的`Web.config`文件复制到输出目录中，但你可以设置 Web 部署项目，以自定义

通过以下方式获取复制到此目录的配置信息：

- 通过`Web.config`文件部分更换过程中，在其中指定要替换的部分和一个包含替换文本的 XML 文件。
- 通过提供外部配置源文件的路径。 选择此选项，Web 部署项目复制特定`Web.config`到输出目录的文件 (而不是`Web.config`在开发环境中使用的文件)。
- 通过将自定义规则添加到使用 Web 部署项目的 MSBuild 文件。

若要部署 web 应用程序生成 Web 部署项目，然后将文件从项目的输出文件夹复制到生产环境。

若要了解有关使用 Web 部署项目的详细信息请查看[Web 部署项目本文](https://msdn.microsoft.com/magazine/cc163448.aspx)从 2007 年 4 月问题的[MSDN 杂志](https://msdn.microsoft.com/magazine/default.aspx)，或查阅处的其他阅读材料一节中的链接本教程末尾。

> [!NOTE]
> 不能与 Visual Web Developer 中使用 Web 部署项目，因为 Web 部署项目实现为 Visual Studio 外接程序和 Visual Studio Express 版本 （包括 Visual Web Developer） 不支持外接程序。


## <a name="summary"></a>摘要

外部资源和 web 应用程序开发中的行为是通常不同于相同的应用程序时在生产环境中。 例如，数据库连接字符串、 编译选项，以及通常发生未处理的异常时的行为不同环境之间。 在部署过程必须适应这些区别。 如我们在本教程中所述，最简单方法是手动将备用的配置文件复制到生产环境。 更适合的解决方案，还可能使用 Web 部署项目外接程序时，或使用更正式的生成或部署进程可以满足此类自定义项。

尽情享受编程 ！

### <a name="further-reading"></a>其他阅读材料

在本教程中讨论的主题的详细信息，请参阅以下资源：

- [所述的连接字符串](http://www.connectionstrings.com/Articles/Show/what-is-a-connection-string)
- [数据库连接字符串 @ ConnectionStrings.com](http://www.connectionstrings.com/)
- [不要运行生产 ASP.NET 应用程序与`debug="true"`启用](https://weblogs.asp.net/scottgu/Don_1920_t-run-production-ASP.NET-Applications-with-debug_3D001D20_true_1D20_-enabled)
- [正常响应未经处理的异常-显示用户友好的错误页](http://aspnet.4guysfromrolla.com/articles/090606-1.aspx)
- [我如何： 使用 Visual Studio 2008 Web 部署项目？](../../../videos/how-do-i/how-do-i-use-a-visual-studio-2008-web-deployment-project.md)
- [部署数据库时的密钥配置设置](http://aspnet.4guysfromrolla.com/articles/121008-1.aspx)
- [Visual Studio 2008 Web 部署项目下载](https://www.microsoft.com/downloads/details.aspx?FamilyId=0AA30AE8-C73B-4BDD-BB1B-FE697256C459&amp;displaylang=en) | [Visual Studio 2005 Web 部署项目下载](https://download.microsoft.com/download/9/4/9/9496adc4-574e-4043-bb70-bc841e27f13c/WebDeploymentSetup.msi)
- [VS 2008 Web 部署项目](https://weblogs.asp.net/scottgu/archive/2005/11/06/429723.aspx) | [VS 2008 Web 部署项目支持发布](https://weblogs.asp.net/scottgu/archive/2008/01/28/vs-2008-web-deployment-project-support-released.aspx)
- [Web 部署项目](https://msdn.microsoft.com/magazine/cc163448.aspx)

>[!div class="step-by-step"]
[上一页](deploying-your-site-using-visual-studio-vb.md)
[下一页](core-differences-between-iis-and-the-asp-net-development-server-vb.md)
