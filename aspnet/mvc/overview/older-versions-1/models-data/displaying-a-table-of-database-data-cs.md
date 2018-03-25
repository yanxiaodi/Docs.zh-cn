---
uid: mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-cs
title: "显示表的数据库数据 (C#) |Microsoft 文档"
author: microsoft
description: "在本教程中，我将演示两种方法可以显示一组数据库记录。 我显示以下两种格式的一组数据库记录在 HTML 数据..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: d6e758b6-6571-484d-a132-34ee6c47747a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 37ea081df2ee26e186669b815a4d769e1976ae9c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="displaying-a-table-of-database-data-c"></a>显示表的数据库数据 (C#)
====================
通过[Microsoft](https://github.com/microsoft)

[下载 PDF](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_11_CS.pdf)

> 在本教程中，我将演示两种方法可以显示一组数据库记录。 我将介绍两种格式的 HTML 表中的数据库记录的一组方法。 首先，我介绍如何设置格式的视图中直接的数据库记录。 接下来，我演示如何可以充分利用它们，当数据库记录进行格式化。


本教程旨在说明如何可以在 ASP.NET MVC 应用程序中显示一个 HTML 表的数据库数据。 首先，你将学习如何使用 Visual Studio 中包含的基架工具来生成自动显示一组记录的视图。 接下来，你将了解如何格式化数据库记录时将用作模板的部分。

## <a name="create-the-model-classes"></a>创建模型类

我们将会显示的电影数据库表中的记录集。 电影数据库表包含以下列：

<a id="0.3_table01"></a>


| **列名称** | **数据类型** | **允许 null 值** |
| --- | --- | --- |
| Id | Int | False |
| 标题 | Nvarchar （200) | False |
| 控制器 | NVarchar(50) | False |
| DateReleased | DateTime | False |


为了表示我们 ASP.NET MVC 应用程序中的电影表，我们需要创建一个模型类。 在本教程中，我们可以使用 Microsoft 实体框架来创建我们模型的类。

> [!NOTE] 
> 
> 在本教程中，我们使用 Microsoft 实体框架。 但是，务必了解你可以使用各种不同的技术来与从 ASP.NET MVC 应用程序包括 LINQ to SQL、 NHibernate 或 ADO.NET 数据库交互。


请按照下列步骤以启动实体数据模型向导：

1. 右键单击解决方案资源管理器窗口，然后选择菜单选项中的 Models 文件夹**添加、 新项**。
2. 选择**数据**类别，然后选择**ADO.NET 实体数据模型**模板。
3. 将命名的数据模型*MoviesDBModel.edmx*单击**添加**按钮。

单击添加按钮后，则将显示实体数据模型向导 （请参见图 1）。 请按照下列步骤以完成向导：

1. 在**选择模型内容**步骤中，选择**从数据库生成**选项。
2. 在**选择你的数据连接**步骤中，使用*MoviesDB.mdf*数据连接和名称*MoviesDBEntities*对于连接设置。 单击**下一步**按钮。
3. 在**选择数据库对象**步骤，展开表节点，选择电影表。 输入的命名空间*模型*单击**完成**按钮。


[![创建 LINQ to SQL 类](displaying-a-table-of-database-data-cs/_static/image1.jpg)](displaying-a-table-of-database-data-cs/_static/image1.png)

**图 01**： 创建 LINQ to SQL 类 ([单击以查看实际尺寸的图像](displaying-a-table-of-database-data-cs/_static/image2.png))


完成实体数据模型向导后，将打开实体数据模型设计器。 设计器应显示电影实体 （请参见图 2）。


[![实体数据模型设计器](displaying-a-table-of-database-data-cs/_static/image2.jpg)](displaying-a-table-of-database-data-cs/_static/image3.png)

**图 02**: 实体数据模型设计器 ([单击以查看实际尺寸的图像](displaying-a-table-of-database-data-cs/_static/image4.png))


我们需要进行一次更改，我们继续操作之前。 实体数据向导生成一个名为的模型类*电影*表示电影数据库表。 因为我们将使用影片类来表示特定电影，我们需要修改的类的名称*电影*而不是*电影*（单数而复数形式）。

双击设计器图面上的类的名称，并将影片中的类的名称更改为影片。 此更改后，单击**保存**按钮 （软盘图标） 生成电影类。

## <a name="create-the-movies-controller"></a>创建电影控制器

现在，我们已表示我们的数据库记录的方式，我们可以创建返回的电影收藏的控制器。 在 Visual Studio 解决方案资源管理器窗口中，右键单击 Controllers 文件夹，然后选择菜单选项**添加、 控制器**（请参见图 3）。


[![添加控制器菜单](displaying-a-table-of-database-data-cs/_static/image3.jpg)](displaying-a-table-of-database-data-cs/_static/image5.png)

**图 03**: 添加控制器菜单 ([单击以查看实际尺寸的图像](displaying-a-table-of-database-data-cs/_static/image6.png))


当**添加控制器**对话框出现时，输入控制器名称 MovieController （请参见图 4）。 单击**添加**按钮以添加新控制器。


[![添加控制器对话框](displaying-a-table-of-database-data-cs/_static/image4.jpg)](displaying-a-table-of-database-data-cs/_static/image7.png)

