---
title: "Razor 视图编译和预编译"
author: rick-anderson
description: "解释如何在 ASP.NET Core 应用程序中启用 MVC Razor 视图编译和预编译的参考文档。"
manager: wpickett
ms.author: riande
ms.date: 12/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/view-compilation
ms.openlocfilehash: bd3f4470035b0375fc79aa7caa73b60ba6fc4f53
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="razor-view-compilation-and-precompilation-in-aspnet-core"></a>ASP.NET Core 中的 Razor 视图编译和预编译

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

调用视图时，Razor 视图在运行时进行编译。 ASP.NET Core 1.1.0 及更高版本可以选择性地编译 Razor 视图，并将其与应用一起部署&mdash;这个过程称为预编译。 ASP.NET Core 2.x 项目模板默认启用预编译。

> [!IMPORTANT]
> 在 ASP.NET Core 2.0 中执行[独立部署 (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) 时，Razor 视图预编译当前不可用。 2.1 版本发布后，该功能将可用于 SCD。 有关详细信息，请参阅 [View compilation fails when cross-compiling for Linux on Windows](https://github.com/aspnet/MvcPrecompilation/issues/102)（对 Windows 上的 Linux 进行交叉编译时，视图编译失败）。

预编译注意事项：

* 预编译视图生成的发布捆绑包更小，启动速度更快。
* 在预编译视图之后，无法编辑 Razor 文件。 已编辑的视图不会出现在发布捆绑包中。 

部署预编译的视图：

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

如果项目面向 .NET Framework，请添加对 [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) 的包引用：

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

如果项目面向 .NET Core，则无需进行任何更改。

默认情况下，ASP.NET Core 2.x 项目模板将 `MvcRazorCompileOnPublish` 隐式设置为 `true`，这意味着可以从 .csproj 文件安全地删除此节点。 如果希望显式设置，那么将 `MvcRazorCompileOnPublish` 属性设置为 `true` 并没有什么坏处。 以下 .csproj 示例突出显示了此设置：

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

将 `MvcRazorCompileOnPublish` 设置为 `true`，并添加对 `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation` 的包引用。 以下 .csproj 示例突出显示了这些设置：

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---

通过执行项目根处的如下命令，为[依赖框架的部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)准备应用：

```console
dotnet publish -c Release
```

预编译成功后，将生成包含已编译 Razor 视图的 <project_name> .PrecompiledViews.dll 文件。 例如，以下屏幕截图描述了 WebApplication1.PrecompiledViews.dll 中 Index.cshtml 的内容：

![DLL 中的 Razor 视图](view-compilation/_static/razor-views-in-dll.png)
