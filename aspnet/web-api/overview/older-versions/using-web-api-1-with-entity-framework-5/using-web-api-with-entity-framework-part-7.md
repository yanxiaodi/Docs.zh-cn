---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
title: "第 7 部分： 创建主页面 |Microsoft 文档"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: eb32a17b-626c-4373-9a7d-3387992f3c04
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
msc.type: authoredcontent
ms.openlocfilehash: 4d06e72bc664f707bbbe4603be41347158c58903
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="part-7-creating-the-main-page"></a>第 7 部分： 创建主页面
====================
通过[Mike Wasson](https://github.com/MikeWasson)

[下载已完成的项目](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-the-main-page"></a>创建主页面

在本部分中，你将创建主应用程序页。 因此我们将方法处理它在几个步骤中，此页将会比管理员页中，更复杂。 与此同时，你将看到一些更高级的 Knockout.js 技术。 下面是基本页的布局：

![](using-web-api-with-entity-framework-part-7/_static/image1.png)

- "Products"包含产品的数组。
- "购物车"包含的产品与数量的数组。 单击"添加放入购物车"更新购物车。
- "Orders"包含订单 Id 的数组。
- "详细信息"包含订单详细信息，这是项 （产品与数量） 的数组

我们将开始通过使用无数据绑定或脚本在 HTML 中，定义一些基本布局。 打开文件 Views/Home/Index.cshtml 并将替换为以下的所有内容：

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample1.html)]

接下来，添加一个脚本部分，并创建一个空的视图模型：

[!code-cshtml[Main](using-web-api-with-entity-framework-part-7/samples/sample2.cshtml)]

根据前面草的设计，我们视图模型需要可观察对象产品、 购物车、 订单和详细信息。 添加到以下变量`AppViewModel`对象：

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample3.js)]

用户可以从产品列表将项添加到购物车，和从购物车中删除项。 若要封装这些函数，我们将创建表示产品的另一个视图模型类。 将下列代码添加到 `AppViewModel`：

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample4.js?highlight=4)]

`ProductViewModel`类包含用于购物车之间转移产品的两个函数：`addItemToCart`将产品的一个单元添加到购物车，和`removeAllFromCart`中移除所有的产品数量。

用户可以选择一个现有的 order 并获取订单详细信息。 我们将封装到另一个视图模型此功能：

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample5.js?highlight=4)]

`OrderDetailsViewModel`初始化顺序，并且它通过向服务器发送 AJAX 请求提取订单详细信息。

另请注意`total`属性`OrderDetailsViewModel`。 此属性是一种特殊的可观测对象调用[计算可观测对象](http://knockoutjs.com/documentation/computedObservables.html)。 顾名思义，计算可观测对象允许你为计算的值 &#8212;的数据绑定; 在这种情况下，总体成本的顺序。

接下来，添加到这些函数`AppViewModel`:

- `resetCart`从购物车中移除所有项。
- `getDetails`获取为某一订单的详细信息 (由一个新的 p 通过`OrderDetailsViewModel`到`details`列表)。
- `createOrder`创建新订单并清空购物车。


[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample6.js?highlight=4)]

最后，发出的产品和订单的 AJAX 请求初始化视图模型：

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample7.js)]

好了，这就是大量的代码，但我们构建分步，希望设计是清除。 现在我们可以将某些 Knockout.js 绑定添加到 HTML。

**产品**

以下是有关产品列表的绑定：

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample8.html)]

这将循环访问产品数组并显示的名称和价格。 仅当用户登录时，"添加到订单"按钮可见。

在"添加到订单"按钮调用`addItemToCart`上`ProductViewModel`产品的实例。 此示例演示一个不错的功能的 Knockout.js： 当视图模型包含其他视图模型时，可以将绑定应用于内部的模型。 在此示例中，在绑定`foreach`应用于每个`ProductViewModel`实例。 这种方法是更清楚比的所有功能置于单个视图模型。

**Cart**

下面是购物车的绑定：

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample9.html)]

这将循环访问购物车数组并显示名称、 价格和数量。 请注意，则会将"删除"链接和"创建订单"按钮绑定到视图模型函数。

**订单**

以下是有关的订单列表的绑定：

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample10.html)]

此循环访问订单，并显示订单 id。 Click 事件的链接绑定到`getDetails`函数。

**订单详细信息**

下面是显示订单详细信息的绑定：

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample11.html)]

这将循环访问的项的顺序，并显示产品、 价格和数量。 周围的 div 是详细信息数组包含一个或多个项时才可见。

## <a name="conclusion"></a>结束语

在本教程中，你创建的应用程序使用实体框架与数据库和 ASP.NET Web API，用于提供在数据层的顶部的面向公众的接口通信。 我们使用 ASP.NET MVC 4 来呈现的 HTML 页面，和 Knockout.js 加 jQuery，以提供动态交互，而无需重新加载页。

其他资源：

- [ASP.NET 数据访问内容映射](https://msdn.microsoft.com/library/6759sth4.aspx)
- [实体框架开发人员中心](https://msdn.microsoft.com/data/ef)

>[!div class="step-by-step"]
[上一篇](using-web-api-with-entity-framework-part-6.md)
