---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
title: "第 5 部分： 编辑窗体和模板化 |Microsoft 文档"
author: jongalloway
description: "本系列教程详细介绍所有生成 ASP.NET MVC 音乐商店示例应用程序所采取的步骤。 第 5 部分介绍如何编辑窗体和模板化。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 6b09413a-6d6a-425a-87c9-629f91b91b28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
msc.type: authoredcontent
ms.openlocfilehash: cde6fe133291254531a797a434a4b2cdd226dd5f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="part-5-edit-forms-and-templating"></a>第 5 部分： 编辑窗体和模板化
====================
通过[Jon Galloway](https://github.com/jongalloway)

> MVC 音乐商店是，从而引入并说明如何使用 ASP.NET MVC 和 Visual Studio 进行 web 开发分步教程应用程序。  
>   
> MVC 音乐商店是一个轻型示例存储区实现，该类销售音乐集联机，并实现基本的站点管理、 用户登录，和购物车功能。
> 
> 本系列教程详细介绍所有生成 ASP.NET MVC 音乐商店示例应用程序所采取的步骤。 第 5 部分介绍如何编辑窗体和模板化。


在过去的章中，我们已将数据从我们的数据库加载并显示它。 在本章中，我们将启用编辑数据。

## <a name="creating-the-storemanagercontroller"></a>创建 StoreManagerController

我们将首先通过创建新的控制器调用**StoreManagerController**。 此控制器中，我们将充分利用 ASP.NET MVC 3 Tools 更新中提供的基架功能。 设置添加控制器对话框中的选项，如下所示。

![](mvc-music-store-part-5/_static/image1.png)

单击添加按钮时，你将看到的 ASP.NET MVC 3 基架机制为你进行大量工作：

- 它创建新的 StoreManagerController 与本地的实体框架变量
- 它将 StoreManager 文件夹添加到项目的 Views 文件夹
- 它将添加 Create.cshtml、 Delete.cshtml、 Details.cshtml、 Edit.cshtml 和 Index.cshtml 视图中，强类型化为唱片集类

![](mvc-music-store-part-5/_static/image2.png)

新的 StoreManager 控制器类包括 CRUD （创建、 读取、 更新、 删除） 知道如何处理与唱片集的控制器操作模型类，并使用我们的实体框架上下文来数据库访问。

## <a name="modifying-a-scaffolded-view"></a>修改基架的视图

请务必记住，虽然我们已生成此代码，它是标准的 ASP.NET MVC 代码，就像我们已在本指南中已编写一样。 它具有为了节省你将会花费的时间编写样本控制器代码和手动创建强类型化的视图，但这不是您可能会看到的前面加上在注释中有关如何不得更改导致可怕警告的生成代码的类型代码。 这是你的代码，并且希望您来对其进行更改。

因此，让我们开始到 StoreManager 索引视图快速编辑 (/ Views/StoreManager/Index.cshtml)。 此视图将显示一个表，该表列出了在我们的存储与编辑唱片集 / 删除的链接，和的详细信息包括唱片集的公共属性。 我们将删除 AlbumArtUrl 字段中，因为它不是在此显示中非常有用。 在&lt;表&gt;部分的查看代码中，删除&lt;th&gt;和&lt;td&gt;元素周围 AlbumArtUrl 引用，如下面突出显示的行所示：

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample1.cshtml)]

修改的视图代码将如下所示：

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample2.cshtml)]

## <a name="a-first-look-at-the-store-manager"></a>首先查看一下商店经理

现在运行应用程序，并浏览到/StoreManager /。 此时将显示存储管理器索引我们刚修改，在具有编辑详细信息，以及删除链接的存储区中显示的唱片集的列表。

![](mvc-music-store-part-5/_static/image3.png)

单击编辑链接唱片集，包括 Genre 和艺术家下拉列表中显示与字段的编辑窗体。

![](mvc-music-store-part-5/_static/image4.png)

单击"返回到列表中"链接，在底部，然后单击唱片集的详细信息链接。 此时将显示各个唱片集的详细信息。

![](mvc-music-store-part-5/_static/image5.png)

同样，单击列表链接回，然后单击删除链接。 此时将显示确认对话框中，显示唱片集详细信息，并询问我们是否确保我们想要将其删除。

![](mvc-music-store-part-5/_static/image6.png)

单击底部的删除按钮，将删除唱片集，并使你返回到显示删除唱片集的索引页。

我们还没有完成与存储管理器，但我们无控制器和查看代码的 CRUD 操作从启动。

## <a name="looking-at-the-store-manager-controller-code"></a>查看存储管理器控制器代码

