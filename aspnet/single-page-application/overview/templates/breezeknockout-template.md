---
uid: single-page-application/overview/templates/breezeknockout-template
title: "轻松/Knockout 模板 |Microsoft 文档"
author: madskristensen
description: "轻松/Knockout 单页面应用程序模板"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/30/2013
ms.topic: article
ms.assetid: 3bd94827-3c59-448f-abc3-36e6df4858db
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/breezeknockout-template
msc.type: authoredcontent
ms.openlocfilehash: 07ec099a0381458fe42c1972a2554f76fd34638c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="breezeknockout-template"></a>轻松/Knockout 模板
====================
通过[Mads Kristensen](https://github.com/madskristensen)

> 轻松/Knockout MVC 模板是编写的选区钟形
> 
> [下载轻松/Knockout MVC 模板](https://go.microsoft.com/fwlink/?LinkId=282649)


听说的"单页面应用程序"(SPA) 并想知道它是什么。 您无法阅读有关它，你将而是亲身体验。 但有时间来下载示例的人员？ 如果你具有 Visual Studio，你将示例 SPA 启动并使用 ASP.NET MVC 4"轻松/Knockout 单页面应用程序"模板运行在小于 60 秒。

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrRunning.png)

## <a name="what-is-the-breezeknockout-spa-template"></a>什么是轻松/Knockout SPA 模板？

大多数项目模板生成应用程序框架。 通过添加你的代码置于这些骨骼 flesh 和最终提供有效的应用程序。 轻松/Knockout SPA 模板是不同的。 它会生成用于你来研究的示例应用程序。 它演示了 SPA 应用程序设计和许多用于生成 SPA 的技术。

轻松/Knockout 模板上是一种变体[KnockoutJS SPA 模板](../introduction/knockoutjs-template.md)包含在 ASP.NET 和 Web Tools 2012.2 更新。 轻松 SPA 模板生成的应用程序相同的用户体验，但它具有不同的实现，轻松使用数据管理的。

KnockoutJS SPA 模板发出与原始 jQuery AJAX 的服务请求足以满足简单的应用程序。 但更复杂的应用程序必须满足更加严苛的数据管理要求。 例如，大多数应用程序：

- 查询并重新在扩展的用户会话期间查询服务器。
- 添加查询筛选器，排序和分页。
- 在多个屏幕之间共享相同的数据。
- 累积到多个对象的更改，然后将其保存为单个事务。
- 验证客户端上的更改，因此用户可以更改提交到数据库之前更正错误。

BreezeJS 库为你，释放你开发而言最为重要的应用程序逻辑和用户体验中处理这些日常任务。

[**轻松**](http://www.breezejs.com/?utm_source=ms-spa)是用于构建丰富的数据应用程序中 JavaScript 和 HTML，从历史上看已传递作为独立的桌面应用程序的应用类型的一个开源库。

轻松/Knockout 模板可帮助你执行的更可靠的数据管理基础结构向该第一个关键步骤。 它将生成的示例 Todo 应用程序与 KnockoutJS SPA 模板表面上是相同。 在内部，它替换轻松 AJAX 数据层，以便你可以比较两个接近并排显示。 当然，它只是涉及轻松应用程序的可能性。 但是，你将看到轻松的工作原理和多么少需要以使该转换。

让我们开始吧。

## <a name="create-a-breezeknockout-template-project"></a>创建一个轻松/Knockout 模板项目

下载并安装该模板通过单击上面的下载按钮。 模板被打包为 Visual Studio 扩展 (VSIX) 文件。 你可能需要重新启动 Visual Studio。

在**模板**窗格中，选择**已安装的模板**展开**Visual C#**节点。 下**Visual C#**，选择**Web**。 在项目模板列表中，选择**ASP.NET MVC 4 Web 应用程序**。 命名该项目并单击**确定**。

在**新项目**向导中，选择**轻松 Knockout SPA**。

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeKOSpaTemplate.png)

按 Ctrl + F5 生成并运行应用程序而不进行调试，或按 F5 边运行边调试。

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrRunning.png)

