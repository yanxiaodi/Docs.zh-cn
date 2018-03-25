---
uid: web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
title: "要开始使用 ASP.NET Web API 2 (C#)"
author: MikeWasson
description: "HTTP 不再仅仅用于为网页提供服务。 它也是一个功能强大的平台，用于构建公开服务和数据的 Api。 HTTP 是简单、 灵活、 和 ubiq..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/28/2017
ms.topic: article
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
msc.type: authoredcontent
ms.openlocfilehash: d881563cdb6449aada444ef0528061581113a925
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
<a name="get-started-with-aspnet-web-api-2-c"></a>要开始使用 ASP.NET Web API 2 (C#)
====================
通过[Mike Wasson](https://github.com/MikeWasson)

[下载已完成的项目](https://code.msdn.microsoft.com/Sample-code-of-Getting-c56ccb28)

HTTP 不再仅仅用于为网页提供服务。 HTTP 也是一个功能强大的平台，用于构建公开服务和数据的 Api。 HTTP 是简单、 灵活且无处不在。 你可以将几乎任何平台都有 HTTP 库，以便 HTTP 服务可以覆盖广泛的客户端，包括浏览器、 移动设备和传统桌面应用程序。
 
ASP.NET Web API 是一个框架，用于生成 web Api 在.NET Framework 之上。 在本教程中，你将使用 ASP.NET Web API 创建的 web API 返回的产品列表。

 
 ## <a name="software-versions-used-in-the-tutorial"></a>在本教程中使用的软件版本
  
 - [Visual Studio 2017](https://www.visualstudio.com/downloads/)
 - Web API 2

请参阅[使用 ASP.NET Core 和 Visual Studio for Windows 创建一个 web API](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api)本教程的较新版本。

## <a name="create-a-web-api-project"></a>创建一个 Web API 项目

在本教程中，你将使用 ASP.NET Web API 创建的 web API 返回的产品列表。 前端的 web 页使用 jQuery 来显示结果。

![](tutorial-your-first-web-api/_static/image1.png)

启动 Visual Studio 并选择**新项目**从**启动**页。 或从**文件**菜单上，选择**新建**然后**项目**。

在**模板**窗格中，选择**已安装的模板**展开**Visual C#**节点。 下**Visual C#**，选择**Web**。 在项目模板列表中，选择**ASP.NET Web 应用程序**。 将"ProductsApp"的项目，然后单击**确定**。

![](tutorial-your-first-web-api/_static/image2.png)

在**新建 ASP.NET 项目**对话框中，选择**空**模板。 下&quot;将文件夹添加和核心引用&quot;，检查**Web API**。 单击 **“确定”**。

![](tutorial-your-first-web-api/_static/image3.png)

> [!NOTE]
> 你还可以创建 Web API 项目使用&quot;Web API&quot;模板。 Web API 模板使用 ASP.NET MVC 提供 API 帮助页。 我现在对于本教程使用空模板，因为我想要显示而无需 MVC Web API。 一般情况下，你不需要知道 ASP.NET MVC，若要使用 Web API。


## <a name="adding-a-model"></a>添加模型

模型是表示应用程序中的数据的对象。 ASP.NET Web API 可以自动序列化到 JSON、 XML 或某种其他格式，您的模型，然后写入 HTTP 响应消息的正文序列化的数据。 只要客户端可以读取的序列化格式，它可以反序列化对象。 大多数客户端可以分析 XML 或 JSON。 此外，客户端可以指示它想通过 HTTP 请求消息中设置 Accept 标头的格式。

让我们首先创建一个表示产品的简单模型。

如果解决方案资源管理器不可见，请单击**视图**菜单，然后选择**解决方案资源管理器**。 在解决方案资源管理器，右键单击模型文件夹。 从上下文菜单中，选择**添加**然后选择**类**。

![](tutorial-your-first-web-api/_static/image4.png)

将类&quot;产品&quot;。 添加到以下属性`Product`类。

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample1.cs)]

## <a name="adding-a-controller"></a>添加控制器

在 Web API 中，*控制器*是处理 HTTP 请求的对象。 我们将添加可返回的产品的列表或单个产品 id。 指定的控制器

> [!NOTE]
> 如果你已使用 ASP.NET MVC，你已熟悉控制器。 Web API 控制器类似于 MVC 控制器，但继承**ApiController**类而不是**控制器**类。

在**解决方案资源管理器**，右键单击 Controllers 文件夹。 选择**添加**，然后选择**控制器**。

![](tutorial-your-first-web-api/_static/image5.png)

在**添加基架**对话框中，选择**Web API 控制器-空**。 单击 **“添加”**。

![](tutorial-your-first-web-api/_static/image6.png)

在**添加控制器**对话框中，控制器&quot;ProductsController&quot;。 单击 **“添加”**。

![](tutorial-your-first-web-api/_static/image7.png)

基架创建名为 ProductsController.cs 在控制器文件夹中的文件。

![](tutorial-your-first-web-api/_static/image8.png)

> [!NOTE]
> 你不需要将你的控制器放入名为控制器的文件夹。 文件夹名称是只是一种可方便方式来组织你的源文件。


如果没有打开此文件，则双击该文件以打开它。 将此文件中的代码替换为以下：

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample2.cs)]

