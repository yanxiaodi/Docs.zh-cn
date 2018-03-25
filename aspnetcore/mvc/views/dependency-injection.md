---
title: "视图中的依赖关系注入"
author: ardalis
description: 
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/dependency-injection
ms.openlocfilehash: 690fdd0fd841341d17de48c0a8c9af121da220de
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="dependency-injection-into-views"></a>视图中的依赖关系注入

作者：[Steve Smith](https://ardalis.com/)

ASP.NET Core 支持将[依赖关系注入](xref:fundamentals/dependency-injection)到视图。 这对于视图特定服务很有用，例如仅为填充视图元素所需的本地化或数据。 应尽量在控制器和视图之间保持[问题分离](http://deviq.com/separation-of-concerns/)。 视图显示的大部分数据应该从控制器传入。

[查看或下载示例代码](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample)（[如何下载](xref:tutorials/index#how-to-download-a-sample)）

## <a name="a-simple-example"></a>简单示例

可使用 `@inject` 指令将服务注入到视图中。 可将 `@inject` 看作向视图添加属性，并用 DI 填充该属性。

`@inject` 的语法：`@inject <type> <name>`

操作中的 `@inject` 示例：

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/ToDo/Index.cshtml?highlight=4,5,15,16,17)]

此视图显示 `ToDoItem` 实例的列表，以及显示总体统计信息的摘要。 摘要从已注入的 `StatisticsService` 中填充。 在 Startup.cs 的 `ConfigureServices` 中为依赖关系注入注册此服务：

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Startup.cs?highlight=6,7&range=15-22)]

`StatisticsService` 通过存储库访问 `ToDoItem` 实例集并执行某些计算：

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/StatisticsService.cs?highlight=15,20,26)]

示例存储库使用内存中集合。 建议不将上示实现（对内存中的所有数据进行操作）用于远程访问的大型数据集。

该示例显示绑定到视图的模型数据以及注入到视图中的服务：

![列出项总数、已完成项、平均优先级以及任务列表（具有其优先级别和表示完成的布尔值）的“To Do”视图。](dependency-injection/_static/screenshot.png)

## <a name="populating-lookup-data"></a>填充查找数据

视图注入可用于填充 UI 元素（如下拉列表）中的选项。 请考虑这样的用户个人资料窗体，其中包含用于指定性别、状态和其他首选项的选项。 使用标准 MVC 方法呈现这样的窗体，需让控制器为每组选项请求数据访问服务，然后用要绑定的每组选项填充模型或 `ViewBag`。

另一种方法是将服务直接注入视图以获取选项。 这最大限度地减少了控制器所需的代码量，将此视图元素构造逻辑移入视图本身。 显示个人资料编辑窗体的控制器操作只需要传递个人资料实例的窗体：

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Controllers/ProfileController.cs?highlight=9,19)]

用于更新这些首选项的 HTML 窗体包括三个属性的下拉列表：

![用窗体更新个人资料视图，可输入姓名、性别、状态和喜爱的颜色。](dependency-injection/_static/updateprofile.png)

这些列表由已注入视图的服务填充：

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Profile/Index.cshtml?highlight=4,16,17,21,22,26,27)]

`ProfileOptionsService` 是 UI 级别的服务，旨在准确提供此窗体所需的数据：

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/ProfileOptionsService.cs?highlight=7,13,24)]

>[!TIP]
> 请记得在 Startup.cs 的 `ConfigureServices` 方法中注册要通过依赖关系注入请求的类型。

## <a name="overriding-services"></a>替代服务

除了注入新的服务之外，此方法也可用于替代以前在页面上注入的服务。 下图显示了第一个示例中使用的页面上的所有可用字段：

![类型化 @ symbol 列表 Html 上的 IntelliSense 上下文菜单, 组件, StatsService, 以及 Url 字段](dependency-injection/_static/razor-fields.png)

如你所见，包括默认字段 `Html`、`Component` 和 `Url`（以及我们注入的 `StatsService`）。 如果想用自己的 HTML 帮助程序替换默认的 HTML 帮助程序，可使用 `@inject` 轻松完成：

[!code-html[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Helper/Index.cshtml?highlight=3,11)]

如果想扩展现有的服务，用自己的方法继承或包装现有的实现时，只需使用此方法。

## <a name="see-also"></a>请参阅

* Simon Timms 的博客：[在视图中查找数据](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)
