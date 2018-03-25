---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
title: "第 6 部分： 创建产品和顺序控制器 |Microsoft 文档"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 91ee29ee-0689-40ee-914a-e7dd733b6622
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
msc.type: authoredcontent
ms.openlocfilehash: 3b33543f02479b97112a63eb3879967ae31ccfb3
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="part-6-creating-product-and-order-controllers"></a>第 6 部分： 创建产品和顺序控制器
====================
通过[Mike Wasson](https://github.com/MikeWasson)

[下载已完成的项目](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-a-products-controller"></a>添加产品控制器

管理控制器是具有管理员特权权限的用户。 客户，另一方面，可以查看产品但不能创建、 更新或删除它们。

我们可以轻松地限制访问的 Post、 Put 和 Delete 的方法，同时 Get 方法。 但讨论的产品返回的数据：

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample1.json?highlight=1)]

`ActualCost`属性不应为对客户可见 ！ 解决方法是定义*数据传输对象*(DTO)，包括应对客户可见的属性的子集。 我们将向项目中使用 LINQ`Product`实例到`ProductDTO`实例。

添加一个名为类`ProductDTO`到 Models 文件夹。

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample2.cs)]

现在添加控制器。 在解决方案资源管理器，右键单击 Controllers 文件夹。 选择**添加**，然后选择**控制器**。 在**添加控制器**对话框中，控制器&quot;ProductsController&quot;。 下**模板**，选择**空 API 控制器**。

![](using-web-api-with-entity-framework-part-6/_static/image1.png)

将源文件中的所有内容替换为以下代码：

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample3.cs)]

控制器仍使用`OrdersContext`查询数据库。 但而不是返回`Product`实例直接，我们调用`MapProducts`投影到它们`ProductDTO`实例：

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample4.cs?highlight=1)]

`MapProducts`方法返回**IQueryable**，因此我们可以编写结果与其他查询参数。 你可以看到此中`GetProduct`方法，以便将添加**其中**对查询子句：

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample5.cs?highlight=2)]

## <a name="add-an-orders-controller"></a>添加 Orders 控制器

接下来，添加允许用户创建和查看订单的控制器。

我们将开始另一个 DTO。 在解决方案资源管理器，右键单击 Models 文件夹，并添加一个名为类`OrderDTO`使用以下实现：

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample6.cs)]

现在添加控制器。 在解决方案资源管理器，右键单击 Controllers 文件夹。 选择**添加**，然后选择**控制器**。 在**添加控制器**对话框中，设置以下选项：

- 下**控制器名称**，输入"OrdersController"。
- 下**模板**，选择"读/写操作，使用 Entity Framework 的 API 控制器"。
- 下**模型类**，选择&quot;顺序 (ProductStore.Models)&quot;。
- 下**数据上下文类**，选择&quot;OrdersContext (ProductStore.Models)&quot;。

![](using-web-api-with-entity-framework-part-6/_static/image2.png)

单击 **“添加”**。 这将添加一个名为 OrdersController.cs 文件。 接下来，我们需要修改控制器的默认实现。

首先，删除`PutOrder`和`DeleteOrder`方法。 此示例中，客户无法修改或删除现有的订单。 在实际应用中，您将需要大量的后端逻辑来处理这些情况。 （例如，订单已发货？）

更改`GetOrders`方法来返回仅属于用户的订单：

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample7.cs)]

更改`GetOrder`方法，如下所示：

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample8.cs)]

下面是我们对方法进行了更改：

- 返回值是`OrderDTO`实例，而不是`Order`。
- 当我们查询数据库中的顺序时，我们使用[DbQuery.Include](https://msdn.microsoft.com/library/gg696395)方法以提取相关`OrderDetail`和`Product`实体。
- 我们使用投影来平展结果。

HTTP 响应将包含产品与数量的数组：

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample9.json)]

此格式是客户端使用比原始对象图，其中包含嵌套的实体 （顺序、 详细信息，和产品） 更容易。

要考虑将它的最后一个方法`PostOrder`。 现在，此方法采用`Order`实例。 请考虑会发生什么情况，但如果客户端发送请求正文如下：

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample10.json)]

这是一个结构良好的顺序，并且实体框架将不假思索地将其插入到数据库。 但是，它包含以前不存在 Product 实体。 客户端只在我们的数据库中创建新产品 ！ 如果用户知道考拉关闭的顺序，这将是向顺序 fullfilment 部门，意外情况发生。 这说明了什么是，请真正谨慎接受 POST 或 PUT 请求中的数据。

若要避免此问题，更改`PostOrder`方法才能`OrderDTO`实例。 使用`OrderDTO`创建`Order`。

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample11.cs)]

请注意，我们使用`ProductID`和`Quantity`属性和我们忽略客户端发送的产品名称或价格任何值。 如果产品 ID 不是有效的它将违反外的键约束在数据库中，并插入将失败，它也应如此。

下面是完整`PostOrder`方法：

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample12.cs)]

最后，添加**Authorize**到控制器属性：

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample13.cs)]

现在只允许注册的用户可以创建或查看订单。

>[!div class="step-by-step"]
[上一页](using-web-api-with-entity-framework-part-5.md)
[下一页](using-web-api-with-entity-framework-part-7.md)
