---
title: "将模型添加到 ASP.NET Core MVC 应用"
author: rick-anderson
description: "将模型添加到简单的 ASP.NET Core 应用。"
manager: wpickett
ms.author: riande
ms.date: 12/8/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 1819aff0e6ae68ad3c609466e52fcb6510fe1dcd
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model1.md)]

请注意：ASP.NET Core 2.0 模板包含 Models 文件夹。

右键单击 Models 文件夹，然后单击“添加” > “类”。 将类命名为“Movie”，并添加以下属性：

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

数据库需要 `ID` 字段以获取主键。 

生成项目以验证有没有任何错误存在。 现在 MVC 应用中已具有模型。

## <a name="scaffolding-a-controller"></a>搭建控制器基架

在“解决方案资源管理器”中，右键单击“控制器”文件夹，然后单击“添加”>“控制器”。

![上述步骤的视图](adding-model/_static/add_controller.png)

如果出现“添加 MVC 依赖项”对话框：

* [将 Visual Studio 更新到最新版本](https://www.visualstudio.com/downloads/)。 15.5 之前的 Visual Studio 版本显示此对话框。
* 如果无法更新，请选择“添加”，然后再次按照添加控制器步骤操作。

在“添加基架”对话框中，点击“包含视图的 MVC 控制器(使用 Entity Framework)”>“添加”。

![“添加基架”对话框](adding-model/_static/add_scaffold2.png)

填写“添加控制器”对话框：

* 模型类：Movie（MvcMovie.Models）
* 数据上下文类：选择 + 图标并添加默认的 MvcMovie.Models.MvcMovieContext 

![“添加数据”上下文](adding-model/_static/dc.png)

* 视图：将每个选项保持为默认选中状态
* 控制器名称：保留默认的 MoviesController
* 点击“添加”

![“添加控制器”对话框](adding-model/_static/add_controller2.png)

Visual Studio 将创建：

* Entity Framework Core [数据库上下文类](xref:data/ef-mvc/intro#create-the-database-context) (Data/MvcMovieContext.cs)
* 电影控制器 (Controllers/MoviesController.cs)
* “创建”、“删除”、“详细信息”、“编辑”和“索引”页面的 Razor 视图文件 (Views/Movies/&ast;.cshtml)

自动创建数据库上下文和 [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete)（创建、读取、更新和删除）操作方法和视图的过程称为“搭建基架”。 你很快将具有功能完整的 Web 应用程序，可使用此应用程序管理电影数据库。

如果运行应用并单击“Mvc 电影”链接，则会出现以下类似的错误：

```
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString 
```

你需要创建数据库，并且使用 EF Core [迁移](xref:data/ef-mvc/migrations)功能来执行此操作。 通过迁移可创建与数据模型匹配的数据库，并在数据模型更改时更新数据库架构。

## <a name="add-ef-tooling-and-perform-initial-migration"></a>添加 EF 工具并执行初始迁移

在本部分中，将使用包管理器控制台 (PMC) 执行以下操作：

* 添加 Entity Framework Core 工具包。 添加迁移并更新数据库需要此工具包。
* 添加初始迁移。
* 使用初始迁移来更新数据库。

从“工具”菜单中，选择“NuGet 包管理器”>“程序包管理器控制台”。

<!-- following image shared with uid: tutorials/razor-pages/model -->
  ![PMC 菜单](adding-model/_static/pmc.png)

在 PMC 中，输入以下命令：

``` PMC
Install-Package Microsoft.EntityFrameworkCore.Tools
Add-Migration Initial
Update-Database
```

请注意：如果在使用 `Install-Package` 命令时收到错误，请打开 NuGet 包管理器并搜索 `Microsoft.EntityFrameworkCore.Tools` 包。 通过此操作，可以安装包或检查是否已安装包。 或者，如果 PMC 存在任何问题，请参阅 [CLI 方法](#cli)。

`Add-Migration` 命令创建代码以创建初始数据库架构。 此架构基于（Data/MvcMovieContext.cs 文件中的）`DbContext` 中指定的模型。 `Initial` 参数用于为迁移命名。 可以使用任意名称，但是按照惯例应选择描述迁移的名称。 有关详细信息，请参阅[迁移简介](xref:data/ef-mvc/migrations#introduction-to-migrations)。

`Update-Database` 命令在用于创建数据库的 Migrations/\<time-stamp>_Initial.cs 文件中运行 `Up` 方法。

<a name="cli"></a> 还可以使用命令行接口 (CLI) 来执行前面的步骤，而不使用 PMC：

* 将 [EF Core 工具](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations)添加到 .csproj 文件。
* 从控制台（在项目目录中）运行以下命令：

  ```console
  dotnet ef migrations add Initial
  dotnet ef database update
  ```     
  

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model4.md)]

![Model 项上的 Intellisense 上下文菜单，列出了 ID、价格、发布日期和标题的可用属性](adding-model/_static/ints.png)

## <a name="additional-resources"></a>其他资源

* [标记帮助程序](xref:mvc/views/tag-helpers/intro)
* [全球化和本地化](xref:fundamentals/localization)

>[!div class="step-by-step"]
[上一篇：添加视图](adding-view.md)
[下一篇：使用 SQL](working-with-sql.md)  
