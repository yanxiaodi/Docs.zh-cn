---
title: "ASP.NET Core 中 Razor 页面的路由和应用约定功能"
author: guardrex
description: "了解路由和应用模型提供程序约定功能如何帮助控制页面路由、发现和处理。"
manager: wpickett
ms.author: riande
ms.date: 10/23/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/razor-pages/razor-pages-convention-features
ms.openlocfilehash: bf1c895fc972310d5541d0098226d58b8183e320
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="razor-pages-route-and-app-convention-features-in-aspnet-core"></a>ASP.NET Core 中 Razor 页面的路由和应用约定功能

作者：[Luke Latham](https://github.com/guardrex)

了解如何使用 Razor 页面应用中的页面路由和应用模型提供程序约定功能，来控制页面路由、发现和处理。 需要为各个页面配置自定义页面路由时，可使用本主题稍后所述的 [AddPageRoute 约定](#configure-a-page-route)配置页面路由。

通过[示例应用](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/razor-pages-convention-features/sample)（[如何下载](xref:tutorials/index#how-to-download-a-sample)）了解本主题所述的功能。

| 功能 | 示例演示... |
| -------- | --------------------------- |
| [路由和应用模型约定](#add-route-and-app-model-conventions)<br><br>Conventions.Add<ul><li>IPageRouteModelConvention</li><li>IPageApplicationModelConvention</li></ul> | 将路由模板和标头添加到应用的页面中。 |
| [页面路由操作约定](#page-route-action-conventions)<ul><li>AddFolderRouteModelConvention</li><li>AddPageRouteModelConvention</li><li>AddPageRoute</li></ul> | 将路由模板添加到某个文件夹中的页面以及单个页面。 |
| [页面模型操作约定](#page-model-action-conventions)<ul><li>AddFolderApplicationModelConvention</li><li>AddPageApplicationModelConvention</li><li>ConfigureFilter（筛选器类、Lambda 表达式或筛选器工厂）</li></ul> | 将标头添加到某个文件夹中的页面，将标头添加到单个页面，以及配置[筛选器工厂](xref:mvc/controllers/filters#ifilterfactory)以将标头添加到应用的页面。 |
| [默认页面应用模型提供程序](#replace-the-default-page-app-model-provider) | 取代默认页面模型提供程序，用于更改处理程序命名约定。 |

## <a name="add-route-and-app-model-conventions"></a>添加路由和应用模型约定

为 [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) 添加委托，以添加应用于 Razor 页面的路由和应用模型约定。

**将路由模型约定添加到所有页面**

使用[约定](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions)创建 [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) 并将其添加到 [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) 实例集合中，这些实例将在路由和页面模型构造过程中应用。

示例应用将 `{globalTemplate?}` 路由模板添加到应用中的所有页面：

[!code-csharp[Main](razor-pages-convention-features/sample/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

> [!NOTE]
> `AttributeRouteModel` 的 `Order` 属性设置为 `0`（零）。 这样可确保，当提供单个路由值时，优先将此模板作为第一个路由数据值位置。 例如，示例在本主题的后面部分添加了一个 `{aboutTemplate?}` 路由模板。 为 `{aboutTemplate?}` 模板指定的 `Order` 为 `1`。 当在 `/About/RouteDataValue` 中请求“关于”页面时，由于设置了 `Order` 属性，“RouteDataValue”会加载到 `RouteData.Values["globalTemplate"]` (`Order = 0`) 而不是 `RouteData.Values["aboutTemplate"]` (`Order = 1`) 中。

*Startup.cs*：

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet1)]

在 `localhost:5000/About/GlobalRouteValue` 中请求示例的“关于”页面并检查结果：

![使用 GlobalRouteValue 路由段请求“关于”页面。 呈现的页面显示，在页面的 OnGet 方法中捕获了路由数据值。](razor-pages-convention-features/_static/about-page-global-template.png)

**将应用模型约定添加到所有页面**

使用[约定](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions)创建 [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) 并将其添加到 [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) 实例集合中，这些实例将在路由和页面模型构造过程中应用。

为了演示此约定以及本主题后面的其他约定，示例应用包含了一个 `AddHeaderAttribute` 类。 类构造函数采用 `name` 字符串和 `values` 字符串数组。 将在其 `OnResultExecuting` 方法中使用这些值来设置响应标头。 本主题后面的[页面模型操作约定](#page-model-action-conventions)部分展示了完整的类。

示例应用使用 `AddHeaderAttribute` 类将标头 `GlobalHeader` 添加到应用中的所有页面：

[!code-csharp[Main](razor-pages-convention-features/sample/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

*Startup.cs*：

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet2)]

在 `localhost:5000/About` 中请求示例的“关于”页面，并检查标头以查看结果：

![“关于”页面的响应标头显示已添加 GlobalHeader。](razor-pages-convention-features/_static/about-page-global-header.png)

## <a name="page-route-action-conventions"></a>页面路由操作约定

默认路由模型提供程序派生自 [IPageRouteModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelprovider)，可调用旨在为页面路由配置提供扩展点的约定。

**文件夹路由模型约定**

使用 [AddFolderRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderroutemodelconvention) 创建并添加 [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention)，后者可以为指定文件夹下的所有页面调用 [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) 上的操作。

示例应用使用 `AddFolderRouteModelConvention` 将 `{otherPagesTemplate?}` 路由模板添加到 *OtherPages* 文件夹中的页面：

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet3)]

> [!NOTE]
> `AttributeRouteModel` 的 `Order` 属性设置为 `1`。 这样可确保，当提供单个路由值时，优先将 `{globalTemplate?}` 的模板（已在本主题的前面部分设置）作为第一个路由数据值位置。 如果在 `/OtherPages/Page1/RouteDataValue` 中请求 Page1 页面，由于设置了 `Order` 属性，“RouteDataValue”会加载到 `RouteData.Values["globalTemplate"]` (`Order = 0`) 而不是 `RouteData.Values["otherPagesTemplate"]` (`Order = 1`) 中。

在 `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` 中请求示例的 Page1 页面并检查结果：

![使用 GlobalRouteValue 和 OtherPagesRouteValue 路由段请求 OtherPages 文件夹中的 Page1。 呈现的页面显示，在页面的 OnGet 方法中捕获了路由数据值。](razor-pages-convention-features/_static/otherpages-page1-global-and-otherpages-templates.png)

**页面路由模型约定**

使用 [AddPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageroutemodelconvention) 创建并添加 [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention)，后者可以为具有指定名称的页面调用 [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) 上的操作。

示例应用使用 `AddPageRouteModelConvention` 将 `{aboutTemplate?}` 路由模板添加到“关于”页面：

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet4)]

> [!NOTE]
> `AttributeRouteModel` 的 `Order` 属性设置为 `1`。 这样可确保，当提供单个路由值时，优先将 `{globalTemplate?}` 的模板（已在本主题的前面部分设置）作为第一个路由数据值位置。 如果在 `/About/RouteDataValue` 中请求“关于”页面，由于设置了 `Order` 属性，“RouteDataValue”会加载到 `RouteData.Values["globalTemplate"]` (`Order = 0`) 而不是 `RouteData.Values["aboutTemplate"]` (`Order = 1`) 中。

在 `localhost:5000/About/GlobalRouteValue/AboutRouteValue` 中请求示例的“关于”页面并检查结果：

![使用 GlobalRouteValue 和 AboutRouteValue 路由段请求“关于”页面。 呈现的页面显示，在页面的 OnGet 方法中捕获了路由数据值。](razor-pages-convention-features/_static/about-page-global-and-about-templates.png)

## <a name="configure-a-page-route"></a>配置页面路由

使用 [AddPageRoute](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.addpageroute) 配置路由，该路由指向指定页面路径中的页面。 生成的页面链接使用指定的路由。 `AddPageRoute` 使用 `AddPageRouteModelConvention` 建立路由。

示例应用为 *Contact.cshtml* 创建指向 `/TheContactPage` 的路由：

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet5)]

还可在 `/Contact` 中通过默认路由访问“联系人”页面。

示例应用的“联系人”页面自定义路由允许使用可选的 `text` 路由段 (`{text?}`)。 该页面还在其 `@page` 指令中包含此可选段，以便访问者在 `/Contact` 路由中访问该页面：

[!code-cshtml[Main](razor-pages-convention-features/sample/Pages/Contact.cshtml?highlight=1)]

请注意，在呈现的页面中，为**联系人**链接生成的 URL 反映了已更新的路由：

![导航栏中的示例应用“联系人”链接](razor-pages-convention-features/_static/contact-link.png)

![检查呈现的 HTML 中的“联系人”链接，可看到 href 设置为“/TheContactPage”](razor-pages-convention-features/_static/contact-link-source.png)

在常规路由 `/Contact` 或自定义路由 `/TheContactPage` 中访问“联系人”页面。 如果提供附加的 `text` 路由段，该页面会显示所提供的 HTML 编码段：

![在 URL 中提供可选“'text”路由段“TextValue”的 Microsoft Edge 浏览器示例。 呈现的页面显示“text”段值。](razor-pages-convention-features/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a>页面模型操作约定

实现 [IPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelprovider) 的默认页面模型提供程序可调用约定，这些约定旨在为页面模型配置提供扩展点。 在生成和修改页面发现及处理功能时，可使用这些约定。

对于此部分中的示例，示例应用使用 `AddHeaderAttribute` 类（一个 [ResultFilterAttribute](/dotnet/api/microsoft.aspnetcore.mvc.filters.resultfilterattribute)）来应用响应标头：

[!code-csharp[Main](razor-pages-convention-features/sample/Filters/AddHeader.cs?name=snippet1)]

示例演示了如何使用约定将该属性应用于某个文件夹中的所有页面以及单个页面。

**文件夹应用模型约定**

使用 [AddFolderApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderapplicationmodelconvention) 创建并添加 [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention)，后者可以为指定文件夹下的所有页面调用 [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) 实例上的操作。

示例演示了如何使用 `AddFolderApplicationModelConvention` 将标头 `OtherPagesHeader` 添加到应用的 *OtherPages* 文件夹内的页面：

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet6)]

在 `localhost:5000/OtherPages/Page1` 中请求示例的 Page1 页面，并检查标头以查看结果：

![OtherPages/Page1 页面的响应标头显示已添加 OtherPagesHeader。](razor-pages-convention-features/_static/page1-otherpages-header.png)

**页面应用模型约定**

使用 [AddPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageapplicationmodelconvention) 创建并添加 [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention)，后者可以为具有指定名称的页面调用 [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) 上的操作。

示例演示了如何使用 `AddPageApplicationModelConvention` 将标头 `AboutHeader` 添加到“关于”页面：

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet7)]

