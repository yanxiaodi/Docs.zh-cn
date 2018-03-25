---
uid: mvc/overview/older-versions-1/overview/asp-net-mvc-overview
title: "ASP.NET MVC 概述 |Microsoft 文档"
author: microsoft
description: "了解有关 ASP.NET MVC 应用程序和 ASP.NET Web 窗体应用程序之间的差异。 了解如何决定何时生成 ASP.NET MVC 应用程序。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 2dcb44a4-5cbf-4d62-b363-718104082d86
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/overview/asp-net-mvc-overview
msc.type: authoredcontent
ms.openlocfilehash: f44e6fb1e19d3c2384ebaeeca0ddea8239dd5a3c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-overview"></a>ASP.NET MVC 概述
====================
通过[Microsoft](https://github.com/microsoft)

> 了解有关 ASP.NET MVC 应用程序和 ASP.NET Web 窗体应用程序之间的差异。 了解如何决定何时生成 ASP.NET MVC 应用程序。


模型-视图-控制器 (MVC) 体系结构模式将应用程序分成三个主要组件： 模型、 视图和控制器。 ASP.NET MVC framework 提供了用于创建基于 MVC 的 Web 应用程序的 ASP.NET Web 窗体模式的替代方法。 ASP.NET MVC framework 是一种轻型、 高度可测试演示框架的 （与基于 Web 窗体的应用程序） 与现有的 ASP.NET 功能，如母版页和基于成员身份的身份验证集成。 在中定义的 MVC 框架**System.Web.Mvc**命名空间和是基本的、 受支持的一部分**System.Web**命名空间。   
  
MVC 是一种标准设计模式，许多开发人员所熟悉。 某些类型的 Web 应用程序将受益于 MVC 框架。 其他人将继续使用传统的 ASP.NET 应用程序模式，基于 Web 窗体和回发。 其他类型的 Web 应用程序将合并这两种方法;这两种做法排除其他。   
  
MVC 框架还包括以下组件：


[![调用需要参数值的控制器操作](asp-net-mvc-overview/_static/image1.jpg)](asp-net-mvc-overview/_static/image1.png)

**图 01**： 调用需要参数值的控制器操作 ([单击以查看实际尺寸的图像](asp-net-mvc-overview/_static/image2.png))


- **模型**。 模型对象是实现应用程序的数据域的逻辑的应用程序的部件。 通常情况下，模型对象检索，并在数据库中存储模型状态。 例如，某个产品对象可能从数据库中检索信息、 对其，然后更新将信息写回到 SQL Server 中的产品表。

在小型应用程序，该模型通常是而不是一个物理概念分离。 例如，如果应用程序仅读取数据集，并将其发送到的视图，应用程序没有物理模型层和关联的类。 在这种情况下，数据集将在模型对象的角色上。

- **视图**。 视图是显示应用程序的用户界面 (UI) 的组件。 通常情况下，此 UI 会从模型数据创建。 示例将显示文本框、 下拉列表和基于产品对象的当前状态的复选框的产品表的编辑视图。

- **控制器**。 控制器在处理用户交互，使用的模型，并最终选择显示的视图来呈现用户界面的组件。 在 MVC 应用程序，该视图仅显示信息;控制器处理，并响应用户输入和交互。 例如，在控制器处理查询字符串值，并将这些值传递给模型，后者进而查询通过使用的值的数据库。

MVC 模式可帮助您创建单独的应用程序 （输入的逻辑、 业务逻辑和 UI 逻辑），同时提供这些元素间的松散耦合的不同方面的应用程序。 该模式指定每种类型的逻辑在应用程序中的位置。 UI 逻辑位于视图中。 输入逻辑位于控制器中。 业务逻辑位于模型中。 这种隔离可帮助你管理复杂性，在生成应用程序，因为它使你能够专注于一次的实现的一个方面。 例如，你可以专注于根据业务逻辑不含视图。   
  
除了管理复杂性，MVC 模式可以更轻松地测试基于 Web 窗体的 ASP.NET Web 应用程序比测试应用程序。 例如，在基于 Web 窗体的 ASP.NET Web 应用程序，同时以显示输出并响应用户输入使用单个类。 编写用于基于 Web 窗体的 ASP.NET 应用程序的自动的测试可能十分复杂，因为若要测试单个页面，你必须实例化页类、 所有其子控件和应用程序中的其他相关类。 实例化类如此之多运行页面，因为它可能很难编写专门重点关注各个部分的应用程序的测试。 基于 Web 窗体的 ASP.NET 应用程序的测试可以因此会更加难以实现都比在 MVC 应用程序的测试。 此外，在基于 Web 窗体的 ASP.NET 应用程序中的测试需要 Web 服务器。 MVC 框架将分离组件和接口，这样就可以用于测试中的 framework 的其余部分分开的各个组件的大量使用。   
  
一个 MVC 应用程序的三个主要组件之间的松耦合还可并行开发。 例如，一位开发人员可以处理视图、 控制器逻辑，可以处理第二个开发人员和第三个开发人员可以专注于模型中的业务逻辑。

## <a name="deciding-when-to-create-an-mvc-application"></a>确定何时创建 MVC 应用程序

你必须仔细考虑是否要通过使用 ASP.NET MVC framework 或 ASP.NET Web 窗体模型执行的 Web 应用程序。 MVC 框架不会替换该 Web 窗体模型;为 Web 应用程序，可以使用任一框架。 （如果你有现有的基于 Web 窗体的应用程序，这些继续工作方式与它们始终具有。）   
  
你决定使用为特定网站的 MVC 框架或 Web 窗体模型之前，请权衡每种方法的优点。

### <a name="advantages-of-an-mvc-based-web-application"></a>基于 MVC 的 Web 应用程序的优点

ASP.NET MVC framework 提供以下优势：

- 它轻松地通过将应用程序划分为模型、 视图和控制器管理的复杂性。
- 它不使用基于服务器的窗体或视图状态。 这使 MVC 框架理想的开发人员想要完全控制应用程序的行为。
- 它使用处理通过单个控制器的 Web 应用程序请求前控制器模式。 这使您可以设计支持丰富的路由基础结构的应用程序。 有关详细信息，请参阅[前端控制器](https://go.microsoft.com/fwlink/?LinkId=106357 "前端控制器")MSDN 网站上。
- 用于测试驱动开发 (TDD) 提供更好地支持它。
- 它适用于 Web 应用程序支持的较大团队的开发人员和 Web 设计人员需要高大程度上控制应用程序的行为。

### <a name="advantages-of-a-web-forms-based-web-application"></a>Web 基于窗体的 Web 应用程序的优点

基于 Web 窗体的框架提供以下优势：

- 它支持通过 HTTP，这进而业务线 Web 应用程序开发，将保留状态的事件模型。 基于 Web 窗体的应用程序提供的许多支持数百个服务器控件中的事件。
- 它使用将功能添加到各个页的页控制器模式。 有关详细信息，请参阅[页控制器](https://go.microsoft.com/fwlink/?LinkId=106359 "页控制器")MSDN 网站上。
- 它使用视图状态或基于服务器的窗体，这可以使管理状态信息更容易。
- 它适用于小团队的 Web 开发人员和设计人员想要利用大量可供快速开发应用程序组件。
- 通常情况下，是不太复杂，应用程序开发，因为组件 (**页**类、 控件和等等) 紧密集成，并且通常需要更少的代码比 MVC 模型。

## <a name="features-of-the-aspnet-mvc-framework"></a>ASP.NET MVC Framework 的功能

ASP.NET MVC framework 提供了以下功能：

- 应用程序任务 （输入的逻辑、 业务逻辑和 UI 逻辑），可测试性和默认的测试驱动开发 (TDD) 分离。 MVC 框架中的所有核心协定是基于接口的并且可以使用模拟对象，后者是模拟的对象，用于模拟应用程序中的实际对象的行为进行测试。 你可以进行单元测试应用程序而无需在 ASP.NET 过程中，这使得单元测试快速且灵活运行控制器。 你可以使用任何单元测试框架与.NET Framework 兼容。
- 一种可扩展和可插入框架。 ASP.NET MVC framework 的组件，以便它们可以轻松地替换或自定义设计。 您可以插入您自己的视图引擎、 URL 路由策略、 操作方法参数序列化和其他组件。 ASP.NET MVC framework 还支持依赖关系注入 (DI) 和控制反向 (IOC) 容器模型的使用。 DI 允许您将插入的类，而不是依赖于要创建对象本身的类的对象。 IOC 指定，是否对象需要另一个对象，第一个对象应从外部源如配置文件获取第二个对象。 这样可更轻松地测试。
- 一个功能强大的 URL 映射组件，用于构建具有易于理解和搜索 Url 的应用程序。 Url 不需要包括文件扩展名，并旨在支持工作的 URL 命名模式适用于搜索引擎搜索引擎优化 (SEO) 和具象状态传输 (REST) 寻址。
- 支持使用现有的 ASP.NET 页面 （.aspx 文件）、 用户控件 （.ascx 文件） 和母版页 （.master 文件） 标记文件中标记为视图模板。 你可以使用现有的 ASP.NET 功能使用 ASP.NET MVC framework，如嵌套的主页面，行表达式 (&lt;%= %&gt;)，声明性的服务器控件、 模板、 数据绑定、 本地化和等等。
- 对现有的 ASP.NET 功能的支持。 ASP.NET MVC 允许你使用如窗体身份验证和 Windows 身份验证、 URL 授权、 成员资格和角色、 输出和数据缓存、 会话和配置文件状态管理、 运行状况监视、 配置系统和提供程序的功能体系结构。
