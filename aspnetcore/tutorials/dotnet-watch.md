---
title: "使用 dotnet watch 开发 ASP.NET Core 应用"
author: rick-anderson
description: "本教程演示如何在 ASP.NET Core 应用程序中安装和使用 .NET Core CLI 的文件观察程序 (dotnet watch) 工具。"
manager: wpickett
ms.author: riande
ms.date: 10/05/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/dotnet-watch
ms.openlocfilehash: cb15e28cb98ea82091cf5ddeed12df8926079e52
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="developing-aspnet-core-apps-using-dotnet-watch"></a>使用 dotnet watch 开发 ASP.NET Core 应用

作者：[Rick Anderson](https://twitter.com/RickAndMSFT) 和 [Victor Hurdugaci](https://twitter.com/victorhurdugaci)

`dotnet watch` 是一种在源文件更改时运行 [.NET Core CLI](/dotnet/core/tools) 命令的工具。 例如，文件更改可能触发编译、测试执行或部署。

本教程中使用现有 Web API 应用，它具有两个终结点：分别返回总计和产品。 产品方法中有一个 bug，我们将在本教程中修复它。

下载[示例应用](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample)。 它包含两个项目：WebApp（一种 ASP.NET Core Web API）和 WebAppTests（Web API 的单元测试）。

在命令外壳中，导航到“WebApp”文件夹并运行以下命令：

```console
dotnet run
```

控制台输出会显示如下类似的消息（表示应用正在运行且正在等待请求）：

```console
$ dotnet run
Hosting environment: Development
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

在 Web 浏览器中，导航到 `http://localhost:<port number>/api/math/sum?a=4&b=5`。 应该会显示结果 `9`。

导航到产品 API (`http://localhost:<port number>/api/math/product?a=4&b=5`)。 它会返回 `9`，而不是所预期的 `20`。 本教程稍后将对其进行修复。

## <a name="add-dotnet-watch-to-a-project"></a>将 `dotnet watch` 添加到项目

1. 将 `Microsoft.DotNet.Watcher.Tools` 包引用添加到 .csproj 文件：

    ```xml
    <ItemGroup>
        <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
    </ItemGroup> 
    ```

1. 运行以下命令，安装 `Microsoft.DotNet.Watcher.Tools` 包：
    
    ```console
    dotnet restore
    ```

## <a name="running-net-core-cli-commands-using-dotnet-watch"></a>使用 `dotnet watch` 运行 .NET Core CLI 命令

`dotnet watch` 可用于运行任何 [.NET Core CLI 命令](/dotnet/core/tools#cli-commands) 例如:

| 命令 | 带 watch 的命令 |
| ---- | ----- |
| dotnet 运行 | dotnet watch run |
| dotnet run -f netcoreapp2.0 | dotnet watch run -f netcoreapp2.0 |
| dotnet run -f netcoreapp2.0 -- --arg1 | dotnet watch run -f netcoreapp2.0 -- --arg1 |
| dotnet test | dotnet watch test |

运行“WebApp”文件夹中的 `dotnet watch run`。 控制台输出指示 `watch` 已启动。

## <a name="making-changes-with-dotnet-watch"></a>通过 `dotnet watch` 进行更改

确保 `dotnet watch` 正在运行。

修复 MathController 的 `Product` 方法中的 bug，使其返回产品而非总和。

```csharp
public static int Product(int a, int b)
{
  return a * b;
} 
```

保存该文件。 控制台输出指示 `dotnet watch` 已检测到文件更改并已重启应用。

验证 `http://localhost:<port number>/api/math/product?a=4&b=5` 是否返回正确结果。

## <a name="running-tests-using-dotnet-watch"></a>使用 `dotnet watch` 运行测试

1. 将 MathController.cs 的 `Product` 方法改回返回总和并保存文件。
1. 在命令外壳中，导航到“WebAppTests”文件夹。
1. 运行 `dotnet restore`。
1. 运行 `dotnet watch test`。 其输出指示测试失败且观察程序正在等待文件更改：

     ```console
     Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
     Test Run Failed.
     ```

1. 修复 `Product` 方法代码，使其返回产品。 保存该文件。

`dotnet watch` 检测到文件更改并重新运行测试。 控制台输出指示测试通过。

## <a name="dotnet-watch-in-github"></a>GitHub 中的 dotnet-watch

dotnet-watch 是 GitHub [DotNetTools 存储库](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch)的一部分。

[dotnet-watch 自述文件](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md)的 [MSBuild 部分](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch#msbuild)阐释了如何通过被监视的 MSBuild 项目文件配置 dotnet-watch。 [dotnet-watch 自述文件](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md)介绍了本教程中没有的 dotnet-watch 相关信息。
