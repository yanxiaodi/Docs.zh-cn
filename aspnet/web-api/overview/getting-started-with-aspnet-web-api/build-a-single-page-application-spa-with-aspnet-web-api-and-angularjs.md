---
uid: web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs
title: "动手实验： 生成与 ASP.NET Web API 和 Angular.js 的单页面应用程序 (SPA) |Microsoft 文档"
author: rick-anderson
description: "在传统 web 应用程序，客户端 （浏览器） 中请求页面来启动与服务器通信。 服务器然后处理该请求..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/30/2015
ms.topic: article
ms.assetid: 719727b7-bef3-45ad-bfe9-ba5bcdb2305f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs
msc.type: authoredcontent
ms.openlocfilehash: 9a748628d53878be380869ac5327de0111d2284d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="hands-on-lab-build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs"></a>动手实验： 生成与 ASP.NET Web API 和 Angular.js 的单页面应用程序 (SPA)
====================
通过[Web 营地团队](https://twitter.com/webcamps)

[下载 Web 营地培训工具包](http://aka.ms/webcamps-training-kit)

> 在传统 web 应用程序，客户端 （浏览器） 中请求页面来启动与服务器通信。 然后，在服务器处理请求，并将页面的 HTML 发送到客户端。 在后续交互与页 – 例如用户导航到的链接，或提交的窗体具有数据 – 新请求发送到服务器，并重新启动流： 服务器处理请求并对新的操作请求的响应中的浏览器发送一个新页ed 客户端。
> 
> 在单页面应用程序 (Spa) 中整个页面已加载，在浏览器后的初始请求，但后续交互发生通过 Ajax 请求。 这意味着，在浏览器具有更新仅已更改; 此页面的部分没有无需重新加载整个页面。 SPA 方法可以降低应用程序以响应用户操作，以此来更流畅体验所花费的时间。
> 
> SPA 的体系结构涉及到一些在传统 web 应用程序中不存在的难题。 但是，新兴技术，如 ASP.NET Web API，JavaScript 框架类似 AngularJS 和 CSS3 提供的新样式功能使其相当容易设计和构建 Spa。
> 
> 在此动手实验，你将利用这些技术来实现专家 Quiz，根据 SPA 概念琐事网站。 首先将实现与 ASP.NET Web API 公开要检索的测验问题和存储答案的所需的终结点的服务层。 然后，你将构建使用 AngularJS 和 CSS3 转换效果的丰富且高度可响应用户界面。
> 
> 在 Web 营地培训工具包中，在包括所有的示例代码和代码段[http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit)。


## <a name="overview"></a>概述

<a id="Objectives"></a>
### <a name="objectives"></a>目标

在此动手实验中，你将了解如何：

- 创建 ASP.NET Web API 服务，以发送和接收 JSON 数据
- 创建用户界面的响应使用 AngularJS
- 增强 CSS3 转换的 UI 体验

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>先决条件

完成本动手实验需要以下：

- [Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/)或更高版本

<a id="Setup"></a>
### <a name="setup"></a>安装

若要运行本动手实验中的练习，你将需要先设置你的环境。

1. 打开 Windows 资源管理器并浏览到本实验的**源**文件夹。
2. 右键单击**Setup.cmd**和选择**以管理员身份运行**以启动安装过程将配置您的环境并安装本实验的 Visual Studio 代码段。
3. 如果显示用户帐户控制对话框中，确认操作以继续。

> [!NOTE]
> 请确保您已在运行安装程序之前检查本实验的所有依赖项。


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>使用代码片段

在整个实验文档中，你将指示要插入代码块。 为方便起见，大多数的此代码提供作为 Visual Studio 代码段，你可以从访问在 Visual Studio 2013，为了避免必须手动添加它。

> [!NOTE]
> 每个练习均附带的开始解决方案位于**开始**本练习，您可以完成相互独立地每个练习的文件夹。 请注意在练习过程中添加的代码段缺少这些开始解决方案中，并且可能不工作，直到完成练习。 在练习的源代码，你将找到**结束**包含具有完成相应练习中的步骤得到的代码的 Visual Studio 解决方案文件夹。 如果您在演练本动手实验需要其他帮助，你可以使用这些解决方案作为指南。


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>练习

本动手实验包括以下练习：

1. [创建一个 Web API](#Exercise1)
2. [创建 SPA 接口](#Exercise2)

估计时间来完成该实验： **60 分钟**

> [!NOTE]
> 当你首次启动 Visual Studio 时，你必须选择一个预定义的设置集合。 每个预定义的集合用于匹配特定的开发风格和确定窗口布局、 编辑器行为、 IntelliSense 代码段和对话框选项。 在本实验中的过程描述完成给定的任务 Visual Studio 中使用时所需的操作**常规开发设置**集合。 如果您为开发环境选择不同的设置集合，则表明可能存在你应考虑到帐户的步骤中的差异。


<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-web-api"></a>练习 1： 创建 Web API

SPA 的要点之一是服务层。 它负责处理发送的 UI 和到该调用的响应中返回数据的 Ajax 调用。 检索的数据应以以便分析和使用的客户端计算机可读格式显示。

Web API 框架是 ASP.NET 堆栈的一部分，旨在方便地实现 HTTP 服务，通常发送和接收 JSON 或 XML 格式的数据，通过 RESTful API。 在本练习中将创建网站以承载专家测验应用程序，然后实现后端服务来公开和保持使用 ASP.NET Web API 的测验数据。

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-the-initial-project-for-geek-quiz"></a>任务 1 – 为专家测验创建初始项目

在此任务将开始创建新的 ASP.NET MVC 项目支持 ASP.NET Web API 基于**一个 ASP.NET**项目附带了 Visual Studio 的类型。 **一个 ASP.NET**统一所有 ASP.NET 技术和为你提供混合并将根据需要对其进行匹配的选项。 然后，你将添加实体框架模型类和数据库 initializator 要插入的测验问题。

1. 打开**Visual Studio Express 2013 for Web**和选择**文件 |新建项目...**启动一个新的解决方案。

    ![创建新的项目](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image1.png "创建新项目")

    *创建新项目*
2. 在**新项目**对话框中，选择**ASP.NET Web 应用程序**下**Visual C# |Web**选项卡。请确保**.NET Framework 4.5**是选择，将其命名*GeekQuiz*，选择**位置**单击**确定**。

    ![创建新的 ASP.NET Web 应用程序项目](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image2.png "创建新的 ASP.NET Web 应用程序项目")

    *创建新的 ASP.NET Web 应用程序项目*
3. 在**新建 ASP.NET 项目**对话框中，选择**MVC**模板，然后选择**Web API**选项。 此外，请确保**身份验证**选项设置为**单个用户帐户**。 单击“确定”  继续。

    ![使用 MVC 模板，其中包括 Web API 组件创建新项目](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image3.png)

    *使用 MVC 模板，其中包括 Web API 组件创建新项目*
4. 在**解决方案资源管理器**，右键单击**模型**文件夹**GeekQuiz**项目，然后选择**添加 |现有项...**.

    ![添加现有项](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image4.png "添加现有项")

    *添加现有项*
5. 在**添加现有项**对话框框中，导航到**源/资产/模型**文件夹，然后选择所有文件。 单击 **“添加”**。

    ![添加模型资产](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image5.png "添加模型资产")

    *添加模型资产*

    > [!NOTE]
    > 通过将添加这些文件，你要添加数据模型、 实体框架数据库上下文和专家 Quiz 应用程序的数据库初始值设定项。
    > 
    > **Entity Framework (EF)**是对象关系映射器 (ORM)，可用于创建数据访问应用程序使用而不是直接使用关系存储架构编程概念应用程序模型编程。 你可以了解有关实体框架[此处](../../../entity-framework.md)。
    > 
    > 下面是你刚添加的类的说明：
    > 
    > - **TriviaOption:**表示与测验问题关联的单个选项
    > - **TriviaQuestion:**表示测验问题，并公开通过关联的选项**选项**属性
    > - **TriviaAnswer:**表示通过对测验问题的响应中的用户选择的选项
    > - **TriviaContext:**表示专家 Quiz 应用程序的实体框架数据库上下文。 此类派生自**DContext**并公开**DbSet**表示集合的上面所述的实体的属性。
    > - **TriviaDatabaseInitializer:**的实体框架初始值设定项的实现**TriviaContext**类继承自**CreateDatabaseIfNotExists**。 此类的默认行为是创建数据库，仅当它不存在中, 插入实体指定**种子**方法。
6. 打开**Global.asax.cs**文件并添加以下 using 语句。

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample1.cs)]
7. 将以下代码添加的开头**应用程序\_启动**方法以设置**TriviaDatabaseInitializer**作为数据库初始值设定项。

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample2.cs)]
8. 修改**主页**控制器限制对访问身份验证的用户。 若要执行此操作，打开**HomeController.cs**文件**控制器**文件夹并添加**Authorize**属性设为**HomeController**类定义。

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample3.cs)]

    > [!NOTE]
    > **Authorize**筛选检查以确定用户进行身份验证。 如果用户未通过身份验证，则将返回 HTTP 状态代码 401 （未授权），而无需调用该操作。 你可以应用筛选器全局范围内，在控制器级别，或各个操作级别。
