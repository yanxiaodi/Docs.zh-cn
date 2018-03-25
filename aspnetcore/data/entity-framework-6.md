---
title: "ASP.NET Core 和 Entity Framework 6 入门"
author: tdykstra
description: "本文演示如何在 ASP.NET Core 应用程序中使用 Entity Framework 6。"
manager: wpickett
ms.author: tdykstra
ms.date: 02/24/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: data/entity-framework-6
ms.openlocfilehash: 7407fe8a976978d7d5077d5e5ac6cc264565621d
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/31/2018
---
# <a name="getting-started-with-aspnet-core-and-entity-framework-6"></a>ASP.NET Core 和 Entity Framework 6 入门

作者：[Paweł Grudzień](https://github.com/pgrudzien12)、[Damien Pontifex](https://github.com/DamienPontifex) 和 [Tom Dykstra](https://github.com/tdykstra)

本文演示如何在 ASP.NET Core 应用程序中使用 Entity Framework 6。

## <a name="overview"></a>概述

若要使用 Entity Framework 6，则项目必须面向 .NET Framework 进行编译，因为 Entity Framework 6 不支持 .NET Core。 如果需要跨平台功能，需升级到 [Entity Framework Core](https://docs.microsoft.com/ef/)。

在 ASP.NET Core 应用程序中使用 Entity Framework 6 的推荐方法是：将 EF6 上下文和模型类放入面向完整框架的类库项目中。 添加对 ASP.NET Core 项目中的类库的引用。 请参阅示例[针对 EF6 和 ASP.NET Core 项目的 Visual Studio 解决方案](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/)。

不能将 EF6 上下文放入 ASP.NET Core 项目，因为 .NET Core 项目不支持 EF6 命令（如 Enable-Migrations）所需的的各项功能。

无论 EF6 上下文属于哪种项目类型，只有 EF6 命令行工具才能使用 EF6 上下文。 例如，`Scaffold-DbContext` 仅在 Entity Framework Core 中可用。 如果需要对数据库执行反向工程以使其成为 EF6 模型，请参阅[从 Code First 到现有数据库](https://msdn.microsoft.com/jj200620)。

## <a name="reference-full-framework-and-ef6-in-the-aspnet-core-project"></a>在 ASP.NET Core 项目中引用完整框架和 EF6

ASP.NET Core 项目需要引用 .NET Framework 和 EF6。 例如，ASP.NET Core 项目的 .csproj 文件将与以下示例类似（仅显示该文件的相关部分）。

[!code-xml[](entity-framework-6/sample/MVCCore/MVCCore.csproj?range=3-9&highlight=2)]

创建新项目时，请使用 ASP.NET Core Web 应用程序（.NET Framework）模板。

## <a name="handle-connection-strings"></a>处理连接字符串

需通过默认构造函数在 EF6 类库项目中使用 EF6 命令行工具，以便它们能够实例化上下文。 但是，如果想指定要在 ASP.NET Core 项目中使用的连接字符串，则上下文构造函数必须具有可允许你在连接字符串中进行传递的参数。 示例如下。

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContext.cs?name=snippet_Constructor)]

由于 EF6 上下文不具有无参数构造函数，因此 EF6 项目必须提供 [IDbContextFactory](https://msdn.microsoft.com/library/hh506876) 的实现。 EF6 命令行工具将查找和使用该实现，以便它们能够实例化上下文。 示例如下。

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContextFactory.cs?name=snippet_IDbContextFactory)]

在此示例代码中，`IDbContextFactory` 实现将在硬编码的连接字符串中传递。 这是命令行工具要使用的连接字符串。 你需要实施策略以确保类库与调用应用程序使用相同的连接字符串。 例如，可从这两个项目的环境变量中获取值。

## <a name="set-up-dependency-injection-in-the-aspnet-core-project"></a>在 ASP.NET Core 项目中设置依赖项注入

在 Core 项目的 Startup.cs 文件中，为 `ConfigureServices` 中的依赖项注入 (DI) 设置 EF6 上下文。 应将 EF 上下文对象的范围设置为按请求生存期。

[!code-csharp[](entity-framework-6/sample/MVCCore/Startup.cs?name=snippet_ConfigureServices&highlight=5)]

然后即可使用 DI 在控制器中获取上下文的实例。 此代码与针对 EF Core 上下文编写的代码相似：

[!code-csharp[](entity-framework-6/sample/MVCCore/Controllers/StudentsController.cs?name=snippet_ContextInController)]

## <a name="sample-application"></a>示例应用程序

若要获取有效的示例应用程序，请参阅本文随附的[示例 Visual Studio 解决方案](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/)。

可在 Visual Studio 中按照以下步骤从头创建此示例：

* 创建解决方案。

* **添加新项目 > Web > ASP.NET Core Web 应用程序 (.NET Framework)**

* **添加新项目 > Windows 经典桌面 > 类库 (.NET Framework)**

* 在两个项目的“包管理器控制台”(PMC) 中运行 `Install-Package Entityframework` 命令。

* 在类库项目中，创建数据模型类和上下文类，并创建 `IDbContextFactory` 的实现。

* 在类库项目的 PMC 中，运行 `Enable-Migrations` 和 `Add-Migration Initial` 命令。 如果已将 ASP.NET Core 项目设置为启动项目，请向这些命令添加 `-StartupProjectName EF6`。

* 在 Core 项目中，添加对类库项目的项目引用。

* 在 Core 项目的 Startup.cs 中，为 DI 注册上下文。

* 在 Core 项目的 appsettings.json 中，添加连接字符串。

* 在 Core 项目中，添加控制器和视图以验证可读取和写入数据。 （请注意，ASP.NET Core MVC 基架不会使用从类库引用的 EF6 上下文。）

## <a name="summary"></a>摘要

本文提供了在 ASP.NET Core 应用程序中使用 Entity Framework 6 的基本指南。

## <a name="additional-resources"></a>其他资源

* [Entity Framework - 基于代码的配置](https://msdn.microsoft.com/data/jj680699.aspx)
