---
uid: web-forms/overview/advanced/aspnet-web-forms-connection-resiliency-and-command-interception
title: "ASP.NET Web 窗体连接复原和命令截获 |Microsoft 文档"
author: Erikre
description: "本教程介绍如何修改示例应用程序以支持连接复原和命令截获。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2014
ms.topic: article
ms.assetid: 6d497001-fa80-4765-b4cc-181fe90b894e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/advanced/aspnet-web-forms-connection-resiliency-and-command-interception
msc.type: authoredcontent
ms.openlocfilehash: e3347657fb5c7bf8c7bb4e51a2e810a1edde826a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-web-forms-connection-resiliency-and-command-interception"></a>ASP.NET Web 窗体连接复原和命令截获
====================
通过[艾力克 Reitan](https://github.com/Erikre)

在本教程中，你将要修改 Wingtip Toys 示例应用程序以支持连接复原和命令截获。 通过启用连接复原，Wingtip Toys 示例应用程序将自动重试数据调用云环境的典型的暂时性错误发生时。 此外，通过实现命令截获，Wingtip Toys 示例应用程序将捕捉所有发送到若要登录，或更改这些数据库的 SQL 查询。

> [!NOTE] 
> 
> 此 Web 窗体教程基于 Tom Dykstra 的以下 MVC 教程：  
> [连接复原和命令截获与实体框架中的 ASP.NET MVC 应用程序](../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)


## <a name="what-youll-learn"></a>你将学习：

- 如何提供连接复原能力。
- 如何实现命令截获。

## <a name="prerequisites"></a>系统必备

在开始之前，请确保你已在计算机上安装以下软件：

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs)或[Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web)。 自动安装.NET Framework。
- Wingtip Toys 示例项目，以便你可以实现在本教程中 Wingtip Toys 项目中所述的功能。 以下链接提供下载详细信息：

    - [Getting Started with ASP.NET 4.5.1 Web 窗体的 Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&amp;clcid=0x409) (C#)
- 之前完成本教程，请考虑查看相关的教程系列， [Getting Started with ASP.NET 4.5 Web 窗体和 Visual Studio 2013](../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md)。 系列教程将帮助你熟悉**WingtipToys**项目和代码。

## <a name="connection-resiliency"></a>连接复原

当你考虑应用程序部署到 Windows Azure 时，可考虑使用一个选项部署到数据库**Windows** **Azure SQL 数据库**，云数据库服务。 连接到云数据库服务时与你的 web 服务器和数据库服务器直接连接在一起位于同一数据中心时，暂时性连接错误是通常更频繁。 即使在同一数据中心中承载的云 web 服务器和云数据库服务时，有更多的网络连接，它们将会出现问题，例如负载平衡器之间。

此外云服务通常由其他用户共享，这意味着其响应能力可能受到它们。 并且您对数据库访问权限可能受到限制。 限制意味着数据库服务会引发异常，当你尝试超出允许的更频繁地访问你*服务级别协议*(SLA)。

许多或大多数发生时你正在访问云服务的连接问题是暂时的也就是说，它们自身解决在短时间的时间。 因此当你尝试数据库操作，并获取一种是通常暂时性的错误，稍等片刻，该操作可能会成功后无法重试该操作。 如果通过自动重试，处理暂时性错误，可以为你的用户提供更好的体验使其中的大多数客户不可见。 连接复原功能 Entity Framework 6 中的自动执行过程的重试失败的 SQL 查询。

针对特定数据库服务，必须适当地配置连接复原功能：

1. 它必须知道哪些异常很可能是暂时的。 你想要重试错误导致通过暂时丢失网络连接，不由程序 bug，例如引起的错误。
2. 它必须等待适当的失败的操作的重试之间的时间量。 你可以等待，再进行批处理的重试之间不是用户正在等待响应的联机 web 页可以。
3. 它具有重试之前的适当次数。 你可能想要重试更多次，就像在联机应用程序中的批处理中。

可以配置为支持的实体框架提供程序的任何数据库环境手动这些设置。

你所要做，以启用连接复原的就是中派生自程序集创建一个类`DbConfiguration`类和在该类中设置 SQL 数据库执行策略，即实体框架中的重试策略另一个术语。

### <a name="implementing-connection-resiliency"></a>实现连接复原

1. 下载并打开[WingtipToys](https://go.microsoft.com/fwlink/?LinkID=389434&amp;clcid=0x409)示例 Visual Studio 中的 Web 窗体应用程序。
2. 在*逻辑*文件夹**WingtipToys**应用程序中，添加一个名为的类文件*WingtipToysConfiguration.cs*。
3. 用下面的代码替换现有代码：  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample1.cs)]

