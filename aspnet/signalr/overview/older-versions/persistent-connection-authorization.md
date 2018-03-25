---
uid: signalr/overview/older-versions/persistent-connection-authorization
title: "身份验证和授权的 SignalR 永久性连接 (SignalR 1.x) |Microsoft 文档"
author: pfletcher
description: "本主题介绍如何强制在持续性连接上的进行授权。 有关将安全集成到 SignalR 应用程序的常规信息..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/21/2013
ms.topic: article
ms.assetid: c34bc627-41af-4c21-a817-e97a19a7f252
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 2e97dfd03c61b110325c41a992b4af490fcd17de
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a>身份验证和授权的 SignalR 永久性连接 (SignalR 1.x)
====================
通过[Patrick Fletcher](https://github.com/pfletcher)， [Tom FitzMacken](https://github.com/tfitzmac)

> 本主题介绍如何强制在持续性连接上的进行授权。 有关将安全集成到 SignalR 应用程序的常规信息，请参阅[安全性简介](index.md)。


## <a name="enforce-authorization"></a>强制授权

若要强制实施授权规则，当使用[PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx)必须重写`AuthorizeRequest`方法。 不能使用`Authorize`具有永久连接属性。 `AuthorizeRequest`方法由每个请求，以验证用户有权执行所请求的操作之前的 SignalR Framework 调用。 `AuthorizeRequest`方法不从客户端调用; 相反，验证用户身份通过应用程序的标准身份验证机制。

下面的示例演示如何限制请求发送到经过身份验证的用户。

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

你可以在 AuthorizeRequest 方法中; 中添加任何自定义的授权逻辑例如，检查用户是否属于特定角色。