存储管理器控制器包含大量的代码。 让我们看一下这从上到下。 控制器包含一个 MVC 控制器，以及对我们的模型命名空间的引用的某些标准命名空间。 控制器有 MusicStoreEntities，由每个控制器操作用于数据访问的专用实例。

[!code-csharp[Main](mvc-music-store-part-5/samples/sample3.cs)]

### <a name="store-manager-index-and-details-actions"></a>存储管理器 Index 和 Details 操作

索引视图检索的专辑，包括每个唱片集的引用的流派和艺术家信息，正如我们以前看到的应用商店浏览方法上工作时的列表。 索引视图是以下对链接的对象的引用，以便它可以显示每个唱片集的流派名称和艺术家名称，以便在控制器正在高效且查询在原始请求此信息。

[!code-csharp[Main](mvc-music-store-part-5/samples/sample4.cs)]

StoreManager 控制器的详细信息控制器操作起作用，我们编写了以前为-它会询问是否唱片集的存储控制器的详细信息操作完全相同通过使用 find （） 方法的 ID，然后将其返回到视图。

[!code-csharp[Main](mvc-music-store-part-5/samples/sample5.cs)]

### <a name="the-create-action-methods"></a>创建操作方法

创建操作方法是从大到目前为止，我们已了解稍有不同的因为它们处理窗体输入。 当用户第一次访问时 /StoreManager/创建/他们将看到一个空白的表单。 此 HTML 页将包含&lt;窗体&gt;元素包含下拉列表中和文本框中输入它们可以在其中输入唱片集的详细信息的元素。

用户填写唱片集窗体值后，他们可以按"保存"按钮以提交这些更改发回应用程序以保存在数据库中。 当用户按"保存"按钮&lt;窗体&gt;将执行回 /StoreManager/创建/URL HTTP POST 和提交&lt;窗体&gt;HTTP POST 的一部分的值。

ASP.NET MVC 使我们能够轻松地通过使我们能够实现两个不同的"创建"操作方法在我们 StoreManagerController 类 – 另一个用于处理初始的 HTTP GET 浏览到 /StoreManager/Create 拆分这两个 URL 调用方案的逻辑/ URL 和其他处理提交的更改 HTTP POST。

### <a name="passing-information-to-a-view-using-viewbag"></a>将信息传递给使用 ViewBag 的视图

我们之前在本教程中中, 已使用 ViewBag，但尚未讨论太了解它。 ViewBag 使得我们可以将信息传递给视图，而无需使用强类型化的模型对象。 在这种情况下，我们编辑 HTTP GET 控制器操作需要将这两种风格和专业人员的列表传递到窗体来填充下拉列表，并执行该操作的最简单方法是以使其恢复为 ViewBag 项。

ViewBag 是动态对象，这意味着你可以无需编写代码来定义这些属性键入 ViewBag.Foo 或 ViewBag.YourNameHere。 在这种情况下，控制器代码使用 ViewBag.GenreId 和 ViewBag.ArtistId 以便提交处理该窗体的下拉列表中值将 GenreId 和 ArtistId，是将设置它们的唱片集属性。

这些下拉列表中值返回到使用此时对象，它只需为该目的构建窗体。 这是使用如下代码：

[!code-csharp[Main](mvc-music-store-part-5/samples/sample6.cs)]

如你所见操作方法代码中，三个参数用于创建此对象：

- 将显示下拉列表中的项的列表。 请注意，这不是仅仅是一个字符串-我们正在传递风格的列表。
- 下一个参数传递给此时是选择值。 此此时如何知道如何预选择列表中的项。 这将会更容易了解何时我们了解一下编辑窗体中，这是非常类似。
- 最后一个参数是要显示的属性。 在这种情况下，这指示该 Genre.Name 属性什么将向用户显示。

这一点，然后，创建 HTTP GET 操作是非常简单-两个 SelectLists 添加到 ViewBag，并没有任何模型对象传递到窗体 （因为它尚未尚未创建）。

[!code-csharp[Main](mvc-music-store-part-5/samples/sample7.cs)]

### <a name="html-helpers-to-display-the-drop-downs-in-the-create-view"></a>HTML 帮助器，在创建视图中显示相应的删除列表

由于我们已讨论的下拉列表值传递到该视图的方式，让我们快速了解一下视图以查看显示这些值的方式。 在查看代码 (/ Views/StoreManager/Create.cshtml)，你将看到进行以下调用以显示流派放下。

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample8.cshtml)]

