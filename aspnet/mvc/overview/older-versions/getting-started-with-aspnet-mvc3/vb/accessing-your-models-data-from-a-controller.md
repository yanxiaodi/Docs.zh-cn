---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/accessing-your-models-data-from-a-controller
title: "从控制器 (VB) 访问您的模型的数据 |Microsoft 文档"
author: Rick-Anderson
description: "本教程将教您构建使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，这是一个 ASP.NET MVC Web 应用程序的基础知识..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: cad00de1-3c68-4ff4-a436-54236d449459
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: f1a1db907aa1d0a62af9b363fabfc74ac11acc68
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="accessing-your-models-data-from-a-controller-vb"></a>从控制器 (VB) 访问您的模型的数据
====================
通过[Rick Anderson](https://github.com/Rick-Anderson)

> 本教程将教您构建使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，这是 Microsoft Visual Studio 的免费版本的 ASP.NET MVC Web 应用程序的基础知识。 在开始之前，请确保已安装下面列出的先决条件。 你可以通过单击以下链接安装所有这些： [Web 平台安装程序](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)。 或者，你可以单独安装系统必备组件，使用以下链接：
> 
> - [Visual Studio Web Developer Express SP1 系统必备](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools 更新](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)（运行时 + 工具支持）
> 
> 如果你正在使用 Visual Studio 2010，而不 Visual Web Developer 2010，通过单击以下链接安装必备组件： [Visual Studio 2010 系统必备组件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)。
> 
> Visual Web Developer 项目与 VB.NET 的源代码是可以附带本主题。 [下载 VB.NET 版本](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)。 如果你愿意 C#，切换到[C# 版本](../cs/accessing-your-models-data-from-a-controller.md)本教程。


在本部分中，将创建一个新`MoviesController`类，并编写代码检索影片数据并将其显示在浏览器中使用视图模板。 请确保生成应用程序然后再继续。

右键单击*控制器*文件夹并创建新`MoviesController`控制器。 选择以下选项：

- 控制器名称： **MoviesController**。 （这是默认值）。
- 模板：**控制器，具有读/写操作和视图使用 Entity Framework**。
- 模型类：**电影 (MvcMovie.Models)**。
- 数据上下文类： **MovieDBContext (MvcMovie.Models)**。
- 视图： **Razor (CSHTML)**。 （默认值。）

[![5addMovieController](accessing-your-models-data-from-a-controller/_static/image2.png)](accessing-your-models-data-from-a-controller/_static/image1.png)

单击 **“添加”**。 Visual Web Developer 会创建以下文件和文件夹：

- *MoviesController.vb*文件在项目的*控制器*文件夹。
- A*电影*文件夹在项目的*视图*文件夹。
- *Create.vbhtml，Delete.vbhtml，Details.vbhtml，Edit.vbhtml*，和*Index.vbhtml*在新*Views\Movies*文件夹。

[![5_ScaffoldMovie](accessing-your-models-data-from-a-controller/_static/image4.png)](accessing-your-models-data-from-a-controller/_static/image3.png)

ASP.NET MVC 3 基架机制自动创建 CRUD （创建、 读取、 更新和删除） 操作方法和为你的视图。 你现在具有完全正常运行的 web 应用程序，你可以创建、 列出、 编辑和删除电影条目。

运行应用程序，并浏览到`Movies`控制器通过追加*/Movies*到你的浏览器的地址栏中的 URL。 因为应用程序依赖于默认路由 (在中定义*Global.asax*文件)，浏览器请求`http://localhost:xxxxx/Movies`路由到默认值`Index`操作方法`Movies`控制器。 换而言之，浏览器请求`http://localhost:xxxxx/Movies`实际上是浏览器请求相同`http://localhost:xxxxx/Movies/Index`。 结果是电影、 一个空列表，因为你尚未添加。

![](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="creating-a-movie"></a>创建电影

选择“新建”链接。 输入一些有关影片详细信息，然后单击**创建**按钮。

![](accessing-your-models-data-from-a-controller/_static/image6.png)

单击**创建**按钮使窗体发布到服务器，其中的影片信息保存到数据库中。 然后转向*/Movies* URL，其中你可以看到在列表中新创建的影片。

[![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image8.png)](accessing-your-models-data-from-a-controller/_static/image7.png)

再创建几个其他的电影条目。 试用“编辑”、“详细信息”和“删除”链接，它们均可正常工作。

## <a name="examining-the-generated-code"></a>检查生成的代码

打开*Controllers\MoviesController.vb*文件并检查生成`Index`方法。 电影控制器与的一部分`Index`方法如下所示。

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample1.vb)]

以下行从`MoviesController`类实例化电影数据库上下文，如前面所述。 电影数据库上下文可用于查询、 编辑和删除电影。

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample2.vb)]

