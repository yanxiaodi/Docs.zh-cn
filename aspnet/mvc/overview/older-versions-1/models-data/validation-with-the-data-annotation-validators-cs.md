---
uid: mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-cs
title: "验证与数据批注验证程序 (C#) |Microsoft 文档"
author: microsoft
description: "利用数据批注模型联编程序来执行验证的 ASP.NET MVC 应用程序中。 了解如何使用不同类型的验证程序..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/29/2009
ms.topic: article
ms.assetid: 7ca8013e-9dfc-4e33-8336-cdccfd5f9414
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-cs
msc.type: authoredcontent
ms.openlocfilehash: 306dcb0197dfc9317ea9665dd2b1c058ba8bd712
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="validation-with-the-data-annotation-validators-c"></a>验证与数据批注验证程序 (C#)
====================
通过[Microsoft](https://github.com/microsoft)

> 利用数据批注模型联编程序来执行验证的 ASP.NET MVC 应用程序中。 了解如何使用不同类型的验证程序属性和 Microsoft 实体框架中使用它们。


在本教程中，您将学习如何使用数据注释验证程序在 ASP.NET MVC 应用程序中执行验证。 使用数据注释验证程序的优点是它们使你可以执行验证，只需通过添加一个或多个属性 – 例如必需或 StringLength 属性 – 到类属性。

你可以使用数据注释验证程序之前，你必须下载数据批注模型联编程序。 你可以从 CodePlex 网站下载数据批注模型联编程序示例，通过单击[此处](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471)。


请务必了解数据批注模型联编程序不是官方 Microsoft ASP.NET MVC framework 的一部分。 尽管数据批注模型联编程序已由 Microsoft ASP.NET MVC 团队创建的但 Microsoft 不提供数据批注模型联编程序的官方产品支持所述，然后在本教程中使用。


## <a name="using-the-data-annotation-model-binder"></a>使用数据批注模型联编程序

若要使用 ASP.NET MVC 应用程序中的数据批注模型联编程序，首先需要添加对 Microsoft.Web.Mvc.DataAnnotations.dll 程序集和 System.ComponentModel.DataAnnotations.dll 程序集的引用。 选择菜单选项**项目中，添加引用**。 接下来，单击**浏览**选项卡，浏览到下载 （并解压缩） 数据批注模型联编程序示例其中的位置 (请参阅**图 1**)。

[![](validation-with-the-data-annotation-validators-cs/_static/image2.png)](validation-with-the-data-annotation-validators-cs/_static/image1.png)

**图 1**： 添加对数据批注模型联编程序的引用 ([单击以查看实际尺寸的图像](validation-with-the-data-annotation-validators-cs/_static/image3.png))

选择 Microsoft.Web.Mvc.DataAnnotations.dll 程序集和 System.ComponentModel.DataAnnotations.dll 程序集，然后单击**确定**按钮。


不能使用与数据批注模型联编程序的.NET Framework Service Pack 1 中包含的 System.ComponentModel.DataAnnotations.dll 程序集。 你必须使用包含数据批注模型联编程序示例下载 System.ComponentModel.DataAnnotations.dll 程序集的版本。


最后，你需要在 Global.asax 文件中注册 DataAnnotations 模型联编程序。 将以下代码行添加到应用程序\_start （） 事件处理程序，以便应用程序\_start （） 方法如下所示：

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample1.cs)]

此代码行将 ataAnnotationsModelBinder 注册为整个 ASP.NET MVC 应用程序的默认模型联编程序。

## <a name="using-the-data-annotation-validator-attributes"></a>使用数据批注验证程序属性

当使用数据批注模型联编程序时，您将使用验证程序属性执行验证。 System.ComponentModel.DataAnnotations 命名空间包括以下的验证程序属性：

- 范围 – 使你可以验证是否属性的值介于之间指定的值范围。
- 正则表达式 – 使你可以验证是否与指定正则表达式模式匹配的属性的值。
- 所需 – 使您能够将根据需要属性标记。
- StringLength –，可指定的字符串属性的最大长度。
- 验证 – 所有验证程序属性的基类。

> [!NOTE] 
> 
> 如果任何标准的验证程序不能满足你的验证需求然后你始终可以通过从基本的验证属性继承新的验证程序属性创建自定义验证程序属性的选项。


中的产品类**列出 1**演示了如何使用这些验证程序属性。 名称、 说明和单价属性标记所需的方式。 Name 属性必须是少于 10 个字符的字符串长度。 最后，单价属性必须与表示货币金额的正则表达式模式匹配。

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample2.cs)]

**列表 1**: Models\Product.cs

产品类演示了如何使用一个附加的属性： DisplayName 属性。 DisplayName 属性，可在一条错误消息中显示属性时修改属性的名称。 而不是显示错误消息"UnitPrice 字段必填"你可以显示错误消息"价格字段是必填"。