9. 现在，您将自定义的 web 页面，并且该品牌的布局。 若要执行此操作，打开 **\_Layout.cshtml**文件**视图 |共享**文件夹和更新的内容**&lt;标题&gt;**元素替换*My ASP.NET Application*与*专家测验*.

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample4.cshtml)]
10. 在相同的文件中，通过删除更新的导航栏*有关*和*联系人*链接和重命名*主页*链接到*播放*。 此外，重命名*应用程序名称*链接到*专家 Quiz*。 导航栏的 HTML 应类似下面的代码。

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample5.cshtml)]
11. 通过替换来更新布局页的页脚*My ASP.NET Application*与*专家 Quiz*。 若要执行此操作，将内容**&lt;页脚&gt;**元素替换为以下突出显示代码。

    [!code-html[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample6.html)]

<a id="Ex1Task2"></a>
#### <a name="task-2--creating-the-triviacontroller-web-api"></a>任务 2 – 创建 TriviaController Web API

你可以在上一任务中，创建专家 Quiz web 应用程序的初始结构。 现在，你将生成一个简单的 Web API 服务，它与测验数据模型进行交互并公开以下操作：

- **GET/api/琐事**： 从通过身份验证的用户来响应的测验列表中检索下一个问题。
- **POST/api/琐事**： 存储已经过身份验证的用户指定的测验答案。

