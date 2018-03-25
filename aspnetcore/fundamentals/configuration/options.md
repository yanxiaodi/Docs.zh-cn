---
title: "ASP.NET Core 中的选项模式"
author: guardrex
description: "了解如何使用选项模式来表示 ASP.NET Core 应用中的相关设置组。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 11/28/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/configuration/options
ms.openlocfilehash: abb3b92af07a7b3b199712fcfdc459ca283d0017
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="options-pattern-in-aspnet-core"></a>ASP.NET Core 中的选项模式

作者：[Luke Latham](https://github.com/guardrex)

选项模式使用选项类来表示相关设置的组。 当配置设置由功能隔离到单独的选项类时，应用遵循两个重要软件工程原则：

* [接口分离原则 (ISP)](http://deviq.com/interface-segregation-principle/)：依赖于配置设置的功能（类）仅依赖于其使用的配置设置。
* [关注点分离](http://deviq.com/separation-of-concerns/)：应用的不同部件的设置不彼此依赖或相互耦合。

[查看或下载示例代码](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)（[如何下载](xref:tutorials/index#how-to-download-a-sample)）跟随示例应用可更轻松地理解本文。

## <a name="basic-options-configuration"></a>基本选项配置

基本选项配置已作为示例 &num;1 在[示例应用](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)中进行了演示。

选项类必须为包含公共无参数构造函数的非抽象类。 以下类 `MyOptions` 具有两种属性：`Option1` 和 `Option2`。 设置默认值为可选，但以下示例中的类构造函数设置了 `Option1` 的默认值。 `Option2` 具有通过直接初始化属性设置的默认值 (Models/MyOptions.cs)：

[!code-csharp[Main](options/sample/Models/MyOptions.cs?name=snippet1)]

`MyOptions` 类已通过 [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) 添加到服务容器并绑定到配置：

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example1)]

以下页上的模型通过 [IOptions&lt;TOptions&gt;](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) 使用[构造函数依赖关系注入](xref:fundamentals/dependency-injection#what-is-dependency-injection)来访问设置 (Pages/Index.cshtml.cs)：

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example1)]

示例的 appsettings.json 文件指定 `option1` 和 `option2` 的值：

[!code-json[Main](options/sample/appsettings.json)]

运行应用时，页模型的 `OnGet` 方法返回显示选项类值的字符串：

```html
option1 = value1_from_json, option2 = -1
```

## <a name="configure-simple-options-with-a-delegate"></a>通过委托配置简单选项

通过委托配置简单选项已作为示例 &num;2 在[示例应用](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)中进行了演示。

使用委托设置选项值。 此示例应用使用 `MyOptionsWithDelegateConfig` 类 (Models/MyOptionsWithDelegateConfig.cs)：

[!code-csharp[Main](options/sample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

在以下代码中，已向服务容器添加第二个 `IConfigureOptions<TOptions>` 服务。 它通过 `MyOptionsWithDelegateConfig` 使用委托来配置绑定：

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example2)]

Index.cshtml.cs:

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example2)]

可添加多个配置提供程序。 配置提供程序在 NuGet 包中可用。 应用此提供程序，以便将其注册。

每次调用 [Configure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) 都将添加 `IConfigureOptions<TOptions>` 服务到服务容器。 在前面的示例中，`Option1` 和 `Option2` 的值同时在 appsettings.json 中指定，但 `Option1` 和 `Option2` 的值被配置的委托替代。

当启用多个配置服务时，指定的最后一个配置源优于其他源，由其设置配置值。 运行应用时，页模型的 `OnGet` 方法返回显示选项类值的字符串：

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a>子选项配置

子选项配置已作为示例 &num;3 在[示例应用](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)中进行了演示。

应用应创建适用于应用中特定功能组（类）的选项类。 需要配置值的部分应用应仅有权访问其使用的配置值。

将选项绑定到配置时，选项类型中的每个属性都将绑定到窗体 `property[:sub-property:]` 的配置键。 例如，`MyOptions.Option1` 属性将绑定到从 appsettings.json 中的 `option1` 属性读取的键 `Option1`。

在以下代码中，已向服务容器添加第三个 `IConfigureOptions<TOptions>` 服务。 它将 `MySubOptions` 绑定到 appsettings.json 文件的 `subsection` 部分：

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example3)]

`GetSection` 扩展方法需要 [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet 包。 如果应用使用 [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) 元包，将自动包含此包。

示例的 appsettings.json 文件定义具有 `suboption1` 和 `suboption2` 的键的 `subsection` 成员：

[!code-json[Main](options/sample/appsettings.json?highlight=4-7)]

`MySubOptions` 类定义属性 `SubOption1` 和 `SubOption2`，以保留子选项值 (Models/MySubOptions.cs)：

[!code-csharp[Main](options/sample/Models/MySubOptions.cs?name=snippet1)]

页模型的 `OnGet` 方法返回包含子选项值的字符串 (Pages/Index.cshtml.cs)：

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example3)]

运行应用时，`OnGet` 方法返回显示子选项类值的字符串：

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a>视图模型或通过直接视图注入提供的选项

视图模型或通过直接视图注入提供的选项已作为示例 &num;4 在[示例应用](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)中进行了演示。

可在视图模型中或通过将 `IOptions<TOptions>` 直接注入到视图 (Pages/Index.cshtml.cs) 来提供选项：

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example4)]

对于直接注入，通过 `@inject` 指令注入 `IOptions<MyOptions>`：

