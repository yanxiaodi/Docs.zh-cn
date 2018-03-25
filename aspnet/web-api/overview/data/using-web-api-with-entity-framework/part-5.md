---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-5
title: "创建数据传输对象 (Dto) |Microsoft 文档"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 0fd07176-b74b-48f0-9fac-0f02e3ffa213
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-5
msc.type: authoredcontent
ms.openlocfilehash: e179dcd52200734a45c84b3c7c3069a4727904c1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="create-data-transfer-objects-dtos"></a>创建数据传输对象 (Dto)
====================
通过[Mike Wasson](https://github.com/MikeWasson)

[下载已完成的项目](https://github.com/MikeWasson/BookService)

右我们 web API 现在，公开给客户端的数据库实体。 客户端接收直接映射到数据库表的数据。 但是，，并不总是一个好主意。 有时你想要更改向客户端发送的数据的形状。 例如，你可能希望：

- 删除循环引用 （请参阅上一节）。
- 隐藏客户端不应查看的特定属性。
- 为了减少负载大小省略某些属性。
- 平展包含嵌套的对象，以使它们更方便的客户端的对象图。
- 避免"过多发布"漏洞。 (请参阅[模型验证](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md)过度发布的讨论。)
- 将你从你的数据库层的服务层中分离出来。

若要实现此目的，可以定义*数据传输对象*(DTO)。 DTO 是定义如何将通过网络发送数据的对象。 我们来看，如何使用 Book 实体。 在 Models 文件夹中，添加两个 DTO 类：

[!code-csharp[Main](part-5/samples/sample1.cs)]

`BookDetailDTO`类包含所有从簿模型，只不过属性`AuthorName`是一个字符串，将保存作者姓名。 `BookDTO`类包含属性的子集`BookDetailDTO`。

接下来，替换中的两个 GET 方法`BooksController`类，与返回 Dto 的版本。 我们将使用 LINQ**选择**语句从 Book 实体将转换为 Dto。

[!code-csharp[Main](part-5/samples/sample2.cs)]

下面是生成新的 SQL`GetBooks`方法。 你可以了解 EF 翻译 LINQ**选择**到 SQL SELECT 语句。

[!code-sql[Main](part-5/samples/sample3.sql)]

最后，修改`PostBook`方法以返回 DTO。

[!code-csharp[Main](part-5/samples/sample4.cs)]

> [!NOTE]
> 在本教程中，我们要转换到 Dto 手动在代码中。 另一个选项是使用诸如的库[AutoMapper](http://automapper.org/)用于自动处理转换。

>[!div class="step-by-step"]
[上一页](part-4.md)
[下一页](part-6.md)
