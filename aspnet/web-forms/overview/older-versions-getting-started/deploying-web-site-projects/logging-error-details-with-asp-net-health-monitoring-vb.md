---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-vb
title: "与 ASP.NET 运行状况监视 (VB) 中记录错误详细信息 |Microsoft 文档"
author: rick-anderson
description: "Microsoft 的运行状况监视系统地轻松且可自定义日志各种 web 事件，包括未经处理的异常。 本教程将指导 thr..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: 09a6c74e-936a-4c04-8547-5bb313a4e4a3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-vb
msc.type: authoredcontent
ms.openlocfilehash: 83f7504e3aeb02ed222712e7e51f612f7ffd5744
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
<a name="logging-error-details-with-aspnet-health-monitoring-vb"></a>日志记录错误的详细信息，ASP.NET 运行状况监视 (VB)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载代码](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_13_VB.zip)或[下载 PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial13_HealthMonitoring_vb.pdf)

> Microsoft 的运行状况监视系统地轻松且可自定义日志各种 web 事件，包括未经处理的异常。 本教程将指导完成设置过程的运行状况监视系统，用于记录到数据库的未经处理的异常以及用于通知开发人员通过电子邮件。


## <a name="introduction"></a>介绍

日志记录是用于监视部署的应用程序的运行状况和诊断任何可能出现的问题的有用工具。 尤其是，务必记录，以便它们可以解决在部署的应用程序中发生的错误。 `Error` ASP.NET 应用程序; 中发生未经处理的异常时将引发事件[前面教程](processing-unhandled-exceptions-vb.md)介绍了如何通知错误的开发人员和通过创建的事件处理程序日志其详细信息`Error`事件。 但是，创建`Error`事件处理程序来记录错误的详细信息并通知开发人员是不必要的与通过 ASP 执行此任务。NET 的*监视系统的运行状况*。

监视系统的运行状况在 ASP.NET 2.0 中引入，它旨在监视部署的 ASP.NET 应用程序的运行状况，通过在应用程序或请求的生存期的过程中发生的日志记录事件。 事件所记录的运行状况监视系统被称为*运行状况监视事件*或*Web 事件*，并且包括：

- 应用程序生命周期事件，例如当应用程序启动或停止
- 安全事件，包括登录尝试失败，操作失败，URL 授权请求
- 应用程序错误，包括未经处理的异常，异常，请求验证异常以及其他类型的错误中的编译错误，分析的视图状态。

如果在运行状况监视事件引发它可以记录到任意数量的指定*日志源*。 监视系统的运行状况随附了 Web 事件记录到 Windows 事件日志，或通过电子邮件，其他的 Microsoft SQL Server 数据库的日志源。 你还可以创建自己的日志源。

在中定义的事件日志的监视系统的运行状况，以及使用，日志源`Web.config`。 使用几行配置标记中，你可以使用运行状况监视记录到数据库的所有未经处理的异常并通知你通过电子邮件的异常。

## <a name="exploring-the-health-monitoring-systems-configuration"></a>浏览运行状况监视系统的配置

监视系统的行为的运行状况由其配置信息，它位于[`<healthMonitoring>`元素](https://msdn.microsoft.com/library/2fwh2ss9.aspx)中`Web.config`。 此外，此配置节定义信息的以下三个重要的部分：

1. 监视事件的运行状况，如果引发，则应记录
2. 日志源，并
3. 如何将每个运行状况监视在 (1) 中定义的事件映射到的日志源 (2) 中定义。

通过三个子级配置元素指定此信息： [ `<eventMappings>` ](https://msdn.microsoft.com/library/yc5yk01w.aspx)， [ `<providers>` ](https://msdn.microsoft.com/library/zaa41kz1.aspx)，和[ `<rules>`](https://msdn.microsoft.com/library/fe5wyxa0.aspx)分别。

在找不到监视系统配置信息的默认运行状况`Web.config`文件中`%WINDIR%\Microsoft.NET\Framework\version\CONFIG`文件夹。 此默认的配置信息，为简便起见，删除一些标记如下所示：

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample1.xml)]

