---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
title: "引入的 ASP.NET Web Pages-删除数据库数据 |Microsoft 文档"
author: tfitzmac
description: "本教程演示了如何删除单个数据库条目。 它假定你已完成的 ASP.NET Web pa。 在更新数据库数据通过一系列..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/02/2018
ms.topic: article
ms.assetid: 75b5c1cf-84bd-434f-8a86-85c568eb5b09
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
msc.type: authoredcontent
ms.openlocfilehash: 5bc92b5d40e7a55dcd730d552c71031d913b277e
ms.sourcegitcommit: 281f0c614543a6c3db565ea4655b70fe49b61d84
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2018
---
<a name="introducing-aspnet-web-pages---deleting-database-data"></a>引入了 ASP.NET Web 页-删除数据库数据
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> 本教程演示了如何删除单个数据库条目。 它假定你已完成通过系列[更新数据库数据中的 ASP.NET Web Pages](updating-data.md)。
> 
> 你将学习：
> 
> - 如何从记录列表中选择单独的记录。
> - 如何从数据库中删除单个记录。
> - 如何检查特定按钮被单击窗体中。
>   
> 
> 功能/技术讨论：
> 
> - `WebGrid`帮助器。
> - SQL`Delete`命令。
> - `Database.Execute`方法来运行 SQL`Delete`命令。


## <a name="what-youll-build"></a>你将生成

在前面的教程，您学习了如何更新现有的数据库记录。 本教程类似，但在于而不是更新记录，你将删除它。 进程是大致相同，只不过删除是更简单，因此本教程将会比较短。

在*电影*页上，你将更新`WebGrid`帮助器以便它显示**删除**旁边每个影片随附链接**编辑**前面添加的链接。

![显示一个删除链接，以便每个电影的电影页](deleting-data/_static/image1.png)

包含编辑，当你单击**删除**链接，它将你带到不同的页的影片信息已在窗体：

![删除具有显示一部电影的电影页](deleting-data/_static/image2.png)

然后，可以单击按钮以永久删除的记录。

## <a name="adding-a-delete-link-to-the-movie-listing"></a>将删除链接添加到影片列表

你将首先，通过添加**删除**链接到`WebGrid`帮助器。 此链接是类似于**编辑**你在前面的教程中添加的链接。

打开*Movies.cshtml*文件。

更改`WebGrid`页上通过添加列的正文中的标记。 下面是已修改的标记：

[!code-html[Main](deleting-data/samples/sample1.html?highlight=9-10)]

新的列是这样一个：

[!code-html[Main](deleting-data/samples/sample2.html)]

网格配置的方式，**编辑**列是在网格中最左侧和**删除**列是最右边。 (没有后的逗号`Year`列现在，如果你未注意到，。)无需进行任何特殊有关这些链接列从何处，并轻松无法将其放彼此相邻。 在这种情况下，它们单独以使它们难以获取混淆。

![电影页，带有编辑和详细信息的链接标记以显示它们彼此不是](deleting-data/_static/image3.png)

新的列显示一个链接 (`<a>`元素) 的文本显示的是"删除"。 链接目标的 (其`href`特性) 是具有最终解析为此 URL，类似于下面的代码`id`为每个电影不同的值：

[!code-css[Main](deleting-data/samples/sample3.css)]

此链接将调用一个名为页*DeleteMovie*并将其传递所选电影的 ID。

本教程将不会进行此链接的构造方式，有关详细信息，因为它是几乎与**编辑**从前面的教程的链接 ([更新数据库数据中的 ASP.NET Web Pages](updating-data.md))。

## <a name="creating-the-delete-page"></a>创建删除页

现在，你可以创建将为目标的页面**删除**在网格中的链接。

> [!NOTE] 
> 
> **重要**首先选择要删除的记录，然后使用单独的页面和按钮以确认该过程的方法便非常重要的安全。 因为你已阅读在前面的教程，使*任何*到你的网站的更改排序应*始终*进行使用窗体&mdash;，即，使用 HTTP POST 操作。 如果你进行可能只需通过单击的链接 （即使用 GET 操作） 更改的站点，人员无法向网站发出简单请求和删除你的数据。 即使为搜索引擎爬网程序，那么编制索引的你的站点可能无意中删除数据，只需通过按照以下链接。
> 
> 当你的应用程序允许用户更改的记录时，你必须以进行编辑仍向用户显示该记录。 但你可能想要跳过此步骤中的删除一条记录。 不要通过跳过该步骤。 （它也是很有帮助的用户，请参阅记录并确认它们要删除的记录，应将它们。）
> 
> 在后续教程组中，你将了解如何添加登录功能，因此用户将需要在删除一条记录之前登录。


创建一个名为页*DeleteMovie.cshtml* ，并将替换为以下标记文件中：

[!code-cshtml[Main](deleting-data/samples/sample4.cshtml)]

此标记就像是*EditMovie*页，而不是使用文本框的只不过 (`<input type="text">`)，标记包括`<span>`元素。 此处没有任何可编辑。 你所要做的只是显示，以便用户可以确保它们正在删除右电影的电影详细信息。

