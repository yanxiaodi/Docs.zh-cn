---
uid: single-page-application/overview/templates/hottowel-template
title: "热毛巾模板 |Microsoft 文档"
author: madskristensen
description: "HotTowel 模板"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/09/2013
ms.topic: article
ms.assetid: 75af2e17-6ed3-4d24-8ea1-bc340027c318
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/hottowel-template
msc.type: authoredcontent
ms.openlocfilehash: bfc6e2c884c422f44e8be5f4f29554ae86f7ecb6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="hot-towel-template"></a>热毛巾模板
====================
通过[Mads Kristensen](https://github.com/madskristensen)

> 热毛巾 MVC 模板中编写的 John 爸爸
> 
> 选择要下载的版本：
> 
> [Visual Studio 2012 的热毛巾 MVC 模板](https://visualstudiogallery.msdn.microsoft.com/1f68fbe8-b4e9-4968-9fd3-ddc7cbc52dca)
> 
> [Visual Studio 2013 的热毛巾 MVC 模板](https://visualstudiogallery.msdn.microsoft.com/1eb8780d-d522-4dcf-bf56-56f0eab305c2)


> 热毛巾： 由于你不想要转到没有 SPA ！


想要生成 SPA，但不能确定从何处着手？ 使用热毛巾和以秒为单位，你将能够 SPA 并在其上生成所需的所有工具 ！

热毛巾创建用于构建单页面应用程序 (SPA)，它使用 ASP.NET 的良好起点。 在初始状态下你它提供模块化结构代码、 视图导航、 数据绑定、 丰富的数据管理和简单但简洁的样式。 热毛巾提供使你可以专注于你的应用，没有管道生成 SPA，所需的所有内容。

> 了解有关生成从 SPA 详细[John 爸爸视频、 教程和 Pluralsight 课程](http://johnpapa.net/spa?vsix)。


## <a name="application-structure"></a>应用程序结构

热毛巾 SPA 提供应用程序文件夹，其中包含定义你的应用程序的 JavaScript 和 HTML 文件。

在应用程序文件夹中：

- Durandal
- 服务
- viewmodel
- 视图

应用程序文件夹包含模块的集合。 这些模块封装各种功能，并在其他模块中声明依赖项。 视图文件夹包含你的应用程序的 HTML，viewmodel 文件夹包含视图 （常用的 MVVM 模式） 的显示逻辑。 服务文件夹非常适合用于存放你的应用程序可能需要如 HTTP 数据检索或本地存储区交互任何公共服务。 很常见的多个 viewmodel 以重复使用从服务模块的代码。

## <a name="aspnet-mvc-server-side-application-structure"></a>ASP.NET MVC 服务器端应用程序结构

热毛巾基于熟悉且功能强大的 ASP.NET MVC 结构。

- 应用\_启动
- 内容
- 控制器
- 模型
- 脚本
- 视图

## <a name="featured-libraries"></a>特色的库

- ASP.NET MVC
- ASP.NET Web API
- ASP.NET Web 优化的绑定和缩减
- [Breeze.js](http://Breezejs.com) -丰富的数据管理
- [Durandal.js](http://Durandaljs.com) -导航和视图构成
- [Knockout.js](http://Knockoutjs.com) -数据绑定
- [Require.js](http://requirejs.org) -使用 AMD 和优化的模块化
- [Toastr.js](http://jpapa.me/c7toastr) -弹出消息
- [Twitter Bootstrap](http://twitter.github.com/bootstrap/) -可靠的 CSS 样式

## <a name="installing-via-the-visual-studio-2012-hot-towel-spa-template"></a>通过 Visual Studio 2012 热毛巾 SPA 模板安装

热毛巾只能作为 Visual Studio 2012 模板进行安装。 只需单击`File`  |  `New Project`选择`ASP.NET MVC 4 Web Application`。 然后选择热毛巾单页面应用程序"模板和运行 ！

## <a name="installing-via-the-nuget-package"></a>通过 NuGet 包安装

热毛巾也是 NuGet 包，它增加现有空 ASP.NET MVC 项目。 只需使用 Nuget 安装，然后运行 ！

[!code-powershell[Main](hottowel-template/samples/sample1.ps1)]

## <a name="how-do-i-build-on-hot-towel"></a>如何生成热毛巾？

只需开始添加的代码 ！

1. 添加你自己的服务器端代码，最好是实体框架和 WebAPI （这非常与 Breeze.js 棒的功能）
2. 添加到视图`App/views`文件夹
3. 添加到 viewmodel`App/viewmodels`文件夹
4. 将 HTML 和 Knockout 数据绑定添加到你新视图
5. 更新中的导航的路由`shell.js`

## <a name="walkthrough-of-the-htmljavascript"></a>HTML/JavaScript 的演练

### <a name="viewshottowelindexcshtml"></a>Views/HotTowel/index.cshtml

index.cshtml 是起始路由和视图的 MVC 应用程序。 它包含所有标准元标记、 css 链接和你希望 JavaScript 引用。 正文包含单个`<div>`这是所有内容 （视图） 将它们请求时放。 `@Scripts.Render` Require.js 用于运行中包含的应用程序的代码的入口点`main.js`文件。 提供的初始屏幕的目的是为了演示如何创建初始屏幕，而你的应用加载。

[!code-cshtml[Main](hottowel-template/samples/sample2.cshtml)]

### <a name="appmainjs"></a>App/main.js

`main.js`文件包含将运行就会立即加载你的应用程序的代码。 这是你想要定义导航路由、 设置视图，你启动和执行任何安装程序/引导如整理你的应用程序数据。

`main.js`文件定义了若干 durandal 的模块，以帮助启动应用程序启动。 定义语句可帮助解决模块依赖项，因此它们可用于该函数。 首先启用调试消息，其中发送有关哪些事件的消息应用程序执行到控制台窗口。 在 app.start 代码指示 durandal 框架在启动应用程序。 以便 durandal 知道所有视图和 viewmodel 分别包含在同一个命名的文件夹中，设置约定。 最后，`app.setRoot`启动加载`shell`使用预定义`entrance`动画。

[!code-javascript[Main](hottowel-template/samples/sample3.js)]

## <a name="views"></a>视图

视图位于`App/views`文件夹。

### <a name="shellhtml"></a>shell.html

`shell.html`包含位于母版布局的 HTML。 将方的某处组成你其他视图的所有你`shell`视图。 热毛巾提供`shell`与此类的三个区域： 标头、 内容区域中和页脚。 每个地区以及加载内容窗体时请求其他视图。

`compose`绑定页眉和页脚会进行硬编码在热毛巾以指向`nav`和`footer`视图，分别。 Compose 绑定节的`#content`绑定到`router`模块的活动项。 换而言之，当你单击它是对应的视图的导航链接将在此区域中加载。

[!code-html[Main](hottowel-template/samples/sample4.html)]

### <a name="navhtml"></a>nav.html

`nav.html` SPA 包含导航链接。 这是菜单结构可在其中放置，例如。 这通常为被数据绑定 （使用 Knockout）`router`模块以显示你在中定义的导航`shell.js`。 Knockout 寻找数据绑定属性，并将绑定到`shell`viewmodel 若要显示的导航路由，以显示进度条 （使用 Twitter Bootstrap） 如果`router`模块正在执行从一个视图导航到另一个 （请参阅`router.isNavigating`).

[!code-html[Main](hottowel-template/samples/sample5.html)]

### <a name="homehtml-and-detailshtml"></a>home.html 和 details.html

对于自定义视图，这些视图包含 HTML。 当`home`中链接`nav`单击视图的菜单后，`home`视图将放置在内容区域的`shell`视图。 这些视图可以扩充或替换为你自己的自定义视图。

### <a name="footerhtml"></a>footer.html

`footer.html`包含显示在页脚中，在底部的 HTML`shell`视图。

## <a name="viewmodels"></a>Viewmodel

在中找到 Viewmodel`App/viewmodels`文件夹。

### <a name="shelljs"></a>shell.js

`shell` Viewmodel 包含属性和绑定到的函数`shell`视图。 这通常是可找到菜单导航绑定 (请参阅`router.mapNav`逻辑)。

[!code-javascript[Main](hottowel-template/samples/sample6.js)]

### <a name="homejs-and-detailsjs"></a>home.js 和 details.js

这些 viewmodel 包含的属性和绑定到的函数`home`视图。 它还包含表示逻辑对于视图，并且在数据与视图之间起来的纽带。

[!code-javascript[Main](hottowel-template/samples/sample7.js)]

## <a name="services"></a>服务

在应用程序/服务文件夹中找到的服务。 理想情况下无法放 dataservice 模块，负责用于获取和发布远程数据，如你将来的服务。

### <a name="loggerjs"></a>logger.js

热毛巾提供`logger`服务文件夹中的模块。 `logger`模块非常适合于日志记录消息到控制台和弹出 toast 中的用户。
