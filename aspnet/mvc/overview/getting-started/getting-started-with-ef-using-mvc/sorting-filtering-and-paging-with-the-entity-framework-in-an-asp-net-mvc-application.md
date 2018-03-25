---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: "排序、 筛选和分页与实体框架中的 ASP.NET MVC 应用程序 |Microsoft 文档"
author: tdykstra
description: "Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 6 Code First 和 Visual Studio 的 ASP.NET MVC 5 应用程序..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/01/2015
ms.topic: article
ms.assetid: d5723e46-41fe-4d09-850a-e03b9e285bfa
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: d54c0e133bc2f6f2021821dc16cdf86cc23a5667
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application"></a>排序、 筛选和分页与实体框架中的 ASP.NET MVC 应用程序
====================
通过[Tom Dykstra](https://github.com/tdykstra)

[下载已完成的项目](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)或[下载 PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 6 Code First 和 Visual Studio 2013 的 ASP.NET MVC 5 应用程序。 有关教程系列的信息，请参阅[序列中的第一个教程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。


在前面的教程中实现一组有关的基本 CRUD 操作的 web 页面`Student`实体。 在本教程中你将添加排序、 筛选和分页功能**学生**索引页。 你还将创建简单分组的页。

下图显示哪些页面将如下所示完成后。 列标题是用户可以单击以按该列排序的链接。 单击列标题反复将在升序和降序之间切换。

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a>将列排序链接添加到学生索引页

若要添加排序学生索引页，你将更改`Index`方法`Student`控制器并将代码添加到`Student`索引视图。

### <a name="add-sorting-functionality-to-the-index-method"></a>添加排序功能向 Index 方法

在*Controllers\StudentController.cs*，替换`Index`方法替换为以下代码：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

此代码接收`sortOrder`从 URL 中的查询字符串参数。 ASP.NET MVC 提供查询字符串值作为参数传递给的操作方法。 参数将为"Name"日期"，可以选择后跟下划线和字符串"desc"指定降序顺序的字符串。 默认排序顺序为升序。

第一次请求索引页时，没有任何查询字符串。 升序排序依据显示学生`LastName`，这是默认设置，如回退通过用例中建立`switch`语句。 当用户单击列标题超链接，相应`sortOrder`查询字符串中提供值。

这两个`ViewBag`变量使用，以便该视图可以配置使用相应的查询字符串值的列标题超链接：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

这些是三元语句。 第一个指定，如果`sortOrder`参数为 null 或为空，`ViewBag.NameSortParm`应设置为"名称\_desc"; 否则为它应设置为一个空字符串。 这两个语句启用要设置列标题的超链接，如下所示的视图：

| 当前的排序顺序 | 最后一个名称超链接 | 日期超链接 |
| --- | --- | --- |
| 上次名称升序排列 | descending | ascending |
| 上次名称降序 | ascending | ascending |
| 升序的日期 | ascending | descending |
| 日期降序 | ascending | ascending |

该方法使用[LINQ to Entities](https://msdn.microsoft.com/library/bb386964.aspx)指定要作为排序依据的列。 该代码创建[IQueryable](https://msdn.microsoft.com/library/bb351562.aspx)变量之前`switch`语句，将修改在`switch`语句，并调用`ToList`方法之后`switch`语句。 当你创建和修改`IQueryable`变量，没有查询发送到数据库。 您将转换之前，不执行查询`IQueryable`到通过调用方法，如集合对象`ToList`。 因此，此代码将导致直到不执行单个查询`return View`语句。

作为编写为每个排序顺序不同 LINQ 语句的替代方法，你可以动态创建 LINQ 语句。 有关动态 LINQ 的信息，请参阅[动态 LINQ](https://go.microsoft.com/fwlink/?LinkID=323957)。

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>添加列标题的学生索引视图的超链接

在*Views\Student\Index.cshtml*，替换`<tr>`和`<th>`具有突出显示的代码的标题行的元素：

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

此代码使用中的信息`ViewBag`属性设置超链接与相应的查询字符串值。

运行页面并单击**姓氏**和**注册日期**列标题，以验证该排序工作原理。

![Students_Index_page_with_sort_hyperlinks](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

单击后**姓氏**标题下，按降序姓氏的顺序显示学生。

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="add-a-search-box-to-the-students-index-page"></a>将一个搜索框添加到学生索引页

若要添加到学生索引页过滤条件，将向视图添加一个文本框和提交按钮，并将中的相应更改`Index`方法。 文本框中，你将输入要在名字和姓氏字段中搜索的字符串。

### <a name="add-filtering-functionality-to-the-index-method"></a>将筛选功能添加到索引方法

在*Controllers\StudentController.cs*，替换`Index`方法替换为以下代码 （突出显示所做的更改）：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

你已添加`searchString`参数`Index`方法。 从将添加到索引视图的文本框中接收到搜索字符串值。 你亦已添加到 LINQ 语句`where`选择仅学生的名字或姓氏包含搜索字符串的子句。 添加语句[其中](https://msdn.microsoft.com/library/bb535040.aspx)子句执行只有在要搜索的值。

> [!NOTE]
> 在许多情况下你可以调用相同的方法，在实体框架实体集或作为扩展方法对内存中集合。 结果通常都是相同，但在某些情况下可能会不同。
> 
> 例如，.NET Framework 实现的`Contains`方法将返回所有行，将一个空字符串传递给它，而 SQL Server Compact 4.0 的实体框架提供程序都返回空字符串零行。 因此该示例中的代码 (将`Where`内的语句`if`语句) 可确保对于所有版本的 SQL Server 获得相同的结果。 此外，.NET Framework 实现的`Contains`方法默认情况下，执行区分大小写的比较，但在实体框架 SQL Server 提供程序默认情况下执行不区分大小写的比较。 因此，调用`ToUpper`来进行测试显式不区分大小写的方法可确保在更改代码更高版本以使用一个存储库，它将返回时，结果不会发生更改`IEnumerable`而不是集合`IQueryable`对象。 (当你调用`Contains`方法`IEnumerable`集合，你将获取.NET Framework 实现; 当上调用它`IQueryable`对象，则会出现的数据库提供程序实现。)
> 
> Null 处理可能也会不同的数据库提供程序，或者在使用不同`IQueryable`使用时，对象相比`IEnumerable`集合。 例如，在某些情况下`Where`如条件`table.Column != 0`可能不会返回具有列`null`作为值。 有关详细信息，请参阅['where' 子句中的 null 变量的不正确处理](https://data.uservoice.com/forums/72025-entity-framework-feature-suggestions/suggestions/1015361-incorrect-handling-of-null-variables-in-where-cl)。


### <a name="add-a-search-box-to-the-student-index-view"></a>将一个搜索框添加到学生索引视图

在*Views\Student\Index.cshtml*，添加突出显示的代码在打开之前立即`table`才能创建标题，文本框中，标记和**搜索**按钮。

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=5-10)]

运行页面，输入搜索字符串，然后单击**搜索**以验证筛选是否正常工作。

![Students_Index_page_with_search_box](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

请注意 URL 不包含""搜索字符串，这意味着，如果此页加入书签，你不会获得的筛选的列表，当你使用书签。 这还适用于列的排序链接，因为它们将对整个列表进行排序。 你将更改**搜索**按钮以更高版本在本教程中使用 for 筛选条件的查询字符串。

## <a name="add-paging-to-the-students-index-page"></a>将分页添加到学生索引页

若要将分页添加到学生索引页，你将首先安装**PagedList.Mvc** NuGet 包。 然后你将使中的其他更改`Index`方法和分页将链接添加到`Index`视图。 **PagedList.Mvc**是许多良好分页和 ASP.NET MVC 的排序包之一，它在本文中的使用应仅作为示例，不是相对于其他选择为其建议。 下图显示分页链接。

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

### <a name="install-the-pagedlistmvc-nuget-package"></a>安装 PagedList.MVC NuGet 包

NuGet **PagedList.Mvc**包会自动安装**PagedList**作为依赖项包。 **PagedList**包安装`PagedList`集合类型和扩展方法`IQueryable`和`IEnumerable`集合。 扩展方法创建一个页面中的数据中`PagedList`集合外的你`IQueryable`或`IEnumerable`，和`PagedList`集合提供了一些属性和方法，以方便分页。 **PagedList.Mvc**程序包将安装的分页的帮助程序显示分页按钮。

从**工具**菜单上，选择**库程序包管理器**然后**程序包管理器控制台**。

在**程序包管理器控制台**窗口中，请确保**包源**是**nuget.org**和**默认项目**是**ContosoUniversity**，然后输入以下命令：

`Install-Package PagedList.Mvc`

![Install PagedList.Mvc](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

生成项目。 

### <a name="add-paging-functionality-to-the-index-method"></a>将分页功能添加到索引方法

在*Controllers\StudentController.cs*，添加`using`语句`PagedList`命名空间：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

将 `Index` 方法替换为以下代码：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=1,3,7-16,41-43)]

此代码将添加`page`参数、 当前的排序顺序参数和向方法签名的当前筛选器参数：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

第一次显示页面，或如果用户未单击分页或排序链接，则所有参数将都为 null。 如果单击的分页链接，`page`变量将包含要显示的页码。

A`ViewBag`属性提供了与当前的排序顺序中，视图，因为这必须包含在分页链接以保持排序顺序分页时相同：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

另一个属性， `ViewBag.CurrentFilter`，与当前的筛选器字符串中提供的视图。 若要分页，过程维护的筛选器设置情况下，此值必须包括在分页链接，必须还原到文本框中时重新显示页。 如果分页期间更改搜索字符串，页将显示被重置为 1，因为新的筛选器可能会导致不同的数据，以显示。 在文本框中输入了值，按下提交按钮，则会更改搜索字符串。 在这种情况下，`searchString`参数不为 null。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

该方法的末尾`ToPagedList`上学生的扩展方法`IQueryable`对象将学生查询转换为支持分页的集合类型中的学生的单页。 学生的单个页面然后传递给视图：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

`ToPagedList`方法采用页号。 两个问号表示[null 合并运算符](https://msdn.microsoft.com/library/ms173224.aspx)。 Null 合并运算符定义可以为 null 的类型; 默认值表达式`(page ?? 1)`意味着返回的值`page`如果它具有一个值，或返回 1，如果`page`为 null。

### <a name="add-paging-links-to-the-student-index-view"></a>将分页链接添加到学生索引视图

在*Views\Student\Index.cshtml*，替换为以下代码替换现有代码。 突出显示所做的更改。

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=1-3,6,9,14,17,24,30,55-56,58-59)]

`@model`页顶部的语句指定视图现在获取`PagedList`对象而不是`List`对象。

`using`语句`PagedList.Mvc`分页按钮访问的 MVC 帮助程序。

该代码使用的重载[BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) ，它指定允许[FormMethod.Get](https://msdn.microsoft.com/library/system.web.mvc.formmethod(v=vs.100).aspx/css)。

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

默认值[BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx)提交表单 post，这意味着，参数将在 HTTP 消息正文中，不能在 URL 作为查询字符串传递的数据。 指定 HTTP GET 时，窗体数据是在 URL 中作为查询字符串传递，这使得用户能够创建 URL 的书签。 [使用 HTTP GET 的 W3C 准则](http://www.w3.org/2001/tag/doc/whenToUseGet.html)建议的操作未导致更新时，你应使用 GET。

因此，当你单击一个新页你可以看到当前的搜索字符串，将使用当前的搜索字符串初始化文本框。

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml?highlight=1)]

列标题链接使用查询字符串将当前的搜索字符串传递到控制器，以便用户可以在筛选器的结果中进行排序：

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1)]

显示当前页和总页数。

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]

如果不没有显示任何页，将显示"页 0 0"。 (在这种情况下页号大于页计数因为`Model.PageNumber`为 1，和`Model.PageCount`为 0。)

分页按钮显示通过`PagedListPager`帮助器：

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

`PagedListPager`帮助器提供了多种你可以自定义，包括 Url 和样式的选项。 有关详细信息，请参阅[TroyGoode / PagedList](https://github.com/TroyGoode/PagedList) GitHub 站点上。

运行页面。

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

单击以确保分页工作原理的不同的排序顺序中的分页链接。 然后输入搜索字符串，然后重试以验证分页还适用正确使用排序和筛选的分页。

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

## <a name="create-an-about-page-that-shows-student-statistics"></a>创建有关显示学生统计信息的页面

Contoso 大学网站的有关页面，将显示如何很多学生已注册的每个注册日期。 这要求在组上的分组和简单计算。 要完成此操作，将执行以下操作：

- 创建一个视图模型类，你需要将传递到该视图的数据。
- 修改`About`中的方法`Home`控制器。
- 修改`About`视图。

### <a name="create-the-view-model"></a>创建视图模型

创建*Viewmodel*项目文件夹中的文件夹。 在该文件夹中，将类文件添加*EnrollmentDateGroup.cs*和模板代码替换为以下代码：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a>修改主控制器

在*HomeController.cs*，添加以下`using`语句文件的顶部：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

类的左大括号后立即添加的数据库上下文的类变量：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

将 `About` 方法替换为以下代码：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

LINQ 语句学生实体分组按注册日期，计算每个组中的实体数并将结果存储在集合中的`EnrollmentDateGroup`查看模型对象。

添加`Dispose`方法：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a>修改有关视图

中的代码替换*Views\Home\About.cshtml*文件替换为以下代码：

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

运行应用并单击**有关**链接。 每个注册日期的学生计数显示在表格中。

![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="summary"></a>摘要

在本教程中，你已了解如何创建数据模型和实现基本的 CRUD，排序、 筛选、 分页和分组功能。 在下一教程将首先通过展开数据模型来查看更高级的主题。

请在如何喜欢本教程的方式，我们可以提高上，留下反馈。 你还可以请求新主题[教我编写代码](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code)。

在找不到其他实体框架资源的链接[ASP.NET 数据访问的推荐资源](../../../../whitepapers/aspnet-data-access-content-map.md)。

>[!div class="step-by-step"]
[上一页](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
[下一页](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)
