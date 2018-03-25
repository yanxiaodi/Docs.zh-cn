---
uid: mvc/overview/older-versions-1/overview/understanding-models-views-and-controllers-vb
title: "了解模型、 视图和控制器 (VB) |Microsoft 文档"
author: StephenWalther
description: "感到迷惑的模型、 视图和控制器？ 在本教程中，Stephen Walther 引入你对 ASP.NET MVC 应用程序的不同部分。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: a106374a-5e74-4fd0-9ac0-1a32280e5d0d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-models-views-and-controllers-vb
msc.type: authoredcontent
ms.openlocfilehash: 36f70503f7e96210e5fb8a2d361da6759c18c85b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="understanding-models-views-and-controllers-vb"></a>了解模型、 视图和控制器 (VB)
====================
通过[Stephen Walther](https://github.com/StephenWalther)

> 感到迷惑的模型、 视图和控制器？ 在本教程中，Stephen Walther 引入你对 ASP.NET MVC 应用程序的不同部分。


本教程提供与 ASP.NET MVC 的高级概述模型、 视图和控制器也是如此。 换而言之，它还说明了 M，V，和 C 在 ASP.NET MVC。

在阅读本教程后，你应该了解 ASP.NET MVC 应用程序的不同部分如何协同工作。 您还应了解 ASP.NET MVC 应用程序的体系结构有何不同从 ASP.NET Web 窗体应用程序或 Active Server Pages 应用程序。

## <a name="the-sample-aspnet-mvc-application"></a>示例 ASP.NET MVC 应用程序

用于创建 ASP.NET MVC Web 应用程序的默认 Visual Studio 模板包括可用于了解 ASP.NET MVC 应用程序的不同部分的极其简单的示例应用程序。 我们充分利用在本教程中此简单应用程序。

使用 MVC 模板中启动 Visual Studio 2008 创建了新的 ASP.NET MVC 应用程序并选择文件菜单选项，新建项目 （请参见图 1）。 在新建项目对话框中，选择您最喜欢的编程语言，在项目类型 （Visual Basic 或 C#） 下，选择**ASP.NET MVC Web 应用程序**下模板。 单击确定按钮。


[![新建项目对话框](understanding-models-views-and-controllers-vb/_static/image1.jpg)](understanding-models-views-and-controllers-vb/_static/image1.png)

**图 01**: 新建项目对话框 ([单击以查看实际尺寸的图像](understanding-models-views-and-controllers-vb/_static/image2.png))


当你创建一个新的 ASP.NET MVC 应用程序，**创建单元测试项目**对话框 （请参见图 2）。 此对话框使你可以在测试 ASP.NET MVC 应用程序解决方案中创建一个单独的项目。 选择选项**否，不创建单元测试项目**单击**确定**按钮。


[![创建单元测试对话框](understanding-models-views-and-controllers-vb/_static/image2.jpg)](understanding-models-views-and-controllers-vb/_static/image3.png)

**图 02**： 创建单元测试对话框 ([单击以查看实际尺寸的图像](understanding-models-views-and-controllers-vb/_static/image4.png))


创建后新的 ASP.NET MVC 应用程序。 你将看到几个文件夹和解决方案资源管理器窗口中的文件。 具体而言，你将看到名为模型、 视图和控制器的三个文件夹。 您可能已经猜到的文件夹名称，这些文件夹包含用于实现模型、 视图和控制器的文件。

如果你展开 Controllers 文件夹，你应看到一个名为 AccountController.vb 文件和一个名为 HomeController.vb 文件。 如果展开视图文件夹，你应看到三个名为帐户，主页和共享的子文件夹。 如果您展开主文件夹，你将看到名为 About.aspx 和 Index.aspx （请参见图 3） 的两个其他文件。 这些文件构成了默认的 ASP.NET MVC 模板中包含的示例应用程序。


[![解决方案资源管理器窗口](understanding-models-views-and-controllers-vb/_static/image3.jpg)](understanding-models-views-and-controllers-vb/_static/image5.png)

**图 03**： 的解决方案资源管理器窗口 ([单击以查看实际尺寸的图像](understanding-models-views-and-controllers-vb/_static/image6.png))


你可以通过选择菜单选项来运行示例应用程序**调试、 启动调试**。 或者，你可以按 F5 键。

当你首次运行 ASP.NET 应用程序时，此时将显示在图 4 对话框，建议你启用调试模式。 单击确定按钮，然后运行该应用程序。


[![调试未启用对话框](understanding-models-views-and-controllers-vb/_static/image4.jpg)](understanding-models-views-and-controllers-vb/_static/image7.png)

**图 04**： 未启用调试对话框 ([单击以查看实际尺寸的图像](understanding-models-views-and-controllers-vb/_static/image8.png))


当你运行的 ASP.NET MVC 应用程序时，Visual Studio 将启动 web 浏览器中的应用程序。 示例应用程序只有两个页组成： 索引页和关于页面。 当第一次启动应用程序时，索引页面显示 （请参见图 5）。 你可以通过单击顶部的菜单链接导航到关于页面右侧的应用程序。


[![索引页面](understanding-models-views-and-controllers-vb/_static/image5.jpg)](understanding-models-views-and-controllers-vb/_static/image9.png)

**图 05**: 索引页 ([单击以查看实际尺寸的图像](understanding-models-views-and-controllers-vb/_static/image10.png))


请注意您的浏览器的地址栏中的 Url。 例如，当你单击关于菜单链接时，浏览器地址栏中的 URL 更改为**/Home/有关**。

如果你关闭浏览器窗口，并返回到 Visual Studio，你将无法使用路径主页 / 关于查找的文件。 文件不存在。 如何这是可能的？

## <a name="a-url-does-not-equal-a-page"></a>URL 不等于页

传统的 ASP.NET Web 窗体应用程序或 Active Server Pages 应用程序生成时，没有一个 URL，另一个页面之间的一一对应关系。 如果请求一个名为从服务器的 SomePage.aspx 页，然后有具有更好地能页在磁盘上名为 SomePage.aspx。 如果 SomePage.aspx 文件不存在，则获取极差**404-找不到页面**错误。

在生成 ASP.NET MVC 应用程序时，与此相反，你的浏览器地址栏中键入 URL 和应用程序中查找的文件之间没有对应关系。 在 ASP.NET MVC 应用程序 URL 对应于而不是在磁盘上页面的控制器操作。

在传统 ASP.NET 或 ASP 应用程序中，浏览器请求将映射到页。 在 ASP.NET MVC 应用程序，与此相反，浏览器请求将映射到控制器操作。 ASP.NET Web 窗体应用程序是以内容为中心。 ASP.NET MVC 应用程序与此相反，是以为中心的应用程序逻辑。

## <a name="understanding-aspnet-routing"></a>了解 ASP.NET 路由

浏览器请求获取映射到通过一项功能的 ASP.NET 框架调用的控制器操作*ASP.NET 路由*。 ASP.NET 路由使用 ASP.NET MVC framework*路由*到控制器操作的传入请求。

ASP.NET 路由使用路由表来处理传入请求。 Web 应用程序首次启动时创建此路由表。 路由表是在 Global.asax 文件中的安装程序。 默认 MVC Global.asax 文件包含在清单 1。

**列表 1-Global.asax**

[!code-vb[Main](understanding-models-views-and-controllers-vb/samples/sample1.vb)]

当 ASP.NET 应用程序第一次启动时，应用程序\_调用 start （） 方法。 在列出 1 中，此方法调用 RegisterRoutes() 方法和 RegisterRoutes() 方法创建的默认路由表。

默认路由表都包含一个路由。 此默认路由分成三个段 （URL 段是正斜杠之间的任何内容） 的所有传入请求。 第一条线段映射到的控制器名称、 第二个段映射到操作名称，和最后一个段映射到传递到名为 id。 操作的参数

例如，考虑以下 URL:

/ 产品/详细信息/3

此 URL 解析为三个参数如下：

控制器 = 产品

操作 = 详细信息

id = 3

在 Global.asax 文件中定义的默认路由包括所有三个参数的默认值。 默认值控制器是主页、 默认操作是索引，和默认 Id 为空字符串。 记住这些默认值，应考虑以下 URL 的分析方式：

/ 员工

此 URL 解析为三个参数如下：

控制器 = 员工

操作 = 索引

Id =

最后，如果你打开 ASP.NET MVC 应用程序而无需提供任何 URL (例如， `http://localhost`)，则将 URL 解析如下：

控制器 = 主页

操作 = 索引

Id =

请求路由到 HomeController 类上的 index （） 操作。

## <a name="understanding-controllers"></a>了解控制器

控制器负责控制用户与 MVC 应用程序交互的方式。 控制器包含一个 ASP.NET MVC 应用程序的流控制逻辑。 控制器将确定哪些响应，以便当用户进行浏览器请求将发送回用户。

控制器是只是一个类 （例如，Visual Basic 或 C# 类）。 此示例 ASP.NET MVC 应用程序包含名为 HomeController.vb 控制器文件夹中的控制器。 将列出 2 中在重现 HomeController.vb 文件的内容。

**列出 2-HomeController.cs**

[!code-vb[Main](understanding-models-views-and-controllers-vb/samples/sample2.vb)]

请注意，HomeController 命名 index （） 和 About() 的两个方法。 这两种方法对应于在控制器公开的两个操作。 URL /Home/索引调用 HomeController.Index() 方法和 URL /Home/About 调用 HomeController.About() 方法。

控制器中的任何公共方法公开为控制器操作。 你需要有关此谨慎。 这意味着控制器中包含任何公共方法可由有权访问 Internet 的任何人都调用通过在浏览器中输入正确的 URL。

## <a name="understanding-views"></a>了解视图

由 HomeController 类，index （） 和 About()，公开的两个控制器操作两者都返回一个视图。 视图包含的 HTML 标记和发送到浏览器的内容。 使用 ASP.NET MVC 应用程序时，视图是单页的等效项。

必须在正确的位置中创建你的视图。 HomeController.Index() 操作返回位于以下路径下的视图：

\Views\Home\Index.aspx

HomeController.About() 操作返回位于以下路径下的视图：

\Views\Home\About.aspx

一般情况下，如果你想要返回的控制器操作的视图，然后你需要在与你的控制器同名的 Views 文件夹中创建一个子文件夹。 在子文件夹，必须使用与控制器操作相同的名称来创建一个.aspx 文件。

列出 3 中的文件包含 About.aspx 视图。

**列出 3-About.aspx**

[!code-aspx[Main](understanding-models-views-and-controllers-vb/samples/sample3.aspx)]

如果您忽略列出 3 中的第一行，大部分视图的其余部分由标准 HTML 组成。 可以通过输入你想此处任何 HTML 来修改视图的内容。

视图是非常类似于 Active Server Pages 或 ASP.NET Web 窗体中的页。 视图可以包含 HTML 内容和脚本。 你可以编写的脚本中你最喜欢的.NET 编程语言 (例如，C# 或 Visual Basic.NET）。 您可以使用脚本来显示如数据库数据的动态内容。

## <a name="understanding-models"></a>了解模型

我们已经讨论了控制器和我们已经讨论了视图。 我们需要讨论的最后一个主题是模型。 MVC 模型是什么？

MVC 模型包含所有未包含在视图或控制器你应用程序逻辑。 此模型应包含的所有应用程序业务逻辑、 验证逻辑和数据库访问逻辑。 例如，如果你使用 Microsoft 实体框架访问你的数据库，然后你将你 Entity Framework 类 （您的.edmx 文件） 的文件夹中创建模型。

视图应包含仅与生成的用户界面相关的逻辑。 控制器应仅包含逻辑返回右视图或将用户重定向到另一项操作 （流控制） 所需的最低的要求。 该模型中，应包含其他任何内容。

一般情况下，也应尽可能 fat 模型和小控制器。 控制器方法应包含只几行代码。 如果控制器操作获取太 fat，则应考虑通过将逻辑移动到 Models 文件夹中的新类。

## <a name="summary"></a>摘要

本教程向您提供的高级别概述的不同部分的 ASP.NET MVC web 应用程序。 你已了解如何 ASP.NET 路由传入浏览器将请求映射到特定控制器操作。 你已了解如何控制器安排如何将视图返回到浏览器。 最后，您学习了如何模型包含应用程序业务、 验证和数据库访问逻辑。
