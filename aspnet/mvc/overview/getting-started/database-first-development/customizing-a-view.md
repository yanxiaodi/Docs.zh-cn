---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: "首先使用 ASP.NET MVC 的 EF 数据库： 自定义视图 |Microsoft 文档"
author: tfitzmac
description: "使用 MVC、 实体框架和 ASP.NET 基架，可以创建的 web 应用程序提供了一个接口到现有数据库。 此教程系列..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: af9609396cff18b08824732731ddb9c5cca578fa
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="ef-database-first-with-aspnet-mvc-customizing-a-view"></a>首先使用 ASP.NET MVC 的 EF 数据库： 自定义视图
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> 使用 MVC、 实体框架和 ASP.NET 基架，可以创建的 web 应用程序提供了一个接口到现有数据库。 本系列教程演示了如何自动生成代码，使用户能够显示、 编辑、 创建和删除驻留在数据库表中的数据。 生成的代码对应于数据库表中的列。
> 
> 此系列的一部分的重点介绍更改要增强演示文稿的自动生成的视图。


## <a name="add-enrolled-courses-to-student-details"></a>将已注册的课程添加到学生详细信息

生成的代码应用程序提供一个良好的起始点，但它不一定提供所有应用程序中需要的功能。 你可以自定义代码以满足你的应用程序的特定要求。 目前，你的应用程序不显示有关所选学生的已注册的课程。 本部分中，在中，你将添加已注册的课程到每个学生**详细信息**学生的视图。

打开**Students/Details.cshtml**，和最后一个下方&lt;/dl&gt;选项卡上，但在关闭前&lt;/div&gt;标记中，添加以下代码。

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

此代码将创建所选学生注册表中显示为每个记录的行的表。 **显示**方法表示表达式的对象 (modelItem) 上呈现 HTML。 使用显示方法 （而不是只需在代码中嵌入的属性值） 以确保值的格式正确基于其类型和该类型的模板。 在此示例中，每个表达式从当前记录在循环中，返回的单个属性和值是指呈现为文本的基元类型。

再次浏览到学生/索引视图并选择**详细信息**学生之一。 你将看到已注册的课程已包含在视图中。

![学生的注册](customizing-a-view/_static/image1.png)

>[!div class="step-by-step"]
[上一页](changing-the-database.md)
[下一页](enhancing-data-validation.md)
