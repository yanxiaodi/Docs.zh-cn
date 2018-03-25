---
uid: web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
title: "添加到使用模型绑定和 web 窗体的项目的业务逻辑层 |Microsoft 文档"
author: tfitzmac
description: "本系列教程演示使用模型绑定的 ASP.NET Web 窗体项目的基本方面。 模型绑定使数据交互详细直接-..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 7ef664b3-1cc8-4cbf-bb18-9f0f3a3ada2b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
msc.type: authoredcontent
ms.openlocfilehash: ca50690052cca73a718342a9725c8096a72f1187
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="adding-business-logic-layer-to-a-project-that-uses-model-binding-and-web-forms"></a>添加到使用模型绑定和 web 窗体的项目的业务逻辑层
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> 本系列教程演示使用模型绑定的 ASP.NET Web 窗体项目的基本方面。 模型绑定使数据交互更直接的比处理数据源对象 （如 ObjectDataSource 或 SqlDataSource）。 这一系列开头介绍性材料，更高版本的教程中移动到更高级的概念。
> 
> 本教程演示如何使用业务逻辑层模型绑定。 你将设置要指定除当前页的对象用于调用数据方法的 OnCallingDataMethods 成员。
> 
> 本教程中创建的项目上[早期](retrieving-data.md)的序列部分。
> 
> 你可以[下载](https://go.microsoft.com/fwlink/?LinkId=286116)在 C# 或 VB.完整项目 可下载的代码适用于 Visual Studio 2012 或 Visual Studio 2013。 它使用 Visual Studio 2012 模板，这是本教程中所示的 Visual Studio 2013 模板过程稍有不同。


## <a name="what-youll-build"></a>你将生成

模型绑定，可将数据交互代码放在 web 页的代码隐藏文件中或在单独的业务逻辑类。 前面的教程已经演示了如何对数据交互代码中使用代码隐藏文件。 此方法适用于小型站点，但它可能会导致大型站点进行维护时代码重复和面临更大困难。 这也可以是很难以编程方式测试驻留在代码隐藏文件，因为没有抽象层的代码。

若要集中的数据交互代码，可以创建包含所有与数据进行交互的逻辑业务逻辑层。 然后，你会从你的 web 页面调用业务逻辑层。 本教程演示如何移动的所有代码都写入到业务逻辑层，前面的教程中，然后使用该代码从页。

在本教程中，你将：

1. 将代码从代码隐藏文件移到业务逻辑层
2. 更改你的数据绑定控件，以调用业务逻辑层中的方法

## <a name="create-business-logic-layer"></a>创建业务逻辑层

现在，你将创建从 web 页调用的类。 此类中的方法类似于你在前面的教程中使用的方法，并且包括值提供程序属性。

首先，添加一个名为的新文件夹**BLL**。

![添加文件夹](adding-business-logic-layer/_static/image1.png)

在 BLL 文件夹中，创建一个名为的新类**SchoolBL.cs**。 它将包含所有原先在代码隐藏文件中的数据操作。 方法代码隐藏文件中的方法几乎相同，但会包括一些更改。

要注意的最重要更改是不再执行中的实例内的代码**页**类。 页类包含**TryUpdateModel**方法和**ModelState**属性。 当此代码移到业务逻辑层中时，你不再有页类来调用这些成员的实例。 若要获取解决此问题，你必须添加**ModelMethodContext**访问 TryUpdateModel 或 ModelState 任何方法参数。 使用此 ModelMethodContext 参数调用 TryUpdateModel 或检索 ModelState。 不需要更改 web 页后，可以为此新参数帐户中的任何内容。

SchoolBL.cs 中的代码替换为以下代码。

[!code-csharp[Main](adding-business-logic-layer/samples/sample1.cs)]

## <a name="revise-existing-pages-to-retrieve-data-from-business-logic-layer"></a>修改现有的页面，从业务逻辑层检索数据

最后，你将从代码隐藏文件使用业务逻辑层中使用查询转换 Students.aspx、 AddStudent.aspx 和 Courses.aspx 的页。

学生、 AddStudent 和课程的代码隐藏文件，删除或注释掉以下的查询方法：

- studentsGrid\_GetData
- studentsGrid\_UpdateItem
- studentsGrid\_删除项
- addStudentForm\_InsertItem
- coursesGrid\_GetData

现在应与数据操作的代码隐藏文件中将出现任何代码。

**OnCallingDataMethods**事件处理程序，可指定要使用的数据方法的对象。 Students.aspx，在为该事件处理程序添加一个值，并将数据方法的名称更改为业务逻辑类中的方法的名称。

[!code-aspx[Main](adding-business-logic-layer/samples/sample2.aspx?highlight=3-4,8)]

在 Students.aspx 代码隐藏文件中，定义的事件处理程序 CallingDataMethods 事件。 在此事件处理程序中，你可以指定数据操作的业务逻辑类。

[!code-csharp[Main](adding-business-logic-layer/samples/sample3.cs)]

在 AddStudent.aspx，进行类似更改。

[!code-aspx[Main](adding-business-logic-layer/samples/sample4.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample5.cs)]

在 Courses.aspx，进行类似更改。

[!code-aspx[Main](adding-business-logic-layer/samples/sample6.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample7.cs)]

运行应用程序，并注意的所有页函数与在其之前。 验证逻辑还可以正常工作。

## <a name="conclusion"></a>结束语

在本教程中，你重新构建应用程序使用的数据访问层和业务逻辑层。 你指定数据控件使用一个对象，它不数据操作的当前页。

>[!div class="step-by-step"]
[上一篇](using-query-string-values-to-retrieve-data.md)
