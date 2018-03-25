---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
title: "URL 路由 |Microsoft 文档"
author: Erikre
description: "本系列教程将教您生成有关我们使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 的 ASP.NET Web 窗体应用程序的基础知识..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 4f4bf092-c400-471f-a876-78fda0417890
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
msc.type: authoredcontent
ms.openlocfilehash: 279617e4ebb475d935c0d1e01e08a3a2def0f9e9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="url-routing"></a>URL 路由
====================
通过[艾力克 Reitan](https://github.com/Erikre)

[下载 Wingtip Toys 示例项目 (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)或[下载电子书 (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> 本系列教程将教您构建使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for Web 的 ASP.NET Web 窗体应用程序的基础知识。 Visual Studio 2013[包含 C# 源代码项目](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)是可以附带本系列教程。


在本教程中，你将要修改 Wingtip Toys 示例应用程序来支持 URL 路由。 路由允许 web 应用程序使用友好、 更轻松地请记住，以及更好地支持按搜索引擎的 Url。 本教程以前一教程"成员身份和管理"上构建，并为 Wingtip Toys 教程系列的一部分。

## <a name="what-youll-learn"></a>你将学习：

- 如何注册为 ASP.NET Web 窗体应用程序的路由。
- 如何将路由添加到网页。
- 如何从数据库以支持路由选择数据。

## <a name="aspnet-routing-overview"></a>ASP.NET 路由概述

URL 路由允许你配置应用程序可以接受请求不映射到物理文件的 Url。 请求 URL 是只需用户输入到其浏览器在网站上查找页的 URL。 使用路由来定义在语义上对用户有意义以及可帮助搜索引擎搜索引擎优化 (SEO) 使用的 Url。

默认情况下，Web 窗体模板包括[ASP.NET 友好 Url](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/)。 大部分基本的路由工作将由使用实现*友好 Url*。 但是，在本教程中，你将添加自定义路由功能。

在自定义 URL 路由之前, Wingtip Toys 示例应用程序可以将链接到产品使用以下 URL:

`https://localhost:44300/ProductDetails.aspx?productID=2`

通过自定义 URL 路由，Wingtip Toys 示例应用程序将链接到相同的产品使用易于阅读 URL:

`https://localhost:44300/Product/Convertible%20Car`

### <a name="routes"></a>路由

路由是映射到处理程序的 URL 模式。 处理程序可以是物理文件，例如 Web 窗体应用程序中的.aspx 文件。 处理程序也可以处理请求的类。 若要定义路由，请通过指定的 URL 模式、 处理程序中，和 （可选） 单击路由名称创建路由类的实例。

您通过添加到应用程序添加路由`Route`为对静态对象`Routes`属性`RouteTable`类。 路由属性是`RouteCollection`对象，用于存储应用程序的所有路由。

### <a name="url-patterns"></a>URL 模式

URL 模式可以包含文本值和变量 （也称为 URL 参数） 的占位符。 文本和占位符位于由斜杠分隔的 URL 段 (`/`) 字符。

发出对 web 应用程序的请求后，URL 解析为段和占位符，并且变量的值提供给请求处理程序。 此过程是相似的方式分析查询字符串中的数据并将其传递给请求处理程序。 在这两种情况下，变量信息是包含在 URL 中，并传递给中键 / 值对形式的处理程序。 查询字符串键和值是在 URL 中。 对于路由，密钥是 URL 模式中定义的占位符名称和都位于 URL 中只使用的值。

你可以在 URL 模式中，来定义通过将它们括在大括号占位符 (`{`和`}`)。 你可以定义多个占位符段中，但必须按某一文字值分隔占位符。 例如，`{language}-{country}/{action}`是有效的路由模式。 但是，`{language}{country}/{action}`不是有效的模式，因为没有文本值或占位符之间的分隔符。 因此，路由无法确定分离值语言占位符的值为国家/地区占位符的位置。

### <a name="mapping-and-registering-routes"></a>映射和注册路由

可以包括 Wingtip Toys 示例应用程序页面的路由之前，你必须在应用程序启动时注册路由。 若要注册路由，你将修改`Application_Start`事件处理程序。

1. 在**解决方案资源管理器**的 Visual Studio 中，找到并打开*Global.asax.cs*文件。
2. 添加代码以到黄色突出显示*Global.asax.cs*文件，如下所示：   

    [!code-csharp[Main](url-routing/samples/sample1.cs?highlight=30-31,34-46)]

当 Wingtip Toys 示例应用程序启动时，它将调用`Application_Start`事件处理程序。 此事件处理程序中，末尾`RegisterCustomRoutes`调用方法。 `RegisterCustomRoutes`方法将每个路由添加通过调用`MapPageRoute`方法`RouteCollection`对象。 使用路由名称，路由 URL 和物理 URL 来定义路由。

第一个参数 ("`ProductsByCategoryRoute`") 是路由名称。 它用于在需要时调用路由。 第二个参数 ("`Category/{categoryName}`") 定义可以是动态的 URL 基于代码友好替换。 将填充一个数据生成基于数据的链接时，你可以使用此路由。 一个路由，如下所示：

[!code-csharp[Main](url-routing/samples/sample2.cs)]

路由的第二个参数包含指定的大括号的动态值 (`{ }`)。 在这种情况下，`categoryName`是一个变量，用于确定正确的路由路径。

> [!NOTE] 
> 
> **Optional**
> 
> 你可能会发现更轻松地管理你的代码通过移动`RegisterCustomRoutes`到单独的类的方法。 在*逻辑*文件夹中，创建一个单独`RouteActions`类。 移动上述`RegisterCustomRoutes`方法从*Global.asax.cs*到新的文件`RoutesActions`类。 使用`RoleActions`类和`createAdmin`作为示例，了解如何调用的方法`RegisterCustomRoutes`方法从*Global.asax.cs*文件。


你可能还会发现`RegisterRoutes`方法调用使用`RouteConfig`的开始处对象`Application_Start`事件处理程序。 进行此调用以实现默认路由。 创建使用 Visual Studio 的 Web 窗体模板的应用程序时，它是作为默认代码。

## <a name="retrieving-and-using-route-data"></a>检索和使用路线数据

如上所述，可以定义路由。 添加到代码`Application_Start`中的事件处理程序*Global.asax.cs*文件加载的可定义的路由。

### <a name="setting-routes"></a>设置路由

路由要求你添加附加代码。 在本教程中，你将使用模型绑定来检索`RouteValueDictionary`生成使用数据控件中的数据的路由时使用的对象。 `RouteValueDictionary`对象将包含属于产品的特定类别的产品名称的列表。 对于基于的数据和路由每个产品创建链接。

#### <a name="enable-routes-for-categories-and-products"></a>启用路由的类别和产品

接下来，你将更新应用程序使用`ProductsByCategoryRoute`来确定正确的路由，以便将包含在每个产品类别链接。 你将更新*ProductList.aspx*页以包含每个产品的路由的链接。 链接将显示之前更改，但链接现在将使用 URL 路由。

1. 在**解决方案资源管理器**，打开*Site.Master*页上，如果尚未打开。
2. 更新**ListView**控件名为"`categoryList`"使用以黄色突出显示的更改，因此标记会显示，如下所示：   

    [!code-aspx[Main](url-routing/samples/sample3.aspx?highlight=7-9)]
3. 在**解决方案资源管理器**，打开*ProductList.aspx*页。
4. 更新`ItemTemplate`元素*ProductList.aspx*因此标记，如下所示显示黄色突出显示的更新的页：   

    [!code-aspx[Main](url-routing/samples/sample4.aspx?highlight=6-9,14-16)]
5. 打开的代码隐藏*ProductList.aspx.cs*并将以下命名空间添加为用黄色突出显示：  

    [!code-csharp[Main](url-routing/samples/sample5.cs?highlight=9)]
6. 替换`GetProducts`方法的代码隐藏 (*ProductList.aspx.cs*) 替换为以下代码：   

    [!code-csharp[Main](url-routing/samples/sample6.cs)]

#### <a name="add-code-for-product-details"></a>将代码添加有关产品详细信息

现在，更新代码隐藏 (*ProductDetails.aspx.cs*) 为*ProductDetails.aspx*页后，可以使用路由数据。 请注意，新`GetProduct`方法还接受的用户在其标有书签的链接上的用例，使用较旧的不友好，非路由 URL 的查询字符串值。

1. 替换`GetProduct`方法的代码隐藏 (*ProductDetails.aspx.cs*) 替换为以下代码：   

    [!code-csharp[Main](url-routing/samples/sample7.cs)]

## <a name="running-the-application"></a>运行应用程序

你可以运行应用程序现在以查看更新的路由。

1. 按**F5**运行 Wingtip Toys 示例应用程序。  
 浏览器将打开并显示*Default.aspx*页。
2. 单击**产品**页顶部的链接。  
 上显示的所有产品*ProductList.aspx*页。 浏览器的显示 （使用端口号） 的以下 URL:  
    `https://localhost:44300/ProductList`
3. 接下来，单击**汽车**页面顶部附近的类别链接。  
 上显示仅汽车*ProductList.aspx*页。 浏览器的显示 （使用端口号） 的以下 URL:  
    `https://localhost:44300/Category/Cars`
4. 单击列在页中包含名称的第一辆车的链接 ("**可转换汽车**") 以显示产品详细信息。  
 浏览器的显示 （使用端口号） 的以下 URL:  
    `https://localhost:44300/Product/Convertible%20Car`
5. 接下来，输入到浏览器中的以下非路由 URL （使用端口号）：  
    `https://localhost:44300/ProductDetails.aspx?productID=2`  
 该代码仍然会识别包含查询字符串，其中用户都具有标有书签的链接的情况的 URL。

## <a name="summary"></a>摘要

在本教程中，你添加的类别和产品的路由。 你已学习如何与使用模型绑定的数据控件集成路由。 在下一步的教程中，你将实现全局错误处理。

## <a name="additional-resources"></a>其他资源

[ASP.NET 友好的 Url](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/)  
[将包含成员资格、 OAuth 和 SQL 数据库的安全的 ASP.NET Web 窗体应用程序部署到 Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure 的免费试用版](https://azure.microsoft.com/pricing/free-trial/)

>[!div class="step-by-step"]
[上一页](membership-and-administration.md)
[下一页](aspnet-error-handling.md)
