---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-cs
title: "使用 LINQ to SQL (C#) 创建模型类 |Microsoft 文档"
author: microsoft
description: "本教程旨在说明创建 ASP.NET MVC 应用程序的模型类的一个方法。 在本教程中，你将学习如何构建模型 c..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: f84b4a16-e8bb-49e8-87a0-1832879a3501
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-cs
msc.type: authoredcontent
ms.openlocfilehash: c640007a75f2421e0f6c1e86e525de4834bbc8e4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="creating-model-classes-with-linq-to-sql-c"></a>使用 LINQ to SQL (C#) 创建模型类
====================
通过[Microsoft](https://github.com/microsoft)

[下载 PDF](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_10_CS.pdf)

> 本教程旨在说明创建 ASP.NET MVC 应用程序的模型类的一个方法。 在本教程中，你将学习如何生成模型的类，并通过利用 Microsoft LINQ to SQL 中执行数据库访问。


本教程旨在说明创建 ASP.NET MVC 应用程序的模型类的一个方法。 在本教程中，你将学习如何构建模型类和通过利用 Microsoft LINQ to SQL 中执行数据库访问

在本教程中，我们构建基本的电影数据库应用程序。 我们首先创建可能的最快和最简单的方法电影数据库应用程序。 我们直接从我们的控制器操作中执行所有我们数据访问。

接下来，你将了解如何使用存储库模式。 使用存储库模式需要稍有更多工作。 但是，采用此模式的优点是它使你能够构建的应用程序适应更改，并可以轻松地测试。

## <a name="what-is-a-model-class"></a>什么是模型类？

MVC 模型包含所有未包含在 MVC 视图或 MVC 控制器中的应用程序逻辑。 具体而言，MVC 模型包含的所有应用程序业务和数据访问逻辑。

可以使用各种不同的技术来实现数据访问逻辑。 例如，你可以生成数据访问类使用的 Microsoft 实体框架、 NHibernate、 Subsonic 或 ADO.NET 类。

在本教程中，我可以使用 LINQ to SQL 来查询和更新数据库。 LINQ to SQL 可为你提供与 Microsoft SQL Server 数据库交互的轻松方法。 但是，务必了解 ASP.NET MVC framework 未绑定到 LINQ to SQL 以任何方式。 ASP.NET MVC 适用于任何数据访问技术。

## <a name="create-a-movie-database"></a>创建电影数据库

在此教程--若要演示如何生成模型类-我们构建一个简单的电影数据库应用程序。 第一步是创建新的数据库。 右键单击该应用\_在解决方案资源管理器窗口中，然后选择菜单选项的数据文件夹**添加、 新项**。 选择**SQL Server 数据库**模板，使其名称 MoviesDB.mdf，并单击**添加**按钮 （请参见图 1）。


[![添加新的 SQL Server 数据库](creating-model-classes-with-linq-to-sql-cs/_static/image2.png)](creating-model-classes-with-linq-to-sql-cs/_static/image1.png)

**图 01**： 添加新的 SQL Server 数据库 ([单击以查看实际尺寸的图像](creating-model-classes-with-linq-to-sql-cs/_static/image3.png))


创建新的数据库后，你可以通过双击 MoviesDB.mdf 文件，在应用中的打开数据库\_数据文件夹。 双击 MoviesDB.mdf 文件打开服务器资源管理器窗口 （请参见图 2）。

服务器资源管理器窗口时使用 Visual Web Developer 调用数据库资源管理器窗口。


[![使用服务器资源管理器窗口](creating-model-classes-with-linq-to-sql-cs/_static/image5.png)](creating-model-classes-with-linq-to-sql-cs/_static/image4.png)

**图 02**： 使用服务器资源管理器窗口 ([单击以查看实际尺寸的图像](creating-model-classes-with-linq-to-sql-cs/_static/image6.png))


我们需要将表添加到我们表示我们电影的数据库。 右键单击表文件夹，然后选择菜单选项**添加新表**。 选择此菜单选项将打开表设计器 （请参见图 3）。


[![使用服务器资源管理器窗口](creating-model-classes-with-linq-to-sql-cs/_static/image8.png)](creating-model-classes-with-linq-to-sql-cs/_static/image7.png)

**图 03**： 表设计器 ([单击以查看实际尺寸的图像](creating-model-classes-with-linq-to-sql-cs/_static/image9.png))


我们需要将以下各列添加到我们的数据库表：

| **列名称** | **数据类型** | **允许 null 值** |
| --- | --- | --- |
| Id | Int | False |
| 标题 | Nvarchar （200) | False |
| 控制器 | nvarchar(50) | False |

你需要执行到 Id 列的两个特殊操作。 首先，你需要将 Id 列标记为主键列，通过在表设计器中选择列并单击项的图标。 LINQ to SQL 要求你在执行插入或更新对数据库时指定主键列。

接下来，你需要将 Id 列标记为标识列，通过是将值分配给**是标识**属性 （请参见图 3）。 标识列是自动将分配较新的数字，每当将新的数据行添加到表的列。

## <a name="create-linq-to-sql-classes"></a>创建 LINQ to SQL 类

我们的 MVC 模型将包含 LINQ to SQL 类表示 tblMovie 数据库表。 若要创建这些 LINQ to SQL 类的最简单方法是右键单击 Models 文件夹，选择**添加、 新项**，选择 LINQ to SQL 类模板，将类命名 Movie.dbml，然后单击**添加**按钮 （请参见图 4）。


[![创建 LINQ to SQL 类](creating-model-classes-with-linq-to-sql-cs/_static/image11.png)](creating-model-classes-with-linq-to-sql-cs/_static/image10.png)

**图 04**： 创建 LINQ to SQL 类 ([单击以查看实际尺寸的图像](creating-model-classes-with-linq-to-sql-cs/_static/image12.png))


立即创建电影 LINQ to SQL 类后，将显示对象关系设计器。 你可以将数据库表从服务器资源管理器窗口拖动对象关系设计器创建 LINQ to SQL 类表示特定数据库表。 我们需要添加 tblMovie 数据库表拖放到对象关系设计器 （请参见图 5）。


[![使用对象关系设计器](creating-model-classes-with-linq-to-sql-cs/_static/image14.png)](creating-model-classes-with-linq-to-sql-cs/_static/image13.png)

**图 05**： 使用对象关系设计器 ([单击以查看实际尺寸的图像](creating-model-classes-with-linq-to-sql-cs/_static/image15.png))


默认情况下，对象关系设计器创建一个具有类与数据库表拖到设计器上非常相同的名称。 但是，我们不想调用我们类`tblMovie`。 因此，单击设计器中的类的名称，并将类的名称更改为影片。

最后，请记住单击**保存**按钮 （软盘图片） 将 LINQ to SQL 类。 否则，不会由对象关系设计器生成 LINQ to SQL 类。

## <a name="using-linq-to-sql-in-a-controller-action"></a>在控制器操作中使用 LINQ to SQL

现在，我们已我们 LINQ to SQL 类，我们可以使用这些类以从数据库检索数据。 在本部分中，你将了解如何使用 LINQ to SQL 类直接中的控制器操作。 我们将在的 MVC 视图中显示 tblMovies 数据库表中的影片的列表。

首先，我们需要修改 HomeController 类。 你的应用程序的 Controllers 文件夹中找不到此类。 修改类，使其类似列表 1 中的类。

**列表 1 –`Controllers\HomeController.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample1.cs)]

`Index()`列表 1 中的操作使用 LINQ to SQL DataContext 类 ( `MovieDataContext`) 来表示`MoviesDB`数据库。 `MoveDataContext`由 Visual Studio 对象关系设计器生成类。

对 DataContext 以检索从电影的所有执行 LINQ 查询`tblMovies`数据库表。 影片列表分配到一个名为的本地变量`movies`。 最后，影片列表传递到视图查看数据。

为了显示电影，我们接下来需要修改索引视图。 你可以查找中的索引视图`Views\Home\`文件夹。 更新索引视图，以便它如下所示列出 2 中的视图。

**列出 2 –`Views\Home\Index.aspx`**

[!code-aspx[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample2.aspx)]

请注意，已修改的索引视图包括`<%@ import namespace %>`指令在视图的顶部。 此指令导入`MvcApplication1.Models namespace`。 我们需要此命名空间才能使用`model`类-具体而言，`Movie`类-在视图中的。

列出 2 中的视图包含`foreach`循环访问所有所表示的项的循环`ViewData.Model`属性。 值`Title`属性显示每个`movie`。

请注意，值`ViewData.Model`属性被强制转换为`IEnumerable`。 这是需循环访问的内容`ViewData.Model`。 此处的另一个选项是创建一个强类型`view`。 当你创建一个强类型`view`，您将转换`ViewData.Model`视图的代码隐藏类中的特定类型的属性。

如果运行修改后的应用程序`HomeController`类和索引视图，则会收到一个空白页。 你将获得一个空白页，因为在没有电影记录`tblMovies`数据库表。

才可添加到记录`tblMovies`数据库表中，右键单击`tblMovies`数据库 （在 Visual Web Developer 的数据库资源管理器窗口） 中的服务器资源管理器窗口中表并选择显示表数据菜单选项。 你可以将插入`movie`使用网格显示 （请参阅图 6） 的记录。


[![插入电影](creating-model-classes-with-linq-to-sql-cs/_static/image17.png)](creating-model-classes-with-linq-to-sql-cs/_static/image16.png)

**图 06**： 插入电影 ([单击以查看实际尺寸的图像](creating-model-classes-with-linq-to-sql-cs/_static/image18.png))


添加到某些数据库记录后`tblMovies`表和你运行应用程序，你将看到图 7 中的页。 项目符号列表中将显示所有影片数据库记录。


[![显示与索引视图的电影](creating-model-classes-with-linq-to-sql-cs/_static/image20.png)](creating-model-classes-with-linq-to-sql-cs/_static/image19.png)

**图 07**： 显示与索引视图的电影 ([单击以查看实际尺寸的图像](creating-model-classes-with-linq-to-sql-cs/_static/image21.png))


## <a name="using-the-repository-pattern"></a>使用存储库模式

在上一节中，我们使用 LINQ to SQL 类直接中的控制器操作。 我们使用`MovieDataContex`t 类直接从`Index()`控制器操作。 没有任何问题时执行此操作对于简单的应用程序。 但是，控制器类中直接使用 LINQ to SQL 的工作创建问题时你需要生成更复杂的应用程序。

在控制器类内使用 LINQ to SQL 难以将来切换数据访问技术。 例如，你可能决定可以从使用 Microsoft LINQ to SQL 将用作你的数据访问技术的 Microsoft 实体框架切换。 在这种情况下，你将需要重写访问该数据库在你的应用程序的每个控制器。

在控制器类内使用 LINQ to SQL 还使难以生成你的应用程序的单元测试。 通常情况下，你不想要执行单元测试时，与数据库交互。 你想要使用你的单元测试来测试你的应用程序逻辑和不是你的数据库服务器。

即以生成一个 MVC 应用程序更适应未来更改并要更轻松地测试，则应考虑使用存储库模式。 当你使用的存储库模式时，你将创建包含所有数据库访问逻辑的一个单独的存储库类。

当你创建存储库类时，你将创建一个接口来表示所有存储库类所使用的方法。 在你的控制器，你将编写针对接口而不是存储库代码。 这样一来，你可以实现在将来使用不同的数据访问技术的存储库。

中列出的 3 的接口名为`IMovieRepository`，它表示一个名为的单个方法`ListAll()`。

**列出 3 –`Models\IMovieRepository.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample3.cs)]