**图 04**: 添加控制器对话框 ([单击以查看实际尺寸的图像](displaying-a-table-of-database-data-cs/_static/image8.png))


我们需要修改从而使其返回的数据库记录集公开的电影控制器的 index （） 操作。 修改控制器，以便它如下所示列出 1 中的控制器。

**列表 1 – Controllers\MovieController.cs**

[!code-csharp[Main](displaying-a-table-of-database-data-cs/samples/sample1.cs)]

在清单 1 MoviesDBEntities 类用于表示 MoviesDB 数据库。 若要使用此类，你需要导入如下 MvcApplication1.Models 命名空间：

使用 MvcApplication1.Models;

表达式*实体。MovieSet.ToList()*返回所有电影的集，从电影数据库表。

## <a name="create-the-view"></a>创建视图

HTML 表中显示一组数据库记录的最简单方法是利用 Visual Studio 提供的基架。

通过选择菜单选项来生成你的应用程序**生成，生成解决方案**。 必须生成应用程序在打开之前**添加视图**数据类或对话框不会显示在对话框。

右键单击 index （） 操作，然后选择菜单选项**添加视图**（请参见图 5）。


[![添加视图](displaying-a-table-of-database-data-cs/_static/image5.jpg)](displaying-a-table-of-database-data-cs/_static/image9.png)

**图 05**： 添加视图 ([单击以查看实际尺寸的图像](displaying-a-table-of-database-data-cs/_static/image10.png))


在**添加视图**对话框中，选中复选框标记为**创建强类型化视图**。 选择的电影类**查看数据类**。 选择*列表*作为**查看内容**（请参阅图 6）。 选择这些选项将生成一个强类型的视图，显示影片列表。


[![添加视图对话框](displaying-a-table-of-database-data-cs/_static/image6.jpg)](displaying-a-table-of-database-data-cs/_static/image11.png)

**图 06**: 添加视图对话框 ([单击以查看实际尺寸的图像](displaying-a-table-of-database-data-cs/_static/image12.png))


单击后**添加**按钮，列出 2 中的视图自动生成。 此视图包含循环访问的电影收藏并显示一部电影的属性的每个所需的代码。

**列出 2 – Views\Movie\Index.aspx**

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample2.aspx)]

你可以通过选择菜单选项来运行该应用程序**调试、 启动调试**（或按 F5 键）。 运行应用程序将启动 Internet Explorer。 如果您导航到 /Movie URL，然后你将看到图 7 中的页。


[![表的电影](displaying-a-table-of-database-data-cs/_static/image7.jpg)](displaying-a-table-of-database-data-cs/_static/image13.png)

**图 07**： 电影的表 ([单击以查看实际尺寸的图像](displaying-a-table-of-database-data-cs/_static/image14.png))


如果你不喜欢有关图 7 中的数据库记录的网格的外观的任何信息则可以只需修改索引视图。 例如，您可以更改*DateReleased*标头到*发布日期*通过修改索引视图。

## <a name="create-a-template-with-a-partial"></a>使用部分创建一个模板

当视图获取过于复杂时，它是启动视图拆分为它们的一个好办法。 使用它们可以更轻松地理解和维护你的视图。 我们将创建一个分部，我们可以使用作为模板来设置每个电影数据库记录的格式。

请按照以下步骤创建分部操作：

1. 右键单击 Views\Movie 文件夹然后选择菜单选项**添加视图**。
2. 选中复选框标记为*创建了分部视图 (.ascx)*。
3. 命名分部*MovieTemplate*。
4. 选中复选框标记为**创建强类型化视图**。
5. 选择部电影作为*查看数据类*。
6. 选择作为空*查看内容*。
7. 单击**添加**按钮将部分添加到你的项目。

完成这些步骤后，修改部分 MovieTemplate 如下所示列出 3。

**列出 3 – Views\Movie\MovieTemplate.ascx**

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample3.aspx)]

列出 3 中的部分包含记录的单个行的模板。

已修改的索引视图，列出 4 中使用部分 MovieTemplate。

**列出 4 – Views\Movie\Index.aspx**

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample4.aspx)]

该视图列出 4 中的包含的 foreach 循环中循环访问所有影片。 为每个的电影，部分 MovieTemplate 用于格式化影片。 MovieTemplate 呈现通过调用 RenderPartial() 帮助器方法。

已修改的索引视图呈现非常同一个 HTML 表的数据库记录。 但是，该视图进行了极大地简化。


RenderPartial() 方法是不同于大多数其他帮助器方法，因为它不返回一个字符串。 因此，必须调用 RenderPartial() 方法使用&lt;%html.renderpartial(); %&gt;而不是&lt;%= Html.RenderPartial(); %&gt;。


## <a name="summary"></a>摘要

本教程的目的是说明如何可以在 HTML 表中显示一组数据库记录。 首先，您学习了如何通过利用 Microsoft 实体框架的控制器操作返回一组数据库记录。 接下来，您学习了如何使用 Visual Studio 基架以生成一个视图，自动显示项的集合。 最后，您学习了如何通过利用部分的简化视图。 您学习了如何将用作模板的部分，以便你可以设置每个数据库记录的格式。

>[!div class="step-by-step"]
[上一页](creating-model-classes-with-linq-to-sql-cs.md)
[下一页](performing-simple-validation-cs.md)
