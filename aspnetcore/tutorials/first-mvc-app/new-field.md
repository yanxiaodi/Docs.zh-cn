---
title: "添加新字段"
author: rick-anderson
description: 
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/new-field
ms.openlocfilehash: f8a5f9528d899f75aaabfbca38f075a27763567f
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="adding-a-new-field"></a>添加新字段

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

在本部分中，将使用 [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First 迁移将新字段添加到模型，并将此更改迁移到数据库。

使用 EF Code First 自动创建数据库时，Code First 会向数据库添加表格，以帮助跟踪数据库的架构是否与从其中生成它的模型类同步。 如果它们不同步，EF 则会引发异常。 这使查找不一致的数据库/代码问题变得更加轻松。

## <a name="adding-a-rating-property-to-the-movie-model"></a>向电影模型添加分级属性

打开 Models/Movie.cs 文件，并添加 `Rating` 属性：

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

生成应用 (Ctrl+Shift+B)。

因为已经添加新字段到 `Movie` 类，所以还需要更新绑定允许名单，将此新属性纳入其中。 在 MoviesController.cs 中，更新 `Create` 和 `Edit` 操作方法的 `[Bind]` 属性，以包括 `Rating` 属性：

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

还需要更新视图模板，以便在浏览器视图中显示、创建和编辑新的 `Rating` 属性。

编辑 /Views/Movies/Index.cshtml 文件并添加 `Rating` 字段：

[!code-HTML[Main](start-mvc/sample/MvcMovie/Views/Movies/IndexGenreRating.cshtml?highlight=17,39&range=24-64)]

使用 `Rating` 字段更新 /Views/Movies/Create.cshtml。 可以复制/粘贴之前的“窗体组”，并让 intelliSense 帮助更新字段。 IntelliSense 适用于[标记帮助程序](xref:mvc/views/tag-helpers/intro)。 请注意：在 Visual Studio 2017 的 RTM 版本中，需要安装 Razor intelliSense 的 [Razor 语言服务](https://marketplace.visualstudio.com/items?itemName=ms-madsk.RazorLanguageServices)。 此问题将在下一版本中修复。

![开发人员已在视图的第二个标签元素中键入字母 R 用作 asp-for 的特性值。 出现了 Intellisense 上下文菜单，其中显示了可用字段，包括在列表中自动突出显示的“Rating”。 开发人员单击此字段或在键盘上按 Enter 键时，此值将设置为“Rating”。](new-field/_static/cr.png)

在将 DB 更新为包括新字段之前，应用不会正常工作。 如果现在运行，会出现以下 `SqlException`：

`SqlException: Invalid column name 'Rating'.`

看到此错误是因为更新的 Movie 模型类与现有数据库的 Movie 表架构不同。 （数据库表中没有“Rating”列。）

可通过几种方法解决此错误：

1. 让 Entity Framework 自动丢弃，并基于新的模型类架构重新创建数据库。 在测试数据库上进行开发时，此方法在开发周期早期很方便；通过它可以一起快速改进模型和数据库架构。 但其缺点是会丢失数据库中的现有数据 - 因此请勿对生产数据库使用此方法！ 使用初始值设定项，以使用测试数据自动设定数据库种子，这通常是开发应用程序的有效方式。

2. 对现有数据库架构进行显式修改，使它与模型类相匹配。 此方法的优点是可以保留数据。 可以手动或通过创建数据库更改脚本进行此更改。

3. 使用 Code First 迁移更新数据库架构。

本教程使用 Code First 迁移。

更新 `SeedData` 类，使它提供新列的值。 示例更改如下所示，但可能需要对每个 `new Movie` 做出此更改。

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

生成解决方案。

从“工具”菜单中，选择“NuGet 包管理器”>“包管理器控制台”。

  ![PMC 菜单](adding-model/_static/pmc.png)

在 PMC 中，输入以下命令：

```powershell
Add-Migration Rating
Update-Database
```

`Add-Migration` 命令会通知迁移框架使用当前 `Movie` DB 架构检查当前 `Movie` 模型，并创建必要的代码，将 DB 迁移到新模型。 名称“Rating”是任意的，用于对迁移文件进行命名。 为迁移文件使用有意义的名称是有帮助的。

如果删除 DB 中的所有记录，初始化会设定 DB 种子，并将包括 `Rating` 字段。 可以使用浏览器中的删除链接，也可从 SSOX 执行此操作。

运行应用，并验证是否可以创建/编辑/显示具有 `Rating` 字段的电影。 还应向 `Edit`、`Details` 和 `Delete` 视图模板添加 `Rating` 字段。

>[!div class="step-by-step"]
[上一页](search.md)
[下一页](validation.md)  