提供 Visual Studio 的 ASP.NET 基架工具将用于创建 Web API 控制器类的基线。

1. 打开**WebApiConfig.cs**文件**应用\_启动**文件夹。 此文件定义的 Web API 服务，如如何路由映射到 Web API 控制器操作的配置。
2. 添加以下 using 语句开头的文件。

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample7.cs)]
3. 添加以下突出显示的代码**注册**方法来全局配置通过 Web API 的操作方法检索到的 JSON 数据的格式化程序。

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample8.cs)]

    > [!NOTE]
    > **CamelCasePropertyNamesContractResolver**会自动将转换到的属性名称*camel*大小写形式，这是 JavaScript 中的属性名称的常规约定。
4. 在**解决方案资源管理器**，右键单击**控制器**文件夹**GeekQuiz**项目，然后选择**添加 |新的基架的项...**.

    ![创建新的基架的项](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image6.png "创建新的基架的项")

    *创建新的基架的项*
5. 在**添加基架**对话框框中，请确保**常见**的左窗格中选择节点。 然后，选择**Web API 2 Controller-Empty**模板在中心窗格中单击**添加**。

    ![选择 Web API 2 控制器空模板](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image7.png "选择 Web API 2 控制器空模板")

    *选择 Web API 2 控制器空模板*

    > [!NOTE]
    > **ASP.NET 基架**是用于 ASP.NET Web 应用程序的代码生成框架。 Visual Studio 2013 包含为 MVC 和 Web API 项目的预安装的代码生成器。 当你想要快速添加代码以减少的时间量与数据模型交互所需开发标准数据操作，应在项目中使用基架。
    > 
    > 基架过程还可确保在项目中安装了所有必需的依赖项。 例如，如果您首先一个空 ASP.NET 项目，然后使用基架添加一个 Web API 控制器，所需的 Web API NuGet 程序包和引用将自动添加到你的项目。
