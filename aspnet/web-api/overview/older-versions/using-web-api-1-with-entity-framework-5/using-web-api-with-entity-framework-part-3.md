---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
title: "第 3 部分： 创建一个管理控制器 |Microsoft 文档"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 6b9ae3c4-0274-4170-a1bb-9df9c546b2a9
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
msc.type: authoredcontent
ms.openlocfilehash: 6fadfb6e96ae287fc5f81516b7535e03853c7e6a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="part-3-creating-an-admin-controller"></a>第 3 部分： 创建一个管理控制器
====================
通过[Mike Wasson](https://github.com/MikeWasson)

[下载已完成的项目](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-controller"></a>添加管理控制器

在本部分中，我们将添加一个 Web API 控制器支持 CRUD （创建、 读取、 更新和删除） 的产品的操作。 控制器将使用实体框架与数据库层进行通信。 只有管理员将能够使用此控制器。 客户将通过另一个控制器访问产品。

在解决方案资源管理器，右键单击 Controllers 文件夹。 选择**添加**然后**控制器**。

![](using-web-api-with-entity-framework-part-3/_static/image1.png)

在**添加控制器**对话框中，控制器`AdminController`。 下**模板**，选择&quot;读/写操作，使用 Entity Framework 的 API 控制器&quot;。 下**模型类**，选择"Product (ProductStore.Models)"。 下**数据上下文**，选择"&lt;新建数据上下文&gt;"。

![](using-web-api-with-entity-framework-part-3/_static/image2.png)

> [!NOTE]
> 如果**模型类**下拉列表不显示任何模型类，确保在编译该项目。 实体框架使用反射，因此它需要编译的程序集。


选择"&lt;新建数据上下文&gt;"将打开**新建数据上下文**对话框。 命名的数据上下文`ProductStore.Models.OrdersContext`。

![](using-web-api-with-entity-framework-part-3/_static/image3.png)

单击**确定**关闭**新建数据上下文**对话框。 在**添加控制器**对话框中，单击**添加**。

下面是什么已将其添加到项目：

- 一个名为类`OrdersContext`派生自**DbContext**。 此类提供的 POCO 模型和数据库之间起来的纽带。
- 名为的 Web API 控制器`AdminController`。 此控制器上支持的 CRUD 操作`Product`实例。 它使用`OrdersContext`类与实体框架进行通信。
- Web.config 文件中的新数据库连接字符串。

![](using-web-api-with-entity-framework-part-3/_static/image4.png)

打开 OrdersContext.cs 文件。 请注意，构造函数指定的数据库连接字符串的名称。 此名称引用已添加到 Web.config 连接字符串。

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample1.cs)]

向 `OrdersContext` 类添加以下属性：

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample2.cs)]

A **DbSet**表示一组可查询的实体。 下面是有关的完整列表`OrdersContext`类：

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample3.cs)]

`AdminController`类定义实现基本的 CRUD 功能的五个方法。 每个方法都对应一个 URI，客户端可以调用：

| 控制器方法 | 描述 | URI | HTTP 方法 |
| --- | --- | --- | --- |
| GetProducts | 获取所有产品。 | api/产品 | GET |
| GetProduct | 找到的产品的 id。 | api/产品/*id* | GET |
| PutProduct | 更新产品。 | api/产品/*id* | PUT |
| PostProduct | 创建新产品。 | api/产品 | 发布 |
| DeleteProduct | 删除产品。 | api/产品/*id* | DELETE |

每个方法调入`OrdersContext`查询数据库。 修改集合 （PUT、 POST 和 DELETE） 的方法调用`db.SaveChanges`以保留对数据库的更改。 控制器与每个 HTTP 请求创建，且然后释放，因此需要来方法返回之前保留更改。

## <a name="add-a-database-initializer"></a>添加数据库初始值设定项

实体框架具有一个不错的功能，你在启动时，在数据库中填充和每当模型更改时自动重新创建数据库。 此功能可在开发期间，因为始终有一些测试数据，即使更改模型。

在解决方案资源管理器，右键单击 Models 文件夹，并创建一个名为的新类`OrdersContextInitializer`。 粘贴以下实现：

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample4.cs)]

通过继承**DropCreateDatabaseIfModelChanges**类，我们将指示实体框架可以删除数据库，只要我们修改模型类。 当实体框架创建 （或重新创建） 数据库时，它将调用**种子**方法以填充表。 我们使用**种子**方法来添加某些示例产品 + 示例订单。

此功能非常适合测试，但不使用**DropCreateDatabaseIfModelChanges**类在生产环境、 中，因为如果有人更改模型类，则可能会丢失数据。

接下来，打开 Global.asax，然后添加以下代码到**应用程序\_启动**方法：

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample5.cs)]

## <a name="send-a-request-to-the-controller"></a>将请求发送到的控制器

此时，我们尚未写入任何客户端代码，但你可以调用的 web API 使用 web 浏览器或 HTTP 调试工具如[Fiddler](http://www.fiddler2.com/fiddler2/)。 在 Visual Studio 中，按 F5 启动调试。 Web 浏览器将打开到`http://localhost:*portnum*/`，其中*portnum*是某些端口号。

发送到 HTTP 请求"`http://localhost:*portnum*/api/admin`。 第一个请求可能慢，无法完成，因为 Entify Framework 需要创建和植入到数据库。 响应应类似于以下内容：

[!code-console[Main](using-web-api-with-entity-framework-part-3/samples/sample6.cmd)]

>[!div class="step-by-step"]
[上一页](using-web-api-with-entity-framework-part-2.md)
[下一页](using-web-api-with-entity-framework-part-4.md)
