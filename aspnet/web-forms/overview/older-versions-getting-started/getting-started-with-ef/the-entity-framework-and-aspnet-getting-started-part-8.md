---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
title: "如何开始使用实体框架 4.0 数据库和 ASP.NET 4 Web 窗体的第 8 部分 |Microsoft 文档"
author: tdykstra
description: "Contoso 大学示例 web 应用程序演示如何创建 ASP.NET Web 窗体应用程序使用实体框架。 该示例应用程序..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: aaadd9bb-5508-448c-ad57-5497dff90e13
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
msc.type: authoredcontent
ms.openlocfilehash: 323ee44f43f6d4081bd9ba50791755696bc9128f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-8"></a>如何开始使用实体框架 4.0 数据库和 ASP.NET 4 Web 窗体的第 8 部分
====================
通过[Tom Dykstra](https://github.com/tdykstra)

> Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 4.0 和 Visual Studio 2010 的 ASP.NET Web 窗体应用程序。 有关教程系列的信息，请参阅[序列中的第一个教程](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="using-dynamic-data-functionality-to-format-and-validate-data"></a>使用动态数据功能进行格式化和验证数据

在以前的教程，你将实现存储的过程。 本教程将演示如何动态数据功能可以提供以下好处：

- 字段的显示基于其数据类型的自动格式设置。
- 字段将自动验证基于其数据类型。
- 你可以在要自定义格式设置和验证行为的数据模型中添加元数据。 在执行此操作时，可以将格式设置和验证规则添加只在一个位置，并且它们正在自动应用无处不在你访问使用动态数据控件字段。

若要查看此工作原理，你将更改用于显示和编辑中的现有字段的控件*Students.aspx*页上，并且你将添加格式设置和验证元数据的名称和日期字段`Student`实体类型。

[![Image01](the-entity-framework-and-aspnet-getting-started-part-8/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image1.png)

## <a name="using-dynamicfield-and-dynamiccontrol-controls"></a>使用 DynamicField 和 DynamicControl 控件

打开*Students.aspx*页并在`StudentsGridView`控件替换**名称**和**注册日期**`TemplateField`元素替换为以下标记：

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample1.aspx)]

此标记使用`DynamicControl`代替了控制`TextBox`和`Label`学生中的控件名称模板字段，并使用`DynamicField`注册日期的控件。 指定无格式字符串。

添加`ValidationSummary`后控制`StudentsGridView`控件。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample2.aspx)]

在`SearchGridView`控件替换的标记**名称**和**注册日期**作为你的列未`StudentsGridView`控制，但省略`EditItemTemplate`元素。 `Columns`元素`SearchGridView`控件现在包含以下标记：

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample3.aspx)]

打开*Students.aspx.cs*并添加以下`using`语句：

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample4.cs)]

添加的处理程序已在页面的`Init`事件：

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample5.cs)]

此代码指定格式设置和字段的这些数据绑定控件中的验证，将提供动态数据`Student`实体。 如果运行页面时，你收到一条错误消息如下例所示，这通常意味着你忘记了调用`EnableDynamicData`中的方法`Page_Init`:

`Could not determine a MetaTable. A MetaTable could not be determined for the data source 'StudentsEntityDataSource' and one could not be inferred from the request URL.`

运行页面。