实体框架将自动运行它派生自的类中找到的代码`DbConfiguration`。 你可以使用`DbConfiguration`类来执行此在中否则所执行的操作的代码中的配置任务*Web.config*文件。 有关详细信息，请参阅[EntityFramework 基于代码的配置](https://msdn.microsoft.com/data/jj680699)。

1. 在*逻辑*文件夹中，打开*AddProducts.cs*文件。
2. 添加`using`语句`System.Data.Entity.Infrastructure`如所示以黄色突出显示：  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample2.cs?highlight=6)]
3. 添加`catch`阻止`AddProduct`方法，以便`RetryLimitExceededException`记录为用黄色突出显示：   

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample3.cs?highlight=14-15,17-22)]

通过添加`RetryLimitExceededException`异常，你可以提供更好地日志记录或向他们可以选择要再次尝试该过程的用户显示一条错误消息。 通过捕获`RetryLimitExceededException`异常，可能是暂时性的唯一错误将已尝试并失败几次。 返回的实际异常将包装在`RetryLimitExceededException`异常。 此外，你还将添加的一般 catch 块。 有关详细信息`RetryLimitExceededException`异常，请参阅[实体框架连接复原 / 重试逻辑](https://msdn.microsoft.com/data/dn456835)。

## <a name="command-interception"></a>命令截获

现在，你已启用的重试策略，如何进行你测试以验证它是否按预期工作？ 不会轻易地强制暂时性错误发生，尤其是当您在本地，运行和它将会特别困难，可以将实际的暂时性错误集成到自动的单元测试。 若要测试连接复原功能，需要一种方法将截获实体框架将发送到 SQL Server 的查询并将替换为通常暂时性异常类型的 SQL Server 响应。

此外可以使用查询截获，以实现云应用程序的最佳做法： 日志的延迟和成功或失败的所有调用到外部服务，例如数据库服务。

在本教程的本部分中，你将使用实体框架的[*截获功能*](https://msdn.microsoft.com/data/dn469464)对于日志记录和模拟暂时性错误。

### <a name="create-a-logging-interface-and-class"></a>创建日志记录接口和类

日志记录的最佳做法是使用可完成的[ `interface` ](https://msdn.microsoft.com/library/ms173156.aspx)而不是硬编码调用`System.Diagnostics.Trace`或日志记录类。 这使可更轻松地更改你的日志记录机制更高版本，如果你在某个时候需要执行该操作。 因此在本部分中，你将创建日志记录接口和类来实现。

根据上面的过程，您必须下载并打开**WingtipToys**示例 Visual Studio 中的应用程序。

1. 中创建一个文件夹**WingtipToys**项目并将其命名*日志记录*。
2. 在*日志记录*文件夹中，创建一个名为的类文件*ILogger.cs*和默认代码替换为以下代码：  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample4.cs)]

 该接口提供三种跟踪级别，以指示日志的相对重要性和旨在提供有关外部服务调用，如数据库查询的滞后时间信息的一个。 日志记录方法具有重载，可以传递引发异常。 这是以便实现的接口，而不是依靠中每个日志记录方法调用在整个应用程序正在执行的类可靠地记录异常信息包括堆栈跟踪和内部异常。  
  
 `TraceApi`方法使你能够跟踪的 SQL 数据库等外部服务每次调用的延迟。
3. 在*日志记录*文件夹中，创建一个名为的类文件*Logger.cs*和默认代码替换为以下代码：  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample5.cs)]

