---
title: "视图组件"
author: rick-anderson
description: "视图组件可用于具有可重用呈现逻辑的任何位置。"
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/view-components
ms.openlocfilehash: 27e77b8fa032c2b5be753a27db748b7499e27105
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="view-components"></a>视图组件

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

[查看或下载示例代码](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample)（[如何下载](xref:tutorials/index#how-to-download-a-sample)）

## <a name="introducing-view-components"></a>视图组件简介

作为 ASP.NET Core MVC 的新功能，视图组件与分部视图类似，但它们的功能更加强大。 视图组件不使用模型绑定，并且仅依赖调用时提供的数据。 视图组件：

* 呈现一个区块而不是整个响应。
* 包括控制器和视图间发现的相同关注点分离和可测试性优势。
* 可以有参数和业务逻辑。
* 通常从布局页调用。

视图组件可用于具有可重用呈现逻辑（对分部视图来说过于复杂）的任何位置，例如：

* 动态导航菜单
* 标记云（查询数据库的位置）
* 登录面板
* 购物车
* 最近发布的文章
* 典型博客上的边栏内容
* 一个登录面板，呈现在每页上并显示注销或登录链接，具体取决于用户的登录状态

视图组件由两部分组成：类（通常派生自 [ViewComponent](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponent)）及其返回的结果（通常为视图）。 与控制器一样，视图组件也可以是 POCO，但大多数开发人员都希望利用派生自 `ViewComponent` 的可用方法和属性。

## <a name="creating-a-view-component"></a>创建视图组件

本部分包含创建视图组件的高级别要求。 本文后续部分将详细检查每个步骤并创建视图组件。

### <a name="the-view-component-class"></a>视图组件类

可通过以下任一方法创建视图组件类：

* 从 ViewComponent 派生
* 使用 `[ViewComponent]` 属性修饰类，或者从具有 `[ViewComponent]` 属性的类派生
* 创建名称以 ViewComponent 后缀结尾的类

与控制器一样，视图组件必须是公共、非嵌套和非抽象的类。 视图组件名称是删除了“ViewComponent”后缀的类名。 也可以使用 `ViewComponentAttribute.Name` 属性显式指定它。

视图组件类：

* 完全支持构造函数[依赖关系注入](../../fundamentals/dependency-injection.md)

* 不参与控制器生命周期，这意味着不能在视图组件中使用[筛选器](../controllers/filters.md)

### <a name="view-component-methods"></a>视图组件方法

视图组件以返回 `InvokeAsync` 的 `IViewComponentResult` 方法定义其逻辑。 参数直接来自视图组件的调用，而不是来自模型绑定。 视图组件从不直接处理请求。 通常，视图组件通过调用 `View` 方法来初始化模型并将其传递到视图。 总之，视图组件方法：

* 定义返回 `IViewComponentResult` 的 `InvokeAsync` 方法
* 一般通过调用 `ViewComponent` `View` 方法来初始化模型并将其传递到视图
* 参数来自调用方法，而不是 HTTP，没有模型绑定
* 不可直接作为 HTTP 终结点访问，它们从代码调用（通常在视图中）。 视图组件从不处理请求
* 在签名上重载，而不是当前 HTTP 请求的任何详细信息

### <a name="view-search-path"></a>视图搜索路径

运行时在以下路径中搜索视图：

   * Views/\<controller_name>/Components/\<view_component_name>/\<view_name>
   * Views/Shared/Components/\<view_component_name>/\<view_name>

视图组件的默认视图名称为“默认”，这意味着视图文件通常命名为“Default.cshtml”。 可以在创建视图组件结果或调用 `View` 方法时指定不同的视图名称。

建议将视图文件命名为“Default.cshtml”并使用 Views/Shared/Components/\<view_component_name>/\<view_name> 路径。 此示例中使用的 `PriorityList` 视图组件对视图组件视图使用 Views/Shared/Components/PriorityList/Default.cshtml。

## <a name="invoking-a-view-component"></a>调用视图组件

要使用视图组件，请在视图中调用以下内容：

```cshtml
@Component.InvokeAsync("Name of view component", <anonymous type containing parameters>)
```

参数将传递给 `InvokeAsync` 方法。 本文中开发的 `PriorityList` 视图组件是从 Views/Todo/Index.cshtml 视图文件中调用的。 在下例中，使用两个参数调用 `InvokeAsync` 方法：

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

## <a name="invoking-a-view-component-as-a-tag-helper"></a>调用视图组件作为标记帮助程序

对于 ASP.NET Core 1.1 及更高版本，可以调用视图组件作为[标记帮助程序](xref:mvc/views/tag-helpers/intro)：

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

标记帮助程序采用 Pascal 大小写格式的类和方法参数将转换为各自相应的[小写短横线格式](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101)。 要调用视图组件的标记帮助程序使用 `<vc></vc>` 元素。 按如下方式指定视图组件：

```cshtml
<vc:[view-component-name]
  parameter1="parameter1 value"
  parameter2="parameter2 value">
</vc:[view-component-name]>
```

注意：为了将视图组件用作标记帮助程序，必须使用 `@addTagHelper` 指令注册包含视图组件的程序集。 例如，如果视图组件位于名为“MyWebApp”的程序集中，请将以下指令添加到 `_ViewImports.cshtml` 文件：

```cshtml
@addTagHelper *, MyWebApp
```

可将视图组件作为标记帮助程序注册到任何引用视图组件的文件。 要详细了解如何注册标记帮助程序，请参阅[管理标记帮助程序作用域](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope)。

本教程中使用的 `InvokeAsync` 方法：

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

在标记帮助程序标记中：

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

在以上示例中，`PriorityList` 视图组件变为 `priority-list`。 视图组件的参数作为小写短横线格式的属性进行传递。

### <a name="invoking-a-view-component-directly-from-a-controller"></a>从控制器直接调用视图组件

视图组件通常从视图调用，但你可以直接从控制器方法调用它们。 尽管视图组件不定义控制器等终结点，但你可以轻松实现返回 `ViewComponentResult` 内容的控制器操作。

在此示例中，视图组件直接从控制器调用：

[!code-csharp[Main](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

## <a name="walkthrough-creating-a-simple-view-component"></a>演练：创建简单的视图组件

[下载](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample)、生成和测试起始代码。 它是一个带有 `Todo` 控制器的简单项目，该控制器显示 Todo 项的列表。

![ToDo 列表](view-components/_static/2dos.png)

### <a name="add-a-viewcomponent-class"></a>添加 ViewComponent 类

创建一个 ViewComponents 文件夹并添加以下 `PriorityListViewComponent` 类：

[!code-csharp[Main](view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponent1.cs?name=snippet1)]

代码说明：

* 视图组件类可以包含在项目的任意文件夹中。
* 因为类名 PriorityListViewComponent 以后缀 ViewComponent 结尾，所以运行时将在从视图引用类组件时使用字符串“PriorityList”。 我稍后将进行详细解释。
* `[ViewComponent]` 属性可以更改用于引用视图组件的名称。 例如，我们可以将类命名为 `XYZ` 并应用 `ViewComponent` 属性：

  ```csharp
  [ViewComponent(Name = "PriorityList")]
     public class XYZ : ViewComponent
     ```

* 上面的 `[ViewComponent]` 属性通知视图组件选择器在查找与组件相关联的视图时使用名称 `PriorityList`，以及在从视图引用类组件时使用字符串“PriorityList”。 我稍后将进行详细解释。
* 组件使用[依赖关系注入](../../fundamentals/dependency-injection.md)以使数据上下文可用。
* `InvokeAsync` 公开可以从视图调用的方法，且可以采用任意数量的参数。
* `InvokeAsync` 方法返回满足 `isDone` 和 `maxPriority` 参数的 `ToDo` 项集。

### <a name="create-the-view-component-razor-view"></a>创建视图组件 Razor 视图

* 创建 Views/Shared/Components 文件夹。 此文件夹 **必须** 命名为 *Components*。

* 创建 Views/Shared/Components/PriorityList 文件夹。 此文件夹名称必须与视图组件类的名称或类名去掉后缀（如果遵照约定并在类名中使用了“ViewComponent”后缀）的名称相匹配。 如果使用了 `ViewComponent` 属性，则类名称需要匹配指定的属性。

* 创建 Views/Shared/Components/PriorityList/Default.cshtml Razor 视图：[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]
    
   Razor 视图获取并显示 `TodoItem` 列表。 如果视图组件 `InvokeAsync` 方法不传递视图名称（如示例中所示），则按照约定使用“默认”作为视图名称。 在本教程后面部分，我将演示如何传递视图名称。 要替代特定控制器的默认样式，请将视图添加到控制器特定的视图文件夹（例如 Views/Todo/Components/PriorityList/Default.cshtml）。
    
    如果视图组件是控制器特定的，则可将其添加到控制器特定的文件夹 (Views/Todo/Components/PriorityList/Default.cshtml)。

* 将包含优先级列表组件调用的 `div` 添加到 Views/Todo/index.cshtml 文件底部：

    [!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFirst.cshtml?range=34-38)]

标记 `@await Component.InvokeAsync` 显示调用视图组件的语法。 第一个参数是要调用的组件的名称。 后续参数将传递给该组件。 `InvokeAsync` 可以采用任意数量的参数。

测试应用。 下图显示 ToDo 列表和优先级项：

![Todo 列表和优先级项](view-components/_static/pi.png)

也可直接从控制器调用视图组件：

[!code-csharp[Main](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

![IndexVC 操作的优先级项](view-components/_static/indexvc.png)

### <a name="specifying-a-view-name"></a>指定视图名称

在某些情况下，复杂的视图组件可能需要指定非默认视图。 以下代码显示如何从 `InvokeAsync` 方法指定“PVC”视图。 更新 `PriorityListViewComponent` 类中的 `InvokeAsync` 方法。

[!code-csharp[Main](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponentFinal.cs?highlight=4,5,6,7,8,9&range=28-39)]

将 Views/Shared/Components/PriorityList/Default.cshtml 文件复制到名为 Views/Shared/Components/PriorityList/PVC.cshtml 的视图。 添加标题以指示正在使用 PVC 视图。

[!code-cshtml[Main](../../mvc/views/view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/PVC.cshtml?highlight=3)]

更新 Views/TodoList/Index.cshtml：

<!-- Views/TodoList/Index.cshtml is never imported, so change to test tutorial -->

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

运行应用并验证 PVC 视图。

![优先级视图组件](view-components/_static/pvc.png)

如果不呈现 PVC 视图，请验证是否调用优先级为 4 或更高的视图组件。

### <a name="examine-the-view-path"></a>检查视图路径

* 将优先级参数更改为 3 或更低，从而不返回优先级视图。
* 将 Views/Todo/Components/PriorityList/Default.cshtml 暂时重命名为 1Default.cshtml。
* 测试应用，你将收到以下错误：

   ```
   An unhandled exception occurred while processing the request.
   InvalidOperationException: The view 'Components/PriorityList/Default' wasn't found. The following locations were searched:
   /Views/ToDo/Components/PriorityList/Default.cshtml
   /Views/Shared/Components/PriorityList/Default.cshtml
   EnsureSuccessful
   ```

* 将 Views/Todo/Components/PriorityList/1Default.cshtml 复制到 Views/Shared/Components/PriorityList/Default.cshtml。
* 将一些标记添加到共享 Todo 视图组件视图以指示视图来自“Shared”文件夹。
* 测试“共享”组件视图。

![有共享组件视图的 ToDo 输出](view-components/_static/shared.png)

### <a name="avoiding-magic-strings"></a>避免魔幻字符串

若要确保编译时的安全性，可以用类名替换硬编码的视图组件名称。 创建没有“ViewComponent”后缀的视图组件：

[!code-csharp[Main](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityList.cs?highlight=10&range=5-35)]

将 `using` 语句添加到 Razor 视图文件，并使用 `nameof` 运算符：

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexNameof.cshtml?range=1-6,33-)]

## <a name="additional-resources"></a>其他资源

* [视图中的依赖关系注入](xref:mvc/views/dependency-injection)