[![Image03](the-entity-framework-and-aspnet-getting-started-part-8/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image3.png)

在**注册日期**列中，时间显示了日期，因为该属性类型是`DateTime`。 你将更高版本修复此问题。

现在，请注意，动态数据自动提供了基本的数据验证。 例如，单击**编辑**，清除日期字段中，单击**更新**，并且你看到，动态数据自动使此必填的字段因为该值不是数据模型中可以为 null。 字段和采用的错误消息后的页显示一个星号`ValidationSummary`控件：

[![Image05](the-entity-framework-and-aspnet-getting-started-part-8/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image5.png)

可以省略`ValidationSummary`管理，因为你还可以通过以下方式将鼠标指针悬停星号以查看错误消息：

[![Image06](the-entity-framework-and-aspnet-getting-started-part-8/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image7.png)

动态数据还将验证输入中的数据**注册日期**字段为有效日期：

[![Image04](the-entity-framework-and-aspnet-getting-started-part-8/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image9.png)

如你所见，这是一般性错误消息。 在下一部分中，你将看到如何自定义消息，以及验证规则和格式设置规则。

## <a name="adding-metadata-to-the-data-model"></a>将元数据添加到数据模型

通常情况下，你想要自定义所提供的动态数据功能。 例如，你可能会更改数据的显示方式以及错误消息的内容。 你通常还自定义数据验证规则来提供比动态数据提供更多的功能自动基于数据类型。 若要执行此操作，你可以创建对应于实体类型的分部类。

在**解决方案资源管理器**，右键单击**ContosoUniversity**项目，依次选择**添加引用**，并添加对引用`System.ComponentModel.DataAnnotations`。

[![Image11](the-entity-framework-and-aspnet-getting-started-part-8/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image11.png)

在*DAL*文件夹中，创建一个新的类文件，将其命名为*Student.cs*，并替换为以下代码将在其中的模板代码。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample6.cs)]

此代码将创建一个分部类`Student`实体。 `MetadataType`应用于此分部类的属性标识要用于指定元数据的类。 元数据类可以有任何名称，但使用的实体名称加上"元数据"是常见的做法。

应用于元数据类中的属性的特性指定格式设置、 验证、 规则和错误消息。 此处显示的属性将具有以下结果：

- `EnrollmentDate`将显示为日期 （不含时间）。
- 这两个名称字段必须是 25 个字符或者小于长度，以及自定义错误消息中提供。
- 这两个名称字段是必需的并提供自定义错误消息。

运行*Students.aspx*页，并且你看到没有时间的情况下要现在显示日期：

[![Image08](the-entity-framework-and-aspnet-getting-started-part-8/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image13.png)

编辑行和尝试清除名称字段中的值。 将字段，保留之前单击时，就会立即显示，该值指示字段错误星号**更新**。 当你单击**更新**，页会显示你指定的错误消息文本。

[![Image10](the-entity-framework-and-aspnet-getting-started-part-8/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image15.png)

尝试输入超过 25 个字符的名称，请单击**更新**，并页显示你指定的错误消息文本。

[![Image09](the-entity-framework-and-aspnet-getting-started-part-8/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image17.png)

现在，你已设置的数据模型元数据中这些格式设置和验证规则，规则将自动应用于每一页，用于显示或允许对这些字段中，所做更改处理程序，但前提是你使用`DynamicControl`或`DynamicField`控件。 这将减少你必须编写，后者将编程和测试更加轻松、 的冗余代码量，并确保数据格式设置和验证是在应用程序保持一致。

## <a name="more-information"></a>详细信息

这一系列的 Entity Framework 入门教程到此结束。 对于可帮助你了解如何使用实体框架的更多资源，请继续[下一步的实体框架教程系列中的第一个教程](../continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)或访问以下站点：

- [实体框架常见问题](http://www.ef-faq.org/introduction.html)
- [实体框架团队博客](https://blogs.msdn.com/b/adonet/)
- [MSDN 库中的实体框架](https://msdn.microsoft.com/library/bb399572.aspx)
- [在 MSDN 数据开发人员中心的实体框架](https://msdn.microsoft.com/data/ef.aspx)
- [MSDN 库中的 EntityDataSource Web 服务器控件概述](https://msdn.microsoft.com/library/cc488502.aspx)
- [EntityDataSource 控件 MSDN 库中的 API 参考](https://msdn.microsoft.com/library/system.web.ui.webcontrols.entitydatasource.aspx)
- [MSDN 上的实体框架论坛](https://social.msdn.microsoft.com/forums/adodotnetentityframework/)
- [Julie Lerman 博客](http://thedatafarm.com/blog/)

>[!div class="step-by-step"]
[上一篇](the-entity-framework-and-aspnet-getting-started-part-7.md)
