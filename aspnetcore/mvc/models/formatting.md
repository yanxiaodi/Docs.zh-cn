---
title: "设置 ASP.NET Core MVC 中响应数据的格式"
author: ardalis
description: "了解如何设置 ASP.NET Core MVC 中响应数据的格式。"
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/models/formatting
ms.openlocfilehash: 36231cd2bf59408e9c858ea99355c1e8dd859e6e
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-formatting-response-data-in-aspnet-core-mvc"></a>设置 ASP.NET Core MVC 中响应数据的格式的简介

作者：[Steve Smith](https://ardalis.com/)

ASP.NET Core MVC 包含对通过固定格式或根据客户端规范来设置响应数据格式的内置支持。

[查看或下载 GitHub 中的示例](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/formatting/sample)。

## <a name="format-specific-action-results"></a>特定于格式的操作结果

一些操作结果类型特定于特殊格式，例如 `JsonResult` 和 `ContentResult`。 操作可以返回始终以特定方式进行格式设置的特定结果。 例如，返回 `JsonResult` 将返回 JSON 格式的数据，而不考虑客户端首选项。 同样，返回 `ContentResult` 将返回纯文本格式的字符串数据（仅返回字符串也是如此）。

> [!NOTE]
> 不需要操作返回任何特定类型；MVC 支持任意对象返回值。 如果操作返回 `IActionResult` 实现且控制器继承自 `Controller` ，则开发人员具有对应于多种选择的帮助程序方法。 返回非 `IActionResult` 类型对象的操作结果将使用相应的 `IOutputFormatter` 实现来进行序列化。

若要从继承自 `Controller` 基类的控制器返回特定格式的数据，请使用内置的帮助程序方法 `Json` 来返回 JSON，并使用 `Content` 来返回纯文本。 操作方法应返回特定的结果类型（例如 `JsonResult`）或 `IActionResult`）。

返回 JSON 格式的数据：

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=21-26)]

此操作的示例响应：

![Microsoft Edge 中开发人员工具的“网络”选项卡显示响应的内容类型为 application/json](formatting/_static/json-response.png)

请注意，响应的内容类型为 `application/json`，网络请求的列表和“响应标头”部分均有显示。 另请注意“响应标头”部分 Accept 标头的浏览器（此示例中为 Microsoft Edge）所呈现的选项列表。 当前技术忽略此标头；下面将讨论遵守它的情况。

若要返回纯文本格式数据，请使用 `ContentResult` 和 `Content` 帮助程序：

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=47-52)]

此操作的响应：

![Microsoft Edge 中开发人员工具的“网络”选项卡显示响应的内容类型为 text/plain](formatting/_static/text-response.png)

请注意此示例中返回的 `Content-Type` 为 `text/plain`。 还可以仅使用字符串响应类型来达到此相同行为：

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=54-59)]

>[!TIP]
> 对于有多个返回类型或选项的重要操作（例如基于所执行操作的结果的不同 HTTP 状态代码），请首选 `IActionResult` 作为返回类型。

## <a name="content-negotiation"></a>内容协商

当客户端指定 [Accept 标头](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html)时，会发生内容协商（简称 conneg）。 ASP.NET Core MVC 使用的默认格式是 JSON。 内容协商由 `ObjectResult` 实现。 它还内置于从帮助程序方法（全部基于 `ObjectResult`）返回的特定于状态代码的操作结果中。 还可以返回一个模型类型（已定义为数据传输类型的类），框架将自动将其打包在 `ObjectResult` 中。

以下操作方法使用 `Ok` 和 `NotFound` 帮助程序方法：

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=8,10&range=28-38)]

