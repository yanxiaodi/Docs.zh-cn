---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-9
title: "向数据库添加新项 |Microsoft 文档"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 0967c29e-e124-4db0-a788-c45d0ff5aff2
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-9
msc.type: authoredcontent
ms.openlocfilehash: d33355b1bd286513958f71ce5521942a6cbb584f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="add-a-new-item-to-the-database"></a>向数据库添加新项
====================
通过[Mike Wasson](https://github.com/MikeWasson)

[下载已完成的项目](https://github.com/MikeWasson/BookService)

在此部分中，你将添加对用户创建新的书籍的功能。 在 app.js，到视图模型中添加以下代码：

[!code-javascript[Main](part-9/samples/sample1.js)]

在 Index.cshtml，将以下标记：

[!code-html[Main](part-9/samples/sample2.html)]

替换为：

[!code-html[Main](part-9/samples/sample3.html)]

此标记将创建用于提交新作者的窗体。 作者下拉列表的值是数据绑定到`authors`可观察到视图模型中。 对于其他窗体输入，值是数据绑定到`newBook`视图模型的属性。

窗体上的提交处理程序绑定到`addBook`函数：

[!code-html[Main](part-9/samples/sample4.html)]

`addBook`函数将读取要创建一个 JSON 对象的数据绑定窗体输入的当前值。 然后它会发送到的 JSON 对象`/api/books`。

>[!div class="step-by-step"]
[上一页](part-8.md)
[下一页](part-10.md)