在 `localhost:5000/About` 中请求示例的“关于”页面，并检查标头以查看结果：

![“关于”页面的响应标头显示已添加 AboutHeader。](razor-pages-convention-features/_static/about-page-about-header.png)

**配置筛选器**

[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter) 可配置要应用的指定筛选器。 用户可以实现筛选器类，但示例应用演示了如何在 Lambda 表达式中实现筛选器，该筛选器在后台作为可返回筛选器的工厂实现：

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet8)]

页面应用模型用于检查指向 *OtherPages* 文件夹中 Page2 页面的段的相对路径。 如果条件通过，则添加标头。 如果不通过，则应用 `EmptyFilter`。

`EmptyFilter` 是一种[操作筛选器](xref:mvc/controllers/filters#action-filters)。 由于 Razor 页面会忽略操作筛选器，因此，如果路径不包含 `OtherPages/Page2`，`EmptyFilter` 会按预期发出空操作指令。

在 `localhost:5000/OtherPages/Page2` 中请求示例的 Page2 页面，并检查标头以查看结果：

![OtherPagesPage2Header 已添加到 Page2 的响应。](razor-pages-convention-features/_static/page2-filter-header.png)

**配置筛选器工厂**

[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_ConfigureFilter_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_Func_Microsoft_AspNetCore_Mvc_ApplicationModels_PageApplicationModel_Microsoft_AspNetCore_Mvc_Filters_IFilterMetadata__) 可配置指定的工厂，以将[筛选器](xref:mvc/controllers/filters)应用于所有 Razor 页面。

示例应用提供了一个示例，说明如何使用[筛选器工厂](xref:mvc/controllers/filters#ifilterfactory)将具有两个值的标头 `FilterFactoryHeader` 添加到应用的页面：

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet9)]

