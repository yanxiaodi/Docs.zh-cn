---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
title: "第 4 部分： 模型和数据访问 |Microsoft 文档"
author: jongalloway
description: "本系列教程详细介绍所有生成 ASP.NET MVC 音乐商店示例应用程序所采取的步骤。 第 4 部分介绍模型和数据访问。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: ab55ca81-ab9b-44a0-8700-dc6da2599335
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
msc.type: authoredcontent
ms.openlocfilehash: 727336344493b439130b2cf0ec6e7b5925dd5637
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="part-4-models-and-data-access"></a>第 4 部分： 模型和数据访问
====================
通过[Jon Galloway](https://github.com/jongalloway)

> MVC 音乐商店是，从而引入并说明如何使用 ASP.NET MVC 和 Visual Studio 进行 web 开发分步教程应用程序。  
>   
> MVC 音乐商店是一个轻型示例存储区实现，该类销售音乐集联机，并实现基本的站点管理、 用户登录，和购物车功能。
> 
> 本系列教程详细介绍所有生成 ASP.NET MVC 音乐商店示例应用程序所采取的步骤。 第 4 部分介绍模型和数据访问。


到目前为止，我们已刚刚已"dummy 数据"将从传递我们控制器到我们查看模板。 现在我们已准备好挂钩实际的数据库。 在本教程中我们将讨论如何使用 SQL Server Compact Edition （通常称为 SQL CE） 作为我们数据库引擎。 SQL CE 是基于的免费、 嵌入式、 文件的数据库不需要任何安装或配置，这样，就以进行本地开发真正方便。

## <a name="database-access-with-entity-framework-code-first"></a>数据库访问与实体框架代码的第一个

我们将使用 ASP.NET MVC 3 项目，以便查询和更新数据库中包含的 Entity Framework (EF) 支持。 EF 是一个灵活的对象关系映射 (ORM) 数据的 API，使开发人员能够以一种面向对象的方式存储在数据库中的查询和更新数据。

实体框架版本 4 支持调用的代码优先的开发范例。 代码的第一个选项，可以通过编写简单的类 (也称为 POCO 从"纯旧式"CLR 对象)，创建模型对象，甚至可以从您的类动态创建数据库。

### <a name="changes-to-our-model-classes"></a>对我们的模型类的更改

我们将利用实体框架中的数据库创建功能在本教程中。 我们在此之前，不过，让我们进行一些次要更改到我们在我们将更高版本使用的事件中添加的模型类。

#### <a name="adding-the-artist-model-classes"></a>添加艺术家模型类

因此，我们将添加一个简单的模型类来描述艺术家，我们专辑将与艺术家，相关联。 将新类添加到名为 Artist.cs 使用下面所示的代码的 Models 文件夹中。

[!code-csharp[Main](mvc-music-store-part-4/samples/sample1.cs)]

#### <a name="updating-our-model-classes"></a>更新我们模型类

更新唱片集类，如下所示。

[!code-csharp[Main](mvc-music-store-part-4/samples/sample2.cs)]

接下来，对流派类进行下列更新。

[!code-csharp[Main](mvc-music-store-part-4/samples/sample3.cs)]

### <a name="adding-the-appdata-folder"></a>添加应用程序\_数据文件夹

我们将添加应用程序\_到我们的项目以保存我们 SQL Server Express 数据库文件的数据目录。 应用\_数据是已具有正确的安全访问权限来访问数据库的 ASP.NET 中的特殊目录。 从项目菜单中，选择添加 ASP.NET 文件夹，然后应用\_数据。

![](mvc-music-store-part-4/_static/image1.png)

### <a name="creating-a-connection-string-in-the-webconfig-file"></a>在 web.config 文件中创建连接字符串

以便实体框架知道如何连接到我们的数据库，我们将向网站的配置文件添加几行。 双击位于项目根目录中的 Web.config 文件。

![](mvc-music-store-part-4/_static/image2.png)

滚动到此文件的底部并添加&lt;connectionStrings&gt;部分的正上方的最后一行，如下所示。

[!code-xml[Main](mvc-music-store-part-4/samples/sample4.xml)]

### <a name="adding-a-context-class"></a>添加上下文类

右键单击 Models 文件夹，并添加一个名为 MusicStoreEntities.cs 的新类。

![](mvc-music-store-part-4/_static/image3.png)

此类将表示实体框架数据库上下文，并将处理我们创建、 读取、 更新和删除为我们的操作。 此类的代码所示。

[!code-csharp[Main](mvc-music-store-part-4/samples/sample5.cs)]

就这么简单-没有任何其他配置、 特殊的接口，等等。通过扩展 DbContext 基类，我们 MusicStoreEntities 类是能够处理我们为我们的数据库操作。 现在，我们提供的挂钩，让我们将几个更多属性添加到我们的模型类来利用我们的数据库中的某些其他信息。

### <a name="adding-our-store-catalog-data"></a>添加我们存储目录数据

将"种子"数据添加到新创建的数据库实体框架中，我们将充分利用一项功能。 这将预先填充我们应用商店目录的风格、 艺术家和唱片集的列表。 MvcMusicStore Assets.zip 下载-包含我们之前在本教程中使用的站点设计文件-已具有此种子数据，位于名为代码的文件夹中的类文件。

在代码中 / Models 文件夹中，找到 SampleData.cs 文件并将其放到我们的项目中的 Models 文件夹中，如下所示。

![](mvc-music-store-part-4/_static/image4.png)

现在，我们需要添加一行代码需要了解的有关 SampleData 该类实体框架。 双击以打开它并添加以下行到顶部应用程序项目的根目录中的 Global.asax 文件\_Start 方法。

[!code-csharp[Main](mvc-music-store-part-4/samples/sample6.cs)]

此时，我们已经完成为我们的项目配置实体框架所必需的工作。

## <a name="querying-the-database"></a>查询数据库

现在让我们更新我们 StoreController 以便而不是使用"虚拟数据"它改为调用到我们的数据库以查询其所有的信息。 我们将开始通过将字段声明上**StoreController**来托管实例的名为 storeDB 的 MusicStoreEntities 类：

[!code-csharp[Main](mvc-music-store-part-4/samples/sample7.cs)]

### <a name="updating-the-store-index-to-query-the-database"></a>更新存储索引来查询数据库

MusicStoreEntities 类由实体框架维护，并公开每个表中我们的数据库使用的集合属性。 让我们更新我们 StoreController 索引操作，以检索在我们的数据库中的所有风格。 以前我们执行此操作的硬编码字符串数据。 现在我们可以只需使用实体框架上下文 Generes 集合：

[!code-csharp[Main](mvc-music-store-part-4/samples/sample8.cs)]

发生到我们的视图模板，因为我们仍在返回之前-我们要仅返回实时数据从我们的数据库现在我们返回相同 StoreIndexViewModel 所不需要的任何更改。

当我们试运行项目和访问"/ 存储"URL 时，我们现在将我们的数据库中看到所有风格的列表：

![](mvc-music-store-part-4/_static/image1.jpg)

### <a name="updating-store-browse-and-details-to-use-live-data"></a>更新存储浏览和详细信息，若要使用实时数据

与应用商店/浏览？ 流派 =*[某些流派]*操作方法，我们要搜索的一种风格的名称。 我们仅预期结果，因为我们不应永远都不会为流派同名的两个条目，因此我们可以使用。在 LINQ 查询为适当的流派对象如下的 Single() 扩展 （不要键入这尚未）：

[!code-csharp[Main](mvc-music-store-part-4/samples/sample9.cs)]

单个方法采用 Lambda 表达式作为一个参数，它指定我们希望单个流派对象，以便其名称与我们已定义的值相匹配。 在上面的示例中，我们会匹配 Disco 为名称值中加载单个流派对象。

我们将充分利用一实体框架功能，使我们可以指示检索流派对象时，我们想加载以及其他相关的实体。 此功能称为查询结果调整，并使我们能够减少我们需要用于访问数据库来检索所有我们需要的信息的次数。 我们想要预提取的流派我们检索唱片集，因此我们将更新我们的查询以从 Genres.Include("Albums") 以指示我们要以及相关唱片集都有。 这是更高效，因为它将检索单个数据库请求中我们 Genre 和唱片集的数据。

说明开，下面是我们更新的浏览控制器操作的外观：

[!code-csharp[Main](mvc-music-store-part-4/samples/sample10.cs)]

我们现在可以更新存储浏览视图以显示专辑中每个风格。 打开视图模板 (在中找到 /Views/Store/Browse.cshtml) 并添加唱片集的项目符号列表，如下所示。

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample11.cshtml)]