第一次运行应用程序，它会显示登录屏幕。 单击"注册"链接和新的页种到视图中，你可以在其中输入用户名和密码。 （登录名和注册页是使用 ASP.NET MVC 构建。）当提交注册表单时，服务器会生成你的帐户提供两个项目的 TodoList。 然后它将它们提供给你在上一个黄色的注释。

![](http://www.breezejs.com/sites/all/images/spa-template/TodoList.png)

现在你已在上关于领土的 SPA 中。 所有内容你看到和遇到时操作 Todos 呈现中，管理 Knockout 和轻松的帮助客户端上。 作为用户浏览应用程序... 但开发人员眼睛。 在浏览器中使用的开发人员工具来捕获网络流量。 (在 Internet Explorer： 按 f12 键，选择**网络**卡，然后单击**开始捕获**。)现在请尝试以下操作：

- 添加新的 Todo 项。
- 单击标签和编辑 Todo 项标题
- 选中复选框以将标记项完成。 请注意已禁用文本框中，所以标题不再可编辑。
- 单击右侧的标签的 'x'。 该项目将消失，从数据库中删除。
- 选择另一个项，并清除其标题。 你将获得标题需要一个验证错误。 简要暂停后，将还原以前的标题。
- 键入在可以极为长标题。 你将获得不同的验证错误的标题太长。
- 单击"添加 Todo 列表"按钮。 前面的列表左侧将显示新列表。
- 播放使用触发其所需的 TodoList 标题和长度验证。
- 单击在标题文本框中，若要清除的错误消息。
- 单击要删除任务列表和其 todos 右上角圆圈，圆圈中的"x"。

验证逻辑是通过轻松执行的客户端。 在的服务器模型类验证特性是传播到客户端，并且自动执行之前客户端与服务器联系。

查看网络流量。 请注意轻松检测到错误时的已连接到服务器的任何调用。 每个有效的更改导致 POST 请求为"/ api/Todo/SaveChanges"。 轻松将打包所做的更改并将其发送在一起以单个请求到 Web API 控制器`SaveChanges`方法。 这是不同于 KockoutJS SPA 模板，这使得 PUT、 POST 和单独删除每个项的请求。

## <a name="peek-inside"></a>在查看

此应用程序具有客户端和服务器端。 客户端堆栈组成少量 HTML 和应用程序 JavaScript 模块 （在"应用"文件夹中） 的组合以及第三方 JavaScript 库 （在"脚本"文件夹中）。

![](http://www.breezejs.com/sites/all/images/spa-template/ClientArchitecture.png)

如果你已调查 KnockoutJS SPA 模板，这应该非常熟悉。 专注于蓝色框。 UI 体系结构是模型-视图-视图模型 (MVVM)，在该视图的 HTML 小组件完全分隔从视图模型中支持的演示文稿代码。 数据绑定系统 (在此情况下 Knockout) 协调的视图和视图模型，以便每个可以完成其他直接不知情的情况下操作。

模型封装 Todo 数据。 模型中的实体构造通过轻松使用 Knockout 可观察属性，以便它们可以直接绑定到视图中的小组件。 视图模型要求数据上下文，以获取并保存模型实体。 数据上下文委托的大部分到轻松工作。

一些开发人员代码和三个原则.NET 库的服务器端堆栈组成： Web API、 实体框架和 Breeze.NET:

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

KockoutJS SPA 模板相同的基本体系结构。 但是，实现是要简单得多： Dto 已删除，并且大部分的实体框架的详细信息已委派给 Breeze.NET。

## <a name="next-steps"></a>后续步骤

我们建议你浏览代码中，由指导[全面讨论](http://www.breezejs.com/spa-template?utm_source=ms-spa)的客户端和服务器堆栈轻松网站上的。

你可以尝试 playing with 轻松客户端查询; 使用添加某些筛选器和排序。 你可以添加更多的模型属性和多个实体来更好地感受用于端到端 SPA 开发。 当你确信的设计时，你可以拉出 Todo 功能并将它们替换为你自己。

很快你将准备就绪供下一大步： 添加客户端屏幕和它们之间导航。 将留下此 SPA 模板并转向更完整的 SPA 堆栈，如[John 爸爸热毛巾](https://github.com/johnpapa/HotTowel#readme "热毛巾")，从而将 Durandal 添加到轻松和 Knockout 组合。
