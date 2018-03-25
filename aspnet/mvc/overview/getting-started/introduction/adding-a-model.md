---
uid: mvc/overview/getting-started/introduction/adding-a-model
title: "添加模型 |Microsoft 文档"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 276552b5-f349-4fcf-8f40-6d042f7aa88e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 79f136257119a8600a65e8d7c5f6e99cb9abceae
ms.sourcegitcommit: d8aa1d314891e981460b5e5c912afb730adbb3ad
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/05/2018
---
<a name="adding-a-model"></a>添加模型
====================
通过[Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

在本部分中，你将添加用于管理数据库中的电影某些类。 这些类将&quot;模型&quot;ASP.NET MVC 应用程序的一部分。

你将使用名为的.NET Framework 数据访问技术[实体框架](https://docs.microsoft.com/ef/)定义和使用这些模型类。 开发范例调用 （通常称为 EF） 的实体框架支持*Code First*。 代码首先，可通过编写简单的类来创建模型对象。 (这些是也称为 POCO 类，从&quot;纯旧式 CLR 对象。&quot;)然后，你可以这样非常干净且快速开发工作流从您的类，动态创建的数据库。 如果你需要首先创建数据库，则仍可按照本教程，若要了解有关 MVC 和 EF 应用程序开发。 然后，你可以按照 Tom Fizmakens [ASP.NET 基架](xref:visual-studio/overview/2013/aspnet-scaffolding-overview)教程，介绍数据库第一种方法。

## <a name="adding-model-classes"></a>添加模型类

在**解决方案资源管理器**，右键单击*模型*文件夹，选择**添加**，然后选择**类**。

![](adding-a-model/_static/image1.png)

输入*类*名称&quot;电影&quot;。

添加到以下五种属性`Movie`类：

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

我们将使用`Movie`类来表示数据库中的电影。 每个实例`Movie`对象将对应于数据库表中，并的每个属性中的行`Movie`类将映射到表中的列。

注意： 若要使用 System.Data.Entity 和相关的类，你需要安装[实体 Framework NuGet 包](https://www.nuget.org/packages/EntityFramework/)。 按链接进行进一步的说明。

在同一文件中，添加以下`MovieDBContext`类：

[!code-csharp[Main](adding-a-model/samples/sample2.cs?highlight=2,15-18)]

`MovieDBContext`类表示的实体框架的电影数据库上下文，用于处理提取、 存储和更新`Movie`类数据库中的实例。 `MovieDBContext`派生自`DbContext`由实体框架提供的基类。

若要引用`DbContext`和`DbSet`，你需要添加以下`using`语句文件的顶部：

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

你可以执行此操作通过手动添加 using 语句，也可以将鼠标悬停在红色波浪线，请单击`Show potential fixes`单击`using System.Data.Entity;`

![](adding-a-model/_static/image2.png)

注意： 多个未使用`using`语句已删除。 Visual Studio 将显示为灰色的未使用的依赖关系。 你可以通过将鼠标悬停在灰色的依赖关系中删除未使用的依赖关系，请单击`Show potential fixes`单击**删除未使用的 Using。**

![](adding-a-model/_static/image3.png)

最后，我们添加了模型 (MVC 中的 M) 上。 在下一部分中你将使用的数据库连接字符串。

>[!div class="step-by-step"]
[上一页](adding-a-view.md)
[下一页](creating-a-connection-string.md)