将返回 JSON 格式的响应，除非请求了另一个格式且服务器可以返回所请求格式。 可以使用 [Fiddler](http://www.telerik.com/fiddler) 等工具创建包括 Accept 标头的请求并指定另一种格式。 在此情况下，如果服务器有可以生成所请求格式的响应的格式化程序，则结果会以服务器首选的格式返回。

![Fiddler 控制台显示 Accept 标头为 application/xml 的手动创建的 GET 请求](formatting/_static/fiddler-composer.png)

在上面的屏幕快照中，使用 Fiddler Composer 生成请求，指定 `Accept: application/xml`。 默认情况下，ASP.NET Core MVC 仅支持 JSON。所以，即使指定另一种格式，返回的结果仍然是 JSON 格式。 将在下一节中看到如何添加其他格式化程序。

控制器操作可以返回 POCO（普通旧 CLR 对象），在这种情况下，ASP.NET MVC 将自动创建打包对象的 `ObjectResult`。 客户端将获取设有格式的序列化对象（默认为 JSON 格式，可以配置 XML 或其他格式）。 如果返回的对象为 `null`，那么框架将返回 `204 No Content` 响应。

返回对象类型：

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3&range=40-45)]

在示例中，请求有效作者别名将收到具有作者数据的“200 正常”响应。 请求无效别名将收到“204 无内容”响应。 下面列出了以 XML 和 JSON 格式显示响应的屏幕快照。

### <a name="content-negotiation-process"></a>内容协商过程

内容协商仅在 `Accept` 标头出现在请求中时发生。 请求包含 accept 标头时，框架会以最佳顺序枚举 accept 标头中的媒体类型，并且尝试查找可以生成一种由 accept 标头指定格式的响应的格式化程序。 如果未找到可以满足客户端请求的格式化程序，框架将尝试找到第一个可以生成响应的格式化程序（除非开发人员配置 `MvcOptions` 上的选项以返回“406 不可接受”）。 如果请求指定 XML，但是未配置 XML 格式化程序，那么将使用 JSON 格式化程序。 一般来说，如果没有配置可以提供所请求格式的格式化程序，那么使用第一个可以设置对象格式的格式化程序。 如果不提供任何标头，则将使用第一个可以处理要返回的对象的格式化程序来序列化响应。 在此情况下，没有任何协商发生 - 服务器确定将使用的格式。

> [!NOTE]
> 如果 Accept 标头包含 `*/*`，则将忽略该标头，除非 `RespectBrowserAcceptHeader` 在 `MvcOptions` 上设置为 true。

### <a name="browsers-and-content-negotiation"></a>浏览器和内容协商

不同于传统 API 客户端，Web 浏览器倾向于提供包括各种格式（含通配符）的 `Accept` 标头。 默认情况下，当框架检测到请求来自浏览器时，它将忽略 `Accept` 标头转而以应用程序的配置默认格式（JSON，除非有其他配置）返回内容。 这在使用不同浏览器使用 API 时提供更一致的体验。

如果首选应用程序服从浏览器 accept 标头，则可以将此配置为 MVC 配置的一部分，方法是在 Startup.cs 中以 `ConfigureServices` 方法将 `RespectBrowserAcceptHeader` 设置为 `true`。

```csharp
services.AddMvc(options =>
{
    options.RespectBrowserAcceptHeader = true; // false by default
});
```

## <a name="configuring-formatters"></a>配置格式化程序

如果应用程序需要支持默认 JSON 格式以外的其他格式，那么可以添加 NuGet 包并配置 MVC 来支持它们。 输入和输出的格式化程序不同。 输入格式化程序由[模型绑定](model-binding.md)使用；输出格式化程序用来设置响应格式。 还可以配置[自定义格式化程序](../advanced/custom-formatters.md)。

### <a name="adding-xml-format-support"></a>添加 XML 格式支持

若要添加对 XML 格式的支持，请安装 `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet 包。

将 XmlSerializerFormatters 添加到 Startup.cs 中 MVC 的配置：

[!code-csharp[Main](./formatting/sample/Startup.cs?name=snippet1&highlight=2)]

或者，可以仅添加输出格式化程序：

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlSerializerOutputFormatter());
});
```