监视感兴趣的事件中定义的运行状况`<eventMappings>`元素，使运行状况监视事件的类的用户友好名称。 在上面的标记`<eventMappings>`元素将用户友好名称"的所有错误"分配给运行状况监视类型的事件`WebBaseErrorEvent`和名称"失败审核"记录到运行状况监视类型的事件`WebFailureAuditEvent`。

`<providers>`元素定义日志源，为其提供用户友好名称和指定的任何日志特定源的配置信息。 第一个`<add>`元素定义"EventLogProvider"提供程序，该记录指定的运行状况监视事件使用提供`EventLogWebEventProvider`类。 `EventLogWebEventProvider`类将事件记录到 Windows 事件日志。 第二个`<add>`元素定义"SqlWebEventProvider"提供程序，后者会将事件记录到通过 Microsoft SQL Server 数据库`SqlWebEventProvider`类。 "SqlWebEventProvider"配置指定数据库的连接字符串 (`connectionStringName`) 以及其他配置选项。

`<rules>`元素映射中指定的事件`<eventMappings>`元素登录源`<providers>`元素。 默认情况下，ASP.NET web 应用程序记录所有未经处理的异常，并且审核失败到 Windows 事件日志中。

## <a name="logging-events-to-a-database"></a>到数据库的日志记录事件

监视系统的默认配置的运行状况可以自定义基于 web 的应用程序的 web 应用程序通过添加`<healthMonitoring>`到应用程序的部分`Web.config`文件。 你可以包括中的其他元素`<eventMappings>`， `<providers>`，和`<rules>`部分使用`<add>`元素。 若要删除设置默认配置，请使用从`<remove>`元素或使用`<clear />`从这些部分之一中删除所有默认值。 让我们图书评论 web 应用程序配置为记录所有未经处理的异常到 Microsoft SQL Server 数据库使用`SqlWebEventProvider`类。

`SqlWebEventProvider`类是运行状况监视系统的一部分，并记录运行状况监视到指定的 SQL Server 数据库的事件。 `SqlWebEventProvider`类需要指定的数据库包括一个名为的存储的过程`aspnet_WebEvent_LogEvent`。 此存储的过程传递事件的详细信息和担负存储事件详细信息。 好消息是你不需要创建此存储过程，也不用于存储事件详细信息表。 你可以将这些对象添加到你的数据库使用`aspnet_regsql.exe`工具。

> [!NOTE]
> `aspnet_regsql.exe`工具所讨论进来[*配置网站，使用应用程序服务*教程](configuring-a-website-that-uses-application-services-vb.md)当我们增加了对 ASP 支持。NET 的应用程序服务。 因此，图书评论网站的数据库已包含`aspnet_WebEvent_LogEvent`存储过程，事件将信息存储到名为的表`aspnet_WebEvent_Events`。


必需存储的过程并添加到你的数据库表后，所有这些保持是指示运行状况监视要到数据库中记录所有未经处理的异常。 完成此操作通过将以下标记添加到你的网站`Web.config`文件：

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample2.xml)]

运行状况监视上面使用的配置标记`<clear />`元素擦除监视中的配置信息的预定义运行状况`<eventMappings>`， `<providers>`，和`<rules>`部分。 然后，它将单个条目添加到以下各节中。

- `<eventMappings>`元素定义单个运行状况监视感兴趣名为"所有错误"，每次发生未经处理的异常时引发的事件。
- `<providers>`元素定义名为"SqlWebEventProvider"使用单个日志源`SqlWebEventProvider`类。 `connectionStringName`属性已设置为"ReviewsConnectionString"，这是我们连接的名称中定义的字符串`<connectionStrings>`部分。
- 最后，&lt;规则&gt;元素指示，在"所有错误"事件调查时它应记录使用"SqlWebEventProvider"提供程序。

