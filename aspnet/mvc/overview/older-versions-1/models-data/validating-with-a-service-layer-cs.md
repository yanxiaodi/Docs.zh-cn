---
uid: mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
title: "验证与服务层 (C#) |Microsoft 文档"
author: StephenWalther
description: "了解如何移动你的验证逻辑控制器操作出来放入单独的服务层。 在本教程中，Stephen Walther 解释了如何你..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 4eabc535-b8a1-43f5-bb99-cfeb86db0fca
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: f36301aef4377c6c00cb4fc33dbc5c57b1c426a9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="validating-with-a-service-layer-c"></a>验证与服务层 (C#)
====================
通过[Stephen Walther](https://github.com/StephenWalther)

> 了解如何移动你的验证逻辑控制器操作出来放入单独的服务层。 在本教程中，Stephen Walther 解释了如何通过隔离接受你的服务层，从控制器层维护尖锐分离问题。


本教程的目的是为了说明的 ASP.NET MVC 应用程序中执行验证的一种方法。 在本教程中，您将学习如何移动你的验证逻辑你的控制器出来放入单独的服务层。

## <a name="separating-concerns"></a>分隔问题

生成 ASP.NET MVC 应用程序时，不应在你的控制器操作内将你数据库的逻辑。 混合使用你的数据库和控制器逻辑让你应用程序随着时间的推移维护更加困难。 推荐方法是你将所有数据库逻辑放在单独的存储库层。

例如，清单 1 包含名为化 ProductRepository 简单存储库。 产品储存库包含的所有应用程序的数据访问代码。 该列表还包括产品存储库实现的 IProductRepository 接口。

**列表 1-Models\ProductRepository.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample1.cs)]

列出 2 中的控制器其 index （） 和 create （） 操作中使用存储库层。 请注意，此控制器不包含任何数据库逻辑。 创建存储库层使您能够维护完全分离关注点。 控制器负责应用程序流控制逻辑和存储库负责数据访问逻辑。

**列出 2-Controllers\ProductController.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample2.cs)]

## <a name="creating-a-service-layer"></a>创建服务层

因此，应用程序流控制逻辑所属控制器中，并且在存储库所属的数据访问逻辑。 在这种情况下，不要放置您的验证逻辑？ 一种选择是将放置在您验证逻辑*服务层*。

服务层是在 ASP.NET MVC 应用程序进行调解控制器和存储库层之间的通信的附加层。 服务层包含业务逻辑。 具体而言，它包含验证逻辑。

例如，清单 3 的产品服务层具有 CreateProduct() 方法。 CreateProduct() 方法调用 ValidateProduct() 方法以将产品传递给产品存储库之前验证新产品。

**列出 3-Models\ProductService.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample3.cs)]

产品控制器已更新列出 4 而不是存储库层使用的服务层中。 控制器层介绍对服务层。 服务层与存储库层通信。 每个层都有单独的责任。

**列出 4-Controllers\ProductController.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample4.cs)]

请注意，在产品控制器构造函数创建产品服务。 创建产品服务后，模型状态字典将传递到服务。 产品服务使用模型状态将验证错误消息传递回控制器。

## <a name="decoupling-the-service-layer"></a>分离的服务层

我们无法隔离控制器和一个方面中的服务层。 控制器和服务层通过模型状态进行通信。 换而言之，服务层具有特定功能的 ASP.NET MVC framework 的依赖关系。

我们想要隔离从尽可能多地我们控制器层的服务层。 从理论上讲，我们应能够使用任何类型的应用程序和不只一个 ASP.NET MVC 应用程序使用的服务层。 例如，在将来，我们可能想要生成 WPF 前端我们为应用程序。 我们应从我们的服务层提供了如何删除对 ASP.NET MVC 依赖关系模型状态。

在列出 5 中，服务层具有已更新，以使其不再使用模型状态。 相反，它使用的任何类都实现 IValidationDictionary 接口。

**列出 5-Models\ProductService.cs （分离）**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample5.cs)]

在列出 6 定义 IValidationDictionary 接口。 此简单的界面都具有单个方法和单个属性。

**列出 6-Models\IValidationDictionary.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample6.cs)]

在列出 7 中，名为 ModelStateWrapper 类，类实现 IValidationDictionary 接口。 可以通过将模型状态字典传递给构造函数来实例化 ModelStateWrapper 类。

**列出 7-Models\ModelStateWrapper.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample7.cs)]

最后，列出 8 中的更新的控制器使用 ModelStateWrapper 在其构造函数中创建服务层时。

**列出 8-Controllers\ProductController.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample8.cs)]

使用 IValidationDictionary 接口和 ModelStateWrapper 类使我们能够将从我们控制器层我们服务层完全隔离。 服务层不再依赖于模型状态。 可以传递任何实现的服务层的 IValidationDictionary 接口的类。 例如，一个 WPF 应用程序可能实现一个简单的集合类 IValidationDictionary 的接口。

## <a name="summary"></a>摘要

本教程的目的是讨论在 ASP.NET MVC 应用程序中执行验证的一种方法。 在本教程中，您学习了如何将您的验证逻辑你的控制器出来放入单独的服务层的所有移动。 你还了解了如何通过创建 ModelStateWrapper 类隔离你从控制器层的服务层。

>[!div class="step-by-step"]
[上一页](validating-with-the-idataerrorinfo-interface-cs.md)
[下一页](validation-with-the-data-annotation-validators-cs.md)