这称为的 HTML 帮助器的实用工具方法，后者将执行一项常见视图任务。 HTML 帮助程序是在使我们视图代码保持简洁、 更具可读性中非常有用。 Html.DropDownList 帮助器提供的 ASP.NET MVC，但正如我们看到更高版本是可以创建我们自己帮助器查看代码，我们将在我们的应用程序中重复使用。

只需会告知其中到 get 列表后，若要显示，和哪些值 （如果有） 应预先选择的两部分内容-Html.DropDownList 调用。 第一个参数，GenreId，指示下拉列表来查找名 GenreId 为模型或 ViewBag 中的值。 第二个参数用于指示要显示的值为最初在下拉列表中选择。 由于此窗体是创建窗体，没有要预先选择的值和传递 String.Empty。

### <a name="handling-the-posted-form-values"></a>处理发布窗体值

如之前所述，有两个与每个窗体关联的操作方法。 第一个处理 HTTP GET 请求，并显示窗体。 第二个处理 HTTP POST 请求，将包含提交窗体值。 请注意，控制器操作 [HttpPost] 属性，它指示 ASP.NET MVC 它应仅响应 HTTP POST 请求。

[!code-csharp[Main](mvc-music-store-part-5/samples/sample9.cs)]

此操作具有四个职责：

- 1. 读取窗体值
- 2. 检查是否窗体值传递的任何验证规则
- 3. 如果表单提交有效，保存数据并显示更新的列表
- 4. 如果表单提交不是有效的则重新显示具有验证错误的窗体

#### <a name="reading-form-values-with-model-binding"></a>使用模型绑定的读取窗体值

控制器操作正在处理包括标题、 价格和 AlbumArtUrl GenreId 和 ArtistId 值 （从下拉列表） 和文本框中值的表单提交。 可以直接访问窗体值时，更好的方法是使用内置于 ASP.NET MVC 的模型绑定功能。 当控制器操作所需模型类型作为参数时，则 ASP.NET MVC 将尝试填充该类型使用窗体输入 （以及路由和查询字符串值） 的对象。 这是通过寻找的值名称匹配的模型对象的属性，例如当设置新唱片集对象的 GenreId 值时，它会查找 GenreId 同名的输入。 创建视图使用 ASP.NET MVC 中的标准方法时，将始终使用属性名称作为输入的字段名称，因此这字段名称将仅匹配呈现窗体。

#### <a name="validating-the-model"></a>验证模型

通过简单调用 ModelState.IsValid 验证模型。 我们尚未将任何验证规则添加到我们唱片集的类，但-我们将执行在某种程度-因此现在此检查没有太多工作要做。 重要的是此 ModelStat.IsValid 检查将适应我们置于我们的模型，以便将来对验证规则的更改将不需要任何更新到的控制器操作代码的验证规则。

#### <a name="saving-the-submitted-values"></a>保存的已提交的值

如果表单提交通过验证后，就可以将值保存到数据库。 使用实体框架，只需将模型添加到专辑集合并调用 SaveChanges。

[!code-csharp[Main](mvc-music-store-part-5/samples/sample10.cs)]

实体框架生成合适的 SQL 命令，来保留的值。 在保存后的数据，我们将重定向回唱片集的列表，我们可以看到我们的更新。 这是通过返回 RedirectToAction 具有我们要显示执行的控制器操作的名称。 在这种情况下，这是 Index 方法。

#### <a name="displaying-invalid-form-submissions-with-validation-errors"></a>显示具有验证错误的无效的窗体提交

在无效的窗体输入的情况下的下拉列表中值添加到 ViewBag （如 HTTP GET） 和绑定的模型值会传递回视图以显示。 验证错误将自动显示使用@Html.ValidationMessageFor的 HTML 帮助器。

#### <a name="testing-the-create-form"></a>测试创建窗体

若要测试这 out，运行的应用程序和浏览到 /StoreManager/创建 /-这将显示返回的 StoreController 创建 HTTP GET 方法的空白窗体。

填写一些值，并单击创建按钮以提交该表单。

![](mvc-music-store-part-5/_static/image7.png)

![](mvc-music-store-part-5/_static/image8.png)

### <a name="handling-edits"></a>处理编辑

编辑操作对 （HTTP GET 和 HTTP POST） 都非常类似于我们刚刚在查看过的创建操作方法。 由于编辑方案涉及使用现有的唱片集，编辑 HTTP GET 方法将加载唱片集基于"id"参数传入通过路由。 如我们之前介绍了中的详细信息的控制器操作，这段代码用于检索通过 AlbumId 唱片集都是相同的。 创建与 / HTTP GET 方法，通过 ViewBag 将返回的下拉列表值。 这使得我们可以将唱片集作为我们模型对象返回到视图 （其中强类型化为唱片集类） 同时通过 ViewBag 传递其他数据 （例如风格的列表）。

