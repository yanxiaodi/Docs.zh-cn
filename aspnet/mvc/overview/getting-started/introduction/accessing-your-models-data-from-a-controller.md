---
uid: mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
title: "从控制器访问您的模型的数据 |Microsoft 文档"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: caa1ba4a-f9f0-4181-ba21-042e3997861d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 91bfa5fe3c5bd3029b7d7c12c8831e1653fb1d2b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="accessing-your-models-data-from-a-controller"></a>从控制器访问您的模型的数据
====================
通过[Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

在本部分中，将创建一个新`MoviesController`类，并编写代码检索影片数据并将其显示在浏览器中使用视图模板。

**生成应用程序**在转到下一步之前。 如果你不生成应用程序，你将收到错误添加控制器。

在解决方案资源管理器，右键单击*控制器*文件夹，然后单击**添加**，然后**控制器**。

![](accessing-your-models-data-from-a-controller/_static/image1.png)

在**添加基架**对话框中，单击**数据与视图，MVC 5 控制器使用 Entity Framework**，然后单击**添加**。

![](accessing-your-models-data-from-a-controller/_static/image2.png)

- 选择**电影 (MvcMovie.Models)**模型类。
- 选择**MovieDBContext (MvcMovie.Models)**数据上下文类。
- 作为控制器名称输入**MoviesController**。

 下图显示已完成对话框。  
  
![](accessing-your-models-data-from-a-controller/_static/image3.png)   

单击 **“添加”**。 （如果收到错误，你可能未生成应用程序，然后开始将控制器添加。）Visual Studio 将创建以下文件和文件夹：

- *MoviesController.cs*文件中*控制器*文件夹。
- A *Views\Movies*文件夹。
- *Create.cshtml，Delete.cshtml，Details.cshtml，Edit.cshtml*，和*Index.cshtml*在新*Views\Movies*文件夹。

Visual Studio 自动创建[CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) （创建、 读取、 更新和删除） 操作方法和为你的视图 （自动创建的 CRUD 操作方法和视图称为基架）。 你现在具有完全正常运行的 web 应用程序，你可以创建、 列出、 编辑和删除电影条目。

运行应用程序并单击**MVC 影片**链接 (或浏览到`Movies`控制器通过追加*/Movies*到你的浏览器的地址栏中的 URL)。 因为应用程序依赖于默认路由 (在中定义*应用\_Start\RouteConfig.cs*文件)，浏览器请求`http://localhost:xxxxx/Movies`路由到默认值`Index`操作方法`Movies`控制器。 换而言之，浏览器请求`http://localhost:xxxxx/Movies`实际上是浏览器请求相同`http://localhost:xxxxx/Movies/Index`。 结果是电影、 一个空列表，因为你尚未添加。

![](accessing-your-models-data-from-a-controller/_static/image4.png)

### <a name="creating-a-movie"></a>创建电影

选择“新建”链接。 输入一些有关影片详细信息，然后单击**创建**按钮。


![](accessing-your-models-data-from-a-controller/_static/image5.png)

> [!NOTE]
> 你不能在 Price 字段中输入小数点或逗号。 若要为使用逗号的非英语区域设置支持 jQuery 验证 (&quot;，&quot;) 对于小数点，和非美国英语的日期格式中，您必须包含*globalize.js*和您的特定*cultures/globalize.cultures.js*文件 (从[https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) 和 JavaScript 使用`Globalize.parseFloat`。 我将介绍如何执行此操作在下一步的教程。 目前只能输入整数，例如 10。


单击**创建**按钮使窗体发布到服务器，其中的影片信息保存到数据库中。 然后转向*/Movies* URL，其中你可以看到在列表中新创建的影片。

![](accessing-your-models-data-from-a-controller/_static/image6.png)

再创建几个其他的电影条目。 试用“编辑”、“详细信息”和“删除”链接，它们均可正常工作。

## <a name="examining-the-generated-code"></a>检查生成的代码

打开*Controllers\MoviesController.cs*文件并检查生成`Index`方法。 电影控制器与的一部分`Index`方法如下所示。

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

对请求`Movies`控制器将返回中的所有条目`Movies`表，然后将传递到结果`Index`视图。 以下行从`MoviesController`类实例化电影数据库上下文，如前面所述。 电影数据库上下文可用于查询、 编辑和删除电影。

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

## <a name="strongly-typed-models-and-the-model-keyword"></a>强类型模型和@model关键字

之前在本教程中，您了解了如何控制器传递数据或对象视图模板使用到`ViewBag`对象。 `ViewBag`是一个动态对象，提供了简便的后期绑定方法，将信息传递给视图。

MVC 还提供了将传递的功能*强*类型化的对象添加到视图模板。 使用此强类型化的方法，更好编译时检查的代码和更丰富[IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx)在 Visual Studio 编辑器中。 Visual Studio 中的基架机制使用这种方法 (即传递*强*类型化的模型) 使用`MoviesController`类和视图模板创建的方法和视图时。

在*Controllers\MoviesController.cs*文件检查生成`Details`方法。 `Details`方法如下所示。

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

`id`参数通常传递作为路由数据，例如`http://localhost:1234/movies/details/1`将设置为电影控制器上，对指定的操作的控制器`details`和`id`为 1。 你可能还传递中使用查询字符串的 id，如下所示：

`http://localhost:1234/movies/details?id=1`

如果`Movie`找到的实例`Movie`的模型传递给`Details`视图：

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample4.cs)]

