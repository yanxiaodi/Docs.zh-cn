---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/asp-net-hosting-options-vb
title: "ASP.NET 宿主选项 (VB) |Microsoft 文档"
author: rick-anderson
description: "ASP.NET web 应用程序通常设计，创建，并在本地开发环境中测试并需要将其部署到生产环境 o..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: 492f5ae2-bad7-4107-89a9-f04a9525dee7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/asp-net-hosting-options-vb
msc.type: authoredcontent
ms.openlocfilehash: 54bac82a96a35d871d764849856c8e31f6570666
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
<a name="aspnet-hosting-options-vb"></a>ASP.NET 宿主选项 (VB)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载 PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial01_Basics_vb.pdf)

> ASP.NET web 应用程序通常设计，创建，并在本地开发环境和需要准备好发布后将部署到生产环境中测试。 本教程提供部署过程的高级概述，并充当本系列教程的介绍。


## <a name="introduction"></a>介绍

Web 应用程序通常设计、 创建和测试的开发环境中，只有在站点上工作的程序员访问。 准备好发布应用程序后，它被移动到生产环境然后可以通过 Internet 上的任何人访问站点。 此部署过程引入了许多挑战：

- 生产环境中必须存在并且可正确安装程序，可以部署 ASP.NET 应用程序; 之前此外，生产环境必须保持最新的最新安全修补程序。
- 到生产环境，必须从开发环境复制正确的标记文件、 代码文件和支持文件集。 对于数据驱动的应用程序，这可能需要复制的数据库架构和/或数据，以及。
- 可能有配置两个环境之间的差异。 在开发环境中使用的数据库连接字符串或电子邮件服务器将可能不同于生产环境。 更重要的是，应用程序的行为可能依赖于环境。 例如，在开发过程中发生错误时可以在屏幕上，显示错误的详细信息，但在生产中发生错误时, 应相反，显示一个用户友好的错误页面和错误详细信息通过电子邮件发送给开发人员。

若要替代的设置和维护生产环境中的第一次质询许多个人和企业外包其生产环境到*的 web 宿主提供程序*。 Web 宿主提供程序是公司管理代表你的生产环境。 有大量的 web 宿主提供程序，每个都有不同的价格和服务级别;请参阅有关查找此类的服务提供程序的提示的"查找 Web 宿主提供程序"部分。

这是一系列查看部署到生产环境中管理 web 宿主提供程序的 ASP.NET web 应用程序所涉及的步骤的教程中的第一个。 在过程中这些教程中，我们将检查：

- 需要什么文件部署到 web 宿主提供程序。
- 用于简化部署过程的工具。
- 如何将数据库部署。
- 部署使用的数据库提示[基于 SQL 的成员资格和角色提供程序](../../older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs.md)，以及如何模拟生产环境中的网站管理工具。
- 顺利地在开发过程中所做的更改更新生产中的数据库的策略。
- 在生产环境，以及在发生错误时通知开发人员的方式发生的日志记录错误的技术。

这些教程适合是简洁并提供引导你完成此过程以可视方式并提供充足的屏幕截图的分步说明。 此篇教程概述了查找 web 宿主提供程序的 ASP.NET 部署过程和建议。 让我们进入正题！

## <a name="an-overview-of-the-aspnet-deployment-process"></a>ASP.NET 部署过程的概述

简而言之，部署 ASP.NET 应用程序涉及以下三个步骤：

1. 在生产环境中配置 web 应用程序、 web 服务器和数据库。
2. 同步 ASP.NET 页、 代码文件中中的程序集`Bin`文件夹，然后与 HTML 相关的支持文件，如 CSS 和 JavaScript 文件。
3. 同步的数据库架构和/或数据。

Web 应用程序的配置信息通常位于`Web.config`文件中，并包括 URL 重写规则和电子邮件服务器信息的数据库的连接字符串，错误处理条件。 通常，此信息与不同中而不是在生产环境中相同的应用程序开发的应用程序。 例如，当开发的应用程序最好是使用开发数据库，以便不针对生产数据库的测试。 因此，开发和生产应用程序之间的数据库连接字符串也通常有所不同。 由于这些差异，部署的一部分涉及到对 web 应用程序的配置信息进行更改。

Web 应用程序配置更改，除了步骤 1 还可能需要为 web 服务器和数据库的配置。 例如，如果将 ASP.NET 页创建或从 web 服务器上的目录中删除文件然后需要为 web 服务器配置为允许这些文件系统修改。 同样，可能需要对数据库进行的权限或身份验证设置。


步骤 2 涉及到同步组基本的 ASP.NET 页面和开发和生产环境之间的支持文件。 这组特定的 ASP。提供与.NET 相关的文件需要两个环境之间同步取决于你在 Visual Studio 中，创建并在下一步的教程中，讨论才具有的项目的类型*[确定文件需要什么部署到](determining-what-files-need-to-be-deployed-vb.md)*. 第三个和第四个教程- *[部署你的站点使用 FTP](deploying-your-site-using-an-ftp-client-vb.md)*和*[部署您站点使用 Visual Studio](deploying-your-site-using-visual-studio-vb.md) * -检查不同的工具和技术用于这些文件的同步。

