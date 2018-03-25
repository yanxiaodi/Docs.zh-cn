---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
title: "从控制器访问您的模型的数据 |Microsoft 文档"
author: Rick-Anderson
description: "注意： 本教程的更新的版本此处提供了使用 ASP.NET MVC 5 和 Visual Studio 2013。 它是更安全，请按照和演示要简单得多..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 61e0206d-7f32-4018-992d-0a51b48b37dc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: f323fe37da739d957a609dc7ca4e71a3c3ab475e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="accessing-your-models-data-from-a-controller"></a>从控制器访问您的模型的数据
====================
通过[Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > 本教程的更新的版本可[此处](../../getting-started/introduction/getting-started.md)使用 ASP.NET MVC 5 和 Visual Studio 2013。 它是更安全，请按照要简单得多，并演示更多的功能。


在本部分中，将创建一个新`MoviesController`类，并编写代码检索影片数据并将其显示在浏览器中使用视图模板。

**生成应用程序**在转到下一步之前。

右键单击*控制器*文件夹并创建新`MoviesController`控制器。 生成你的应用程序之前，不会显示以下选项。 选择以下选项：

- 控制器名称： **MoviesController**。 （这是默认值。 )
- 模板： **MVC 控制器，具有读/写操作和视图，使用 Entity Framework**。
- 模型类：**电影 (MvcMovie.Models)**。
- 数据上下文类： **MovieDBContext (MvcMovie.Models)**。
- 视图： **Razor (CSHTML)**。 （默认值。）

![AddScaffoldedMovieController](accessing-your-models-data-from-a-controller/_static/image1.png)

单击 **“添加”**。 Visual Studio Express 创建以下文件和文件夹：

- *MoviesController.cs*文件在项目的*控制器*文件夹。
- A*电影*文件夹在项目的*视图*文件夹。
- *Create.cshtml，Delete.cshtml，Details.cshtml，Edit.cshtml*，和*Index.cshtml*在新*Views\Movies*文件夹。

ASP.NET MVC 4 自动创建 CRUD （创建、 读取、 更新和删除） 操作方法和为你的视图 （自动创建的 CRUD 操作方法和视图称为基架）。 你现在具有完全正常运行的 web 应用程序，你可以创建、 列出、 编辑和删除电影条目。

运行应用程序，并浏览到`Movies`控制器通过追加*/Movies*到你的浏览器的地址栏中的 URL。 因为应用程序依赖于默认路由 (在中定义*Global.asax*文件)，浏览器请求`http://localhost:xxxxx/Movies`路由到默认值`Index`操作方法`Movies`控制器。 换而言之，浏览器请求`http://localhost:xxxxx/Movies`实际上是浏览器请求相同`http://localhost:xxxxx/Movies/Index`。 结果是电影、 一个空列表，因为你尚未添加。

![](accessing-your-models-data-from-a-controller/_static/image2.png)

### <a name="creating-a-movie"></a>创建电影

选择“新建”链接。 输入一些有关影片详细信息，然后单击**创建**按钮。

![](accessing-your-models-data-from-a-controller/_static/image3.png)

单击**创建**按钮使窗体发布到服务器，其中的影片信息保存到数据库中。 然后转向*/Movies* URL，其中你可以看到在列表中新创建的影片。

![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image4.png "IndexWhenHarryMet")

再创建几个其他的电影条目。 试用“编辑”、“详细信息”和“删除”链接，它们均可正常工作。

## <a name="examining-the-generated-code"></a>检查生成的代码

打开*Controllers\MoviesController.cs*文件并检查生成`Index`方法。 电影控制器与的一部分`Index`方法如下所示。

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

以下行从`MoviesController`类实例化电影数据库上下文，如前面所述。 电影数据库上下文可用于查询、 编辑和删除电影。

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

对请求`Movies`控制器将返回中的所有条目`Movies`电影数据库表，然后将传递到结果`Index`视图。

## <a name="strongly-typed-models-and-the-model-keyword"></a>强类型模型和@model关键字

之前在本教程中，您了解了如何控制器传递数据或对象视图模板使用到`ViewBag`对象。 `ViewBag`是一个动态对象，提供了简便的后期绑定方法，将信息传递给视图。

ASP.NET MVC 还提供了将强传递的功能类型化数据或查看模板的对象。 此强类型方法允许更好地进行编译时检查的代码和在 Visual Studio 编辑器中的更丰富智能感知。 使用此方法的 Visual Studio 中的基架机制`MoviesController`类和视图模板创建的方法和视图时。

在*Controllers\MoviesController.cs*文件检查生成`Details`方法。 电影控制器与的一部分`Details`方法如下所示。

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs?highlight=3,8)]

