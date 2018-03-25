---
title: "ASP.NET Core MVC 和 EF Core - 迁移 - 第 4 个教程（共 10 个）"
author: tdykstra
description: "本教程使用 EF Core 迁移功能管理 ASP.NET Core MVC 应用程序中的数据模型更改。"
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/migrations
ms.openlocfilehash: fd466af8a73bf4c568fafe7e7fdcaa82021624da
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/31/2018
---
# <a name="migrations---ef-core-with-aspnet-core-mvc-tutorial-4-of-10"></a>迁移 - EF Core 和 ASP.NET Core MVC 教程（第 4 个教程，共 10 个）


作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)
作者：[Tom Dykstra](https://github.com/tdykstra)和[Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso University 示例 Web 应用程序演示如何使用 Entity Framework Core 和 Visual Studio 创建 ASP.NET Core MVC Web 应用程序。 若要了解教程系列，请参阅[本系列中的第一个教程](intro.md)。

本教程使用 EF Core 迁移功能管理数据模型更改。 后续教程将在更改数据模型时添加更多迁移。

## <a name="introduction-to-migrations"></a>迁移简介
开发新应用程序时，数据模型会频繁更改。每当模型更改时，模型都无法与数据库保持同步。 本系列教程首先配置 Entity Framework 以创建数据库（如果不存在）。 之后，每当更改数据模型（添加、删除或更改实体类或更改 DbContext 类）时，你都可以删除数据库，EF 将创建匹配该模型的新数据库并用测试数据为其设定种子。

这种使数据库与数据模型保持同步的方法适用于多种情况，但将应用程序部署到生产环境的情况除外。 当应用程序在生产环境中运行时，它通常会存储要保留的数据，以便不会在每次更改（如添加新列）时丢失所有数据。 EF Core 迁移功能可通过使 EF 更新数据库 架构而不是创建新数据库来解决此问题。


## <a name="entity-framework-core-nuget-packages-for-migrations"></a>用于实现迁移功能的 Entity Framework Core NuGet 包

要使用迁移，可使用“包管理器控制台”(PMC) 或命令行接口 (CLI)。  以下教程演示如何使用 CLI 命令。 有关 PMC 的信息，请转到[本教程末尾](#pmc)。

[Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) 中提供了适用于命令行接口 (CLI) 的 EF 工具。 若要安装此程序包，请将它添加到 .csproj 文件中的 `DotNetCliToolReference` 集合，如下所示。 注意：必须通过编辑 .csproj 文件来安装此包；不能使用 `install-package` 命令或程序包管理器 GUI。 若要编辑 .csproj 文件，可右键单击解决方案资源管理器中的项目名称，然后选择“编辑 ContosoUniversity.csproj”。

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?range=12-15&highlight=2)]
  
（编写本教程时，本示例中的版本号是最新的。） 

## <a name="change-the-connection-string"></a>更改连接字符串

在 appsettings.json 文件中，将连接字符串中的数据库的名称更改为 ContosoUniversity2 或正在使用的计算机上未使用过的其他名称。

[!code-json[Main](intro/samples/cu/appsettings2.json?range=1-4)]

此更改将设置项目，以便初始迁移创建新的数据库。 这并不是开始使用迁移的必要操作，但稍后你便会了解这样做的好处。

> [!NOTE]
> 除更改数据库名称外，删除数据库同样可行。 使用 SQL Server 对象资源管理器 (SSOX) 或 `database drop` CLI 命令：
> ```console
> dotnet ef database drop
> ```
> 下面的部分说明如何运行 CLI 命令。

## <a name="create-an-initial-migration"></a>创建初始迁移

保存更改并生成项目。 然后打开命令窗口并导航到项目文件夹。 下面是执行此操作的快速方法：

* 在解决方案资源管理器中，右键单击项目，然后从上下文菜单中选择“在文件资源管理器中打开”。

  ![“在文件资源管理器中打开”菜单项](migrations/_static/open-in-file-explorer.png)

* 在地址栏中输入“cmd”，然后按 Enter。

  ![打开命令窗口](migrations/_static/open-command-window.png)

在命令窗口中输入以下命令：

```console
dotnet ef migrations add InitialCreate
```

命令窗口中出现如下输出：

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

> [!NOTE]
> 如果出现错误消息“找不到任何匹配 "dotnet-ef" 命令的可执行文件”，请参阅[此博客文章](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/)获取故障排除帮助。

如果看到错误消息“无法访问文件...ContosoUniversity.dll，因为它正被另一个进程使用。”，请在 Windows 系统托盘中找到 IIS Express 图标并右键单击，然后单击“ContosoUniversity”>“停止站点”。

## <a name="examine-the-up-and-down-methods"></a>了解 Up 和 Down 方法