[!code-csharp[Main](mvc-music-store-part-5/samples/sample11.cs)]

编辑 HTTP POST 操作是非常类似于创建的 HTTP POST 操作。 唯一的区别是而不是将新唱片集添加到数据库。唱片集集合中，我们发现唱片集的当前实例使用 db。Entry(album) 并将其状态设置为已修改。 这将告知我们正在修改现有唱片集而不是创建一个新实体框架。

[!code-csharp[Main](mvc-music-store-part-5/samples/sample12.cs)]

我们可以通过运行应用程序并浏览到/StoreManger/，然后单击唱片集的编辑链接来测试此项。

![](mvc-music-store-part-5/_static/image9.png)

此时将显示通过编辑 HTTP GET 方法显示的编辑窗体。 填写一些值，并单击保存按钮。

![](mvc-music-store-part-5/_static/image10.png)

这将窗体发送、 保存值，并返回我们到唱片集列表中，显示已更新值。

![](mvc-music-store-part-5/_static/image11.png)

### <a name="handling-deletion"></a>处理删除

删除遵循同一模式，编辑以及创建、 使用一个控制器操作以显示确认窗体中，以及另一个控制器操作处理提交窗体。

HTTP GET 删除控制器操作正是我们之前的存储管理器详细信息控制器操作相同。

[!code-csharp[Main](mvc-music-store-part-5/samples/sample13.cs)]

我们为唱片集类型，使用删除视图内容模板显示了一个强类型的窗体。

![](mvc-music-store-part-5/_static/image12.png)

删除模板显示所有字段的模型，但我们可以很多简化该列表。 将在 /Views/StoreManager/Delete.cshtml 中的查看代码更改为以下。

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample14.cshtml)]

此时将显示一条简化的删除确认消息。

![](mvc-music-store-part-5/_static/image13.png)

单击删除按钮使窗体发布回执行 DeleteConfirmed 操作的服务器。

[!code-csharp[Main](mvc-music-store-part-5/samples/sample15.cs)]

我们 HTTP POST 删除控制器操作将执行以下操作：

- 1. 加载按 ID 唱片集
- 2. 将其删除唱片集并保存更改
- 3. 重定向到索引中，显示唱片集已从列表中删除

若要测试这一点，运行应用程序，并浏览到 /StoreManager。 从列表中选择唱片集，然后单击删除链接。

![](mvc-music-store-part-5/_static/image14.png)

此时将显示我们删除确认屏幕。

![](mvc-music-store-part-5/_static/image15.png)

单击删除按钮移除唱片集，并返回我们为存储管理器索引页，可显示唱片集已被删除。

![](mvc-music-store-part-5/_static/image16.png)

### <a name="using-a-custom-html-helper-to-truncate-text"></a>使用自定义 HTML 帮助程序进行截断操作文本

我们与我们的存储管理器索引页面提供一个潜在问题。 我们唱片集的标题和艺术家名称的属性都可以是足够长，无法引发关闭我们表格式设置。 我们将创建用于使我们能够轻松地截断这些属性和我们的视图中的其他属性的自定义 HTML 帮助。

![](mvc-music-store-part-5/_static/image17.png)

Razor 的@helper语法变得很容易在您的视图中创建您自己使用的帮助器函数。 打开 /Views/StoreManager/Index.cshtml 视图并添加以下代码直接@model行。

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample16.cshtml)]

此帮助程序方法采用一个字符串，以允许的最大长度。 帮助器提供的文本短于指定的长度时，将其作为输出-是。 如果它的长度，然后它将截断的文本，并且"..."剩余时间内呈现。

现在我们可以使用我们截断的帮助器以确保唱片集标题和艺术家名称属性不超过 25 个字符。 使用我们新截断帮助器的完整视图代码如下所示。

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample17.cshtml)]

现在我们浏览 /StoreManager/ URL，专辑和标题将保留下面我们最大长度。

![](mvc-music-store-part-5/_static/image18.png)

注意： 这将显示简单的情况下的创建和使用程序的帮助程序在一个视图中。 若要了解有关创建可以在整个站点使用的帮助器的详细信息，请参阅我的博客文章： [http://bit.ly/mvc3-helper-options](http://bit.ly/mvc3-helper-options)


>[!div class="step-by-step"]
[上一页](mvc-music-store-part-4.md)
[下一页](mvc-music-store-part-6.md)