实现使用`System.Diagnostics`以执行跟踪。 这是可以轻松地生成和使用跟踪信息的.NET 的内置功能。 有许多&quot;侦听器&quot;可与`System.Diagnostics`日志写入文件，例如，或将它们写入 Windows Azure 中的 blob 存储的跟踪。 在看到某些选项，以及指向其他资源的详细信息， [Troubleshooting Visual Studio 中 Windows Azure Web Sites](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio)。 对于本教程，你将仅查看 Visual Studio 中的日志**输出**窗口。

在生产应用程序中，你可能需要考虑使用跟踪框架不`System.Diagnostics`，和`ILogger`接口可以相对轻松地切换到不同的跟踪机制，如果你决定要执行该操作。

### <a name="create-interceptor-classes"></a>创建侦听器类

接下来，你将创建每次它要将查询发送到数据库来模拟暂时性错误，其中进行日志记录时，实体框架将调入的类。 这些侦听器类必须派生自`DbCommandInterceptor`类。 在它们，编写查询是将要执行时，将自动调用的方法重写。 您可以在这些方法可以检查或日志查询正被发送到数据库，并可以更改查询发送到数据库之前，也可以返回某些内容到实体框架自己而无需甚至传递到数据库的查询。

1. 若要创建发送到数据库之前，将记录每个 SQL 查询拦截器类，创建一个名为的类文件*InterceptorLogging.cs*中*逻辑*文件夹并将默认代码与以下代码：  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample6.cs)]

 对于成功的查询或命令，此代码将与滞后时间信息的信息日志。 对于异常，它将创建错误日志。
2. 若要创建在输入时，将生成 dummy 暂时性错误的侦听器类&quot;引发&quot;中**名称**上名为的页的文本框中*AdminPage.aspx*，创建一个类名为文件*InterceptorTransientErrors.cs*中*逻辑*文件夹和替换默认代码替换为以下代码：  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample7.cs)]

    此代码仅重写`ReaderExecuting`方法，该调用可以返回多行数据的查询方法。 如果你想要检查连接复原对于其他类型的查询，也可以重写`NonQueryExecuting`和`ScalarExecuting`方法，为日志记录侦听器未。  
  
 更高版本，你将登录为"Admin"，并选择**管理员**顶部导航栏上的链接。 然后，在*AdminPage.aspx*页将添加名为产品&quot;引发&quot;。 该代码创建错误号 20，类型是已知是通常暂时性 dummy SQL 数据库的异常。 其他当前被识别为暂时性的错误号均 64、 233、 10053、 10054、 10060、 10928、 10929、 40197、 40501 和 40613，但它们是可能会有新版本的 SQL 数据库中的更改。 产品将重命名为"TransientErrorExample"，可在代码中遵循*InterceptorTransientErrors.cs*文件。  
  
 代码会返回该异常对 Entity Framework 而不是运行查询并通过返回结果。 返回暂时性异常*四个*次，然后将代码恢复到将查询传递给数据库的一般过程。

    记录的所有内容，因为你将能够看到实体框架尝试之后才最后成功，四次执行查询和应用程序中的唯一区别是，它使用更长时间才能呈现包含查询结果页。  
  
 实体框架将重试次数是可配置;该代码指定四次，因为它是 SQL 数据库执行策略的默认值。 如果更改执行策略，则必须还更改此处指定多少次暂时性错误生成的代码。 您还可以更改代码以生成更多的异常，使实体框架将引发`RetryLimitExceededException`异常。
3. 在*Global.asax*，添加以下 using 语句：  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample8.cs)]
4. 然后，将添加到突出显示的行`Application_Start`方法：  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample9.cs?highlight=17-20)]

这些代码行是什么因素会导致拦截器代码可在实体框架会将查询发送到数据库时运行。 请注意，因为创建单独的拦截器类进行暂时性错误模拟和日志记录，你可以单独启用和禁用它们。   
  
 你可以将拦截器添加使用`DbInterception.Add`方法任何位置中你的代码; 它不一定要处于`Application_Start`方法。 另一个选项，如果你未将拦截器添加在`Application_Start`方法，可以更新或添加名为的类*WingtipToysConfiguration.cs*和将上述代码中的构造函数末尾`WingtipToysbConfiguration`类。

