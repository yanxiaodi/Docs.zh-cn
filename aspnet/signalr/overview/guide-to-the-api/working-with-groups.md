---
uid: signalr/overview/guide-to-the-api/working-with-groups
title: "使用 SignalR 中的组 |Microsoft 文档"
author: pfletcher
description: "本主题介绍如何保持与中心 API 的组成员身份信息。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: cd378ecd-3e9e-4236-b902-65916d85a048
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/working-with-groups
msc.type: authoredcontent
ms.openlocfilehash: 11f5be1ac4e74b692f0db3daac971a2c9d74a64c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="working-with-groups-in-signalr"></a>使用 SignalR 中的组
====================
通过[Patrick Fletcher](https://github.com/pfletcher)， [Tom FitzMacken](https://github.com/tfitzmac)

> 本主题介绍如何将用户添加到组和保留组成员身份信息。 
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


## <a name="overview"></a>概述

SignalR 中的组提供一种方法将消息广播到连接的客户端的指定子集。 组可以具有任意数量的客户端，并且客户端可以是任意数量的组的成员。 你无需显式创建组。 实际上，第一次 Groups.Add，对的调用中指定其名称自动创建一个组并将其删除从中它的成员身份中删除最后一次连接时。 有关使用组的简介，请参阅[如何从中心类管理组成员身份](hubs-api-guide-server.md#groupsfromhub)中心 API-Server 指南中。

用于获取组成员身份列表或组的列表中没有任何 API。 SignalR 将消息发送到客户端和基于发布/订阅模型的组和服务器不维护组或组成员身份的列表。 这有助于最大程度提高可伸缩性，因为每当向 web 场添加节点时，SignalR 维护任何状态都将传播到新节点。

当将用户添加到组使用`Groups.Add`方法，用户会收到消息定向到该组的当前连接的持续时间内，但该组中的用户的成员身份不会保留超出当前的连接。 如果你想要永久保留关于组和组成员身份的信息，你必须如数据库或 Azure 表存储存储库中存储该数据。 然后，用户连接到你的应用程序，每次你从存储库中检索用户所属的组并手动将该用户添加到这些组。

当重新连接暂时中断后，用户自动重新将加入的以前分配的组。 自动重新加入一组仅适用时重新连接后，不是在建立新连接时。 从包含以前分配的组列表中的客户端传递进行数字签名的令牌。 如果你想要验证用户是否属于请求的组，则可以重写默认行为。

本主题包括以下部分：

- [添加和删除用户](#add)
- [调用组的成员](#call)
- [在数据库中存储组成员身份](#storedatabase)
- [在 Azure 表存储中存储组成员身份](#storeazuretable)
- [当重新连接时验证组成员身份](#verify)

<a id="add"></a>

## <a name="adding-and-removing-users"></a>添加和删除用户

若要添加或从组中删除用户，请调用[添加](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx)或[删除](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx)方法，并在用户的连接 id 和组的名称作为参数传递。 不需要手动用户从组中删除连接结束时。

下面的示例演示`Groups.Add`和`Groups.Remove`中心方法中使用的方法。

[!code-csharp[Main](working-with-groups/samples/sample1.cs?highlight=5,10)]

`Groups.Add`和`Groups.Remove`方法异步执行。

如果你想要将客户端添加到组并立即向客户端发送一条消息，通过使用组，你必须确保 Groups.Add 方法先完成。 下面的代码示例演示如何执行该操作。

[!code-csharp[Main](working-with-groups/samples/sample2.cs?highlight=1,3)]

一般情况下，不应包含`await`调用时`Groups.Remove`方法因为想要删除的连接 id 可能不再可用。 在这种情况下，`TaskCanceledException`后在请求超时时引发。如果你的应用程序必须确保用户具有已删除从组在将消息发送到组之前，你可以添加`await`Groups.Remove，然后 catch 之前`TaskCanceledException`可能引发的异常。

<a id="call"></a>

## <a name="calling-members-of-a-group"></a>调用组的成员

下面的示例中所示，你可以将消息发送到的所有组的成员或仅指定的组的成员。

- **所有**连接客户端在指定的组。 

    [!code-css[Main](working-with-groups/samples/sample3.css)]
- 所有连接指定的组中的客户端**除外指定客户端**，标识由连接 id。 

    [!code-csharp[Main](working-with-groups/samples/sample4.cs)]
- 所有连接指定的组中的客户端**除外调用的客户端**。 

    [!code-css[Main](working-with-groups/samples/sample5.css)]

<a id="storedatabase"></a>

## <a name="storing-group-membership-in-a-database"></a>在数据库中存储组成员身份

下面的示例演示如何保留在数据库中的组和用户信息。 你可以使用任何数据访问技术;但是，下面的示例演示如何定义使用 Entity Framework 模型。 这些实体模型对应于数据库表和字段。 您的数据结构无法千差万别，具体取决于你的应用程序的要求。 该示例包括一个名为类`ConversationRoom`这将是唯一的该应用程序，用户加入有关不同主题，例如体育或园对话。 此示例还包括为所连接的类。 连接类不是绝对必需用于跟踪组成员身份，但经常是可靠的解决方案，跟踪用户的一部分。

[!code-csharp[Main](working-with-groups/samples/sample6.cs)]

然后，在中心，你可以从数据库中检索组和用户信息并手动将用户添加到相应的组。 该示例不包括用于跟踪的用户连接的代码。 在此示例中，`await`关键字不应用之前`Groups.Add`因为到组的成员未立即发送一条消息。 如果你想要添加新成员后，立即将消息发送到组的所有成员，你将想要应用`await`关键字，若要确保异步操作已完成。

[!code-csharp[Main](working-with-groups/samples/sample7.cs)]

<a id="storeazuretable"></a>

## <a name="storing-group-membership-in-azure-table-storage"></a>在 Azure 表存储中存储组成员身份

使用 Azure 表存储来存储组和用户信息是类似于使用数据库。 下面的示例演示将存储的用户名和组名称的表实体。

[!code-csharp[Main](working-with-groups/samples/sample8.cs)]

在中心，您将检索分配的组，当用户连接。

[!code-csharp[Main](working-with-groups/samples/sample9.cs)]

<a id="verify"></a>

## <a name="verifying-group-membership-when-reconnecting"></a>当重新连接时验证组成员身份

默认情况下，SignalR 会自动重新分配用户到相应的组时从临时中断，如连接时删除并重新建立连接超时之前重新连接。用户的组信息时，会传入令牌重新连接，并且该令牌会验证服务器上。 有关重新加入到组的用户的验证过程的信息，请参阅[当重新连接时重新加入组](../security/introduction-to-security.md#rejoingroup)。

一般情况下，应使用自动重新加入上的组重新连接的默认行为。 SignalR 组并不作为限制对敏感数据的访问的安全机制。 但是，如果你的应用程序必须仔细检查用户的组成员身份，当重新连接时，你可以重写默认行为。 更改默认行为可以添加了负担到你的数据库因为必须针对每个重新连接而不是仅当用户连接检索到用户的组成员身份。

如果你必须在验证组成员身份重新连接，请创建一个新的中心管道模块返回的已分配的组列表，如下所示。

[!code-csharp[Main](working-with-groups/samples/sample10.cs)]

然后，将该模块添加到中心管道，作为下面突出显示。

[!code-csharp[Main](working-with-groups/samples/sample11.cs?highlight=4)]