6. 在**添加控制器**对话框中，键入*TriviaController*中**控制器名称**文本框中，然后单击**添加**。

    ![添加琐事控制器](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image8.png "添加琐事控制器")

    *添加琐事控制器*
7. **TriviaController.cs**文件然后添加到**控制器**文件夹**GeekQuiz**包含一个空的项目**TriviaController**类。 添加以下 using 语句开头的文件。

    (代码段- *AspNetWebApiSpa-Ex1-TriviaControllerUsings*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample9.cs)]
8. 将以下代码添加的开头**TriviaController**类来定义、 初始化和释放**TriviaContext**控制器中的实例。

    (代码段- *AspNetWebApiSpa-Ex1-TriviaControllerContext*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample10.cs)]

    > [!NOTE]
    > **释放**方法**TriviaController**时，将调用**释放**方法**TriviaContext**实例，这可确保所有发布使用 context 对象的资源时**TriviaContext**实例已释放或垃圾回收。 这包括关闭所有打开的实体框架的数据库连接。
9. 在末尾添加以下帮助器方法**TriviaController**类。 此方法从有待回答的精选由指定的用户数据库中检索下面的测验问题。

    (代码段- *AspNetWebApiSpa-Ex1-TriviaControllerNextQuestion*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample11.cs)]
10. 添加以下**获取**到操作方法**TriviaController**类。 此操作方法调用**NextQuestionAsync**在上面的步骤以检索经过身份验证的用户的下一步问题中定义的帮助器方法。

    (代码段- *AspNetWebApiSpa-Ex1-TriviaControllerGetAction*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample12.cs)]
11. 在末尾添加以下帮助器方法**TriviaController**类。 此方法的数据库中存储指定的应答，并返回一个布尔值，该值指示答案正确。

    (代码段- *AspNetWebApiSpa-Ex1-TriviaControllerStoreAsync*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample13.cs)]
12. 添加以下**Post**到操作方法**TriviaController**类。 此操作方法将关联的身份验证的用户和调用的答案**StoreAsync**帮助器方法。 然后，它将响应发送与帮助器方法返回的布尔值。

    (代码段- *AspNetWebApiSpa-Ex1-TriviaControllerPostAction*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample14.cs)]
13. 修改要通过将添加到已经过身份验证的用户限制访问的 Web API 控制器**Authorize**属性设为**TriviaController**类定义。

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample15.cs)]

<a id="Ex1Task3"></a>
#### <a name="task-3--running-the-solution"></a>任务 3 – 运行解决方案

在此任务中，你将验证你在上一任务中生成的 Web API 服务按预期工作。 你将使用 Internet Explorer **F12 开发人员工具**捕获网络流量，以查看全部响应从 Web API 服务。

> [!NOTE]
> 请确保**Internet Explorer**中选择**启动**按钮位于 Visual Studio 工具栏上。
> 
> ![Internet Explorer 选项](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image9.png)


