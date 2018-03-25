---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-7
title: "创建视图 (UI) |Microsoft 文档"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: b2445062-a1fe-4133-8994-f510280f6d9a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-7
msc.type: authoredcontent
ms.openlocfilehash: 8c5cc662e2e3c9cb07ca9e30ff57eb084d58e1bb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="create-the-view-ui"></a>创建视图 (UI)
====================
通过[Mike Wasson](https://github.com/MikeWasson)

[下载已完成的项目](https://github.com/MikeWasson/BookService)

在本部分中，你将开始定义应用程序中的 HTML 和添加 HTML 和视图模型之间的数据绑定。

打开文件 Views/Home/Index.cshtml。 将该文件的全部内容替换为以下。

[!code-cshtml[Main](part-7/samples/sample1.cshtml)]

大部分`div`元素还有适用于[Bootstrap](http://getbootstrap.com/)样式。 重要的元素是带有的`data-bind`属性。 此属性链接到视图模型的 HTML。

例如: 

[!code-html[Main](part-7/samples/sample2.html)]

在此示例中， &quot; `text` &quot;绑定原因`<p>`元素以显示的值`error`从视图模型的属性。 回想一下，`error`被声明为`ko.observable`:

[!code-javascript[Main](part-7/samples/sample3.js)]

每当一个新值分配给`error`，Knockout 更新中的文本`<p>`元素。

`foreach`绑定告知 Knockout 循环访问的内容`books`数组。 对于数组中每个项，请 Knockout 创建一个新&lt;li&gt;元素。 绑定的上下文中的`foreach`引用数组项的属性。 例如: 

[!code-html[Main](part-7/samples/sample4.html)]

此处`text`绑定读取每本书的作者属性。

如果你运行应用程序现在，它应如下所示：

![](part-7/_static/image1.png)

在页面加载之后，以异步方式加载的书籍列表。 现在，&quot;详细信息&quot;链接不起作用。 我们将在下一部分中添加此功能。

>[!div class="step-by-step"]
[上一页](part-6.md)
[下一页](part-8.md)
