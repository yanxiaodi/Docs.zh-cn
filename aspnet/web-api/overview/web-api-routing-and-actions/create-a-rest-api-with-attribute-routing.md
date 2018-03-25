---
uid: web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
title: "使用 ASP.NET Web API 2 中的属性路由创建 REST API |Microsoft 文档"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2013
ms.topic: article
ms.assetid: 23fc77da-2725-4434-99a0-ff872d96336b
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
msc.type: authoredcontent
ms.openlocfilehash: c1d0b3e1644ef7f9ebb4be74c3fdf3df90cf3537
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="create-a-rest-api-with-attribute-routing-in-aspnet-web-api-2"></a>使用 ASP.NET Web API 2 中的路由的属性创建 REST API
====================
通过[Mike Wasson](https://github.com/MikeWasson)

Web API 2 支持一种新型的路由，调用*的属性路由*。 有关的属性路由的一般概述，请参阅[Web API 2 中的属性路由](attribute-routing-in-web-api-2.md)。 在本教程中，你将使用的属性路由以创建用于丛书的集合的 REST API。 API 将支持以下操作：

| 操作 | 示例 URI |
| --- | --- |
| 获取所有书籍的列表。 | / api/丛书 |
| 获取一本书的 id。 | /api/books/1 |
| 获取书的详细信息。 | /api/books/1/details |
| 获取按风格的书籍的列表。 | /api/books/fantasy |
| 按发布日期中获取的书籍的列表。 | /api/books/date/2013-02-16 /api/books/date/2013/02/16 （备用窗体） |
| 获取由特定作者的丛书的列表。 | /api/authors/1/books |

所有方法都是只读的 （HTTP GET 请求）。

对于数据层中，我们将使用实体框架。 簿记录将具有以下字段：

- Id
- 标题
- 风格
- 发布日期
- 价格
- 描述
- 作者 Id （到作者表的外键）

但是，对于大多数请求，API 将返回此数据 （标题、 author 和风格） 的子集。 若要获取完整的记录，客户端请求`/api/books/{id}/details`。

## <a name="prerequisites"></a>系统必备

[Visual Studio 2017](https://www.visualstudio.com/vs/) Community、 Professional 或 Enterprise 版本。

## <a name="create-the-visual-studio-project"></a>创建 Visual Studio 项目

通过运行 Visual Studio 来启动。 从**文件**菜单上，选择**新建**，然后选择**项目**。

在**模板**窗格中，选择**已安装的模板**展开**Visual C#**节点。 下**Visual C#**，选择**Web**。 在项目模板列表中，选择**ASP.NET MVC 4 Web 应用程序**。 将项目&quot;BooksAPI&quot;。

![](create-a-rest-api-with-attribute-routing/_static/image1.png)

在**新建 ASP.NET 项目**对话框中，选择**空**模板。 在"添加文件夹和核心引用"，选择**Web API**复选框。 单击**创建项目**。

![](create-a-rest-api-with-attribute-routing/_static/image2.png)

这将创建为 Web API 功能配置的主干项目。

### <a name="domain-models"></a>域模型

接下来，添加用于域模型的类。 在解决方案资源管理器，右键单击模型文件夹。 选择**添加**，然后选择**类**。 将此类命名为 `Author`。

![](create-a-rest-api-with-attribute-routing/_static/image3.png)

将替换为以下 Author.cs 中的代码：

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample1.cs)]

现在，添加名为的另一个类`Book`。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample2.cs)]

### <a name="add-a-web-api-controller"></a>添加一个 Web API 控制器

在此步骤中，我们将添加作为数据层使用 Entity Framework 的 Web API 控制器。

按 Ctrl+Shift+B 生成项目。 实体框架使用反射来发现的属性的模型，因此它需要编译的程序集，若要创建数据库架构。

在解决方案资源管理器，右键单击 Controllers 文件夹。 选择**添加**，然后选择**控制器**。

