---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
title: "监视和遥测 （使用 Azure 构建真实世界云应用） |Microsoft 文档"
author: MikeWasson
description: "构建真实世界云应用程序与 Azure 的电子书基于由 Scott Guthrie 的演示。 它还说明了 13 模式和实践，他可以..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/09/2015
ms.topic: article
ms.assetid: 7e986ab5-6615-4638-add7-4614ce7b51db
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
msc.type: authoredcontent
ms.openlocfilehash: 9baddd1836323385239206a3cf49e5938bbaff58
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="monitoring-and-telemetry-building-real-world-cloud-apps-with-azure"></a>监视和遥测 （使用 Azure 构建真实世界云应用）
====================
通过[Mike Wasson](https://github.com/MikeWasson)， [Rick Anderson](https://github.com/Rick-Anderson)， [Tom Dykstra](https://github.com/tdykstra)

[下载修复此错误项目](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)或[下载电子书](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **构建真实世界云应用程序与 Azure**电子书基于由 Scott Guthrie 的演示。 它还说明了 13 模式和实践，从而帮助你为成功开发适用于云中的 web 应用。 有关电子书的信息，请参阅[第一章](introduction.md)。


许多人依赖于客户能够让他们知道他们的应用程序出现故障时。 就不这么一种最佳做法任意位置，并特别是在云中未。 没有快速通知，不能保证，当你执行获取通知，请你通常将收到有关发生了什么情况的最少监视或误导性数据。 与良好遥测和日志记录系统，你可以在使用你的应用，并在内容了解所发生事件的未会立即找出并具有很有帮助的疑难解答信息以使用错误。

## <a name="buy-or-rent-a-telemetry-solution"></a>购买或出租遥测解决方案

> [!NOTE]
> 本文截稿之前[Application Insights](https://azure.microsoft.com/services/application-insights/)发布。 Application Insights 是遥测解决方案在 Azure 上的首选的方法。 请参阅[设置为你的 ASP.NET 网站的 Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net)有关详细信息。


有关云环境很好的事情之一是它是真的很简单购买或出租当您转到胜利。 遥测是一个示例。 不含大量工作量就可以非常好遥测系统最多并且正在运行，非常节省成本。 存在大量的极佳合作伙伴的集成使用 Azure，并且其中一些必须可用层级 –，以便您可以执行任何操作获取基本遥测。 以下是只是几个与当前可在 Azure 上：

- [New Relic](http://newrelic.com/)
- [AppDynamics](http://www.appdynamics.com/)
- [Dynatrace](https://datamarket.azure.com/application/b4011de2-1212-4375-9211-e882766121ff)

截至 2015 年 3 月， [Microsoft Application Insights for Visual Studio Online](https://azure.microsoft.com/documentation/articles/app-insights-get-started/)尚未发布，但在来试用预览版中可用。[Microsoft System Center](http://www.petri.co.il/microsoft-system-center-introduction.htm#)还包括监视功能。

我们将快速演练设置 New Relic，以显示使用的遥测系统能够多么容易。

在 Azure 管理门户中，为服务注册。 单击**新建**，然后单击**存储**。 **选择外接程序**对话框随即出现。 向下滚动并单击**New Relic**。

![选择外接程序](monitoring-and-telemetry/_static/image1.png)

单击右箭头并选择所需的服务层。 对于本演示中，我们将使用免费层。

![个性化外接程序](monitoring-and-telemetry/_static/image2.png)

单击右箭头，确认"购买"，New Relic 现在显示为在门户中的外接程序。

![查看购买](monitoring-and-telemetry/_static/image3.png)

![在管理门户中的新 Relic 外接程序](monitoring-and-telemetry/_static/image4.png)

单击**连接信息**，并将复制的许可证密钥。

![连接信息](monitoring-and-telemetry/_static/image5.png)

转到**配置**选项卡上，在门户中，web 应用设置**性能监视**到**外接程序**，并设置**选择外接程序**下拉列表到**New Relic**。 然后单击**保存**。

![配置选项卡中的新 Relic](monitoring-and-telemetry/_static/image6.png)

在 Visual Studio 中，安装你的应用程序中的 New Relic NuGet 包。

![配置选项卡中的开发人员分析](monitoring-and-telemetry/_static/image7.png)

将应用部署到 Azure 并开始使用它。 创建几个修复它任务以提供一些活动为 New Relic 监视。

然后转回**New Relic**页面**外接程序**门户并单击选项卡**管理**。 门户将你带到 New Relic 管理门户中，使用单一登录进行身份验证，因此无需再次输入你的凭据。 概述页显示各种性能统计信息。 （单击要查看的概述页完整大小的图像。）

[![新 Relic 监视选项卡](monitoring-and-telemetry/_static/image9.png)](monitoring-and-telemetry/_static/image8.png)

以下是只是几个可以看到的统计信息：

- 在每天的不同时间的平均响应时间。

    ![响应时间](monitoring-and-telemetry/_static/image10.png)
- 在每天的不同时间的吞吐率 （以每分钟的请求）。

    ![吞吐量](monitoring-and-telemetry/_static/image11.png)
- 处理不同的 HTTP 请求所花的服务器 CPU 时间。

    ![Web 事务时间](monitoring-and-telemetry/_static/image12.png)
- 在应用程序代码的不同部分中所用的 CPU 时间：

    ![跟踪详细信息](monitoring-and-telemetry/_static/image13.png)
- 历史性能统计信息。

    ![历史性能](monitoring-and-telemetry/_static/image14.png)
- 服务已被调用到外部服务，如 Blob 服务以及有关如何可靠且高度可响应统计信息。

    ![外部服务](monitoring-and-telemetry/_static/image15.png)

    ![外部服务](monitoring-and-telemetry/_static/image16.png)

    ![外部服务](monitoring-and-telemetry/_static/image17.png)
- 有关在何处世界上或在美国 web 应用程序流量来自何处中的信息。

    ![Geography](monitoring-and-telemetry/_static/image18.png)

你还可以将设置报告和事件。 例如，你可以说随时开始看到错误，将一封电子邮件发送到该问题的警报的支持人员。

![报表](monitoring-and-telemetry/_static/image19.png)

New Relic 仅仅是一个示例的遥测系统;你可以获得所有这些以及其他服务。 云的优点在于，而无需编写任何代码，以及最小，也没有支出突然可以获取有关如何使用你的应用程序和你的客户实际如何遇到大量详细信息。

<a id="log"></a>
## <a name="log-for-insight"></a>要深入探索日志

遥测包是良好的第一步，但你仍必须检测你自己的代码。 遥测服务告知你问题时，并告诉您什么遇到客户，但它可能不为你提供大量的深入了解发生了什么情况在代码中。

你不想要为远程连接到生产服务器，请参阅您的应用程序正在执行什么操作。 这可能是实际，当你具有一台服务器，但什么时候已扩展到数百台服务器并且你不知道你需要远程连接到哪些功能？ 你的日志记录应提供足够的信息永远不会具有到远程连接到生产服务器来分析和调试问题。 你应将日志记录，足够的信息，以便你可以隔离仅通过日志的问题。

### <a name="log-in-production"></a>在生产中记录

许多人开启在生产环境中的跟踪，仅当没有问题，他们想要调试时。 这可以大幅之间加入延迟你发现问题的时间和获取有关它的有用疑难解答信息的时间。 并且你获取的信息可能不是有用的间歇性的错误。

我们建议存储比较便宜的云环境其中是你始终不要在生产环境中的日志记录。 这样，你已有这些记录，并且必须可帮助的历史数据，将发生错误时你分析问题，随着时间的推移开发或在不同的时间定期发生。 你无法自动执行清除过程删除旧日志，但您可能发现设置此类过程，要保留日志比成本更高。

日志记录的额外的费用是信息的普通的故障排除的时间和金钱，你可以通过让所有时出现问题，你需要已提供来节省量相比较。 然后在时有人告诉你所具有的随机错误过一段时间大约 8:00 昨晚，但它们不记得错误时，你可以轻松地查明出现了什么问题。

对于小于 4 $ 另一方面，你仍可以保留 50 千兆字节的日志月份和日志记录的性能影响是普通处理程序，但前提是保留记住-以避免性能瓶颈，请确保你的日志记录库是异步的一件事情。

### <a name="differentiate-logs-that-inform-from-logs-that-require-action"></a>需要操作的日志区分开来通知日志

日志专用于 INFORM （我希望你了解一些） 或 ACT （你希望执行其他操作）。 请注意仅编写 ACT 日志中真正需要个人或一个自动化的过程执行操作的问题。 过多的 ACT 日志将创建干扰，需要太多工作要筛查其所有发现正版问题。 如果你的 ACT 日志自动触发某种操作如发送电子邮件支持人员，避免让数千个此类操作触发的一个问题。

在.NET System.Diagnostics 跟踪日志可以分配错误、 警告、 信息以及调试/详细级别。 你可以通过保留 ACT 日志将错误级别和 INFORM 日志使用较低的级别来区分 INFORM 日志中的 ACT。

![日志级别](monitoring-and-telemetry/_static/image20.png)

### <a name="configure-logging-levels-at-run-time"></a>在运行时配置日志记录级别

值得能够登录始终在生产环境中时，另一种最佳做法是实现日志记录框架，可用于在运行时调整内容层级的在日志记录的详细信息，而无需重新部署或重新启动你的应用程序。 例如当使用中的跟踪功能`System.Diagnostics`错误、 警告、 信息，可以创建和调试/详细记录。 我们建议始终记录错误、 警告、 和信息记录在生产中，并且你将想要能够动态添加的故障排除-情况提出的调试/详细日志记录。

在 Azure App Service web Apps 具有以进行写入的内置支持`System.Diagnostics`日志传输到文件系统、 表存储或 Blob 存储。 你可以选择不同的日志记录级别的每个存储目标，并且你可以在运行过程中的日志记录级别更改无需重新启动你的应用程序。 Blob 存储支持，更便于运行[HDInsight](https://docs.microsoft.com/azure/hdinsight/) analysis 的作业，在将应用程序日志，因为 HDInsight 知晓如何直接使用 Blob 存储。

### <a name="log-exceptions"></a>日志异常

不要只是放置*异常。Tostring （)*日志记录代码中。 省略了大部分的上下文信息。 在 SQL 错误的情况下它留出 SQL 错误号。 所有异常，包括上下文信息、 异常自身和内部异常以确保你要提供的所有内容，将需要进行故障排除。 例如，上下文信息可能包括服务器名称、 事务标识符和用户名称 （但不是密码或任何机密 ！）。

如果你的每个开发人员如何正确的操作日志记录的异常处理，其中一些不会。 若要确保它获取完成右侧方式每次，在生成中记录器接口直接处理的异常： 传递到记录器类的异常对象本身并正确登录异常数据记录器类。

### <a name="log-calls-to-services"></a>对服务的日志调用

我们强烈建议你编写日志每次你的应用程序调用到服务，它是否是数据库或 REST API 或任何外部服务。 日志中包括不仅相对值的指示成功或失败，但所花费的时间每个请求。 在云环境中，你通常将看到与减慢速度，而不是完整的服务中断相关的问题。 这通常需要花费 10 毫秒突然可能开始进行第二个。 当有人告诉你你的应用速度变慢时，你想要能够查看 New Relic 或任何遥测服务具有并验证其体验中，然后你希望能够看起来是你自己的日志，以深入了解为什么说是慢速的详细信息。

### <a name="use-an-ilogger-interface"></a>使用 ILogger 接口

我们建议执行操作时创建生产应用程序是创建一个简单*ILogger*接口并在其中坚持一些方法。 这便于更改日志记录实现更高版本，而不必转通过所有的代码来完成此操作。 我们无法使用`System.Diagnostics.Trace`类整个修复它应用，但改为我们将使用它的日志记录类中实现实际上*ILogger*，并使我们*ILogger*方法调用在整个应用程序。

这样一来，如果你想要使你的日志记录更丰富，你可以替换[ `System.Diagnostics.Trace` ](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio#apptracelogs)要与任何日志记录机制。 例如，你的应用程序随着你可能决定你想要使用更全面的日志记录包如[NLog](http://nlog-project.org/)或[企业库日志记录应用程序块](https://msdn.microsoft.com/library/dn440731(v=pandp.60).aspx)。 ([Log4Net](http://logging.apache.org/log4net/)是另一个常用的日志记录框架，但它不执行异步日志记录。)

使用 NLog 等框架的一个可能的原因是为了便于将分割成独立的大容量和高值数据存储日志记录输出。 它可帮助你高效地存储大量不需要快速对执行查询，同时保持快速 ACT 数据访问的信息数据。

### <a name="semantic-logging"></a>语义的日志记录

执行操作可以生成更多有用的诊断信息的日志记录的相对较新方法，请参阅[企业库语义日志记录应用程序块 （碎片）](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/)。 碎片使用[Windows 事件跟踪](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx)(ETW) 和[EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx)支持在.NET 4.5，可以创建详细的结构化和可查询日志中。 定义为每个类型的日志，可用于自定义你编写的信息的事件的不同方法。 例如，若要记录可能调用了 SQL 数据库错误`LogSQLDatabaseError`方法。 对于该类型的异常，你知道信息的关键部分是错误号，因此你可以将方法签名中包括错误编号参数，并作为单独的域在你编写的日志记录中记录的错误号。 因为该号码是在单独的字段中你可以更轻松可靠地获取报表根据 SQL 错误号，不是你可以使用如果已只需串联成一个消息字符串的错误号。

## <a name="logging-in-the-fix-it-app"></a>记录在解决应用程序

### <a name="the-ilogger-interface"></a>ILogger 接口

下面是*ILogger*修复它应用中的接口。

[!code-csharp[Main](monitoring-and-telemetry/samples/sample1.cs)]

这些方法可用于编写在支持的相同的四个级别的日志*System.Diagnostics*。 TraceApi 方法适用于日志外部服务调用记录有关延迟的信息。 你还可以添加一组用于调试/详细级别的方法。

### <a name="the-logger-implementation-of-the-ilogger-interface"></a>ILogger 接口的记录器实现

接口的实现过程非常简单。 它基本上只调用到标准*System.Diagnostics*方法。 下面的代码段显示的信息方法的所有三个另外两个分别的其他。

[!code-csharp[Main](monitoring-and-telemetry/samples/sample2.cs)]

### <a name="calling-the-ilogger-methods"></a>调用 ILogger 方法

每次代码修复它应用中的捕获异常，它调用*ILogger*方法来记录异常详细信息。 和每次它进行到数据库、 Blob 服务或 REST API 调用时，它启动的调用之前秒表，停止秒表，当服务返回，并记录经过的时间以及成功或失败的相关信息。

请注意，日志消息包含的类名称和方法名称。 它是作为最佳做法，请确保日志消息，它标识应用程序代码部分编写它们。

[!code-csharp[Main](monitoring-and-telemetry/samples/sample3.cs?highlight=6,14,20-21,25)]

为 Fix It 应用程序调用到 SQL 数据库每次，因此现在你可以看到的调用，该方法调用它，以及完全的时间。

![在日志中的 SQL 数据库查询](monitoring-and-telemetry/_static/image21.png)

![](monitoring-and-telemetry/_static/image22.png)

如果您转浏览通过日志，你可以看到数据库调用时间长的时间是变量。 信息可能十分有用： 因为应用程序将所有该错误记录可以分析历史趋势，随着时间的推移数据库服务的执行方式。 例如，大多数情况下时，可能快速服务，但请求可能会失败或者响应可能会降低在每天的某些时间。

每次应用程序上载新文件，则日志，你可以看到完全它所花费的时间上载每个文件时，你可以执行 Blob 服务 – 相同的操作。

![Blob 上载日志](monitoring-and-telemetry/_static/image23.png)

就是代码的要编写每次调用服务，并且现在每当有人回答说他们遇到了问题，你知道完全什么问题，或它是否可以被错误，即使只需缓慢运行只需几个额外的行。 你可以查明问题的来源，而无需远程连接到服务器或启用日志记录后该错误发生，并且希望重新创建它。

## <a name="dependency-injection-in-the-fix-it-app"></a>在解决的依赖关系注入它应用

你可能想知道如何上面所示的示例中的存储库构造函数获取记录器接口的实现：

[!code-csharp[Main](monitoring-and-telemetry/samples/sample4.cs?highlight=6)]

若要将连接接口到实现应用程序使用[依赖关系注入](http://en.wikipedia.org/wiki/Dependency_injection)(DI) 与[AutoFac](http://autofac.org/)。 DI，可使用基于在整个代码的多个位置中的接口的对象，并只需在一个位置中指定接口实例化时，获取使用的实现。 这使得更易于更改实现： 例如，你可能想要替换 NLog 记录器 System.Diagnostics 记录器。 或者，以进行自动测试，可能想要替换记录器的模拟版本。

修复该应用程序使用 DI 所有存储库和的所有控制器中。 控制器类的构造函数获取*ITaskRepository*接口相同的方式存储库获取记录器接口：

[!code-csharp[Main](monitoring-and-telemetry/samples/sample5.cs?highlight=5)]

该应用使用 AutoFac DI 库来自动提供*TaskRepository*和*记录器*这些构造函数的实例。

[!code-csharp[Main](monitoring-and-telemetry/samples/sample6.cs?highlight=8,10)]

此代码基本上显示任何位置，构造函数需要*ILogger*接口，则传递的实例中*记录器*类，并在每次它需要*IFixItTaskRepository*接口，则传递的实例中*FixItTaskRepository*类。

[AutoFac](http://autofac.org/)是你可以使用的很多依赖关系注入框架之一。 另一个常用的一个是[Unity](https://blogs.msdn.com/b/agile/archive/2013/08/20/new-guide-dependency-injection-with-unity.aspx)，这是建议，并且 Microsoft 模式与实践中的支持。

## <a name="built-in-logging-support-in-azure"></a>在 Azure 中的内置日志记录支持

Azure 支持以下类型的[日志记录，用于在 Azure App Service Web Apps](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio):

- System.Diagnostics 跟踪 （你可以打开和关闭和动态设置级别，无需重新启动站点）。
- Windows 事件。
- IIS 日志 (HTTP/FREB)。

Azure 支持以下类型的[云服务中的日志记录](https://docs.microsoft.com/azure/cloud-services/cloud-services-dotnet-diagnostics):

- System.Diagnostics 跟踪。
- 性能计数器。
- Windows 事件。
- IIS 日志 (HTTP/FREB)。
- 监视自定义目录。

修复它应用使用 System.Diagnostics 跟踪。 你只需要做，以启用日志记录在 web 应用的 System.Diagnostics 是在门户中翻转交换机或调用 REST API。 在门户中，单击**配置**选项卡为您的网站，并向下滚动以查看**Application Diagnostics**部分。 您可以打开或关闭日志记录并选择所需的日志记录级别。 你可以将日志写入到文件系统或写入存储帐户的 Azure。

![应用程序诊断和配置选项卡中的站点诊断](monitoring-and-telemetry/_static/image24.png)

启用日志记录在 Azure 中后，你可以看到在 Visual Studio 输出窗口中的日志，因为它们创建。

![流式处理日志菜单](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-viewlogsmenu.png)

![流式处理日志菜单](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-nologsyet.png)

此外可将日志写入你的存储帐户，并且视图与任意这些工具，可以访问 Azure 存储表服务，例如**服务器资源管理器**Visual Studio 中或[Azure 存储资源管理器](https://azure.microsoft.com/features/storage-explorer/)。

![服务器资源管理器中的日志](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-storagelogs.png)

## <a name="summary"></a>摘要

它是非常简单，以实现现成可用遥测系统、 检测登录自己的代码，并配置日志记录在 Azure 中。 和时必须生产问题，遥测系统和自定义日志的组合可帮助你快速解决问题，你的客户趋于主要问题之前。

在[下一章](transient-fault-handling.md)我们将探讨如何处理暂时性错误，因此它们不会变得生产需要调查的问题。

## <a name="resources"></a>资源

有关更多信息，请参见以下资源。

主要与遥测的文档：

- [Microsoft 模式和实践-Azure 指南](https://msdn.microsoft.com/library/dn568099.aspx)。 请参阅检测和遥测指南、 服务计数指南、 运行状况终结点监视模式和运行时重新配置模式。
- [在云中挤压尾： 启用 New Relic 性能监视的 Azure 网站上](http://www.hanselman.com/blog/PennyPinchingInTheCloudEnablingNewRelicPerformanceMonitoringOnWindowsAzureWebsites.aspx)。
- [在 Azure 云服务上的大规模服务的设计的最佳实践](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx)。 Mark Simms 和 Michael Thomassy 白皮书。 请参阅遥测和诊断部分。
- [使用 Application Insights 的下一代开发](https://msdn.microsoft.com/magazine/dn683794.aspx)。 MSDN 杂志文章。

有关日志记录主要文档：

- [语义的日志记录应用程序块 （碎片）](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/)。 Neil Mackenzie 显示与碎片的语义日志记录的大小写。
- [使用语义的日志记录创建结构化且有意义的日志](https://channel9.msdn.com/Events/Build/2013/3-336)。 （视频）儒略历 Dominguez 显示与碎片的语义日志记录的大小写。
- [从 EF6 SQL 日志记录 – 第 1 部分： 简单的日志记录](http://blog.oneunicorn.com/2013/05/08/ef6-sql-logging-part-1-simple-logging/)。 Arthur Vickers 演示如何登录 EF 6 中的实体框架执行的查询。
- [连接复原和与实体框架中的 ASP.NET MVC 应用程序的命令截获](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)。 第四个在包含 9 个一部分教程系列中，演示如何使用 EF 6 命令截获功能来记录实体框架通过发送到数据库的 SQL 命令。
- [改进日志记录使用 C# 5.0 调用方信息特性](http://www.dotnetcurry.com/showarticle.aspx?ID=972)。 如何轻松地记录不进行硬编码到文本或使用反射来手动获取它的调用的方法的名称。

主要有关故障排除的文档：

- [Azure 的故障排除&amp;调试博客](https://blogs.msdn.com/b/kwill/)。
- [AzureTools – 诊断实用程序使用的 Azure 开发人员支持团队](https://blogs.msdn.com/b/kwill/archive/2013/08/26/azuretools-the-diagnostic-utility-used-by-the-windows-azure-developer-support-team.aspx?Redirected=true)。 介绍并提供用于在 Azure VM 上下载并运行各种诊断和监视工具的工具的下载链接。 如果你需要进行诊断的特定 VM 上的问题非常有用。
- [对 Azure App Service 中使用 Visual Studio 中的 web 应用进行故障排除](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio)。 关于如何开始使用 System.Diagnostics 跟踪和远程调试的分步教程。

视频：

- [防故障： 构建可扩展、 有弹性的云服务](https://channel9.msdn.com/Series/FailSafe)。 包含 9 个部分组成的系列 Ulrich Homann、 Marc Mercuri 和 Mark Simms。 高级概念和体系结构原理以非常可访问且有趣方式，提供与 Microsoft 客户咨询团队 (CAT) 体验与实际客户从绘制的情景。 有关监视和遥测是段数 4 和 9。 段 9 包括监视服务 MetricsHub、 AppDynamics、 New Relic 中，和 PagerDuty 的概述。
- [构建大： 从 Azure 客户的第 ii 部分到的经验教训](https://channel9.msdn.com/Events/Build/2012/3-030)。 Mark Simms 介绍的失败的设计和检测的所有内容。 类似于防故障系列但进入更多操作指南的详细信息。

代码示例：

- [云服务在 Azure 中的基础知识](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649)。 通过 Microsoft Azure 客户咨询团队创建了示例应用程序。 以下文章中所述演示遥测和日志记录方案。 此示例使用实现应用程序日志记录[NLog](http://nlog-project.org/)。 相关文档，请参阅[系列有关遥测和日志记录的四个 TechNet wiki 文章](https://social.technet.microsoft.com/wiki/contents/articles/17987.cloud-service-fundamentals.aspx#Telemetry_coming_soon)。

>[!div class="step-by-step"]
[上一页](design-to-survive-failures.md)
[下一页](transient-fault-handling.md)