标记已包含允许用户返回到影片列表页的链接。

作为 in *EditMovie*隐藏字段中存储页上，选中的电影的 ID。 （它传递到页面首先作为查询字符串值。）没有`Html.ValidationSummary`将显示验证错误的调用。 在这种情况下，错误可能是没有电影 ID 已传递到页或电影 ID 无效。 如果有人运行而不事先选择中的一个影片的此页可能出现此情况*电影*页。

该按钮标题**删除电影**，并且其名称属性设置为`buttonDelete`。 `name`属性将使用在代码中，用于标识提交了表单的按钮。

你将需要编写代码以 1） 时将首先显示的页读取电影详细信息，并且用户单击按钮时，2） 实际删除影片。

## <a name="adding-code-to-read-a-single-movie"></a>添加代码以读取单个电影

在顶部*DeleteMovie.cshtml*页上，添加下面的代码块：

[!code-cshtml[Main](deleting-data/samples/sample5.cshtml)]

此标记是中的相应代码相同*EditMovie*页。 它获取查询字符串外的电影 ID，并使用 ID 来从数据库中读取一条记录。 代码包含验证测试 (`IsInt()`和`row != null`) 若要确保传递到页面的电影 ID 是否有效。

请记住，此代码仅应运行第一次运行页面。 你不想要重新读取从数据库的电影记录，当用户单击**删除电影**按钮。 因此，用于读取电影位于显示测试的代码`if(!IsPost)` &mdash; ，即*如果请求不是 post 操作 （提交窗体）*。

## <a name="adding-code-to-delete-the-selected-movie"></a>添加代码以删除所选的电影

若要删除的电影，用户单击按钮时，添加以下代码的右大括号内`@`块：

[!code-csharp[Main](deleting-data/samples/sample6.cs)]

此代码是相似的代码更新现有记录，但更简单。 基本上，代码将运行 SQL`Delete`语句。

 作为 in *EditMovie*页上，代码都处于`if(IsPost)`块。 此时，`if()`条件是稍微有些复杂： 

[!code-csharp[Main](deleting-data/samples/sample7.cs)]

有以下两种情况。 第一种是，在正在提交页面，如你所见之前&mdash; `if(IsPost)`。

第二个条件是`!Request["buttonDelete"].IsEmpty()`，表示的请求都有一个名为对象`buttonDelete`。 不可否认，它是一种测试哪个按钮提交了表单的间接方法。 如果窗体包含多个提交按钮，则只有被单击的按钮名称将出现在请求中。 因此，在逻辑上，如果特定的按钮的名称将出现在请求&mdash;规定在代码中，如果该按钮不为空或&mdash;即提交了表单的按钮。

`&&`运算符表示"和"（逻辑与）。 因此整个`if`条件是...

*此请求是 post （而不是第一次请求）*  
  
 AND  
  
 `buttonDelete`*按钮已提交了表单的按钮。*

（事实上，此页） 此表单包含只包含一个按钮，因此的其他测试`buttonDelete`从技术上讲不是必需的。 但是，你将要执行的操作将永久删除数据。 因此，你想要为确保尽可能只在用户显式请求它时要执行该操作。 例如，假设你展开此页更高版本，并向其添加其他按钮。 仅当删除电影的代码将运行即使如此，`buttonDelete`按钮被单击。

作为 in *EditMovie*页上，从隐藏的字段中获取的 ID，然后运行 SQL 命令。 语法`Delete`语句是：

`DELETE FROM table WHERE ID = value`

务必包括`WHERE`子句和 id。 如果您忽略 WHERE 子句，*将删除表中的所有记录*。 如您所见，你向 SQL 命令的 ID 值通过使用占位符传递。

## <a name="testing-the-movie-delete-process"></a>测试电影删除过程

现在，你可以测试。 运行*电影*页，然后单击**删除**旁边影片。 当*DeleteMovie*页出现时，单击**删除电影**。

![突出显示的删除电影按钮删除影片页](deleting-data/_static/image4.png)

单击按钮时，代码删除电影，并返回到影片列表。 存在可以搜索已删除的电影并确认它已被删除。

## <a name="coming-up-next"></a>接下来

下一教程演示如何在你的站点上提供的所有页，常见的外观和布局。

## <a name="complete-listing-for-movie-page-updated-with-delete-links"></a>电影页 （经过更新带有删除链接） 的完整列表

[!code-cshtml[Main](deleting-data/samples/sample8.cshtml)]

## <a name="complete-listing-for-deletemovie-page"></a>DeleteMovie 页的完整列表

[!code-cshtml[Main](deleting-data/samples/sample9.cshtml)]

## <a name="additional-resources"></a>其他资源

- [使用 Razor 语法的 ASP.NET Web 编程简介](../introducing-razor-syntax-c.md)
- [SQL DELETE 语句](http://www.w3schools.com/sql/sql_delete.asp)W3Schools 站点上

>[!div class="step-by-step"]
[上一页](updating-data.md)
[下一页](layouts.md)
