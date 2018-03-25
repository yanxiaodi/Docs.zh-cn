---
uid: web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
title: "排序、 分页和筛选使用模型的绑定和 web 窗体的数据 |Microsoft 文档"
author: tfitzmac
description: "本系列教程演示使用模型绑定的 ASP.NET Web 窗体项目的基本方面。 模型绑定使数据交互详细直接-..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 266e7866-e327-4687-b29d-627a0925e87d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
msc.type: authoredcontent
ms.openlocfilehash: 94fc84533be5fcbcf0612fcdcabea7dee738d89b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="sorting-paging-and-filtering-data-with-model-binding-and-web-forms"></a>排序、 分页和筛选使用模型的绑定和 web 窗体的数据
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> 本系列教程演示使用模型绑定的 ASP.NET Web 窗体项目的基本方面。 模型绑定使数据交互更直接的比处理数据源对象 （如 ObjectDataSource 或 SqlDataSource）。 这一系列开头介绍性材料，更高版本的教程中移动到更高级的概念。
> 
> 本教程演示如何添加排序、 分页和筛选数据，通过模型绑定。
> 
> 本教程基于第一个中创建的项目[一部分](retrieving-data.md)的序列。
> 
> 你可以[下载](https://go.microsoft.com/fwlink/?LinkId=286116)在 C# 或 VB.完整项目 可下载的代码适用于 Visual Studio 2012 或 Visual Studio 2013。 它使用 Visual Studio 2012 模板，这是本教程中所示的 Visual Studio 2013 模板过程稍有不同。


## <a name="what-youll-build"></a>你将生成

在本教程中，你将：

1. 启用排序和分页的数据
2. 启用筛选数据，根据用户的选择

## <a name="add-sorting"></a>添加排序

启用在 GridView 中排序是非常简单。 在 Student.aspx 文件中，只需设置**AllowSorting**到**true**中 GridView。 不需要设置**SortExpression** DataField 会自动使用值为每个列。 GridView 修改查询以包含所选值通过对数据进行排序。 突出显示的以下代码显示如何添加需要进行启用排序。

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample1.aspx?highlight=5)]

运行 web 应用程序，并测试排序学生记录不同的列中的值。

![排序学生版](sorting-paging-and-filtering-data/_static/image2.png)

## <a name="add-paging"></a>将分页添加

启用分页也是非常简单的。 在 GridView，集中**AllowPaging**属性**true**并设置**PageSize**属性设置为你想要在每个页面上显示的记录数。 在本教程中，可以将其设置为 4。

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample2.aspx?highlight=5)]

运行 web 应用程序，并请注意，现在不能超过 4 记录在单个页面上显示多个页通过划分记录。

![将分页添加](sorting-paging-and-filtering-data/_static/image4.png)

延迟的执行查询提高了应用程序效率。 而不是检索整个数据集，GridView 修改查询以检索仅针对当前页的记录。

## <a name="filter-records-by-user-selection"></a>按用户选择筛选记录

模型绑定将添加多个特性使你可以指定如何在模型绑定方法中设置参数的值。 这些特性是在**System.Web.ModelBinding**命名空间。 它们包括：

- 控件
- Cookie
- 窗体
- 配置文件
- QueryString
- RouteData
- 会话
- 用户配置文件
- 视图状态

在本教程中，你将使用控件的值来筛选的记录会显示在 GridView。 你将添加**控件**属性设为你创建了前面的查询方法。 在[更高版本](using-query-string-values-to-retrieve-data.md)教程中，你将应用**QueryString**属性设为参数来指定参数值来自查询字符串值。

首先，ValidationSummary，上面添加的下拉列表进行筛选的列表显示哪些学生。

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample3.aspx?highlight=3-11)]

在代码隐藏文件中，修改选择的方法，以接收值，通过控件，并将参数的名称为提供的值的控件的名称。

你必须添加**使用**语句**System.Web.ModelBinding**命名空间，若要解决控件属性。

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample4.cs)]

下面的代码演示 select 方法重新处理来筛选返回的数据的下拉列表中的值。 一个参数指定此参数的值来自具有相同名称的控件之前，将添加控件属性。

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample5.cs)]

运行 web 应用程序并从下拉列表来筛选学生列表中选择不同的值。

![筛选器学生版](sorting-paging-and-filtering-data/_static/image6.png)

## <a name="conclusion"></a>结束语

在本教程中，你可以启用排序和分页的数据。 你也可以启用控件的值筛选数据。

在下一个[教程](integrating-jquery-ui.md)您将通过将 JQuery UI 小组件集成到动态数据模板来增强 UI。

>[!div class="step-by-step"]
[上一页](updating-deleting-and-creating-data.md)
[下一页](integrating-jquery-ui.md)