![](create-a-rest-api-with-attribute-routing/_static/image4.png)

在**添加基架**对话框中，选择"Web API 2 控制器具有读/写操作使用 Entity Framework。"

[![](create-a-rest-api-with-attribute-routing/_static/image6.png)](create-a-rest-api-with-attribute-routing/_static/image5.png)

在**添加控制器**对话框中，为**控制器名称**，输入&quot;BooksController&quot;。 选择&quot;使用异步控制器操作&quot;复选框。 有关**模型类**，选择&quot;簿&quot;。 (如果看不到`Book`类列在下拉列表中，则应确保生成项目。)然后单击"+"按钮。

![](create-a-rest-api-with-attribute-routing/_static/image7.png)

单击**添加**中**新建数据上下文**对话框。

![](create-a-rest-api-with-attribute-routing/_static/image8.png)

单击**添加**中**添加控制器**对话框。 基架添加一个名为类`BooksController`，它定义的 API 控制器。 它还添加了一个名为类`BooksAPIContext`Models 文件夹中定义的数据上下文用于实体框架。

![](create-a-rest-api-with-attribute-routing/_static/image9.png)

### <a name="seed-the-database"></a>植入到数据库

从工具菜单中，选择**库程序包管理器**，然后选择**程序包管理器控制台**。

在 Package Manager Console 窗口中，输入以下命令：

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample3.ps1)]

此命令创建一个 Migrations 文件夹并将添加名为 Configuration.cs 的新代码文件。 打开此文件并添加以下代码`Configuration.Seed`方法。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample4.cs)]

在 Package Manager Console 窗口中，键入以下命令。

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample5.ps1)]

这些命令创建一个本地数据库，并调用 Seed 方法来填充数据库。

![](create-a-rest-api-with-attribute-routing/_static/image10.png)

## <a name="add-dto-classes"></a>添加 DTO 类

如果你运行应用程序现在，并将 GET 请求发送到 /api/books/1，响应类似于以下。 （我添加缩进以提高可读性。）

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample6.json)]

相反，我想要此请求返回的字段的子集。 此外，我想让它返回作者的姓名，而不是作者 id。 为了实现此目的，我们将修改控制器方法以返回*数据传输对象*(DTO) 而不是 EF 模型。 DTO 是仅用于携带数据的对象。

在解决方案资源管理器，右键单击该项目并选择**添加** | **新文件夹**。 将文件夹命名为&quot;Dto&quot;。 添加一个名为类`BookDto`到 Dto 文件夹中，替换为以下定义：

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample7.cs)]

添加名为的另一个类`BookDetailDto`。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample8.cs)]

接下来，更新`BooksController`类返回`BookDto`实例。 我们将使用[Queryable.Select](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx)到项目的方法`Book`实例到`BookDto`实例。 以下是控制器类更新的代码。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample9.cs)]

> [!NOTE]
> 我删除了`PutBook`， `PostBook`，和`DeleteBook`方法，因为本教程不需要它们。


现在如果你运行应用程序，并请求 /api/books/1，响应正文应如下所示：

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample10.json)]

## <a name="add-route-attributes"></a>添加路由属性

接下来，我们将转换要使用的属性路由的控制器。 首先，添加**RoutePrefix**到控制器属性。 此属性定义在此控制器上的所有方法的初始 URI 段。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample11.cs?highlight=1)]

然后添加**[Route]**特性以控制器操作，如下所示：

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample12.cs?highlight=1,7)]

每个控制器方法的路由模板是前缀加上在指定的字符串**路由**属性。 有关`GetBook`方法，路由模板包括参数化的字符串&quot;{id: int}&quot;，它匹配如果 URI 段包含一个整数值。

| 方法 | 路由模板 | 示例 URI |
| --- | --- | --- |
| `GetBooks` | "api/书籍" | `http://localhost/api/books` |
| `GetBook` | "api/丛书 / {id: int}" | `http://localhost/api/books/5` |

