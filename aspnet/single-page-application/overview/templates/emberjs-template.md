---
uid: single-page-application/overview/templates/emberjs-template
title: "EmberJS 模板 |Microsoft 文档"
author: xqiu
description: "EmberJS 模板"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/30/2013
ms.topic: article
ms.assetid: 04d5f142-5f62-494a-b5ea-4f3d068d34cb
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/emberjs-template
msc.type: authoredcontent
ms.openlocfilehash: 1fb7633aee288be648d4f9681b43c8911b7dbab9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="emberjs-template"></a>EmberJS 模板
====================
通过[Xinyang Qiu](https://github.com/xqiu)

> EmberJS MVC 模板是由 Nathan totten 所写、 Thiago Santos 和 Xinyang Qiu 写入。
> 
> [下载 EmberJS MVC 模板](https://go.microsoft.com/fwlink/?LinkId=282647)


EmberJS SPA 模板旨在让您快速构建使用 EmberJS 的客户端的交互式 web 应用。

"单页面应用程序"(SPA) 是常规的 web 应用程序加载单个 HTML 页，然后而不是加载新页面，动态地更新页术语。 初始页加载后, SPA 介绍与通过 AJAX 请求服务器。

![](emberjs-template/_static/image1.png)

AJAX 已是老生常谈，但目前有更加轻松地生成和维护一个大型的复杂的 SPA 应用程序的 JavaScript 框架。 此外，html5 和 CSS3 使其更方便地创建丰富的用户界面。

EmberJS SPA 模板使用[Ember](http://emberjs.com/) JavaScript 库来处理从 AJAX 请求的页更新。 Ember.js 使用数据绑定来使用最新的数据同步页。 这样一来，你无需编写任何代码，它将指导完成的 JSON 数据并更新 DOM 相反，你可以声明性属性置于告诉 Ember.js 如何将数据呈现的 HTML。

在服务器端，EmberJS 模板是几乎与[KnockoutJS SPA 模板](../introduction/knockoutjs-template.md)。 它使用 ASP.NET MVC 为 HTML 文档和 ASP.NET Web API，以处理来自客户端 AJAX 请求提供服务。 有关模板的这些方面的问题的详细信息，请参阅[KnockoutJS 模板](../introduction/knockoutjs-template.md)文档。 本主题重点介绍 Knockout 模板和 EmberJS 模板之间的差异。

## <a name="create-an-emberjs-spa-template-project"></a>创建 EmberJS SPA 模板项目

下载并安装该模板通过单击上面的下载按钮。 你可能需要重新启动 Visual Studio。

在**模板**窗格中，选择**已安装的模板**展开**Visual C#**节点。 下**Visual C#**，选择**Web**。 在项目模板列表中，选择**ASP.NET MVC 4 Web 应用程序**。 命名该项目并单击**确定**。

![](emberjs-template/_static/image2.png)

在**新项目**向导中，选择**Ember.js SPA 项目**。

![](emberjs-template/_static/image4.png)

## <a name="emberjs-spa-template-overview"></a>EmberJS SPA 模板概述

EmberJS 模板使用 jQuery，Ember.js，Handlebars.js 创建平滑的交互式 UI 的组合。

Ember.js 是一个 JavaScript 库，使用客户端 MVC 模式。

- A*模板*、 在车模板化语言中，写入描述应用程序用户界面。 在发布模式下，[车编译器](https://github.com/Myslik/csharp-ember-handlebars)用于捆绑在一起并编译车模板。
- A*模型*将它从 （ToDo 列表和 ToDo 项） 的服务器获取的应用程序数据存储。
- A*控制器*存储应用程序状态。 控制器通常存在模型数据复制到相应的模板。
- A*视图*转换从应用程序的基元事件并将传递到控制器。
- A*路由器*管理应用程序状态，从而 Url 和模板同步。

此外，Ember 数据库可用于同步 （通过 RESTful API 从服务器获取） 的 JSON 对象和客户端模型。

EmberJS SPA 模板将脚本整理为八个层：

- webapi\_adapter.js、 webapi\_serializer.js： 扩展 Ember 数据库为使用 ASP.NET Web API。
- Scripts/helpers.js： 定义新 Ember 车帮助器。
- Scripts/app.js： 创建应用程序，并配置适配器和序列化程序。
- 脚本/应用/模型/\*.js： 定义模型。
- 脚本/应用程序/视图/\*.js： 定义的视图。
- 脚本/应用程序/控制器/\*.js： 定义控制器。
- 脚本/应用程序/routes，Scripts/app/router.js： 定义路由。
- 模板 /\*.hbs： 定义车模板。

让我们看看其中一些更详细地这些脚本。

## <a name="models"></a>模型

在脚本 / 应用 / 模型文件夹中定义模型。 有两个模型文件： todoItem.js 和 todoList.js。

**todo.model.js**定义待办事项列表客户端 （浏览器） 模型。 有两个模型类： todoItem 和 todoList。 在 Ember，模型是 DS 的子类。模型。 模型可以具有属性的属性：

[!code-javascript[Main](emberjs-template/samples/sample1.js)]

模型可以与其他模型中定义的关系：

[!code-css[Main](emberjs-template/samples/sample2.css)]

模型可以都计算将绑定到其他属性的属性：

[!code-javascript[Main](emberjs-template/samples/sample3.js)]

模型可以观察到的属性更改时调用的观察程序函数：

[!code-javascript[Main](emberjs-template/samples/sample4.js)]

## <a name="views"></a>视图

视图定义中的脚本/应用程序/views 文件夹中。 视图将应用程序 UI 中的事件。 事件处理程序可以回叫控制器函数，或只需直接调用的数据上下文。

例如，下面的代码是从 views/TodoItemEditView.js。 它定义的事件处理的输入的文本字段。

[!code-javascript[Main](emberjs-template/samples/sample5.js)]

## <a name="controller"></a>控制器

控制器定义脚本/应用程序/controllers 文件夹中。 若要表示的一个模型，扩展`Ember.ObjectController`:

[!code-javascript[Main](emberjs-template/samples/sample6.js)]

控制器还可以通过扩展表示的模型集合`Ember.ArrayController`。 例如，TodoListController 表示数组的`todoList`对象。 控制器按 todoList ID，按降序进行排序：

[!code-javascript[Main](emberjs-template/samples/sample7.js)]

控制器定义一个名为函数`addTodoList`，它创建新的 todoList 并将其添加到数组。 若要查看如何调用此函数获取，打开名为 todoListTemplate.html，模板文件夹中的模板文件。 下面的模板代码将绑定到按钮`addTodoList`函数：

[!code-html[Main](emberjs-template/samples/sample8.html)]

控制器还包含`error`属性，其中包含一条错误消息。 下面是要显示错误消息 （也在 todoListTemplate.html) 的模板代码：

[!code-html[Main](emberjs-template/samples/sample9.html)]

## <a name="routes"></a>路由

Router.js 定义路由和默认模板，以显示，设置应用程序状态，并为路由匹配 Url:

[!code-javascript[Main](emberjs-template/samples/sample10.js)]

TodoListRoute.js 为 TodoListRoute 中加载数据，通过重写 setupController 函数：

[!code-javascript[Main](emberjs-template/samples/sample11.js)]

Ember 使用命名约定来匹配 Url、 路由名称、 控制器和模板。 有关详细信息，请参阅[http://emberjs.com/guides/routing/defining-your-routes/](http://emberjs.com/guides/routing/defining-your-routes/)在 EmberJS 文档。

## <a name="templates"></a>模板

模板文件夹包含四个模板：

- application.hbs： 时启动应用程序呈现的默认模板。
- about.hbs:"/ 关于"路由的模板。
- index.hbs： 根模板"/"的路由。
- todoList.hbs： 的模板"/ todo"路由。
- \_navbar.hbs: 模板定义导航菜单。

应用程序模板的作用类似母版页。 它包含页眉、 页脚和"{{插座}}"可插入具体取决于路由中的其他模板。 有关 Ember 中的应用程序模板的详细信息，请参阅[http://guides.emberjs.com/v1.10.0/templates/the-application-template//](http://guides.emberjs.com/v1.10.0/templates/the-application-template/)。

"/ TodoList"模板包含两个循环表达式。 外部循环是`{{#each controller}}`，与内部循环是`{{#each todos}}`。 下面的代码演示内置`Ember.Checkbox`查看，自定义`App.TodoItemEditView`，以及使用的链接`deleteTodo`操作。

[!code-html[Main](emberjs-template/samples/sample12.html)]

`HtmlHelperExtensions` Controllers/HtmlHelperExensions.cs 中, 定义的类定义一个帮助器函数以缓存和插入模板文件时**调试**设置为**true** Web.config 文件中。 从 Views/Home/App.cshtml 中定义的 ASP.NET MVC 视图文件调用此函数：

[!code-cshtml[Main](emberjs-template/samples/sample13.cshtml)]

调用不带任何参数，该函数将呈现所有模板文件夹中的模板文件。 你还可以指定一个子文件夹或特定的模板文件。

当**调试**是**false**在 Web.config 中，该应用程序包括捆绑项"~/bundles/templates"。 BundleConfig.cs，使用车编译器库中添加此捆绑包项目：

[!code-csharp[Main](emberjs-template/samples/sample14.cs)]
