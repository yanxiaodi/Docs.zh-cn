---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-details-and-delete-methods
title: "检查的详细信息和删除方法 |Microsoft 文档"
author: Rick-Anderson
description: "注意： 本教程的更新的版本此处提供了使用 ASP.NET MVC 5 和 Visual Studio 2013。 它是更安全，请按照和演示要简单得多..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 11425ff3-09fc-4efa-be9a-b53bce503460
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: f3c56356aaa595e200a16fe0045a8b00dc5823b7
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="examining-the-details-and-delete-methods"></a>检查的详细信息和删除方法
====================
通过[Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > 本教程的更新的版本可[此处](../../getting-started/introduction/getting-started.md)使用 ASP.NET MVC 5 和 Visual Studio 2013。 它是更安全，请按照要简单得多，并演示更多的功能。


在本教程的此部分中，你将检查自动生成`Details`和`Delete`方法。

## <a name="examining-the-details-and-delete-methods"></a>检查的详细信息和删除方法

打开`Movie`控制器并检查`Details`方法。

![](examining-the-details-and-delete-methods/_static/image1.png)

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample1.cs)]

MVC 基架引擎，可用于创建此操作添加注释，显示调用方法的 HTTP 请求。 在这种情况下它是`GET`请求包含三个 URL 段，`Movies`控制器，`Details`方法和一个`ID`值。

代码首先可以轻松地搜索数据使用`Find`方法。 生成到方法的一个重要的安全功能是代码验证`Find`代码尝试使用它执行任何操作之前，已找到一部电影方法。 例如，黑客无法将错误引入到站点通过更改由来自链接的 URL`http://localhost:xxxx/Movies/Details/1`为类似于`http://localhost:xxxx/Movies/Details/12345`（或不表示实际的电影的一些其他值）。 如果你没有检查 null 电影，null 电影将导致数据库错误。

检查 `Delete` 和 `DeleteConfirmed` 方法。

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample2.cs?highlight=17)]

请注意，`HTTP Get``Delete`方法不会删除指定的电影，则返回其中可以提交电影的视图 (`HttpPost`) 删除... 执行删除操作以响应 GET 请求（或者说，执行编辑操作、创建操作或更改数据的任何其他操作）会打开安全漏洞。 有关此的详细信息，请参阅博客条目，Stephen Walther [ASP.NET MVC 提示 #46-不要使用删除的链接，因为他们创建的安全漏洞](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx)。

删除数据的 `HttpPost` 方法命名为 `DeleteConfirmed`，以便为 HTTP POST 方法提供一个唯一的签名或名称。 下面显示了两个方法签名：

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample3.cs)]

公共语言运行时 (CLR) 需要重载方法拥有唯一的参数签名（相同的方法名称但不同的参数列表）。 但是，此处你需要两个删除方法-一个 get-，一个用于 POST 都具有相同的参数签名。 （它们都需要接受单个整数作为参数。）

若要对此项进行排序，你可以执行几个事项。 之一是为方法提供不同的名称。 这正是前面的示例中的基架机制进行的操作。 但是，这会造成一个小问题：ASP.NET 按名称将 URL 段映射到操作方法，如果重命名方法，则路由通常无法找到该方法。 该示例中也提供了解决方案，即向 `DeleteConfirmed` 方法添加 `ActionName("Delete")` 属性。 这有效地执行映射路由的系统，以使 URL，包括*/Delete/*的 post 请求将查找`DeleteConfirmed`方法。

另一种常见的方法以避免问题具有相同的名称和签名的方法是人为地更改要包括未使用的参数的 POST 方法的签名。 例如，一些开发人员添加的参数类型`FormCollection`传递给 POST 方法中，然后只需不使用参数：

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample4.cs)]

## <a name="summary"></a>摘要

你现在具有一个完整的 ASP.NET MVC 应用程序在本地的 DB 数据库中存储数据。 你可以创建、 读取、 更新、 删除和搜索电影。

![](examining-the-details-and-delete-methods/_static/image2.png)

## <a name="next-steps"></a>后续步骤

已生成和测试 web 应用程序后下, 一步是将其提供给其他人通过 Internet 使用。 若要做到这一点，你必须将其部署到 web 宿主提供程序。 Microsoft 提供了可用的 web 宿主中的最多 10 个网站[免费 Azure 试用帐户](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604)。 我建议你接下来请按照我的教程[将包含成员资格、 OAuth 和 SQL 数据库的安全 ASP.NET MVC 应用程序部署到 Windows Azure 网站](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)。 绝佳教程是 Tom Dykstra 的中间级[为 ASP.NET MVC 应用程序创建实体框架数据模型](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。 [Stackoverflow](http://stackoverflow.com/help)和[ASP.NET MVC 论坛](https://forums.asp.net/1146.aspx)是出色放置询问的问题。 请按照[我](https://twitter.com/RickAndMSFT)以便您可以获取我的最新教程上的更新在 twitter 上。

反馈是受欢迎。

- [Rick Anderson](https://blogs.msdn.com/rickAndy) twitter:[@RickAndMSFT](https://twitter.com/RickAndMSFT)  
- [Scott Hanselman](http://www.hanselman.com/blog/) twitter:[@shanselman](https://twitter.com/shanselman)

>[!div class="step-by-step"]
[上一篇](adding-validation-to-the-model.md)
