---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-new-field
title: "将新字段添加到的电影模型和表 (C#) |Microsoft 文档"
author: Rick-Anderson
description: "本教程将教您构建使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，这是一个 ASP.NET MVC Web 应用程序的基础知识..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: b4e76c1a-f66e-43a0-aa72-f39df79c07c1
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: c10f3be30a92a605c34fa1c56fa3691389374beb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-new-field-to-the-movie-model-and-table-c"></a>将新字段添加到的电影模型和表 (C#)
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


在本部分将对模型类进行某些更改，并了解如何更新数据库架构以匹配的模型更改。

## <a name="adding-a-rating-property-to-the-movie-model"></a>向电影模型添加分级属性

添加一个新的启动`Rating`到现有的属性`Movie`类。 打开*Movie.cs*文件并添加`Rating`属性如下所示：

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

完整`Movie`类现在如下所示的以下代码：

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

重新编译应用程序使用**调试** &gt;**生成电影**菜单命令。

现在，你已更新`Model`类，你还需要更新*\Views\Movies\Index.cshtml*和*\Views\Movies\Create.cshtml*查看模板以支持新`Rating`属性。

打开*\Views\Movies\Index.cshtml*文件并添加`<th>Rating</th>`列标题之后**价格**列。 然后添加`<td>`结尾处的模板来呈现的列`@item.Rating`值。 下面是哪些更新*Index.cshtml*视图模板如下所示：

[!code-cshtml[Main](adding-a-new-field/samples/sample3.cshtml)]

接下来，打开*\Views\Movies\Create.cshtml*文件并添加以下标记窗体末尾附近。 这会呈现一个文本框，以便创建新的影片时，可以指定的分级。

[!code-cshtml[Main](adding-a-new-field/samples/sample4.cshtml)]

## <a name="managing-model-and-database-schema-differences"></a>管理模型和数据库架构差异

你现在已更新应用程序代码以支持新`Rating`属性。

现在运行应用程序并导航到*/Movies* URL。 执行此操作，不过，你将看到以下错误：

![](adding-a-new-field/_static/image1.png)

你看到此错误，因为更新`Movie`应用程序中的模型类现在是不同的架构`Movie`现有数据库的表。 （数据库表中没有 `Rating` 列。）

默认情况下，当你使用 Entity Framework Code First 自动创建的数据库，就像此前在本教程中，Code First 表向数据库添加来帮助跟踪数据库的架构是否与从生成的模型类同步。 如果它们不同步，实体框架将引发错误。 这使得更轻松地在开发期间，你可能会否则仅发现 （通过晦涩的错误） 在运行时跟踪问题。 同步检查功能是什么因素会导致要显示错误消息，你刚才看到的。

有两种方法来解决错误：

1. 让 Entity Framework 自动丢弃，并基于新的模型类架构重新创建数据库。 此方法时非常方便进行活动开发上测试数据库，因为它允许你快速发展的模型和数据库架构在一起。 缺点，不过，是，则会失去在数据库中的现有数据，以便你*不*想要在生产数据库上使用此方法 ！
2. 对现有数据库架构进行显式修改，使它与模型类相匹配。 此方法的优点是可以保留数据。 可以手动或通过创建数据库更改脚本进行此更改。

对于本教程中，我们将使用第一种方法，你必须 Entity Framework Code First 模型更改的任何时候自动重新创建该数据库。

## <a name="automatically-re-creating-the-database-on-model-changes"></a>自动重新创建模型更改上的数据库

因此，Code First 自动将删除并重新创建数据库更改为应用程序模型的任何时候，让我们更新应用程序。

> [!NOTE] 
> 
> **警告**应启用的自动删除并重新创建数据库，仅当你正在使用开发或测试数据库，此方法和*永远不会*对生产数据库包含的真实数据。 在生产服务器上使用它可能导致数据丢失。


在**解决方案资源管理器**，右键单击*模型*文件夹，选择**添加**，然后选择**类**。

![](adding-a-new-field/_static/image2.png)

将类"MovieInitializer"。 更新`MovieInitializer`类包含以下代码：

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

`MovieInitializer`类指定应删除并自动重新创建的模型类更改使用模型的数据库。 代码包含`Seed`方法，以便指定一些默认数据自动向数据库添加任何时间已创建 （或重新创建）。 这提供了有用的方式来使用填充数据库一些示例数据，而无需手动填充它每次进行更改的模型。

既然你已经定义`MovieInitializer`类，你将需要连接它，以便每次应用程序运行时，它会检查是否从数据库中的架构不同的模型类。 如果它们是，你可以运行初始值设定项来重新创建要与模型匹配，然后填充示例数据的数据库的数据库。

打开*Global.asax*根目录下的文件`MvcMovies`项目：

[![](adding-a-new-field/_static/image4.png)](adding-a-new-field/_static/image3.png)

*Global.asax*文件包含定义的整个应用程序项目，而包含的类`Application_Start`运行应用程序首次启动时的事件处理程序。

让我们添加两个使用到文件顶部的语句。 第一个引用实体框架命名空间，而第二个引用命名空间其中我们`MovieInitializer`类生活：

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs)]

然后找到`Application_Start`方法，并添加到调用`Database.SetInitializer`开头的方法，如下所示：

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs)]

`Database.SetInitializer`刚添加的语句指示该数据库由`MovieDBContext`应自动删除并重新创建如果不匹配的架构和数据库实例。 如你所看到的它还将使用数据库中指定示例数据填充和`MovieInitializer`类。

关闭*Global.asax*文件。

重新运行该应用程序并导航到*/Movies* URL。 当应用程序启动时，它将检测模型结构不再与数据库架构相匹配。 自动重新创建数据库，以匹配新的模型结构，并填充示例电影的数据库：

![7_MyMovieList_SM](adding-a-new-field/_static/image5.png)

单击**新建**链接添加新的影片。 请注意，你可以添加一个级别。

[![7_CreateRioII](adding-a-new-field/_static/image7.png)](adding-a-new-field/_static/image6.png)

单击 **“创建”**。 新的影片，包括评级，现在显示在电影列出：

[![7_ourNewMovie_SM](adding-a-new-field/_static/image9.png)](adding-a-new-field/_static/image8.png)

在本部分中您将了解如何修改模型对象和保留数据库与更改同步。 你还了解了一种方法来填充新创建的数据库使用示例数据，因此你还可以尝试方案。 接下来，让我们看一下如何将更丰富的验证逻辑添加到模型类和启用一些业务规则，以强制执行。

>[!div class="step-by-step"]
[上一页](examining-the-edit-methods-and-edit-view.md)
[下一页](adding-validation-to-the-model.md)
