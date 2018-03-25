---
uid: mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-vb
title: "验证 IDataErrorInfo 接口 (VB) |Microsoft 文档"
author: StephenWalther
description: "Stephen Walther 演示了如何通过模型类中实现该 IDataErrorInfo 接口显示自定义的验证错误消息。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 3a8a9d9f-82dd-4959-b7c6-960e9ce95df1
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 1439d470a7fa3cb1171dbdd0b7eec6a6aa52912d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="validating-with-the-idataerrorinfo-interface-vb"></a>验证 IDataErrorInfo 接口 (VB)
====================
通过[Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther 演示了如何通过模型类中实现该 IDataErrorInfo 接口显示自定义的验证错误消息。


本教程旨在向 ASP.NET MVC 应用程序中执行验证说明一种方法。 了解如何防止别人提交一个 HTML 表单，而无需为所需的窗体字段提供值。 在本教程中，你将了解如何通过使用 IErrorDataInfo 界面执行验证。

## <a name="assumptions"></a>假设

在本教程中，我将使用 MoviesDB 数据库和电影数据库表。 此表包含以下列：

<a id="0.6_table01"></a>


| **列名称** | **数据类型** | **允许 null 值** |
| --- | --- | --- |
| Id | Int | False |
| 标题 | nvarchar(100) | False |
| 控制器 | nvarchar(100) | False |
| DateReleased | DateTime | False |


在本教程中，我可以使用 Microsoft 实体框架生成我的数据库模型类。 实体框架生成的电影类如图 1 所示。


[![电影实体](validating-with-the-idataerrorinfo-interface-vb/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image1.png)

**图 01**: 电影实体 ([单击以查看实际尺寸的图像](validating-with-the-idataerrorinfo-interface-vb/_static/image2.png))


> [!NOTE] 
> 
> 若要了解有关使用实体框架生成你的数据库模型类的详细信息，请参阅我的教程标题为与实体框架创建模型类。


## <a name="the-controller-class"></a>控制器类

我们将使用到列表的电影的主页控制器，并创建新影片。 列表 1 中包含此类代码。

**列表 1-Controllers\HomeController.vb**

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample1.vb)]

列表 1 中的主页控制器类包含两个 create （） 操作。 第一项操作显示用于创建新的影片的 HTML 窗体。 第二个 create （） 操作执行新的影片实际插入到数据库。 显示的第一个 create （） 操作的窗体提交到服务器时，将调用第二个 create （） 操作。

请注意，第二个 create （） 操作包含以下代码行：

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample2.vb)]

验证错误时，IsValid 属性返回 false。 在这种情况下，创建视图，其中包含用于创建一部电影的 HTML 窗体将重新显示。

## <a name="creating-a-partial-class"></a>创建分部类

电影类由实体框架生成。 如果你展开解决方案资源管理器窗口中的 MoviesDBModel.edmx 文件并打开 MoviesDBModel.Designer.vb 文件在代码编辑器中，你可以看到电影类的代码 （请参见图 2）。


[![电影实体的代码](validating-with-the-idataerrorinfo-interface-vb/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image3.png)

**图 02**： 电影实体的代码 ([单击以查看实际尺寸的图像](validating-with-the-idataerrorinfo-interface-vb/_static/image4.png))


电影类是分部类。 这意味着，我们可以添加具有相同名称来扩展电影类的功能的另一个分部类。 我们将添加到新的分部类的我们验证逻辑。

将列出 2 中的类添加到 Models 文件夹。

**列出 2-Models\Movie.vb**

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample3.vb)]

请注意，列出 2 中的类包括*部分*修饰符。 任何方法或属性添加到此类成为生成实体框架的电影类的一部分。

## <a name="adding-onchanging-and-onchanged-partial-methods"></a>添加 OnChanging 和 OnChanged 分部方法

当实体框架生成实体类时，实体框架分部方法与类会自动添加。 实体框架生成 OnChanging 和 OnChanged 对应于每个属性类的分部方法。

对于电影类中，实体框架将创建以下方法：

