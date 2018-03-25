---
uid: signalr/overview/security/persistent-connection-authorization
title: "身份验证和授权的 SignalR 永久性连接 |Microsoft 文档"
author: pfletcher
description: "本主题介绍如何强制在持续性连接上的进行授权。 有关将安全集成到 SignalR 应用程序的常规信息..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: e264677b-9c01-47ec-94f9-3cd8f08f94af
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/security/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: d559cfa21f6444b2361fd003b9ce3d2c9c6c57a4
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="authentication-and-authorization-for-signalr-persistent-connections"></a>身份验证和授权的 SignalR 永久性连接
====================
通过[Patrick Fletcher](https://github.com/pfletcher)， [Tom FitzMacken](https://github.com/tfitzmac)

> 本主题介绍如何强制在持续性连接上的进行授权。 有关将安全集成到 SignalR 应用程序的常规信息，请参阅[安全性简介](introduction-to-security.md)。 
> 
> ## <a name="software-versions-used-in-this-topic"></a>本主题中使用的软件版本
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR 版本 2
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>本主题的早期版本
> 
> 有关 SignalR 的早期版本的信息，请参阅[SignalR 较旧版本](../older-versions/index.md)。
> 
> ## <a name="questions-and-comments"></a>问题和意见
> 
> 请留下反馈上如何喜欢本教程的方式，我们可以提高在页面底部的注释中。 如果你有与本教程不直接相关的问题，你可以发布到[ASP.NET SignalR 论坛](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或[StackOverflow.com](http://stackoverflow.com/)。


## <a name="enforce-authorization"></a>强制授权

若要强制实施授权规则，当使用[PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx)必须重写`AuthorizeRequest`方法。 不能使用`Authorize`具有永久连接属性。 `AuthorizeRequest`方法由每个请求，以验证用户有权执行所请求的操作之前的 SignalR Framework 调用。 `AuthorizeRequest`方法不从客户端调用; 相反，验证用户身份通过应用程序的标准身份验证机制。

下面的示例演示如何限制请求发送到经过身份验证的用户。

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

你可以在 AuthorizeRequest 方法中; 中添加任何自定义的授权逻辑例如，检查用户是否属于特定角色。
