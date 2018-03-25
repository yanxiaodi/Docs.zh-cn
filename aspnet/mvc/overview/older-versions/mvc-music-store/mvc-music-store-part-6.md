---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
title: "第 6 部分： 使用数据注释为模型验证 |Microsoft 文档"
author: jongalloway
description: "本系列教程详细介绍所有生成 ASP.NET MVC 音乐商店示例应用程序所采取的步骤。 第 6 部分介绍如何为模型 V 使用数据注释..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: b3193d33-2d0b-4d98-9712-58bd897c62ec
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
msc.type: authoredcontent
ms.openlocfilehash: b2083d5604741a0142f504f92779c9f931d0d6d9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="part-6-using-data-annotations-for-model-validation"></a>模型验证的的第 6 部分： 使用数据注释
====================
通过[Jon Galloway](https://github.com/jongalloway)

> MVC 音乐商店是，从而引入并说明如何使用 ASP.NET MVC 和 Visual Studio 进行 web 开发分步教程应用程序。  
>   
> MVC 音乐商店是一个轻型示例存储区实现，该类销售音乐集联机，并实现基本的站点管理、 用户登录，和购物车功能。  
>   
> 本系列教程详细介绍所有生成 ASP.NET MVC 音乐商店示例应用程序所采取的步骤。 第 6 部分介绍如何为模型验证中使用数据注释。


我们使用我们创建和编辑的窗体具有主要问题： 未做任何验证。 我们可以执行某些操作，如将必填的字段保留为空或类型字母保留为 Price 字段中，因此我们将看到第一个错误为从数据库。

我们可以轻松地添加验证我们的应用程序通过将数据注释添加到我们的模型类。 数据注释使我们能够描述应用于我们的模型属性，我们想要的规则和 ASP.NET MVC 将会负责的实施它们并向我们的用户显示相应的消息。

## <a name="adding-validation-to-our-album-forms"></a>将验证添加到我们唱片集窗体

我们将使用以下数据注释属性：

- **所需**– 指示该属性是必填的字段
- **DisplayName** – 定义我们想要将其用于在窗体字段和验证消息的文本
- **StringLength** – 定义字符串字段的最大长度
- **范围**– 为数字字段提供最大和最小值
- **将绑定**– 列出要排除或包括绑定到模型属性的参数或窗体值时字段
- **ScaffoldColumn** – 允许隐藏编辑器窗体中的字段

*注意： 有关使用数据注释属性的模型验证的详细信息，请参阅 MSDN 文档：*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)

打开唱片集类并添加以下*使用*到顶部的语句。

[!code-csharp[Main](mvc-music-store-part-6/samples/sample1.cs)]

接下来，更新以添加显示和验证的特性，如下所示的属性。

[!code-csharp[Main](mvc-music-store-part-6/samples/sample2.cs)]

当我们解决存在时，我们已更改为虚拟属性的流派和艺术家。 这使实体框架可以延迟加载它们根据需要。

[!code-csharp[Main](mvc-music-store-part-6/samples/sample3.cs)]

后具有这些属性添加到我们唱片集的模型，我们创建和编辑屏幕立即开始验证字段，并使用显示名称，我们已选择 (例如唱片集艺术作品 Url 而不是 AlbumArtUrl)。 运行应用程序，并浏览到 /StoreManager/Create。

![](mvc-music-store-part-6/_static/image1.png)

接下来，我们会破坏的一些的验证规则。 输入指价格为 0，并将标题留空。 当我们单击创建按钮时，我们将看到显示验证错误消息显示哪些字段不符合验证规则我们已定义的表单。

![](mvc-music-store-part-6/_static/image2.png)

## <a name="testing-the-client-side-validation"></a>测试客户端验证

服务器端验证是从应用程序的角度看，非常重要，因为用户可以绕过客户端验证。 但是，仅实现服务器端验证网页窗体表现出三个重大问题。

1. 用户必须等待的窗体发布，验证在服务器上，且其浏览器发送的响应。
2. 它们更正字段，以便它现在通过验证规则时，用户不会收到的即时反馈。
3. 我们会浪费的服务器资源，以便执行验证逻辑，而不是利用用户的浏览器。

幸运的是，ASP.NET MVC 3 基架模板具有客户端验证内置的需要无论如何任何额外工作。

在标题字段中键入是一个字母满足验证要求，因此验证消息将立即删除。

![](mvc-music-store-part-6/_static/image3.png)


>[!div class="step-by-step"]
[上一页](mvc-music-store-part-5.md)
[下一页](mvc-music-store-part-7.md)
