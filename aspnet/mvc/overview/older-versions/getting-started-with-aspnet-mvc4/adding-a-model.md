---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
title: "添加模型 |Microsoft 文档"
author: Rick-Anderson
description: "注意： 本教程的更新的版本此处提供了使用 ASP.NET MVC 5 和 Visual Studio 2013。 它是更安全，请按照和演示要简单得多..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 53db72da-e0b9-44d9-b60b-6e6988c00b28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 304c428b0d787e902f30c1989471c476f54d3b39
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="adding-a-model"></a>添加模型
====================
通过[Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > 本教程的更新的版本可[此处](../../getting-started/introduction/getting-started.md)使用 ASP.NET MVC 5 和 Visual Studio 2013。 它是更安全，请按照要简单得多，并演示更多的功能。


在本部分中，你将添加用于管理数据库中的电影某些类。 这些类将&quot;模型&quot;ASP.NET MVC 应用程序的一部分。

你将使用名为的.NET Framework 数据访问技术[实体框架](https://msdn.microsoft.com/library/bb399572(VS.110).aspx)定义和使用这些模型类。 开发范例调用 （通常称为 EF） 的实体框架支持*Code First*。 代码首先，可通过编写简单的类来创建模型对象。 (这些是也称为 POCO 类，从&quot;纯旧式 CLR 对象。&quot;)然后，你可以这样非常干净且快速开发工作流从您的类，动态创建的数据库。

## <a name="adding-model-classes"></a>添加模型类

在**解决方案资源管理器**，右键单击*模型*文件夹，选择**添加**，然后选择**类**。

![](adding-a-model/_static/image1.png)

输入*类*名称&quot;电影&quot;。

添加到以下五种属性`Movie`类：

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

我们将使用`Movie`类来表示数据库中的电影。 每个实例`Movie`对象将对应于数据库表中，并的每个属性中的行`Movie`类将映射到表中的列。

在同一文件中，添加以下`MovieDBContext`类：

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

`MovieDBContext`类表示的实体框架的电影数据库上下文，用于处理提取、 存储和更新`Movie`类数据库中的实例。 `MovieDBContext`派生自`DbContext`由实体框架提供的基类。

若要引用`DbContext`和`DbSet`，你需要添加以下`using`语句文件的顶部：

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

完整*Movie.cs*文件如下所示。 (多个使用不是语句需要已删除。)

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>创建连接字符串和使用 SQL Server LocalDB

`MovieDBContext`你创建的类处理任务连接到数据库并映射`Movie`数据库记录的对象。 一个问题可能会问，不过，是如何指定它将连接到的数据库。 将执行该操作通过添加中的连接信息*Web.config*应用程序文件。

打开应用程序根目录*Web.config*文件。 (不*Web.config*文件中*视图*文件夹。)打开*Web.config*用红色的文件。

![](adding-a-model/_static/image2.png)

添加到以下连接字符串`<connectionStrings>`中的元素*Web.config*文件。

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

下面的示例显示的一部分*Web.config*文件以添加新的连接字符串：

[!code-xml[Main](adding-a-model/samples/sample6.xml?highlight=6-9)]

这种少量的代码和 XML 是你需要编写以便表示，并将影片数据存储在数据库中的所有内容。

接下来，你将生成一个新`MoviesController`类，该类可以用于显示影片数据，并允许用户创建新的影片列表。

>[!div class="step-by-step"]
[上一页](adding-a-view.md)
[下一页](accessing-your-models-data-from-a-controller.md)
