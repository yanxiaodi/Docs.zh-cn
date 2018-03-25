---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
title: "从.NET 客户端 (C#) 调用 OData 服务 |Microsoft 文档"
author: MikeWasson
description: "本教程演示如何从 C# 客户端应用程序调用 OData 服务。 在教程的 Visual Studio 2013 （适用于 Visual s.中使用的软件版本"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2014
ms.topic: article
ms.assetid: 6f448917-ad23-4dcc-9789-897fad74051b
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 497102cfa98680f2156a56ff9e36d84b7c820020
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="calling-an-odata-service-from-a-net-client-c"></a>从.NET 客户端 (C#) 调用 OData 服务
====================
通过[Mike Wasson](https://github.com/MikeWasson)

[下载已完成的项目](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> 本教程演示如何从 C# 客户端应用程序调用 OData 服务。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教程中使用的软件版本
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) （适用于 Visual Studio 2012）
> - [WCF Data Services 客户端库](https://msdn.microsoft.com/library/cc668772.aspx)
> - Web API 2。 （使用 Web API 2 OData 服务的示例进行构建，但客户端应用程序不依赖于 Web API）。


在本教程中，我将演练创建的客户端应用程序调用 OData 服务。 OData 服务公开以下实体：

- `Product`
- `Supplier`
- `ProductRating`

![](calling-an-odata-service-from-a-net-client/_static/image1.png)

以下文章介绍如何在 Web API 中实现的 OData 服务。 （不需要来阅读它们以了解本教程中，但是工作。）

- [在 Web API 2 中创建 OData 终结点](creating-an-odata-endpoint.md)
- [Web API 2 中的 OData 实体关系](working-with-entity-relations.md)
- [Web API 2 中的 OData 操作](odata-actions.md)

## <a name="generate-the-service-proxy"></a>生成的服务代理

第一步是生成服务代理。 服务代理是一个.NET 类，定义用于访问 OData 服务的方法。 代理将转换为 HTTP 请求的方法调用。

![](calling-an-odata-service-from-a-net-client/_static/image2.png)

首先在 Visual Studio 中打开 OData 服务项目。 按 CTRL + F5 来在 IIS Express 中本地运行服务。 记下的本地地址，包括 Visual Studio 将分配的端口号。 在创建代理时，你将需要此地址。

接下来，打开 Visual Studio 的另一个实例，并创建一个控制台应用程序项目。 控制台应用程序将是我们 OData 客户端应用程序。 （你还可以添加项目到与该服务位于同一解决方案。）

> [!NOTE]
> 剩余的步骤，请参阅控制台项目。


在解决方案资源管理器，右键单击**引用**和选择**添加服务引用**。

![](calling-an-odata-service-from-a-net-client/_static/image3.png)

在**添加服务引用**对话框中，键入的 OData 服务的地址：

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample1.cmd)]

其中*端口*是端口号。

[![](calling-an-odata-service-from-a-net-client/_static/image5.png)](calling-an-odata-service-from-a-net-client/_static/image4.png)

有关**Namespace**，键入"ProductService"。 此选项定义的代理类的命名空间。

单击 **“转到”**。 Visual Studio 中读取要发现服务中的实体的 OData 元数据文档。

[![](calling-an-odata-service-from-a-net-client/_static/image7.png)](calling-an-odata-service-from-a-net-client/_static/image6.png)

单击**确定**将代理类添加到你的项目。

![](calling-an-odata-service-from-a-net-client/_static/image8.png)

## <a name="create-an-instance-of-the-service-proxy-class"></a>创建服务代理类的实例

在你`Main`方法，创建代理类的新实例，如下所示：

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample2.cs)]

同样，使用你的服务正在其中运行的实际端口号。 在部署你的服务时，你将使用实时服务的 URI。 你不需要更新代理。

以下代码添加的事件处理程序将打印到控制台窗口的请求 Uri。 此步骤不是必需的但想要查看每个查询的 Uri。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample3.cs)]

## <a name="query-the-service"></a>查询服务

下面的代码从 OData 服务获取产品的列表。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample4.cs)]

请注意，你无需编写任何代码来发送 HTTP 请求或分析的响应。 代理类这自动执行时枚举`Container.Products`中的集合**foreach**循环。

运行应用程序时，输出应显示如下：

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample5.cmd)]

若要获取按 ID 实体，请使用`where`子句。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample6.cs)]

本主题的其余部分，我不会显示整个`Main`函数，只需调用该服务的代码。

## <a name="apply-query-options"></a>应用查询选项

OData 定义[查询选项](../supporting-odata-query-options.md)可以用于筛选器、 排序、 页数据和等。 在服务代理中，可以通过使用各种 LINQ 表达式来应用这些选项。

在此部分中，我将显示简短示例。 有关详细信息，请参阅主题[LINQ 注意事项 （WCF 数据服务）](https://msdn.microsoft.com/library/ee622463.aspx) MSDN 上。

### <a name="filtering-filter"></a>筛选 ($filter)

若要筛选，请使用`where`子句。 下面的示例筛选按产品类别。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample7.cs)]

