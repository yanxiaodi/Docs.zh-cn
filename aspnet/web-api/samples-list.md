---
uid: web-api/samples-list
title: "Web API 示例列表 |Microsoft 文档"
author: rick-anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/18/2012
ms.topic: article
ms.assetid: 8cbd9d7f-7027-4390-b098-cb81a63ecd6f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/samples-list
msc.type: content
ms.openlocfilehash: 1e1f43bbeedfc052f0b3a3924f51b544a5a79dca
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="web-api-samples-list"></a>Web API 示例列表
====================
## <a name="httpclient-samples"></a>HttpClient 示例

**必应翻译示例** | [VS 2012 源](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fBingTranslateSample%2fReadMe.txt)

演示如何调用[Microsoft Translator 服务](https://msdn.microsoft.com/library/ff512419.aspx)使用**HttpClient**类。 Microsoft Translator 服务 API 要求应用程序请求发送到转换器服务的每个请求的 Azure 令牌服务器获取 OAuth 令牌。 令牌的服务器的结果会提供给翻译服务发送的请求。 运行此示例之前，你必须获取[从 Azure 应用商店的应用程序密钥](https://msdn.microsoft.com/library/hh454950.aspx)并填写 AccessTokenMessageHandler 示例类中的信息。

**Google 地图示例** | [详细说明](https://blogs.msdn.com/b/henrikn/archive/2012/02/17/downloading-a-google-map-to-local-file.aspx) | [VS 2012 源](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fGoogleMapsSample%2fReadMe.txt)

使用**HttpClient**若要下载的 Redmond，WA 从地图[Google 地图 API](https://developers.google.com/maps/)、 将其保存为本地文件，并打开默认图像查看器。

**Twitter 客户端示例** | [详细说明](https://blogs.msdn.com/b/henrikn/archive/2012/02/16/extending-httpclient-with-oauth-to-access-twitter.aspx) | [VS 2012 源](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fTwitterSample%2fReadMe.txt)

演示如何编写一个简单的 Twitter 客户端使用**HttpClient**。 此示例使用**HttpMessageHandler** OAuth 身份验证信息插入传出**HttpRequestMessage**。 使用 JSON.NET 读取 Twitter 的结果。 运行此示例之前，你必须获取[Twitter 中的应用程序密钥](https://dev.twitter.com/)，并填写 OAuthMessageHandler 示例类中的信息。

**World Bank 示例** | [详细说明](https://blogs.msdn.com/b/henrikn/archive/2012/02/16/httpclient-is-here.aspx) | [VS 2010 源](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fHttpClient%2fWorldBankSample%2fReadMe.txt) | [VS 2012 源](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fWorldBankSample%2fReadMe.txt)

演示如何从使用 JSON.NET 分析结果 World Bank 数据站点检索数据。

## <a name="web-api-samples"></a>Web API 示例

**ASP.NET Web API 入门** | [VS 2012 源](overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)

演示如何创建基本 web API 支持 HTTP GET 请求。 本教程中包含的源代码[你的第一个 ASP.NET Web API](overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)。

**ASP.NET Web API JavaScript 方案 – 注释** | [VS 2012 源](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7)

演示如何使用 ASP.NET Web API 构建 web Api 支持浏览器客户端并使用 jQuery 来轻松地调用。

**联系人管理器** | [VS 2010 源](https://code.msdn.microsoft.com/Contact-Manager-Web-API-0e8e373d)

此示例使用 ASP.NET Web API 来构建一个简单的联系人管理器应用程序。 该应用程序包含的联系人管理器 web API 由 ASP.NET MVC 应用程序和 Windows Phone 应用程序使用来显示和管理的联系人列表。

**批处理示例** | [详细说明](http://trocolate.wordpress.com/2012/07/19/mitigate-issue-260-in-batching-scenario/) | [VS 2012 源](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fHostedBatchSample%2fReadMe.txt)

演示如何实现 HTTP 批处理在 ASP.NET 内。 批处理组成，将在一个单独的 MIME 多部分的实体主体，然后将它发送到 HTTP POST 作为服务器内的多个 HTTP 请求。 可以单独处理的请求和响应将放置到另一个 MIME 多部分的实体主体，就会归还客户端。

**内容控制器示例** | [详细说明](https://blogs.msdn.com/b/henrikn/archive/2012/02/24/async-actions-in-asp-net-web-api.aspx) | [VS 2010 源](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fContentControllerSample%2fReadMe.txt) | [VS 2012 源](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fContentControllerSample%2fReadMe.txt)

演示如何读取和写入请求和响应实体以异步方式使用流。 示例控制器都有两个操作： 的 PUT 操作的异步读取请求实体主体，并将其存储在本地文件中，并返回本地文件的内容的 GET 操作。

**自定义程序集冲突解决程序示例** | [VS 2012 源](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fCustomAssemblyResolverSample%2fReadMe.txt)

演示如何修改 ASP.NET Web API，以支持从动态加载的类库程序集的控制器的发现。 此示例实现一个自定义**IAssembliesResolver**其调用的默认实现，然后将库程序集添加到默认结果。

**自定义媒体类型格式化程序示例** | [详细说明](https://blogs.msdn.com/b/henrikn/archive/2012/04/23/using-cookies-with-asp-net-web-api.aspx) | [VS 2010 源](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fCustomMediaTypeFormatterSample%2fReadMe.txt)

演示如何创建自定义媒体类型格式化程序使用**BufferedMediaTypeFormatter**基类。 此基类用于格式化程序的主要使用同步读取和写入操作。 除了显示的媒体类型格式化程序，此示例演示如何通过注册的一部分进行挂钩**HttpConfiguration**你的应用程序。 请注意，它也可以使用**MediaTypeFormatter**基类直接，主要使用异步的格式化程序读取和写操作。

**自定义参数绑定示例** | [详细说明](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx) | [VS 2010 源](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fCustomParameterBinding%2fReadMe.txt)

演示如何自定义参数绑定过程中，是确定如何将请求中的信息绑定到操作参数的过程。 在此示例中，主控制器具有四个操作：

1. BindPrincipal 演示如何从自定义的泛型主体，不是从 HTTP GET 消息; 绑定 IPrincipal 参数
2. BindCustomComplexTypeFromUriOrBody 演示如何将绑定一个复杂类型参数，它可能会出现从消息正文或于请求 URI 的 HTTP POST 消息;
3. BindCustomComplexTypeFromUriWithRenamedProperty 演示如何将绑定使用来自请求 URI 的 HTTP POST 消息; 重命名属性的复杂类型参数
4. PostMultipleParametersFromBody 演示如何将多个参数绑定从 POST 消息; 正文

**文件上载示例** | [详细说明](https://blogs.msdn.com/b/henrikn/archive/2012/03/01/file-upload-and-asp-net-web-api.aspx) | [VS 2012 源](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fFileUploadSample%2fReadMe.txt)

演示如何将文件上载到**ApiController**使用 MIME Multipart 文件上载以及如何设置与进度通知**HttpClient**使用**ProgressNotificationHandler**. 控制器的 HTML 文件上载内容以异步方式读取和写入本地文件的一个或多个正文部分。 响应包含有关已上载文件 （或文件） 的信息。

**文件上载到 Azure Blob 存储示例** | [详细说明](https://blogs.msdn.com/b/yaohuang1/archive/2012/07/02/asp-net-web-api-and-azure-blob-storage.aspx) | [VS 2012 源](http://aspnet.codeplex.com/SourceControl/changeset/view/61dfed023e50#Samples%2fNet45%2fCS%2fWebApi%2fAzureBlobsFileUploadSample%2fReadMe.txt)

此示例是类似于文件上载示例中，但而不是保存在本地磁盘上已上载的文件，它以异步方式将文件上载到[Azure Blob 存储区](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs)使用[Windows Azure SDK for.NET](https://www.windowsazure.com/develop/net/)。 它还提供了一种机制，用于列出中当前存在的 blob [Azure Blob 存储容器](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs)。 你还可以尝试对运行此示例**Azure 存储模拟器**使用 Azure SDK 随附。 如果你有[Azure 存储帐户](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs)，你可以针对实际的存储服务以及运行。

**Http 消息处理程序管道示例** | [详细说明](https://blogs.msdn.com/b/henrikn/archive/2012/08/07/httpclient-httpclienthandler-and-httpwebrequesthandler.aspx) | [VS 2010 源](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fHttpMessageHandlerPipelineSample%2fReadMe.txt)

演示如何连接**HttpMessageHandler**这两个客户端上的实例 (**HttpClient**) 和服务器 (ASP.NET Web API)。 在此示例中，客户端和服务器上使用相同的处理程序。 尽管很少的确切的同一处理程序将运行在这两个位置中，对象模型是在客户端和服务器端相同的。

**上载示例 JSON** | [VS 2012 源](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fJsonUploadSample%2fReadMe.txt)

演示如何上载和下载 JSON 来回**ApiController**。 此示例使用最小**ApiController**和访问它使用**HttpClient**。

**混合示例** | [详细说明](https://blogs.msdn.com/b/henrikn/archive/2012/03/03/async-mashups-using-asp-net-web-api.aspx) | [VS 2012 源](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fMashupSample%2fReadMe.txt)

演示如何从以异步方式访问多个远程站点**ApiController**操作。 每次点击操作时，会执行请求以异步方式，以便任何线程被阻止。

**内存跟踪示例** | [详细说明](https://blogs.msdn.com/b/roncain/archive/2012/04/12/tracing-in-asp-net-web-api.aspx) | [VS 2010 源](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fMemoryTracingSample%2fReadMe.txt)

此示例项目创建 Nuget 程序包将安装到 ASP.NET Web API 应用程序的自定义的内存中跟踪编写器。

**MongoDB 示例** | [详细说明](https://blogs.msdn.com/b/henrikn/archive/2012/02/19/using-web-api-with-mongodb.aspx) | [VS 2012 源](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fMongoSample%2fReadMe.txt)

演示如何使用 MongoDB 作为持久性存储**ApiController**，使用的存储库模式。

**响应正文处理器示例** | [VS 2012 源](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fResponseEntityProcessorSample%2fReadMe.txt)

演示如何将响应实体 （即，一个 HTTP 响应正文） 复制到本地文件之前将它传送到客户端，并执行其他对该文件以异步方式处理。 此示例实现**HttpMessageHandler**包装响应实体具有一个同时本身写入，到将输出作为正常和本地文件。

**上载 XDocument 示例** | [详细说明](https://blogs.msdn.com/b/henrikn/archive/2012/02/17/push-and-pull-streams-using-httpclient.aspx) | [VS 2012 源](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fUploadXDocumentSample%2fReadMe.txt)

演示如何上载到 XDocument **ApiController**使用**PushStreamContent**和**HttpClient**。

**验证示例** | [VS 2010 源](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fValidationSample%2fReadMe.txt)

演示如何使用验证属性在 ASP.NET WebAPI 中的模型上验证 HTTP 请求的内容。 演示如何将标记为必选、 如何同时使用这两者框架定义和自定义验证属性进行批注您的模型的属性以及如何返回无效的模型状态的错误响应。

**Web 窗体示例** | [详细说明](https://blogs.msdn.com/b/henrikn/archive/2012/02/23/using-asp-net-web-api-with-asp-net-web-forms.aspx) | [VS 2010 源](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fWebFormSample%2fReadMe.txt)

显示添加到 Web 窗体项目 ApiController。

**[RestBugs 示例](https://github.com/howarddierking/RestBugs)**

RestBugs 是跟踪应用程序演示如何使用 ASP.NET Web API 和新的 HTTP 客户端库来创建超媒体驱动的系统的简单 bug。 示例包括客户端和服务器实现，使用 ASP.NET Web API。 服务器使用自定义的 Razor 格式化程序来生成资源表示形式。 该示例还提供了用于说明来自超媒体设计用于将客户端和服务器中分离出来的好处的 node.js 服务器。

## <a name="web-api-extensions-preview-samples"></a>Web API 扩展预览示例

**OData 查询示例** | [详细说明](https://blogs.msdn.com/b/alexj/archive/2012/08/15/odata-support-in-asp-net-web-api.aspx) | [VS 2010 源](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fODataQueryableSample%2fReadMe.txt)

演示如何使用 ASP.NET Web API 中的 OData 查询引入`[Queryable]`属性或通过使用**ODataQueryOptions**这样的操作之前正在执行手动检查查询的操作参数。

CustomerController 演示使用 [Queryable] 特性并 OrderController 演示如何使用 ODataQueryOptions 参数。 ResponseController 是类似于 CustomerController 但而不是返回的 GET 操作`IEnumerable<Customer>`它将返回**HttpResponseMessage**。 这使得我们可以添加额外的标头字段，同时仍可使用查询功能操作状态代码，等等。 此示例演示如何使用 $orderby、 $skip、 $top、 any （）、 all()，和 $filter 将查询。

**OData 服务示例** | [详细说明](https://blogs.msdn.com/b/alexj/archive/2012/08/15/odata-support-in-asp-net-web-api.aspx) | [VS 2010 源](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fODataServiceSample%2fReadMe.txt)

此示例演示如何创建 OData 服务包括三个实体和三个 Web API 控制器。 控制器都提供各种级别的功能在它们公开的 OData 功能方面：

SupplierController 公开包括查询的功能子集，由键和创建，会通过处理这些请求：

- 获取 /Suppliers
- 获取 /Suppliers(key)
- GET /Suppliers？ $filter =...&amp;$orderby =...&amp;$top =...&amp;$skip =...
- POST /Suppliers

ProductsController 公开 GET、 PUT、 POST、 删除和修补程序通过直接实现每个这些操作的操作。

ProductFamilesController 利用 EntitySetController 基类公开实现的丰富的 OData 服务的有用模式。

此外 OData 服务公开 $metadata 文档，它允许数据转换为使用由 WCF 数据服务客户端和接受 $metadata 格式的其他客户端。
