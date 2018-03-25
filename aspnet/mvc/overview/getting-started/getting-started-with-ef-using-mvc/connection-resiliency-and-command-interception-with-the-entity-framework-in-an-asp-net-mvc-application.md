---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application
title: "连接复原和与实体框架中的 ASP.NET MVC 应用程序的命令截获 |Microsoft 文档"
author: tdykstra
description: "Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 6 Code First 和 Visual Studio 的 ASP.NET MVC 5 应用程序..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/13/2015
ms.topic: article
ms.assetid: c89d809f-6c65-4425-a3fa-c9f6e8ac89f2
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 1a28284e203904cc943e5e46b369e8a58ea5c820
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="connection-resiliency-and-command-interception-with-the-entity-framework-in-an-aspnet-mvc-application"></a>连接复原和命令截获与实体框架中的 ASP.NET MVC 应用程序
====================
通过[Tom Dykstra](https://github.com/tdykstra)

[下载已完成的项目](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)或[下载 PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 6 Code First 和 Visual Studio 2013 的 ASP.NET MVC 5 应用程序。 有关教程系列的信息，请参阅[序列中的第一个教程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。


到目前为止应用程序已经运行本地 IIS Express 在开发计算机上。 若要使实际的应用程序可用于通过 Internet 使用其他人，你必须将其部署到 web 宿主提供程序，并且必须将数据库部署到数据库服务器。

在本教程中，你将了解如何使用 Entity Framework 6 的两个特别有价值，当你要部署到云环境的功能： 连接复原 （自动重试的暂时性错误） 和命令截获 （捕捉所有 SQL 查询发送到数据库才能登录或更改它们）。

此连接复原能力和命令截获教程是可选的。 如果你跳过本教程中，将具有少量小的调整将在后续教程中进行。

## <a name="enable-connection-resiliency"></a>启用连接复原

在部署到 Windows Azure 应用程序时，你将部署到 Windows Azure SQL Database、 云数据库服务数据库。 连接到云数据库服务时与你的 web 服务器和数据库服务器直接连接在一起位于同一数据中心时，暂时性连接错误是通常更频繁。 即使在同一数据中心中承载的云 web 服务器和云数据库服务时，有更多的网络连接，它们将会出现问题，例如负载平衡器之间。

此外云服务通常由其他用户共享，这意味着其响应能力可能受到它们。 并且您对数据库访问权限可能受到限制。 当你尝试访问更多经常大于所允许你服务级别协议 (SLA) 中时限制意味着数据库服务将引发异常。

许多或大多数连接问题时你正在访问云服务是暂时的也就是说，它们自身解决在短时间的时间。 因此当你尝试数据库操作，并获取一种是通常暂时性的错误，稍等片刻，该操作可能会成功后无法重试该操作。 如果通过自动重试，处理暂时性错误，可以为你的用户提供更好的体验使其中的大多数客户不可见。 连接复原功能 Entity Framework 6 中的自动执行过程的重试失败的 SQL 查询。

针对特定数据库服务，必须适当地配置连接复原功能：

- 它必须知道哪些异常很可能是暂时的。 你想要重试错误导致通过暂时丢失网络连接，不由程序 bug，例如引起的错误。
- 它必须等待适当的失败的操作的重试之间的时间量。 你可以等待，再进行批处理的重试之间不是用户正在等待响应的联机 web 页可以。
- 它具有重试之前的适当次数。 你可能想要重试更多次，就像在联机应用程序中的批处理中。

你可以配置这些设置的任何数据库环境中支持的实体框架提供程序中，手动但通常适用于的联机应用程序使用 Windows Azure SQL 数据库的默认值已为你配置和这些都是你将实施 Contoso 大学应用程序的设置。

你所要做，以启用连接复原能力是中派生自程序集创建一个类[DbConfiguration](https://msdn.microsoft.com/data/jj680699.aspx)类和在该类中设置 SQL 数据库*执行策略*，即 EF 中另一个术语*重试策略*。

1. 在 DAL 文件夹中，添加一个名为的类文件*SchoolConfiguration.cs*。
2. 模板代码替换为以下代码：

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

    实体框架将自动运行它派生自的类中找到的代码`DbConfiguration`。 你可以使用`DbConfiguration`类来执行此在中否则所执行的操作的代码中的配置任务*Web.config*文件。 有关详细信息，请参阅[EntityFramework 基于代码的配置](https://msdn.microsoft.com/data/jj680699)。
3. 在*StudentController.cs*，添加`using`语句`System.Data.Entity.Infrastructure`。

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]
4. 更改所有`catch`阻止该 catch`DataException`异常，以便它们捕获`RetryLimitExceededException`异常相反。 例如:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=1)]

    已使用`DataException`来尝试标识可能是暂时的以便提供友好的"重试"消息的错误。 但现在，你已启用的重试策略，可能是暂时性的唯一错误将已尝试并失败几次和实际返回的异常将包装在`RetryLimitExceededException`异常。

有关详细信息，请参阅[实体框架连接复原 / 重试逻辑](https://msdn.microsoft.com/data/dn456835)。

## <a name="enable-command-interception"></a>启用命令截获

现在，你已启用的重试策略，如何进行你测试以验证它是否按预期工作？ 不会轻易地强制暂时性错误发生，尤其是当您在本地，运行和它将会特别困难，可以将实际的暂时性错误集成到自动的单元测试。 若要测试连接复原功能，需要一种方法将截获实体框架将发送到 SQL Server 的查询并将替换为通常暂时性异常类型的 SQL Server 响应。

此外可以使用查询截获，以实现云应用程序的最佳做法：[记录延迟和成功或失败对外部服务的所有调用](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log)例如数据库服务。 从 EF6 提供[专用日志记录 API](https://msdn.microsoft.com/data/dn469464) ，可以使其更轻松地执行日志记录，但在本教程的本部分中，你将了解如何使用实体框架的[截获功能](https://msdn.microsoft.com/data/dn469464)直接，同时包括这两者日志记录和用于模拟暂时性错误。

### <a name="create-a-logging-interface-and-class"></a>创建日志记录接口和类

A[最佳做法的日志记录](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log)都需要执行使用接口而不是硬编码调用 System.Diagnostics.Trace 或日志记录类。 这使可更轻松地更改你的日志记录机制更高版本，如果你在某个时候需要执行该操作。 因此在本部分中，你将创建日志记录接口和类来实现它。 / p > 

1. 在项目中创建一个文件夹并将其命名*日志记录*。
2. 在*日志记录*文件夹中，创建一个名为的类文件*ILogger.cs*，并将模板代码替换为以下代码：

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

    该接口提供三种跟踪级别，以指示日志的相对重要性和旨在提供有关外部服务调用，如数据库查询的滞后时间信息的一个。 日志记录方法具有重载，可以传递引发异常。 这是以便实现的接口，而不是依靠中每个日志记录方法调用在整个应用程序正在执行的类可靠地记录异常信息包括堆栈跟踪和内部异常。

    TraceApi 方法使你能够跟踪的 SQL 数据库等外部服务每次调用的延迟。
3. 在*日志记录*文件夹中，创建一个名为的类文件*Logger.cs*，并将模板代码替换为以下代码：

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

    实现使用 System.Diagnostics 执行跟踪。 这是可以轻松地生成和使用跟踪信息的.NET 的内置功能。 有许多"侦听器"你可以使用与 System.Diagnostics 跟踪日志写入文件，例如，或将它们写入到在 Azure 中的 blob 存储。 在看到某些选项，以及指向其他资源的详细信息，[排除 Azure 网站在 Visual Studio 中](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio)。 在 Visual Studio 中的本教程中你将仅查看日志**输出**窗口。

    在生产应用程序中，你可能需要考虑 System.Diagnostics，以外的跟踪包和 ILogger 接口可以相对轻松地切换到不同的跟踪机制，如果你决定要执行该操作。

### <a name="create-interceptor-classes"></a>创建侦听器类

接下来你将创建每次它要将查询发送到数据库来模拟暂时性错误，其中进行日志记录时，实体框架将调入的类。 这些侦听器类必须派生自`DbCommandInterceptor`类。 在它们编写查询是将要执行时，将自动调用的方法重写。 您可以在这些方法可以检查或日志查询正被发送到数据库，并可以更改查询发送到数据库之前，也可以返回某些内容到实体框架自己而无需甚至传递到数据库的查询。

1. 若要创建将日志发送到数据库的每个 SQL 查询拦截器类，创建一个名为的类文件*SchoolInterceptorLogging.cs*中*DAL*文件夹，并将模板代码与以下代码：

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

    对于成功的查询或命令，此代码将与滞后时间信息的信息日志。 对于异常，它将创建错误日志。
2. 若要创建在输入"引发"时，将生成 dummy 暂时性错误的侦听器类**搜索**框中，创建一个名为的类文件*SchoolInterceptorTransientErrors.cs*中*DAL*文件夹，并将替换为以下代码的模板代码：

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

    此代码仅重写`ReaderExecuting`方法，该调用可以返回多行数据的查询方法。 如果你想要检查连接复原对于其他类型的查询，也可以重写`NonQueryExecuting`和`ScalarExecuting`方法，为日志记录侦听器未。

    当你运行学生页面，并输入搜索字符串形式的"引发"时，此代码将创建错误号 20，类型是已知是通常暂时性 dummy SQL 数据库异常。 其他当前被识别为暂时性的错误号均 64、 233、 10053、 10054、 10060、 10928、 10929、 40197、 40501 和 40613，但它们是可能会有新版本的 SQL 数据库中的更改。

    该代码对 Entity Framework 而不是运行查询并通过后的查询结果返回的异常。 暂时性异常返回四次，之后代码恢复为将查询传递给数据库的一般过程。

    记录的所有内容，因为你将能够看到实体框架尝试之后才最后成功，四次执行查询和应用程序中的唯一区别是，它使用更长时间才能呈现包含查询结果页。

    实体框架将重试次数是可配置;该代码指定四次，因为它是 SQL 数据库执行策略的默认值。 如果更改执行策略，则必须还更改此处指定多少次暂时性错误生成的代码。 您还可以更改代码以生成更多的异常，使实体框架将引发`RetryLimitExceededException`异常。

    在搜索框中输入的值将采用`command.Parameters[0]`和`command.Parameters[1]`（一个用于名字和姓氏的一个）。 当找到"%throw%"值时，"引发"都替换为这些参数在""，以便将找到某些学生，并将其返回。

    这是只是测试连接复原根据不断变化的应用程序 UI 的某些输入一种可方便方式。 你也可以编写生成的所有查询或更新的暂时性错误的代码，如后面所述注释有关*DbInterception.Add*方法。
3. 在*Global.asax*，添加以下`using`语句：

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]
4. 添加到突出显示的行`Application_Start`方法：

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs?highlight=7-8)]

    这些代码行是什么因素会导致拦截器代码可在实体框架会将查询发送到数据库时运行。 请注意，因为创建单独的拦截器类进行暂时性错误模拟和日志记录，你可以单独启用和禁用它们。

    你可以将拦截器添加使用`DbInterception.Add`方法任何位置中你的代码; 它不一定要处于`Application_Start`方法。 另一个选项是将此代码放置在你之前创建配置的执行策略 DbConfiguration 类。

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs?highlight=6-7)]

    无论你将此代码放，请注意不要执行`DbInterception.Add`对于相同的侦听器超过一次，或你将获取其他拦截器实例。 例如，如果两次添加日志记录拦截器，你将看到为每个 SQL 查询的两个日志。

    拦截器执行顺序注册 (的顺序`DbInterception.Add`调用方法)。 顺序可能重要具体取决于您所做侦听器中。 例如，侦听器可能会更改获取中的 SQL 命令`CommandText`属性。 如果它未更改的 SQL 命令下, 一步侦听器将获取已更改的 SQL 命令，而不是原始的 SQL 命令。

    你可以通过在 UI 中输入不同的值导致暂时性错误的方式编写的暂时性错误模拟代码。 作为替代方法，你可以编写拦截器代码以始终生成暂时性异常的序列，而检查有特定的参数值。 仅当你想要生成暂时性错误，然后可以添加侦听器。 如果这样做，但是，在数据库初始化完成后不添加直到侦听器。 在开始生成暂时性错误之前，换而言之，执行如查询实体集之一上的至少一个数据库操作。 实体框架在数据库初始化期间所执行多个查询和它们不执行在事务中，因此在初始化期间的错误可能会导致才能进入不一致状态的上下文。