这两个方法将使用 `System.Xml.Serialization.XmlSerializer` 来序列化结果。 如果愿意，可以通过添加相关联的格式化程序使用 `System.Runtime.Serialization.DataContractSerializer`：

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlDataContractSerializerOutputFormatter());
});
```

一旦添加了对 XML 格式的支持，控制器方法应基于请求的 `Accept` 标头返回相应的格式，如以下 Fiddler 示例所示：

![Fiddler 控制台：请求的 Raw 选项卡显示 Accept 标头值为 application/xml。 响应的 Raw 选项卡显示 Content-Type 标头值为 application/xml。](formatting/_static/xml-response.png)

可以在“检查器”选项卡中看到使用 `Accept: application/xml` 标头集做出 Raw GET 请求。 响应窗格显示 `Content-Type: application/xml` 标头，且 `Author` 对象已序列化为 XML。

使用“编辑器”选项卡修改请求以指定 `Accept` 标头中的 `application/json`。 执行请求，响应格式将设为 JSON：

![Fiddler 控制台：请求的 Raw 选项卡显示 Accept 标头值为 application/json。 响应的 Raw 选项卡显示 Content-Type 标头值为 pplication/json。](formatting/_static/json-response-fiddler.png)

在此屏幕快照中，可以看到请求了设置 `Accept: application/json` 的标头，且响应也将它指定为其 `Content-Type`。 `Author` 对象以 JSON 格式显示在响应正文中。

### <a name="forcing-a-particular-format"></a>强制执行特定格式

如果需要限制特定操作的响应格式，那么可以应用 `[Produces]` 筛选器。 `[Produces]` 筛选器指定特定操作（或控制器）的响应格式。 如同大多[筛选器](../controllers/filters.md)，这可以在操作层面、控制器层面或全局范围内应用。

```csharp
[Produces("application/json")]
public class AuthorsController
```

`[Produces]` 筛选器将强制 `AuthorsController` 中的所有操作返回 JSON 格式的响应，即使已经为应用程序配置其他格式化程序且客户端提供了请求其他可用格式的 `Accept` 标头也是如此。 请参阅[筛选器](../controllers/filters.md)了解详细信息，包括如何全局应用筛选器。

### <a name="special-case-formatters"></a>特例格式化程序

一些特例是使用内置格式化程序实现的。 默认情况下，`string` 返回类型的格式将设为 text/plain（如果通过 `Accept` 标头请求则为 text/html）。 可以通过删除 `TextOutputFormatter` 删除此行为。 在 Startup.cs 中通过 `Configure` 方法删除格式化程序（如下所示）。 有模型对象返回类型的操作将在返回 `null` 时返回“204 无内容”响应。 可以通过删除 `HttpNoContentOutputFormatter` 删除此行为。 以下代码删除 `TextOutputFormatter` 和 `HttpNoContentOutputFormatter`。

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.RemoveType<TextOutputFormatter>();
    options.OutputFormatters.RemoveType<HttpNoContentOutputFormatter>();
});
```

没有 `TextOutputFormatter` 时，`string` 返回类型将返回“406 不可接受”等内容。 请注意，如果 XML 格式化程序存在，当删除 `TextOutputFormatter` 时，它将设置 `string` 返回类型的格式。

没有 `HttpNoContentOutputFormatter`，null 对象将使用配置的格式化程序来进行格式设置。 例如，JSON 格式化程序仅返回正文为 `null` 的响应，而 XML 格式化程序将返回属性设为 `xsi:nil="true"` 的空 XML 元素。

## <a name="response-format-url-mappings"></a>响应格式 URL 映射

客户端可要求特定格式作为 URL 一部分，例如在查询字符串中或在路径的一部分中，或者通过使用特定格式的文件扩展名（例如 .xml 或 .json）。 请求路径的映射必须在 API 使用的路由中指定。 例如:

```csharp
[FormatFilter]
public class ProductsController
{
    [Route("[controller]/[action]/{id}.{format?}")]
    public Product GetById(int id)
```

此路由将允许所所请求格式指定为可选文件扩展名。 `[FormatFilter]` 属性检查 `RouteData` 中格式值是否存在，并在响应创建时将响应格式映射到相应格式化程序。

| 路由                      | 格式化程序                          |
| -------------------------- | ---------------------------------- |
| `/products/GetById/5`      | 默认输出格式化程序       |
| `/products/GetById/5.json` | JSON 格式化程序（如配置） |
| `/products/GetById/5.xml`  | XML 格式化程序（如配置）  |
