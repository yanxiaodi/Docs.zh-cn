---
title: "生成使用 Bootstrap 和 ASP.NET 核心美观、 响应迅速站点"
author: ardalis
description: "了解如何开发使用 ASP.NET Core 的响应式 web 应用程序使用 Bootstrap。"
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/bootstrap
ms.openlocfilehash: c3dfaa53e9e3277d025d014f65004e4c24a5acc4
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
# <a name="building-beautiful-responsive-sites-with-bootstrap-and-aspnet-core"></a>生成使用 Bootstrap 和 ASP.NET 核心美观、 响应迅速站点

<a name="bootstrap-index"></a>

作者：[Steve Smith](https://ardalis.com/)

Bootstrap 目前最常用的 web 框架开发响应式 web 应用程序。 无论您是在前端设计和开发或方面的专家新手，它提供大量的功能和优势，这样可以提高您的网站，用户的体验。 Bootstrap 部署为一组 CSS 和 JavaScript 文件，并旨在从手机有效地帮助你的网站或应用程序缩放，平板电脑和桌面。

## <a name="get-started"></a>入门

有多种，若要开始使用 Bootstrap。 如果你在 Visual Studio 中开始新的 web 应用程序，则可以为 ASP.NET Core，区分大小的 Bootstrap 会预安装选择默认初学者模板：

![在初学者模板解决方案视图中启动](bootstrap/_static/bootstrap-in-starter-template.png)

添加 ASP.NET Core Bootstrap 项目是只需将其添加到*bower.json*作为依赖项：

[!code-json[](../common/samples/WebApplication1/bower.json?highlight=5)]

这是 Bootstrap 添加到 ASP.NET 核心项目的建议的方法。

你还可以安装使用多个包管理器，例如 Bower、 npm 或 NuGet 之一的 bootstrap。 在每个情况下，该过程是实质上是相同的：

### <a name="bower"></a>Bower

```console
bower install bootstrap
```

### <a name="npm"></a>npm

```console
npm install bootstrap
```

### <a name="nuget"></a>NuGet

```console
Install-Package bootstrap
```

> [!NOTE]
> 安装客户端依赖关系，如 ASP.NET Core 中的 Bootstrap 为通过 Bower 的建议的方法 (使用*bower.json*，如上所示)。 使用 npm/NuGet 显示来演示如何轻松地 Bootstrap 可以添加到其他类型的 web 应用程序，包括 ASP.NET 的早期版本。

如果您要引用的 Bootstrap 您自己的本地版本，你将需要在将使用它的所有页中引用它们。 在生产中应引用使用 CDN 的 bootstrap。 在默认 ASP.NET 站点模板中， *_Layout.cshtml*文件因此像这样：

[!code-html[](../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=9,13,51,59)]

> [!NOTE]
> 如果你要使用的任何 Bootstrap 的 jQuery 插件，你将需要引用 jQuery。

## <a name="basic-templates-and-features"></a>基本的模板和功能

最基本启动模板看起来非常相似*_Layout.cshtml*所示的文件更高版本，并只包括基本菜单用于导航和呈现页面的其余部分的一个位置。

### <a name="basic-navigation"></a>基本导航

默认模板使用的一组`<div>`元素来呈现顶部导航栏和页的正文。 如果你使用 HTML5，则可以将第一个`<div>`使用标记`<nav>`标记来获取相同的效果，但具有更精确的语义。 此第一部分中`<div>`可以看到有多个其他人。 首先，`<div>`与类的"容器"，然后在中，两个多`<div>`元素:"导航条标头"和"导航栏折叠"。 导航条标头 div 包括时一定最小宽度，显示 3 水平行下面的屏幕，将出现一个按钮 (所谓"汉堡图标")。 使用纯 HTML 和 CSS; 呈现图标没有图像是必需的。 这是显示的图标，与每个代码<span>标记呈现白色条之一：

```html
<button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse">
    <span class="icon-bar"></span>
    <span class="icon-bar"></span>
    <span class="icon-bar"></span>
</button>
```

它还包括应用程序名称，将显示在左上角。 由呈现的主导航菜单`<ul>`中的第二个 div 元素并包括指向链接为主页，关于，和联系人。 通过在行 29 _LoginPartial 行添加注册和登录名的其他链接。 下面的导航窗格中，主正文中的每个页面呈现在另一个`<div>`、 标记与"容器"和"正文内容"类。 在此处显示简单的默认 _Layout 文件中，页的内容所呈现页面上，，然后选择一个简单与关联的特定视图`<footer>`添加到末尾`<div>`元素。 你可以看到有关页面内置将显示使用此模板：

![有关页面](bootstrap/_static/about-page-wide.png)

窗口低于一定宽度时，将显示折叠的导航栏中，与在右上角，"汉堡"按钮：

![有关与汉堡菜单的页面](bootstrap/_static/about-page-hamburger.png)

单击图标将显示从页面顶部向下滑垂直抽屉中的菜单项：

![有关与汉堡菜单展开页面](bootstrap/_static/about-page-hamburger-open.png)

### <a name="typography-and-links"></a>版式和链接

Bootstrap 设置了站点的基本版式、 颜色和其 CSS 文件中的格式设置的链接。 此 CSS 文件包括表、 按钮、 窗体元素、 图像和的详细信息的默认样式 ([了解](http://getbootstrap.com/css/))。 一个特别有用功能是，接下来涵盖的网格布局系统。

### <a name="grids"></a>网格

Bootstrap 的最流行的功能之一是其网格布局系统。 现代 web 应用程序应避免使用`<table>`布局，而将此元素的用途限制为实际的表格数据的标记。 相反，列和行可以进行布局使用一系列`<div>`元素和相应的 CSS 类。 有许多好处包括能够调整以显示垂直屏幕窄，如在手机上的网格布局，这种方法。

[Bootstrap 的网格布局系统](http://getbootstrap.com/css/#grid)基于 12 个列。 已选择此数字，原因是可以为 1、 2、 3 或 4 列均匀划分和到之间变化的列宽可以在 1/12 的垂直屏幕的宽度。 若要开始使用网格布局系统，你应该开始使用容器`<div>`，然后添加行`<div>`，如下所示：

```html
<div class="container">
    <div class="row">
        ...
    </div>
</div>
```

接下来，添加其他`<div>`元素为每个列，并指定的列数，`<div>`应占用 （带 12)"列-md-"开头的 CSS 类的一部分。 例如，如果你想要只具有大小相等的两个列，你将为每个使用"列-md-6"的类。 在这种情况下"md"是短，无法用于"medium"并引用标准尺寸台式计算机的显示大小。 有四个不同选项，你可以选择从，和每个将使用更高版本的宽度除非重写 （因此，如果你想要无论屏幕宽度固定的布局，你可以只需指定 xs 类）。

CSS 类前缀 | 设备层 | 宽度
:---: | :---: | :---:
col-xs- | 手机 | < 768px
col-sm- | 平板电脑 | >= 768px
col-md- | 桌面 | >= 992px
col-lg- | 更大的桌面显示 | >= 1200px

指定的两个列这两个"列-md-6"生成的布局中不会在桌面的解决方法的两个列，但较小的设备 （或在桌面上较窄的浏览器窗口），允许用户轻松地查看上呈现时，这两列将垂直堆叠时而无需水平滚动的内容。

Bootstrap 始终默认为单列布局，以便只需指定列，如果希望多个列。 你想要显式指定的唯一时间`<div>`占用所有 12 列可以重写更大的设备层的行为。 在指定多个设备层类时，你可能需要重置在某些点列呈现。 添加只会显示某些视区内 clearfix div 可以实现此目的，如下所示：

![窄和宽视区网格](bootstrap/_static/narrow-and-wide-viewport-grid.png)

在上面的示例中，一个和第二个共享中行"md"布局中，而两个和第三共享"xs"布局中的行。 而无需 clearfix `<div>`，2 和 3 中未显示正确的"xs"视图 （请注意，只有一个、 4 和 5 所示）：

![而无需使用 clearfix 网格](bootstrap/_static/grid-without-clearfix.png)

在此示例中，只有一行`<div>`所使用的并且启动仍然主要未正确的操作方面的布局和堆叠的列。 通常情况下，应指定行`<div>`对于每个水平行布局要求，和当然可以嵌套在另一个的 Bootstrap 网格。 执行操作时，每个嵌套的网格将占用 100%的元素在其中放置它，然后可以通过使用列类细分的宽度。

### <a name="jumbotron"></a>Jumbotron

如果你已使用 Visual Studio 2012 或 2013年中的默认 ASP.NET MVC 模板，你可能已了解 Jumbotron 操作中。 它将引用到大型全角部分可以用于显示较大的背景图像，操作、 旋转或类似的元素调用的页。 若要添加到页面 jumbotron，只需添加`<div>`并为其提供的类"jumbotron"，然后放置容器`<div>`内并添加你的内容。 我们可以轻松地调整有关页后，可以使用它显示的主要标题 jumbotron 标准：

![jumbotron 示例](bootstrap/_static/jumbotron.png)

### <a name="buttons"></a>按钮

在下图中显示的默认按钮类和它们的颜色。

![主题的按钮](bootstrap/_static/theme-buttons.png)

### <a name="badges"></a>徽章

徽章是指导航项旁边的小，通常为数值标注。 它们可以指明大量消息或通知等待或更新的状态。 指定此类徽章非常简单，只添加<span>包含文本，与"徽章"的类：

![主题徽章](bootstrap/_static/theme-badges.png)

### <a name="alerts"></a>警报

你可能需要向应用程序的用户显示通知、 警报或错误消息的某种类型。 这是标准的警报类是很有用。 有四个不同的严重级别与关联的颜色方案：

![主题的警报](bootstrap/_static/theme-alerts.png)

### <a name="navbars-and-menus"></a>Navbars 和菜单

我们布局已经包括标准的导航栏中，但是的 Bootstrap 主题支持其他样式选项。 我们也很容易可以选择垂直显示导航栏，而不是水平如果，具有首选，以及为添加的子导航中的项弹出菜单。 简单导航菜单，选项卡条带，如生成的顶部 <ul> 元素。 这些内容可以创建非常只需通过只需为他们提供的 CSS 类"导航"和"导航选项卡":

![主题 tabstrips](bootstrap/_static/theme-tabstrips.png)

Navbars 同样，内置，但较之前更复杂。 启动时出现`<nav>`或`<div>`"导航栏"容器 div 其中包含的元素的其余部分的类。 我们的页面导航栏在其标题中已包括 – 所示只需对此进行扩展，添加对下拉菜单中的支持：

![主题 navbars](bootstrap/_static/theme-navbars.png)

### <a name="additional-elements"></a>其他元素

此外可以使用默认主题在格式设置精美的样式，包括支持条带化的视图中呈现 HTML 表。 有类似的按钮的样式的标签。 你可以创建自定义支持的标准 HTML 之外的其他样式选项的下拉列表菜单`<select>`元素及其 Navbars 我们默认入门站点已在使用所示。 如果你需要一个进度栏，有几种样式可供选择，以及列出组和面板，包括标题和内容。 浏览标准的 Bootstrap 主题中的其他选项：

[http://getbootstrap.com/examples/theme/](http://getbootstrap.com/examples/theme/)

## <a name="more-themes"></a>更多主题

你可以通过重写某些或所有其 CSS，扩展的标准的 Bootstrap 主题调整颜色和样式，以满足自己的应用程序的需要。 如果你想要从头现成的主题，有几个主题库可用联机，在启动主题，例如 WrapBootstrap.com （其的各种商业主题） 和 Bootswatch.com （它提供了可用的主题） 的专用化。 某些付费的可用模板提供大量之上的基本的 Bootstrap 主题，如对管理菜单和仪表板的丰富支持的功能丰富的图表和仪表。 常用的付费模板示例目前将 Inspinia，$18，其中包括除了 AngularJS 和静态 HTML 版本的 ASP.NET MVC5 模板的销售。 示例屏幕快照所示。

![示例主题 inspinia](bootstrap/_static/theme-inspinia.png)

如果你想要更改您的 Bootstrap 主题，请将放*bootstrap.css*主题中所需的文件**wwwroot/css**文件夹并将更改中的引用*_Layout.cshtml*以将其指向。 更改所有环境的链接：

```html
<environment names="Development">
    <link rel="stylesheet" href="~/css/bootstrap.css" />
```

```html
<environment names="Staging,Production">
    <link rel="stylesheet" href="~/css/bootstrap.min.css" />
```

如果你想要生成你自己的仪表板，你可以从提供的免费示例从这里开始： [ http://getbootstrap.com/examples/dashboard/ ](http://getbootstrap.com/examples/dashboard/)。

## <a name="components"></a>组件数

Bootstrap 除了已经讨论了这些元素，包括对各种支持[内置 UI 组件](http://getbootstrap.com/components/)。

### <a name="glyphicons"></a>Glyphicons

Bootstrap 包括从 Glyphicons 图标集 ([http://glyphicons.com](http://glyphicons.com))，与超过 200 个自由可用于启用 Bootstrap 的 web 应用程序中使用的图标。 下面是只是一个小的示例：

![Glyphicons](bootstrap/_static/theme-glyphicons.png)

### <a name="input-groups"></a>输入的组

输入的组允许绑定的附加文本或按钮使用输入元素，从而为用户提供更直观的体验：

![输入的组](bootstrap/_static/input-groups.png)

### <a name="breadcrumbs"></a>痕迹导航

痕迹导航是用于显示用户，其最新历史记录或在站点的导航层次结构中的深度的常见 UI 组件。 通过将"痕迹导航"类应用于任何轻松地添加`<ol>`列表元素。 包括内置支持分页上使用"分页"类`<ul>`中的元素`<nav>`。 通过使用添加响应嵌入的幻灯片和视频`<iframe>`， `<embed>`， `<video>`，或`<object>`Bootstrap 将自动设置样式的元素。 通过使用特定的类，如"嵌入的响应性-16by9"中指定特定的纵横比。

## <a name="javascript-support"></a>JavaScript 支持

Bootstrap 的 JavaScript 库包括对包含的组件，使您可以控制其行为以编程方式在你的应用程序的 API 支持。 此外， *bootstrap.js*包括超过 12 个自定义 jQuery 插件，提供其他功能，例如转换，模式对话框，向下滚动 （更新样式基于用户具有滚动文档中的何处） 的检测，因此，它们不会滚动出屏幕折叠到窗口的行为、 行李传送带和将附加菜单。 没有足够的空间来涵盖所有 Bootstrap – 若要了解详细信息，请访问内置的 JavaScript 外接程序[ http://getbootstrap.com/javascript/ ](http://getbootstrap.com/javascript/)。

## <a name="summary"></a>摘要

Bootstrap 提供一个可用来快速和高效布局和样式各种网站和应用程序的 web 框架。 其基本版式和样式提供通过可以手工编写或商业上购买的自定义主题支持可以容易地操作获得愉快外观和感觉。 它支持的 web 组件在过去会要求昂贵的第三方控件，若要完成，同时支持现代和打开 web 标准的主机。