1. 按**F5**运行该解决方案。 **登录**页应出现在浏览器。

    > [!NOTE]
    > 当应用程序启动时，默认 MVC 路由触发时，它们的默认值映射到**索引**操作**HomeController**类。 由于**HomeController**仅限于经过身份验证的用户 (请记住修饰该类**Authorize**练习 1 中的属性) 和存在尚没有用户通过身份验证，应用程序原始请求重定向到登录页。

    ![运行解决方案](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image10.png "运行解决方案")

    *运行解决方案*
2. 单击**注册**来创建新用户。

    ![注册一个新用户](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image11.png "注册一个新用户")

    *注册一个新用户*
3. 在**注册**页上，输入**用户名**和**密码**，然后单击**注册**。

    ![注册页](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image12.png "注册页")

    *注册页*
4. 应用程序注册新帐户和用户经过身份验证和重定向回主页。

    ![用户进行身份验证](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image13.png "身份验证的用户")

    *用户进行身份验证*
5. 在浏览器中，按**F12**以打开**开发人员工具**面板。 按**CTRL + 4**或单击**网络**图标，，然后单击绿色箭头按钮，以开始捕获网络流量。

    ![启动 Web API 网络捕获](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image14.png "启动 Web API 网络捕获")

    *启动 Web API 网络捕获*
6. 追加**api/琐事**到浏览器的地址栏中的 URL。 现在将检查从响应的详细信息**获取**中的操作方法**TriviaController**。

    ![检索通过 Web API 的下一步问题数据](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image15.png "检索下一步问题数据通过 Web API")

    *检索下一步问题数据通过 Web API*

    > [!NOTE]
    > 下载完成后，系统将提示与下载的文件进行操作。 使对话框保持打开状态，以能够查看响应内容通过开发人员工具窗口。
7. 现在，你将检查响应的正文。 若要执行此操作，请单击**详细信息**选项卡，然后单击**响应正文**。 你可以检查下载的数据是具有属性的对象**选项**(这是一份**TriviaOption**对象)， **id**和**标题**对应于这样的**TriviaQuestion**类。

    ![查看 Web API 响应正文](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image16.png "查看 Web API 响应正文")

    *查看 Web API 响应正文*
8. 返回到 Visual Studio，然后按**SHIFT + F5**来停止调试。

<a id="Exercise2"></a>
### <a name="exercise-2-creating-the-spa-interface"></a>练习 2： 创建 SPA 接口

在本练习中您将首先生成专家 Quiz 的 web 前端部分将重点放在单页面应用程序交互使用**AngularJS**。 然后，你将增强 CSS3 执行丰富的动画并提供视觉效果的上下文切换到下一步转换从一个问题时的用户体验。

<a id="Ex2Task1"></a>
#### <a name="task-1--creating-the-spa-interface-using-angularjs"></a>任务 1 – 创建使用 AngularJS 的 SPA 接口

在此任务将使用**AngularJS**实现专家 Quiz 应用程序的客户端。 **AngularJS**是一种开放源代码 JavaScript 框架，它增加基于浏览器的应用程序与*模型-视图-控制器*(MVC) 功能，方便了这两个开发和测试。

首先，将通过从 Visual Studio 的程序包管理器控制台安装 AngularJS。 然后，你将创建要提供专家测验应用以及要呈现的测验问题和解答使用 AngularJS 模板引擎的视图的行为的控制器。