> [!NOTE] 
> 
> 如果你想要完全自定义验证程序显示的错误消息可以将自定义错误消息分配给此类的验证程序的 ErrorMessage 属性：`<Required(ErrorMessage:="This field needs a value!")>`


你可以使用中的产品类**列出 1**与中的 create （） 控制器操作**列出 2**。 当模型状态中包含任何错误，则此控制器操作重新显示创建视图。

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample3.cs)]

**列出 2**: Controllers\ProductController.vb

最后，你可以在其中创建中的视图**列出 3**右键单击 create （） 操作并选择菜单选项**添加视图**。 作为模型类使用的产品类来创建强类型视图。 选择**创建**视图内容的下拉列表中 (请参阅**图 2**)。

[![](validation-with-the-data-annotation-validators-cs/_static/image5.png)](validation-with-the-data-annotation-validators-cs/_static/image4.png)

**图 2**： 添加 Create 视图

[!code-aspx[Main](validation-with-the-data-annotation-validators-cs/samples/sample4.aspx)]

**列出 3**: Views\Product\Create.aspx

> [!NOTE] 
> 
> 从生成的创建表单中删除 Id 字段**添加视图**菜单选项。 Id 字段对应于标识列，因为你不想要允许用户输入此字段的值。


如果提交的窗体创建产品并且不要为必填的字段中，输入值，则验证错误消息中**图 3**显示。

[![](validation-with-the-data-annotation-validators-cs/_static/image7.png)](validation-with-the-data-annotation-validators-cs/_static/image6.png)

**图 3**： 缺少必填的字段

如果输入无效的货币金额，然后中的错误消息**图 4**显示。

[![](validation-with-the-data-annotation-validators-cs/_static/image9.png)](validation-with-the-data-annotation-validators-cs/_static/image8.png)

**图 4**： 无效的货币金额

## <a name="using-data-annotation-validators-with-the-entity-framework"></a>使用随实体框架数据批注验证程序

如果你使用 Microsoft 实体框架来生成你的数据模型类不能将验证程序属性应用直接向你的类。 因为实体框架设计器生成的模型类下, 一步时在设计器中进行任何更改将覆盖您对模型类进行任何更改。

如果你想要使用实体框架生成的类中使用验证程序然后你需要创建元数据类。 你将验证程序应用于元数据类，而不是将验证程序应用到实际类。

例如，想象一下这样，你已创建了使用实体框架的电影类 (请参阅**图 5**)。 此外，假设你想要使影片标题和控制器属性所需的属性。 在这种情况下，你可以在其中创建分部类和中的元数据类**列出 4**。

[![](validation-with-the-data-annotation-validators-cs/_static/image11.png)](validation-with-the-data-annotation-validators-cs/_static/image10.png)

**图 5**： 生成的实体框架的电影类

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample5.cs)]

**列出 4**: Models\Movie.cs

中的文件**列出 4**包含名为电影和 MovieMetaData 的两个类。 电影类是分部类。 它对应于 DataModel.Designer.vb 文件中包含的实体框架生成的分部类。

目前，.NET framework 不支持部分属性。 因此，没有方法将验证程序特性应用于电影类将验证程序特性应用到的属性内的文件中定义的电影类 DataModel.Designer.vb 文件中定义的属性**列出 4**.

请注意，具有指向 MovieMetaData 类 MetadataType 属性修饰电影分部类。 MovieMetaData 类包含属性的电影类的代理属性。

验证程序特性应用到 MovieMetaData 类的属性。 标题、 控制器和 DateReleased 属性所有被标记为必需属性。 必须将控制器属性分配包含小于 5 个字符的字符串。 最后，DisplayName 属性应用于要显示一条错误消息，如"日期发布字段是必需的。"的 DateReleased 属性 而不是错误"DateReleased 字段是必需的。"

> [!NOTE] 
> 
> 请注意，不需要 MovieMetaData 类中的代理属性来表示电影类中的相应属性相同的类型。 例如，目录属性是电影类中的字符串属性和 MovieMetaData 类中的对象属性。


中的页**图 6**演示当电影属性中输入无效值时，返回的错误消息。

[![](validation-with-the-data-annotation-validators-cs/_static/image13.png)](validation-with-the-data-annotation-validators-cs/_static/image12.png)

**图 6**： 使用实体框架的验证程序 ([单击以查看实际尺寸的图像](validation-with-the-data-annotation-validators-cs/_static/image14.png))

## <a name="summary"></a>摘要

在本教程中，您学习了如何充分利用数据批注模型联编程序来执行验证的 ASP.NET MVC 应用程序中。 您学习了如何使用不同类型的如所需的验证程序属性和 StringLength 属性。 你还了解了如何使用 Microsoft 实体框架时使用这些属性。

>[!div class="step-by-step"]
[上一页](validating-with-a-service-layer-cs.md)
[下一页](creating-model-classes-with-the-entity-framework-vb.md)