- OnIdChanging
- OnIdChanged
- OnTitleChanging
- OnTitleChanged
- OnDirectorChanging
- OnDirectorChanged
- OnDateReleasedChanging
- OnDateReleasedChanged

对应的属性发生更改之前，称为右 OnChanging 方法。 OnChanged 方法调用右后更改的属性。

你可以利用这些分部方法，以向电影类添加验证逻辑。 更新列出 3 中的电影类验证的标题和控制器属性分配非空值。

> [!NOTE] 
> 
> 分部方法是你不需要实现的类中定义的方法。 如果不实现分部方法然后编译器移除方法签名，并对存在的方法的所有调用都是分部方法不运行时收费。 在 Visual Studio 代码编辑器中，你可以通过键入关键字添加分部方法*部分*跟一个空格，以查看它们实现的列表。


**列出 3-Models\Movie.vb**

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample4.vb)]

例如，如果你尝试将一个空字符串分配给 Title 属性，则一条错误消息分配给一个名为词典\_错误。

此时，会执行任何操作实际时将一个空字符串分配给 Title 属性和错误添加到私有\_错误字段。 我们需要实现 IDataErrorInfo 接口可公开的 ASP.NET MVC framework 这些验证错误。

## <a name="implementing-the-idataerrorinfo-interface"></a>实现 IDataErrorInfo 接口

IDataErrorInfo 接口起的第一个版本已是.NET framework 的一部分。 此接口是一个非常简单的接口：

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample5.vb)]

如果一个类实现 IDataErrorInfo 接口，则 ASP.NET MVC framework 创建类的实例时将使用此接口。 例如，create （） 操作中的主页控制器接受电影类的实例：

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample6.vb)]

ASP.NET MVC framework 创建影片通过使用模型联编程序 (DefaultModelBinder) 传递给 create （） 操作的实例。 模型联编程序负责通过将 HTML 窗体字段绑定到影片对象的实例创建电影对象的实例。

DefaultModelBinder 检测类实现 IDataErrorInfo 接口。 如果一个类实现此接口模型联编程序调用类的每个属性的 IDataErrorInfo.this 索引器。 如果索引器返回一条错误消息模型联编程序将添加此错误消息，以自动模型状态。

DefaultModelBinder 还检查 IDataErrorInfo.Error 属性。 此属性用于表示与类关联的非属性特定验证错误。 例如，你可能想要执行验证规则依赖于电影类的多个属性的值。 在这种情况下，你将从错误属性返回验证错误。

列出 4 中更新后的电影类实现 IDataErrorInfo 接口。

**列出 4-Models\Movie.vb （实现 IDataErrorInfo）**

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample7.vb)]

在列出 4 中，检查索引器属性\_errors 集合，可以查看它是否包含对应于的属性名称的密钥传递给索引器。 如果没有任何与属性关联的验证错误则返回空字符串。

你不需要修改任何方式来使用修改后的电影类中的主页控制器。 图 3 中显示的页面说明了用于标题或控制器窗体字段中不输入任何值时，会发生什么情况。


[![自动创建操作方法](validating-with-the-idataerrorinfo-interface-vb/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image5.png)

**图 03**： 的窗体具有缺失值 ([单击以查看实际尺寸的图像](validating-with-the-idataerrorinfo-interface-vb/_static/image6.png))


请注意，自动验证 DateReleased 值。 因为 DateReleased 属性不接受 NULL 值，DefaultModelBinder 验证错误，此属性会自动生成时它不具有值。 如果你想要修改 DateReleased 属性的错误消息，则需要创建自定义模型联编程序。

## <a name="summary"></a>摘要

在本教程中，您学习了如何使用 IDataErrorInfo 接口生成验证错误消息。 首先，我们创建扩展的功能由实体框架生成的部分电影类的分部电影类。 接下来，我们验证逻辑添加到的电影类 OnTitleChanging() 和 OnDirectorChanging() 分部方法。 最后，我们实现 IDataErrorInfo 接口才能公开的 ASP.NET MVC framework 这些验证消息。

>[!div class="step-by-step"]
[上一页](performing-simple-validation-vb.md)
[下一页](validating-with-a-service-layer-vb.md)
