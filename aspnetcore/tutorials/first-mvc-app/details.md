---
title: "检查详细信息和删除方法"
author: rick-anderson
description: "基本 ASP.NET Core MVC 应用中的详细信息控制器方法和视图。"
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/details
ms.openlocfilehash: 4a0004fc79f8e1d334e3acb96b28b2954d19f0a1
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="examining-the-details-and-delete-methods"></a>检查详细信息和删除方法

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

打开电影控制器，并检查 `Details` 方法：

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_details)]

创建此操作方法的 MVC 基架引擎添加显示调用方法的 HTTP 请求的注释。 在此情况下，它是包含三个 URL 段的 GET 请求，这三个段为 `Movies` 控制器、`Details` 方法和 `id` 值。 回顾这些在 Startup.cs 中定义的段。

[!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

EF 可以使用 `SingleOrDefaultAsync` 方法轻松搜索数据。 该方法中内置的一个重要安全功能是，代码会先验证搜索方法已经找到电影，然后再执行操作。 例如，一个黑客可能通过将链接创建的 URL 从 `http://localhost:xxxx/Movies/Details/1` 更改为类似 `http://localhost:xxxx/Movies/Details/12345` 的值（或者不代表任何实际电影的其他值）将错误引入站点。 如果未检查是否有空电影，则应用可能引发异常。

检查 `Delete` 和 `DeleteConfirmed` 方法。

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete)]

请注意，`HTTP GET Delete` 方法不删除指定的电影，而是返回可在其中提交 (HttpPost) 删除的电影视图。 执行删除操作以响应 GET 请求（或者说，执行编辑操作、创建操作或更改数据的任何其他操作）会打开安全漏洞。

删除数据的 `[HttpPost]` 方法命名为 `DeleteConfirmed`，以便为 HTTP POST 方法提供一个唯一的签名或名称。 下面显示了两个方法签名：

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete2)]

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete3)]


公共语言运行时 (CLR) 需要重载方法拥有唯一的参数签名（相同的方法名称但不同的参数列表）。 但是，这里需要两个 `Delete` 方法 -- 一个用于 GET，另一个用于 POST -- 这两个方法拥有相同的参数签名。 （它们都需要接受单个整数作为参数。）

可通过两种方法解决此问题，一种是为方法提供不同的名称。 这正是前面的示例中的基架机制进行的操作。 但是，这会造成一个小问题：ASP.NET 按名称将 URL 段映射到操作方法，如果重命名方法，则路由通常无法找到该方法。 该示例中也提供了解决方案，即向 `DeleteConfirmed` 方法添加 `ActionName("Delete")` 属性。 该属性对路由系统执行映射，以便包括 POST 请求的 /Delete/ 的 URL可找到 `DeleteConfirmed` 方法。

对于名称和签名相同的方法，另一个常用解决方法是手动更改 POST 方法的签名以包括额外（未使用）的参数。 这正是前面的文章中添加 `notUsed` 参数时进行的操作。 这里为了 `[HttpPost] Delete` 方法可以执行同样的操作：

```csharp
// POST: Movies/Delete/6
[ValidateAntiForgeryToken]
public async Task<IActionResult> Delete(int id, bool notUsed)
```

### <a name="publish-to-azure"></a>发布到 Azure

有关如何使用 Visual Studio 将该应用发布到 Azure 的说明，请参阅[使用 Visual Studio 将 ASP.NET Core Web 应用发布到 Azure App Service](xref:tutorials/publish-to-azure-webapp-using-vs)。  此外，还可以从[命令行](xref:tutorials/publish-to-azure-webapp-using-cli)发布应用。

感谢读完这篇 ASP.NET Core MVC 简介。 我们期待你的意见。 完成本教程后，大力推荐了解 [MVC 和 EF Core 入门](xref:data/ef-mvc/intro)。

>[!div class="step-by-step"]
[上一篇](validation.md)