执行 `migrations add` 命令时，EF 已生成将用于从头创建数据库的代码。 此代码位于“Migrations”文件夹中名为 \<timestamp>_InitialCreate.cs 的文件中。 `InitialCreate` 类的 `Up` 的方法将创建与数据模型实体集相对应的数据库表，`Down` 方法将删除这些表，如下面的示例所示。

[!code-csharp[Main](intro/samples/cu/Migrations/20170215220724_InitialCreate.cs?range=92-118)]

迁移调用 `Up` 方法为迁移实现数据模型更改。 输入用于回退更新的命令时，迁移调用 `Down` 方法。

此代码适用于输入 `migrations add InitialCreate` 命令时所创建的初始迁移。 迁移名称参数（本示例中为“InitialCreate”）用于指定文件名，并且你可以按需使用任何名称。 最好选择能概括迁移中所执行操作的字词或短语。 例如，可将后面的迁移命名为“AddDepartmentTable”。

如果创建初始迁移时已存在数据库，则会生成数据库创建代码，但此代码不必运行，因为数据库已与数据库模型相匹配。 将应用部署到其中尚不存在数据库的其他环境时，此代码将运行以创建数据库，因此最好提前进行测试。 这也是提前更改连接字符串中数据库的名称的原因，这样迁移才能从头创建新数据库。

## <a name="examine-the-data-model-snapshot"></a>了解数据模型快照

迁移还会在 Migrations/SchoolContextModelSnapshot.cs 中创建当前数据库架构的快照。 该代码与以下类似：

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot1.cs?name=snippet_Truncate)]

由于当前数据库架构以代码表示，因此 EF Core 无需与数据库交互即可创建迁移。 添加迁移时，EF 会通过将数据模型与快照文件进行对比来确定已更改的内容。 EF 仅在必须更新数据库时才与数据库进行交互。 

快照文件必须与创建它的迁移保持同步，因此仅删除名为 \<timestamp>_\<migrationname>.cs 的文件并不能删除迁移。 删除该文件后，剩余的迁移将不会与数据库快照文件保持同步。 若要删除上次添加的迁移，请使用 [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) 命令。

## <a name="apply-the-migration-to-the-database"></a>将迁移应用到数据库

在命令窗口中，输入以下命令以创建数据库并在其中创建表。

```console
dotnet ef database update
```

该命令的输出与 `migrations add` 命令的输出相似，但其中还包含设置该数据库的 SQL 命令的日志。 下面的示例输出中省略了大部分日志。 如果希望日志消息中不使用此详细级别，则可更改 appsettings.Development.json 文件中的日志级别。 有关详细信息，请参阅[日志记录介绍](xref:fundamentals/logging/index)。

```text
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (467ms) [Parameters=[], CommandType='Text', CommandTimeout='60']
      CREATE DATABASE [ContosoUniversity2];
info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (20ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      CREATE TABLE [__EFMigrationsHistory] (
          [MigrationId] nvarchar(150) NOT NULL,
          [ProductVersion] nvarchar(32) NOT NULL,
          CONSTRAINT [PK___EFMigrationsHistory] PRIMARY KEY ([MigrationId])
      );

<logs omitted for brevity>

info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (3ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      INSERT INTO [__EFMigrationsHistory] ([MigrationId], [ProductVersion])
      VALUES (N'20170816151242_InitialCreate', N'2.0.0-rtm-26452');
Done.
```

使用 SQL Server 对象资源管理器检查数据库（与第一个教程中的做法相同）。  你会发现添加了 __EFMigrationsHistory 表，该表可用于跟踪已应用到数据库的迁移。 查看该表中的数据，其中显示对应初始迁移的一行数据。 （上面的 CLI 输出示例中最后部分的日志显示了创建此行的 INSERT 语句。）

运行应用程序以验证所有内容照旧运行。

![“学生索引”页](migrations/_static/students-index.png)

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a>命令行接口 (CLI) 与包管理器控制台 (PMC)

可通过 .NET Core CLI 命令或 Visual Studio 包管理器控制台 (PMC) 窗口中的 PowerShell cmdlet 使用可管理迁移的 EF 工具。 本教程演示如何使用 CLI，但也可以根据喜好使用 PMC。

适用于 PMC 命令的 EF 命令位于 [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) 程序包中。 此程序包已包含在 [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) 元包中，因此无需另外安装。

**重要说明：**此程序包与通过编辑 .csproj 文件为 CLI 安装的程序包不同。 此程序包的名称以 `Tools` 结尾，而 CLI 程序包的名称以 `Tools.DotNet` 结尾。

有关 CLI 命令的详细信息，请参阅 [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet)。 

有关 PMC 命令的详细信息，请参阅[包管理器控制台 (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell)。

## <a name="summary"></a>总结

本教程已介绍如何创建并应用初始迁移。 下一教程将介绍有关展开数据模型的更高级主题。 同时还将介绍创建并应用其他迁移的方法。

>[!div class="step-by-step"]
[上一页](sort-filter-page.md)
[下一页](complex-data-model.md)  
