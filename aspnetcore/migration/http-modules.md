---
title: "迁移的 HTTP 处理程序和 ASP.NET Core 中间件的模块"
author: rick-anderson
description: 
manager: wpickett
ms.author: tdykstra
ms.date: 12/07/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/http-modules
ms.openlocfilehash: 7f08e155491b56933ae183818e9b9ee562ad8286
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="migrating-http-handlers-and-modules-to-aspnet-core-middleware"></a>迁移的 HTTP 处理程序和 ASP.NET Core 中间件的模块 

通过[Matt Perdeck](https://www.linkedin.com/in/mattperdeck)

这篇文章演示如何迁移现有的 ASP.NET [HTTP 模块和处理程序 system.webserver](https://docs.microsoft.com/iis/configuration/system.webserver/)到 ASP.NET 核心[中间件](xref:fundamentals/middleware/index)。

## <a name="modules-and-handlers-revisited"></a>模块和处理程序重新访问

在继续之前到 ASP.NET 核心中间件，让我们首先会扼要重述 HTTP 模块和处理程序的工作原理：

![模块处理程序](http-modules/_static/moduleshandlers.png)

**处理程序：**

   * 类实现[IHttpHandler](https://docs.microsoft.com/dotnet/api/system.web.ihttphandler)

   * 用于使用处理请求的给定的文件名或扩展，如*.report*

   * [配置](https://docs.microsoft.com//iis/configuration/system.webserver/handlers/)中*Web.config*

**模块为：**

   * 类实现[IHttpModule](https://docs.microsoft.com/dotnet/api/system.web.ihttpmodule)

   * 调用为每个请求

   * 能够短路 （停止进一步处理请求）

   * 无法添加到 HTTP 响应中，或创建自己

   * [配置](https://docs.microsoft.com//iis/configuration/system.webserver/modules/)中*Web.config*

**模块顺序处理传入的请求的顺序取决于：**

   1. [应用程序生命周期](https://msdn.microsoft.com/library/ms227673.aspx)，这是由 ASP.NET 激发的系列事件： [BeginRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.beginrequest)， [AuthenticateRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.authenticaterequest)等。每个模块可以创建一个或多个事件处理程序。

   2. 对于相同的事件，它们在配置中的顺序*Web.config*。

除模块，还可以添加到生命周期事件的处理程序你*Global.asax.cs*文件。 在已配置的模块中的处理程序后运行这些处理程序。

## <a name="from-handlers-and-modules-to-middleware"></a>从处理程序和到中间件模块

**中间件是 HTTP 模块和处理程序比简单得多：**

   * 模块、 处理程序， *Global.asax.cs*， *Web.config* （除外 IIS 配置） 和应用程序生命周期已消失

   * 模块和处理程序的角色具有接管中间件

   * 中间件配置使用代码而不是在*Web.config*

   * [管道分支](xref:fundamentals/middleware/index#middleware-run-map-use)允许将请求发送到特定的中间件，基于不仅也上请求标头、 查询字符串等的 URL。

**中间件是非常类似于模块：**

   * 在每个请求主体中调用

   * 能够通过短路请求[不将请求传递到下一步的中间件](#http-modules-shortcircuiting-middleware)

   * 能够创建他们自己的 HTTP 响应

**中间件和模块按不同顺序处理：**

   * 中间件的顺序基于在其中插入到请求管道中，而模块的顺序主要基于的顺序[应用程序生命周期](https://msdn.microsoft.com/library/ms227673.aspx)事件

   * 响应的中间件顺序是对于请求，与反向而模块的顺序是相同的请求和响应

   * 请参阅[使用 IApplicationBuilder 创建中间件管道](xref:fundamentals/middleware/index#creating-a-middleware-pipeline-with-iapplicationbuilder)

![中间件](http-modules/_static/middleware.png)

请注意如何在上图中，身份验证中间件 short-circuited 请求。

## <a name="migrating-module-code-to-middleware"></a>迁移到中间件模块代码

现有 HTTP 模块看起来类似于此：

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyModule.cs?highlight=6,8,24,31)]

中所示[中间件](xref:fundamentals/middleware/index)页上，ASP.NET Core 中间件是公开的类`Invoke`方法拍摄`HttpContext`并返回`Task`。 新中间件将如下所示：

<a name="http-modules-usemiddleware"></a>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddleware.cs?highlight=9,13,20,24,28,30,32)]

前面的中间件模板摘录自部分[编写中间件](xref:fundamentals/middleware/index#middleware-writing-middleware)。

*MyMiddlewareExtensions*帮助器类，更便于配置中的中间件你`Startup`类。 `UseMyMiddleware`方法将您中间件的类添加到请求管道。 所需的中间件服务获取注入到中间件的构造函数。

<a name="http-modules-shortcircuiting-middleware"></a>

你的模块可能终止请求，例如，如果用户未授权：

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyTerminatingModule.cs?highlight=9,10,11,12,13&name=snippet_Terminate)]

中间件的方式处理这通过不调用`Invoke`在管道中的下一步中间件。 请注意这不完全终止请求，因为响应使成为管道上返回到其方法时，仍可以调用以前的中间件。

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyTerminatingMiddleware.cs?highlight=7,8&name=snippet_Terminate)]

当你迁移到新中间件模块的功能时，你可能会发现你的代码不会编译，因为`HttpContext`类中 ASP.NET Core 已显著更改。 [更高版本上](#migrating-to-the-new-httpcontext)，你将了解如何将迁移到新的 ASP.NET 核心 HttpContext。

## <a name="migrating-module-insertion-into-the-request-pipeline"></a>迁移模块插入请求管道

HTTP 模块通常会添加到请求管道使用*Web.config*:

[!code-xml[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32-33,36,43,50,101)]

转换这一点[添加新中间件](xref:fundamentals/middleware/index#creating-a-middleware-pipeline-with-iapplicationbuilder)向请求管道中你`Startup`类：

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=16)]

你可以将插入新中间件管道中的确切点取决于它作为模块处理的事件 (`BeginRequest`，`EndRequest`等) 和在列表中的模块中的顺序*Web.config*。

如前面所述，没有任何应用程序生命周期中 ASP.NET 核心，中间件处理响应的顺序不同于使用模块的顺序。 这会使排序决策更具挑战性。

如果排序将为问题，无法将你的模块分成多个可在单独对其进行排序的中间件组件。

## <a name="migrating-handler-code-to-middleware"></a>迁移到中间件的处理程序代码

HTTP 处理程序如下所示：

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/HttpHandlers/ReportHandler.cs?highlight=5,7,13,14,15,16)]

在 ASP.NET Core 项目中，你将翻译以下到中间件类似于以下内容：

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/ReportHandlerMiddleware.cs?highlight=7,9,13,20,21,22,23,40,42,44)]