运行我们的应用程序，并浏览至/存储/浏览？ 流派 = 爵士乐表明，从数据库中，在我们所选风格中显示所有专辑现在拉出我们的结果。

![](mvc-music-store-part-4/_static/image2.jpg)

我们将使将更改为我们 /Store/详细信息 / [id] URL，并将我们 dummy 数据替换为数据库查询，后者将加载唱片集的 ID 匹配的参数值相同。

[!code-csharp[Main](mvc-music-store-part-4/samples/sample12.cs)]

运行我们的应用程序和浏览到 /Store/Details/1 演示了我们的结果从数据库正在请求。

![](mvc-music-store-part-4/_static/image5.png)

现在，我们存储详细信息页面设置为按唱片集 ID 显示唱片集，让我们更新**浏览**视图以链接到详细信息视图。 我们将使用次 Html.ActionLink，严格按照我们未从应用商店索引链接到应用商店浏览在上一节的末尾。 浏览视图的完整源如下所示。

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample13.cshtml)]

我们现在能够从我们的应用商店页面浏览到流派页，其中列出了可用的专辑，并通过单击唱片集，我们可以查看该唱片集的详细信息。

![](mvc-music-store-part-4/_static/image6.png)

>[!div class="step-by-step"]
[上一页](mvc-music-store-part-3.md)
[下一页](mvc-music-store-part-5.md)
