---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: "排序、 筛选和分页与实体框架中的 ASP.NET MVC 应用程序 (3 的 10) |Microsoft 文档"
author: tdykstra
description: "Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 5 Code First 和 Visual Studio 的 ASP.NET MVC 4 应用程序..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 8af630e0-fffa-4110-9eca-c96e201b2724
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: f9b68abeba19561a327bad5ee4be80d79af1a550
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application-3-of-10"></a>排序、 筛选和分页与实体框架中的 ASP.NET MVC 应用程序 (3 的 10)
====================
通过[Tom Dykstra](https://github.com/tdykstra)

[下载已完成的项目](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 5 Code First 和 Visual Studio 2012 的 ASP.NET MVC 4 应用程序。 有关教程系列的信息，请参阅[序列中的第一个教程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。 你可以从头开始教程系列或[下载这一章的初学者项目](building-the-ef5-mvc4-chapter-downloads.md)和从这里开始。
> 
> > [!NOTE] 
> > 
> > 如果你遇到无法解决的问题[下载已完成的章](building-the-ef5-mvc4-chapter-downloads.md)并尝试重现你的问题。 你通常可以通过比较你的代码已完成的代码会发现问题的解决方案。 一些常见的错误和如何解决它们，请参阅[错误和解决方法。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


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

你已添加`searchString`参数`Index`方法。 你亦已添加到 LINQ 语句`where`clausethat 选择仅学生的名字或姓氏包含搜索字符串。 从将添加到索引视图的文本框中接收到搜索字符串值。添加语句[其中](https://msdn.microsoft.com/library/bb535040.aspx)子句执行只有在要搜索的值。

> [!NOTE]
> 在许多情况下你可以调用相同的方法，在实体框架实体集或作为扩展方法对内存中集合。 结果通常都是相同，但在某些情况下可能会不同。 例如，.NET Framework 实现的`Contains`方法将返回所有行，将一个空字符串传递给它，而 SQL Server Compact 4.0 的实体框架提供程序都返回空字符串零行。 因此该示例中的代码 (将`Where`内的语句`if`语句) 可确保对于所有版本的 SQL Server 获得相同的结果。 此外，.NET Framework 实现的`Contains`方法默认情况下，执行区分大小写的比较，但在实体框架 SQL Server 提供程序默认情况下执行不区分大小写的比较。 因此，调用`ToUpper`来进行测试显式不区分大小写的方法可确保在更改代码更高版本以使用一个存储库，它将返回时，结果不会发生更改`IEnumerable`而不是集合`IQueryable`对象。 (当你调用`Contains`方法`IEnumerable`集合，你将获取.NET Framework 实现; 当上调用它`IQueryable`对象，则会出现的数据库提供程序实现。)


### <a name="add-a-search-box-to-the-student-index-view"></a>将一个搜索框添加到学生索引视图

在*Views\Student\Index.cshtml*，添加突出显示的代码在打开之前立即`table`才能创建标题，文本框中，标记和**搜索**按钮。

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=5-10)]

运行页面，输入搜索字符串，然后单击**搜索**以验证筛选是否正常工作。

![Students_Index_page_with_search_box](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

请注意 URL 不包含""搜索字符串，这意味着，如果此页加入书签，你不会获得的筛选的列表，当你使用书签。 你将更改**搜索**按钮以更高版本在本教程中使用 for 筛选条件的查询字符串。

## <a name="add-paging-to-the-students-index-page"></a>将分页添加到学生索引页

若要将分页添加到学生索引页，你将首先安装**PagedList.Mvc** NuGet 包。 然后你将使中的其他更改`Index`方法和分页将链接添加到`Index`视图。 **PagedList.Mvc**是许多良好分页和 ASP.NET MVC 的排序包之一，它在本文中的使用应仅作为示例，不是相对于其他选择为其建议。 下图显示分页链接。

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

### <a name="install-the-pagedlistmvc-nuget-package"></a>安装 PagedList.MVC NuGet 包

NuGet **PagedList.Mvc**包会自动安装**PagedList**作为依赖项包。 **PagedList**包安装`PagedList`集合类型和扩展方法`IQueryable`和`IEnumerable`集合。 扩展方法创建一个页面中的数据中`PagedList`集合外的你`IQueryable`或`IEnumerable`，和`PagedList`集合提供了一些属性和方法，以方便分页。 **PagedList.Mvc**程序包将安装的分页的帮助程序显示分页按钮。

从**工具**菜单上，选择**库程序包管理器**然后**管理解决方案的 NuGet 包**。

在**管理 NuGet 包**对话框中，单击**联机**在左侧选项卡，然后在搜索框中输入"分页"。 当你看到**PagedList.Mvc**包、 单击**安装**。

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

在**选择项目**框中，单击**确定**。

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

### <a name="add-paging-functionality-to-the-index-method"></a>将分页功能添加到索引方法

在*Controllers\StudentController.cs*，添加`using`语句`PagedList`命名空间：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

将 `Index` 方法替换为以下代码：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

此代码将添加`page`参数、 当前的排序顺序参数和当前筛选器参数传递给方法的签名，如下所示：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

第一次显示页面，或如果用户未单击分页或排序链接，则所有参数将都为 null。 如果单击的分页链接，`page`变量将包含要显示的页码。

`A ViewBag`属性提供的视图具有当前的排序顺序，因为这必须包含在分页链接以保持排序顺序分页时相同：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

另一个属性， `ViewBag.CurrentFilter`，与当前的筛选器字符串中提供的视图。 若要分页，过程维护的筛选器设置情况下，此值必须包括在分页链接，必须还原到文本框中时重新显示页。 如果分页期间更改搜索字符串，页将显示被重置为 1，因为新的筛选器可能会导致不同的数据，以显示。 在文本框中输入了值，按下提交按钮，则会更改搜索字符串。 在这种情况下，`searchString`参数不为 null。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

该方法的末尾`ToPagedList`上学生的扩展方法`IQueryable`对象将学生查询转换为支持分页的集合类型中的学生的单页。 学生的单个页面然后传递给视图：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

`ToPagedList`方法采用页号。 两个问号表示[null 合并运算符](https://msdn.microsoft.com/library/ms173224.aspx)。 Null 合并运算符定义可以为 null 的类型; 默认值表达式`(page ?? 1)`意味着返回的值`page`如果它具有一个值，或返回 1，如果`page`为 null。

### <a name="add-paging-links-to-the-student-index-view"></a>将分页链接添加到学生索引视图

在*Views\Student\Index.cshtml*，将现有代码替换为以下代码：

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=6,9,14-20,56-58)]

`@model`页顶部的语句指定视图现在获取`PagedList`对象而不是`List`对象。

`using`语句`PagedList.Mvc`分页按钮访问的 MVC 帮助程序。

该代码使用的重载[BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) ，它指定允许[FormMethod.Get](https://msdn.microsoft.com/library/system.web.mvc.formmethod(v=vs.100).aspx/css)。

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

默认值[BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx)提交表单 post，这意味着，参数将在 HTTP 消息正文中，不能在 URL 作为查询字符串传递的数据。 指定 HTTP GET 时，窗体数据是在 URL 中作为查询字符串传递，这使得用户能够创建 URL 的书签。 [使用 HTTP GET 的 W3C 准则](http://www.w3.org/2001/tag/doc/whenToUseGet.html)指定当操作未导致更新时应使用 GET。

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

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

单击以确保分页工作原理的不同的排序顺序中的分页链接。 然后输入搜索字符串，然后重试以验证分页还适用正确使用排序和筛选的分页。

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="create-an-about-page-that-shows-student-statistics"></a>创建有关显示学生统计信息的页面

Contoso 大学网站的有关页面，将显示如何很多学生已注册的每个注册日期。 这要求在组上的分组和简单计算。 要完成此操作，将执行以下操作：

- 创建一个视图模型类，你需要将传递到该视图的数据。
- 修改`About`中的方法`Home`控制器。
- 修改`About`视图。

### <a name="create-the-view-model"></a>创建视图模型

创建*Viewmodel*文件夹。 在该文件夹中，将类文件添加*EnrollmentDateGroup.cs*和替换为以下代码替换现有代码：

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

![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

## <a name="optional-deploy-the-app-to-windows-azure"></a>可选： 将应用部署到 Windows Azure

到目前为止您的应用程序已经运行本地 IIS Express 在开发计算机上。 若要使其可供其他人通过 Internet 使用，必须将其部署到 web 宿主提供程序。 在本教程的这个可选部分将将其部署到 Windows Azure 网站。

### <a name="using-code-first-migrations-to-deploy-the-database"></a>使用 Code First 迁移将数据库部署

若要将数据库部署将使用 Code First 迁移。 在创建使用从 Visual Studio 部署的配置设置的发布配置文件时，你将选择一个复选框，标记为**执行 Code First 迁移 （应用程序启动时运行）**。 此设置会导致自动配置应用程序的部署过程*Web.config*文件在目标服务器上，以便代码优先使用`MigrateDatabaseToLatestVersion`初始值设定项类。

Visual Studio 不执行任何与数据库在部署过程。 当部署的应用程序部署后首次访问数据库时，Code First 自动创建数据库或数据库架构更新到最新版本。 如果应用程序实施迁移`Seed`方法，创建数据库或架构更新后的方法运行。

迁移过程`Seed`方法插入测试数据。 如果你在部署到生产环境，你将必须更改`Seed`方法，以便它只会将你想要插入到你的生产数据库的数据。 例如，当前数据模型中你可能想要开发数据库中具有真实课程但虚构学生。 你可以编写`Seed`加载同时在开发中，并在部署到生产环境之前然后注释虚构学生方法。 也可以编写`Seed`加载仅课程，并使用应用程序的用户界面手动测试数据库中输入虚构学生方法。

### <a name="get-a-windows-azure-account"></a>获取 Windows Azure 帐户

你将需要 Windows Azure 帐户。 如果你还没有一个，可以在几分钟内创建一个免费试用帐户。 有关详细信息，请参阅[Windows Azure 免费试用版](https://azure.microsoft.com/free/?WT.mc_id=A443DD604)。

### <a name="create-a-web-site-and-a-sql-database-in-windows-azure"></a>在 Windows Azure 中创建网站和 SQL 数据库

Windows Azure 网站将运行在共享宿主环境中，这意味着它与其他 Windows Azure 客户端共享的虚拟机 (Vm) 上运行。 共享宿主环境是在云中开始低成本方法。 更高版本，如果你的 web 流量增加，应用程序可以缩放以满足需要通过在专用 Vm 上运行。 如果你需要更复杂的体系结构，你可以将其迁移到 Windows Azure 云服务。 在你可以根据需要配置的专用 Vm 上运行云服务。

Windows Azure SQL 数据库是根据 SQL Server 技术构建的基于云的关系数据库服务。 工具和应用程序使用 SQL Server 还处理 SQL 数据库。

1. 在[Windows Azure 管理门户](https://manage.windowsazure.com/)，单击**网站**在左侧选项卡，然后单击**新建**。

    ![在管理门户中的新按钮](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)
2. 单击**自定义创建**。

    ![创建包含在管理门户中的数据库链接](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

 **新建网站-自定义创建**向导随即打开。
3. 在**新网站**步骤的向导中，输入中的字符串**URL**框，以用作你的应用程序的唯一 URL。 完整的 URL 将包含的你在此处输入和文本框旁边看到的后缀。 图中显示"ConU"，但该 URL 是可能因此必须另外选择一个。

    ![创建包含在管理门户中的数据库链接](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)
4. 在**区域**下拉列表中，选择靠近你的区域。 此设置指定您的网站将运行在哪个数据中心。
5. 在**数据库**下拉列表中，选择**创建免费的 20 MB SQL 数据库**。

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)
6. 在**数据库连接字符串名称**，输入*SchoolContext*。

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)
7. 单击框底部右侧箭头。 向导将前进到**数据库设置**步骤。
8. 在**名称**框中，输入*ContosoUniversityDB*。
9. 在**服务器**框中，选择**新建 SQL 数据库服务器**。 或者，如果你以前创建的服务器，你可以从下拉列表中选择该服务器。
10. 输入管理员**登录名**和**密码**。 如果你选择**新建 SQL 数据库服务器**不要输入现有名称和密码在此处，你应输入新名称和密码，你现在定义以后访问数据库时使用。 如果你选择你之前创建的服务器，你将为该服务器输入凭据。 对于本教程，你将不会选择***高级***复选框。 ***高级***选项使您可以将数据库设置[排序规则](https://msdn.microsoft.com/library/aa174903(v=SQL.80).aspx)。
11. 选择相同**区域**与你为网站选择。
12. 单击右下角的框以指示你已完成的复选标记。   
  
    ![数据库设置步骤使用数据库向导创建的新网站-](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image16.png)  

 下图显示了使用现有 SQL Server 和登录名。   
  
    ![数据库设置步骤使用数据库向导创建的新网站-](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image17.png)  
  
 管理门户返回到网站页中，与**状态**列显示正在创建网站。 （通常小于一分钟），一段时间后**状态**列显示已成功创建网站。 在左侧导航栏中，必须在你的帐户中的站点数旁边将出现**网站**旁边显示图标和数据库的数目**SQL 数据库**图标。

## <a name="deploy-the-application-to-windows-azure"></a>部署到 Windows Azure 应用程序

1. 在 Visual Studio 中，右键单击中的项目**解决方案资源管理器**和选择**发布**从上下文菜单。  
  
    ![在项目上下文菜单中发布](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image18.png)
2. 在**配置文件**选项卡**发布 Web**向导中，单击**导入**。  
  
    ![导入发布设置](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image19.png)
3. 如果之前未添加 Windows Azure 订阅在 Visual Studio 中，执行以下步骤。 在这些步骤中添加你的订阅，以便下拉列表下**从 Windows Azure 网站导入**将包括您的网站。

    a. 在**导入发布配置文件**对话框中，单击**从 Windows Azure 网站导入**，然后单击**添加 Windows Azure 订阅**。

    ![添加 Windows Azure 订阅](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image20.png)

    b. 在**导入 Windows Azure 订阅**对话框中，单击**下载订阅文件**。

    ![下载订阅文件](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image21.png)

    c. 在浏览器窗口中，保存*.publishsettings*文件。

    ![下载的.publishsettings 文件](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image22.png)

    > [!WARNING]
    > 安全- *publishsettings*文件包含你用于管理你的 Windows Azure 订阅和服务的凭据 （未编码）。 此文件的最佳安全方案是将其暂时存储源目录以外 (例如在*Libraries\Documents*文件夹)，然后将其导入完毕后删除。 恶意用户获得访问权`.publishsettings`文件可以编辑、 创建和删除 Windows Azure 服务。

    d. 在**导入 Windows Azure 订阅**对话框中，单击**浏览**并导航到*.publishsettings*文件。

    ![下载订阅](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image23.png)

    e. 单击“导入” 。

    ![import](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image24.png)
4. 在**导入发布配置文件**对话框中，选择**从 Windows Azure 网站导入**，从下拉列表中，选择您的网站，然后单击**确定**。  
  
    ![导入发布配置文件](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image25.png)
5. 在**连接**选项卡上，单击**验证连接**若要确保设置正确无误。  
  
    ![验证连接](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image26.png)
6. 在验证连接后，旁边出现一个绿色的复选标记**验证连接**按钮。 单击 **“下一步”**。  
  
    ![已成功验证的连接](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image27.png)
7. 打开**远程连接字符串**下的下拉列表**SchoolContext** ，然后选择你创建的数据库的连接字符串。
8. 选择**执行 Code First 迁移 （应用程序启动时运行）**。
9. 取消选中**在运行时使用此连接字符串**为**上下文 (DefaultConnection)**，因为此应用程序不使用成员资格数据库。   
  
    ![设置选项卡](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image28.png)
10. 单击 **“下一步”**。
11. 在**预览**选项卡上，单击**开始预览**。  
  
    ![预览选项卡中的开始预览按钮](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image29.png)  
  
 选项卡显示将复制到服务器的文件列表。 显示预览不需要发布应用程序但有用的功能，需要注意。 在这种情况下，你不必使用显示的文件列表执行任何操作。 下次部署此应用程序，仅将已更改的文件将在此列表中。  
  
    ![开始预览文件输出](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image30.png)
12. 单击“发布” 。  
 Visual Studio 开始将文件复制到 Windows Azure 服务器的过程。
13. **输出**窗口将显示已执行的部署操作并报告已成功完成的部署。  
  
    ![输出窗口报告部署成功](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image31.png)
14. 成功部署后，默认浏览器自动打开指向已部署的 web 站点的 URL。  
 你创建的应用程序现在在云中运行。 单击学生选项卡。  
  
    ![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image32.png)

此时你*SchoolContext*已在 Windows Azure SQL 数据库中创建了数据库由于你选择了**执行 Code First 迁移 （在应用启动时运行）**。 *Web.config*已部署的 web 站点中的文件已更改，以便[MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx)初始值设定项将运行你的代码读取或写入数据库中的数据的第一个时间 (所选时发生**学生**选项卡上):

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image33.png)

在部署过程还创建新的连接字符串*(SchoolContext\_DatabasePublish*) 的代码优先迁移来用于更新数据库架构和数据库进行种子设定。

![Database_Publish 连接字符串](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image34.png)

*DefaultConnection*连接字符串用于成员资格数据库 （其中我们不使用在本教程中）。 *SchoolContext* ContosoUniversity 数据库是连接字符串。

您可以找到在你自己的计算机上的 Web.config 文件的已部署的版本*ContosoUniversity\obj\Release\Package\PackageTmp\Web.config*。你可以访问部署*Web.config*文件本身通过使用 FTP。 有关说明，请参阅[使用 Visual Studio 的 ASP.NET Web 部署： 部署某一代码更新](../../../../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md)。 按照以开头的说明"若要使用 FTP 工具，需要以下三项： 的 FTP URL、 用户名和密码。"

> [!NOTE]
> Web 应用未实现安全，因此任何找到的 URL 的人都可以更改的数据。 有关如何确保网站的安全的说明，请参阅[将包含成员资格、 OAuth 和 SQL 数据库的安全 ASP.NET MVC 应用程序部署到 Windows Azure 网站](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)。 可以通过使用 Windows Azure 管理门户中使用该站点中阻止其他人或**服务器资源管理器**Visual Studio 停止该站点中。


![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image35.png)

## <a name="code-first-initializers"></a>代码 First 初始值设定项

在部署部分你已看到[MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx)正在使用的初始值设定项。 代码首先还提供您可以使用，其中包括其他初始值设定项[CreateDatabaseIfNotExists](https://msdn.microsoft.com/library/gg679221(v=vs.103).aspx) （默认值）、 [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=VS.103).aspx)和[DropCreateDatabaseAlways](https://msdn.microsoft.com/library/gg679506(v=VS.103).aspx)。 `DropCreateAlways`初始值设定项可用于设置单元测试的条件。 你还可以编写你自己初始值设定项，并且如果不想等待，直到应用程序从读取或写入数据库，你可以显式调用初始值设定项。 初始值设定项的完整说明，请参阅书籍的第 6 章[编程实体框架： Code First](http://shop.oreilly.com/product/0636920022220.do) Julie Lerman 和 Rowan Miller。

## <a name="summary"></a>摘要

在本教程中，你已了解如何创建数据模型和实现基本的 CRUD，排序、 筛选、 分页和分组功能。 在下一教程将首先通过展开数据模型来查看更高级的主题。

在找不到其他实体框架资源的链接[ASP.NET 数据访问内容映射](../../../../whitepapers/aspnet-data-access-content-map.md)。

>[!div class="step-by-step"]
[上一页](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
[下一页](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