[!code-cshtml[Main](options/sample/Pages/Index.cshtml?range=1-10&highlight=5)]

运行应用时，选项值显示在已呈现的页中：

![选项值 Option1：value1_from_json 和 Option2：-1 从模型中加载并注入视图。](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a>通过 IOptionsSnapshot 重新加载配置数据

通过 `IOptionsSnapshot` 重新加载配置数据已作为示例 &num;5 在[示例应用](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)中进行了演示。

需要 ASP.NET Core 1.1 或更高版本。

[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) 支持包含最小处理开销的重新加载选项。 在 ASP.NET Core 1.1 中，`IOptionsSnapshot` 是 [IOptionsMonitor&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) 的快照，且在每次监视器基于数据源更改触发更改时自动更新。 在 ASP.NET Core 2.0 及更高版本中，在针对请求的生存期访问和缓存选项时，将针对每个请求计算一次选项。

以下示例演示如何在更改 appsettings.json (Pages/Index.cshtml.cs) 后创建新的 `IOptionsSnapshot`。 在更改文件和重新加载配置之前，针对服务器的多个请求返回 appsettings.json 文件提供的常数值。

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example5)]

下图显示从 appsettings.json 文件加载的初始 `option1` 和 `option2` 值：

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

将 appsettings.json 文件中的值更改为 `value1_from_json UPDATED` 和 `200`。 保存 appsettings.json 文件。 刷新浏览器，查看更新的选项值：

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a>包含 IConfigureNamedOptions 的命名选项支持

包含 [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) 的命名选项支持已作为示例 &num;6 在[示例应用](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)中进行了演示。

需要 ASP.NET Core 2.0 或更高版本。

命名选项支持允许应用在命名选项配置之间进行区分。 在示例应用中，命名选项通过 [ConfigureNamedOptions&lt;TOptions&gt;.Configure](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) 方法声明：

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example6)]

示例应用通过 [IOptionsSnapshot&lt;TOptions&gt;.Get](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (Pages/Index.cshtml.cs) 访问命名选项：

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example6)]

运行示例应用，将返回命名选项：

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

从配置中提供从 appsettings.json 文件中加载的 `named_options_1` 值。 通过以下内容提供 `named_options_2` 值：

* 针对 `Option1` 的 `ConfigureServices` 中的 `named_options_2` 委托。
* `MyOptions` 类提供的 `Option2` 的默认值。

通过 [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) 方法配置所有命名选项实例。 以下代码将针对包含公共值的所有命名配置实例配置 `Option1`。 将以下代码手动添加到 `Configure` 方法：

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

添加代码后运行示例应用将产生以下结果：

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> 在 ASP.NET Core 2.0 及更高版本中，所有选项都为命名实例。 现有 `IConfigureOption` 实例将被视为面向为 `string.Empty` 的 `Options.DefaultName` 实例。 `IConfigureNamedOptions` 还可实现 `IConfigureOptions`。 [IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1)（[引用源](https://github.com/aspnet/Options/blob/release/2.0.0/src/Microsoft.Extensions.Options/OptionsFactory.cs)）的默认实现具有适当使用每个选项的逻辑。 `null` 命名选项用于面向所有命名实例而不是某一特定命名实例（[ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) 和 [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) 使用此约定）。

## <a name="ipostconfigureoptions"></a>IPostConfigureOptions

需要 ASP.NET Core 2.0 或更高版本。

通过 [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1) 设置后期配置。 在发生所有 [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) 配置后运行后期配置：

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

[PostConfigure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) 可用于对命名选项进行后期配置：

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

使用 [PostConfigureAll&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) 对所有命名配置实例进行后期配置：

```csharp
services.PostConfigureAll<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="options-factory-monitoring-and-cache"></a>选项工厂、监视和缓存

[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) 用于在 `TOptions` 实例更改时进行通知。 `IOptionsMonitor` 支持可重载选项、更改通知和 `IPostConfigureOptions`。

[IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1)（ASP.NET Core 2.0 或更高版本）负责创建新选项实例。 它具有单个[创建](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create)方法。 默认实现采用所有已注册 `IConfigureOptions` 和 `IPostConfigureOptions` 并首先运行所有配置，然后才进行后期配置。 它区分 `IConfigureNamedOptions` 和 `IConfigureOptions` 且仅调用适当的接口。

[IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1)（ASP.NET Core 2.0 或更高版本）由 `IOptionsMonitor` 用于缓存 `TOptions` 实例。 `IOptionsMonitorCache` 可使监视器中的选项实例无效，以便重新计算值 ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove))。 还可通过 [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd) 手动引入值。 在应按需重新创建所有命名实例时使用[清除](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear)方法。

## <a name="accessing-options-during-startup"></a>在启动期间访问选项

`IOptions` 可用于 `Configure` 中，因为在 `Configure` 方法执行之前已生成服务。 如果在 `ConfigureServices` 中生成服务提供程序以访问选项，则它不应包含生成服务提供程序后所提供的任何选项配置。 因此，由于服务注册的顺序，可能存在不一致的选项状态。

由于选项通常从配置中加载，因此，可在 `Configure` 和 `ConfigureServices` 中的启动中使用配置。 有关在启动期间使用配置的示例，请参阅[应用程序启动](xref:fundamentals/startup)主题。

## <a name="see-also"></a>请参阅

* [配置](xref:fundamentals/configuration/index)
