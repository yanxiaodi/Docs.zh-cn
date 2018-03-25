---
uid: single-page-application/overview/templates/breezeangular-template
title: "轻松/角模板 |Microsoft 文档"
author: madskristensen
description: "轻松/角单页面应用程序模板"
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/08/2013
ms.topic: article
ms.assetid: db31e909-563a-4516-aadd-62aa210ac7e4
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/breezeangular-template
msc.type: authoredcontent
ms.openlocfilehash: faf28a510a83b7fa07585904344176601c2e1f34
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="breezeangular-template"></a>轻松/角模板
====================
通过[Mads Kristensen](https://github.com/madskristensen)

> 轻松/角 MVC 模板是编写的选区钟形
> 
> [下载轻松/角度 MVC 模板](https://go.microsoft.com/fwlink/?LinkId=286437)


[AngularJS](http://angularjs.org)用于构建单页面应用程序 (Spa) 是从 Google 一个开放源代码库。 它提供数据绑定、 依赖关系注入和屏幕管理。 将其与结合[BreezeJS](http://www.breezejs.com/?utm_source=ms-spa)，数据建模和数据管理，并且你的另一个开放源代码库具有很好的 HTML/JavaScript 客户端应用程序的基本成分。

轻松/角 SPA 模板上是一种变体[KnockoutJS SPA 模板](../introduction/knockoutjs-template.md)包含在 ASP.NET 和 Web Tools 2012.2 更新。 如果您有 Visual Studio，则必须示例 SPA 启动并正在运行 60 秒内。

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningTodoPage.png)

表面上是，应用程序看起来非常相似 KnockoutJS SPA 模板。 但它是实质上却大不相同。 KnockoutJS 模板用于数据绑定和数据访问的原始 AJAX Knockout。 轻松/角模板使用角进行数据绑定和轻松进行数据访问。 这些库启用其他功能，包括页面导航和历史记录。

下面是应用程序的关于页上：

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningAboutPage.png)

此页当前用户会话期间，将显示一份运行日志的事件包括：

- 分页。 请注意在 #2 和快照 #7 在 Todo 控制器创建。
- 远程查询 (#3) 和本地缓存查询 (#7)。
- 保存新 （#5，6） 和修改 (#4) 实体。
- 在上验证客户端 (#9)，以便用户可以在提交更改之前更正错误到数据库的更改。

多个要在此模板中，浏览包括：

- 动态加载的 HTML 视图模板。
- 通过角"指令。"的自定义数据绑定
- 模块性和依赖关系注入。
- 查询筛选器、 排序、 分页、 投影和包含相关实体。
- 跨多个屏幕共享数据。
- 作为单个事务中保存多个更改。
- 验证规则传播自动从服务器到 JavaScript 客户端。

让我们开始吧。

## <a name="create-a-breezeangular-template-project"></a>创建一个角度轻松/模板项目

下载并安装该模板通过单击上面的下载按钮。 模板被打包为 Visual Studio 扩展 (VSIX) 文件。 你可能需要重新启动 Visual Studio。

在**模板**窗格中，选择**已安装的模板**展开**Visual C#**节点。 下**Visual C#**，选择**Web**。 在项目模板列表中，选择**ASP.NET MVC 4 Web 应用程序**。 命名该项目并单击**确定**。

在**新项目**向导中，选择**轻松角度 SPA**。

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeNgSpaTemplate.png)

按 Ctrl + F5 生成并运行应用程序而不进行调试，或按 F5 边运行边调试。

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrLogin.png)

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
- 单击右上角，若要查看这些活动日志中的"关于"链接。

验证逻辑是通过轻松执行的客户端。 在的服务器模型类验证特性是传播到客户端，并且自动执行之前客户端与服务器联系。

查看网络流量。 请注意轻松检测到错误时的已连接到服务器的任何调用。 每个有效的更改导致 POST 请求为"/ api/Todo/SaveChanges"。 轻松将打包所做的更改并将其发送在一起以单个请求到 Web API 控制器`SaveChanges`方法。 这是不同于 KockoutJS SPA 模板，这使得 PUT、 POST 和单独删除每个项的请求。

此外，请注意没有任何网络流量时切换之间任务列表和有关页。 这是因为已到本地缓存中轻松约束查询。

## <a name="peek-inside"></a>在查看

此应用程序具有客户端和服务器端。 客户端堆栈组成少量 HTML 和应用程序 JavaScript 模块 （在"应用"文件夹中） 的组合以及第三方 JavaScript 库 （在"脚本"文件夹中）。

![](http://www.breezejs.com/sites/all/images/spa-template/NgClientArchitecture2.png)

UI 体系结构控制器中的支持演示文稿代码分离开来的视图的 HTML 小组件。 角度的数据绑定系统协调视图和控制器，以便每个可以完成其他直接不知情的情况下操作。

控制器要求的数据上下文，以获取并保存模型实体。 数据上下文委托的大部分工作到轻松，构造自跟踪模型对象从 JSON 查询结果。

一些开发人员代码和三个原则.NET 库的服务器端堆栈组成： Web API、 实体框架和 Breeze.NET:

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

KockoutJS SPA 模板相同的基本体系结构。 但是，实现是要简单得多： Dto 已删除，并且大部分的实体框架的详细信息已委派给 Breeze.NET。

## <a name="next-steps"></a>后续步骤

我们建议你浏览代码中，由指导[全面讨论](http://www.breezejs.com/ng-spa-template?utm_source=ms-spa)的客户端和服务器堆栈轻松网站上的。

你可以尝试 playing with 轻松客户端查询; 使用添加某些筛选器和排序。 你可以添加更多的模型属性和多个实体来更好地感受用于端到端 SPA 开发。 当你确信的设计时，你可以拉出 Todo 功能并将它们替换为你自己。

祝你编码愉快！
