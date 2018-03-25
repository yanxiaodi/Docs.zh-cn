---
title: "布局"
author: ardalis
description: 
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/layout
ms.openlocfilehash: 3e9e5949d8940a33508e24f0da015b49b7ba468c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="layout"></a>布局

作者：[Steve Smith](https://ardalis.com/)

视图经常共享可视和编程元素。 本文介绍如何在 ASP.NET 应用中呈现视图之前，使用通用布局、共享指令和运行常见代码。

## <a name="what-is-a-layout"></a>什么是布局

大多数 Web 应用都有一个通用布局，可在页面间切换时为用户提供一致体验。 该布局通常包括应用标头、导航或菜单元素以及页脚等常见的用户界面元素。

![页面布局示例](layout/_static/page-layout.png)

应用中的许多页面也经常使用脚本和样式表等常用的 HTML 结构。 所有这些共享元素均可在布局文件中进行定义，应用内使用的任何视图随后均可引用此文件。 布局可减少视图中的重复代码，帮助它们遵循[不要自我重复 (DRY) 原则](http://deviq.com/don-t-repeat-yourself/)。

按照约定，ASP.NET 应用的默认布局名为 `_Layout.cshtml`。 Visual Studio ASP.NET Core MVC 项目模板在 `Views/Shared` 文件夹中包含此布局文件：

![解决方案资源管理器中的视图文件夹](layout/_static/web-project-views.png)

此布局为应用中的视图定义顶级模板。 应用不需要布局，它们可以定义多个布局，并且不同的视图指定不同的布局。

示例 `_Layout.cshtml`：

[!code-html[Main](../../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=42,66)]

## <a name="specifying-a-layout"></a>指定布局

Razor 视图具有 `Layout` 属性。 单个视图通过设置此属性来指定布局：

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

指定的布局可以使用完整路径（例如：`/Views/Shared/_Layout.cshtml`）或部分名称（例如：`_Layout`）。 如果提供部分名称，Razor 视图引擎将使用其标准发现过程来搜索布局文件。 首先搜索与控制器关联的文件夹，然后搜索 `Shared` 文件夹。 此发现过程与用于发现[分部视图](partial.md)的过程相同。

默认情况下，每个布局必须调用 `RenderBody`。 无论在何处调用 `RenderBody`，都会呈现视图的内容。

<a name="layout-sections-label"></a>

### <a name="sections"></a>部分

布局可以通过调用 `RenderSection` 来选择引用一个或多个节。 节提供一种方法来组织某些页面元素应当放置的位置。 对 `RenderSection` 的每次调用均可指定该节是必需的还是可选的。 如果找不到所需的节，则会引发异常。 单个视图使用 `@section` Razor 语法指定要在节中呈现的内容。 如果视图定义某个节，则必须呈现该节（否则会发生错误）。

视图中的示例 `@section` 定义：

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
   }
   ```

在上面的代码中，验证脚本添加到了视图（包含窗体）上的 `scripts` 节。 同一应用程序中的其他视图可能不需要任何其他脚本，因此无需定义脚本节。

视图中定义的节仅在其即时布局页面中可用。 不能从部分、视图组件或视图系统的其他部分引用它们。

### <a name="ignoring-sections"></a>忽略节

默认情况下，必须由布局页面呈现内容页中的正文和所有节。 Razor 视图引擎通过跟踪是否已呈现正文和每个节来强制执行此操作。

要让视图引擎忽略正文或节，请调用 `IgnoreBody` 和 `IgnoreSection` 方法。

必须呈现或忽略 Razor 页面中的正文和每个节。

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a>导入共享指令

视图可以使用 Razor 指令来执行许多操作，例如导入命名空间或执行[依赖关系注入](dependency-injection.md)。 可在一个共同的 `_ViewImports.cshtml` 文件中指定由许多视图共享的指令。 `_ViewImports` 文件支持以下指令：

* `@addTagHelper`

* `@removeTagHelper`

* `@tagHelperPrefix`

* `@using`

* `@model`

* `@inherits`

* `@inject`

该文件不支持函数和节定义等其他 Razor 功能。

示例 `_ViewImports.cshtml` 文件：

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]

针对 ASP.NET Core MVC 应用的 `_ViewImports.cshtml` 文件通常放置在 `Views` 文件夹中。 `_ViewImports.cshtml` 文件可以放置在任何文件夹中，在这种情况下，它仅适用于该文件夹及其子文件夹中的视图。 由于从根级别开始处理 `_ViewImports` 文件，然后处理视图本身的位置之前的每个文件夹，因此在根级别指定的设置可能会覆盖在文件夹级别。

例如，如果根级别 `_ViewImports.cshtml` 文件指定 `@model` 和 `@addTagHelper`，在该视图的控制器关联文件夹中，另一个 `_ViewImports.cshtml` 文件指定不同的 `@model` 并添加另一个 `@addTagHelper`，该视图将有权访问这两个标记帮助程序，并将使用后一个 `@model`。

如果对一个视图运行多个 `_ViewImports.cshtml` 文件，那么包含在 `ViewImports.cshtml` 文件中的指令的组合行为如下所示：

* `@addTagHelper``@removeTagHelper`：按顺序全部运行

* `@tagHelperPrefix`：最接近视图的文件会替代任何其他文件

* `@model`：最接近视图的文件会替代任何其他文件

* `@inherits`：最接近视图的文件会替代任何其他文件

* `@using`：全部包括在内；忽略重复项

* `@inject`：针对每个属性，最接近视图的属性会替代具有相同属性名的任何其他属性

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a>在呈现每个视图之前运行代码

如果有需要在呈现每个视图之前运行的代码，应将其置于 `_ViewStart.cshtml` 文件中。 按照约定，`_ViewStart.cshtml` 文件位于 `Views` 文件夹中。 在呈现每个完整视图（不是布局，也不是分部视图）之前运行 `_ViewStart.cshtml` 中列出的语句。 与 [ViewImports.cshtml](xref:mvc/views/layout#viewimports) 一样，`_ViewStart.cshtml` 是分层的。 如果在控制器关联的视图文件夹中定义了 `_ViewStart.cshtml` 文件，则将在 `Views` 文件夹根目录中定义的文件（如有）之后运行该文件。

示例 `_ViewStart.cshtml` 文件：

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

上述文件指定所有视图都将使用 `_Layout.cshtml` 布局。

> [!NOTE]
> `_ViewStart.cshtml` 和 `_ViewImports.cshtml` 通常不会放置在 `/Views/Shared` 文件夹中。 这些应用级别版本的文件应直接放置在 `/Views` 文件夹中。
