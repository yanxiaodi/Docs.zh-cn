---
uid: web-api/overview/advanced/configuring-aspnet-web-api
title: "配置 ASP.NET Web API 2 |Microsoft 文档"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2014
ms.topic: article
ms.assetid: 9e10a700-8d91-4d2e-a31e-b8b569fe867c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/configuring-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: f9b471fe2afdce278869a2e4d9b693a78030324b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="configuring-aspnet-web-api-2"></a>配置 ASP.NET Web API 2
====================
通过[Mike Wasson](https://github.com/MikeWasson)

本主题介绍如何配置 ASP.NET Web API。

- [配置设置](#settings)
- [使用 ASP.NET 宿主配置 Web API](#webhost)
- [配置带 OWIN 自承载的 Web API](#selfhost)
- [全局 Web API 服务](#services)
- [每个控制器配置](#percontrollerconfig)

<a id="settings"></a>
## <a name="configuration-settings"></a>配置设置

在中定义的配置设置，web API [HttpConfiguration](https://msdn.microsoft.com/library/system.web.http.httpconfiguration.aspx)类。

| 成员 | 描述 |
| --- | --- |
| **DependencyResolver** | 使控制器的依赖关系注入。 请参阅[使用 Web API 依赖项解析程序](dependency-injection.md)。 |
| **筛选器** | 操作筛选器。 |
| **格式化程序** | [媒体类型格式化程序](../formats-and-model-binding/media-formatters.md)。 |
| **IncludeErrorDetailPolicy** | 指定服务器是否应在 HTTP 响应消息中包含错误详细信息，例如异常消息和堆栈跟踪。 请参阅[IncludeErrorDetailPolicy](https://msdn.microsoft.com/library/system.web.http.includeerrordetailpolicy(v=vs.108))。 |
| **Initializer** | 执行最终的初始化函数**HttpConfiguration**。 |
| **MessageHandlers** | [HTTP 消息处理程序](http-message-handlers.md)。 |
| **ParameterBindingRules** | 用于在控制器操作绑定参数的规则的集合。 |
| **属性** | 一个泛型属性包。 |
| **路由** | 路由的集合。 请参阅[ASP.NET Web API 中的路由](../web-api-routing-and-actions/routing-in-aspnet-web-api.md)。 |
| **服务** | 服务集合。 请参阅[服务](#services)。 |


## <a name="prerequisites"></a>系统必备

[Visual Studio 2017](https://www.visualstudio.com/vs/) Community、 Professional 或 Enterprise Edition。

<a id="webhost"></a>
## <a name="configuring-web-api-with-aspnet-hosting"></a>使用 ASP.NET 宿主配置 Web API

在 ASP.NET 应用程序，通过调用来配置 Web API [GlobalConfiguration.Configure](https://msdn.microsoft.com/library/system.web.http.globalconfiguration.configure.aspx)中**应用程序\_启动**方法。 **配置**方法采用单个参数的类型的委托**HttpConfiguration**。 执行所有委托内你配置。

下面是使用匿名委托的示例：

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample1.cs)]

在 Visual Studio 2017，"ASP.NET Web 应用程序"项目模板会自动设置的配置代码，如果在中选择"Web API"**新建 ASP.NET 项目**对话框。

[![](configuring-aspnet-web-api/_static/image2.png)](configuring-aspnet-web-api/_static/image1.png)

项目模板创建一个名为应用程序在 WebApiConfig.cs 文件\_开始文件夹。 此代码文件定义应放置 Web API 配置代码的委托。

![](configuring-aspnet-web-api/_static/image3.png)

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample2.cs?highlight=12)]

项目模板还添加调用从委托的代码，**应用程序\_启动**。

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample3.cs?highlight=5)]

<a id="selfhost"></a>
## <a name="configuring-web-api-with-owin-self-hosting"></a>配置带 OWIN 自承载的 Web API

如果你是带 OWIN 自承载，创建一个新**HttpConfiguration**实例。 在此实例上执行任何配置，然后将传递到实例**Owin.UseWebApi**扩展方法。

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample4.cs)]

本教程[使用 OWIN 自承载 ASP.NET Web API 2 来](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)显示的完整步骤。

<a id="services"></a>
## <a name="global-web-api-services"></a>全局 Web API 服务

**HttpConfiguration.Services**集合包含一组 Web API 用于执行各种任务，如控制器所选内容和内容协商的全局服务。

> [!NOTE]
> **服务**集合不是用于服务发现或依赖关系注入的通用机制。 它只存储到 Web API 框架已知的服务类型。


**服务**使用一组默认的服务，初始化集合，你可以提供你自己的自定义实现。 某些服务支持多个实例，而其他人可以只有一个实例。 (但是，还可以在控制器级别的服务提供; 请参阅[-控制器配置](#percontrollerconfig)。

单实例服务


| 服务 | 描述 |
| --- | --- |
| **IActionValueBinder** | 获取参数绑定。 |
| **IApiExplorer** | 获取应用程序公开的 api 的说明。 请参阅[创建一个 Web API 的帮助页](../getting-started-with-aspnet-web-api/creating-api-help-pages.md)。 |
| **IAssembliesResolver** | 获取应用程序的程序集的列表。 请参阅[路由和操作选择](../web-api-routing-and-actions/routing-and-action-selection.md)。 |
| **IBodyModelValidator** | 验证从请求正文中的媒体类型格式化程序读取的模型。 |
| **IContentNegotiator** | 执行内容协商。 |
| **IDocumentationProvider** | 提供有关 Api 的文档。 默认值是**null**。 请参阅[创建一个 Web API 的帮助页](../getting-started-with-aspnet-web-api/creating-api-help-pages.md)。 |
| **IHostBufferPolicySelector** | 指示主机是否应缓冲 HTTP 消息实体正文。 |
| **IHttpActionInvoker** | 调用的控制器操作。 请参阅[路由和操作选择](../web-api-routing-and-actions/routing-and-action-selection.md)。 |
| **IHttpActionSelector** | 选择的控制器操作。 请参阅[路由和操作选择](../web-api-routing-and-actions/routing-and-action-selection.md)。 |
| **IHttpControllerActivator** | 激活一个控制器。 请参阅[路由和操作选择](../web-api-routing-and-actions/routing-and-action-selection.md)。 |
| **IHttpControllerSelector** | 选择一个控制器。 请参阅[路由和操作选择](../web-api-routing-and-actions/routing-and-action-selection.md)。 |
| **IHttpControllerTypeResolver** | 提供应用程序中的 Web API 控制器类型的列表。 请参阅[路由和操作选择](../web-api-routing-and-actions/routing-and-action-selection.md)。 |
| **ITraceManager** | 初始化跟踪 framework。 请参阅[在 ASP.NET Web API 中进行跟踪](../testing-and-debugging/tracing-in-aspnet-web-api.md)。 |
| **ITraceWriter** | 提供的跟踪编写器。 默认值为"无操作"跟踪编写器。 请参阅[在 ASP.NET Web API 中进行跟踪](../testing-and-debugging/tracing-in-aspnet-web-api.md)。 |
| **IModelValidatorCache** | 提供的模型验证程序的缓存。 |

多实例服务


| 服务 | 描述 |
| --- | --- |
| **IFilterProvider** | 返回的控制器操作的筛选器的列表。 |
| **ModelBinderProvider** | 返回给定类型的模型联编程序。 |
| **ModelMetadataProvider** | 模型可提供元数据。 |
| **ModelValidatorProvider** | 为模型提供一个验证程序。 |
| **ValueProviderFactory** | 创建值提供程序。 有关详细信息，请参阅 Mike 停止的博客文章[如何在 WebAPI 中创建自定义值提供程序](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx) |.

若要添加到多实例服务的自定义实现，调用**添加**或**插入**上**服务**集合：

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample5.cs)]

若要替换的自定义实现单实例服务，请调用**替换**上**服务**集合：

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample6.cs)]

<a id="percontrollerconfig"></a>
## <a name="per-controller-configuration"></a>每个控制器配置

您可以重写每个控制器基础上的以下设置：

- 媒体类型格式化程序
- 参数绑定规则
- 服务

为此，请定义实现的自定义特性**IControllerConfiguration**接口。 然后，将特性应用到控制器。

下面的示例将默认媒体类型格式化程序替换为自定义格式化程序。

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample7.cs)]

**IControllerConfiguration.Initialize**方法采用两个参数：

- **HttpControllerSettings**对象
- **HttpControllerDescriptor**对象

**HttpControllerDescriptor**包含控制器，它可以检查用于信息说明 （例如，若要区分两个控制器） 的说明。

使用**HttpControllerSettings**要配置控制器对象。 此对象包含你可以基于每个控制器重写的配置参数的子集。 不要更改任何设置默认为全局**HttpConfiguration**对象。
