---
uid: signalr/overview/getting-started/supported-platforms
title: "受支持的平台 |Microsoft 文档"
author: pfletcher
description: "本指南介绍了 SignalR 支持哪些客户端和服务器。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: eac31beb-0f46-4afa-9def-e80904dea4f0
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/supported-platforms
msc.type: authoredcontent
ms.openlocfilehash: 1379b9fb638f67896d88d7aa4312d95280ef7318
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
<a name="supported-platforms"></a>支持的平台
====================
通过[Patrick Fletcher](https://github.com/pfletcher)

> 本指南介绍了 SignalR 支持哪些客户端和服务器。 
> 
> ## <a name="questions-and-comments"></a>问题和意见
> 
> 请留下反馈上如何喜欢本教程的方式，我们可以提高在页面底部的注释中。 如果你有与本教程不直接相关的问题，你可以发布到[ASP.NET SignalR 论坛](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或[StackOverflow.com](http://stackoverflow.com/)。


SignalR 支持在各种服务器和客户端配置下。 此外，每个传输选项具有一组自己的; 的要求如果传输的系统要求不可用，SignalR 会顺利地故障转移到其他传输。 有关 SignalR 支持的传输的详细信息，请参阅[传输和回退](introduction-to-signalr.md#transports)。

## <a name="server-system-requirements"></a>服务器系统要求

可以在各种服务器配置上承载的 SignalR 服务器组件。 本部分介绍支持的操作系统、.NET framework、 Internet 信息服务器和其他组件版本。

### <a name="supported-server-operating-systems"></a>支持的服务器操作系统

SignalR 服务器组件可以承载于以下服务器或客户端操作系统。 请注意，适用于 SignalR 使用 Websocket，Windows Server 2012 或 Windows 8 需要 （WebSocket 可以使用 Windows Azure 网站上，只要该站点的.NET framework 版本设置为 4.5，并且在站点的配置页中启用 Web 套接字）。

- Windows Server 2012
- Windows Server 2008 r2
- Windows 10
- Windows 8
- Windows 7
- Microsoft Azure

### <a name="supported-server-net-framework-version"></a>支持的服务器.NET Framework 版本

在.NET Framework 4.5 上仅支持 SignalR 2。 请参阅[推荐的更新](#updates)增强可靠性、 兼容性、 稳定性和性能的更新的部分。

### <a name="supported-server-iis-versions"></a>支持的服务器的 IIS 版本

当 SignalR 承载于 IIS 中时，支持以下版本。 请注意，是否使用客户端操作系统，例如用于开发 （Windows 8 或 Windows 7） 的完整版本的 IIS 或 Cassini 不应，因为将有 10 个并发连接的施加，这将非常快速地达到自连接以来的限制暂时性故障，频繁重新建立，而不会释放，立即在不再使用。 客户端操作系统上，应使用 IIS Express。

此外请注意，适用于 SignalR 使用 WebSocket，必须使用 IIS 8 或 IIS 8 Express，服务器必须使用 Windows 8、 Windows Server 2012 或更高版本，必须在 IIS 中启用 WebSocket。 有关如何启用 WebSocket 在 IIS 中的信息，请参阅[IIS 8.0 WebSocket 协议支持](https://www.iis.net/learn/get-started/whats-new-in-iis-8/iis-80-websocket-protocol-support)。

- IIS 8 IIS Express。
- IIS 7 和 7.5。 支持[无扩展名的 Url](https://support.microsoft.com/kb/980368)是必需的。
- IIS 必须运行在集成模式下;不支持经典模式。 如果在使用 Server-Sent 事件传输的经典模式下运行 IIS，可能遇到的最多 30 秒的消息延迟。
- 在承载应用程序必须在完全信任模式下运行。

## <a name="client-system-requirements"></a>客户端系统要求

SignalR 可在各种客户端平台。 本部分介绍有关在 web 浏览器、 Windows 桌面应用程序、 Silverlight 应用程序和移动设备使用 SignalR 的系统要求。

### <a name="web-browsers"></a>Web 浏览器

SignalR 可在各种 web 浏览器，但通常情况下，支持的最新两个版本。

使用 SignalR 在浏览器中的应用程序必须使用 jQuery 版本 1.6.4 或主要更高版本 （如 1.7.2、 1.8.2 或 1.9.1）。

SignalR 可在以下浏览器：

- Microsoft Internet Explorer 版本 8、 9、 10 和 11。 现代，桌面版和移动版本支持。
- Mozilla Firefox： 当前版本-1，Windows 和 Mac 版本。
- Google Chrome： 当前版本-1，Windows 和 Mac 版本。
- Safari： 当前版本-1，Mac 和 iOS 版本。
- Opera： 当前版本-1，仅限 Windows。
- Android 浏览器

除了需要某些浏览器，SignalR 使用各种传输具有其自己的要求。 下列传输支持在以下配置：

<a id="browser"></a>

**Web 浏览器传输要求**

| 传输 | Internet Explorer | Chrome （Windows 或 iOS） | Firefox | Safari （OSX 或 iOS） | Android |
| --- | --- | --- | --- | --- | --- |
| WebSockets | 10+ | 当前值-1 | 当前值-1 | 当前值-1 | 不可用 |
| 服务器发送事件 | 不可用 | 当前值-1 | 当前值-1 | 当前值-1 | 不可用 |
| ForeverFrame | 8+ | 不可用 | 不可用 | 不可用 | 4.1 |
| 长轮询 | 8+ | 当前值-1 | 当前值-1 | 当前值-1 | 4.1 |

\*: 6 + 所需的全部功能。

#### <a name="unsupported-browsers"></a>不受支持的浏览器

尽管 SignalR*可能*运行时未出现在旧的浏览器版本的主要问题，我们不主动测试 SignalR 在其中，通常不修复可能在它们中出现的 bug。

### <a name="windows-desktop-and-silverlight-applications"></a>Windows 桌面和 Silverlight 应用程序

除了在 web 浏览器中运行，SignalR 可以承载于独立 Windows 客户端或 Silverlight 应用程序。 Windows 桌面版和 Silverlight SignalR 应用程序具有以下系统要求。

- Windows XP SP3 或更高版本支持使用.NET 4 的应用程序。
- Windows Vista 或更高版本支持应用程序使用.NET Framework 4.5。

除了操作系统和.NET framework 的要求，供 SignalR 传输都具有其自己的要求。 下列传输支持在以下配置：

**Windows 桌面版和 Silverlight 传输要求**

| 传输 | .NET 应用程序 | Silverlight |
| --- | --- | --- |
| Web 套接字 | Windows 8 + 和.NET 4.5 + | 不可用 |
| 不限次数帧 | 不可用 | 不可用 |
| 服务器发送事件 | .NET 4+ | 5+ |
| 长轮询 | .NET 4+ | 5+ |

<a id="android"></a>

### <a name="windows-store-and-windows-phone-applications"></a>Windows 应用商店和 Windows Phone 应用程序

SignalR 可在 Windows 应用商店应用程序和 Windows Phone 8 应用程序。 下列传输支持在以下配置：

**Windows 应用商店和 Windows Phone 传输要求**

| 传输 | Windows 应用商店 /.NET | Windows 应用商店 / JavaScript | Windows Phone/ IE | Windows Phone/ .NET |
| --- | --- | --- | --- | --- |
| WebSockets | 不可用 | Win8+ | 8+ | 不可用 |
| 不限次数帧 | 不可用 | Win8+ | 7.5+ | 不可用 |
| 服务器发送事件 | Win8+ | 不可用 | 不可用 | 8+ |
| 长轮询 | Win8+ | Win8+ | 7.5+ | 8+ |

<a id="updates"></a>

## <a name="recommended-updates"></a>建议的更新

建议进行 SignalR 服务器以下更新：

- 有可用的.NET Framework 4.5 的更新[此处](https://support.microsoft.com/kb/2750149)。
- 对于 ASP.NET，Microsoft 将定期发布 Qfe。 这些应应用为可用。
