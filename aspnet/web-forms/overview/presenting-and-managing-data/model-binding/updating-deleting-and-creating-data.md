---
uid: web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
title: "更新、 删除和使用模型的绑定和 web 窗体中创建的数据 |Microsoft 文档"
author: tfitzmac
description: "本系列教程演示使用模型绑定的 ASP.NET Web 窗体项目的基本方面。 模型绑定使数据交互详细直接-..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 602baa94-5a4f-46eb-a717-7a9e539c1db4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
msc.type: authoredcontent
ms.openlocfilehash: 18c065b44524e7738c048b5908fa50c592188064
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="updating-deleting-and-creating-data-with-model-binding-and-web-forms"></a>更新、 删除和使用模型的绑定和 web 窗体中创建的数据
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> 本系列教程演示使用模型绑定的 ASP.NET Web 窗体项目的基本方面。 模型绑定使数据交互更直接的比处理数据源对象 （如 ObjectDataSource 或 SqlDataSource）。 这一系列开头介绍性材料，更高版本的教程中移动到更高级的概念。
> 
> 本教程演示如何创建、 更新和删除包含模型绑定的数据。 将设置以下属性：
> 
> - Delete 方法
> - InsertMethod
> - UpdateMethod
> 
> 这些属性接收处理相应的操作的方法的名称。 在该方法中，你提供的逻辑与数据进行交互。
> 
> 本教程基于第一个中创建的项目[一部分](retrieving-data.md)的序列。
> 
> 你可以[下载](https://go.microsoft.com/fwlink/?LinkId=286116)在 C# 或 VB.完整项目 可下载的代码适用于 Visual Studio 2012 或 Visual Studio 2013。 它使用 Visual Studio 2012 模板，这是本教程中所示的 Visual Studio 2013 模板过程稍有不同。


## <a name="what-youll-build"></a>你将生成

在本教程中，你将：

1. 添加动态数据模板
2. 通过模型绑定方法启用更新和删除数据
3. 将数据验证规则应用-支持在数据库中创建一条新记录

## <a name="add-dynamic-data-templates"></a>添加动态数据模板

若要提供最佳用户体验和最大程度减少代码重复，你将使用动态数据模板。 通过安装 NuGet 包可以轻松地将预建的动态数据模板集成到您的现有网站。

从**管理 NuGet 包**，安装**DynamicDataTemplatesCS**。

![动态数据模板](updating-deleting-and-creating-data/_static/image1.png)

请注意你的项目现在包括一个名为文件夹**DynamicData**。 在该文件夹中，你会发现将自动应用于在 web 窗体中动态控件的模板。

![动态数据文件夹](updating-deleting-and-creating-data/_static/image2.png)

## <a name="enable-updating-and-deleting"></a>启用更新和删除

使用户能够更新和删除数据库中的记录是检索数据的过程非常相似。 在**UpdateMethod**和**delete 方法**属性，您指定的执行这些操作的方法的名称。 GridView 控件后，你还可以指定自动生成的编辑和删除按钮。 下面突出显示的代码演示上述元素添加到的 GridView 代码。

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample1.aspx?highlight=4-5)]

在代码隐藏文件中，添加 using 语句**System.Data.Entity.Infrastructure**。

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample2.cs)]

然后，添加以下更新和删除方法。

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample3.cs)]

**TryUpdateModel**方法将 web 窗体中匹配的数据绑定值应用于数据项目。 数据中检索项根据 id 参数的值。

## <a name="enforce-validation-requirements"></a>强制执行验证要求

将数据更新时，会自动强制执行你应用到学生类中的 FirstName、 LastName 和年份属性的验证属性。 DynamicField 控件添加基于验证特性的客户端和服务器验证程序。 FirstName 和 LastName 属性都需要。 名字不能超过 20 个字符的长度，并且姓氏不能超过 40 个字符。 年份必须是 AcademicYear 枚举的有效值。

如果用户违反了某个验证要求，才可以继续更新。 若要查看错误消息，请添加上面 GridView ValidationSummary 控件。 若要显示模型绑定中的验证错误，请设置**ShowModelStateErrors**属性设置为**true**。 

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample4.aspx)]

运行 web 应用程序，并更新和删除的任何记录。

![更新数据](updating-deleting-and-creating-data/_static/image3.png)

请注意，当处于编辑模式年属性的值自动呈现为下拉列表。 年属性是枚举值，并一个枚举值的动态数据模板指定的下拉列表以进行编辑的列表。 你可以找到该模板通过打开**枚举\_Edit.ascx**文件中**DynamicData**/**FieldTemplates**文件夹。

如果你提供有效的值，则更新将成功完成。 如果您违反验证要求之一，更新将不会继续和网格上方显示错误消息。

![错误消息](updating-deleting-and-creating-data/_static/image4.png)

## <a name="add-new-records"></a>添加新的记录

GridView 控件不包括**InsertMethod**属性，因此不能使用与模型绑定添加一条新记录。 你可以查找中的 InsertMethod 属性**FormView**，**说明**，或**ListView**控件。 在本教程中，你将使用 FormView 控件以添加新的记录。

首先，添加将为添加新记录中创建的新页的链接。 上方 ValidationSummary，添加：

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample5.aspx)]

在学生页上的内容的顶部将显示新的链接。

![新的链接](updating-deleting-and-creating-data/_static/image5.png)

然后，添加新 web 窗体使用母版页，并将其命名为**AddStudent**。 选择作为主控页的 Site.Master。

将呈现为通过添加新的学生字段**DynamicEntity**控件。 DynamicEntity 控件呈现该 ItemType 属性中指定的类中可编辑的属性。 StudentID 属性已标记有**[ScaffoldColumn(false)]**属性以便不呈现。 在 AddStudent 页主内容占位符，添加以下代码。

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample6.aspx)]

在代码隐藏文件 (AddStudent.aspx.cs) 中，添加**使用**语句**ContosoUniversityModelBinding.Models**命名空间。

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample7.cs)]

然后，添加以下方法以指定如何插入一条新记录和取消按钮事件处理程序。

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample8.cs)]

保存的所有更改。

运行 web 应用程序并创建新的学生。

![添加新的学生](updating-deleting-and-creating-data/_static/image6.png)

单击**插入**并注意已创建新的学生。

![显示新学生](updating-deleting-and-creating-data/_static/image7.png)

## <a name="conclusion"></a>结束语

在本教程中，你可以启用更新、 删除和创建的数据。 你确保与数据交互时应用验证规则。

在下一个[教程](sorting-paging-and-filtering-data.md)在此系列中，将启用排序、 分页和筛选数据。

>[!div class="step-by-step"]
[上一页](retrieving-data.md)
[下一页](sorting-paging-and-filtering-data.md)
