---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
title: "第 2 部分： 创建域模型 |Microsoft 文档"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2012
ms.topic: article
ms.assetid: fe3ef85f-bdc6-4e10-9768-25aa565c01d0
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
msc.type: authoredcontent
ms.openlocfilehash: a573b47d27767dc78d557cd2b6c73714eb9e94f4
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="part-2-creating-the-domain-models"></a>第 2 部分： 创建域模型
====================
通过[Mike Wasson](https://github.com/MikeWasson)

[下载已完成的项目](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-models"></a>添加模型

有三种方法方法实体框架：

- 数据库优先： 对于数据库，启动和实体框架生成的代码。
- 模型优先： 开头 visual 模型和实体框架生成的数据库和代码。
- 代码优先： 与代码中，启动，实体框架生成数据库。

我们使用代码为先的方法，因此我们首先我们域对象定义为 POCOs （纯旧式 CLR 对象）。 使用代码优先方法时，域对象不需要任何额外的代码，以支持数据库层，如事务或持久性。 (具体而言，它们不需要从继承[EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx)类。)你仍然可以使用数据注释控制实体框架如何创建数据库架构。

因为 POCOs 不执行任何额外的属性，用于描述[的数据库状态](https://msdn.microsoft.com/library/system.data.entitystate.aspx)，它们可以轻松地序列化为 JSON 或 XML。 但是，，并不意味着你始终应公开实体框架模型直接向客户端，正如我们看到在教程的后面。

我们将创建以下 POCOs:

- 产品
- 订单
- OrderDetail

若要创建每个类，右键单击解决方案资源管理器中的 Models 文件夹。 从上下文菜单中，选择**添加**，然后选择**类。**

![](using-web-api-with-entity-framework-part-2/_static/image1.png)

添加`Product`类具有以下实现：

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample1.cs)]

按照约定，实体框架使用`Id`为主键的属性并将其映射到数据库表中的标识列。 当你创建一个新`Product`实例，不会设置的值`Id`，因为数据库将生成值。

**ScaffoldColumn**属性告知 ASP.NET MVC 要跳过`Id`属性生成的编辑器窗体时。 **所需**属性用来验证模型。 它指定`Name`属性必须为非空字符串。

添加`Order`类：

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample2.cs)]

添加`OrderDetail`类：

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample3.cs)]

## <a name="foreign-key-relations"></a>外键关系

订单包含许多订单详细信息，并且每个订单详细信息指向单个产品。 来表示这些关系，`OrderDetail`类定义属性名为`OrderId`和`ProductId`。 实体框架将推断这些属性表示外键，并会将外键约束添加到数据库。

![](using-web-api-with-entity-framework-part-2/_static/image2.png)

`Order`和`OrderDetail`类还包括"导航"属性，其中包含对相关对象的引用。 给定订单，你可以通过导航到订单中产品以下导航属性。

现在编译该项目。 实体框架使用反射来发现的属性的模型，因此它需要编译的程序集，若要创建数据库架构。

## <a name="configure-the-media-type-formatters"></a>配置的媒体类型格式化程序

A[媒体类型格式化程序](../../formats-and-model-binding/media-formatters.md)是当 Web API 写入 HTTP 响应正文将你的数据序列化的对象。 内置的格式化程序支持的 JSON 和 XML 输出。 默认情况下，这两个这些格式化程序序列化的所有对象的值。

如果对象图包含循环引用，则按值序列化会产生问题。 这是完全与用例`Order`和`OrderDetail`类，因为每个保存到其他的引用。 格式化程序将按照编写值，每个对象的引用，并在兜圈子转。 因此，我们需要更改默认行为。

在解决方案资源管理器，展开应用程序\_启动文件夹并打开名为 WebApiConfig.cs 文件。 将下面的代码添加到 `WebApiConfig` 类中:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample4.cs?highlight=11)]

此代码会设置 JSON 格式化程序来保留对象引用，并从管道中完全移除 XML 格式化程序。 （你可以配置 XML 格式化程序来保留对象引用，但该稍有更多工作，并且我们只需要为此应用程序的 JSON。 有关详细信息，请参阅[处理循环对象引用](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references)。)

>[!div class="step-by-step"]
[上一页](using-web-api-with-entity-framework-part-1.md)
[下一页](using-web-api-with-entity-framework-part-3.md)
