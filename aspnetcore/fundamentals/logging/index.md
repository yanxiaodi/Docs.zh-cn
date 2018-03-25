---
title: "ASP.NET Core 中的日志记录"
author: ardalis
description: "了解 ASP.NET Core 中的记录框架。 发现内置日志记录提供程序，并详细了解常见第三方提供程序。"
manager: wpickett
ms.author: tdykstra
ms.date: 12/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/logging/index
ms.openlocfilehash: c8152b94311acb672e9810828b634c744cb46eae
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-logging-in-aspnet-core"></a>ASP.NET Core 中的日志记录简介

作者：[Steve Smith](https://ardalis.com/) 和 [Tom Dykstra](https://github.com/tdykstra)

ASP.NET Core 支持适用于各种日志记录提供程序的日志记录 API。 通过内置提供程序，可向一个或多个目标发送日志，还可插入第三方记录框架。 本文介绍如何在代码中使用内置日志记录 API 和提供程序。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[查看或下载示例代码](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2)（[如何下载](xref:tutorials/index#how-to-download-a-sample)）

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[查看或下载示例代码](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample)（[如何下载](xref:tutorials/index#how-to-download-a-sample)）

---

## <a name="how-to-create-logs"></a>如何创建日志

要创建日志，请先从[依赖关系注入](xref:fundamentals/dependency-injection)容器获取 `ILogger` 对象：

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

然后在该记录器对象上调用日志记录方法：

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

此示例使用 `TodoController` 类创建日志作为类别。 [本文的稍后部分](#log-category)对这些类别进行了介绍。

ASP.NET Core 不提供异步记录器方法，因为日志记录的速度应非常快，使用异步的代价是不值得的。 如果发现自己的实际情况与上述不同，请考虑更改记录方式。 如果数据存储速度较慢，请先将日志消息写入快速存储，稍后再将其转移至低速存储。 例如，记录到由另一进程读取和暂留以减缓存储的消息队列。

## <a name="how-to-add-providers"></a>如何添加提供程序

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

日志记录提供程序通过 `ILogger` 对象获取所创建的消息，并显示或存储它们。 例如，控制台提供程序在控制台上显示消息，Azure App Service 提供程序可将消息存储在 Azure blob 存储中。

要使用提供程序，请在 Program.cs 中调用提供程序的 `Add<ProviderName>` 扩展方法：

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

通过默认项目模板可以使用 [CreateDefaultBuilder](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0#Microsoft_AspNetCore_WebHost_CreateDefaultBuilder_System_String___) 方法登录：

[!code-csharp[](index/sample2/Program.cs?name=snippet_TemplateCode&highlight=7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

日志记录提供程序通过 `ILogger` 对象获取所创建的消息，并显示或存储它们。 例如，控制台提供程序在控制台上显示消息，Azure App Service 提供程序可将消息存储在 Azure blob 存储中。

要使用提供程序，请安装其 NuGet 包，并在 `ILoggerFactory` 的实例上调用提供程序的扩展方法，如下方示例所示。

[!code-csharp[](index/sample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

ASP.NET Core [依赖关系注入](xref:fundamentals/dependency-injection) (DI) 将提供 `ILoggerFactory` 实例。 在 [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) 和 [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) 包中定义了 `AddConsole` 和 `AddDebug` 扩展方法。 每个扩展方法都调用 `ILoggerFactory.AddProvider` 方法，传入提供程序的一个实例。 

> [!NOTE]
> 本文的示例应用程序将在 `Startup` 类的 `Configure` 方法中添加日志记录提供程序。 要从先前执行的代码获取日志输出，请改为在 `Startup` 类构造函数中添加日志记录提供程序。 

---

本文稍后部分介绍了每个[内置日志记录提供程序](#built-in-logging-providers)，并提供了[第三方日志记录提供程序](#third-party-logging-providers)的链接。

## <a name="sample-logging-output"></a>日志记录输出示例

从命令行运行上一部分所示的示例代码时，将在控制台中看到日志。 以下是控制台输出示例：

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:5000/api/todo/0
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 42.9286ms
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 148.889ms 404
```
 
转到 `http://localhost:5000/api/todo/0`，触发上一部分所示的两个 `ILogger` 调用的执行，创建了以上日志。

在 Visual Studio 中运行示例应用程序时，“调试”窗口中将显示如下日志：

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404 
```

由上一部分所示的 `ILogger` 调用创建的日志以“TodoApi.Controllers.TodoController”开头的。 以“Microsoft”类别开头的日志来自 ASP.NET Core。 ASP.NET Core 本身和应用程序代码使用相同的日志记录 API 和日志记录提供程序。

本文余下部分将介绍有关日志记录的某些详细信息及选项。

## <a name="nuget-packages"></a>NuGet 包

`ILogger` 和 `ILoggerFactory` 接口位于 [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/) 中，其默认实现位于 [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/) 中。

## <a name="log-category"></a>日志类别

所创建的每个日志都包含一个类别。 在创建 `ILogger` 对象时指定类别。 类别可以是任意字符串，但约定使用写入日志的类的完全限定名称。 例如“TodoApi.Controllers.TodoController”。

可以将类别指定为字符串，或使用从类型派生类别的扩展方法。 要将类别指定为字符串，请在 `ILoggerFactory` 实例上调用 `CreateLogger`，如下所示。

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

如下方示例所示，在大多数情况下使用 `ILogger<T>` 更简单。

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

这相当于使用 `T` 的完全限定类型名称来调用 `CreateLogger`。

## <a name="log-level"></a>日志级别

每次写入日志时都需指定其 [LogLevel](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel)。 日志级别指示严重性或重要程度。 例如，如果方法正常结束则写入 `Information` 日志，如果方法返回 404 返回代码则写入 `Warning` 日志，如果捕获未知异常则写入 `Error` 日志。

在下列代码示例中，由方法的名称（如 `LogWarning`）指定日志级别。 第一个参数是[日志事件 ID](#log-event-id)。 第二个形参是[消息模板](#log-message-template)，包含由剩余方法形参提供的实参值的占位符。 本文稍后部分更详细地介绍了方法参数。

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

在方法名称中包含级别的日志方法是 [ILogger 的扩展方法](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loggerextensions)。 在后台，这些方法调用可接受 `LogLevel` 参数的 `Log` 方法。 可直接调用 `Log` 方法而不调用其中某个扩展方法，但语法相对复杂。 有关详细信息，请参阅 [ILogger 接口](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger)和[记录器扩展源代码](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs)。

ASP.NET Core 定义了以下[日志级别](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel)（按严重性从低到高排列）。

* 跟踪 = 0

  表示仅对于开发人员调试问题有价值的信息。 这些消息可能包含敏感应用程序数据，因此不得在生产环境中启用它们。 默认情况下禁用。 示例：`Credentials: {"User":"someuser", "Password":"P@ssword"}`

* 调试 = 1

  表示在开发和调试过程中短期有用的信息。 示例：`Entering method Configure with flag set to true.`。除非要排查问题，否则通常不会在生产中启用 `Debug` 级别日志，因为日志数量过多。

* 信息 = 2

  用于跟踪应用程序的常规流。 这些日志通常有长期价值。 示例：`Request received for path /api/todo`

* 警告 = 3

  表示应用程序流中的异常或意外事件。 可能包括不会中断应用程序运行但仍需调查的错误或其他条件。 `Warning` 日志级别常用于已处理的异常。 示例：`FileNotFoundException for file quotes.txt.`

* 错误 = 4

  表示无法处理的错误和异常。 这些消息指示的是当前活动或操作（如当前 HTTP 请求）中的失败，而不是应用程序范围的失败。 日志消息示例：`Cannot insert record due to duplicate key violation.`

* 严重 = 5

  需要立即关注的失败。 例如数据丢失、磁盘空间不足。

可以使用日志级别控制写入到特定存储介质或显示窗口的日志输出量。 例如在生产中，可将所有 `Information` 及以下级别的日志放在卷数据存储，将所有 `Warning` 及以上级别的日志放在值数据存储。 在开发期间，通常要将严重性为 `Warning` 或以上级别的日志发送到控制台。 需要进行故障排除时，可添加 `Debug` 级别。 本文稍后的[日志筛选](#log-filtering)部分介绍如何控制提供程序处理的日志级别。

ASP.NET Core 框架为框架事件写入 `Debug` 级别日志。 本文前面部分提供的日志示例排除了低于 `Information` 级别的日志，因此未显示 `Debug` 级别日志。 如果运行配置为显示控制台提供程序 `Debug` 及以上级别的日志的示例应用程序，则控制台日志示例如下所示。

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:62555/api/todo/0
dbug: Microsoft.AspNetCore.Routing.Tree.TreeRouter[1]
      Request successfully matched the route with name 'GetTodo' and template 'api/Todo/{id}'.
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Update (TodoApi)' with id '089d59b6-92ec-472d-b552-cc613dfd625d' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Delete (TodoApi)' with id 'f3476abe-4bd9-4ad3-9261-3ead09607366' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action TodoApi.Controllers.TodoController.GetById (TodoApi)
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action method TodoApi.Controllers.TodoController.GetById (TodoApi), returned result Microsoft.AspNetCore.Mvc.NotFoundResult.
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 0.8788ms
dbug: Microsoft.AspNetCore.Server.Kestrel[9]
      Connection id "0HL6L7NEFF2QD" completed keep alive response.
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 2.7286ms 404
```

## <a name="log-event-id"></a>日志事件 ID

每次写入日志时都可指定一个事件 ID。 该示例应用通过使用本地定义的 `LoggingEvents` 类来执行此操作：

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/sample//Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

事件 ID 是一个整数值，可用于关联多组已记录事件。 例如，向购物车添加项的日志可以是事件 ID 1000，完成购买的日志可以是事件 ID 1001。

在日志记录输出中，事件 ID 既可存储在字段里，也可包含在文本消息内，这由提供程序来决定。 调试提供程序不显示事件 ID，但控制台提供程序会将其显示在类别后的括号里：

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a>日志消息模板

每次写入日志消息都会提供一个消息模板。 消息模板可以是字符串，也可以包含放置参数值的命名占位符。 该模板不是格式字符串，应对占位符进行命名而不是编号。

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

占位符的顺序（而非其名称）决定了为其提供值的参数。 如果你有以下代码：

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

生成的日志消息如下所示：

```
Parameter values: parm1, parm2
```

记录框架以这种方式对消息进行格式化，以便日志记录提供程序能实现[语义日志记录（又称结构化日志记录）](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)。 由于传递到日志记录系统的是参数本身（而不仅仅是格式化的消息模板），因此除消息模板外，日志记录提供程序还可将参数值存储为字段。 如果将日志输出定向到 Azure 表存储，则记录器方法调用将如下所示：

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

每个 Azure 表实体都具有 `ID` 和 `RequestTime` 属性，简化了对日志数据的查询。 无需分析文本消息的超时，即可找到特定 `RequestTime` 范围内的全部日志。

## <a name="logging-exceptions"></a>日志记录异常

记录器方法有可传入异常的重载，如下方示例所示：

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

不同的提供程序处理异常信息的方式不同。 以下是上示代码的调试提供程序输出示例。

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a>日志筛选

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

可为特定或所有提供程序和类别指定最低日志级别。 最低级别以下的日志不会传递给该提供程序，因此不会显示或存储它们。 

要禁止显示所有日志，可将 `LogLevel.None` 指定为最低日志级别。 `LogLevel.None` 的整数值为 6，它大于 `LogLevel.Critical` (5)。

**在配置中创建筛选规则**

项目模板创建调用 `CreateDefaultBuilder` 的代码，以便为控制台和调试提供程序设置日志记录。 `CreateDefaultBuilder` 方法还使用如下所示的代码，设置日志记录以查找 `Logging` 部分的配置：

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=15)]

配置数据按提供程序和类别指定最低日志级别，如下方示例所示：

[!code-json[](index/sample2/appsettings.json)]

此 JSON 将创建六条筛选规则，一条用于调试提供程序，四条用于控制台提供程序，一条用于所有提供程序。 稍后你将了解在创建 `ILogger` 对象后，如何为每个提供程序从上述规则中选择其一。

**代码中的筛选规则**

可在代码中注册筛选规则，如下方示例所示：

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

第二个 `AddFilter` 使用类型名称来指定调试提供程序。 第一个 `AddFilter` 应用于全部提供程序，因为它未指定提供程序类型。

**如何筛选已应用的规则**

先前示例中显示的配置数据和 `AddFilter` 代码会创建下表所示的规则。 前六条由配置示例创建，后两条由代码示例创建。

| 数字 | 提供程序      | 类别的开头为...          | 最低日志级别 |
| :----: | ------------- | --------------------------------------- | ----------------- |
| 1      | 调试         | 全部类别                          | 信息       |
| 2      | 控制台       | Microsoft.AspNetCore.Mvc.Razor.Internal | 警告           |
| 3      | 控制台       | Microsoft.AspNetCore.Mvc.Razor.Razor    | 调试             |
| 4      | 控制台       | Microsoft.AspNetCore.Mvc.Razor          | Error             |
| 5      | 控制台       | 全部类别                          | 信息       |
| 6      | 全部提供程序 | 全部类别                          | 调试             |
| 7      | 全部提供程序 | 系统                                  | 调试             |
| 8      | 调试         | Microsoft                               | 跟踪             |

创建 `ILogger` 对象来写入日志时，`ILoggerFactory` 对象将从根据提供程序选择一条规则，将其应用于该记录器。 将按所选规则筛选该 `ILogger` 对象写入的所有消息。 从可用规则中为每个提供程序和类别对选择最具体的规则。

在为给定的类别创建 `ILogger` 时，以下算法将用于每个提供程序：

* 选择匹配提供程序或其别名的所有规则。 如果找不到，则选择具有空提供程序的所有规则。
* 根据上一步的结果，选择具有最长匹配类别前缀的规则。 如果找不到，则选择未指定类别的所有规则。
* 如果选择了多条规则，则采用最后一条。
* 如果未选择任何规则，则使用 `MinimumLevel`。
 
例如，假设你拥有上述规则列表，并为类别“Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine”创建了 `ILogger` 对象：

* 对于调试提供程序，规则 1、6 和 8 适用。 规则 8 最为具体，因此选择了它。
* 对于控制台提供程序，符合的有规则 3、规则 4、规则 5 和规则 6。 规则 3 最为具体。

在使用 `ILogger` 为类别“Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine”创建日志时，`Trace` 及以上级别的日志将发送到调试提供程序，`Debug` 及以上级别的日志将发送到控制台提供程序。

**提供程序别名**

虽然可以使用类型名称在配置中指定提供程序，但每个提供程序都定义了更短且更易于使用的别名。 对于内置提供程序，请使用以下别名：

- 控制台
- 调试
- EventLog
- AzureAppServices
- TraceSource
- EventSource

**默认最低级别**

仅当配置或代码中的规则对给定提供程序和类别都不适用时，最低级别设置才会生效。 下面的示例演示如何设置最低级别：

[!code-csharp[](index/sample2/Program.cs?name=snippet_MinLevel&highlight=3)]

如果没有明确设置最低级别，则默认值为 `Information`，它表示 `Trace` 和 `Debug` 日志将被忽略。

**筛选器函数**

可向筛选器函数写入代码以应用筛选规则。 对配置或代码没有向其分配规则的所有提供程序和类别调用筛选器函数。 函数中的代码有权访问提供程序类型、类别和日志级别，以决定是否记录某条消息。 例如:

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

通过某些日志记录提供程序，可根据日志级别和类别指定何时向存储介质写入日志、何时忽略日志。

`AddConsole` 和 `AddDebug` 扩展方法提供允许传入筛选条件的重载。 下列示例代码将使控制台提供程序忽略低于 `Warning` 级别的日志，而使调试提供程序忽略由框架创建的日志。

[!code-csharp[](index/sample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

`AddEventLog` 方法拥有接受 `EventLogSettings` 实例的重载，该实例的 `Filter` 属性中可能包含筛选函数。 TraceSource 提供程序不提供以上任何重载，因为其日志记录级别和其他参数都以它使用的 `SourceSwitch` 和 `TraceListener` 为依据。

可以使用 `WithFilter` 扩展方法为所有通过 `ILoggerFactory` 实例注册的提供程序设置筛选规则。 以下示例将（类别以“Microsoft”或“System”开头的）框架日志限制为警告，并在调试级别记录应用日志。

[!code-csharp[](index/sample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

要通过筛选来防止写入某个特定类别的所有日志，可将 `LogLevel.None` 指定为该类别的最低日志级别。 `LogLevel.None` 的整数值为 6，它大于 `LogLevel.Critical` (5)。

`WithFilter` 扩展方法由 [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet 包提供。 该方法返回一个新的 `ILoggerFactory` 实例，该实例将筛选传递给注册的所有记录器提供程序的日志消息。 它不会影响其他任何 `ILoggerFactory` 实例，包括原始 `ILoggerFactory` 实例。

---

## <a name="log-scopes"></a>日志作用域

可以将逻辑操作集划入范围，从而将相同的数据附加到在此集中创建的每个日志。 例如，可让处理事务时创建的每个日志都包含事务 ID。

范围是由 `ILogger.BeginScope<TState>` 方法返回的 `IDisposable` 类型，持续至释放为止。 要使用作用域，请在 `using` 块中包装记录器调用，如下所示：

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

下列代码为控制台提供程序启用作用域：

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

在 Program.cs 中：

[!code-csharp[](index/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> 要启用基于作用域的日志记录，必须先配置 `IncludeScopes` 控制台记录器选项。 ASP.NET Core 2.1 发布后，将支持使用 appsettings 配置文件来配置 `IncludeScopes`。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

在 Startup.cs 中：

[!code-csharp[](index/sample/Startup.cs?name=snippet_Scopes&highlight=6)]

---

每条日志消息都包含作用域内的信息：

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a>内置日志记录提供程序

ASP.NET Core 提供以下提供程序：

* [控制台](#console)
* [调试](#debug)
* [EventSource](#eventsource)
* [EventLog](#eventlog)
* [TraceSource](#tracesource)
* [Azure 应用服务](#appservice)

<a id="console"></a>
### <a name="the-console-provider"></a>控制台提供程序

[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) 提供程序包向控制台发送日志输出。 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
logging.AddConsole()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddConsole()
```

[AddConsole 重载](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.consoleloggerextensions)允许传入一个最低日志级别、一个筛选器函数，以及一个用于指示作用域是否受支持的布尔值。 另一个选项是传递 `IConfiguration` 对象，可通过它来指定支持的作用域及日志记录级别。 

在为生产环境选择控制台提供程序时，请考虑到它将对性能产生重大影响。

在 Visual Studio 中创建新项目时，`AddConsole` 方法如下所示：

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

此代码引用 appSettings.json 文件的 `Logging` 部分：

[!code-json[](index/sample//appsettings.json)]

所显示的设置将框架日志限制为警告，并允许在调试级别记录应用日志，如[日志筛选](#log-filtering)部分所述。 有关详细信息，请参阅[配置](xref:fundamentals/configuration/index)。

---

<a id="debug"></a>
### <a name="the-debug-provider"></a>调试提供程序

[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) 提供程序包使用 [System.Diagnostics.Debug](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.debug#System_Diagnostics_Debug) 类（`Debug.WriteLine` 方法调用）来写入日志输出。

在 Linux 中，此提供程序将日志写入 /var/log/message。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
logging.AddDebug()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddDebug()
```

[AddDebug 重载](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.debugloggerfactoryextensions)允许传入最低日志级别或筛选器函数。

---

<a id="eventsource"></a>
### <a name="the-eventsource-provider"></a>EventSource 提供程序

对于面向 ASP.NET Core 1.1.0 或更高版本的应用，[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) 提供程序包可实现事件跟踪。 在 Windows 中，它使用 [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803)。 提供程序可跨平台使用，但尚无支持 Linux 或 macOS 的事件集合和显示工具。 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
logging.AddEventSourceLogger()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventSourceLogger()
```

---

可使用 [PerfView 实用工具](https://www.microsoft.com/download/details.aspx?id=28567)收集和查看日志。 虽然其他工具也可以查看 ETW 日志，但在处理由 ASP.NET 发出的 ETW 事件时，使用 PerfView 能获得最佳体验。 

要将 PerfView 配置为收集此提供程序记录的事件，请向 Additional Providers 列表添加字符串 `*Microsoft-Extensions-Logging`。 （请勿遗漏字符串起始处的星号。）

![其他 Perfview 提供程序](index/_static/perfview-additional-providers.png)

要捕获 Nano Server 的事件，需要进行一些额外设置：

* 将 PowerShell 远程连接到 Nano Server：

  ```powershell
  Enter-PSSession [name]
  ```

* 创建 ETW 会话：

  ```powershell
  New-EtwTraceSession -Name "MyAppTrace" -LocalFilePath C:\trace.etl
  ```

* 按需求为 [CLR](https://docs.microsoft.com/dotnet/framework/performance/clr-etw-providers) 和 ASP.NET Core 等添加 ETW 提供程序。 ASP.NET Core 提供程序 GUID 为 `3ac73b97-af73-50e9-0822-5da4367920d0`。 

  ```powershell
  Add-EtwTraceProvider -Guid "{e13c0d23-ccbc-4e12-931b-d9cc2eee27e4}" -SessionName MyAppTrace
  Add-EtwTraceProvider -Guid "{3ac73b97-af73-50e9-0822-5da4367920d0}" -SessionName MyAppTrace
  ```

* 运行站点，并执行要跟踪其信息的操作。

* 在结束后停止跟踪会话：

  ```powershell
  Stop-EtwTraceSession -Name "MyAppTrace"
  ```

与在其他 Windows 版本中类似，可使用 PerfView 分析所生成的 C:\trace.etl 文件。

<a id="eventlog"></a>
### <a name="the-windows-eventlog-provider"></a>Windows EventLog 提供程序

[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) 提供程序包向 Windows 事件日志发送日志输出。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
logging.AddEventLog()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventLog()
```

[AddEventLog 重载](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.eventloggerfactoryextensions)允许传入 `EventLogSettings` 或最低日志级别。

---

<a id="tracesource"></a>
### <a name="the-tracesource-provider"></a>TraceSource 提供程序

[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) 提供程序包使用 [System.Diagnostics.TraceSource](https://docs.microsoft.com/dotnet/api/system.diagnostics.tracesource) 库和提供程序。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
logging.AddTraceSource(sourceSwitchName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddTraceSource(sourceSwitchName);
```

---

[AddTraceSource 重载](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.tracesourcefactoryextensions) 允许传入资源开关和跟踪侦听器。

要使用此提供程序，应用程序必须在 .NET Framework（而非 .NET Core）上运行。 使用提供程序，可以将消息路由到多个[侦听器](https://docs.microsoft.com/dotnet/framework/debug-trace-profile/trace-listeners)，例如示例应用程序中使用的 [TextWriterTraceListener](https://docs.microsoft.com/dotnet/api/system.diagnostics.textwritertracelistenerr)。

以下示例配置了 `TraceSource` 提供程序，用于向控制台窗口记录 `Warning` 和更高级别的消息。

[!code-csharp[](index/sample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

<a id="appservice"></a>
### <a name="the-azure-app-service-provider"></a>Azure App Service 提供程序

[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) 提供程序包将日志写入 Azure App Service 应用的文件系统，以及 Azure 存储帐户中的 [blob 存储](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage)。 该提供程序仅适用于面向 ASP.NET Core 1.1.0 或更高版本的应用。 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

如果面向 .NET Core，无需安装提供程序包或显式调用 `AddAzureWebAppDiagnostics`。 将应用部署到 Azure App Service 时，提供程序对应用自动可用。

如果面向 .NET Framework，需要向项目添加提供程序包并调用 `AddAzureWebAppDiagnostics`：

```csharp
logging.AddAzureWebAppDiagnostics();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

`AddAzureWebAppDiagnostics` 重载允许传入 [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs)，可用它替代默认设置，例如日志记录输出模板、blob 名称和文件大小限制等。 （输出模板是应用于所有日志的消息模板，其优先级高于调用 `ILogger` 方法时提供的模板。）

---

在部署 App Service 应用时，应用程序将遵循 Azure 门户中 App Service 页下[诊断日志](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag)部分的设置。 这些设置将在更改后立即生效，无需重启应用或重新向其部署代码。 

![Azure 日志记录设置](index/_static/azure-logging-settings.png)

日志文件的默认位置是 D:\\home\\LogFiles\\Application 文件夹，默认文件名为 diagnostics-yyyymmdd.txt。 默认文件大小上限为 10 MB，默认最大保留文件数为 2。 默认 blob 名为 {app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt。 有关默认行为的详细信息，请参阅 [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs)。

该提供程序仅当项目在 Azure 环境中运行时有效。 它在本地运行时无效。也就是说，不会写入本地文件或 blob 的本地开发存储。

## <a name="third-party-logging-providers"></a>第三方日志记录提供程序

以下第三方日志记录框架适用于 ASP.NET Core：

* [elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging) - Elmah.Io 服务的提供程序

* [JSNLog](http://jsnlog.com) - 在服务器端日志中记录 JavaScript 异常和其他客户端事件。

* [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging) - Loggr 服务的提供程序

* [NLog](https://github.com/NLog/NLog.Extensions.Logging) - NLog 库的提供程序

* [Serilog](https://github.com/serilog/serilog-extensions-logging) - Serilog 库的提供程序

某些第三方框架可以执行[语义日志记录（又称结构化日志记录）](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)。

使用第三方框架与使用内置提供程序类似：向项目添加 NuGet 包并在 `ILoggerFactory` 上调用扩展方法即可。 有关详细信息，请参阅各框架的相关文档。

还可创建自定义提供程序，以支持其他日志记录框架或满足自身日志记录需求。

## <a name="azure-log-streaming"></a>Azure 日志流式处理

通过 Azure 日志流式处理，可从以下位置实时查看日志活动： 

* 应用程序服务器 
* Web 服务器
* 请求跟踪失败 

要配置 Azure 日志流式处理，请执行以下操作： 

* 从应用程序的门户页导航到“诊断日志”页
* 将“应用程序日志记录 (Filesystem)”设置为启用。 

![Azure 门户诊断日志页](index/_static/azure-diagnostic-logs.png)

导航到“日志流式处理”页，查看应用程序消息。 它们由应用程序通过 `ILogger` 接口记录。 

![Azure 门户应用程序日志流式处理](index/_static/azure-log-streaming.png)


## <a name="see-also"></a>请参阅

[使用 LoggerMessage 的高性能日志记录](xref:fundamentals/logging/loggermessage)
