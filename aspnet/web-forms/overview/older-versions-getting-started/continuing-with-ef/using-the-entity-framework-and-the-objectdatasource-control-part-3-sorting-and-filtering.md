---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
title: "使用 Entity Framework 4.0 和 ObjectDataSource 控件，第 3 部分： 排序和筛选 |Microsoft 文档"
author: tdykstra
description: "本教程系列上的 Contoso 大学 web 应用程序创建的 Getting Started with 实体 Framework 4.0 教程系列生成。 我..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 2990bd10-590d-43d5-9529-6b503ce5455d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
msc.type: authoredcontent
ms.openlocfilehash: 4accd3381a66bde1f87f0dc7dd95beeb54fcc6a2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-3-sorting-and-filtering"></a>使用 Entity Framework 4.0 和 ObjectDataSource 控件，第 3 部分： 排序和筛选
====================
通过[Tom Dykstra](https://github.com/tdykstra)

> 本教程系列上的 Contoso 大学 web 应用程序创建的生成[Getting Started with 实体 Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started)教程系列。 如果未完成前面的教程，作为一个起始点本教程，你可以[下载应用程序](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a)，你应已创建。 你还可以[下载应用程序](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa)，它由完整教程系列创建。 如果你有疑问的教程，你可以发布到[ASP.NET 实体框架论坛](https://forums.asp.net/1227.aspx)。


在前面的教程中实现的 n 层 web 应用程序使用实体框架中的存储库模式和`ObjectDataSource`控件。 本教程演示如何执行排序和筛选和处理主-详细信息方案。 你将添加到以下增强功能*Departments.aspx*页：

- 若要允许用户按名称选择部门一个文本框。
- 课程，每个部门的网格中显示的列表。
- 能够通过单击列标题进行排序。

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)

## <a name="adding-the-ability-to-sort-gridview-columns"></a>将功能添加到 GridView 列排序

打开*Departments.aspx*页上，添加`SortParameterName="sortExpression"`属性设为`ObjectDataSource`控件名为`DepartmentsObjectDataSource`。 (稍后将创建`GetDepartments`采用名为的参数的方法`sortExpression`。)控件的开始标记的标记现在类似于下面的示例。

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample1.aspx)]

添加`AllowSorting="true"`属性设为的开始标记`GridView`控件。 控件的开始标记的标记现在类似于下面的示例。

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample2.aspx)]

在*Departments.aspx.cs*，通过调用设置默认的排序顺序`GridView`控件的`Sort`方法从`Page_Load`方法：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample3.cs)]

你可以在业务逻辑类或存储库类中添加对进行排序或筛选器的代码。 如果业务逻辑类以执行此操作，排序或筛选工作将会完成后从数据库中检索的数据因为业务逻辑类使用`IEnumerable`返回的存储库中的对象。 如果你添加排序和筛选存储库类中的代码和执行此操作之前 LINQ 表达式或对象查询已转换为`IEnumerable`对象，你的命令将传递到处理，这是通常效率更高的数据库。 在本教程中你将实施排序和筛选中引起操作由数据库执行的处理的方式 — 也就是说，存储库中。

若要添加排序功能，必须将新方法添加到存储库接口和存储库类以及业务逻辑类中。 在*ISchoolRepository.cs*文件中，添加一个新`GetDepartments`采用的方法`sortExpression`将用于返回的部门对列表进行排序的参数：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample4.cs)]

`sortExpression`参数将指定要作为排序依据的列以及排序方向。

添加到新的方法代码*SchoolRepository.cs*文件：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample5.cs)]

更改现有无参数`GetDepartments`方法调用新方法：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample6.cs)]

在测试项目中，添加以下新方法*MockSchoolRepository.cs*:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample7.cs)]

如果你已准备创建依赖于此方法返回的已排序的列表的任何单元测试，你需要将其返回之前对列表进行排序。 你不需要创建测试类似的在本教程中，因此该方法可以只返回的部门排序的列表。

在*SchoolBL.cs*文件中，将以下新方法添加到业务逻辑类：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample8.cs)]

此代码将排序参数传递给存储库方法。

运行*Departments.aspx*页。

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)

你现在可以单击任何列标题以按该列排序。 如果已排序的列，则单击标题将反转排序方向。

## <a name="adding-a-search-box"></a>添加搜索框

将在本部分中添加搜索文本框中，将其链接到`ObjectDataSource`控件使用控件参数，并将方法添加到业务逻辑类，以支持筛选。

打开*Departments.aspx*页上，添加以下标记之间标题和第一个`ObjectDataSource`控件：

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample9.aspx)]

在`ObjectDataSource`控件名为`DepartmentsObjectDataSource`，执行以下操作：

- 添加`SelectParameters`名为的参数的元素`nameSearchString`获取输入中的值`SearchTextBox`控件。
- 更改`SelectMethod`属性值到`GetDepartmentsByName`。 （你将创建此方法更高版本。）

标记`ObjectDataSource`控件现在类似于下面的示例：

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample10.aspx)]

在*ISchoolRepository.cs*，添加`GetDepartmentsByName`方法接受`sortExpression`和`nameSearchString`参数：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample11.cs)]

在*SchoolRepository.cs*，添加以下新方法：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample12.cs)]

此代码使用`Where`方法来选择包含搜索字符串的项。 如果搜索字符串为空，则将选择所有记录。 请注意，当你指定如下一个语句中一起调用的方法 (`Include`，然后`OrderBy`，然后`Where`)，则`Where`方法必须始终为最后一个。

更改现有`GetDepartments`采用的方法`sortExpression`参数来调用新方法：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample13.cs)]

在*MockSchoolRepository.cs*在测试项目中，添加以下新方法：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample14.cs)]

在*SchoolBL.cs*，添加以下新方法：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample15.cs)]

运行*Departments.aspx*页上，并输入搜索字符串来请确保选择逻辑正常工作。 将文本框留空，并重试搜索以确保返回的所有记录。

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)

## <a name="adding-a-details-column-for-each-grid-row"></a>为每个网格行中添加详细信息列

接下来，你想要查看所有在右边的单元格中的网格中显示每个部门的课程。 若要执行此操作，你将使用嵌套`GridView`控制和数据绑定到数据从`Courses`导航属性`Department`实体。

打开*Departments.aspx*和的标记中`GridView`控制，请指定处理程序为`RowDataBound`事件。 控件的开始标记的标记现在类似于下面的示例。

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample16.aspx)]

添加新`TemplateField`元素的后面`Administrator`模板字段：

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample17.aspx)]

此标记将创建嵌套`GridView`说明的课程数量和标题的课程的列表的控件。 而不指定数据源，因为你将数据绑定中的代码在`RowDataBound`处理程序。

打开*Departments.aspx.cs*并添加以下处理程序`RowDataBound`事件：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample18.cs)]

此代码获取`Department`实体从事件自变量，将转换`Courses`导航属性`List`集合和嵌套的 databinds`GridView`到集合。

打开*SchoolRepository.cs*文件，并指定为预先加载`Courses`导航属性，通过调用`Include`中创建的对象查询中的方法`GetDepartmentsByName`方法。 `return`中的语句`GetDepartmentsByName`方法现在类似于下面的示例。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample19.cs)]

运行页面。 排序和筛选你前面添加的功能，除了 GridView 控件现在显示为每个部门的嵌套的过程详细信息。

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)

这将完成排序、 筛选，和主 / 从方案简介。 在下一步的教程中，你将看到如何处理并发。

>[!div class="step-by-step"]
[上一页](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
[下一页](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)
