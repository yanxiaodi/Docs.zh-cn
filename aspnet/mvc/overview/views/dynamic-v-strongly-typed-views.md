---
uid: mvc/overview/views/dynamic-v-strongly-typed-views
title: "动态 v。 强类型视图 |Microsoft 文档"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2011
ms.topic: article
ms.assetid: 0cbd88da-0da6-4605-b222-2835c6478304
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/views/dynamic-v-strongly-typed-views
msc.type: authoredcontent
ms.openlocfilehash: 8a96d43e04a0a50d5176c10c26aa49918a0e56ef
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="dynamic-v-strongly-typed-views"></a>动态 v。 强类型化的视图
====================
通过[Rick Anderson](https://github.com/Rick-Anderson)

有三种方法将从控制器的信息传递给 ASP.NET MVC 3 中的视图：

1. 作为强类型化的模型对象。
2. 为动态类型 (使用@model动态)
3. 使用 ViewBag

我已编写简单的 MVC 3 顶部博客应用程序进行比较和对比动态和强类型化视图。 控制器开头博客的简单列表：

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample1.cs)]

右键单击 IndexNotStonglyTyped() 方法中并添加 Razor 视图。

[![8475.NotStronglyTypedView [1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)

请确保**创建强类型化视图**不选中复选框。 获得的视图不包含很多：

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample2.cshtml)]

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample3.cshtml)]

由于我们要使用动态和不是强类型化的视图，intellisense 不会帮助我们。 已完成的代码所示：

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample4.cshtml)]

[![6646.NotStronglyTypedView_5F00_IE [1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)

现在我们将添加一个强类型化的视图。 将以下代码添加到控制器：

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample5.cs)]


请注意它是完全相同返回 View(topBlogs);调用作为非强类型化视图。 右键单击内*StonglyTypedIndex()*和选择**添加视图**。 这次请选择**博客**模型类，并选择**列表**作为基架模板。

[![5658.StrongView [1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)

在新的视图模板内我们获得 intellisense 支持。

[![7002.intellesince [1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)

可以下载 c# 项目[此处](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip)。
