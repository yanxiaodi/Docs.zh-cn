---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-4
title: "处理实体关系 |Microsoft 文档"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: d2f5710c-23c7-40a5-9cd9-5d0516570cba
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-4
msc.type: authoredcontent
ms.openlocfilehash: 58a9dfb621630f23b37247b96ed3a19a661857f1
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="handling-entity-relations"></a>处理实体关系
====================
通过[Mike Wasson](https://github.com/MikeWasson)

[下载已完成的项目](https://github.com/MikeWasson/BookService)

本部分介绍如何 EF 加载相关的实体，以及如何处理模型类中的循环的导航属性的某些详细信息。 （本部分提供背景知识，并不需要完成本教程。 如果你愿意，请跳到[第 5 部分。](part-5.md)。)

## <a name="eager-loading-versus-lazy-loading"></a>预先加载与延迟加载

使用关系数据库支持 EF，时，一定要了解如何 EF 加载相关的数据。

它也是有用若要查看 EF 生成的 SQL 查询。 若要跟踪 SQL，添加以下代码的行`BookServiceContext`构造函数：

[!code-csharp[Main](part-4/samples/sample1.cs)]

如果将 GET 请求发送到 /api/books 中时，它将返回 JSON，如下所示：

[!code-console[Main](part-4/samples/sample2.cmd)]

你可以看到 Author 属性为 null，，即使该指南包含有效的作者 Id 也是如此。 这是因为 EF 不在加载相关的作者实体。 SQL 查询的跟踪日志对此进行确认：

[!code-console[Main](part-4/samples/sample3.sql)]

SELECT 语句采用丛书表中并不引用作者表。

作为参考，下面是中的方法`BooksController`返回的书籍列表的类。

[!code-csharp[Main](part-4/samples/sample4.cs)]

我们来看作为 JSON 数据的一部分，我们就可以返回作者。 有三种方法来加载相关的实体框架中的数据： 预先加载、 延迟加载和显式加载。 有权衡与每种技术，因此务必了解它们如何工作。

### <a name="eager-loading"></a>预先加载

与*预先加载*，EF 加载相关的实体作为初始数据库查询的一部分。 若要执行预先加载，使用**System.Data.Entity.Include**扩展方法。

[!code-csharp[Main](part-4/samples/sample5.cs)]

这将告知 EF 在查询中包含作者数据。 如果你进行此更改，并运行应用，现在的 JSON 数据如下所示：

[!code-console[Main](part-4/samples/sample6.cmd)]

跟踪日志显示 EF Book 和作者表执行联接。

[!code-console[Main](part-4/samples/sample7.cmd)]

### <a name="lazy-loading"></a>延迟加载

使用延迟加载，EF 自动加载相关的实体时该实体的导航属性取消引用。 若要启用延迟加载，请导航属性虚拟。 例如，在 Book 类中：

[!code-csharp[Main](part-4/samples/sample8.cs?highlight=6)]

现在请考虑下面的代码：

[!code-csharp[Main](part-4/samples/sample9.cs)]

启用延迟加载后，访问`Author`属性`books[0]`导致 EF 作者查询数据库。

延迟加载需要多个数据库访问，原因是 EF 发送查询每次它检索的相关的实体。 一般而言，你希望在序列化的对象禁用延迟加载。 序列化程序具有读取所有模型，随即将会触发加载相关的实体的属性。 例如，以下是出现 EF 使用启用的延迟加载序列化的书籍列表的 SQL 查询。 你可以看到 EF 的三个作者使三个单独的查询。

[!code-console[Main](part-4/samples/sample10.sql)]

仍有的时候你可能想要使用延迟加载。 预先加载可能会导致 EF 生成非常复杂的联接。 或可能的数据，一小部分需要相关的实体和延迟加载将更有效。

若要避免序列化问题的一种方法是序列化而不是实体对象的数据传输对象 (Dto)。 在本文的后面，我将介绍这种方法。

### <a name="explicit-loading"></a>显式加载

显式加载具有类似于延迟加载的只不过您显式将相关的数据进入代码;它不会发生自动访问导航属性时。 显式加载功能允许更好地控制何时加载相关的数据，但需要额外的代码。 有关显式加载的详细信息，请参阅[加载相关实体](https://msdn.microsoft.com/data/jj574232#explicit)。

## <a name="navigation-properties-and-circular-references"></a>导航属性并循环引用

当定义了 Book 和作者模型时，定义一个导航属性上`Book`类对于册作者关系，但我未在另一个方向定义导航属性。

如果你添加到对应的导航属性，将会发生什么情况`Author`类？

[!code-csharp[Main](part-4/samples/sample11.cs?highlight=7)]

遗憾的是，这将创建问题在序列化模型时。 如果你加载相关的数据，它将创建循环对象图。

![](part-4/_static/image1.png)

当 JSON 或 XML 格式化程序尝试序列化关系图时，它将引发异常。 两个格式化程序引发了不同的异常消息。 下面是一个示例，用于 JSON 格式化程序：

[!code-console[Main](part-4/samples/sample12.cmd)]

下面是 XML 格式化程序：

[!code-xml[Main](part-4/samples/sample13.xml)]

一种解决方案是使用 Dto，我介绍了下一节中。 或者，你可以配置 JSON 和 XML 格式化程序来处理 graph 周期。 有关详细信息，请参阅[处理循环对象引用](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references)。

对于本教程中，不需要使用`Author.Book`导航属性，因此您可以忽略它。

>[!div class="step-by-step"]
[上一页](part-3.md)
[下一页](part-5.md)
