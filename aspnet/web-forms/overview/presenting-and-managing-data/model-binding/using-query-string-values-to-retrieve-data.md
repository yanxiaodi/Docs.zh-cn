---
uid: web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
title: "使用查询字符串值来筛选具有模型绑定的数据和 web 窗体 |Microsoft 文档"
author: tfitzmac
description: "本系列教程演示使用模型绑定的 ASP.NET Web 窗体项目的基本方面。 模型绑定使数据交互详细直接-..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: b90978bd-795d-4871-9ade-1671caff5730
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
msc.type: authoredcontent
ms.openlocfilehash: 2e5328ccda019462163b984da3661f7322b738df
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="using-query-string-values-to-filter-data-with-model-binding-and-web-forms"></a>模型绑定和 web 窗体中使用查询字符串值将筛选器数据
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> 本系列教程演示使用模型绑定的 ASP.NET Web 窗体项目的基本方面。 模型绑定使数据交互更直接的比处理数据源对象 （如 ObjectDataSource 或 SqlDataSource）。 这一系列开头介绍性材料，更高版本的教程中移动到更高级的概念。
> 
> 本教程演示如何传递查询字符串中的一个值，并使用该值通过模型绑定中检索数据。
> 
> 本教程中创建的项目上[早期](retrieving-data.md)的序列部分。
> 
> 你可以[下载](https://go.microsoft.com/fwlink/?LinkId=286116)在 C# 或 VB.完整项目 可下载的代码适用于 Visual Studio 2012 或 Visual Studio 2013。 它使用 Visual Studio 2012 模板，这是本教程中所示的 Visual Studio 2013 模板过程稍有不同。


## <a name="what-youll-build"></a>你将生成

在本教程中，你将：

1. 添加新的页以显示学生的已注册的课程
2. 检索所选基于查询字符串中的值的学生的已注册的课程
3. 将带有查询字符串值的超链接从网格视图添加到新的页

本教程中的步骤是非常类似于您进行的操作在早期版本[教程](sorting-paging-and-filtering-data.md)来筛选显示的学生基于用户选择下拉列表中。 在该教程中，你使用了**控件**中选择的方法，以指定的参数值来自控件的属性。 在本教程中，你将使用**QueryString**中选择的方法，以指定的参数值来自查询字符串属性。

## <a name="add-new-page-for-displaying-a-students-courses"></a>添加用于显示学生的课程的新页

添加新的 web 窗体使用 Site.master 主页上，然后将该页命名**课程**。

在**Courses.aspx**文件中，添加一个网格视图以显示所选学生课程。

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample1.aspx)]

## <a name="define-the-select-method"></a>定义 select 方法

在**Courses.aspx.cs**，你将添加具有在网格视图中指定的名称的选择方法**SelectMethod**属性。 在该方法中，你将定义用于检索学生的课程，查询，并指定参数来自具有相同名称作为参数的查询字符串值。

首先，必须添加以下**使用**语句。

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample2.cs)]

然后，将以下代码添加到 Courses.aspx.cs:

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample3.cs)]

查询字符串属性意味着一个名为 StudentID 的查询字符串值将自动分配给此方法中的参数。

## <a name="add-hyperlink-with-query-string-value"></a>添加查询字符串值以超链接

在上 Students.aspx 网格视图中，你将添加链接到你的新课程页面的超链接字段。 超链接将包括具有学生 id 的查询字符串值。

在 Students.aspx，将以下字段添加到字段的正下方的网格视图列的总的信用额度。

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample4.aspx?highlight=7-8)]

运行应用程序，并请注意，网格视图现在包含课程链接。

![添加超链接](using-query-string-values-to-retrieve-data/_static/image1.png)

当你单击一个链接时，你将看到该学生的已注册的课程。

![显示课程](using-query-string-values-to-retrieve-data/_static/image2.png)

## <a name="conclusion"></a>结束语

在本教程中，你添加的链接的查询字符串值。 该查询字符串值用于在 select 方法的参数值。

在下一个[教程](adding-business-logic-layer.md)，会将代码隐藏文件的代码移入业务逻辑层和数据访问层。

>[!div class="step-by-step"]
[上一页](integrating-jquery-ui.md)
[下一页](adding-business-logic-layer.md)