此配置信息指示运行状况监视系统到图书评论数据库中记录所有未经处理的异常。

> [!NOTE]
> `WebBaseErrorEvent`事件只出现服务器错误; 它不会引发 HTTP 错误，如找不到 ASP.NET 资源的请求。 这不同于的行为`HttpApplication`类的`Error`为服务器和 HTTP 错误引发的事件。


若要查看运行状况监视操作中的系统，请访问网站和生成运行时错误，请访问`Genre.aspx?ID=foo`。 你应看到相应的错误页-异常详细信息黄色屏幕的死亡 （时访问本地） 或自定义错误页 （当来访生产中的站点）。 在后台，监视系统的运行状况记录到数据库的错误信息。 应在一条记录`aspnet_WebEvent_Events`表 (请参阅**图 1**); 该记录包含有关刚刚发生运行时错误的信息。

[![](logging-error-details-with-asp-net-health-monitoring-vb/_static/image2.png)](logging-error-details-with-asp-net-health-monitoring-vb/_static/image1.png)

**图 1**： 错误详细信息记录到`aspnet_WebEvent_Events`表  
([单击以查看实际尺寸的图像](logging-error-details-with-asp-net-health-monitoring-vb/_static/image3.png))

### <a name="displaying-the-error-log-in-a-web-page"></a>在网页中显示错误日志

使用网站的当前配置，监视系统的运行状况到数据库中记录所有未经处理的异常。 但是，运行状况监视不提供任何机制来查看错误日志通过网页。 但是，你可以生成显示此信息从数据库的 ASP.NET 页。 （如我们马上就能看到，你可以选择拥有电子邮件发送给你的错误详细信息。）

如果你创建这样的页，请确保您采取措施来仅允许授权的用户以查看错误详细信息。 如果你的站点已使用用户帐户，则可以使用 URL 授权规则来限制对某些用户或角色页的访问。 有关如何授予或限制对基于在已登录用户的网页的访问的详细信息，请参阅我[网站安全教程](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)。

> [!NOTE]
> 后续教程探讨了名为 ELMAH 的备用的错误日志记录和通知系统。 ELMAH 包括内置的机制来查看错误日志从这两个网页和为 RSS 源。


## <a name="logging-events-to-email"></a>日志事件记录到电子邮件

监视系统的运行状况包括到电子邮件"记录"事件日志源提供程序。 日志源包括记录到电子邮件消息正文中的数据库的相同信息。 你可以使用此日志源以在特定运行状况监视事件发生时通知开发人员。

让我们更新网站的配置，以便我们收到一封电子邮件每当异常发生图书评论。 若要实现此目的，我们需要执行三个任务：

1. 配置 ASP.NET web 应用程序发送电子邮件。 这通过指定如何通过发送电子邮件消息实现`<system.net>`配置元素。 有关发送电子邮件的详细信息请参阅 ASP.NET 应用程序中的消息[在 ASP.NET 中发送电子邮件](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)和[System.Net.Mail 常见问题](http://systemnetmail.com/)。
2. 注册中的电子邮件日志源提供程序`<providers>`元素，并
3. 添加到一个条目`<rules>`映射到在步骤 (2) 中添加的日志源提供程序的"所有错误"事件的元素。

监视系统的运行状况包含两个电子邮件日志源提供程序类：`SimpleMailWebEventProvider`和`TemplatedMailWebEventProvider`。 [ `SimpleMailWebEventProvider`类](https://msdn.microsoft.com/library/system.web.management.simplemailwebeventprovider.aspx)发送纯文本电子邮件消息，其中包含事件详细信息，并提供电子邮件正文的稍加自定义。 与[`TemplatedMailWebEventProvider`类](https://msdn.microsoft.com/library/system.web.management.templatedmailwebeventprovider.aspx)指定其呈现的标记用作电子邮件正文 ASP.NET 页。 [ `TemplatedMailWebEventProvider`类](https://msdn.microsoft.com/library/system.web.management.templatedmailwebeventprovider.aspx)为你提供的内容和格式的电子邮件消息中，多更好地控制，但需要更多的前期工作因为你必须创建 ASP.NET 页生成电子邮件消息的正文。 本教程重点介绍使用`SimpleMailWebEventProvider`类。

更新运行状况监视系统的`<providers>`中的元素`Web.config`文件以包含的日志源`SimpleMailWebEventProvider`类：

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample3.xml)]

