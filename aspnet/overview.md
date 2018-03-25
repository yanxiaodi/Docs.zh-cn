---
uid: overview
title: "ASP.NET 概述 |Microsoft 文档"
author: rick-anderson
description: "ASP.NET，可用的框架，用于创建网站、 web 应用程序和 web Api 的简介。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/12/2010
ms.topic: article
ms.assetid: 3a309468-f1ca-4e51-b9c3-536af79d7a8b
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: 
msc.type: content
ms.openlocfilehash: 0ba7814d4004b17e678eab9a2a41a6d6f34773e1
ms.sourcegitcommit: 53ee14b9c8200f44705d8997c3619fa874192d45
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2018
---
# <a name="aspnet-overview"></a>ASP.NET 概述

ASP.NET 是用于构建出色的网站和 web 应用程序使用 HTML、 CSS 和 JavaScript 的免费 web 框架。 你还可以创建的 Web Api 和比如 Web 套接字使用实时技术。

[ASP.NET 核心](https://docs.microsoft.com/aspnet/core/)是 ASP.NET 的替代方法。  请参阅[指导如何选择 ASP.NET 和 ASP.NET Core 之间](https://docs.microsoft.com/aspnet/core/choose-aspnet-framework)。

## <a name="get-started"></a>入门

[Visual Studio 社区 2017年](https://www.visualstudio.com/downloads/)、 适用于 Windows 上的 ASP.NET 免费 IDE。

## <a name="websites-and-web-applications"></a>网站和 web 应用程序

 ASP.NET 提供了三个框架用于创建 web 应用程序： Web 窗体、 ASP.NET MVC 和 ASP.NET Web Pages。 所有三个框架处于稳定状态并且成熟，并可以使用其中任何创建好的 web 应用程序。 无论你选择何种框架，你将获取所有的优点和无处不在 ASP.NET 的功能。

每个框架都针对不同的开发风格。 您选择何种取决于编程资产 （知识、 技能和开发体验） 的组合，你要创建的应用程序和您很熟悉的开发方法的类型。

下面是每个框架和有关如何选择它们之间的一些观点的概述。 如果你喜欢的视频介绍，请参见[使网站使用 ASP.NET](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/Making-Websites-with-ASPNET)和[Web 工具是什么？](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/what-is-web-tools)

|   | 如果你具有的经验 | 开发样式 | 专业知识 | 
|-----------|----------------------|-----------------------------------------------------|----------------|
| Web Forms — Web 窗体 | Win 窗体、 WPF、.NET | 使用丰富的封装 HTML 标记的控件库的快速开发 | 中间层的高级 RAD |
| MVC       | Ruby on Rails，.NET  | 对 HTML 标记、 代码和标记分离，且轻松地编写测试的完全控制。 移动和单页面应用程序 (SPA) 的最佳选择。 | 中间层高级 |
| 网页  | 经典 ASP 中 PHP     | HTML 标记和在同一文件一起代码 | 新的、 中间层 |

### <a name="web-forms"></a>Web Forms — Web 窗体

使用 ASP.NET Web 窗体，你可以构建动态网站使用熟悉的拖放、 事件驱动模型。 设计图面和数百个控件和组件可以快速生成允许复杂且功能强大的 UI 驱动站点数据访问。 

[了解有关 Web 窗体的详细信息](web-forms/index.md)

### <a name="mvc"></a>MVC

ASP.NET MVC 为你提供了功能强大、 基于模式的方式构建动态网站这样完全分离关注点，并让你完全控制标记进行愉快、 灵活的开发。 ASP.NET MVC 包括许多功能，启用快速、 TDD 友好的开发，以便创建使用最新的 web 标准的复杂应用程序。 

[了解有关 MVC 的详细信息](mvc/index.md)

### <a name="aspnet-web-pages"></a>ASP.NET 网页

ASP.NET 网页和 Razor 语法提供快速、 便于访问，轻量的方法与 HTML 中以创建动态 web 内容组合服务器代码。 连接到数据库、 添加视频、 链接到社交网络站点，并包括许多多个功能，可帮助你创建美观符合最新的 web 标准的站点。

[了解有关 Web 页的详细信息](web-pages/index.md)

### <a name="notes-about-web-forms-mvc-and-web-pages"></a>有关 Web 窗体、 MVC 和 Web 页的说明

所有三个的 ASP.NET 框架在基于.NET Framework 和共享的.NET 和 ASP.NET 的核心功能。 例如，所有三个框架提供基于围绕成员身份的登录安全模型，并且所有三个共享作为核心 ASP.NET 功能的一部分用于管理请求、 处理会话和等等的相同功能。

此外，三个框架不是完全不相关，并选择一个不会阻止使用另一个。 框架可以共存于同一 web 应用程序，因为它不是少见的编写使用的不同框架的应用程序的各个组件。 例如，可能在 MVC 来优化标记，而在 Web 窗体以充分利用数据控件和简单数据访问开发数据访问和管理的部分中开发的应用程序面向客户的部分。

## <a name="web-apis"></a>Web API

ASP.NET Web API 是一个框架，可以轻松地生成覆盖广泛的客户端，包括浏览器和移动设备的 HTTP 服务。 ASP.NET Web API 是用于在 .NET Framework 上生成 RESTful 应用程序的理想平台。

[了解有关 Web API 的详细信息](web-api/index.md)

<!-- Put first under Web API TOC:  Watch video (9 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/services-and-aspnet -->

## <a name="real-time-technologies"></a>实时技术

ASP.NET SignalR 是一个新的 ASP.NET 开发人员库，可以更加轻松开发实时 web 功能。 SignalR 允许服务器和客户端之间的双向通信。 服务器可以推送到连接的客户端立即内容变得可用。 SignalR 支持 Web 套接字，将回退到旧版浏览器的其他兼容的技术。 SignalR 包括用于连接管理的 Api （例如，连接和断开连接事件），分组连接和授权。

[了解有关 SignalR 的详细信息](signalr/index.md)

<!-- Put first under SignalR TOC:  Watch video (6 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/signalr-and-the-real-time-web -->

## <a name="mobile-apps-and-sites"></a>移动应用程序和站点 

ASP.NET 可以电源的 Web API 后端，以及使用响应式设计框架，如 Twitter Bootstrap 的移动 web 站点具有的本机移动应用程序。 如果要生成的本机移动应用，很容易创建基于 JSON 的 Web API 与句柄数据访问、 身份验证和你的应用程序的推送通知。 如果你正在构建响应性移动站点，你可以使用任何 CSS 框架或打开网格系统你愿意，也可以选择功能强大的移动系统，如 jQuery Mobile 或 Sencha 以及使用 PhoneGap 的极佳移动应用程序。

[了解有关移动应用程序和网站开发的详细信息](mobile/index.md)

<!-- Put first under mobile TOC:  Watch video (11 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/aspnet-and-mobile -->

## <a name="single-page-applications"></a>单页面应用程序 

ASP.NET 单页面应用程序 (SPA) 可帮助你生成应用程序包括使用 HTML 5、 CSS 3 和 JavaScript 的重要客户端交互。 Visual Studio 包含一个用于构建单页面应用程序使用 knockout.js 和 ASP.NET Web API 模板。 除了内置的 SPA 模板，社区创建的 SPA 模板也是可供下载的。

[了解有关单页面应用程序开发的详细信息](single-page-application/index.md)

## <a name="webhooks"></a>WebHook

Webhook 是一种轻型的 HTTP 模式，为连接在一起的 Web Api 和 SaaS 服务提供简单的发布/订阅模型。 时事件发生在服务中，通知形式的 HTTP POST 请求发送到已注册的订阅服务器。 POST 请求包含有关事件这样就可以接收方可以采取相应行动的信息。

Webhook 公开的大量服务包括 Dropbox、 GitHub、 Instagram、 MailChimp、 PayPal、 Slack、 Trello 和多。 例如，WebHook 可能表示 Dropbox 中已更改文件或代码更改已提交在 GitHub 中，或已在 PayPal，启动付款或卡 Trello 中已经创建。

[了解有关 Webhook 详细信息](webhooks/index.md)





<!--
Create Deployment TOC based on https://www.asp.net/aspnet/overview/deployment
Copy deployment content map to MVC, WebForms, Web Pages, Web API sections.
Copy Web Deployment in Enterprise from WebForms to MVC
Move under ASP.NET Best practices
    What not to do in ASP.NET, and what to do instead https://review.docs.microsoft.cus/aspnet/aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
    Async and await https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/async-and-await
    Building Real World Cloud Apps with Azure https://review.docs.microsoft.com/aspnet/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
    Hands on Lab: Maintainable Azure Websites: Managing Change and Scale https://review.docs.microsoft.com/aspnet/aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale

-->
