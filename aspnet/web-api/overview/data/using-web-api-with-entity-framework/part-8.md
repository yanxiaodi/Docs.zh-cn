---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-8
title: "显示项的详细信息 |Microsoft 文档"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 75ef94b1-bbec-4681-9210-452dba816144
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-8
msc.type: authoredcontent
ms.openlocfilehash: 0b6ae9384843712cae824ea662b984a40f021e57
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="display-item-details"></a>显示项详细信息
====================
通过[Mike Wasson](https://github.com/MikeWasson)

[下载已完成的项目](https://github.com/MikeWasson/BookService)

在此部分中，你将添加的功能，若要查看的每本书的详细信息。 在 app.js，添加到视图模型的以下代码：

[!code-javascript[Main](part-8/samples/sample1.js)]

在 Views/Home/Index.cshtml，将添加到详细信息链接的数据绑定元素：

[!code-html[Main](part-8/samples/sample2.html?highlight=5)]

这会将绑定的单击处理程序&lt;&gt;元素`getBookDetail`视图模型上的函数。

在同一文件中，将以下标记向上：

[!code-html[Main](part-8/samples/sample3.html)]

替换为以下内容：

[!code-html[Main](part-8/samples/sample4.html)]

此标记将创建一个表，其中被数据绑定到的属性`detail`可观察到视图模型中。

"&lt;！-Ko-&gt; &quot;语法，您可以包含在 DOM 元素之外的 Knockout 绑定。 在这种情况下，`if`绑定会导致要显示的标记的本部分仅当`details`为非 null。

[!code-html[Main](part-8/samples/sample5.html)]

现在，如果你运行应用并单击其中一个&quot;详细信息&quot;链接，该应用程序将显示簿详细信息。

[![](part-8/_static/image2.png)](part-8/_static/image1.png)

>[!div class="step-by-step"]
[上一页](part-7.md)
[下一页](part-9.md)