为了简化本示例，产品都存储在控制器类中的固定数组。 当然，在实际应用中，你将查询数据库或使用其他一些外部数据源。

控制器定义返回产品的两个方法：

- `GetAllProducts`方法返回的产品为整个列表**IEnumerable&lt;产品&gt;**类型。
- `GetProduct`方法查找单个产品 id

就这么简单！ 具有工作 web API。 在控制器上的每个方法对应于一个或多个 Uri:

| 控制器方法 | URI |
| --- | --- |
| GetAllProducts | / api/产品 |
| GetProduct | /api/products/*id* |

有关`GetProduct`方法， *id* URI 中是一个占位符。 例如，若要获取的产品与 ID 为 5，URI 是`api/products/5`。

有关如何 Web API 将 HTTP 请求路由到控制器方法的详细信息，请参阅[路由在 ASP.NET Web API 中](../web-api-routing-and-actions/routing-in-aspnet-web-api.md)。

## <a name="calling-the-web-api-with-javascript-and-jquery"></a>调用 Web API 与 Javascript 和 jQuery

在此部分中，我们将添加使用 AJAX 调用 web API 的 HTML 页。 进行 AJAX 调用，并更新页面使用的结果，我们将使用 jQuery。

在解决方案资源管理器，右键单击该项目并选择**添加**，然后选择**新项**。

![](tutorial-your-first-web-api/_static/image9.png)

在**添加新项**对话框中，选择**Web**节点下的**Visual C#**，然后选择**HTML 页**项。 将该页命名为&quot;index.html&quot;。

![](tutorial-your-first-web-api/_static/image10.png)

将此文件中的所有内容替换为以下：

[!code-html[Main](tutorial-your-first-web-api/samples/sample3.html)]

有多种方法来获取 jQuery。 在此示例中，我使用[Microsoft Ajax CDN](../../../ajax/cdn/overview.md)。 你也可以下载它从[ http://jquery.com/ ](http://jquery.com/)，和 ASP.NET"Web API"项目模板包括 jQuery 以及。

### <a name="getting-a-list-of-products"></a>获取产品的列表

若要获取的产品列表，请将发送到 HTTP GET 请求&quot;/api/产品&quot;。

JQuery [getJSON](http://api.jquery.com/jQuery.getJSON/)函数发送 AJAX 请求。 响应包含 JSON 对象的数组。 `done`函数指定如果请求成功，则调用的回调。 在回调中，我们将更新 DOM 提供的产品信息。

[!code-html[Main](tutorial-your-first-web-api/samples/sample4.html)]

### <a name="getting-a-product-by-id"></a>获取按 ID 的产品

若要获取按 ID 的产品，发送到 HTTP GET 请求&quot;/api/产品/*id*&quot;，其中*id*是产品 id。

[!code-javascript[Main](tutorial-your-first-web-api/samples/sample5.js)]

我们仍调用`getJSON`发送 AJAX 请求，但这次我们将 ID 放在请求 URI 中。 来自此请求的响应是一种产品的 JSON 表示。

## <a name="running-the-application"></a>运行应用程序

按 F5 开始调试应用程序。 网页应如下所示：

![](tutorial-your-first-web-api/_static/image11.png)

若要获取按 ID 的产品，请输入 ID，然后单击搜索:

![](tutorial-your-first-web-api/_static/image12.png)

如果你输入的 ID 无效，服务器将返回 HTTP 错误：

![](tutorial-your-first-web-api/_static/image13.png)

## <a name="using-f12-to-view-the-http-request-and-response"></a>使用 F12 查看 HTTP 请求和响应

当你使用的 HTTP 服务时，它可能非常有用，请参阅 HTTP 请求和请求消息。 可以使用 Internet Explorer 9 中的 F12 开发人员工具来执行此操作。 在 Internet Explorer 9 中，按**F12**打开工具。 单击**网络**选项卡并按**启动捕获**。 现在请转回 web 页面，按**F5**重新加载网页。 Internet Explorer 将捕获在浏览器和 web 服务器之间的 HTTP 流量。 摘要视图显示页的所有网络流量：

![](tutorial-your-first-web-api/_static/image14.png)

找到的项的相对 URI 为"api 中的产品 /"。 选择此项，然后单击**转到详细视图**。 在详细信息视图中，有一些选项卡以查看请求和响应标头和正文。 例如，如果你单击**请求标头**选项卡上，你可以看到客户端请求&quot;应用程序/json&quot; Accept 标头中。

![](tutorial-your-first-web-api/_static/image15.png)

如果单击响应正文选项卡，你可以看到如何产品列表已序列化为 JSON。 其他浏览器具有类似的功能。 另一个有用工具是[Fiddler](http://www.fiddler2.com/fiddler2/)，一个 web 调试代理。 你可以使用 Fiddler，以查看 HTTP 流量，以及构成 HTTP 请求，这将使您可以完全控制的 HTTP 标头在请求中。

## <a name="see-this-app-running-on-azure"></a>请参阅在 Azure 上运行此应用程序

若要查看与实时 web 应用运行的已完成的站点吗？ 只需单击下面的按钮，可以将应用程序的完整版本部署到你的 Azure 帐户。

[![](http://azuredeploy.net/deploybutton.png)](https://deploy.azure.com/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebAPI-ProductsApp#/form/setup)

你需要将此解决方案部署到 Azure 的 Azure 帐户。 如果你还没有帐户，可以使用以下选项：

- [免费建立一个 Azure 帐户](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604)-获取信用额度来试用付费版 Azure 服务，你可以使用和甚至在用完后，最多可以保留帐户和使用免费的 Azure 服务。
- [激活 MSDN 订户权益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604)-MSDN 订阅为你的信用额度可以用于付费版 Azure 服务的每个月。

## <a name="next-steps"></a>后续步骤

- 有关支持 POST、 PUT 和 DELETE 操作，并将写入数据库的 HTTP 服务的更完整示例，请参阅[Entity Framework 6 Web API 2](../data/using-web-api-with-entity-framework/part-1.md)。
- 有关创建基于 HTTP 服务的流畅且高度可响应的 web 应用程序的详细信息，请参阅[ASP.NET 单页面应用程序](../../../single-page-application/index.md)。
- 有关如何将 Visual Studio web 项目部署到 Azure App Service 的信息，请参阅[在 Azure App Service 中创建 ASP.NET web 应用](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)。