对请求`Movies`控制器将返回中的所有条目`Movies`电影数据库表，然后将传递到结果`Index`视图。

## <a name="strongly-typed-models-and-the-model-keyword"></a>强类型模型和@model关键字

之前在本教程中，您了解了如何控制器传递数据或对象视图模板使用到`ViewBag`对象。 `ViewBag`是一个动态对象，提供了简便的后期绑定方法，将信息传递给视图。

ASP.NET MVC 还提供了将强传递的功能类型化数据或查看模板的对象。 此强类型方法允许更好地进行编译时检查的代码和在 Visual Web Developer 编辑器中的更丰富智能感知。 我们将使用此方法`MoviesController`类和*Index.vbhtml*查看模板。

请注意该代码如何创建[ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx)对象时它调用`View`中的帮助器方法`Index`操作方法。 在代码然后传递此`Movies`从控制器到视图的列表：

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample3.vb)]

通过包括`@ModelType`语句视图模板文件的顶部，你可以指定视图需要的对象的类型。 在创建电影控制器时，Visual Web Developer 将自动包括以下`@model`语句的顶部*Index.vbhtml*文件：

[!code-vbhtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.vbhtml)]

这`@ModelType`指令允许你访问的控制器传递到视图中使用的影片列表`Model`强类型的对象。 例如，在*Index.vbhtml*模板，该代码循环访问电影通过这样做，`foreach`通过强类型化的语句`Model`对象：

[!code-vbhtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.vbhtml)]

因为`Model`对象被强类型 (作为`IEnumerable<Movie>`对象)，则每个`item`循环中的对象被类型化为`Movie`。 其他优点，这意味着你获取编译时检查的代码，并在代码编辑器中的 IntelliSense 支持：

[![5_Intellisense](accessing-your-models-data-from-a-controller/_static/image10.png)](accessing-your-models-data-from-a-controller/_static/image9.png)

## <a name="working-with-sql-server-compact"></a>使用 SQL Server Compact

提供的数据库连接字符串指向 entity Framework Code First 检测到`Movies`不存在，因此 Code First 数据库自动创建的数据库。 你可以验证它由查找中已创建*应用\_数据*文件夹。 如果看不到*Movies.sdf*文件中，单击**显示所有文件**按钮**解决方案资源管理器**工具栏上，单击**刷新**按钮，然后再展开*应用\_数据*文件夹。

[![SDF_in_SolnExp](accessing-your-models-data-from-a-controller/_static/image12.png)](accessing-your-models-data-from-a-controller/_static/image11.png)

双击*Movies.sdf*以打开**服务器资源管理器**。 然后展开**表**文件夹以查看已在数据库中创建的表。

> [!NOTE]
> 如果你收到错误，当你双击*Movies.sdf*，请确保已安装**Visual Studio 2010 SP1 Tools for SQL Server Compact 4.0**。 （有关软件的链接，请参阅本系列教程的第 1 部分中的先决条件的列表。）如果你现在安装版本，你将需要关闭并重新打开 Visual Web Developer。


[![DB_explorer](accessing-your-models-data-from-a-controller/_static/image14.png)](accessing-your-models-data-from-a-controller/_static/image13.png)

有两个表，一个用于`Movie`实体集，然后`EdmMetadata`表。 `EdmMetadata`实体框架使用表来确定何时模型和数据库同步。

右键单击`Movies`表，然后选择**显示表数据**以查看你创建的数据。

[![MoviesTable](accessing-your-models-data-from-a-controller/_static/image16.png)](accessing-your-models-data-from-a-controller/_static/image15.png)

右键单击`Movies`表，然后选择**编辑表架构**。

[![EditTableSchema](accessing-your-models-data-from-a-controller/_static/image18.png)](accessing-your-models-data-from-a-controller/_static/image17.png)

![TableSchemaSM](accessing-your-models-data-from-a-controller/_static/image19.png)

请注意如何的架构`Movies`表映射到`Movie`前面创建的类。 Entity Framework Code First 自动创建此架构根据你`Movie`类。

完成后，关闭连接。 （如果不关闭连接，你可能会错误在下次运行项目时）。

[![CloseConnection](accessing-your-models-data-from-a-controller/_static/image21.png)](accessing-your-models-data-from-a-controller/_static/image20.png)

你现在具有数据库和一个简单列表页，以便显示从它的内容。 在下一步的教程中，我们将检查基架的代码的其余部分并添加`SearchIndex`方法和一个`SearchIndex`可以搜索此数据库中的电影的视图。

>[!div class="step-by-step"]
[上一页](adding-a-model.md)
[下一页](examining-the-edit-methods-and-edit-view.md)
