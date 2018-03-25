---
uid: web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
title: "在 ASP.NET Web API 2 中测试控制器的单元 |Microsoft 文档"
author: MikeWasson
description: "本主题介绍一些特定技术进行单元测试 Web API 2 中的控制器。 在阅读本主题之前, 可能需要阅读教程单元..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/11/2014
ms.topic: article
ms.assetid: 43a6cce7-a3ef-42aa-ad06-90d36d49f098
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: bda5148a4c1553d70f3173de66371fbb8576e83f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="unit-testing-controllers-in-aspnet-web-api-2"></a>在 ASP.NET Web API 2 中测试控制器的单元
====================
通过[Mike Wasson](https://github.com/MikeWasson)

> 本主题介绍一些特定技术进行单元测试 Web API 2 中的控制器。 在阅读本主题之前, 可能需要阅读本教程[单元测试 ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md)，其中说明了如何将单元测试项目添加到你的解决方案。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教程中使用的软件版本
> 
> - [Visual Studio 2017](https://www.visualstudio.com/vs/)
> - Web API 2
> - [Moq](https://github.com/Moq) 4.5.30

> [!NOTE]
> 我使用 Moq，但相同的思想适用于任何模拟框架。 Moq 4.5.30 （和更高版本） 支持 Visual Studio 2017、 Roslyn 和.NET 4.5 和更高版本。

在单位测试中的常见模式是&quot;排列 act 断言&quot;:

- 排列： 设置测试运行的任何必备组件。
- Act： 执行测试。
- 断言： 验证测试成功完成。

在准备步骤中，你通常将使用 mock 或存根对象。 最小化依赖项，数以便测试侧重于测试的一件事情。

下面是一些你应在你的 Web API 控制器中的单元测试：

- 操作返回响应的正确的类型。
- 无效的参数返回正确的错误响应。
- 操作在存储库或服务层上调用的正确方法。
- 如果响应包含域模型，请验证模型类型。

这些是的一些常规操作，若要测试，但具体情况取决于你的控制器实现。 具体而言，它有很大差别是否控制器操作返回**HttpResponseMessage**或**IHttpActionResult**。 有关这些结果类型的详细信息，请参阅[Web Api 2 中的操作结果](../getting-started-with-aspnet-web-api/action-results.md)。

## <a name="testing-actions-that-return-httpresponsemessage"></a>返回 HttpResponseMessage 的测试操作

下面是控制器的一个示例其操作返回**HttpResponseMessage**。

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample1.cs)]

请注意的控制器使用依赖关系注入注入`IProductRepository`。 这使得控制器更可测试性，因为可以插入模拟的存储库。 下面的单元测试验证`Get`方法写入`Product`向响应正文。 假定`repository`是模型`IProductRepository`。

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample2.cs)]

务必要设置**请求**和**配置**在控制器上。 否则，测试将会失败并**ArgumentNullException**或**InvalidOperationException**。

## <a name="testing-link-generation"></a>测试链接生成

`Post`方法调用**UrlHelper.Link**在响应中创建链接。 这需要为单元测试中的一些更多的设置：

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample3.cs)]

**UrlHelper**类需要请求 URL 和路由数据，因此测试必须为这些设置的值。 另一个选项是模型或存根 （stub） **UrlHelper**。 使用此方法时，将默认值的[ApiController.Url](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) mock 或存根 （stub） 的版本，返回固定的值。

让我们重写测试使用[Moq](https://github.com/Moq) framework。 安装`Moq`测试项目中的 NuGet 包。

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample4.cs)]

在此版本中，你无需设置的任何路由数据，因为 mock **UrlHelper**返回常量字符串。


## <a name="testing-actions-that-return-ihttpactionresult"></a>返回 IHttpActionResult 的测试操作

Web API 2 控制器操作可以返回**IHttpActionResult**，这是类似于**ActionResult** ASP.NET mvc。 **IHttpActionResult**接口定义命令的模式创建 HTTP 响应。 而不是直接创建响应，则控制器将返回**IHttpActionResult**。 更高版本，管道时，将调用**IHttpActionResult**创建响应。 此方法使它成为更轻松地编写单元测试，因为你可以跳过的安装程序所需的很多**HttpResponseMessage**。

下面是示例控制器其操作返回**IHttpActionResult**。

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample5.cs)]

此示例演示使用一些常见模式**IHttpActionResult**。 让我们看看如何与单元测试它们。

### <a name="action-returns-200-ok-with-a-response-body"></a>操作返回 200 （正常） 的响应正文

`Get`方法调用`Ok(product)`如果找到该产品。 在单元测试，请确保返回的类型是**OkNegotiatedContentResult**和退回的产品具有正确的 id。

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample6.cs)]

请注意，单元测试不会执行的操作结果。 你可以假定的操作结果正确创建 HTTP 响应。 (这就是为什么 Web API 框架具有其自己的单元测试 ！)

### <a name="action-returns-404-not-found"></a>操作返回 404 （未找到）

`Get`方法调用`NotFound()`如果找不到产品。 这种情况下，单元测试仅检查，如果返回类型为**NotFoundResult**。

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample7.cs)]

### <a name="action-returns-200-ok-with-no-response-body"></a>操作返回 200 （正常） 的任何响应正文

`Delete`方法调用`Ok()`返回了空的 HTTP 200 响应。 如前面的示例中，单元测试检查返回类型，在这种情况下**OkResult**。

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample8.cs)]

### <a name="action-returns-201-created-with-a-location-header"></a>操作返回 201 （已创建） 使用位置的标头

`Post`方法调用`CreatedAtRoute`以位置标头中返回 HTTP 201 响应，其中一个 URI。 在单元测试中，验证操作将设置正确路由值。

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample9.cs)]

### <a name="action-returns-another-2xx-with-a-response-body"></a>操作返回响应正文的另一个的 2xx

`Put`方法调用`Content`返回响应正文的 HTTP 202 （已接受） 响应。 这种情况下是类似于返回 200 （正常），但单元测试还应检查的状态代码。

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample10.cs)]

## <a name="additional-resources"></a>其他资源

- [模拟实体框架时单元测试 ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)
- [为 ASP.NET Web API 服务编写测试](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx)（通过 Youssef Moussaoui 博客文章）。
- [调试 ASP.NET Web API 与路由调试器](https://blogs.msdn.com/b/webdev/archive/2013/04/04/debugging-asp-net-web-api-with-route-debugger.aspx)
