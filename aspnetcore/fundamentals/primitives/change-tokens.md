---
title: "使用 ASP.NET Core 中的更改令牌检测更改"
author: guardrex
description: "了解如何使用更改令牌来跟踪更改。"
manager: wpickett
ms.author: riande
ms.date: 11/10/2017
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/primitives/change-tokens
ms.openlocfilehash: 94bf356fcbfab3930804485c1b65e4a0f4c52b8e
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/31/2018
---
# <a name="detect-changes-with-change-tokens-in-aspnet-core"></a>使用 ASP.NET Core 中的更改令牌检测更改

作者：[Luke Latham](https://github.com/guardrex)

更改令牌是用于跟踪更改的通用、低级别构建基块。

[查看或下载示例代码](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/primitives/change-tokens/sample/)（[如何下载](xref:tutorials/index#how-to-download-a-sample)）

## <a name="ichangetoken-interface"></a>IChangeToken 接口

[IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken) 传播已发生更改的通知。 `IChangeToken` 位于 [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) 命名空间中。 对于不使用 [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) 元包的应用，将在项目文件中引用 [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet 包。

`IChangeToken` 具有以下两个属性：

* [ActiveChangedCallbacks](/dotnet/api/microsoft.extensions.primitives.ichangetoken.activechangecallbacks)，指示令牌是否主动引发回调。 如果将 `ActiveChangedCallbacks` 设置为 `false`，则不会调用回调，并且应用必须轮询 `HasChanged` 获取更改。 如果未发生任何更改，或者丢弃或禁用基础更改侦听器，还可能永远不会取消令牌。
* [HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged)，获取一个指示是否发生更改的值。

此接口具有一种方法，即 [RegisterChangeCallback(Action&lt;Object&gt;, Object)](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback)，用于注册在令牌更改时调用的回调。 调用回调之前，必须设置 `HasChanged`。

## <a name="changetoken-class"></a>ChangeToken 类

`ChangeToken` 是静态类，用于传播已发生更改的通知。 `ChangeToken` 位于 [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) 命名空间中。 对于不使用 [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) 元包的应用，将在项目文件中引用 [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet 包。

`ChangeToken` [OnChange(Func&lt;IChangeToken&gt;, Action)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action_) 方法注册令牌更改时要调用的 `Action`：
* `Func<IChangeToken>` 生成令牌。
* 令牌更改时，调用 `Action`。

`ChangeToken` 具有 [OnChange&lt;TState&gt;(Func&lt;IChangeToken&gt;, Action&lt;TState&gt;, TState)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange__1_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action___0____0_) 重载，它接受传递到令牌使用者 `Action` 的附加 `TState` 参数。

`OnChange` 返回 [IDisposable](/dotnet/api/system.idisposable)。 调用 [Dispose](/dotnet/api/system.idisposable.dispose) 将使令牌停止侦听更多更改并释放令牌的资源。

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a>ASP.NET Core 中更改令牌的使用示例

更改令牌主要用于 ASP.NET Core 监视对象更改：

* 为了监视文件更改，[IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) 的 [Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) 方法将为要监视的指定文件或文件夹创建 `IChangeToken`。
* 可以将 `IChangeToken` 令牌添加到缓存条目，以在更改时触发缓存逐出。
* 对于 `TOptions` 更改，[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) 的默认 [OptionsMonitor](/dotnet/api/microsoft.extensions.options.optionsmonitor-1) 实现具有重载，该重载接受一个或多个 [IOptionsChangeTokenSource](/dotnet/api/microsoft.extensions.options.ioptionschangetokensource-1) 实例。 每个实例返回 `IChangeToken`，以注册用于跟踪选项更改的更改通知回调。

## <a name="monitoring-for-configuration-changes"></a>监视配置更改

默认情况下，ASP.NET Core 模板使用 [JSON 配置文件](xref:fundamentals/configuration/index#json-configuration)（appsettings.json、appsettings.Development.json 和 appsettings.Production.json）来加载应用配置设置。

使用接受 `reloadOnChange` 参数（ASP.NET Core 1.1 以及更高版本）的 [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder) 上的 [AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions.addjsonfile?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_JsonConfigurationExtensions_AddJsonFile_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_System_Boolean_System_Boolean_) 扩展方法配置这些文件。 `reloadOnChange` 指示文件更改时是否应该重载配置。 请参阅 [WebHost](/dotnet/api/microsoft.aspnetcore.webhost) 便捷方法 [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder)（[引用源](https://github.com/aspnet/MetaPackages/blob/rel/2.0.3/src/Microsoft.AspNetCore/WebHost.cs#L152-L193)）中的此设置：

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, reloadOnChange: true);
```

基于文件的配置由 [FileConfigurationSource](/dotnet/api/microsoft.extensions.configuration.fileconfigurationsource) 表示。 `FileConfigurationSource` 使用 [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider)（[引用源](https://github.com/aspnet/FileSystem/blob/patch/2.0.1/src/Microsoft.Extensions.FileProviders.Abstractions/IFileProvider.cs)）来监视文件。

默认情况下，由使用 [FileSystemWatcher](/dotnet/api/system.io.filesystemwatcher) 来监视配置文件更改的 [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider)（[引用源](https://github.com/aspnet/Configuration/blob/patch/2.0.1/src/Microsoft.Extensions.Configuration.FileExtensions/FileConfigurationSource.cs#L82)）提供 `IFileMonitor`。

示例应用演示监视配置更改的两个实现。 如果 appsettings.json 文件更改或者文件的 Environment 版本更改，则每个实现执行自定义代码。 示例应用向控制台写入消息。

配置文件的 `FileSystemWatcher` 可以触发多个令牌回调，以用于单个配置文件更改。 该示例的实现通过检查配置文件上的文件哈希来防范此问题。 检查文件哈希可确保在运行自定义代码之前，至少已更改了其中一个配置文件。 本示例使用 SHA1 文件哈希 (Utilities/Utilities.cs)：

   [!code-csharp[Main](change-tokens/sample/Utilities/Utilities.cs?name=snippet1)]

   使用指数回退实现重试。 进行重试是因为文件锁定可能暂时阻止对其中一个文件计算新的哈希。

### <a name="simple-startup-change-token"></a>简单启动更改令牌

将用于更改通知的令牌使用者 `Action` 回调注册到配置重载令牌 (Startup.cs)：

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet2)]

`config.GetReloadToken()` 提供令牌。 回调是 `InvokeChanged` 方法：

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet3)]

回调的 `state` 用于在 `IHostingEnvironment` 中传递。 这有助于确定要监视的正确 appsettings 配置 JSON 文件，即 appsettings.&lt;Environment&gt;.json。 只更改了一次配置文件时，文件哈希用于防止 `WriteConsole` 语句由于多个令牌回调而多次运行。

只要应用正在运行，该系统就会运行，并且用户不能禁用。

### <a name="monitoring-configuration-changes-as-a-service"></a>将配置更改作为服务进行监视

示例实现：

* 基本启动令牌监视。
* 作为服务监视。
* 启用和禁用监视的机制。

示例建立 `IConfigurationMonitor` 接口 (Extensions/ConfigurationMonitor.cs)：

[!code-csharp[Main](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet1)]

已实现的类的构造函数 `ConfigurationMonitor` 注册用于更改通知的回调：

[!code-csharp[Main](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet2)]

`config.GetReloadToken()` 提供令牌。 `InvokeChanged` 是回调方法。 此实例中的 `state` 是描述监视状态的字符串。 使用了以下两个属性：

* `MonitoringEnabled`，指示回调是否应该运行其自定义代码。
* `CurrentState`，描述在 UI 中使用的当前监视状态。

`InvokeChanged` 方法类似于前面的方法，不同之处在于：

* 不运行其代码，除非 `MonitoringEnabled` 为 `true`。
* 将 `CurrentState` 属性字符串设置为记录代码运行时间的描述性消息。
* 注意其 `WriteConsole` 输出中的当前 `state`。

[!code-csharp[Main](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet3)]

将实例 `ConfigurationMonitor` 作为服务注册在 Startup.cs 的 `ConfigureServices` 中：

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet1)]

索引页提供对配置监视的用户控制。 将 `IConfigurationMonitor` 的实例注入到 `IndexModel` 中：

[!code-csharp[Main](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet1)]

按钮启用和禁用监视：

[!code-cshtml[Main](change-tokens/sample/Pages/Index.cshtml?range=35)]

[!code-csharp[Main](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet2)]

触发 `OnPostStartMonitoring` 时，会启用监视并清除当前状态。 触发 `OnPostStopMonitoring` 时，会禁用监视并设置状态以反映未进行监视。

## <a name="monitoring-cached-file-changes"></a>监视缓存文件更改

可以使用 [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) 将文件内容缓存在内存中。 [在内存中缓存](xref:performance/caching/memory)主题中介绍了在内存中缓存。 无需采取其他步骤（如下面所述的实现），如果源数据更改，将从缓存返回陈旧（过时）数据。

当续订[可调过期](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration)时段造成陈旧缓存数据时，不考虑所缓存源文件的状态。 数据的每个请求续订可调过期时段，但不会将文件重载到缓存中。 任何使用文件缓存内容的应用功能都可能会收到陈旧的内容。

在文件缓存方案中使用更改令牌可防止缓存中出现陈旧的文件内容。 示例应用演示方法的实现。

示例使用 `GetFileContent` 来完成以下操作：

* 返回文件内容。
* 实现具有指数回退的重试算法，以涵盖文件锁暂时阻止读取文件的情况。

Utilities/Utilities.cs：

[!code-csharp[Main](change-tokens/sample/Utilities/Utilities.cs?name=snippet2)]

创建 `FileService` 以处理缓存文件查找。 服务的 `GetFileContent` 方法调用尝试从内存中缓存获取文件内容并将其返回到调用方 (Services/FileService.cs)。

如果使用缓存键未找到缓存的内容，则将执行以下操作：

1. 使用 `GetFileContent` 获取文件内容。
1. 使用 [IFileProviders.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) 从文件提供程序获取更改令牌。 修改该文件时，会触发令牌的回调。
1. [可调过期](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration)时段将缓存文件内容。 如果缓存文件时发生了更改，则将更改令牌与 [MemoryCacheEntryExtensions.AddExpirationToken](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.addexpirationtoken) 连接在一起，以逐出缓存条目。

[!code-csharp[Main](change-tokens/sample/Services/FileService.cs?name=snippet1)]

将 `FileService` 和内存缓存服务 (Startup.cs) 一起注册在服务容器中：

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet4)]

页面模型使用服务 (Pages/Index.cshtml.cs) 加载文件内容：

[!code-csharp[Main](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a>CompositeChangeToken 类

若要在单个对象中表示一个或多个 `IChangeToken` 实例，请使用 [CompositeChangeToken](/dotnet/api/microsoft.extensions.primitives.compositechangetoken) 类（[引用源](https://github.com/aspnet/Common/blob/patch/2.0.1/src/Microsoft.Extensions.Primitives/CompositeChangeToken.cs)）。

```csharp
var firstCancellationTokenSource = new CancellationTokenSource();
var secondCancellationTokenSource = new CancellationTokenSource();

var firstCancellationToken = firstCancellationTokenSource.Token;
var secondCancellationToken = secondCancellationTokenSource.Token;

var firstCancellationChangeToken = new CancellationChangeToken(firstCancellationToken);
var secondCancellationChangeToken = new CancellationChangeToken(secondCancellationToken);

var compositeChangeToken = 
    new CompositeChangeToken(
        new List<IChangeToken> 
        { 
            firstCancellationChangeToken, 
            secondCancellationChangeToken
        });
```

如果任何表示的令牌 `HasChanged` 为 `true`，则复合令牌上的 `HasChanged` 报告 `true`。 如果任何表示的令牌 `ActiveChangeCallbacks` 为 `true`，则复合令牌上的 `ActiveChangeCallbacks` 报告 `true`。 如果发生多个并发更改事件，则调用一次复合更改回调。

## <a name="see-also"></a>请参阅

* [内存中缓存](xref:performance/caching/memory)
* [使用分布式缓存](xref:performance/caching/distributed)
* [使用更改令牌检测更改](xref:fundamentals/primitives/change-tokens)
* [响应缓存](xref:performance/caching/response)
* [响应缓存中间件](xref:performance/caching/middleware)
* [缓存标记帮助程序](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [分布式缓存标记帮助程序](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
