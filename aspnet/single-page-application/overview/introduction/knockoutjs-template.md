---
uid: single-page-application/overview/introduction/knockoutjs-template
title: "单页面应用程序： KnockoutJS 模板 |Microsoft 文档"
author: MikeWasson
description: "Knockout 模板"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/30/2013
ms.topic: article
ms.assetid: f9c07af0-4b20-4b08-af8f-47fc3df169a2
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/introduction/knockoutjs-template
msc.type: authoredcontent
ms.openlocfilehash: e6c0c45bed098a8a1160ff11e4f77244bf55ffd3
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
<a name="single-page-application-knockoutjs-template"></a>单页面应用程序： KnockoutJS 模板
====================
通过[Mike Wasson](https://github.com/MikeWasson)

> Knockout MVC 模板是 ASP.NET 和 Web Tools 2012.2 的一部分
> 
> [下载 ASP.NET 和 Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650)


ASP.NET 和 Web Tools 2012.2 更新包含 ASP.NET MVC 4 的单页面应用程序 (SPA) 模板。 此模板用于帮助你入门快速生成客户端的交互式 web 应用。

"单页面应用程序"(SPA) 是常规的 web 应用程序加载单个 HTML 页，然后而不是加载新页面，动态地更新页术语。 初始页加载后, SPA 介绍与通过 AJAX 请求服务器。

![](knockoutjs-template/_static/image1.png)

AJAX 已是老生常谈，但目前有更加轻松地生成和维护一个大型的复杂的 SPA 应用程序的 JavaScript 框架。 此外，html5 和 CSS3 使其更方便地创建丰富的用户界面。

为了帮助你入门，SPA 模板，请创建一个示例"待办事项列表"应用程序。 在本教程中，我们将该模板的指导的教程。 首先我们将查看待办事项列表应用程序本身，然后检查使其适用的技术部分。

## <a name="create-a-new-spa-template-project"></a>创建一个新的 SPA 模板项目

要求：

- Visual Studio 2012 or Visual Studio Express 2012 for Web
- ASP.NET Web Tools 2012.2 更新。 你可以在安装更新[此处](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2)。

启动 Visual Studio 并选择**新项目**从开始页。 或从**文件**菜单上，选择**新建**然后**项目**。

在**模板**窗格中，选择**已安装的模板**展开**Visual C#**节点。 下**Visual C#**，选择**Web**。 在项目模板列表中，选择**ASP.NET MVC 4 Web 应用程序**。 命名该项目并单击**确定**。

![](knockoutjs-template/_static/image2.png)

在**新项目**向导中，选择**单页面应用程序**。

![](knockoutjs-template/_static/image3.png)

按 F5 生成并运行该应用程序。 第一次运行应用程序，它会显示登录屏幕。

![](knockoutjs-template/_static/image4.png)

单击&quot;注册&quot;链接并创建新用户。

![](knockoutjs-template/_static/image5.png)

你在登录后，应用程序将创建具有以下两项默认 Todo 列表。 你可以单击"添加 Todo 列表"以添加新列表。

![](knockoutjs-template/_static/image6.png)

重命名列表，将项添加到列表中，并取消选中它们。 你还可以删除项或删除整个列表。 自动将所做的更改保存到服务器上的数据库 (实际 LocalDB 此时，因为正在本地运行应用程序)。

![](knockoutjs-template/_static/image7.png)

## <a name="architecture-of-the-spa-template"></a>SPA 模板的体系结构

下图显示应用程序主要构建基块。

![](knockoutjs-template/_static/image8.png)

在服务器端中，ASP.NET MVC 提供服务的 HTML，还可以处理基于窗体的身份验证。

ASP.NET Web API 处理与 ToDoLists 和 ToDoItems，包括获取、 创建、 更新和删除相关的所有请求。 客户端交换使用 Web API 以 JSON 格式的数据。

Entity Framework (EF) 是 O/RM 层。 它面向对象的世界上的 ASP.NET 和基础数据库之间担当中介。 数据库使用 LocalDB，但您可以在 Web.config 文件中更改此。 通常，你将以进行本地开发使用 LocalDB，并随后部署到 SQL 数据库在服务器上，使用 EF 代码优先迁移。

在客户端一侧，Knockout.js 库可处理从 AJAX 请求的页更新。 Knockout 使用数据绑定来使用最新的数据同步页。 这样一来，你无需编写任何代码，它将指导完成的 JSON 数据并更新 DOM 相反，你可以声明性属性置于告诉 Knockout 如何将数据呈现的 HTML。

此体系结构的大优点是它将表示层与应用程序逻辑分离。 你可以创建的 Web API 部分而不必知道有关 web 页面将看起来的任何信息。 在客户端上创建"视图模型"到表示该数据和视图模型使用 Knockout 将绑定到 HTML。 它允许你轻松地将 HTML 更改而无需更改视图模型。 （我们来看看 Knockout 有点更高版本。）

## <a name="models"></a>模型

在 Visual Studio 项目中，模型文件夹包含在服务器端使用的模型。 （客户端上还有模型; 我们将获取到那些。）

![](knockoutjs-template/_static/image9.png)

**TodoItem TodoList**

这些是用于 Entity Framework Code First 的数据库模型。 请注意这些模型具有指向对方的属性。 `ToDoList` 包含的 Todoitem，并且每个集合`ToDoItem`已返回到其父 ToDoList 的引用。 这些属性被称为导航属性，以及它们所表示的一对多关系待办事项列表和其待办事项。

`ToDoItem`类还使用**[ForeignKey]**特性来指定`ToDoListId`是到的外键`ToDoList`表。 这将告知 EF 将 foreign key 约束添加到数据库。

[!code-csharp[Main](knockoutjs-template/samples/sample1.cs)]

**TodoItemDto TodoListDto**

这些类定义将发送到客户端的数据。 "DTO"代表"数据传输对象"。 DTO 定义如何将实体序列化为 JSON。 一般情况下，有几个使用 Dto 的原因：

- 若要控制序列化的属性。 DTO 可以包含从域模型属性的子集。 你可能会出于安全原因 （以隐藏敏感数据） 或只需执行此来减少你发送的数据量。
- 若要更改形状的数据，例如以平展更复杂的数据结构。
- 若要保留 DTO （关注点分离） 之外的任何业务逻辑。
- 如果出于某种原因，域模型无法序列化。 例如，循环引用可能会导致问题在序列化有一个对象时如何处理此问题，Web API 中的 (请参阅[处理循环对象引用](../../../web-api/overview/formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references)); 但是使用 DTO 只是完全避免问题。

在 SPA 模板中，Dto 包含域模型相同的数据。 但是，它们是仍可因为它们避免循环引用导航属性，并且它们演示了一个常规模式，DTO。

**AccountModels.cs**

此文件包含站点成员身份的模型。 `UserProfile`类定义成员资格数据库中的用户配置文件的架构。 （在这种情况下，唯一信息是用户 ID 和用户名。）此文件中的其他模型类用于创建用户注册和登录窗体。

## <a name="entity-framework"></a>Entity Framework

SPA 模板使用 EF Code First。 在 Code First 开发中，你定义模型首先在代码中，而 EF 然后使用模型来创建数据库。 此外可以使用现有的数据库使用 EF ([Database First](https://msdn.microsoft.com/data/jj206878.aspx))。

`TodoItemContext`在 Models 文件夹中的类派生自**DbContext**。 此类提供"粘附"模型和 EF 之间。 `TodoItemContext`保存`ToDoItem`集合和一个`TodoList`集合。 若要查询数据库时，你只需编写针对这些集合的 LINQ 查询。 例如，下面是如何选择所有用户"Alice"待办事项列表：

[!code-csharp[Main](knockoutjs-template/samples/sample2.cs)]

你也可以向集合添加新项，更新项，或从集合中删除项和保留对数据库的更改。

## <a name="aspnet-web-api-controllers"></a>ASP.NET Web API 控制器

在 ASP.NET Web API 中，控制器在处理 HTTP 请求的对象。 如前文所述，SPA 模板使用 Web API 上启用 CRUD 操作`ToDoList`和`ToDoItem`实例。 控制器位于该解决方案的 Controllers 文件夹中。

![](knockoutjs-template/_static/image10.png)

- `TodoController`： 处理待办事项的 HTTP 请求
- `TodoListController`： 处理待办事项列表的 HTTP 请求。

因为 Web API 与控制器名称的 URI 路径相匹配，都有意义，这些名称。 (若要了解 Web API 将 HTTP 请求路由到控制器的方式，请参阅[路由在 ASP.NET Web API 中](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md)。)

让我们看一下`ToDoListController`类。 它包含单个数据成员：

[!code-csharp[Main](knockoutjs-template/samples/sample3.cs)]

`TodoItemContext`用于通信和 EF，如前面所述。 在控制器上的方法实现的 CRUD 操作。 Web API，如下所示映射到控制器的方法，从客户端的 HTTP 请求：

| HTTP 请求 | 控制器方法 | 描述 |
| --- | --- | --- |
| GET /api/todo | `GetTodoLists` | 获取待办事项列表的集合。 |
| GET/api/todo/*id* | `GetTodoList` | 获取按 ID 待办事项列表 |
| PUT/api/todo/*id* | `PutTodoList` | 更新一个待办事项列表。 |
| POST /api/todo | `PostTodoList` | 创建一个新的待办事项列表。 |
| DELETE /api/todo/*id* | `DeleteTodoList` | 删除 TODO 列表。 |

请注意，对于某些操作的 Uri 包含的 ID 值的占位符。 例如，若要删除到-列表 id 为 42，URI 是`/api/todo/42`。

若要了解有关使用 Web API 的 CRUD 操作的详细信息，请参阅[创建该支持 CRUD 操作的 Web API](../../../web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations.md)。 此控制器的代码将非常简单。 以下是一些有趣的要点：

- `GetTodoLists`方法使用 LINQ 查询来筛选的登录的用户 ID 通过结果。 这样一来，用户仅看到他 / 她所属的数据。 此外，请注意 Select 语句用于将转换`ToDoList`实例到`TodoListDto`实例。
- PUT 和 POST 方法修改数据库之前检查模型状态。 如果**ModelState.IsValid**为 false，这些方法将返回 HTTP 400 错误的请求。 阅读有关在 Web API 中的模型验证的详细信息[模型验证](../../../web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api.md)。
- 控制器类也用修饰**[Authorize]**属性。 此属性检查 HTTP 请求进行身份验证。 如果请求未通过身份验证，客户端收到 HTTP 401 未授权。 阅读有关在身份验证的详细信息[身份验证和 ASP.NET Web API 中的授权](../../../web-api/overview/security/authentication-and-authorization-in-aspnet-web-api.md)。

`TodoController`类是非常类似于`TodoListController`。 最大的区别是它不定义任何 GET 方法中，因为客户端将会获得待办事项项目，以及每个待办事项列表。

## <a name="mvc-controllers-and-views"></a>MVC 控制器和视图

MVC 控制器也位于该解决方案的 Controllers 文件夹中。 `HomeController` 应用程序的主要 HTML 呈现。 Views/Home/Index.cshtml 中定义了主控制器的视图。 主页视图呈现具体取决于是否将用户记录的不同内容：

[!code-cshtml[Main](knockoutjs-template/samples/sample4.cshtml)]

当用户登录时，他们将看到的主 UI。 否则，它们将看到登录面板。 请注意在服务器端时发生此条件的呈现。 决不会尝试隐藏客户端上的敏感内容 （&) 在 HTTP 响应中发送的 #8212anything 是对观察原始 HTTP 消息的任何人员可见。

## <a name="client-side-javascript-and-knockoutjs"></a>客户端 JavaScript 和 Knockout.js

现在我们将从服务器端到客户端应用程序。 SPA 模板使用 jQuery 和 Knockout.js 的组合来创建平滑的交互式 UI。 Knockout.js 是一个 JavaScript 库，可以轻松地将 HTML 绑定到数据。 Knockout.js 使用一种模式称为"模型-视图-视图模型。"

- 该模型是域数据 （ToDo 列表和 ToDo 项）。
- 该视图是 HTML 文档。
- 视图模型是包含模型数据的 JavaScript 对象。 视图模型是 UI 的代码抽象。 它具有的 HTML 表示不知道。 相反，它表示抽象视图，例如"ToDo 项的列表"的功能。

该视图被数据绑定到视图模型。 对视图模型更新会自动反映在视图中。 绑定都可以其他的方向。 DOM （如，单击） 中的事件是数据绑定到函数在视图模型上触发 AJAX 调用的。

SPA 模板将客户端 JavaScript 组织成三层：

- todo.datacontext.js： 发送 AJAX 请求。
- todo.model.js： 定义模型。
- todo.viewmodel.js： 定义视图模型。

![](knockoutjs-template/_static/image11.png)

这些脚本文件位于解决方案的脚本/应用程序文件夹中。

![](knockoutjs-template/_static/image12.png)

**todo.datacontext**处理所有 AJAX 调用到 Web API 控制器。 （用于登录的 AJAX 调用在其他位置中, 定义 ajaxlogin.js。）

**todo.model.js**定义待办事项列表客户端 （浏览器） 模型。 有两个模型类： todoItem 和 todoList。

许多模型类中的属性属于类型"ko.observable"。 可观察对象是 Knockout 原理其幻数。 从[Knockout 文档](http://knockoutjs.com/documentation/introduction.html): 可观测对象是一个"JavaScript 对象，可以通知有关更改的订阅服务器。" 当可观测对象的值发生更改时，Knockout 更新绑定到这些可观察对象的任何 HTML 元素。 例如，todoItem 具有标题和 isDone 属性的可观察对象：

[!code-javascript[Main](knockoutjs-template/samples/sample5.js)]

你还可以订阅在代码中的可观测对象。 例如，"isDone"和"title"属性中的更改订阅的 todoItem 类：

[!code-javascript[Main](knockoutjs-template/samples/sample6.js)]

**视图模型**

视图模型是在 todo.viewmodel.js 中定义的。 视图模型是其中应用程序将 HTML 页元素绑定到域数据的中心点。 在 SPA 模板中，视图模型包含 todoLists 可观察到的数组。 视图模型中的以下代码将指示 Knockout 要应用绑定：

[!code-javascript[Main](knockoutjs-template/samples/sample7.js)]

## <a name="html-and-data-binding"></a>HTML 和数据绑定

Views/Home/Index.cshtml 中定义了主要 HTML 页。 因为我们正在使用数据绑定，HTML 只是一个模板实际获取呈现效果。 使用 knockout*声明性*绑定。 通过将"数据绑定"属性添加到该元素将页元素绑定到数据。 下面是一个非常简单的示例，摘自 Knockout 文档：

[!code-html[Main](knockoutjs-template/samples/sample8.html)]

在此示例中，Knockout 更新的内容**&lt;跨越&gt;**具有的值的元素`myItems.count()`。 当此值发生更改时，Knockout 更新文档。

Knockout 提供大量的不同绑定类型。 下面是一些在 SPA 模板中使用的绑定：

- **foreach**： 可以循环访问一个循环并应用于列表中的每个项目的相同标记。 这用于呈现的待办事项列表和待办事项。 在**foreach**，绑定应用于列表中的元素。
- **可见**： 用于切换可见性。 隐藏标记，当集合为空，或使错误消息可见。
- **值**： 用于填充窗体值。
- **单击**： 将一个 click 事件绑定到视图模型上的函数。

## <a name="anti-csrf-protection"></a>反 CSRF 保护

跨站点请求伪造 (CSRF) 是一种攻击，恶意站点向在当前登录用户的易受攻击站点发送请求的位置。 为了帮助防止 CSRF 攻击，ASP.NET MVC 使用*防伪令牌*，也称为请求验证令牌。 思路是服务器将随机生成的令牌放到网页。 当客户端提交到服务器的数据时，它必须在请求消息中包括此值。

防伪令牌，因为恶意页无法读取用户的令牌，由于同源策略工作。 （同源策略将阻止访问彼此的内容的两个不同站点上托管的文档）。

ASP.NET MVC 防伪令牌，通过提供内置支持[AntiForgery](https://msdn.microsoft.com/library/system.web.helpers.antiforgery.aspx)类和[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute.aspx)属性。 目前，此功能的非内置 Web API。 但是，SPA 模板包含自定义实现 Web api。 此代码中定义`ValidateHttpAntiForgeryTokenAttribute`类，该类位于该解决方案的筛选器文件夹中。 若要了解有关 Web API 中的反 CSRF 的详细信息，请参阅[防止跨站点请求伪造 (CSRF) 攻击](../../../web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks.md)。

## <a name="conclusion"></a>结束语

SPA 模板旨在帮助你入门快速编写现代的交互式 web 应用程序。 它使用 Knockout.js 库来分离数据和应用程序逻辑的演示文稿 （HTML 标记）。 但 Knockout 不是唯一可用于创建 SPA 的 JavaScript 库。 如果你想要浏览其他一些选项，看一看[社区创建的 SPA 模板](../templates/index.md)。