*AddHeaderWithFactory.cs*：

[!code-csharp[Main](razor-pages-convention-features/sample/Factories/AddHeaderWithFactory.cs?name=snippet1)]

在 `localhost:5000/About` 中请求示例的“关于”页面，并检查标头以查看结果：

![“关于”页面的响应标头显示已添加两个 FilterFactoryHeader 标头。](razor-pages-convention-features/_static/about-page-filter-factory-header.png)

## <a name="replace-the-default-page-app-model-provider"></a>替换默认页面应用模型提供程序

Razor 页面使用 `IPageApplicationModelProvider` 接口创建 [DefaultPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider)。 用户可以从默认模型提供程序继承，以便为处理程序发现和处理提供自己的实现逻辑。 默认实现（[引用源](https://github.com/aspnet/Mvc/blob/rel/2.0.1/src/Microsoft.AspNetCore.Mvc.RazorPages/Internal/DefaultPageApplicationModelProvider.cs)）为*未命名*和*已命名*处理程序的命名建立了约定，如下所述。

**默认的未命名处理程序方法**

Http 谓词的处理程序方法（“未命名”的处理程序方法）遵循以下约定：`On<HTTP verb>[Async]`（追加 `Async` 是可选操作，但建议为异步方法执行此操作）。

| 未命名处理程序方法     | 操作                      |
| -------------------------- | ------------------------------ |
| `OnGet`/`OnGetAsync`       | 初始化页面状态。     |
| `OnPost`/`OnPostAsync`     | 处理 POST 请求。          |
| `OnDelete`/`OnDeleteAsync` | 处理 DELETE 请求&#8224;。 |
| `OnPut`/`OnPutAsync`       | 处理 PUT 请求&#8224;。    |
| `OnPatch`/`OnPatchAsync`   | 处理 PATCH 请求&#8224;。  |

&#8224;用于向页面发出 API 调用。

**默认的已命名处理程序方法**

由开发人员提供的处理程序方法（“已命名”的处理程序方法）遵循类似的约定。 处理程序名称出现在 Http 谓词之后或者 Http 谓词与 `Async` 之间：`On<HTTP verb><handler name>[Async]`（追加 `Async` 是可选操作，但建议为异步方法执行此操作）。 例如，用于处理消息的方法可能采用下表所示的命名。

| 已命名处理程序方法示例             | 操作示例        |
| ---------------------------------------- | ------------------------ |
| `OnGetMessage`/`OnGetMessageAsync`       | 获取消息。        |
| `OnPostMessage`/`OnPostMessageAsync`     | POST 消息。          |
| `OnDeleteMessage`/`OnDeleteMessageAsync` | DELETE 消息&#8224;。 |
| `OnPutMessage`/`OnPutMessageAsync`       | PUT 消息&#8224;。    |
| `OnPatchMessage`/`OnPatchMessageAsync`   | PATCH 消息&#8224;。  |

&#8224;用于向页面发出 API 调用。

**自定义处理程序方法名称**

假设用户想更改未命名和已命名处理程序方法的命名方式。 备选命名方案是避免让方法名称以“On”开头，并使用第一个分词来确定 Http 谓词。 也可以进行其他更改，比如将 DELETE、PUT 和 PATCH 的谓词转换为 POST。 这种方案提供下表所示的方法名称。

| 处理程序方法                       | 操作                      |
| ------------------------------------ | ------------------------------ |
| `Get`                                | 初始化页面状态。     |
| `Post`/`PostAsync`                   | 处理 POST 请求。          |
| `Delete`/`DeleteAsync`               | 处理 DELETE 请求&#8224;。 |
| `Put`/`PutAsync`                     | 处理 PUT 请求&#8224;。    |
| `Patch`/`PatchAsync`                 | 处理 PATCH 请求&#8224;。  |
| `GetMessage`                         | 获取消息。              |
| `PostMessage`/`PostMessageAsync`     | POST 消息。                |
| `DeleteMessage`/`DeleteMessageAsync` | POST 消息以进行删除。      |
| `PutMessage`/`PutMessageAsync`       | POST 消息以进行放置。         |
| `PatchMessage`/`PatchMessageAsync`   | POST 消息以进行修补。       |

&#8224;用于向页面发出 API 调用。

若要建立此方案，请从 `DefaultPageApplicationModelProvider` 类继承并重写 [CreateHandlerModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider.createhandlermodel) 方法，以提供自定义逻辑来解析 [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) 处理程序名称。 示例应用展示了如何在其 `CustomPageApplicationModelProvider` 类中执行此操作：

[!code-csharp[Main](razor-pages-convention-features/sample/CustomPageApplicationModelProvider.cs?name=snippet1&highlight=1-2,45-46,64-68,78-85,87,92,106)]

该类的要点包括：

* 该类继承自 `DefaultPageApplicationModelProvider`。
* `TryParseHandlerMethod` 对处理程序进行处理，以便在创建 `PageHandlerModel` 时确定 Http 谓词 (`httpMethod`) 和已命名处理程序名称 (`handlerName`)。
  * 忽略 `Async` 后缀（如果存在）。
  * 使用大小写分析方法名称中的 Http 谓词。
  * 当方法名称（不带 `Async`）与 Http 谓词名称相同时，就不存在已命名处理程序。 `handlerName` 设置为 `null`，且方法名称为 `Get`、`Post`、`Delete`、`Put` 或 `Patch`。
  * 当方法名称（不带 `Async`）的长度超过 Http 谓词名称时，就存在已命名处理程序。 `handlerName` 设置为 `<method name (less 'Async', if present)>`。 例如，“GetMessage”和“GetMessageAsync”都生成处理程序名称“GetMessage”。
  * DELETE、PUT 和 PATCH Http 谓词都转换为 POST。

在 `Startup` 类中注册 `CustomPageApplicationModelProvider`：

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet10)]

