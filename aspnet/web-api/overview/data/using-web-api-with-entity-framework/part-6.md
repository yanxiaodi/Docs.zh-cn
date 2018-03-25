---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-6
title: "创建 JavaScript 客户端 |Microsoft 文档"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 20360326-b123-4b1e-abae-1d350edf4ce4
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-6
msc.type: authoredcontent
ms.openlocfilehash: b397c5a413ae213c9b79da1c0e0626efe21c7e21
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="create-the-javascript-client"></a>创建 JavaScript 客户端
====================
通过[Mike Wasson](https://github.com/MikeWasson)

[下载已完成的项目](https://github.com/MikeWasson/BookService)

在本部分中，你将创建客户端应用程序，使用 HTML、 JavaScript 和[Knockout.js](http://knockoutjs.com/)库。 我们将分阶段生成客户端应用程序：

- 显示的书籍的列表。
- 显示簿详细信息。
- 添加新书籍。

Knockout 库使用模型-视图-视图模型 (MVVM) 模式：

- **模型**是服务器端表示形式 （在我们的用例、 丛书和作者） 对业务领域中的数据。
- **视图**是表示层 (HTML)。
- **视图模型**是包含模型的 JavaScript 对象。 视图模型是 UI 的代码抽象。 它具有的 HTML 表示不知道。 相反，它表示抽象功能的视图，如&quot;的书籍的列表&quot;。

视图是数据绑定到视图模型。 对视图模型更新会自动反映在视图中。 视图模型还获取事件从视图中，如按钮单击。

![](part-6/_static/image1.png)

这种方法轻松地更改的布局和 UI 应用程序，因为您可以更改绑定，无需重写任何代码。 例如，可能显示的项作为列表`<ul>`，然后稍后将其更改为表。

## <a name="add-the-knockout-library"></a>添加 Knockout 库

在 Visual Studio 中，从**工具**菜单上，选择**库程序包管理器**。 然后选择**程序包管理器控制台**。 在 Package Manager Console 窗口中，输入以下命令：

[!code-console[Main](part-6/samples/sample1.cmd)]

此命令将 Knockout 文件添加到脚本文件夹中。

## <a name="create-the-view-model"></a>创建视图模型

添加一个名为脚本文件夹的 app.js 的 JavaScript 文件。 (在解决方案资源管理器，右键单击脚本文件夹中，选择**添加**，然后选择**JavaScript 文件**。)粘贴以下代码：

[!code-javascript[Main](part-6/samples/sample2.js)]

在 Knockout，`observable`类实现数据绑定。 当可观测对象的内容发生更改时，可观测对象的通知的所有数据绑定控件，以便他们能够更新自己。 (`observableArray`类是数组新版*可观测对象*。)开始时，我们视图模型具有两个可观察对象：

- `books`包含书籍的列表。
- `error`如果 AJAX 调用失败，则包含一条错误消息。

`getAllBooks`方法进行的 AJAX 调用，以便获取的书籍列表。 然后它会推送到结果`books`数组。

`ko.applyBindings`方法是 Knockout 库的一部分。 它采用视图模型作为参数，并将数据绑定设置。

## <a name="add-a-script-bundle"></a>添加脚本捆绑包

绑定是可以轻松地合并或捆绑到单个文件的多个文件的 ASP.NET 4.5 中的功能。 绑定到服务器，这可以提高页面加载时间减少请求的数。

打开文件应用\_Start/BundleConfig.cs。 将以下代码添加到 RegisterBundles 方法。

[!code-csharp[Main](part-6/samples/sample3.cs)]

>[!div class="step-by-step"]
[上一页](part-5.md)
[下一页](part-7.md)
