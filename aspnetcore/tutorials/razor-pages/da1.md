---
title: "更新生成的页面"
author: rick-anderson
description: "更新生成的页面，提供更好的显示效果。"
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/da1
ms.openlocfilehash: a1bb1ab1e4fac9c634f4048947ac3f934af3d625
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="update-the-generated-pages"></a>更新生成的页面

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

我们的电影应用有个不错的开始，但是展示效果还不够理想。 我们不希望看到时间（如下图所示的 12:00:00 AM），并且“ReleaseDate”应为“Release Date”（两个词）。

![在 Chrome 中打开的显示电影数据的电影应用程序](sql/_static/m55.png)

## <a name="update-the-generated-code"></a>更新生成的代码

打开 Models/Movie.cs 文件，并添加以下代码中突出显示的行：

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

右键单击红色波浪线，然后单击“快速操作和重构”。

  ![上下文菜单中显示“快速操作和重构”。](da1/qa.png)

选择 `using System.ComponentModel.DataAnnotations;`

  ![使用列表顶部的 System.ComponentModel.DataAnnotations](da1/da.png)

  Visual studio 添加 `using System.ComponentModel.DataAnnotations;`。

[!INCLUDE[model1](../../includes/RP/da2.md)]

>[!div class="step-by-step"]
[上一篇：使用 SQL Server LocalDB](xref:tutorials/razor-pages/sql)
[添加搜索](xref:tutorials/razor-pages/search)
