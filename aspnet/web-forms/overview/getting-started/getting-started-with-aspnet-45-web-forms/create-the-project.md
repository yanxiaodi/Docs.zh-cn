---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create-the-project
title: "创建项目 |Microsoft 文档"
author: Erikre
description: "本系列教程将教您生成有关我们使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 的 ASP.NET Web 窗体应用程序的基础知识..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 2ce36f78-8ecb-4ab1-b748-6d0ab633ea3f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create-the-project
msc.type: authoredcontent
ms.openlocfilehash: 094733dcbe31486385dda2f8b44ba77a17486c82
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="create-the-project"></a>创建项目
====================
通过[艾力克 Reitan](https://github.com/Erikre)

[下载 Wingtip Toys 示例项目 (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)或[下载电子书 (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> 本系列教程将教您构建使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for Web 的 ASP.NET Web 窗体应用程序的基础知识。 Visual Studio 2013[包含 C# 源代码项目](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)是可以附带本系列教程。


在本教程中你将创建、 查看，并在 Visual Studio 中，这将允许您以熟悉的 ASP.NET 功能运行默认项目。 此外，你将查看 Visual Studio 环境。

## <a name="what-youll-learn"></a>你将学习：

- 如何创建一个新的 Web 窗体项目。
- Web 窗体项目的文件结构。
- 如何在 Visual Studio 中运行项目。
- 默认 Web 窗体应用程序的不同功能。
- 有关如何使用 Visual Studio 环境的一些基础知识。

## <a name="creating-the-project"></a>创建项目

1. 打开 Visual Studio。
2. 选择**新项目**从**文件**Visual Studio 中的菜单。 

    ![创建新项目菜单项的项目-](create-the-project/_static/image1.png)
3. 选择**模板** - &gt; **Visual C#**  - &gt; **Web**左侧的模板组。
4. 选择**ASP.NET Web 应用程序**中心列中的模板。  
 本教程系列使用.NET Framework 4.5.2。
5. 命名你的项目*WingtipToys*选择**确定**按钮。 

    ![创建项目-新建项目对话框](create-the-project/_static/image2.png)

    > [!NOTE]
    > 本教程系列中的项目的名称是**WingtipToys**。 建议你使用此*确切*项目名称，以便整个系列教程函数按预期方式提供的代码。
6. 接下来，选择**Web 窗体**模板，然后选择**创建项目**按钮。  

    ![创建新的项目模板的项目-](create-the-project/_static/image3.png)

该项目将花一些时间来创建。 准备就绪后，打开**Default.aspx**页。

![创建新的项目模板的项目-](create-the-project/_static/image4.png)

你可以之间进行切换**设计**视图和**源**视图中选择中心窗口底部的选项。 **设计**视图显示 ASP.NET 网页、 母版页、 内容页、 HTML 页和用户控件使用附近的所见即所得视图。 **源**视图显示你 Web 页上，你可以编辑的 HTML 标记。

> [!TIP] 
> 
> **了解的 ASP.NET 框架**
> 
> ASP.NET Web 窗体，可以使用熟悉的拖放、 事件驱动模型生成动态网站。 设计图面和数百个控件和组件可以快速生成允许复杂且功能强大的 UI 驱动站点数据访问。 Wingtip 无实用价值存储基于 ASP.NET Web 窗体上，但是有许多你了解在本教程系列的概念适用于所有 ASP.NET。
> 
> ASP.NET 提供了四个主要开发框架：
> 
> - [ASP.NET Web 窗体](../../../index.md)  
>  Web 窗体框架都针对开发人员喜欢声明性和基于控件的编程中，例如 Microsoft Windows 窗体 (WinForms) 和 WPF/XAML/Silverlight。 它提供的所见即所得的设计器驱动的开发模型，因此很常用与寻找用于 web 开发的快速应用程序开发 (RAD) 环境的开发人员。 如果你不熟悉如何 web 编程，并熟悉的传统的 Microsoft RAD 客户端开发工具 （例如，对于 Visual Basic 和 Visual C# 中），你可以快速构建 web 应用程序，而无需在 HTML 和 JavaScript 经验。
> - [ASP.NET MVC](../../../../mvc/index.md)  
>  ASP.NET MVC 面向开发人员感兴趣模式和如测试驱动开发、 分离问题、 反向 (IoC)，控制和依赖关系注入 (DI) 的原则。 此框架鼓励将业务逻辑层中其表示层的 web 应用程序分开。
> - [ASP.NET 网页](../../../../web-pages/index.md)  
>  ASP.NET Web Pages 面向开发人员需简单的 web 开发情景，沿 PHP 的行。 在网页模型中，你将创建 HTML 页面，然后向页面添加基于服务器的代码，以便动态控制该标记的呈现方式。 网页专门用于能轻型框架，它并且知道 HTML，但可能没有广泛的编程体验-例如人员、 学生或热衷于 ASP.NET 的最简单入口点。 它也是为 web 开发人员知道 PHP 或类似的框架，若要开始使用 ASP.NET 的好方法。
> - [ASP.NET 单页面应用程序](../../../../single-page-application/index.md)  
>  ASP.NET 单页面应用程序 (SPA) 可帮助你生成应用程序包括使用 HTML 5、 CSS 3 和 JavaScript 的重要客户端交互。 ASP.NET 和 Web Tools 2012.2 更新提供用于构建使用 knockout.js 和 ASP.NET Web API 的单页面应用程序的新模板。 除了新的 SPA 模板中，新社区创建的 SPA 模板也是可供下载的。
> 
> 除了四个主要的开发框架，ASP.NET 还提供其他技术，它们是一定要意识到并熟悉，但不是包括在本教程系列：
> 
> - [ASP.NET Web API](../../../../web-api/index.md) -一个框架，用于生成覆盖广泛的客户端，包括浏览器和移动设备的 HTTP 服务。
> - [ASP.NET SignalR](../../../../signalr/index.md) -轻松开发实时 web 功能的库。


### <a name="reviewing-the-project"></a>查看项目

在 Visual Studio 中，**解决方案资源管理器**窗口允许你管理的项目文件。 让我们看看已添加到你的应用程序中的文件夹**解决方案资源管理器**。 Web 应用程序模板将添加基本的文件夹结构：

![创建项目的解决方案资源管理器](create-the-project/_static/image5.png)

Visual Studio 创建一些初始文件夹和为你的项目的文件。 您将使用在本教程后面的第一个文件如下所示：

| **文件** | **目的** |
| --- | --- |
| *Default.aspx* | 通常第一页的浏览器中运行应用程序时显示。 |
| *Site.Master* | 页，您可以在你的应用程序中创建页一致布局和使用标准行为。 |
| *Global.asax* | 一个包含对应用程序级别和会话级由 ASP.NET 或 HTTP 模块引发的事件作出响应的代码的可选文件。 |
| *Web.config* | 应用程序的配置数据。 |

### <a name="running-the-default-web-application"></a>运行默认 Web 应用程序

默认 Web 应用程序提供丰富的体验根据内置功能和支持。 无需对默认 Web 窗体项目中的任何更改，应用程序已准备好在你本地的 Web 浏览器上运行。

1. 按***F5***时 Visual Studio 中的键。   
 应用程序将生成并在 Web 浏览器中显示。  

    ![创建项目-默认页](create-the-project/_static/image6.png)
2. 一旦必须已完成的审阅运行的应用程序，请关闭浏览器窗口。

此默认 Web 应用程序中有三个主要的网页： *Default.aspx* （主页）， *About.aspx*，和*Contact.aspx*。 可以从顶部导航栏访问上述每个页。 也有两个帐户文件夹中包含的其他页、 Register.aspx 页和 Login.aspx 页面。 这些两个页面，可以使用 ASP.NET 成员资格功能来创建、 存储和验证用户凭据。

## <a name="aspnet-web-forms-background"></a>ASP.NET Web 窗体的背景

ASP.NET Web 窗体是基于 Microsoft ASP.NET 技术，在其中动态在服务器运行的代码生成到浏览器或客户端设备的 Web 页面输出的页。 ASP.NET Web 窗体页自动呈现功能，如样式、 布局和等等的正确符合浏览器的 HTML。 Web 窗体并符合支持的.NET 公共语言运行时，如 Microsoft Visual Basic 和 Microsoft Visual C# 任何语言。 此外，基于 Web 窗体[Microsoft.NET Framework](https://msdn.microsoft.com/vstudio/aa496123)，其中提供的好处，例如托管的环境、 类型安全和继承。

ASP.NET Web 窗体页运行时，此页将经历生命周期中将执行一系列处理步骤。 这些步骤包括初始化，实例化控件、 还原和保持状态，运行事件处理程序代码，以及进行呈现。 当你变得更熟悉的 ASP.NET Web 窗体的能力，务必了解[ASP.NET 页面生命周期](https://msdn.microsoft.com/library/ms178472(v=vs.100).aspx)，以便你可以在适当的生命周期阶段的预期的效果编写代码。

当 Web 服务器收到页请求时，它找到页面、 处理它，将其发送到浏览器中，然后放弃所有页信息。 如果用户再次请求同一页上，服务器则会重复整个序列，重新处理从零开始的页。 将另一种方法，一台服务器具有它有处理页的页没有内存是无状态。 ASP.NET 页框架会自动处理维护你的页面和其控件的状态的任务，并且它提供你以显式方式维护应用程序特定信息的状态。

> [!TIP] 
> 
> **在 Web 窗体应用程序模板中的 web 应用程序功能**
> 
> ASP.NET Web 窗体应用程序模板提供了一套丰富的内置功能。 它不仅可为你提供*Home.aspx*页上， *About.aspx*页上， *Contact.aspx*页上，但还包括成员资格功能注册用户和保存其凭据，以便他们可以登录到你的网站。 此概述提供了有关的一些功能包含在 ASP.NET Web 窗体应用程序模板和 Wingtip Toys 应用程序中如何使用它们的详细信息。
> 
> **成员身份**
> 
> [ASP.NET](https://msdn.microsoft.com/library/yh26yfzy.aspx)标识将用户的凭据存储在由应用程序创建的数据库。 你的用户登录时，应用程序通过读取数据库来验证其凭据。 你的项目的*帐户*文件夹包含实现的成员身份的各个部分的文件： 注册、 登录、 更改密码，并授予的访问权限。 此外，ASP.NET Web 窗体支持 OAuth 和 OpenID。 这些身份验证增强功能使用户能够登录到你的站点使用现有凭据，从 Facebook、 Twitter、 Windows Live，和 Google 作为此类帐户。
> 
> ![创建项目的解决方案资源管理器 （ASP.NET 标识）](create-the-project/_static/image7.png)
> 
> 默认情况下，此模板创建的 SQL Server Express LocalDB，附带了 Visual Studio Express 2013 for Web 开发数据库服务器实例上使用默认数据库名称的成员资格数据库。
> 
> **SQL Server Express LocalDB**
> 
> [SQL Server Express LocalDB](https://technet.microsoft.com/library/hh510202.aspx)是具有许多可编程性功能的 SQL Server 数据库的 SQL Server 的轻量版本。 SQL Server Express LocalDB 在用户模式下运行，并包含具有安装系统必备组件的短列表的快速的零配置安装。 Microsoft SQL Server、 任何数据库或 TRANSACT-SQL 代码中可以从移动 SQL Server Express LocalDB 到 SQL Server 和 SQL Azure 无需升级即可。 因此，SQL Server Express LocalDB 还可以用作进行开发人员环境，面向所有版本的 SQL Server 应用程序。 SQL Server Express LocalDB 启用功能，例如存储的过程、 用户定义函数和聚合，.NET Framework 集成、 空间类型和其他人在 SQL Server Compact 中不可用。
> 
> **母版页**
> 
> [ASP.NET 母版页](https://msdn.microsoft.com/library/wtxbf3hh.aspx)定义一致的外观和行为的所有页在你的应用程序中。 主控页的布局合并各个内容页，以便生成的最后一页，则用户将看到的内容。 在 Wingtip Toys 应用程序，你修改*Site.master*母版页以便 Wingtip Toys 网站中的所有页都共享同一个独特的徽标和导航栏。
> 
> **HTML5**
> 
> ASP.NET Web 窗体应用程序模板支持[HTML5](http://www.w3schools.com/html/html5_intro.asp)，这是最新版本的 HTML 标记语言。 HTML5 支持新的元素和功能，它可以更轻松地创建网站。
> 
> **Modernizr**
> 
> 对于不支持 HTML5 的浏览器，你可以使用[Modernizr](http://www.modernizr.com/)。 Modernizr 是开放源代码 JavaScript 库，可以检测是否浏览器支持 HTML5 功能，如果不启用它们。 在 ASP.NET Web 窗体应用程序模板中，Modernizr 作为 NuGet 包进行安装。
> 
> **Bootstrap**
> 
> Visual Studio 2013 项目模板使用[Bootstrap](http://getbootstrap.com/)，由 Twitter 的布局和主题框架。 Bootstrap 使用 CSS3 提供响应灵敏的设计，这意味着布局可以动态调整以适应不同的浏览器窗口大小。 Bootstrap 的主题功能还可用于轻松地影响应用程序的外观和行为的更改。 默认情况下，Visual Studio 2013 中的 ASP.NET Web 应用程序模板包括 Bootstrap 作为 NuGet 程序包。
> 
> **NuGet 包**
> 
> ASP.NET Web 窗体应用程序模板包括一套[NuGet](http://www.nuget.org/)包。 这些包提供开放源代码库和工具的窗体中的组件化的功能。 还有许多丰富的程序包，以帮助你创建和测试你的应用程序。 Visual Studio，可以轻松添加、 删除和更新 NuGet 程序包。 开发人员可以创建，也将包添加到 NuGet。
> 
> ![创建项目-NuGet 对话框](create-the-project/_static/image8.png)
> 
> 在安装包时，NuGet 将文件复制到你的解决方案，并自动使所需的任何更改，例如添加引用，以及更改与 Web 应用程序关联的配置。 如果你决定删除库，NuGet 将删除文件，并反转任何它所做的更改在你的项目，以便保留没有混乱。 NuGet 是可从**工具**Visual Studio 中的菜单。
> 
> **jQuery**
> 
> [jQuery](http://jquery.com/)是一个快速且简洁的 JavaScript 库，简化了 HTML 文档遍历、 事件处理，对进行动画处理，和快速的 web 开发的 Ajax 交互。 JQuery JavaScript 库作为 NuGet 程序包包含 ASP.NET Web 窗体应用程序模板中。
> 
> **非介入式验证**
> 
> 内置的验证程序控件已配置为使用非介入式 JavaScript 客户端验证逻辑。 这显著缩短 JavaScript 呈现的页标记中的内联量，并减少总体的页大小。 非介入式验证全局添加到基于中的设置的 ASP.NET Web 窗体应用程序模板&lt;appSettings&gt;元素*Web.config*在应用程序的根目录的文件。
> 
> **实体框架代码优先**
> 
> ASP.NET Web 窗体应用程序模板中的功能，除了 Wingtip Toys 应用程序使用[Entity Framework Code First](https://weblogs.asp.net/scottgu/archive/2010/12/08/announcing-entity-framework-code-first-ctp5-release.aspx)，这是当您使用数据时，可以以代码为中心的开发一个 NuGet 库。 简单地说，它将创建的数据库部分，根据你编写的代码，为你的应用程序。 使用实体框架，你可以检索和处理数据作为强类型化的对象。 这样，你专注于业务逻辑在你的应用程序，而不是访问数据的方式的详细信息。
> 
> 有关已安装的库和包附带的 ASP.NET Web 窗体模板的其他信息，请参阅安装 NuGet 程序包的列表。 若要执行此操作，在 Visual Studio 中创建一个新的 Web 窗体项目，选择**工具** - &gt; **库程序包管理器** - &gt; **管理解决方案的 NuGet 包**，然后选择**安装包**中**管理 NuGet 包**对话框。


### <a name="touring-visual-studio"></a>Touring Visual Studio

Visual Studio 中的主窗口包含**解决方案资源管理器**、**服务器资源管理器**(**数据库资源管理器**速成版中)，则**属性窗口**、**工具箱**、**工具栏**，和**文档窗口**。

![创建项目-NuGet 对话框](create-the-project/_static/image9.png)

有关 Visual Studio 的详细信息，请参阅[Visual 指南 Visual Web Developer](https://msdn.microsoft.com/library/ee410104.aspx)。

## <a name="summary"></a>摘要

在本教程中具有创建、 查看和运行默认的 Web 窗体应用程序。 在查阅默认 Web 窗体应用程序的不同功能并了解有关如何使用 Visual Studio 环境的一些基础知识。 在以下教程中，你将创建数据访问层。

## <a name="additional-resources"></a>其他资源

[选择编程模型的权限](../../../videos/how-do-i/choosing-the-right-programming-model.md)   
[与网站项目的 web 应用程序项目](https://msdn.microsoft.com/library/dd547590.aspx)   
[ASP.NET Web 窗体页概述](https://msdn.microsoft.com/library/428509ah.aspx)

>[!div class="step-by-step"]
[上一页](introduction-and-overview.md)
[下一页](create_the_data_access_layer.md)