此代码对应于以下的 OData 查询。

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample8.cmd)]

请注意，代理将转换`where`到 OData 子句`$filter`表达式。

### <a name="sorting-orderby"></a>排序 ($orderby)

若要排序，请使用`orderby`子句。 下面的示例按定价从高到低对进行排序。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample9.cs)]

下面是相应的 OData 请求。

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample10.cmd)]

### <a name="client-side-paging-skip-and-top"></a>客户端分页 （$skip 和 $top）

大型实体集的情况下，客户端可能会想要限制结果数。 例如，客户端可能一次显示 10 个条目。 这称为*客户端分页*。 (此外还有[服务器端分页](../supporting-odata-query-options.md#server-paging)，其中服务器限制结果数。)若要执行客户端分页，使用 LINQ**跳过**和**采取**方法。 下面的示例跳过前 40 结果，并采用接下来的 10。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample11.cs)]

下面是相应的 OData 请求：

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample12.cmd)]

### <a name="select-select-and-expand-expand"></a>Select ($select) 和 Expand ($expand)

若要包含相关的实体，使用`DataServiceQuery<t>.Expand`方法。 例如，若要包括`Supplier`每个`Product`:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample13.cs)]

下面是相应的 OData 请求：

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample14.cmd)]

若要更改形状的响应，请使用 LINQ**选择**子句。 下面的示例获取只是每个产品，且没有其他属性的名称。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample15.cs)]

下面是相应的 OData 请求：

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample16.cmd)]

Select 子句可以包括相关的实体。 在这种情况下，不要调用**展开**; 代理会自动在这种情况下包括扩展。 下面的示例获取的名称和每个产品的供应商。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample17.cs)]

下面是相应的 OData 请求。 请注意，它包括**$expand**选项。

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample18.cmd)]

有关更多信息 $select 和 $展开，请参阅[使用 $select，$expand、 和 Web API 2 中的 $value](../using-select-expand-and-value.md)。

## <a name="add-a-new-entity"></a>添加新实体

若要将新实体添加到实体集，调用`AddToEntitySet`，其中*EntitySet*是实体集的名称。 例如，`AddToProducts`添加一个新`Product`到`Products`实体集。 当生成代理，则 WCF 数据服务将自动创建这些强类型**AddTo**方法。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample19.cs)]

若要添加两个实体之间的链接，使用**AddLink**和**SetLink**方法。 下面的代码将添加一个新的供应商和新产品，然后创建它们之间的链接。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample20.cs)]

使用**AddLink**时的导航属性是集合。 在此示例中，我们将添加到产品`Products`供应商上的集合。

使用**SetLink**的导航属性时单个实体。 在此示例中，我们正在设置`Supplier`产品上的属性。

## <a name="update--patch"></a>更新/修补程序

若要更新实体，调用**UpdateObject**方法。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample21.cs)]

在调用时执行更新**SaveChanges**。 默认情况下，WCF 发送 HTTP MERGE 请求。 **PatchOnUpdate**选项告知 WCF 改为发送 HTTP 修补程序。

> [!NOTE]
> 为什么与合并修补程序？ 原始的 HTTP 1.1 规范 ([RCF 2616](http://tools.ietf.org/html/rfc2616)) 未定义任何具有"部分更新"语义的 HTTP 方法。 若要支持部分更新，OData 规范定义 MERGE 方法。 在 2010 中， [RFC 5789](http://tools.ietf.org/html/rfc5789)定义部分更新的修补程序方法。 你可以阅读一些的历史记录中这[博客文章](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx)WCF 数据服务博客上。 现在，修补程序通过合并是首选。 创建的 Web API 基架 OData 控制器支持这两种方法。


如果你想要替换整个实体 （PUT 语义），指定**ReplaceOnUpdate**选项。 这会导致 WCF 发送 HTTP PUT 请求。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample22.cs)]

## <a name="delete-an-entity"></a>删除实体

若要删除实体，调用**DeleteObject**。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample23.cs)]

## <a name="invoke-an-odata-action"></a>调用 OData 操作

在 OData 中，[操作](odata-actions.md)是一种方法中添加不很容易定义为对实体的 CRUD 操作的服务器端行为。

尽管 OData 元数据文档描述的操作，则代理类不为它们创建强类型的任何方法。 你仍可以通过使用泛型调用 OData 操作**执行**方法。 但是，你将需要知道的参数和返回值的数据类型。

例如，`RateProduct`操作将名为"评级"的类型参数`Int32`并返回`double`。 下面的代码演示如何调用此操作。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample24.cs)]

有关详细信息，请参阅[调用服务操作和操作](https://msdn.microsoft.com/library/hh230677.aspx)。

一种方法是扩展**容器**类以提供将调用该操作的强类型化的方法：

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample25.cs)]