## <a name="test-logging-and-connection-resiliency"></a>测试日志记录和连接复原

1. 按 F5 在调试模式下运行应用程序，然后单击**学生**选项卡。
2. 查看 Visual Studio**输出**窗口以查看跟踪输出。 你可能需要向上滚动过去的某些 JavaScript 错误可用于访问由你记录程序写入的日志。

    请注意，你可以看到发送到数据库的实际 SQL 查询。 你看到的一些初始查询和实体框架执行的操作若要开始，检查该版本的数据库的命令和 （你将了解如何迁移在下一教程中） 的迁移历史记录表。 和查看分页，若要了解如何很多学生，一个查询，并最后看到获取学生数据的查询。

    ![正常的查询的日志记录](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. 在**学生**页上，作为搜索字符串中，输入"引发"，单击**搜索**。

    ![引发搜索字符串](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

    你会注意到，浏览器看上去已挂起了几秒钟时实体框架重试查询多次。 首次重试发生速度非常快，然后之前增加每个其他的重试之前等待。 此过程的调用每次重试之前再等待*指数退让*。

    当页显示时，显示学生都具有"的"在其名称中查看输出窗口中，并且您将看到相同的查询试图五次，第一次四次返回暂时性异常。 对于每个暂时性错误，你将看到你编写在生成中的暂时性错误时的日志`SchoolInterceptorTransientErrors`类 （"返回暂时性错误命令..."），你将会看到日志写入时`SchoolInterceptorLogging`获取的异常。

    ![日志记录输出显示重试](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

    由于你输入的搜索字符串，返回学生数据的查询进行参数化：

    [!code-sql[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.sql)]

    不日志记录的值的参数，但您可以这样做。 如果你想要查看的参数值，你可以编写日志记录代码，以获取从参数值`Parameters`属性`DbCommand`侦听器方法中获取的对象。

    请注意，不能重复此测试，除非停止应用程序并重新启动它。 如果你想要能够在应用程序的单次运行中测试连接复原多次，你可以编写代码以重置中的错误计数器`SchoolInterceptorTransientErrors`。
4. 可以查看的差异 （重试策略） 的执行策略进行，注释`SetExecutionStrategy`中一行*SchoolConfiguration.cs*、 学生页在调试模式下再次运行，然后再次搜索"Throw"。

    这一次调试器停止上第一次生成异常立即时它将尝试执行第一次的查询。

    ![Dummy 异常](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
5. 取消注释*SetExecutionStrategy*中一行*SchoolConfiguration.cs*。

## <a name="summary"></a>摘要

在本教程中，你已了解如何启用连接复原和记录的实体框架撰写并发送到数据库的 SQL 命令。 在下一教程中，你将部署到 Internet，使用 Code First 迁移将数据库部署应用程序。

请在如何喜欢本教程的方式，我们可以提高上，留下反馈。 你还可以请求新主题[教我编写代码](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code)。

在找不到其他实体框架资源的链接[ASP.NET 数据访问的推荐资源](../../../../whitepapers/aspnet-data-access-content-map.md)。

>[!div class="step-by-step"]
[上一页](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[下一页](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md)