当生成通常为两个数据库正在使用的数据驱动的应用程序有： 一个用于开发，一个在生产上。 在开发期间，开发数据库的架构可能修改为包含新表、 列、 存储的过程和触发器，或可能修改以删除或重命名现有数据库对象。 进行这些更改的时间，应用程序部署到生产环境的时间，开发和生产数据库不同步。此异步需要解决在部署过程。 将来的教程中，将检查这些挑战。

## <a name="finding-a-web-host-provider"></a>查找 Web 宿主提供程序

ASP.NET 应用程序可以部署到任何 web 服务器上的.NET Framework 和 Internet 信息服务 (IIS) 安装。 可以从个人计算机中，托管站点，假设你具有宽带连接到 Internet，了解如何配置路由器以允许传入 web 请求。 许多公司一样，你也无法承载从 intranet 中的计算机的站点。 但是，这些教程中，焦点承载你的网站与 web 宿主提供程序。

> [!NOTE]
> [IIS](https://www.iis.net/)是 Microsoft 的企业级 web 服务器。 它附带了 Windows，如 Windows Server 2008 和某些版本的 Windows Vista 的非主页版本。 不需要安装 IIS 以在开发环境中，支持 ASP.NET 应用程序，如 Visual Studio 提供了 ASP.NET 开发 Web 服务器。 但是，ASP.NET 开发 Web 服务器只接受本地连接，并因此不能在生产环境中使用。


可以将您的网站部署到 web 宿主提供程序之前，您必须首先确定业务往来哪些公司。 有大量的 web 托管公司应用商店; 中"web 托管公司"的搜索返回五 100 万个结果。 如何查找一个最适合你？ 你喜爱的搜索引擎是类似的网站是不错的起点， [TopHosts](http://www.tophosts.com/)和[HostCritique](http://www.hostcritique.net/)，其比较和对比各种托管服务。 我还建议要求您的同事和同事任何建议;你还可以请求建议[承载打开论坛](https://forums.asp.net/158.aspx)在此处[ASP.NET 论坛](https://forums.asp.net/)。

Web 托管公司的通常提供共享托管计划，专用托管计划。 与共享承载单个 web 服务器主机数十个，如果不是数百的不同的网站。 使用专用承载你租约从服务于您的网站和你的站点单独的公司的计算机。 共享的宿主计划可能包括 ASP.NET 页中，使其与 Microsoft Access 数据库，5 GB 的磁盘空间和 100 GB 的带宽每月流量适用于 $9.95 每月的功能的支持。 另一个共享的宿主计划可能包括 ASP.NET 页访问 Microsoft SQL Server 2008 数据库服务器、 10 GB 的磁盘空间和带宽每月流量的 250 GB $19.95 每月的支持。 专用托管计划通常成本更高，成本计算数百美元，每个月，但提供更好的性能和更好地控制比共享托管选项。 你选择何种计划取决于你的预算，多少流量收到你的网站，并且需要你预计你的功能。

选择 web 宿主提供程序时的两个重要注意事项是客户服务和服务质量。 如果你有一个问题或配置问题，如何长时间从提交到 web 主机的支持人员联系你的问题，直到获得响应？ 如何可靠是公司的服务？ 他们经常有数据库停机吗？ 频率未其电子邮件服务器脱机？ 你始终可以要求公司可以提供有关其运行时间的详细信息和查询有关其客户服务策略，但更有效的办法是请求的当前和过去客户，你可以通过在线论坛、 新闻组和电子邮件列表服务中执行此操作的反馈.

> [!NOTE]
> 一些 web 托管公司专注于特定的技术堆栈，如.NET 的其业务或[LAMP](http://en.wikipedia.org/wiki/LAMP_stack) (**L** inux， **A** pache， **M**ySQL，和**P** HP)，因此请确保您选择的公司承载 ASP.NET 应用程序。 另请检查以确保它们支持你要用于生成应用程序的 ASP.NET 的版本。 并且，如果要生成数据驱动的应用程序，请确保 web 主机提供的相同的数据库服务器和你使用的版本。


## <a name="summary"></a>摘要

ASP.NET web 应用程序通常设计，创建，并在本地开发环境中测试。 准备好发布版本后，则将它移动到生产环境。 虽然可能托管在你的个人计算机或你公司内的服务器上的 ASP.NET 网站，但许多企业和个人选择外包其托管到 web 主机提供商。

本教程系列检查部署 ASP.NET 应用程序到 web 主机提供商，浏览常见的难题的步骤。 本教程提供 ASP.NET 部署过程的高级概述，并为提示提供用于查找合适的 web 宿主提供程序。 下一教程查看与 ASP.NET 相关的文件需要在部署你的网站时要复制到生产环境。

尽情享受编程 ！

### <a name="special-thanks-to"></a>特别感谢...

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已 Teresa 墨。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)。

>[!div class="step-by-step"]
[上一页](users-and-roles-on-the-production-website-cs.md)
[下一页](determining-what-files-need-to-be-deployed-vb.md)