检查的内容*Views\Movies\Details.cshtml*文件：

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml?highlight=1-2)]

通过包括`@model`语句视图模板文件的顶部，你可以指定视图需要的对象的类型。 创建电影控制器时，Visual Studio 会自动在 Details.cshtml 文件的顶端包括以下 `@model` 语句：

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

此 `@model` 指令使你能够使用强类型的 `Model` 对象访问控制器传递给视图的电影。 例如，在*Details.cshtml*模板，在代码传递到每个电影字段`DisplayNameFor`和[DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx)通过强类型化的 HTML 帮助器`Model`对象。 `Create`和`Edit`方法和查看模板还将传递电影模型对象。

检查*Index.cshtml*视图模板和`Index`中的方法*MoviesController.cs*文件。 请注意该代码如何创建[ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx)对象时它调用`View`中的帮助器方法`Index`操作方法。 在代码然后传递此`Movies`列表中按从`Index`到视图的操作方法：

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample7.cs?highlight=3)]

在创建电影控制器时，Visual Studio 将自动包括以下`@model`语句的顶部*Index.cshtml*文件：

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample8.cshtml)]

这`@model`指令允许你访问的控制器传递到视图中使用的影片列表`Model`强类型的对象。 例如，在*Index.cshtml*模板，该代码循环访问电影通过这样做，`foreach`通过强类型化的语句`Model`对象：

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample9.cshtml?highlight=1,4,7,10,13,16,19-21)]

因为`Model`对象被强类型 (作为`IEnumerable<Movie>`对象)，则每个`item`循环中的对象被类型化为`Movie`。 其他优点，这意味着你获取编译时检查的代码，并在代码编辑器中的 IntelliSense 支持：

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image8.png)

## <a name="working-with-sql-server-localdb"></a>使用 SQL Server LocalDB

提供的数据库连接字符串指向 entity Framework Code First 检测到`Movies`不存在，因此 Code First 数据库自动创建的数据库。 你可以验证它由查找中已创建*应用\_数据*文件夹。 如果看不到*Movies.mdf*文件中，单击**显示所有文件**按钮**解决方案资源管理器**工具栏上，单击**刷新**按钮，然后再展开*应用\_数据*文件夹。

![](accessing-your-models-data-from-a-controller/_static/image9.png)

双击*Movies.mdf*以打开**服务器资源管理器**，然后展开**表**文件夹以查看电影表。 请注意的密钥图标旁边 id。 默认情况下，EF 将名为 ID 为主键的属性。 EF 和 MVC 的详细信息，请参阅 Tom Dykstra 的绝佳教程上[MVC 和 EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。

![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")

右键单击`Movies`表，然后选择**显示表数据**以查看你创建的数据。

![](accessing-your-models-data-from-a-controller/_static/image11.png) 

![](accessing-your-models-data-from-a-controller/_static/image12.png)

右键单击`Movies`表，然后选择**打开表定义**以查看结构该 Entity Framework Code First 为你创建的表格。

![](accessing-your-models-data-from-a-controller/_static/image13.png)

![](accessing-your-models-data-from-a-controller/_static/image14.png)

请注意如何的架构`Movies`表映射到`Movie`前面创建的类。 Entity Framework Code First 自动创建此架构根据你`Movie`类。

完成后，通过右键单击关闭连接*MovieDBContext*并选择**关闭连接**。 （如果不关闭连接，你可能会错误在下次运行项目时）。

![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")

现在你已拥有用于显示、编辑、更新和删除数据的数据库和页面。 在下一步的教程中，我们将检查基架的代码的其余部分并添加`SearchIndex`方法和一个`SearchIndex`可以搜索此数据库中的电影的视图。 在实体框架使用 MVC 的详细信息，请参阅[为 ASP.NET MVC 应用程序创建实体框架数据模型](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。

>[!div class="step-by-step"]
[上一页](creating-a-connection-string.md)
[下一页](examining-the-edit-methods-and-edit-view.md)
