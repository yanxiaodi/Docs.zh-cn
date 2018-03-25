---
uid: signalr/overview/guide-to-the-api/mapping-users-to-connections
title: "将 SignalR 用户映射至连接 |Microsoft 文档"
author: tfitzmac
description: "本主题说明如何保留有关用户和他们的连接信息。 Patrick Fletcher 协助编写本主题。 本主题中使用的软件版本..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/30/2014
ms.topic: article
ms.assetid: f80c08b1-3f1f-432c-980c-c7b6edeb31b1
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: c4f95a3b65c57dd7cb7c5c7f1ee09daa17fa9616
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="mapping-signalr-users-to-connections"></a>将 SignalR 用户映射至连接
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> 本主题说明如何保留有关用户和他们的连接信息。
> 
> Patrick Fletcher 协助编写本主题。
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


## <a name="introduction"></a>介绍

每个客户端将连接到中心将传递唯一连接 id。你可以检索中的此值`Context.ConnectionId`中心上下文的属性。 如果你的应用程序需要以将用户映射到的连接 id 并将保留该映射，你可以使用以下项之一：

- [用户 ID 提供程序 (SignalR 2)](#IUserIdProvider)
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
| 用户标识提供程序 | ![](mapping-users-to-connections/_static/image1.png) |  |  | ![](mapping-users-to-connections/_static/image2.png) |
| 内存中 |  | ![](mapping-users-to-connections/_static/image3.png) |  | ![](mapping-users-to-connections/_static/image4.png) |
| 单用户组 | ![](mapping-users-to-connections/_static/image5.png) |  |  | ![](mapping-users-to-connections/_static/image6.png) |
| 永久外部 | ![](mapping-users-to-connections/_static/image7.png) | ![](mapping-users-to-connections/_static/image8.png) | ![](mapping-users-to-connections/_static/image9.png) |  |

<a id="IUserIdProvider"></a>

## <a name="iuserid-provider"></a>IUserID 提供程序

此功能允许用户指定用户 Id 是基于通过新接口 IUserIdProvider IRequest。

**IUserIdProvider**

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

默认情况下，将使用用户的实现`IPrincipal.Identity.Name`作为用户名。 若要更改此设置，注册的实现`IUserIdProvider`与全局主机应用程序启动时：

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

从中心中你将能够将消息发送到这些用户通过以下 API:

**向特定用户发送消息**

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs?highlight=5)]

<a id="inmemory"></a>

## <a name="in-memory-storage"></a>内存中存储

下面的示例演示如何保留一个字典，其中存储在内存中的连接和用户信息。 字典使用`HashSet`存储的连接 id。在任何时候用户可能具有多个连接到 SignalR 应用程序。 例如，通过多个设备或多个浏览器选项卡连接的用户将具有多个连接 id。

如果应用程序关闭时，所有信息都将丢失，但它也将进行重新填充，如用户重新建立连接。 如果您的环境包括多个 web 服务器，因为每个服务器都可以单独的连接集合，则内存中存储无效。

第一个示例演示管理连接到的用户的映射的类。 HashSet 的键将为用户的名称。

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

下一个示例演示如何使用连接映射类从一个中心。 类的实例存储在变量名`_connections`。

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a>单用户组

可以为每个用户创建组，然后将消息发送到该组，当你想要访问只允许该用户。 每个组的名称是用户的名称。 如果用户具有多个连接，则将每个连接 id 添加到用户的组。

你不应手动删除用户从组用户断开连接时。 由 SignalR 框架自动执行此操作。

下面的示例演示如何实现单用户组。

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a>永久、 外部存储

本主题演示如何使用数据库或 Azure 表存储来存储连接信息。 当你有多个 web 服务器，因为每个 web 服务器可以与相同的数据存储库进行交互时，此方法有效。 如果你的 web 服务器停止工作或应用程序重新启动，`OnDisconnected`不调用方法。 因此，很可能你的数据存储库将具有的连接 id，将不再有效的记录。 若要清理这些孤立的记录，你可能希望使与你的应用程序相关的时间范围之外创建任何连接。 此部分中的示例包括用于跟踪，创建连接时的一个值，但不是显示如何清除旧的记录，因为你可能想要执行此操作作为后台进程。

### <a name="database"></a>数据库

下面的示例演示如何保留在数据库中的连接和用户信息。 你可以使用任何数据访问技术;但是，下面的示例演示如何定义使用 Entity Framework 模型。 这些实体模型对应于数据库表和字段。 您的数据结构无法千差万别，具体取决于你的应用程序的要求。

第一个示例演示如何定义可以与多个连接实体相关联的用户实体。

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]

然后，从中心，你可以跟踪每个连接的状态与下面显示的代码。

[!code-csharp[Main](mapping-users-to-connections/samples/sample8.cs)]

<a id="azure"></a>
### <a name="azure-table-storage"></a>Azure 表存储

下面的 Azure 表存储示例是类似于数据库的示例。 它不包括所有需要若要开始使用 Azure 表存储服务的信息。 有关信息，请参阅[如何通过.NET 使用表存储](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/)。

下面的示例演示用于存储连接信息的表实体。 它按用户名称，将数据分区，并由连接 id 标识每个实体，以便用户可以在任何时间有多个连接。

[!code-csharp[Main](mapping-users-to-connections/samples/sample9.cs)]

在中心，你可以跟踪每个用户的连接的状态。

[!code-csharp[Main](mapping-users-to-connections/samples/sample10.cs)]
