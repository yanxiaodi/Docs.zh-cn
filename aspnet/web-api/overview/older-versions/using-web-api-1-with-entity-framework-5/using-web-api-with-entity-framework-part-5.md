---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
title: "第 5 部分： 使用 Knockout.js 创建动态 UI |Microsoft 文档"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 9d9cb3b0-f4a7-434e-a508-9fc0ad0eb813
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
msc.type: authoredcontent
ms.openlocfilehash: 20ebdb1b8ba710e0fbc6040f7cd4064b44658c53
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="part-5-creating-a-dynamic-ui-with-knockoutjs"></a>第 5 部分： 使用 Knockout.js 创建动态 UI
====================
通过[Mike Wasson](https://github.com/MikeWasson)

[下载已完成的项目](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-a-dynamic-ui-with-knockoutjs"></a>使用 Knockout.js 创建动态 UI

在此部分中，我们将使用 Knockout.js 将功能添加到管理视图。

[Knockout.js](http://knockoutjs.com/)是一个 Javascript 库，可以轻松地将 HTML 控件绑定到数据。 Knockout.js 使用模型-视图-视图模型 (MVVM) 模式。

- *模型*是服务器端表示形式 （在我们用例、 产品和 orders） 对业务领域中的数据。
- *视图*是表示层 (HTML)。
- *视图模型*是保存模型数据的 Javascript 对象。 视图模型是 UI 的代码抽象。 它具有的 HTML 表示不知道。 相反，它表示抽象视图，如"的项的列表"的功能。

该视图被数据绑定到视图模型。 对视图模型更新会自动反映在视图中。 视图模型还从视图中，例如按钮单击、 获取事件，并对模型，如创建顺序执行操作。

![](using-web-api-with-entity-framework-part-5/_static/image1.png)

首先我们将定义视图模型。 之后，我们将到视图模型绑定的 HTML 标记。

将以下 Razor 部分添加到 Admin.cshtml:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample1.cshtml)]

可以在文件中的任意位置添加此部分。 该视图呈现时，部分会出现在 HTML 页中，底部右键在关闭前&lt;/b o&gt;标记。

所有此页的脚本将放入注释所指示的脚本标记：

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample2.html)]

首先，定义一个视图模型类：

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample3.js)]

**ko.observableArray**是中 Knockout，调用对象的特殊*可观测对象*。 从[Knockout.js 文档](http://knockoutjs.com/documentation/observables.html): 可观测对象是一个"JavaScript 对象，可以通知有关更改的订阅服务器。" 当可观测对象的内容更改时，视图将自动更新以匹配。

若要填充`products`数组，对 web API 进行 AJAX 请求。 回想一下我们在视图包 API 为存储的基 URI (请参阅[第 4 部分](using-web-api-with-entity-framework-part-4.md)本教程的)。

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample4.js?highlight=5)]

接下来，将函数添加到视图模型来创建、 更新和删除产品。 这些函数将提交到 web API 的 AJAX 调用，并使用的结果来更新视图模型。

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample5.js?highlight=7)]

现在的最重要的部分： 当 DOM 是 fulled 加载，调用**ko.applyBindings**函数并传入的新实例`ProductsViewModel`:

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample6.js)]

**Ko.applyBindings**方法激活 Knockout 和到视图连线视图模型。

现在，我们已视图模型，我们可以创建绑定。 在 Knockout.js，你执行此操作通过添加`data-bind`于 HTML 元素属性。 例如，若要将一个 HTML 列表绑定到一个数组，请使用`foreach`绑定：

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample7.html?highlight=1)]

`foreach`绑定循环访问数组和数组中创建子元素的每个对象。 上的子元素的绑定可以指数组对象上的属性。

将以下绑定添加到"更新产品"列表中：

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample8.html)]

`<li>`元素出现在范围内**foreach**绑定。 表示 Knockout 将呈现一次为每个产品中的元素`products`数组。 中的绑定的所有`<li>`元素引用该产品实例。 例如，`$data.Name`指`Name`产品上的属性。

若要设置的文本输入的值，使用`value`绑定。 按钮上模型-视图中，绑定到函数使用`click`绑定。 产品实例作为参数传递给每个函数。 有关详细信息， [Knockout.js 文档](http://knockoutjs.com/documentation/observables.html)具有良好的各种绑定的说明。

接下来，添加的绑定**提交**添加产品窗体上的事件：

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample9.html)]

此绑定调用`create`视图模型来创建新产品上的函数。

下面是管理视图的完整代码：

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample10.cshtml)]

运行应用程序的管理员帐户，登录，单击"Admin"链接。 你应查看的产品的列表，并能够创建、 更新或删除产品。

>[!div class="step-by-step"]
[上一页](using-web-api-with-entity-framework-part-4.md)
[下一页](using-web-api-with-entity-framework-part-6.md)
