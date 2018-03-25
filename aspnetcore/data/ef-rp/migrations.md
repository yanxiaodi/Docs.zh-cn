---
title: "Razor 页面和 EF Core - 迁移 - 第 4 个教程，共 8 个教程"
author: rick-anderson
description: "本教程使用 EF Core 迁移功能管理 ASP.NET Core MVC 应用中的数据模型更改。"
manager: wpickett
ms.author: riande
ms.date: 10/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/migrations
ms.openlocfilehash: e89d95702cb94556bc6e5dc73253c51acaa11578
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/31/2018
---
# <a name="migrations---ef-core-with-razor-pages-tutorial-4-of-8"></a>迁移 - EF Core 和 Razor 页面教程（第 4 个教程，共 8 个）

作者：[Tom Dykstra](https://github.com/tdykstra)、[Jon P Smith](https://twitter.com/thereformedprog) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

本教程使用 EF Core 迁移功能管理数据模型更改。

如果遇到无法解决的问题，请下载[本阶段的已完成应用](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations)。

开发新应用时，数据模型会频繁更改。 每当模型发生更改时，都无法与数据库进行同步。 本教程首先配置 Entity Framework 以创建数据库（如果不存在）。 每当数据模型发生更改时：

* DB 都会被删除。
* EF 都会创建一个新数据库来匹配该模型。
* 应用使用测试数据为 DB 设定种子。

这种使 DB 与数据模型保持同步的方法适用于多种情况，但将应用部署到生产环境的情况除外。 当应用在生产环境中运行时，应用通常会存储需要保留的数据。 每当发生更改（例如添加新列）时，应用都无法在具有测试 DB 的环境下启动。 EF Core 迁移功能可通过使 EF Core 更新 DB 架构而不是创建新 DB 来解决此问题。

数据模型发生更改时，迁移将更新架构并保留现有数据，而无需删除或重新创建 DB。

## <a name="entity-framework-core-nuget-packages-for-migrations"></a>用于进行迁移的 Entity Framework Core NuGet 包

要使用迁移，请使用“包管理器控制台”(PMC) 或命令行接口 (CLI)。 以下教程演示如何使用 CLI 命令。 有关 PMC 的信息，请转到[本教程末尾](#pmc)。

[Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) 中提供了适用于命令行接口 (CLI) 的 EF Core 工具。 若要安装此程序包，请将它添加到 .csproj 文件中的 `DotNetCliToolReference` 集合，如下所示。 **注意：**必须通过编辑 .csproj 文件来安装此程序包。 不能使用 `install-package` 命令或包管理器 GUI 安装此程序包。 要编辑 .csproj 文件，请右键单击解决方案资源管理器中的项目名称，然后选择“编辑 ContosoUniversity.csproj”。

以下标记显示已更新的 .csproj 文件，其中突出显示了 EF Core CLI 工具：

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?highlight=12)]
  
编写本教程时，前面示例中的版本号是最新的。 请对在其他程序包中发现的 EF Core CLI 工具使用相同的版本。

## <a name="change-the-connection-string"></a>更改连接字符串

在 appsettings.json 文件中，将连接字符串中 DB 名称更改为 ContosoUniversity2。

[!code-json[Main](intro/samples/cu/appsettings2.json?range=1-4)]

更改连接字符串中的 DB 名称会导致初始迁移创建新的 DB。 之所以会创建新的 DB，是因为不存在具有该名称的 DB。 使用迁移无需更改连接字符串。

更改 DB 名称的另一种方法是删除 DB。 使用 SQL Server 对象资源管理器 (SSOX) 或 `database drop` CLI 命令：

 ```console
 dotnet ef database drop
 ```

下面的部分说明如何运行 CLI 命令。

## <a name="create-an-initial-migration"></a>创建初始迁移

生成项目。

打开命令窗口并导航到项目文件夹。 项目文件夹包含 Startup.cs 文件。

在命令窗口中输入以下内容：

```console
dotnet ef migrations add InitialCreate
```

命令窗口显示类似于以下内容的信息：

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

如果迁移失败，并出现消息“无法访问文件...ContosoUniversity.dll，因为它正被另一个进程使用。” ：

* 停止 IIS Express。

   * 退出并重启 Visual Studio，或
   * 在 Windows 系统托盘中找到 IIS Express 图标。
   * 右键单击 IIS Express 图标，然后单击“ContosoUniversity”>“停止站点”。

如果出现错误消息“生成失败。”， 请再次运行该命令。 如果收到此错误，请在本教程底部留下说明。

### <a name="examine-the-up-and-down-methods"></a>了解 Up 和 Down 方法

EF Core 命令 `migrations add` 已生成用于创建 DB 的代码。 此迁移代码位于 Migrations\<timestamp>_InitialCreate.cs 文件中。 `InitialCreate` 类的 `Up` 的方法创建与数据模型实体集相对应的 DB 表。 `Down` 方法删除这些表，如下例所示：

[!code-csharp[Main](intro/samples/cu/Migrations/20171026010210_InitialCreate.cs?range=8-24,77-)]

迁移调用 `Up` 方法为迁移实现数据模型更改。 输入用于回退更新的命令时，迁移调用 `Down` 方法。

