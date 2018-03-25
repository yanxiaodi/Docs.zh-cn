---
uid: mvc/overview/releases/mvc51-release-notes
title: "什么是 ASP.NET MVC 5.1 中的新增功能 |Microsoft 文档"
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 9a83a058-9b01-48aa-acce-ec041e694567
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/releases/mvc51-release-notes
msc.type: authoredcontent
ms.openlocfilehash: be10486c9fd39738f44cdda4fedb409058017601
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-mvc-51"></a>什么是 ASP.NET MVC 5.1 中的新增功能
====================
通过[Microsoft](https://github.com/microsoft)

本主题介绍什么是用于 ASP.NET Web MVC 5.1 的新功能。

- [软件要求](#SoftwareRequirements)
- [下载](#download)
- [文档](#documentation)
- [ASP.NET MVC 5.1 中的新增功能](#new-features)

    - [属性路由改进](#AttributeRouting)
    - [Bootstrap 支持编辑器模板](#Bootstrap)
    - [在视图中的枚举支持](#Enum)
    - [非介入式 MinLength/MaxLength 属性验证](#Unobtrusive)
    - [支持在非介入式 Ajax 中的 this 上下文](#thisContext)
- [已知问题和重大更改](#KnownBreakingChanges)- [Bug 修复](#bug-fixes)

<a id="SoftwareRequirements"></a>
## <a name="software-requirements"></a>软件要求

- Visual Studio 2012： 下载[ASP.NET 和 Web Tools for Visual Studio 2012 2013.1](https://go.microsoft.com/fwlink/?LinkId=390062)。
- Visual Studio 2013： 下载[Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=390064)。 编辑 ASP.NET MVC 5.1 Razor 视图需要此更新。

<a id="download"></a>
## <a name="download"></a>下载

运行时功能将发布为 NuGet 库上的 NuGet 包。 所有运行时包按照[语义版本控制](http://semver.org/)规范。 最新的 ASP.NET MVC 5.1 RTM 包具有以下版本:"5.1.2"。 你可以安装或更新这些包通过[NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/)。 版本还包括在 NuGet 上的相应本地化的包。

你可以安装或使用 NuGet 程序包管理器控制台更新到已发布的 NuGet 程序包：

[!code-console[Main](mvc51-release-notes/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>文档

ASP.NET web 站点 (https://www.asp.net) 提供了教程和有关 ASP.NET MVC 5.1 RTM 的其他信息。 

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-51"></a>ASP.NET MVC 5.1 中的新增功能

<a id="AttributeRouting"></a>

### <a name="attribute-routing-improvements"></a>属性路由改进

 基于路由选择路由现在支持约束，启用版本控制和标头的属性。 现可通过自定义的属性路由的许多方面`IDirectRouteFactory`接口和`RouteFactoryAttribute`类。 路由前缀现在是可扩展的通过`IRoutePrefix`接口和`RoutePrefixAttribute`类。 

<a id="Enum"></a>

### <a name="enum-support-in-views"></a>在视图中的枚举支持

1. 新`@Html.EnumDropDownListFor()`帮助器方法。 这些应使用与大多数表达式的计算结果必须为需要注意的 HTML 帮助器[枚举](https://msdn.microsoft.com/en-us/library/cc138362.aspx)类型或[可以为 Null&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx)其中`T`是[枚举](https://msdn.microsoft.com/en-us/library/cc138362.aspx)类型。 使用`EnumHelper.IsValidForEnumHelper()`检查这些要求。
2. 新`EnumHelper.GetSelectList()`返回方法`IList<SelectListItem>`。 这是有用的需要进行操作之前调用，例如，选择列表时`@Html.DropDownListFor()`，当您希望显示的名称或其`@Html.EnumDropDownListFor()`显示。

下面的代码演示了这些 Api。

[!code-cshtml[Main](mvc51-release-notes/samples/sample2.cshtml)]

你可以看到的完整示例[此处](https://aspnet.codeplex.com/SourceControl/latest#Samples/MVC/EnumSample/)。

<a id="Bootstrap"></a>

### <a name="bootstrap-support-for-editor-templates"></a>Bootstrap 支持编辑器模板

我们现在允许在中的 HTML 特性中使用传递[EditorFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(v=vs.100).aspx)作为[匿名对象](https://msdn.microsoft.com/en-us/library/bb397696.aspx)。

例如: 

[!code-cshtml[Main](mvc51-release-notes/samples/sample3.cshtml)]

<a id="Unobtrusive"></a>

### <a name="unobtrusive-validation-for-minlengthattribute-and-maxlengthattribute"></a>MinLengthAttribute 和 MaxLengthAttribute 非介入式验证

只有在为使用修饰属性，现在支持字符串和数组类型的客户端验证[MinLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.minlengthattribute(v=vs.110).aspx)和[MaxLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.maxlengthattribute(v=vs.110).aspx)属性。

<a id="thisContext"></a>

### <a name="supporting-the-this-context-in-unobtrusive-ajax"></a>支持在非介入式 Ajax 中的 this 上下文

回调函数 (`OnBegin, OnComplete, OnFailure, OnSuccess`) 现在能够找到调用元素通过`this`上下文。 例如: 

[!code-html[Main](mvc51-release-notes/samples/sample4.html)]

<a id="KnownBreakingChanges"></a>

## <a name="known-issues-and-breaking-changes"></a>已知的问题和重大更改

### <a name="attribute-routing"></a>属性路由

在属性路由匹配项中的多义性现在将报告错误，而不是无需选择第一个匹配项。

属性路由禁止使用`{controller}`参数，并从使用`{action}`路由的参数放置在操作上。 使用这些参数会很有可能导致多义性。 

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a>到 5.1 包在中使用结果尚不存在项目中的那些 5.0 包项目的基架 MVC/Web API

有关 ASP.NET MVC 5.1 RTM 更新 NuGet 程序包不会更新 Visual Studio 工具，例如 ASP.NET 基架或 ASP.NET Web 应用程序项目模板。 它们使用以前版本的 ASP.NET 运行时包 (5.0.0.0)。 因此，ASP.NET 基架将安装所需的包的以前版本 (5.0.0.0)，如果它们尚不存在在你项目中可用。 但是，在 Visual Studio 2013 RTM 或 Update 1 中的 ASP.NET 基架不会覆盖你的项目中的最新包。 如果您使用 ASP.NET 后更新你的项目的包到 Web API 2.1 或 ASP.NET MVC 5.1 的基架，请确保 Web API 和 ASP.NET MVC 的版本都一致。 

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a>Visual Studio 2013 中的 Razor 视图的语法突出显示

如果你更新到 ASP.NET MVC 5.1 RTM 而无需更新 Visual Studio 2013，你将获取 Visual Studio 编辑器支持语法突出显示编辑 Razor 视图时。 你将需要更新 Visual Studio 2013，若要获得此支持。 

### <a name="type-renames"></a>类型重命名

一些用于属性路由扩展的类型将在 5.1 RTM 中重命名。

| **旧的类型名称 (5.1 RC)** | **新的类型名称 (5.1 RTM)** |
| --- | --- |
| IDirectRouteProvider | IDirectRouteFactory |
| RouteProviderAttribute | RouteFactoryAttribute |
| DirectRouteProviderContext | DirectRouteFactoryContext |

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a>Bug 修复

此版本还包括多项 bug 修复。 您可以找到完整的列表：

- [5.1.0 包](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [5.1.1 包](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.1.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

5.1.2 包包含 IntelliSense 更新但没有 bug 修复。
