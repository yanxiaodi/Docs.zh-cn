---
uid: mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
title: "了解在 ASP.NET MVC 执行过程 |Microsoft 文档"
author: microsoft
description: "了解如何 ASP.NET MVC framework 处理分步的浏览器请求。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: d1608db3-660d-4079-8c15-f452ff01f1db
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
msc.type: authoredcontent
ms.openlocfilehash: 5837c6e49709d6b86ee52cd88ffd4759c1850544
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="understanding-the-aspnet-mvc-execution-process"></a>了解在 ASP.NET MVC 执行过程
====================
通过[Microsoft](https://github.com/microsoft)

> 了解如何 ASP.NET MVC framework 处理分步的浏览器请求。


基于 ASP.NET MVC Web 应用程序的请求首先穿过**UrlRoutingModule**对象，它是一个 HTTP 模块。 此模块将分析请求，并执行路由选择。 **UrlRoutingModule**对象选择与当前请求匹配的第一个路由对象。 (路由对象是一个类，实现**RouteBase**，并且是通常的实例**路由**类。)如果任何路由都不匹配， **UrlRoutingModule**对象不执行任何操作，并允许回退到正则 ASP.NET 或 IIS 请求处理请求。

从所选**路由**对象， **UrlRoutingModule**对象获取**IRouteHandler**与关联的对象**路由**对象。 通常情况下，在 MVC 应用程序，这将是实例**MvcRouteHandler**。 **IRouteHandler**实例创建**IHttpHandler**对象并将其传递**IHttpContext**对象。 默认情况下， **IHttpHandler**实例 MVC 为**MvcHandler**对象。 **MvcHandler**对象然后选择最终将要处理该请求的控制器。

> [!NOTE]
> 当在 IIS 7.0 中运行的 ASP.NET MVC Web 应用时，没有文件扩展名是必要条件 MVC 项目。 但是，在 IIS 6.0 中，该处理程序需要将.mvc 文件扩展名映射到 ASP.NET ISAPI DLL。


模块和处理程序是 ASP.NET MVC framework 的入口点。 他们执行以下操作：

- MVC Web 应用程序中选择相应的控制器。
- 获取特定的控制器实例。
- 调用该控制器的**执行**方法。

下面列出了一个 MVC Web 项目执行的各个阶段：

- 接收应用程序的第一个请求 

    - 在 Global.asax 文件中，**路由**对象添加到**RouteTable**对象。
- 执行路由 

    - **UrlRoutingModule**模块使用的第一个匹配**路由**对象在**RouteTable**集合创建**RouteData**对象，它然后使用来创建**requestcontext 已**(**IHttpContext**) 对象。
- 创建 MVC 请求处理程序 

    - **MvcRouteHandler**对象创建的实例**MvcHandler**类并将其传递**requestcontext 已**实例。
- 创建控制器 

    - **MvcHandler**对象使用**requestcontext 已**实例标识**IControllerFactory**对象 (通常的实例**DefaultControllerFactory**类) 来创建具有的控制器实例。
- 执行控制器- **MvcHandler**实例调用控制器 s**执行**方法。 |
- 调用操作 

    - 大多数控制器继承**控制器**基类。 为此，请的控制器**ControllerActionInvoker**控制器与相关联的对象确定哪种操作方法的控制器类来调用，然后调用该方法。
- 执行结果 

    - 典型的操作方法可能接收用户输入，准备适当的响应数据，并通过返回的结果类型执行结果。 可以执行内置的结果类型包括以下类型： **ViewResult** （这将视图呈现和是最常用的结果类型） **RedirectToRouteResult**， **RedirectResult**， **ContentResult**， **JsonResult**，和**EmptyResult**。