如果`Movie`找到的实例`Movie`的模型传递给详细信息视图。 检查的内容*Views\Movies\Details.cshtml*文件。

通过包括`@model`语句视图模板文件的顶部，你可以指定视图需要的对象的类型。 创建电影控制器时，Visual Studio 会自动在 Details.cshtml 文件的顶端包括以下 `@model` 语句：

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.cshtml)]

此 `@model` 指令使你能够使用强类型的 `Model` 对象访问控制器传递给视图的电影。 例如，在*Details.cshtml*模板，在代码传递到每个电影字段`DisplayNameFor`和[DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx)通过强类型化的 HTML 帮助器`Model`对象。 创建和编辑方法和视图模板还传递电影模型对象。

检查*Index.cshtml*视图模板和`Index`中的方法*MoviesController.cs*文件。 请注意该代码如何创建[ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx)对象时它调用`View`中的帮助器方法`Index`操作方法。 在代码然后传递此`Movies`从控制器到视图的列表：

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample5.cs?highlight=3)]

在创建电影控制器时，Visual Studio Express 自动包括以下`@model`语句的顶部*Index.cshtml*文件：

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

这`@model`指令允许你访问的控制器传递到视图中使用的影片列表`Model`强类型的对象。 例如，在*Index.cshtml*模板，该代码循环访问电影通过这样做，`foreach`通过强类型化的语句`Model`对象：

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample7.cshtml?highlight=1,4,7,10,13,16,19-21)]

因为`Model`对象被强类型 (作为`IEnumerable<Movie>`对象)，则每个`item`循环中的对象被类型化为`Movie`。 其他优点，这意味着你获取编译时检查的代码，并在代码编辑器中的 IntelliSense 支持：

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="working-with-sql-server-localdb"></a>使用 SQL Server LocalDB

提供的数据库连接字符串指向 entity Framework Code First 检测到`Movies`不存在，因此 Code First 数据库自动创建的数据库。 你可以验证它由查找中已创建*应用\_数据*文件夹。 如果看不到*Movies.mdf*文件中，单击**显示所有文件**按钮**解决方案资源管理器**工具栏上，单击**刷新**按钮，然后再展开*应用\_数据*文件夹。

![](accessing-your-models-data-from-a-controller/_static/image6.png)

双击*Movies.mdf*以打开**数据库资源管理器**，然后展开**表**文件夹以查看电影表。

![DB_explorer](accessing-your-models-data-from-a-controller/_static/image7.png "DB_explorer")

> [!NOTE]
> 如果未显示的数据库资源管理器，从**工具**菜单上，选择**连接到数据库**，然后取消**选择数据源**对话框。 这将强制打开的数据库资源管理器。


> [!NOTE]
> 如果你使用的 VWD 或 Visual Studio 2010 并获取错误类似于以下下列任意项：
> 
> - 数据库 C:\Webs\MVC4\MVCMOVIE\MVCMOVIE\APP\_DATA\MOVIES。MDF 无法打开，因为它是版本 706。 此服务器支持版本 655 及更早版本。 不支持降级路径。
> - &quot;用户代码未处理 InvalidOperation 异常&quot;提供的 SqlConnection 未指定初始目录。
> 
> 你需要安装[SQL Server Data Tools](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx)和[LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0)。 验证`MovieDBContext`在上一页上指定的连接字符串。


右键单击`Movies`表，然后选择**显示表数据**以查看你创建的数据。

![](accessing-your-models-data-from-a-controller/_static/image8.png)

右键单击`Movies`表，然后选择**打开表定义**以查看结构该 Entity Framework Code First 为你创建的表格。

![](accessing-your-models-data-from-a-controller/_static/image9.png "MoviesTable")

![](accessing-your-models-data-from-a-controller/_static/image10.png)

请注意如何的架构`Movies`表映射到`Movie`前面创建的类。 Entity Framework Code First 自动创建此架构根据你`Movie`类。

完成后，通过右键单击关闭连接*MovieDBContext*并选择**关闭连接**。 （如果不关闭连接，你可能会错误在下次运行项目时）。

![](accessing-your-models-data-from-a-controller/_static/image11.png "CloseConnection")

你现在具有数据库和一个简单列表页，以便显示从它的内容。 在下一步的教程中，我们将检查基架的代码的其余部分并添加`SearchIndex`方法和一个`SearchIndex`可以搜索此数据库中的电影的视图。

>[!div class="step-by-step"]
[上一页](adding-a-model.md)
[下一页](examining-the-edit-methods-and-edit-view.md)