> [!NOTE]
> 有关 AngularJS 的详细信息，请参阅[ [http://angularjs.org/](http://angularjs.org/)](http://angularjs.org/)。


1. 打开**Visual Studio Express 2013 for Web**并打开**GeekQuiz.sln**解决方案位于**源/Ex2-CreatingASPAInterface/开始**文件夹。 或者，继续与解决方案并在上一练习中获取。
2. 打开**程序包管理器控制台**从**工具** | **库程序包管理器**。 键入以下命令以安装**AngularJS.Core** NuGet 包。

    [!code-powershell[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample16.ps1)]
3. 在**解决方案资源管理器**，右键单击**脚本**文件夹**GeekQuiz**项目，然后选择**添加 |新文件夹**。 将文件夹命名为**应用**按**Enter**。
4. 右键单击**应用**文件夹只需创建并选择**添加 |JavaScript 文件**。

    ![创建新的 JavaScript 文件](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image17.png)

    *创建新的 JavaScript 文件*
5. 在**指定项名称**对话框中，键入*测验控制器*中**项名称**文本框中，然后单击**确定**。

    ![命名新的 JavaScript 文件](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image18.png)

    *命名新的 JavaScript 文件*
6. 在**测验 controller.js**文件中，添加以下代码以声明和初始化 AngularJS **QuizCtrl**控制器。

    (代码段- *AspNetWebApiSpa-Ex2-AngularQuizController*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample17.js)]

    > [!NOTE]
    > 构造函数的**QuizCtrl**控制器需要名为的注射参数**$scope**。 作用域的初始状态应设置在构造函数中通过附加到属性**$scope**对象。 属性包含**视图模型**，并可供模板时注册了该控制器。
    > 
    > **QuizCtrl**控制器定义在名为模块内**QuizApp**。 模块是工作的让你单元将应用程序分解为单独的组件。 使用模块的主要优势是易于理解的代码，并便于单元测试、 可重用性和可维护性。
7. 若要对从视图触发的事件做出响应，现在将将行为添加到作用域。 将以下代码添加在结束**QuizCtrl**控制器定义**nextQuestion**函数中**$scope**对象。

    (代码段- *AspNetWebApiSpa-Ex2-AngularQuizControllerNextQuestion*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample18.js)]

    > [!NOTE]
    > 此函数可检索下一个问题从**琐事**Web API 在上一练习中创建和附加到的问题数据**$scope**对象。
8. 将下面的代码插入的结尾处**QuizCtrl**控制器定义**sendAnswer**函数中**$scope**对象。

    (代码段- *AspNetWebApiSpa-Ex2-AngularQuizControllerSendAnswer*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample19.js)]

    > [!NOTE]
    > 此函数将发送到用户的所选回答**琐事**Web API，并将存储中的结果 – 即如果答案不正确，或未 – **$scope**对象。
    > 
    > **NextQuestion**和**sendAnswer**上面提供的函数使用 AngularJS **$http**对象抽象与通过 XMLHttpRequest Web API 的通信从浏览器的 JavaScript 对象。 AngularJS 支持使更高级别的抽象，以执行针对通过 RESTful Api 资源的 CRUD 操作的另一个服务。 AngularJS **$resource**对象有提供高级行为而无需与之交互的操作方法**$http**对象。 请考虑使用**$resource**方案需要 CRUD 模型的对象 (fore 信息，请参阅[$resource 文档](https://docs.angularjs.org/api/ngResource/service/$resource))。
9. 下一步是创建定义测验的视图的 AngularJS 模板。 若要执行此操作，打开**Index.cshtml**文件**视图 |主页**文件夹，然后替换其中的内容替换为以下代码。

    (代码段- *AspNetWebApiSpa-Ex2-GeekQuizView*)

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample20.cshtml)]

    > [!NOTE]
    > AngularJS 模板是使用来自模型和控制器的信息可将静态标记转换为该用户将看到在浏览器中的动态视图的声明性规范。 AngularJS 元素和可以在模板中使用的元素属性的示例如下：
    > 
    > - **Ng 应用**指令指示 AngularJS 表示应用程序的根元素的 DOM 元素。
    > - **Ng 控制器**指令将控制器附加到 DOM 指令的声明位置的点处。
    > - 大括号表示法**{{}}**表示到控制器中定义的作用域属性的绑定。
    > - **Ng 单击**指令用于调用以响应用户单击作用域中定义的函数。
10. 打开**Site.css**文件**内容**文件夹和文件可供测验视图的外观和感觉的末尾添加以下突出显示的样式。

    (代码段- *AspNetWebApiSpa-Ex2-GeekQuizStyles*)

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample21.css)]

