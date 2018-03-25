---
title: "添加 ASP.NET 核心中使用特定于平台的配置的应用程序功能"
author: guardrex
description: "了解如何从使用 IHostingStartup 实现的外部程序集添加到 ASP.NET 核心应用程序的功能。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 12/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/platform-specific-configuration
ms.openlocfilehash: c36b8acd6f7fcb4e4d11e43013ccaf5ca6d1b0ab
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="add-app-features-using-a-platform-specific-configuration-in-aspnet-core"></a>添加 ASP.NET 核心中使用特定于平台的配置的应用程序功能

作者：[Luke Latham](https://github.com/guardrex)

[IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup)实现允许将从外部应用程序的外部程序集的功能添加到应用程序在启动`Startup`类。 例如，可以使用外部工具库`IHostingStartup`实现，用于提供其他配置提供程序或服务添加到应用。 `IHostingStartup` *是可用在 ASP.NET 核心 2.0 及更高版本。*

[查看或下载示例代码](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/platform-specific-configuration/sample/)（[如何下载](xref:tutorials/index#how-to-download-a-sample)）

## <a name="discover-loaded-hosting-startup-assemblies"></a>发现加载托管的启动程序集

若要发现由应用程序或库承载启动程序集加载，启用日志记录和检查应用程序日志。 发生所记录的加载程序集时的错误。 加载托管的启动程序集将记录的调试级别，并将记录所有错误。

示例应用程序读取[HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey)到`string`数组并在应用程序的索引页中显示结果：

[!code-csharp[](platform-specific-configuration/sample/HostingStartupSample/Pages/Index.cshtml.cs?name=snippet1&highlight=14-16)]

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a>禁用托管启动程序集的自动的加载

有两种方法来禁用自动加载的托管启动程序集：

* 设置[防止承载启动](xref:fundamentals/hosting#prevent-hosting-startup)承载配置设置。
* 设置`ASPNETCORE_preventHostingStartup`环境变量。

当主机设置或环境变量设置为`true`或`1`、 宿主启动的程序集并不自动加载。 如果两者都设置，主机设置将控制行为。

禁用托管启动程序集使用的主机设置或环境变量全局禁用它们，并可能会禁用应用程序的多个功能。 它不是当前可以有选择性地禁用添加由库，除非库提供其自己的配置选项的宿主启动程序集。 未来版本将提供的功能有选择性地禁用宿主启动程序集 (请参阅[GitHub 发出 aspnet/宿主 # 1243年](https://github.com/aspnet/Hosting/pull/1243))。

## <a name="implement-ihostingstartup-features"></a>实现 IHostingStartup 功能

### <a name="create-the-assembly"></a>创建程序集

`IHostingStartup`功能部署为基于没有入口点的控制台应用程序集。 程序集引用[Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/)包：

[!code-xml[](platform-specific-configuration/snapshot_sample/StartupFeature.csproj)]

A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute)属性将类标识的实现为`IHostingStartup`加载和执行生成时[IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost)。 在下面的示例中，命名空间是`StartupFeature`，和类是`StartupFeatureHostingStartup`:

[!code-csharp[](platform-specific-configuration/snapshot_sample/StartupFeature.cs?name=snippet1)]

类实现`IHostingStartup`。 类的[配置](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure)方法使用[IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)将功能添加到应用程序：

[!code-csharp[](platform-specific-configuration/snapshot_sample/StartupFeature.cs?name=snippet2&highlight=3,5)]

生成时`IHostingStartup`项目依赖项文件 (*\*。 deps.json*) 设置`runtime`到程序集的位置*bin*文件夹：

[!code-json[](platform-specific-configuration/snapshot_sample/StartupFeature1.deps.json?range=2-13&highlight=8)]

显示仅属于文件。 在示例中的程序集名称是`StartupFeature`。

### <a name="update-the-dependencies-file"></a>更新依赖项文件

中指定的运行时位置 *\*。 deps.json*文件。 为活动状态的功能，`runtime`元素必须指定该功能的运行时程序集的位置。 前缀`runtime`位置与`lib/netcoreapp2.0/`:

[!code-json[](platform-specific-configuration/snapshot_sample/StartupFeature2.deps.json?range=2-13&highlight=8)]

在示例应用中，修改 *\*。 deps.json*文件执行的[PowerShell](/powershell/scripting/powershell-scripting)脚本。 PowerShell 脚本自动触发项目文件中生成目标。

### <a name="feature-activation"></a>功能激活

**将程序集文件放在**

`IHostingStartup`实现的程序集文件必须为*bin*-在应用程序部署或放入[运行时存储](/dotnet/core/deploying/runtime-store):

为每个用户使用，将在用户配置文件的运行时存储在该程序集：

```
<DRIVE>\Users\<USER>\.dotnet\store\x64\netcoreapp2.0\<FEATURE_ASSEMBLY_NAME>\<FEATURE_VERSION>\lib\netcoreapp2.0\
```

共用，将程序集放入.NET 核心安装的运行时存储：

```
<DRIVE>\Program Files\dotnet\store\x64\netcoreapp2.0\<FEATURE_ASSEMBLY_NAME>\<FEATURE_VERSION>\lib\netcoreapp2.0\
```

在部署到运行时存储程序集，符号文件可能也部署，但并不是必需的功能正常工作。

**将依赖项文件**

实现 *\*。 deps.json*文件必须在可访问的位置。

为每个用户使用，将该文件置于`additonalDeps`的用户配置文件的文件夹`.dotnet`设置： 

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

共用，将该文件置于`additonalDeps`.NET 核心安装的文件夹：

```
<DRIVE>\Program Files\dotnet\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

记下的版本， `2.0.0`，反映的共享的运行时的目标应用程序使用的版本。 共享的运行时所示 *\*。 runtimeconfig.json*文件。 在示例应用中，共享的运行时中指定*HostingStartupSample.runtimeconfig.json*文件。

**设置环境变量**

在应用程序使用的功能的上下文中设置以下环境变量。

ASPNETCORE\_HOSTINGSTARTUPASSEMBLIES

只有宿主启动的程序集扫描的`HostingStartupAttribute`。 此环境变量中提供了实现的程序集名称。 示例应用程序将此值设置为`StartupDiagnostics`。

此外可以使用设置值[承载启动程序集](xref:fundamentals/hosting#hosting-startup-assemblies)承载配置设置。

DOTNET\_其他\_DEPS

实现的位置 *\*。 deps.json*文件。

如果将文件放在用户配置文件的*.dotnet*为每个用户使用的文件夹：

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\
```

如果该文件放置在共用的.NET Core 安装中，提供该文件的完整路径：

```
<DRIVE>\Program Files\dotnet\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\<FEATURE_ASSEMBLY_NAME>.deps.json
```

示例应用程序将此值设置为：

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

有关如何设置各种操作系统的环境变量的示例，请参阅[使用多个环境](xref:fundamentals/environments)。

## <a name="sample-app"></a>示例应用程序

[示例应用程序](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/platform-specific-configuration/sample/)([如何下载](xref:tutorials/index#how-to-download-a-sample)) 使用`IHostingStartup`若要创建一个诊断工具。 该工具将两个中间件添加到提供的诊断信息的启动时的应用：

* 已注册的服务
* 地址： 方案、 主机、 基路径、 路径、 查询字符串
* 连接： 远程 IP、 远程端口、 本地 IP，本地端口，客户端证书
* 请求标头
* 环境变量

若要运行示例：

1. 启动诊断项目使用[PowerShell](/powershell/scripting/powershell-scripting)若要修改其*StartupDiagnostics.deps.json*文件。 默认情况下，从 Windows 7 SP1 和 Windows Server 2008 R2 SP1 的 Windows OS 上安装 PowerShell。 若要在其他平台上获取 PowerShell，请参阅[安装 Windows PowerShell](/powershell/scripting/setup/installing-windows-powershell)。
2. 生成诊断启动项目。 项目文件中的生成目标：
   * 将程序集和符号文件添加到用户配置文件的运行时存储。
   * 触发 PowerShell 脚本来修改*StartupDiagnostics.deps.json*文件。
   * 将移动*StartupDiagnostics.deps.json*用户配置文件的文件`additionalDeps`文件夹。
3. 设置环境变量：
    * `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`
    * `DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`
4. 运行示例应用程序。
5. 请求`/services`终结点以查看应用程序的注册服务。 请求`/diag`终结点若要查看诊断信息。