上面的标记使用`SimpleMailWebEventProvider`类作为日志源提供程序，并将其分配的友好名称"EmailWebEventProvider"。 此外，`<add>`属性包含其他配置选项，如收件人，以及从电子邮件地址。

与电子邮件日志源定义，剩下的就是以指示运行状况监视系统以使用此源可"日志"未经处理的异常。 这通过添加新规则中的实现`<rules>`部分：

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample4.xml)]

`<rules>`部分现在包括两个规则。 第一个名为"所有错误到的电子邮件"，将所有未经处理的异常发送到"EmailWebEventProvider"日志源。 此规则的效果将网站上的有关错误的详细信息发送到指定的地址。 "到数据库的所有错误"规则记录的错误详细信息的站点的数据库。 因此，无论何时未经处理的异常发生在站点上其详细信息会同时记录到数据库而发送到指定的电子邮件地址。

**图 2**显示由生成的电子邮件`SimpleMailWebEventProvider`类访问时`Genre.aspx?ID=foo`。

[![](logging-error-details-with-asp-net-health-monitoring-vb/_static/image5.png)](logging-error-details-with-asp-net-health-monitoring-vb/_static/image4.png)

**图 2**： 按电子邮件发送错误详细信息  
([单击以查看实际尺寸的图像](logging-error-details-with-asp-net-health-monitoring-vb/_static/image6.png))

## <a name="summary"></a>摘要

ASP.NET 运行状况监视系统设计用于允许管理员监视已部署的 web 应用程序的运行状况。 运行状况监视事件引发时展开某些操作，例如当应用程序停止时，当用户成功登录到站点，或发生未处理的异常时引发。 可以将这些事件记录到任意数量的日志源。 本教程介绍了如何将登录到数据库以及通过电子邮件的未经处理的异常的详细信息。

本教程侧重于使用运行状况监视日志未经处理的异常，但请记住，运行状况监视旨在度量部署的 ASP.NET 应用程序的总体运行状况，包括丰富的运行状况监视事件和不日志源此处介绍了。 什么是多个，你可以创建你自己运行状况监视事件和日志源的需求出现。 如果你有兴趣了解更多有关运行状况监视，良好的第一步是读取[艾力克 Reitan](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)的[运行状况监视常见问题](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)。 接下来，请查阅[How To： 在 ASP.NET 2.0 中使用运行状况监视](https://msdn.microsoft.com/library/ms998306.aspx)。

尽情享受编程 ！

### <a name="further-reading"></a>其他阅读材料

在本教程中讨论的主题的详细信息，请参阅以下资源：

- [ASP.NET 运行状况监视概述](https://msdn.microsoft.com/library/bb398933.aspx)
- [配置和自定义监视的 ASP.NET 的系统的运行状况](http://dotnetslackers.com/articles/aspnet/ConfiguringAndCustomizingTheHealthMonitoringSystemOfASPNET.aspx)
- [常见问题-运行状况监视在 ASP.NET 2.0](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)
- [如何： 发送电子邮件运行状况监视通知](https://msdn.microsoft.com/library/ms227553.aspx)
- [如何： 使用 ASP.NET 中的运行状况监视](https://msdn.microsoft.com/library/ms998306.aspx)
- [在 ASP.NET 中监视的运行状况](http://aspnet.4guysfromrolla.com/articles/031407-1.aspx)

>[!div class="step-by-step"]
[上一页](processing-unhandled-exceptions-vb.md)
[下一页](logging-error-details-with-elmah-vb.md)