*Index.cshtml.cs* 中的页面模型演示了如何针对应用中的页面更改常规的处理程序方法命名约定。 用于 Razor 页面的常规“On”前缀命名已被删除。 用于初始化页面状态的方法现在命名为 `Get`。 为任何页面打开任何页面模型时，都可以看到此约定贯穿于整个应用。

其他各个方法均以描述其处理的 Http 谓词开头。 以 `Delete` 开头的两个方法通常被视为 DELETE Http 谓词，但 `TryParseHandlerMethod` 中的逻辑会针对这两种处理程序将该谓词显式设置为 POST。

请注意，`Async` 在 `DeleteAllMessages` 和 `DeleteMessageAsync` 之间是可选的。 它们都是异步方法，但用户可以选择是否使用 `Async` 后缀；我们建议使用。 此处使用 `DeleteAllMessages` 只是为了进行演示，但建议将此类方法命名为 `DeleteAllMessagesAsync`。 它对示例的实现处理没有影响，但使用 `Async` 后缀强调了它是异步方法的事实。

[!code-csharp[Main](razor-pages-convention-features/sample/Pages/Index.cshtml.cs?name=snippet1&highlight=1,6,16,29)]

请注意，*Index.cshtml* 中提供的处理程序名称与 `DeleteAllMessages` 和 `DeleteMessageAsync` 处理程序方法相匹配：