此中间件是非常类似于对应于模块的中间件。 唯一的真正的区别是，此处没有不需要调用`_next.Invoke(context)`。 有意义，因为该处理程序末尾的请求管道，因此将没有下一步的中间件来调用。

## <a name="migrating-handler-insertion-into-the-request-pipeline"></a>迁移的处理程序插入到请求管道

配置的 HTTP 处理程序中完成*Web.config*和如下所示：

[!code-xml[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32,46-48,50,101)]

你无法将其转换通过将新的处理程序中间件添加到请求管道中你`Startup`类，类似于从模块转换的中间件。 这种方法的问题是，它会将所有请求都发送到新的处理程序中间件。 但是，你只想具有给定扩展名的请求来访问中间件。 这样，你必须与 HTTP 处理程序的相同功能。

一种解决方案是分支的扩展名为给定的请求管道使用`MapWhen`扩展方法。 执行此操作在同一`Configure`你在其中添加其他中间件的方法：

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=27-34)]

`MapWhen` 使用这些参数：

1. 采用的 lambda`HttpContext`并返回`true`如果请求应会减少分支。 这意味着可以分支请求而不仅仅是基于其扩展，而且取决请求标头、 查询字符串参数，等等。

2. 采用的 lambda`IApplicationBuilder`并添加分支的所有中间件。 这意味着处理程序中间件的前面添加到分支其他中间件。

