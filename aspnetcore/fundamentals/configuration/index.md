---
title: "ASP.NET Core 中的配置"
author: rick-anderson
description: "使用配置 API 通过多种方法配置 ASP.NET Core 应用。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/configuration/index
ms.openlocfilehash: 7c41621db835b452c9aad9463a9ffccdf0c06484
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
# <a name="configure-an-aspnet-core-app"></a>配置 ASP.NET Core 应用

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)、[Mark Michaelis](http://intellitect.com/author/mark-michaelis/)、[Steve Smith](https://ardalis.com/)、[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)

通过配置 API ，可基于名称/值对列表来配置 ASP.NET Core Web 应用。 在运行时从多个源读取配置。 可将名称/值对分组到多级层次结构。

配置提供程序适用于：

* 文件格式（INI、JSON 和 XML）。
* 命令行参数。
* 环境变量。
* 内存中的 .NET 对象。
* 未加密的[机密管理器](xref:security/app-secrets)存储。
* 加密的用户存储，如 [Azure Key Vault](xref:security/key-vault-configuration)。
* （已安装或已创建的）自定义提供程序。

每个配置值映射到一个字符串键。 可借助内置绑定支持，将设置反序列化为自定义 [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) 对象（一种具有属性的简单 .NET 类）。

选项模式使用选项类来表示相关设置的组。 有关使用选项模式的详细信息，请参阅[选项](xref:fundamentals/configuration/options)主题。

[查看或下载示例代码](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample)（[如何下载](xref:tutorials/index#how-to-download-a-sample)）

## <a name="json-configuration"></a>JSON 配置

以下控制台应用使用 JSON 配置提供程序：

[!code-csharp[](index/sample/ConfigJson/Program.cs)]

该应用将读取和显示下列配置设置：

[!code-json[](index/sample/ConfigJson/appsettings.json)]

配置就是名称/值对的分层列表，其中节点是由冒号分隔。 要检索某个值，请使用相应项的键访问 `Configuration` 索引器：

[!code-csharp[](index/sample/ConfigJson/Program.cs?range=21-22)]

要在 JSON 格式的配置源中使用数组，请在由冒号分隔的字符串中使用数组索引。 以下示例获取上述 `wizards` 数组中第一个项的名称：

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}");
// Output: Gandalf
```

写入内置[配置](/dotnet/api/microsoft.extensions.configuration)提供程序的名称/值对不是持久的。 但是，可以创建一个自定义提供程序来保存值。 请参阅[自定义配置提供程序](xref:fundamentals/configuration/index#custom-config-providers)。

前面的示例使用配置索引器来读取值。 要访问 `Startup` 外部的配置，请使用选项模式。 有关详细信息，请参阅[选项](xref:fundamentals/configuration/options)主题。

## <a name="xml-configuration"></a>XML 配置

若要在 XML 格式的配置源中使用数组，请向每个元素提供一个 `name` 索引。 使用该索引访问以下值：

```xml
<wizards>
  <wizard name="Gandalf">
    <age>1000</age>
  </wizard>
  <wizard name="Harry">
    <age>17</age>
  </wizard>
</wizards>
```

```csharp
Console.Write($"{Configuration["wizard:Harry:age"]}");
// Output: 17
```

## <a name="configuration-by-environment"></a>按环境配置

通常而言，配置设置因环境（如开发、测试和生产等）而异。 ASP.NET Core 2.x 应用中的 `CreateDefaultBuilder` 扩展方法（或直接在 ASP.NET Core 1.x 应用中使用 `AddJsonFile` 和 `AddEnvironmentVariables`）添加了用于读取 JSON 文件和系统配置源的配置提供程序：

* *appsettings.json*
* appsettings.\<EnvironmentName>.json
* 环境变量

ASP.NET Core 1.x 应用需要调用 `AddJsonFile` 和 [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_)。

有关参数的说明，请参阅 [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions)。 仅 ASP.NET Core 1.1 及更高版本支持 `reloadOnChange`。

按指定配置源的顺序读取它们。 在前面的代码中，最后才读取环境变量。 在环境中设置的任意配置值将替换先前两个提供程序中设置的配置值。

请考虑使用以下 appsettings.Staging.json 文件：

[!code-json[](index/sample/appsettings.Staging.json)]

环境设置为 `Staging` 时，以下 `Configure` 方法将读取 `MyConfig` 的值：

[!code-csharp[](index/sample/StartupConfig.cs?name=snippet&highlight=3,4)]


环境通常设置为 `Development`、`Staging` 或 `Production`。 有关详细信息，请参阅[使用多个环境](xref:fundamentals/environments)。

配置注意事项：

* 配置数据发生更改时，`IOptionsSnapshot` 可将其重载。 有关详细信息，请参阅 [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot)。
* 配置密钥不区分大小写。
* 请勿在配置提供程序代码或纯文本配置文件中存储密码或其他敏感数据。 不要在开发或测试环境中使用生产机密。 请在项目外部指定机密，避免将其意外提交到源代码存储库。 详细了解如何[使用多个环境](xref:fundamentals/environments)和[在开发期间管理应用机密的安全存储](xref:security/app-secrets)。
* 如果系统不支持在环境变量中使用冒号 (`:`)，请将冒号 (`:`) 替换为双下划线 (`__`)。

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a>内存中提供程序及绑定到 POCO 类

以下示例演示如何使用内存中提供程序及绑定到类：

[!code-csharp[](index/sample/InMemory/Program.cs)]

配置值以字符串的形式返回，但绑定使对象的构造成为可能。 通过绑定可检索 POCO 对象，甚至可检索整个对象图。

### <a name="getvalue"></a>GetValue

以下示例演示 [GetValue&lt;T&gt;](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_) 扩展方法：

[!code-csharp[](index/sample/InMemoryGetValue/Program.cs?highlight=31)]

ConfigurationBinder 的 `GetValue<T>` 方法允许指定默认值（在此示例中为 80）。 `GetValue<T>` 适用于简单方案，并不绑定到整个部分。 `GetValue<T>` 从转换为特定类型的 `GetSection(key).Value` 中获取标量值。

## <a name="bind-to-an-object-graph"></a>绑定至对象图

可递归绑定类中的每个对象。 请考虑使用以下 `AppSettings` 类：

[!code-csharp[](index/sample/ObjectGraph/AppSettings.cs)]

以下示例绑定到 `AppSettings` 类：

[!code-csharp[](index/sample/ObjectGraph/Program.cs?highlight=15-16)]

ASP.NET Core 1.1 及更高版本可使用 `Get<T>`，它适用于整个部分。 使用 `Get<T>` 可能比 `Bind` 方便。 以下代码演示了如何通过前述示例使用 `Get<T>`：

```csharp
var appConfig = config.GetSection("App").Get<AppSettings>();
```

使用以下 appsettings.json 文件：

[!code-json[](index/sample/ObjectGraph/appsettings.json)]

该程序显示 `Height 11`。

可使用以下代码对配置进行单元测试：

```csharp
[Fact]
public void CanBindObjectTree()
{
    var dict = new Dictionary<string, string>
        {
            {"App:Profile:Machine", "Rick"},
            {"App:Connection:Value", "connectionstring"},
            {"App:Window:Height", "11"},
            {"App:Window:Width", "11"}
        };
    var builder = new ConfigurationBuilder();
    builder.AddInMemoryCollection(dict);
    var config = builder.Build();

    var settings = new AppSettings();
    config.GetSection("App").Bind(settings);

    Assert.Equal("Rick", settings.Profile.Machine);
    Assert.Equal(11, settings.Window.Height);
    Assert.Equal(11, settings.Window.Width);
    Assert.Equal("connectionstring", settings.Connection.Value);
}
```

<a name="custom-config-providers"></a>

## <a name="create-an-entity-framework-custom-provider"></a>创建 Entity Framework 自定义提供程序

本部分将创建一个使用 EF 从数据库读取名称/值对的基本配置提供程序。

定义 `ConfigurationValue` 实体，用于在数据库中存储配置值：

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

添加 `ConfigurationContext` 以便存储和访问配置值：

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

创建实现 [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource) 的类：

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

通过从 [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider) 继承来创建自定义配置提供程序。 当数据库为空时，配置提供程序将对其进行初始化：

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

运行示例时将显示数据库中突出显示的值（“value_from_ef_1”和“value_from_ef_2”）。

可使用 `EFConfigSource` 扩展方法添加配置源：

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

下面的代码演示如何使用自定义 `EFConfigProvider`：

[!code-csharp[](index/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

请注意，示例在 JSON 提供程序之后添加自定义 `EFConfigProvider`，因此数据库中的任何设置都将替代 appsettings.json 文件中的设置。

使用以下 appsettings.json 文件：

[!code-json[](index/sample/CustomConfigurationProvider/appsettings.json)]

显示以下输出：

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a>CommandLine 配置提供程序

[CommandLine 配置提供程序](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider)在运行时接收用于配置的命令行参数键值对。

[查看或下载命令行配置示例](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/index/sample/CommandLine)

### <a name="setup-and-use-the-commandline-configuration-provider"></a>设置和使用 CommandLine 配置提供程序

# <a name="basic-configurationtabbasicconfiguration"></a>[基本配置](#tab/basicconfiguration)

要激活命令行配置，请在 [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder) 的实例上调用 `AddCommandLine` 扩展方法：

[!code-csharp[](index/sample_snapshot//CommandLine/Program.cs?highlight=18,21)]

运行代码将显示以下输出：

```console
MachineName: MairaPC
Left: 1980
```

在命令行上传递参数键值对将更改 `Profile:MachineName` 和 `App:MainWindow:Left` 的值：

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

控制台窗口将显示：

```console
MachineName: BartPC
Left: 1979
```

要使用命令行配置替代由其他配置提供程序提供的配置，请在 `ConfigurationBuilder` 上最后调用 `AddCommandLine`：

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

典型的 ASP.NET Core 2.x 应用使用静态简便方法 `CreateDefaultBuilder` 生成主机：

[!code-csharp[](index/sample_snapshot//Program.cs?highlight=12)]

`CreateDefaultBuilder` 从 appsettings.json、appsettings.{Environment}.json、[用户机密](xref:security/app-secrets)（在 `Development` 环境中）、环境变量和命令行参数中加载可选配置。 在最后调用 CommandLine 配置提供程序。 最后调用提供程序，则在运行时传递的命令行参数可以替代之前调用的其他配置提供程序设置的配置。

对于满足以下条件的 appsettings 文件：

* 已启用 `reloadOnChange`。
* 在命令行参数和 appsettings 文件中包含相同的设置。
* 应用启动后，包含匹配的命令行参数的 appsettings 文件发生更改。

如果上述所有条件均成立，则命令行参数将被覆盖。

ASP.NET Core 2.x 应用可使用 [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)，而不是 `CreateDefaultBuilder`。 使用 `WebHostBuilder` 时，请手动通过 [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) 设置配置。 有关详细信息，请参阅 ASP.NET Core 1.x 选项卡。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

创建 [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) 并调用 `AddCommandLine` 方法来使用 CommandLine 配置提供程序。 最后调用提供程序，则在运行时传递的命令行参数可以替代之前调用的其他配置提供程序设置的配置。 使用 `UseConfiguration` 方法向 [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) 应用配置：

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a>自变量

在命令行上传递的参数必须符合下表所示的两种格式之一：

| 参数格式                                                     | 示例        |
| ------------------------------------------------------------------- | :------------: |
| 单一参数：由等于号 (`=`) 分隔的键值对 | `key1=value`   |
| 两个参数的序列：由空格分隔的键值对    | `/key1 value1` |

**单一参数**

值必须跟在等于号 (`=`) 之后。 其值可以为 NULL（例如 `mykey=`）。

键可以具有前缀。

| 键前缀               | 示例         |
| ------------------------ | :-------------: |
| 无前缀                | `key1=value1`   |
| 单划线 (`-`)† | `-key2=value2`  |
| 双划线 (`--`)        | `--key3=value3` |
| 正斜杠 (`/`)      | `/key4=value4`  |

†[交换映射](#switch-mappings)中必须提供带有单划线前缀 (`-`) 的键，如下所示。

示例命令：

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

注意：如果向配置提供程序提供的[交换映射](#switch-mappings)中没有 `-key1`，则将引发 `FormatException`。

**两个参数的序列**

该值不可为 NULL，且必须跟在由空格分隔的键之后。

键必须具有前缀。

| 键前缀               | 示例         |
| ------------------------ | :-------------: |
| 单划线 (`-`)† | `-key1 value1`  |
| 双划线 (`--`)        | `--key2 value2` |
| 正斜杠 (`/`)      | `/key3 value3`  |

†[交换映射](#switch-mappings)中必须提供带有单划线前缀 (`-`) 的键，如下所示。

示例命令：

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

注意：如果向配置提供程序提供的[交换映射](#switch-mappings)中没有 `-key1`，则将引发 `FormatException`。

### <a name="duplicate-keys"></a>重复键

如果提供了重复的键，将使用最后一个键值对。

### <a name="switch-mappings"></a>交换映射

使用 `ConfigurationBuilder` 手动生成配置时，可将交换映射字典添加到 `AddCommandLine` 方法。 交换映射支持键名替换逻辑。

当使用交换映射字典时，会检查字典中是否有与命令行参数提供的键匹配的键。 如果在字典中找到命令行键，则传回字典值（替换键）以设置配置。 对任何具有单划线 (`-`) 前缀的命令行键而言，交换映射都是必需的。

交换映射字典键规则：

* 交换必须以单划线 (`-`) 或双划线 (`--`) 开头。
* 交换映射字典不得包含重复键。

在以下示例中，`GetSwitchMappings` 方法允许命令行参数使用单划线 (`-`) 键前缀，并避免使用前导子键前缀。

[!code-csharp[](index/sample/CommandLine/Program.cs?highlight=10-19,32)]

如果不提供命令行参数，则由提供给 `AddInMemoryCollection` 的字典来设置配置值。 使用以下命令运行应用：

```console
dotnet run
```

控制台窗口将显示：

```console
MachineName: RickPC
Left: 1980
```

使用以下命令传递配置设置：

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

控制台窗口将显示：

```console
MachineName: DahliaPC
Left: 1984
```

创建交换映射字典后，它将包含下表所示的数据：

| 键            | “值”                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

要使用字典演示键交换，请运行下列命令：

```console
dotnet run -MachineName=ChadPC -Left=1988
```

将交换命令行键。 控制台窗口将显示 `Profile:MachineName` 和 `App:MainWindow:Left` 的配置值：

```console
MachineName: ChadPC
Left: 1988
```

## <a name="webconfig-file"></a>web.config 文件

在 IIS 或 IIS Express 中托管应用时，需要 web.config 文件。 通过 web.config 中的设置，[ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) 可以启动应用并配置其他 IIS 设置和模块。 如果 *web.config* 文件不存在，并且项目文件中包含 `<Project Sdk="Microsoft.NET.Sdk.Web">`，则发布项目时会在发布的输出（“发布”文件夹）中创建一个 *web.config* 文件。 有关详细信息，请参阅 [使用 IIS 在 Windows 上托管 ASP.NET Core](xref:host-and-deploy/iis/index#webconfig-file)。

## <a name="accessing-configuration-during-startup"></a>在启动时访问配置

若要在启动时访问 `ConfigureServices` 或 `Configure` 中的配置，请参阅[应用程序启动](xref:fundamentals/startup)主题中的示例。

## <a name="additional-notes"></a>附加说明

* 调用 `ConfigureServices` 后才会设置依赖项注入 (DI)。
* 配置系统无法感知 DI。
* `IConfiguration` 具有两项专用化：
  * `IConfigurationRoot` 用于根节点。 可以触发重载。
  * `IConfigurationSection` 表示配置值的一节。 `GetSection` 和 `GetChildren` 方法返回 `IConfigurationSection`。
  * 重新加载配置或要访问每个提供程序时，请使用 [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot)。 这两种情况都不常见。

## <a name="additional-resources"></a>其他资源

* [选项](xref:fundamentals/configuration/options)
* [使用多个环境](xref:fundamentals/environments)
* [在开发期间安全存储应用密钥](xref:security/app-secrets)
* [ASP.NET Core 中的托管](xref:fundamentals/hosting)
* [依赖关系注入](xref:fundamentals/dependency-injection)
* [Azure Key Vault 配置提供程序](xref:security/key-vault-configuration)
