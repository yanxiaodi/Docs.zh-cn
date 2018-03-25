---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-validation-to-the-model
title: "将验证添加到模型 (C#) |Microsoft 文档"
author: Rick-Anderson
description: "创建控制器"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 9af927e2-1c3b-43d9-917d-1df75be3c482
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: 1e1235972d1e16153faee113af09edaa676d70d8
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/12/2018
---
<a name="adding-validation-to-the-model-c"></a>将验证添加到模型 (C#)
====================
通过[Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > 本教程的更新的版本可[此处](../../../getting-started/introduction/getting-started.md)使用 ASP.NET MVC 5 和 Visual Studio 2013。 它是更安全，请按照要简单得多，并演示更多的功能。
> 
> 
> 本教程将教您构建使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，这是 Microsoft Visual Studio 的免费版本的 ASP.NET MVC Web 应用程序的基础知识。 在开始之前，请确保已安装下面列出的先决条件。 你可以通过单击以下链接安装所有这些： [Web 平台安装程序](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)。 或者，你可以单独安装系统必备组件，使用以下链接：
> 
> - [Visual Studio Web Developer Express SP1 系统必备](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools 更新](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)（运行时 + 工具支持）
> 
> 如果你正在使用 Visual Studio 2010，而不 Visual Web Developer 2010，通过单击以下链接安装必备组件： [Visual Studio 2010 系统必备组件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)。
> 
> 使用 C# 源代码的 Visual Web Developer 项目是可以附带本主题。 [下载的 C# 版本](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)。 如果你愿意 Visual Basic，切换到[Visual Basic 版本](../vb/intro-to-aspnet-mvc-3.md)本教程。


在本部分中，你将添加到的验证逻辑`Movie`模型，且你将确保验证规则会实施任何时候用户试图创建或编辑使用应用程序的影片。

## <a name="keeping-things-dry"></a>保持操作干

ASP.NET MVC 的核心设计原则之一是模拟 （"不重复自己"）。 ASP.NET MVC 鼓励您一次，指定功能或行为，然后使用它 everywhere 反映在应用程序。 这可减少所需编写的代码的数量，并使你确实要编写更加容易维护的代码。

提供 ASP.NET MVC 和 Entity Framework Code First 的验证支持是在操作中原则出色示例。 在一个位置 （在模型类中） 以声明方式指定验证规则，然后强制实施这些规则无处不在应用程序中。

让我们看一下影片应用程序中，你就可以充分利用此验证支持。

## <a name="adding-validation-rules-to-the-movie-model"></a>将验证规则添加到影片模型

将通过添加到某些验证逻辑开始`Movie`类。

打开 Movie.cs 文件。 添加`using`语句引用的文件的顶部[ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)命名空间：

[!code-csharp[Main](adding-validation-to-the-model/samples/sample1.cs)]

命名空间是.NET Framework 的一部分。 它提供了可以以声明方式应用于任何类或属性的验证属性内置组。

现在更新`Movie`类以利用内置[ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx)， [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)，和[ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx)验证特性. 使用以下代码作为示例，了解将特性应用的位置。

[!code-csharp[Main](adding-validation-to-the-model/samples/sample2.cs)]

验证特性指定要对应用这些特性的模型属性强制执行的行为。 `Required`属性指示属性必须具有一个值; 在此示例中，一部电影必须具有值`Title`， `ReleaseDate`， `Genre`，和`Price`属性才有效。 `Range` 特性将值限制在指定范围内。 `StringLength` 特性使你能够设置字符串属性的最大长度，以及可选的最小长度。

代码首先将确保： 在一个模型类指定的验证规则之前应用程序将更改保存在数据库会强制实施。 例如，下面的代码将引发异常时`SaveChanges`调用方法，因为几个需要`Movie`属性值为缺失，价格为零 （这是超出了有效范围）。

[!code-csharp[Main](adding-validation-to-the-model/samples/sample3.cs)]

具有自动由.NET Framework 强制执行的验证规则有助于使你的应用程序更可靠。 同时它能确保你无法忘记验证某些内容，并防止你无意中将错误数据导入数据库。

下面是完整的代码列出了已更新的*Movie.cs*文件：

[!code-csharp[Main](adding-validation-to-the-model/samples/sample4.cs)]

## <a name="validation-error-ui-in-aspnet-mvc"></a>验证错误在 ASP.NET MVC UI

重新运行该应用程序并导航到*/Movies* URL。

单击**创建电影**链接添加新的影片。 填写表单的某些无效的值，然后单击**创建**按钮。

[![8_validationErrors](adding-validation-to-the-model/_static/image2.png)](adding-validation-to-the-model/_static/image1.png)

请注意如何窗体具有自动使用背景色来突出显示的文本框中，它包含无效的数据，并且已发出旁边每个相应的验证错误消息。 错误消息匹配的错误字符串，指定当批注`Movie`类。 错误会强制客户端 （使用 JavaScript） 和服务器端执行，（如果用户已禁用 JavaScript）。

实际的好处是，你不需要更改单个行中的代码`MoviesController`类或在*Create.cshtml*以启用此验证 UI 的视图。 控制器和自动创建本教程中前面的视图中选取验证规则，指定在使用属性的向上`Movie`模型类。

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>如何验证发生在创建查看和创建操作方法

你可能想知道在不对控制器或视图中的代码进行任何更新的情况下，验证 UI 是如何生成的。 下一步的列表显示什么`Create`中的方法`MovieController`类如下所示。 这些更改将从你创建这些此前在本教程不变。

[!code-csharp[Main](adding-validation-to-the-model/samples/sample5.cs)]

第一种操作方法显示初始创建窗体。 第二个处理窗体发布请求。 第二个`Create`方法调用`ModelState.IsValid`若要检查的影片是否有任何验证错误。 调用此方法将评估已应用于对象的任何验证特性。 如果该对象具有验证错误`Create`方法重新显示窗体。 如果没有错误，此方法则将新电影保存在数据库中。

下面是*Create.cshtml*了教程前面的基架的视图模板。 以上所示的操作方法使用它来显示初始表单，并在发生错误时重新显示此表单。

[!code-cshtml[Main](adding-validation-to-the-model/samples/sample6.cshtml)]

请注意代码如何使用`Html.EditorFor`帮助器输出`<input>`每个元素`Movie`属性。 此帮助器的旁边是调用`Html.ValidationMessageFor`帮助器方法。 这两个帮助器方法使用由控制器传递给视图的模型对象 (在这种情况下，`Movie`对象)。 它们在自动显示为相应的模型和显示错误消息上指定的验证特性。

有关这种方法非常令人愉快的是，在控制器和创建视图模板都不知道任何有关强制实施的实际的验证规则或显示的特定错误消息。 仅可在 `Movie` 类中指定验证规则和错误字符串。

如果你想要以后更改验证逻辑，你可以这样在一个位置。 无需担心对应用程序的不同部分所强制执行规则的方式不一致 - 所有验证逻辑都将定义在一个位置并用于整个应用程序。 这使代码非常简洁，并且更易于维护和改进。 这意味着对 DRY 原则的完全遵守。

## <a name="adding-formatting-to-the-movie-model"></a>添加到影片模型格式设置

打开 Movie.cs 文件。 [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)命名空间提供内置集以及验证特性的格式设置属性。 将应用[ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)属性和[ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)枚举值到发布日期和价格字段。 下面的代码演示`ReleaseDate`和`Price`使用相应的属性[ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)属性。

[!code-csharp[Main](adding-validation-to-the-model/samples/sample7.cs)]

或者，你可以显式设置[ `DataFormatString` ](https://msdn.microsoft.com/library/system.string.format.aspx)值。 下面的代码演示具有日期格式字符串的发行日期属性 (也就是说，"d")。 将使用此参数来指定你不想为时间作为发布日期的一部分。

[!code-csharp[Main](adding-validation-to-the-model/samples/sample8.cs)]

下面的代码格式`Price`属性作为货币。

[!code-csharp[Main](adding-validation-to-the-model/samples/sample9.cs)]

完整`Movie`类如下所示。

[!code-csharp[Main](adding-validation-to-the-model/samples/sample10.cs)]

运行应用程序，并浏览到`Movies`控制器。

![8_format_SM](adding-validation-to-the-model/_static/image3.png)

在本系列的下一部分中，我们将回顾应用程序，并对自动生成的 `Details` 和 `Delete` 方法进行一些改进。

>[!div class="step-by-step"]
[上一页](adding-a-new-field.md)
[下一页](improving-the-details-and-delete-methods.md)