中间件将添加到管道，然后将所有请求; 调用分支分支将在其上没有任何影响。

## <a name="loading-middleware-options-using-the-options-pattern"></a>加载使用选项模式的中间件选项

一些模块和处理程序已在存储的配置选项*Web.config*。但是，在 ASP.NET 核心中新的配置模型使用代替了*Web.config*。

新[配置系统](xref:fundamentals/configuration/index)为你提供了这些选项，以解决此问题：

* 直接注入到中间件，选项中所示[下一节](#loading-middleware-options-through-direct-injection)。

* 使用[选项模式](xref:fundamentals/configuration/options):

1.  创建一个类来保存中间件的选项，例如：

    [!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Options)]

2.  存储选项的值

    配置系统，可存储选项任意位置所需的值。 但是，最站点使用*appsettings.json*，因此我们将采用这种办法：

    [!code-json[](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,14-18)]

    *MyMiddlewareOptionsSection*下面是部分名称。 它不必是你的选项类别的名称相同。

3. 将选项值与选项类相关联

    选项模式使用 ASP.NET 核心依赖关系注入框架将选项类型相关联 (如`MyMiddlewareOptions`) 与`MyMiddlewareOptions`具有实际选项对象。

    更新你`Startup`类：

    1.  如果你使用*appsettings.json*，将其添加到中的配置生成器`Startup`构造函数：

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Ctor&highlight=5-6)]

    2.  配置选项服务：

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

    3.  将你的选项与选项类相关联：

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=6-8)]

4.  插入到中间件构造函数的选项。 这是类似于将注入到控制器的选项。

  [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_MiddlewareWithParams&highlight=4,7,10,15-16)]

  [UseMiddleware](#http-modules-usemiddleware)将添加到中间件的扩展方法`IApplicationBuilder`负责的依赖关系注入。

  这已不再局限于`IOptions`对象。 这种方式，可插入中间件需要的任何其他对象。

## <a name="loading-middleware-options-through-direct-injection"></a>加载通过直接注入的中间件选项

选项模式具有创建松散耦合选项值与其使用者之间的优点。 一旦你已为实际的选项值其关联选项类别，任何其他类可获得访问通过依赖关系注入框架的选项。 没有无需传递选项值。

这将分解但如果你想要使用相同的中间件两次，使用不同的选项。 例如授权中间允许不同的角色的不同分支中使用。 不能将两个不同的选项对象与一个选项类相关联。

解决方法是获取使用中的实际选项值的选项对象你`Startup`类并将这些直接向中间件的每个实例参数传递。

1.  添加到的第二个键*appsettings.json*

    若要添加另一组选项*appsettings.json*文件中，使用新密钥来唯一标识它：

    [!code-json[](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,10-18&highlight=2-5)]

2.  检索选项值，并将它们传递给中间件。 `Use...`扩展方法 （其中添加到管道的中间件） 是传入选项值的逻辑位置： 

    [!code-csharp[](http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=20-23)]

4.  启用的中间件来采用选项参数。 提供的重载`Use...`扩展方法 (它接收 options 参数并将其传递给`UseMiddleware`)。 当`UseMiddleware`调用具有参数，它将参数传递给中间件构造函数时它实例化的中间件对象。

    [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Extensions&highlight=9-14)]

    请注意这如何包装中的选项对象`OptionsWrapper`对象。 这将实现`IOptions`，如下所需的中间件构造函数。