无论你将此代码放，请注意不要执行`DbInterception.Add`对于相同的侦听器超过一次，或你将获取其他拦截器实例。 例如，如果两次添加日志记录拦截器，你将看到为每个 SQL 查询的两个日志。

拦截器执行顺序注册 (的顺序`DbInterception.Add`调用方法)。 顺序可能重要具体取决于您所做侦听器中。 例如，侦听器可能会更改获取中的 SQL 命令`CommandText`属性。 如果它未更改的 SQL 命令下, 一步侦听器将获取已更改的 SQL 命令，而不是原始的 SQL 命令。

你可以通过在 UI 中输入不同的值导致暂时性错误的方式编写的暂时性错误模拟代码。 作为替代方法，你可以编写拦截器代码以始终生成暂时性异常的序列，而检查有特定的参数值。 仅当你想要生成暂时性错误，然后可以添加侦听器。 如果这样做，但是，在数据库初始化完成后不添加直到侦听器。 在开始生成暂时性错误之前，换而言之，执行如查询实体集之一上的至少一个数据库操作。 实体框架在数据库初始化期间所执行多个查询和它们不执行在事务中，因此在初始化期间的错误可能会导致才能进入不一致状态的上下文。

## <a name="test-logging-and-connection-resiliency"></a>测试日志记录和连接复原

1. 在 Visual Studio 中，按**F5**运行该应用程序在调试模式，然后为"Admin"，然后使用"Pa$ $word"作为密码登录。
2. 选择**管理员**顶部导航栏中。
3. 输入新的产品名为"引发"与相应的说明、 价格和图像文件。
4. 按**添加产品**按钮。  
 你会注意到，浏览器看上去已挂起了几秒钟时实体框架重试查询多次。 首次重试发生速度非常快，然后在每个其他的重试之前的等待增大。 此过程的调用每次重试之前再等待*指数退让*。
5. 等待，直到页面不再 atttempting 加载。
6. 停止项目，并查看 Visual Studio**输出**窗口以查看跟踪输出。 你可以找到**输出**窗口，通过选择**调试** - &gt; **Windows**  - &gt; **输出**。 你可能必须滚动条越过数据编写你记录器的几个其他日志。  
  
 请注意，你可以看到发送到数据库的实际 SQL 查询。 你看到一些初始查询和命令的实体框架执行的操作若要开始，检查数据库版本和迁移历史记录表。   
    ![输出窗口](aspnet-web-forms-connection-resiliency-and-command-interception/_static/image1.png)   
 请注意，不能重复此测试，除非停止应用程序并重新启动它。 如果你想要能够在应用程序的单次运行中测试连接复原多次，你可以编写代码以重置中的错误计数器`InterceptorTransientErrors`。
7. 可以查看的差异 （重试策略） 的执行策略进行，注释`SetExecutionStrategy`中一行*WingtipToysConfiguration.cs*文件中*逻辑*文件夹中，运行**管理员**同样，在调试模式下页上，添加名为产品&quot;引发&quot;试。  
  
 这一次调试器停止上第一次生成异常立即时它将尝试执行第一次的查询。  
    ![调试-查看详细信息](aspnet-web-forms-connection-resiliency-and-command-interception/_static/image2.png)
8. 取消注释`SetExecutionStrategy`中一行*WingtipToysConfiguration.cs*文件。

## <a name="summary"></a>摘要

在本教程中，你已了解如何修改 Web 窗体示例应用程序，以支持连接复原和命令截获。

## <a name="next-steps"></a>后续步骤

查看连接复原和 ASP.NET Web 窗体中的命令截获后，查看 ASP.NET Web 窗体主题[ASP.NET 4.5 中的异步方法](../performance-and-caching/using-asynchronous-methods-in-aspnet-45.md)。 本主题将教您生成使用 Visual Studio 的异步 ASP.NET Web 窗体应用程序的基础知识。