[!code-cshtml[Main](razor-pages-convention-features/sample/Pages/Index.cshtml?range=29-60&highlight=7-8,24-25)]

为了方便处理程序将 POST 请求与方法进行匹配，`TryParseHandlerMethod` 剔除了处理程序方法名称 `DeleteMessageAsync` 中的 `Async`。 `DeleteMessage` 的 `asp-page-handler` 名称与处理程序方法 `DeleteMessageAsync` 匹配。

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a>MVC 筛选器和页面筛选器 (IPageFilter)

Razor 页面会忽略 MVC [操作筛选器](xref:mvc/controllers/filters#action-filters)，因为 Razor 页面使用处理程序方法。 可使用其他类型的 MVC 筛选器：[授权](xref:mvc/controllers/filters#authorization-filters)、[异常](xref:mvc/controllers/filters#exception-filters)、[资源](xref:mvc/controllers/filters#resource-filters)和[结果](xref:mvc/controllers/filters#result-filters)。 有关详细信息，请参阅[筛选器](xref:mvc/controllers/filters)主题。

页面筛选器 ([IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter)) 是应用于 Razor 页面的一种筛选器。 它涵盖页面处理程序方法的执行。 它允许在页面处理程序方法执行阶段处理自定义代码。 下面是来自示例应用的一个示例：

[!code-csharp[Main](razor-pages-convention-features/sample/Filters/ReplaceRouteValueFilterAttribute.cs?name=snippet1)]

此筛选器检查 `globalTemplate` 路由值“TriggerValue”并换入“ReplacementValue”。

`ReplaceRouteValueFilter` 属性可直接应用于 `PageModel`：

[!code-csharp[Main](razor-pages-convention-features/sample/Pages/OtherPages/Page3.cshtml.cs?range=10-12&highlight=1)]

在 `localhost:5000/OtherPages/Page3/TriggerValue` 处从示例应用中请求 Page3 页面。 注意筛选器是如何替换路由值的：

![使用 TriggerValue 路由段请求 OtherPages/Page3 导致筛选器将路由值替换为 ReplacementValue。](razor-pages-convention-features/_static/otherpages-page3-filter-replacement-value.png)

## <a name="see-also"></a>请参阅

* [Razor 页面授权约定](xref:security/authorization/razor-pages-authorization)
