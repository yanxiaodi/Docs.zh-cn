---
uid: signalr/overview/older-versions/mapping-users-to-connections
title: "将 SignalR 用户映射至在 SignalR 连接 1.x |Microsoft 文档"
author: pfletcher
description: "本主题说明如何保留有关用户和他们的连接信息。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: ebbc93a8-e6c4-4122-8e0d-3aa42293c747
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: 896bf4142ce090e39ed5697ff053cd56728318ed
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="mapping-signalr-users-to-connections-in-signalr-1x"></a>将 SignalR 用户映射至在 SignalR 连接 1.x
====================
通过[Patrick Fletcher](https://github.com/pfletcher)， [Tom FitzMacken](https://github.com/tfitzmac)

> 本主题说明如何保留有关用户和他们的连接信息。


## <a name="introduction"></a>介绍

每个客户端将连接到中心将传递唯一连接 id。你可以检索中的此值`Context.ConnectionId`中心上下文的属性。 如果你的应用程序需要以将用户映射到的连接 id 并将保留该映射，你可以使用以下项之一：

- [内存中存储](#inmemory)，如字典
- [每个用户的的 SignalR 组](#groups)
- [永久、 外部存储](#database)，如数据库表或 Azure 表存储

每个这些实现是此主题中所示。 你使用`OnConnected`， `OnDisconnected`，和`OnReconnected`方法`Hub`类来跟踪用户连接状态。

你的应用程序的最佳方法取决于：

- 承载你的应用程序的 web 服务器的数目。
- 是否需要获取当前连接的用户的列表。
- 是否需要应用程序或服务器重新启动时持久保存组和用户信息。
- 调用外部服务器的滞后时间是否出现问题。

下表显示了哪种方法适用于这些注意事项。

|  | 多个服务器 | 获取当前连接的用户的列表 | 将重新启动后的信息保留 | 获得最佳性能 |
| --- | --- | --- | --- | --- |
| 内存中 |  | ![](mapping-users-to-connections/_static/image1.png) |  | ![](mapping-users-to-connections/_static/image2.png) |
| 单用户组 | ![](mapping-users-to-connections/_static/image3.png) |  |  | ![](mapping-users-to-connections/_static/image4.png) |
| 永久外部 | ![](mapping-users-to-connections/_static/image5.png) | ![](mapping-users-to-connections/_static/image6.png) | ![](mapping-users-to-connections/_static/image7.png) |  |

<a id="inmemory"></a>

## <a name="in-memory-storage"></a>内存中存储

下面的示例演示如何保留一个字典，其中存储在内存中的连接和用户信息。 字典使用`HashSet`存储的连接 id。在任何时候用户可能具有多个连接到 SignalR 应用程序。 例如，通过多个设备或多个浏览器选项卡连接的用户将具有多个连接 id。

如果应用程序关闭时，所有信息都将丢失，但它也将进行重新填充，如用户重新建立连接。 如果您的环境包括多个 web 服务器，因为每个服务器都可以单独的连接集合，则内存中存储无效。

第一个示例演示管理连接到的用户的映射的类。 HashSet 的键将为用户的名称。

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

下一个示例演示如何使用连接映射类从一个中心。 类的实例存储在变量名`_connections`。

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a>单用户组

可以为每个用户创建组，然后将消息发送到该组，当你想要访问只允许该用户。 每个组的名称是用户的名称。 如果用户具有多个连接，则将每个连接 id 添加到用户的组。

你不应手动删除用户从组用户断开连接时。 由 SignalR 框架自动执行此操作。

下面的示例演示如何实现单用户组。

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a>永久、 外部存储

本主题演示如何使用数据库或 Azure 表存储来存储连接信息。 当你有多个 web 服务器，因为每个 web 服务器可以与相同的数据存储库进行交互时，此方法有效。 如果你的 web 服务器停止工作或应用程序重新启动，`OnDisconnected`不调用方法。 因此，很可能你的数据存储库将具有的连接 id，将不再有效的记录。 若要清理这些孤立的记录，你可能希望使与你的应用程序相关的时间范围之外创建任何连接。 此部分中的示例包括用于跟踪，创建连接时的一个值，但不是显示如何清除旧的记录，因为你可能想要执行此操作作为后台进程。

### <a name="database"></a>数据库

下面的示例演示如何保留在数据库中的连接和用户信息。 你可以使用任何数据访问技术;但是，下面的示例演示如何定义使用 Entity Framework 模型。 这些实体模型对应于数据库表和字段。 您的数据结构无法千差万别，具体取决于你的应用程序的要求。

第一个示例演示如何定义可以与多个连接实体相关联的用户实体。

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

然后，从中心，你可以跟踪每个连接的状态与下面显示的代码。

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

### <a name="azure-table-storage"></a>Azure 表存储

下面的 Azure 表存储示例是类似于数据库的示例。 它不包括所有需要若要开始使用 Azure 表存储服务的信息。 有关信息，请参阅[如何通过.NET 使用表存储](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/)。

下面的示例演示用于存储连接信息的表实体。 它按用户名称，将数据分区，并由连接 id 标识每个实体，以便用户可以在任何时间有多个连接。

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

在中心，你可以跟踪每个用户的连接的状态。

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]
