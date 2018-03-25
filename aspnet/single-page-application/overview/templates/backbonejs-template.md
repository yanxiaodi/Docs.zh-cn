---
uid: single-page-application/overview/templates/backbonejs-template
title: "Backbone 模板 |Microsoft 文档"
author: madskristensen
description: "Backbone.js SPA 模板"
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/04/2013
ms.topic: article
ms.assetid: 00aca413-f067-4108-9bd1-cf21e64a2646
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/backbonejs-template
msc.type: authoredcontent
ms.openlocfilehash: 3b8eabd3cefcb96dc40bbf6cc6e3ee81accb0d7c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="backbone-template"></a>Backbone 模板
====================
通过[Mads Kristensen](https://github.com/madskristensen)

> Backbone SPA 模板已写入 Kazi Manzur 发展的见解
> 
> [下载 Backbone.js SPA 模板](https://go.microsoft.com/fwlink/?LinkId=293631)


Backbone.js SPA 模板旨在帮助你入门快速构建客户端的交互式 web 应用程序使用[Backbone.js。](http://backbonejs.org/)

模板提供用于开发 Backbone.js ASP.NET mvc 应用程序的初始主干。 在初始状态下，它提供基本的用户登录功能，包括用户注册、 登录，密码重置，以及使用基本的电子邮件模板的用户确认。

要求：

- [ASP.NET 和 Web Tools 2012.2 的更新](https://go.microsoft.com/fwlink/?LinkId=282650)

## <a name="create-a-backbone-template-project"></a>创建一个主干模板项目

下载并安装该模板通过单击上面的下载按钮。 模板被打包为 Visual Studio 扩展 (VSIX) 文件。 你可能需要重新启动 Visual Studio。

在**模板**窗格中，选择**已安装的模板**展开**Visual C#**节点。 下**Visual C#**，选择**Web**。 在项目模板列表中，选择**ASP.NET MVC 4 Web 应用程序**。 命名该项目并单击**确定**。

![](backbonejs-template/_static/image1.png)

在**新项目**向导，选择 Backbone.js SPA 项目。

![](backbonejs-template/_static/image2.png)

按 Ctrl + F5 生成并运行应用程序而不进行调试，或按 F5 边运行边调试。

![](backbonejs-template/_static/image3.png)

单击"我的帐户"将打开登录页：

![](backbonejs-template/_static/image4.png)

## <a name="walkthrough-client-code"></a>演练： 客户端代码

让我们开头的客户端。 客户端应用程序脚本位于 ~/Scripts/application 文件夹中。 应用用编写[TypeScript](http://www.typescriptlang.org/) （.ts 文件） 进行编译到 JavaScript （.js 文件）。

**应用程序**

`Application`在 application.ts 中定义。 此对象初始化应用程序，并且可作为根命名空间。 这样可保持在应用程序之间共享的配置和状态信息，如用户已登录。

`application.start`方法创建的模式视图，并将附加应用程序级事件，例如用户登录事件处理程序。 接下来，它创建的默认路由器，并检查是否指定任何客户端的 URL。 如果不是，它将重定向到默认 url (# ！ /)。

**事件**

当开发松散耦合的组件时，事件始终是非常重要。 应用程序通常可以执行多个操作以响应用户操作。 Backbone 提供内置的事件的组件，如模型、 集合和视图。 该模板而不是创建这些组件间的相互依赖性，使用"发布/订阅"模型： `events` events.ts，在定义的对象充当用于发布和订阅应用程序事件的事件中心。 `events`对象是单一实例。 下面的代码演示如何订阅事件，然后触发该事件：

[!code-csharp[Main](backbonejs-template/samples/sample1.cs)]

**路由器**

在 Backbone.js，路由器提供方法以路由客户端页，并将它们连接到操作和事件。 模板定义中 router.ts 单一路由器。 路由器创建 activable 视图，并切换视图时维护的状态。 （activable 视图所述的下一节。）最初，该项目包含两个 dummy 视图，家庭和有关。 它还具有未找到视图，如果不知道路由才会显示。

**视图**

视图定义中 ~/Scripts/application/视图。 有两种类型的视图、 activable 视图和模式对话框视图。 Activable 视图都是通过路由器调用的。 当显示 activable 视图时，所有其他 activable 视图将变为不活动状态。 若要创建 activable 视图，请扩展与视图`Activable`对象：

[!code-javascript[Main](backbonejs-template/samples/sample2.js)]

使用扩展`Activable`将两个新方法添加到视图中，`activate`和`deactivate`。 路由器调用这些方法来激活和停用仍视图。

作为实现的模式视图[Twitter Bootstrap](http://twitter.github.com/bootstrap/)模式对话框。 `Membership`和`Profile`视图是模式的视图。 可以通过任何应用程序事件中调用模型视图。 例如，在`Navigation`视图中，单击"我的帐户"链接显示任一`Membership`视图或`Profile`视图中的，具体取决于用户已登录。 `Navigation`附加单击事件处理程序与具有任何子元素`data-command`属性。 下面是 HTML 标记：

[!code-html[Main](backbonejs-template/samples/sample3.html)]

以下是 navigation.ts 要挂钩到事件中的代码：

[!code-csharp[Main](backbonejs-template/samples/sample4.cs)]

**模型**

模型进行 ~/Scripts/application/模型中定义。 所有模型都具有三个基本操作： 默认属性、 验证规则和服务器端终结点。 下面是一个典型的示例：

[!code-javascript[Main](backbonejs-template/samples/sample5.js)]

**插件**

~/Scripts/application/lib 文件夹包含几个方便 jQuery 插件。Form.ts 文件定义插件用于使用窗体数据。 通常，你需要序列化或反序列化的表单数据并显示任何模型验证错误。 插件 form.ts 有方法，如`serializeFields`， `deserializeFields`，和`showFieldErrors`。 下面的示例演示如何序列化到模型的窗体。

[!code-javascript[Main](backbonejs-template/samples/sample6.js)]

插件 flashbar.ts 向用户提供的各种类型的反馈消息。 方法都是`$.showSuccessbar`，`$.showErrorbar`和`$.showInfobar`。 在后台，它将使用 Twitter Bootstrap 警报以显示可以很好地经过动画处理的消息。

插件 confirm.ts 替换浏览器的确认对话框中，虽然 API 是稍有不同：

[!code-javascript[Main](backbonejs-template/samples/sample7.js)]

## <a name="walkthrough-server-code"></a>演练： 服务器代码

现在让我们看一下服务器端。

**控制器**

在单页面应用程序中，服务器扮演仅在用户界面很小的作用。 通常情况下，服务器将呈现的初始页以及然后发送和接收 JSON 数据。

该模板具有两个 MVC 控制器：`HomeController`呈现的初始页，和`SupportsController`用于确认新的用户帐户和重置密码。 在模板中的所有其他控制器都是 ASP.NET Web API 控制器，发送和接收 JSON 数据。 默认情况下，控制器使用新`WebSecurity`类来执行与用户相关的任务。 但是，它们还具有可选，可以将其在委托中传递这些任务的构造函数。 这使测试更加轻松，并让您替换`WebSecurity`替换为其他的情况下，使用 IoC 容器。 下面是一个示例：

[!code-csharp[Main](backbonejs-template/samples/sample8.cs)]

## <a name="views"></a>视图

设计视图为模块化： 页面的每个部分都有自己专用的视图。 在单页面应用程序中，很普遍包括不具有任何相应的控制器的视图。 你可以通过调用包括视图`@Html.Partial('myView')`，但这将得到需要很长时间。 若要简化此过程，该模板定义了一个帮助器方法， `IncludeClientViews`，呈现所有指定的文件夹中的视图：

[!code-cshtml[Main](backbonejs-template/samples/sample9.cshtml)]

如果未指定文件夹名称，默认文件夹名称是"ClientViews"。 如果你的客户端视图还使用分部视图，名称以下划线字符分部视图 (例如， `_SignUp`)。 `IncludeClientViews`方法排除名称以下划线开头的任何视图。 若要在客户端视图中包含了分部视图，调用`Html.ClientView('SignUp')`而不是`Html.Partial('_SignUp')`。

**发送电子邮件**

若要发送电子邮件，该模板使用[邮递](http://aboutcode.net/postal)。 但是，从与代码的其余部分提取邮递`IMailer`接口，所以可以轻松地将其替换另一个实现。 电子邮件模板位于视图/电子邮件文件夹。 发件人的电子邮件地址中，在 web.config 文件中，指定`sender.email`键**appSettings**部分。 此外，当`debug="true"`在 web.config 中，应用程序不需要用户电子邮件确认，以加速开发。

## <a name="github"></a>GitHub

你还可以上查找 Backbone.js SPA 模板[GitHub](https://github.com/kazimanzurrashid/AspNetMvcBackboneJsSpa)。