<a id="Ex2Task2"></a>
#### <a name="task-2--running-the-solution"></a>任务 2 – 运行解决方案

在将执行此任务使用新的用户的解决方案接口你使用 AngularJS 来回答一些测验问题生成。

1. 按**F5**运行该解决方案。
2. 注册新的用户帐户。 若要执行此操作，请按照在练习 1 中，任务 3 中所述的注册步骤。

    > [!NOTE]
    > 如果使用从上一练习解决方案时，你可以登录之前创建的用户帐户。
3. **主页**页应显示，其中显示测验的第一个问题。 通过单击某个选项来回答问题。 这将触发**sendAnswer**更早版本，定义的函数，它将发送到所选的选项**琐事**Web API。

    ![回答问题](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image19.png "回答问题")

    *回答问题*
4. 单击后的一个按钮，应显示答案。 单击**下一个问题**以显示下面的问题。 这将触发**nextQuestion**控制器中定义的函数。

    ![请求的下一个问题](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image20.png "请求下一个问题")

    *请求的下一个问题*
5. 应显示下一个问题。 继续根据需要多次通过回答问题。 完成所有问题后，您应返回到第一个问题。

    ![另一个问题](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image21.png "另一个问题")

    *下一个问题*
6. 返回到 Visual Studio，然后按**SHIFT + F5**来停止调试。

<a id="Ex2Task3"></a>
#### <a name="task-3--creating-a-flip-animation-using-css3"></a>任务 3 – 创建使用 CSS3 翻转动画

在此任务将使用 CSS3 属性通过添加翻转效果时问题答案和检索下一个问题时执行丰富的动画。

1. 在**解决方案资源管理器**，右键单击**内容**文件夹**GeekQuiz**项目，然后选择**添加 |现有项...**.

    ![将现有项添加到内容文件夹](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image22.png "将现有项添加到内容文件夹")

    *将现有项添加到内容文件夹*
2. 在**添加现有项**对话框框中，导航到**源/资产**文件夹，然后选择**Flip.css**。 单击 **“添加”**。

    ![从资产添加 Flip.css 文件](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image23.png "从资产添加 Flip.css 文件")

    *从资产添加 Flip.css 文件*
3. 打开**Flip.css**文件您刚刚添加并检查其内容。
4. 找到**翻转转换**注释。 下面的注释样式使用 CSS**透视**和**rotateY**转换生成&quot;卡翻转&quot;效果。

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample22.css)]
5. 找到**期间翻转背面窗格隐藏**注释。 该注释下面的样式时它们将面临离开查看器通过设置隐藏的平面的后端**背面可见性**CSS 属性*隐藏*。

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample23.css)]
6. 打开**BundleConfig.cs**文件**应用\_启动**文件夹并添加对引用**Flip.css**文件中 **&quot;~/Content/css&quot;** 样式捆绑包

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample24.cs)]
7. 按**F5**运行解决方案，并登录你的凭据。
8. 通过单击某个选项来回答问题。 视图之间转换时，请注意翻转的效果。

    ![回答的问题将有翻转效果](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image24.png "回答的问题将有翻转的效果")

    *回答的问题将有翻转的效果*
9. 单击**下一个问题**检索下面的问题。 翻转效果应该再次出现。

    ![检索的翻转的效果与下面的问题](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image25.png "检索的翻转的效果与下面的问题")

    *检索的翻转的效果与下面的问题*

* * *

<a id="Summary"></a>
## <a name="summary"></a>摘要

通过完成本动手实验你已学习如何：

- 创建 ASP.NET Web API 控制器使用 ASP.NET 基架
- 实现一个 Web API 获取操作，以便检索下一步的测验问题
- 实现 Web API 后操作来存储测验答案
- 从 Visual Studio 包管理器控制台安装 AngularJS
- 实现 AngularJS 模板和控制器
- CSS3 转换用于执行动画效果