列出 4 中的存储库类实现`IMovieRepository`接口。 请注意，它包含一个名为方法`ListAll()`相对应的所需的方法`IMovieRepository`接口。

**列出 4 –`Models\MovieRepository.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample4.cs)]

最后，`MoviesController`列出 5 中的类使用的存储库模式。 它不再使用 LINQ to SQL 类直接。

**列出 5-`Controllers\MoviesController.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample5.cs)]

请注意，`MoviesController`中列出 5 类都具有两个构造函数。 第一个构造函数，无参数构造函数中，称为应用程序运行时。 此构造函数创建的实例`MovieRepository`类并将其传递给第二个构造函数。

第二个构造函数具有单个参数：`IMovieRepository`参数。 此构造函数只需将参数的值分配给名为的类级字段`_repository`。

`MoviesController`类正在调用的依赖关系注入模式软件设计模式的优点。 具体而言，它使用调用构造函数依赖关系注入。 你可以阅读更多有关通过阅读以下文章 Martin Fowler 通过此模式：

[http://martinfowler.com/articles/injection.html](http://martinfowler.com/articles/injection.html)

请注意，中的代码的所有`MoviesController`（不包括第一个构造函数） 的类与交互`IMovieRepository`而不是实际接口`MovieRepository`类。 代码而不是该接口的具体实现的抽象接口与交互。

如果你想要修改应用程序使用的数据访问技术，则你可以只是要实施`IMovieRepository`使用备用数据库访问技术的类接口。 例如，你可以创建`EntityFrameworkMovieRepository`类或`SubSonicMovieRepository`类。 由于针对接口进行编程控制器类，因此可以传递的新实现`IMovieRepository`到控制器类和类将继续工作。

此外，如果你想要测试`MoviesController`类，则可以将假的电影存储库类中，以便传递`HomeController`。 你可以实现`IMovieRepository`与不执行实际访问数据库，但包含所有所需的方法的类的类`IMovieRepository`接口。 这样一来，你可以将单元测试`MoviesController`不用真正访问实际的数据库的类。

## <a name="summary"></a>摘要

本教程的目标是演示如何利用 Microsoft LINQ to SQL 来创建 MVC 模型类。 有关 ASP.NET MVC 应用程序中显示数据库数据的两种策略，我们探讨。 首先，我们创建 LINQ to SQL 类，并使用直接中的控制器操作的类。 使用 LINQ to SQL 类控制器中，您可以快速并轻松地在 MVC 应用程序中显示数据库数据。

接下来，我们探讨了显示数据库数据的稍微有些困难，但肯定更良性的路径。 我们采用存储库模式的优点，并放在单独的存储库类中的所有我们数据库访问逻辑。 在我们控制器中，我们已写入所有我们针对而不是具体的类接口的代码。 存储库模式的优点是，它使我们能够轻松地在将来更改数据库访问技术，并且它允许我们能够轻松地测试我们控制器类。

>[!div class="step-by-step"]
[上一页](creating-model-classes-with-the-entity-framework-cs.md)
[下一页](displaying-a-table-of-database-data-cs.md)