## <a name="get-book-details"></a>获取书详细信息

若要获取簿详细信息，客户端将发送 GET 请求到`/api/books/{id}/details`，其中*{id}*是书籍的 ID。

将以下方法添加到 `BooksController` 类。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample13.cs)]

如果请求`/api/books/1/details`，响应类似如下所示：

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample14.json)]

## <a name="get-books-by-genre"></a>获取按风格的丛书

若要获取在特定风格的书籍的列表，客户端将发送 GET 请求到`/api/books/genre`，其中*流派*是流派的名称。 （例如 `/get/books/fantasy`。）

添加以下方法`BooksController`。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample15.cs)]

此处我们要定义包含 URI 模板中的 {流派} 参数的路由。 请注意，Web API 是能够区分这些两个 Uri，并将它们路由到不同的方法：

`/api/books/1`

`/api/books/fantasy`

这是因为`GetBook`方法包含的"id"段必须为整数值的约束：

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample16.cs?highlight=1)]

如果请求 /api/books/fantasy，响应如下所示：

`[ { "Title": "Midnight Rain", "Author": "Ralls, Kim", "Genre": "Fantasy" }, { "Title": "Maeve Ascendant", "Author": "Corets, Eva", "Genre": "Fantasy" }, { "Title": "The Sundered Grail", "Author": "Corets, Eva", "Genre": "Fantasy" } ]`

## <a name="get-books-by-author"></a>获取由作者的丛书

若要获取特定的作者的书籍的列表，客户端将发送 GET 请求到`/api/authors/id/books`，其中*id*是作者的 ID。

添加以下方法`BooksController`。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample17.cs)]

此示例是有趣因为&quot;丛书&quot;是处理的子资源&quot;作者&quot;。 此模式是 RESTful Api 中十分常见。

波形符 （~） 路由模板中的重写中的路由前缀**RoutePrefix**属性。

## <a name="get-books-by-publication-date"></a>获取按发布日期的丛书

若要按发布日期中获取的书籍的列表，客户端将发送 GET 请求到`/api/books/date/yyyy-mm-dd`，其中*yyyy-月-日*是日期。

下面是执行此操作的一种方法：

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample18.cs)]

`{pubdate:datetime}`参数约束以匹配**DateTime**值。 此方法有效，但实际更多的权限不是我们想要。 例如，这些 Uri 还将匹配路由：

`/api/books/date/Thu, 01 May 2008`

`/api/books/date/2000-12-16T00:00:00`

没有任何错误与允许这些 Uri。 但是，可以通过将正则表达式约束添加到路由模板，路由限制到特定的格式：

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample19.cs?highlight=1)]

在窗体中的仅限现在日期&quot;yyyy-月-日&quot;将匹配。 请注意，我们不使用正则表达式来验证我们获得的实际日期。 当尝试将转换到的 URI 段 Web API 处理**DateTime**实例。 无效的日期如"2012年-47-99 将无法进行转换，并且客户端将收到 404 错误。

你还可以支持斜杠分隔符 (`/api/books/date/yyyy/mm/dd`) 通过添加另一个**[Route]**与其他正则表达式的属性。

[!code-html[Main](create-a-rest-api-with-attribute-routing/samples/sample20.html)]

没有细微但重要此详述。 第二个路由模板具有通配符 (\*) 开头的 {pubdate} 参数：

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample21.json)]

这将告知路由引擎该 {pubdate} 应匹配的 uri 的其余部分。 默认情况下，模板参数与单个 URI 段相匹配。 在这种情况下，我们希望 {pubdate} 跨越多个 URI 段：

`/api/books/date/2013/06/17`

## <a name="controller-code"></a>控制器代码

下面是 BooksController 类的完整代码。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample22.cs)]

## <a name="summary"></a>摘要

在设计你的 API 的 Uri 的属性路由提供的更多的控制和更大的灵活性。
