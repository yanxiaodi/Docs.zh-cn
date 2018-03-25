---
title: "控制器方法和视图"
author: rick-anderson
description: "使用控制器方法、视图和 DataAnnotations"
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/controller-methods-views
ms.openlocfilehash: 200f02f9815966653b3b46918737c60d11f11d5a
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="controller-methods-and-views"></a>控制器方法和视图

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

我们的电影应用有个不错的开始，但是展示效果还不够理想。 我们不希望看到时间（如下图所示的 12:00:00 AM），并且“ReleaseDate”应为两个词。

![索引视图：Release Date 为一个词（没有空格），每个电影发行日期都显示时间中午 12 点](working-with-sql/_static/m55.png)

打开 Models/Movie.cs 文件，并添加以下代码中突出显示的行：

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]

右键单击红色的波浪线，然后选择“快速操作和重构”。

  ![上下文菜单中显示“快速操作和重构”。](controller-methods-views/_static/qa.png)


点击 `using System.ComponentModel.DataAnnotations;`

  ![使用列表顶部的 System.ComponentModel.DataAnnotations](controller-methods-views/_static/da.png)

  Visual studio 添加 `using System.ComponentModel.DataAnnotations;`。

删除不需要的 `using` 语句。 默认情况下，它们显示为浅灰色字体。 右键单击 Movie.cs 文件中的任意位置，然后选择“对 Using 进行删除和排序”。

![对 Using 进行删除和排序](controller-methods-views/_static/rm.png)

更新的代码：

[!code-csharp[Main](./start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1)]

<!-- include start -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
[上一页](working-with-sql.md)
[下一页](search.md)  
