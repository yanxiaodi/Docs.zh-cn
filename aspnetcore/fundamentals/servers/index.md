---
title: "ASP.NET Core 中的 Web 服务器实现"
author: tdykstra
description: "发现适用于 ASP.NET Core 的 Web 服务器 Kestrel 和 HTTP.sys。 了解如何进行选择以及何时将其与反向代理服务器结合使用。"
manager: wpickett
ms.author: tdykstra
ms.date: 08/03/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/index
ms.openlocfilehash: b9a7fa4e33c56a5973b4bc35f88ca0ebb3d67101
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="web-server-implementations-in-aspnet-core"></a>ASP.NET Core 中的 Web 服务器实现

作者：[Tom Dykstra](https://github.com/tdykstra)、 [Steve Smith](https://ardalis.com/)、[Stephen Halter](https://twitter.com/halter73) 和 [Chris Ross](https://github.com/Tratcher)

ASP.NET Core 应用程序与进程内 HTTP 服务器实现一起运行。 服务器实现侦听 HTTP 请求，并在一系列[请求功能](https://docs.microsoft.com/aspnet/core/fundamentals/request-features)被撰写到 `HttpContext` 时将这些请求展现到应用程序中。

ASP.NET Core 交付两种服务器实现：

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

* [Kestrel](kestrel.md) 是跨平台 HTTP 服务器，它基于 [libuv](https://github.com/libuv/libuv)（一个跨平台异步 I/O 库）。

* [HTTP.sys](httpsys.md) 是仅适用于 Windows 的 HTTP 服务器，它基于 [Http.Sys 核心驱动程序](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* [Kestrel](kestrel.md) 是跨平台 HTTP 服务器，它基于 [libuv](https://github.com/libuv/libuv)（一个跨平台异步 I/O 库）。

* [WebListener](weblistener.md) 是仅适用于 Windows 的 HTTP 服务器，它基于 [Http.Sys 核心驱动程序](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)。

---

## <a name="kestrel"></a>Kestrel

Kestrel 是 Web 服务器，默认包括在 ASP.NET Core 新项目模板中。 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

可以单独使用 Kestrel，也可以将其与反向代理服务器（如 IIS、Nginx 或 Apache）结合使用。 反向代理服务器接收到来自 Internet 的 HTTP 请求，并在进行一些初步处理后将这些请求转发到 Kestrel。

![Kestrel 直接与 Internet 通信，不使用反向代理服务器](kestrel/_static/kestrel-to-internet2.png)

![Kestrel 通过反向代理服务器（如 IIS、Nginx 或 Apache）间接与 Internet 进行通信](kestrel/_static/kestrel-to-internet.png)

如果仅向内部网络公开 Kestrel，可以使用任一配置（无需考虑是否使用反向代理服务器）。

有关何时将 Kestrel 与反向代理结合使用的信息，请参阅 [Kestrel 简介](kestrel.md)。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

如果应用程序仅接受来自内部网络的请求，则可以单独使用 Kestrel。

![Kestrel 直接与你的内部网络进行通信](kestrel/_static/kestrel-to-internal.png)

如果将应用程序公开到 Internet，则必须将 IIS、Nginx 或 Apache 用作反向代理服务器。 反向代理服务器接收到来自 Internet 的 HTTP 请求，并在进行一些初步处理后将其转发到 Kestrel，如下图所示。

![Kestrel 通过反向代理服务器（如 IIS、Nginx 或 Apache）间接与 Internet 进行通信](kestrel/_static/kestrel-to-internet.png)

为边缘部署使用反向代理的最重要的原因（从 Internet 公开到流量）是出于安全考虑。 Kestrel 的 1.x 版本不包含对防御攻击的充分补充。 这包括但不限于相应的超时、大小限制和并发连接限制。

有关何时将 Kestrel 与反向代理结合使用的信息，请参阅 [Kestrel 简介](kestrel.md)。

---

在没有 Kestrel 或[自定义服务器实现](#custom-servers)的情况下，不能使用 IIS、Nginx 或 Apache。 ASP.NET Core 设计为在其自己的进程中运行，以实现跨平台统一操作。 IIS、 Nginx 和 Apache 规定自己的启动进程和环境；若要直接使用它们，ASP.NET Core 必须满足其各自的需求。 使用 Kestrel 等 Web 服务器实现可提供对启动进程和环境的 ASP.NET Core 控制。 因此，只需将这些 Web 服务器设置为对 Kestrel 发出代理请求，而无需尝试将 ASP.NET Core 应用到 IIS、Nginx 或 Apache。 无论你部署的位置如何，此安排都可以使 `Program.Main` 和 `Startup` 类在实质上相同。

### <a name="iis-with-kestrel"></a>IIS 与 Kestrel

将 IIS 或 IIS Express 用作 ASP.NET Core 的反向代理时，ASP.NET Core 应用程序在独立于 IIS 工作进程的某个进程中运行。 在 IIS 进程中，运行一个特殊的 IIS 模块，以协调反向代理关系。  这就是 ASP.NET Core 模块。 ASP.NET Core 模块的主要功能是启动 ASP.NET Core 应用程序，在其出现故障时重新启动，并向其转发 HTTP 流量。 有关详细信息，请参阅 [ASP.NET Core 模块](aspnet-core-module.md)。 

### <a name="nginx-with-kestrel"></a>Nginx 与 Kestrel

有关如何在 Linux 上使用 Nginx 作为 Kestrel 的反向代理服务器的信息，请参阅[在 Linux 上使用 Nginx 进行托管](xref:host-and-deploy/linux-nginx)。

### <a name="apache-with-kestrel"></a>Apache 与 Kestrel

有关如何在 Linux 上使用 Apache 作为 Kestrel 的反向代理服务器的信息，请参阅[在 Linux 上使用 Apache 进行托管](xref:host-and-deploy/linux-apache)。

## <a name="httpsys"></a>HTTP.sys

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

如果在 Windows 上运行 ASP.NET Core 应用，则 HTTP.sys 是 Kestrel 的替代方法。 如果想要在方案中向 Internet 公开应用并且需要使用 Kestrel 并不支持的 HTTP.sys 功能，可以为该方案使用 HTTP.sys。 

![HTTP.sys 直接与 Internet 进行通信](httpsys/_static/httpsys-to-internet.png)

对于仅向内部网络公开的应用程序而言，HTTP.sys 同样适用。 

![HTTP.sys 直接与你的内部网络进行通信](httpsys/_static/httpsys-to-internal.png)

对于内部网络方案，通常建议使用 Kestrel 以获得最佳性能；但在某些方案中，你可能只想使用 HTTP.sys 提供的功能。 有关 HTTP.sys 功能的信息，请参阅 [HTTP.sys](httpsys.md)。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

HTTP.sys 在 ASP.NET Core 1.x 中被命名为 WebListener。 如果在 Windows 上运行 ASP.NET Core 应用，在你想要向 Internet 公开应用但无法使用 IIS 时，可以为方案使用 WebListener 作为替代方法。

![Weblistener 直接与 Internet 进行通信](weblistener/_static/weblistener-to-internet.png)

如果需要使用 Kestrel 并不支持的 WebListener 功能，则对于仅向内部网络公开的应用而言，还可以使用 WebListener 来替代 Kestrel。 

![Weblistener 直接与你的内部网络进行通信](weblistener/_static/weblistener-to-internal.png)

对于内部网络方案，通常建议使用 Kestrel 以获得最佳性能，但在某些方案中，你可能只想使用 WebListener 提供的功能。 有关 WebListener 功能的信息，请参阅 [WebListener](weblistener.md)。

---

## <a name="notes-about-aspnet-core-server-infrastructure"></a>有关 ASP.NET Core 服务器基础结构的说明

`Startup` 类 `Configure`方法中提供的 [`IApplicationBuilder`](/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder) 公开了类型 [`IFeatureCollection`](/aspnet/core/api/microsoft.aspnetcore.http.features.ifeaturecollection) 的 `ServerFeatures` 属性。 Kestrel 和 WebListener 仅公开单个功能，即 [`IServerAddressesFeature`](/aspnet/core/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature)，但是不同的服务器实现可能公开其他功能。

`IServerAddressesFeature` 可用于查找在运行时服务器实现绑定的的端口类型。

## <a name="custom-servers"></a>自定义服务器

如果内置服务器无法满足你的需求，可以创建一个自定义服务器实现。 [.NET 的开放 Web 接口 (OWIN) 指南](../owin.md) 演示了如何编写基于 [Nowin](https://github.com/Bobris/Nowin) 的 [IServer](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.server.iserver) 实现。 可以仅执行应用程序所需的功能接口，但至少必须支持 [IHttpRequestFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttprequestfeature) 和 [IHttpResponseFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttpresponsefeature)。

## <a name="next-steps"></a>后续步骤

有关更多信息，请参见以下资源：

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

- [Kestrel](kestrel.md)
- [Kestrel 与 IIS](aspnet-core-module.md)
- [在 Linux 上使用 Nginx 进行托管](xref:host-and-deploy/linux-nginx)
- [在 Linux 上使用 Apache 进行托管](xref:host-and-deploy/linux-apache)
- [HTTP.sys](httpsys.md)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

- [Kestrel](kestrel.md)
- [Kestrel 与 IIS](aspnet-core-module.md)
- [在 Linux 上使用 Nginx 进行托管](xref:host-and-deploy/linux-nginx)
- [在 Linux 上使用 Apache 进行托管](xref:host-and-deploy/linux-apache)
- [WebListener](weblistener.md)

---
