---
title: "将新字段添加到 Razor 页面"
author: rick-anderson
description: "演示如何使用 Entity Framework Core 将新字段添加到 Razor 页面"
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: 36412e9d1f3143f0d1999d0e754e6627f0984ad5
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="adding-a-new-field-to-a-razor-page"></a>将新字段添加到 Razor 页面

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

在本部分中，将使用 [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First 迁移将新字段添加到模型，并将此更改迁移到数据库。

使用 EF Code First 自动创建数据库时，Code First 会向数据库添加表格，以帮助跟踪数据库的架构是否与从其中生成它的模型类同步。 如果它们不同步，EF 则会引发异常。 这使查找不一致的数据库/代码问题变得更加轻松。

## <a name="adding-a-rating-property-to-the-movie-model"></a>向电影模型添加分级属性

打开 Models/Movie.cs 文件，并添加 `Rating` 属性：

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

生成应用 (Ctrl+Shift+B)。

编辑 Pages/Movies/Index.cshtml，并添加 `Rating` 字段：

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=40-42,61-63)]

将 `Rating` 字段添加到“删除”和“详细信息”页面。

使用 `Rating` 字段更新 Create.cshtml。 可以复制/粘贴之前的 `<div>` 元素，并让 intelliSense 帮助更新字段。 IntelliSense 适用于[标记帮助程序](xref:mvc/views/tag-helpers/intro)。

![开发人员已在视图的第二个标签元素中键入字母 R 用作 asp-for 的特性值。 出现了 Intellisense 上下文菜单，其中显示了可用字段，包括在列表中自动突出显示的“Rating”。 开发人员单击此字段或在键盘上按 Enter 键时，此值将设置为“Rating”。](new-field/_static/cr.png)

下面的代码显示具有 `Rating` 字段的 Create.cshtml：

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?highlight=36-40)]

将 `Rating` 字段添加到“编辑”页面。

在 DB 更新为包括新字段之前，应用将不会正常工作。 如果立即运行，应用会引发 `SqlException`：

```
SqlException: Invalid column name 'Rating'.
```

此错误是由于更新的 Movie 模型类与数据库的 Movie 表架构不同导致的。 （数据库表中没有 `Rating` 列。）

可通过几种方法解决此错误：

1. 让 Entity Framework 自动丢弃并使用新的模型类架构重新创建数据库。 此方法在开发周期早期很方便；通过它可以一起快速改进模型和数据库架构。 此方法的缺点是会导致数据库中的现有数据丢失。 请勿对生产数据库使用此方法！ 当架构更改时丢弃数据库并使用初始值设定项以使用测试数据自动设定数据库种子，这通常是开发应用的有效方式。

2. 对现有数据库架构进行显式修改，使它与模型类相匹配。 此方法的优点是可以保留数据。 可以手动或通过创建数据库更改脚本进行此更改。

3. 使用 Code First 迁移更新数据库架构。

对于本教程，请使用 Code First 迁移。

更新 `SeedData` 类，使它提供新列的值。 示例更改如下所示，但可能需要对每个 `new Movie` 块做出此更改。

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

请参阅[已完成的 SeedData.cs 文件](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs)。

生成解决方案。

<a name="pmc"></a> 从“工具”菜单中，选择“NuGet 包管理器”>“包管理器控制台”。
在 PMC 中，输入以下命令：

```powershell
Add-Migration Rating
Update-Database
```

`Add-Migration` 命令会通知框架执行以下操作：

* 将 `Movie` 模型与 `Movie` DB 架构进行比较。
* 创建代码以将 DB 架构迁移到新模型。

名称“Rating”是任意的，用于对迁移文件进行命名。 为迁移文件使用有意义的名称是有帮助的。

<a name="ssox"></a> 如果删除 DB 中的所有记录，种子初始值设定项会设定 DB 种子，并将包括 `Rating` 字段。 可以使用浏览器中的删除链接，也可以从 [Sql Server 对象资源管理器](xref:tutorials/razor-pages/sql#ssox) (SSOX) 执行此操作。 从 SSOX 中删除数据库：

* 在 SSOX 中选择数据库。
* 右键单击数据库，并选择“删除”。
* 检查“关闭现有连接”。
* 选择“确定”。
* 在 [PMC](xref:tutorials/razor-pages/new-field#pmc) 中更新数据库：

  ```powershell
  Update-Database
  ```

运行应用，并验证是否可以创建/编辑/显示具有 `Rating` 字段的电影。 如果数据库未设定种子，请先停止 IIS Express，然后再运行应用。

>[!div class="step-by-step"]
[上一篇：添加搜索](xref:tutorials/razor-pages/search)
[下一篇：添加验证](xref:tutorials/razor-pages/validation)