前面的代码适用于初始迁移。 该代码是运行 `migrations add InitialCreate` 命令时创建的。 迁移名称参数（本示例中为“InitialCreate”）用于指定文件名。 迁移名称可以是任何有效的文件名。 最好选择能概括迁移中所执行操作的字词或短语。 例如，添加了系表的迁移可称为“AddDepartmentTable”。

如果创建了初始迁移并且 DB 已退出：

* 会生成 DB 创建代码。
* DB 创建代码不需要运行，因为 DB 已与数据模型相匹配。 即使 DB 创建代码运行也不会做出任何更改，因为 DB 已与数据模型相匹配。

如果将应用部署到新环境，则必须运行 DB 创建代码才能创建 DB。

以前，需要更改连接字符串才能使用 DB 的新名称。 指定的 DB 不存在，因此迁移会创建 DB。

### <a name="examine-the-data-model-snapshot"></a>了解数据模型快照

迁移会在 Migrations/SchoolContextModelSnapshot.cs 中创建当前 DB 架构的快照：

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot1.cs?name=snippet_Truncate)]

由于当前 DB 架构以代码表示，因此 EF Core 无需与 DB 交互即可创建迁移。 添加迁移时，EF Core 会通过将数据模型与快照文件进行对比来确定已更改的内容。 EF Core 仅在必须更新 DB 时才与 DB 进行交互。

快照文件必须与创建它的迁移保持同步。 删除名为 \<timestamp>_\<migrationname>.cs 的文件时不会删除迁移。 删除该文件后，剩余的迁移不会与 DB 快照文件保持同步。 若要删除上次添加的迁移，请使用 [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) 命令。

## <a name="remove-ensurecreated"></a>删除 EnsureCreated

以前的开发通常使用 `EnsureCreated` 命令。 本教程将使用迁移。 `EnsureCreated` 具有以下限制：

* 绕过迁移并创建 DB 和架构。
* 不会创建迁移表。
* 不能与迁移一起使用。
* 专门用于在频繁删除并重新创建 DB 的情况下进行测试或快速制作原型。

删除 `DbInitializer` 中的以下行：

```csharp
context.Database.EnsureCreated();
```

## <a name="apply-the-migration-to-the-db-in-development"></a>在开发过程中将迁移应用于 DB

在命令窗口中，输入以下内容以创建 DB 和表。

```console
dotnet ef database update
```

注意：如果 `update` 命令返回“生成失败。”错误：

* 请再次运行该命令。
* 如果再次失败，请退出 Visual Studio，然后运行 `update` 命令。
* 请在页面底部留言。

该命令的输出与 `migrations add` 命令的输出相似。 上面的命令中显示了用于设置 DB 的 SQL 命令的日志。 下面的示例输出中省略了大部分日志：

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

若要降低日志消息的详细级别，请更改 appsettings.Development.json 文件中的日志级别。 有关详细信息，请参阅[日志记录介绍](xref:fundamentals/logging/index)。

使用 SQL Server 对象资源管理器检查 DB。 请注意，增加了 `__EFMigrationsHistory` 表。 `__EFMigrationsHistory` 表跟踪已应用到 DB 的迁移。 查看 `__EFMigrationsHistory` 表中的数据，其中显示对应初始迁移的一行数据。 上面的 CLI 输出示例中最后部分的日志显示了创建此行的 INSERT 语句。

运行应用并验证一切正常运行。

## <a name="appling-migrations-in-production"></a>在生产环境中应用迁移

不建议生产应用在应用程序启动时调用 [Database.Migrate](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_)。 不应从服务器场中的应用调用 `Migrate`。 例如，已将应用在云中部署为横向扩展（运行应用的多个示例）的情况。

应在部署过程中以受控的方式执行数据库迁移。 生产数据库迁移方法包括：

* 使用迁移创建 SQL 脚本，并在部署过程中使用 SQL 脚本。
* 在受控的环境中运行 `dotnet ef database update`。

EF Core 使用 `__MigrationsHistory` 表查看是否需要运行任何迁移。 如果 DB 已是最新，则无需运行迁移。

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a>命令行接口 (CLI) 与包管理器控制台 (PMC)

可通过以下项使用可管理迁移的 EF Core 工具：

* .NET Core CLI 命令。
* Visual Studio 包管理器控制台 (PMC) 窗口中的 PowerShell cmdlet。

本教程演示如何使用 CLI，但某些开发人员更倾向于使用 PMC。

适用于 PMC 的 EF Core 命令位于 [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) 程序包中。 此包包含在 [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) 元包中，因此无需另外安装。

**重要说明：**此程序包与通过编辑 .csproj 文件为 CLI 安装的程序包不同。 此程序包的名称以 `Tools` 结尾，而 CLI 程序包的名称以 `Tools.DotNet` 结尾。

有关 CLI 命令的详细信息，请参阅 [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet)。

有关 PMC 命令的详细信息，请参阅[包管理器控制台 (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell)。

## <a name="troubleshooting"></a>疑难解答

请下载[本阶段的已完成应用](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations)。

应用会生成以下异常：

```text
`SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

解决方案：运行 `dotnet ef database update`

如果 `update` 命令返回“生成失败。”错误：

* 请再次运行该命令。
* 请在页面底部留言。

>[!div class="step-by-step"]
[上一页](xref:data/ef-rp/sort-filter-page)
[下一页](xref:data/ef-rp/complex-data-model)