## <a name="migrating-to-the-new-httpcontext"></a>迁移到新 HttpContext

你此前看到的`Invoke`中间件中的方法采用一个参数的类型`HttpContext`:

```csharp
public async Task Invoke(HttpContext context)
```

`HttpContext` 已显著更改 ASP.NET Core 中。 本部分说明如何将转换的最常用的属性[System.Web.HttpContext](https://docs.microsoft.com/dotnet/api/system.web.httpcontext)对新`Microsoft.AspNetCore.Http.HttpContext`。

### <a name="httpcontext"></a>HttpContext

**HttpContext.Items**都会转换为：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Items)]

**唯一请求 ID （没有 System.Web.HttpContext 对应项）**

为你的唯一 id 用于每个请求。 日志中包括非常有用。

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Trace)]

### <a name="httpcontextrequest"></a>HttpContext.Request

**HttpContext.Request.HttpMethod**都会转换为：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Method)]

**HttpContext.Request.QueryString**都会转换为：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Query)]

**HttpContext.Request.Url**和**HttpContext.Request.RawUrl**将转换为：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Url)]

**HttpContext.Request.IsSecureConnection**都会转换为：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Secure)]

**HttpContext.Request.UserHostAddress**都会转换为：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Host)]

**HttpContext.Request.Cookies**都会转换为：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Cookies)]

**HttpContext.Request.RequestContext.RouteData** translates to:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Route)]

**HttpContext.Request.Headers**都会转换为：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Headers)]

**HttpContext.Request.UserAgent**都会转换为：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Agent)]

**HttpContext.Request.UrlReferrer** translates to:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Referrer)]

**HttpContext.Request.ContentType**都会转换为：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Type)]

**HttpContext.Request.Form**都会转换为：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Form)]

> [!WARNING]
> 仅当内容的子类型为读取窗体值*x-响应客户-窗体-urlencoded*或*窗体数据*。

**HttpContext.Request.InputStream**都会转换为：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Input)]

> [!WARNING]
> 仅在处理程序类型中间件，末尾的管道中使用此代码。
>
>每个请求的唯一一次如上所示，你可以阅读原始的正文。 尝试后第一次读取读取正文的中间件将读取正文为空。
>
>这不适用于读取窗体，如下所示更早版本，因为缓冲区中完成。

### <a name="httpcontextresponse"></a>HttpContext.Response

**HttpContext.Response.Status**和**HttpContext.Response.StatusDescription**将转换为：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Status)]

**HttpContext.Response.ContentEncoding**和**HttpContext.Response.ContentType**将转换为：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespType)]

**HttpContext.Response.ContentType**上其自身还转换为：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespTypeOnly)]

**HttpContext.Response.Output**都会转换为：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Output)]

**HttpContext.Response.TransmitFile**

为文件提供服务讨论[此处](../fundamentals/request-features.md#middleware-and-request-features)。

**HttpContext.Response.Headers**

发送响应标头非常复杂，这一事实，如果任何内容都已写入响应正文将它们设置，它们将不发送。

解决方案是将设置将右之前调用写入响应启动的回调方法。 最好的做法是在开始`Invoke`中间件中的方法。 这是此回调方法，设置响应标头。

下面的代码设置调用的回调方法`SetHeaders`:

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

`SetHeaders`回调方法将如下所示：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetHeaders)]

**HttpContext.Response.Cookies**

Cookie 出差到中的浏览器*Set-cookie*响应标头。 因此，发送 cookie 都需要用于发送响应标头为使用相同的回调：

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetCookies, state: httpContext);
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

`SetCookies`回调方法将如下所示：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetCookies)]

## <a name="additional-resources"></a>其他资源

* [HTTP 处理程序和 HTTP 模块概述](/iis/configuration/system.webserver/)
* [配置](xref:fundamentals/configuration/index)
* [应用程序启动](xref:fundamentals/startup)
* [中间件](xref:fundamentals/middleware/index)
