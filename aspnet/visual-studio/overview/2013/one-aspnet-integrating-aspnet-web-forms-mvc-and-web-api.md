---
uid: visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
title: "动手实验： 一个 ASP.NET： 集成 ASP.NET Web 窗体、 MVC 和 Web API |Microsoft 文档"
author: rick-anderson
description: "ASP.NET 是一个框架，用于生成 Web 站点、 应用程序和服务使用专用的技术，如 MVC、 Web API 及其他。 与 ASP.NET h 的扩展..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: 4fe2558d-67cc-4d12-a5c1-6fb9f6f16137
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
msc.type: authoredcontent
ms.openlocfilehash: 55109723e566a9f7c66c1a59414377b05dbec760
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="hands-on-lab-one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api"></a>动手实验： 一个 ASP.NET： 将 ASP.NET Web 窗体、 MVC 和 Web API 集成
====================
通过[Web 营地团队](https://twitter.com/webcamps)

[下载 Web 营地培训工具包](http://aka.ms/webcamps-training-kit)

> ASP.NET 是一个框架，用于生成 Web 站点、 应用程序和服务使用专用的技术，如 MVC、 Web API 及其他。 ASP.NET 其创建后看到与的扩展和表示需要具有集成这些技术，在向工作了新的工作**一个 ASP.NET**。
> 
> Visual Studio 2013 引入了新的统一的项目系统，这样就可以生成应用程序，并使用一个项目中的所有 ASP.NET 技术。 此功能消除了需要在开始时的项目和它，坚持选择一种技术，并改为鼓励使用一个项目中的多个 ASP.NET 框架。
> 
> 在 Web 营地培训工具包中，在包括所有的示例代码和代码段[http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit)。


<a id="Overview"></a>
## <a name="overview"></a>概述

<a id="Objectives"></a>
### <a name="objectives"></a>目标

在此动手实验中，你将了解如何：

- 创建基于网站**一个 ASP.NET**项目类型
- 使用不同**ASP.NET**框架喜欢**MVC**和**Web API**同一项目中
- 标识的主要组件**ASP.NET**应用程序
- 利用**ASP.NET 基架**framework 框架来自动创建控制器和视图，以执行 CRUD 操作根据模型类
- 公开相同的信息的计算机和可读的格式为每个作业使用的正确工具集

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>先决条件

完成本动手实验需要以下：

- [Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/)或更高版本
- [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=301714)

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

1. [创建一个新的 Web 窗体项目](#Exercise1)
2. [创建使用基架 MVC 控制器](#Exercise2)
3. [创建使用基架 Web API 控制器](#Exercise3)

估计时间来完成该实验： **60 分钟**

> [!NOTE]
> 当你首次启动 Visual Studio 时，你必须选择一个预定义的设置集合。 每个预定义的集合用于匹配特定的开发风格和确定窗口布局、 编辑器行为、 IntelliSense 代码段和对话框选项。 在本实验中的过程描述完成给定的任务 Visual Studio 中使用时所需的操作**常规开发设置**集合。 如果您为开发环境选择不同的设置集合，则表明可能存在你应考虑到帐户的步骤中的差异。


<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-new-web-forms-project"></a>练习 1： 创建新的 Web 窗体项目

在本练习中，你将创建在 Visual Studio 2013 中使用新的 Web 窗体站点**一个 ASP.NET**统一项目体验，您便可轻松地集成在同一应用程序的 Web 窗体、 MVC 和 Web API 的组件。 然后将浏览生成的解决方案，并识别其部分，你将在最后看到操作中的 Web 站点。

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-a-new-site-using-the-one-aspnet-experience"></a>任务 1 – 创建新站点使用一个 ASP.NET 体验

在此任务中，您将开始在 Visual Studio 中创建新的 Web 站点基于**一个 ASP.NET**项目类型。 **一个 ASP.NET**统一所有 ASP.NET 技术和为你提供混合并将根据需要对其进行匹配的选项。 然后将应用程序中并排识别实时 Web 窗体、 MVC 和 Web API 的不同组件。

1. 打开**Visual Studio Express 2013 for Web**和选择**文件 |新建项目...**启动一个新的解决方案。

    ![创建新项目](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image1.png)

    *创建新项目*
2. 在**新项目**对话框中，选择**ASP.NET Web 应用程序**下**Visual C# |Web**选项卡，并确保**.NET Framework 4.5**选择。 将项目*MyHybridSite*，选择**位置**单击**确定**。

    ![新的 ASP.NET Web 应用程序项目](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image2.png)

    *创建新的 ASP.NET Web 应用程序项目*
3. 在**新建 ASP.NET 项目**对话框中，选择**Web 窗体**模板，然后选择**MVC**和**Web API**选项。 此外，请确保**身份验证**选项设置为**单个用户帐户**。 单击“确定”  继续。

    ![使用 Web 窗体模板，其中包括 Web API 和 MVC 组件创建新项目](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image3.png)

    *使用 Web 窗体模板，其中包括 Web API 和 MVC 组件创建新项目*
4. 你现在可以浏览生成解决方案的结构。

    ![浏览生成的解决方案](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image4.png)

    *浏览生成的解决方案*

    1. **帐户：**此文件夹包含要注册、 登录和管理应用程序的用户帐户的 Web 窗体页。 此文件夹时添加**单个用户帐户**在 Web 窗体项目模板的配置过程中选择身份验证选项。
    2. **模型：**此文件夹将包含表示应用程序数据的类。
    3. **控制器**和**视图**： 这些文件夹所需的**ASP.NET MVC**和**ASP.NET Web API**组件。 您将了解在下一步的练习中的 MVC 和 Web API 的技术。
    4. **Default.aspx**， **Contact.aspx**和**About.aspx**文件是预定义的 Web 窗体页面，其中你可以使用作为起始点构建特定于页你应用程序。 这些文件的编程逻辑驻留在单独的文件称为&quot;代码隐藏&quot;文件，该文件&quot;。 为&quot;或&quot;。 aspx.cs&quot;扩展 (具体取决于使用语言）。 代码隐藏逻辑服务器上运行，并动态生成你的页面的 HTML 输出。
    5. **Site.Master**和**Site.Mobile.Master**页在应用程序中定义外观和感觉和所有的页面的标准行为。
5. 双击**Default.aspx**文件浏览页面的内容。

    ![浏览 Default.aspx 页](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image5.png)

    *浏览 Default.aspx 页*

    > [!NOTE]
    > **页**文件顶部的指令定义的 Web 窗体页的属性。 例如， **MasterPageFile**特性指定为主路径页上的这种情况下， *Site.Master*页-和**Inherits**属性定义页后，可以继承的代码隐藏类。 此类位于由文件**代码隐藏**属性。
    > 
    > **Asp： 内容**控件保持页面 （文本、 标记和控件） 的实际内容，并映射到**asp: ContentPlaceHolder**母版页上的控件。 在这种情况下，将内呈现页面内容*主内容*中定义的控件*Site.Master*页。
6. 展开**应用\_启动**文件夹，请注意**WebApiConfig.cs**文件。 由于使用一个 ASP.NET 模板配置你的项目时包括 Web API，因此，visual Studio 生成的解决方案中包含该文件。
7. 打开**WebApiConfig.cs**文件。 在*WebApiConfig*类将查找映射 HTTP 与 Web API，配置将路由到**Web API 控制器**。

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample1.cs)]
8. 打开**RouteConfig.cs**文件。 内部*RegisterRoutes*方法将查找与 MVC，映射到 HTTP 路由关联的配置**MVC 控制器**。

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample2.cs)]

<a id="Ex1Task2"></a>
#### <a name="task-2--running-the-solution"></a>任务 2 – 运行解决方案

在此任务将运行生成的解决方案，浏览应用程序和它的某些功能，如 URL 重写和内置身份验证。

1. 若要运行解决方案，按**F5**或单击**启动**按钮位于工具栏上。 该应用程序主页应在浏览器中打开。

    ![运行解决方案](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image6.png)
2. 验证正在调用的 Web 窗体页。 若要执行此操作，请追加**/contact.aspx**到地址栏，然后按 URL **Enter**。

    ![友好 Url](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image7.png)

    *友好 Url*

    > [!NOTE]
    > 如你所见，URL 更改为**/请联系**。 从开始**ASP.NET 4**，URL 路由功能已添加到 Web 窗体，因此你可以编写 Url 喜欢 *[http://www.mysite.com/products/software](http://www.mysite.com/products/software)* 而不是*[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)*。 有关详细信息，请参阅[URL 路由](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md)。
3. 现在，您将了解到应用程序集成的身份验证流。 若要执行此操作，请单击**注册**页面的右上角。

    ![注册一个新用户](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image8.png)

    *注册一个新用户*
4. 在**注册**页上，输入**用户名**和**密码**，然后单击**注册**。

    ![注册页](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image9.png)

    *注册页*
5. 应用程序注册新帐户，并对用户身份验证。

    ![用户通过身份验证](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image10.png)

    *用户通过身份验证*
6. 返回到 Visual Studio，然后按**SHIFT + F5**来停止调试。

<a id="Exercise2"></a>
### <a name="exercise-2-creating-an-mvc-controller-using-scaffolding"></a>练习 2： 创建使用基架 MVC 控制器

在本练习中将利用 ASP.NET 基架框架提供的 Visual Studio 来创建 ASP.NET MVC 5 控制器操作与 Razor 视图可用于执行 CRUD 操作，而无需编写一行代码。 基架过程将使用 Entity Framework Code First 在 SQL 数据库中生成的数据上下文和数据库架构。

**有关实体框架代码优先**

Entity Framework (EF) 是对象关系映射器 (ORM)，可用于创建数据访问应用程序使用而不是直接使用关系存储架构编程概念应用程序模型编程。

Entity Framework Code First 建模工作流，可使用您自己的域类来表示在执行查询时，EF 依赖于该模型更改跟踪和更新函数。 使用 Code First 开发工作流，你不需要通过创建数据库或指定架构开始你的应用程序。 相反，你可以编写定义你的应用程序的最合适域模型对象的标准.NET 类，实体框架将为你创建数据库。

> [!NOTE]
> 你可以了解有关实体框架[此处](../../../entity-framework.md)。


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-new-model"></a>任务 1 – 创建新的模型

现在，你将定义**人员**类，该类将基架过程用于创建 MVC 控制器和视图的模型。 首先，将通过创建**人员**模型类，在控制器中的 CRUD 操作将自动创建和使用基架功能。

1. 打开**Visual Studio Express 2013 for Web**和**MyHybridSite.sln**解决方案位于**源/Ex2-MvcScaffolding/开始**文件夹。 或者，继续与解决方案并在上一练习中获取。
2. 在**解决方案资源管理器**，右键单击**模型**文件夹**MyHybridSite**项目，然后选择**添加 |类...**.

    ![添加人员模型类](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image11.png)

    *添加人员模型类*
3. 在**添加新项**对话框中，将文件*Person.cs*单击**添加**。

    ![创建个人模型类](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image12.png)

    *创建个人模型类*
4. 替换的内容**Person.cs**文件替换为以下代码。 按**CTRL + S**以保存所做的更改。

    (代码段- *BringingTogetherOneAspNet-Ex2-PersonClass*)

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample3.cs)]
5. 在**解决方案资源管理器**，右键单击**MyHybridSite**项目，然后选择**生成**，或按**CTRL + SHIFT + B**以生成项目。

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-an-mvc-controller"></a>任务 2 – 创建 MVC 控制器

现在，**人员**创建模型，将使用与实体框架的 ASP.NET MVC 基架创建的 CRUD 控制器操作和视图**人员**。

1. 在**解决方案资源管理器**，右键单击**控制器**文件夹**MyHybridSite**项目，然后选择**添加 |新的基架的项...**.

    ![创建新的基架的控制器](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image13.png)

    *创建新的基架控制器*
2. 在**添加基架**对话框中，选择**数据与视图，MVC 5 控制器使用 Entity Framework** ，然后单击**添加。**

    ![选择 MVC 5 控制器视图和实体框架](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image14.png)

    *选择 MVC 5 控制器视图和实体框架*
3. 设置*MvcPersonController*作为**控制器名称**，选择**使用异步控制器操作**选项并选择**人员 (MyHybridSite.Models)**作为**模型类**。

    ![添加基架 MVC 控制器](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image15.png)

    *添加基架 MVC 控制器*
4. 下**数据上下文类**，单击**新建数据上下文...**.

    ![创建新的数据上下文](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image16.png)

    *创建新的数据上下文*
5. 在**新建数据上下文**对话框中，名称的新的数据上下文*PersonContext*单击**添加**。

    ![创建新的 PersonContext](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image17.png)

    *创建新的 PersonContext 类型*
6. 单击**添加**若要创建的新控制器**人员**使用基架。 控制器操作、 Person 数据上下文和 Razor 视图，然后将生成 visual Studio。

    ![之后使用基架创建 MVC 控制器](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image18.png)

    *之后使用基架创建 MVC 控制器*
7. 打开**MvcPersonController.cs**文件中**控制器**文件夹。 请注意自动生成的 CRUD 操作方法。

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample4.cs)]

    > [!NOTE]
    > 通过选择**使用异步控制器操作**从基架的复选框选项前面步骤中，Visual Studio 将生成对涉及到人员数据上下文的访问的所有操作的异步操作方法。 建议的异步操作方法用于长时间运行、 非 CPU 绑定请求以避免阻止 Web 服务器在处理请求时执行工作。

<a id="Ex2Task3"></a>
#### <a name="task-3--running-the-solution"></a>任务 3 – 运行解决方案

在此任务中，你将运行以验证的解决方案的视图**人员**是否按预期工作。 你将添加新的用户来验证已成功保存到数据库。

1. 按**F5**运行该解决方案。
2. 导航到**/MvcPerson**。 应显示的基架的视图显示的人员列表。
3. 单击**新建**以添加新成员。

    ![导航到基架 MVC 视图](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image19.png)

    *导航到基架 MVC 视图*
4. 在**创建**视图中，提供**名称**和**年龄**人猜出，然后单击**创建**。

    ![添加新的用户](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image20.png)

    *添加新的用户*
5. 新的用户添加到列表。 在元素列表中，单击**详细信息**若要显示的人员的详细信息视图。 然后，在**详细信息**视图中，单击**回列表**回到列表视图。

    ![人员的详细信息视图](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image21.png)

    *人员的详细信息视图*
6. 单击**删除**链接以删除用户。 在**删除**视图中，单击**删除**以确认该操作。

    ![删除用户](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image22.png)

    *删除用户*
7. 返回到 Visual Studio，然后按**SHIFT + F5**来停止调试。

<a id="Exercise3"></a>
### <a name="exercise-3-creating-a-web-api-controller-using-scaffolding"></a>练习 3： 创建 Web API 控制器使用基架

Web API 框架是 ASP.NET 堆栈的一部分，旨在使实现 HTTP 服务更容易，但通常发送和接收 JSON 或 XML 格式的数据，通过 RESTful API。

在此练习中，您将再次使用 ASP.NET 基架以生成 Web API 控制器。 你将使用相同**人员**和**PersonContext**从上一练习，以提供相同的个人数据以 JSON 格式的类。 你将看到相同的 ASP.NET 应用程序中以不同方式，你就可以公开相同的资源。

<a id="Ex3Task1"></a>
#### <a name="task-1--creating-a-web-api-controller"></a>任务 1 – 创建一个 Web API 控制器

在此任务将创建一个新**Web API 控制器**，将公开机耗材格式，如 JSON 中的个人数据。

1. 如果尚未打开，则打开**Visual Studio Express 2013 for Web**并打开**MyHybridSite.sln**解决方案位于**源/Ex3-WebAPI/开始**文件夹。 或者，继续与解决方案并在上一练习中获取。

    > [!NOTE]
    > 如果你从开始从练习 3 开始解决方案，按**CTRL + SHIFT + B**要生成解决方案。
2. 在**解决方案资源管理器**，右键单击**控制器**文件夹**MyHybridSite**项目，然后选择**添加 |新的基架的项...**.

    ![创建新的基架的控制器](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image23.png)

    *创建新的基架的控制器*
3. 在**添加基架**对话框中，选择**Web API**的左窗格中，然后**Web API 2 Controller 其操作使用 Entity Framework**在中间窗格中，然后单击**添加。**

    ![选择包含操作和实体框架的 Web API 2 Controller](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "选择包含操作和实体框架 Web API 2 控制器")

    *选择包含操作和实体框架的 Web API 2 控制器*
4. 设置*ApiPersonController*作为**控制器名称**，选择**使用异步控制器操作**选项并选择**人员 (MyHybridSite.Models)**和**PersonContext (MyHybridSite.Models)**作为**模型**和**数据上下文**分别类。 然后单击 **“添加”**。

    ![添加基架使用一个 Web API 控制器](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "添加基架使用一个 Web API 控制器")

    *添加基架使用一个 Web API 控制器*
5. 然后，visual Studio 将生成**ApiPersonController**其四个的 CRUD 操作，可用于处理数据的类。

    ![之后使用基架创建的 Web API 控制器](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "后使用基架创建的 Web API 控制器")

    *之后使用基架创建的 Web API 控制器*
6. 打开**ApiPersonController.cs**文件并检查*GetPeople*操作方法。 此方法的查询的数据库字段**PersonContext**以获取数据的人员的类型。

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample5.cs)]
7. 请注意，上面的方法定义的注释。 它提供了公开此操作将在下一个任务中使用的 URI。

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample6.cs)]

    > [!NOTE]
    > 默认情况下，Web API 配置为捕获到的查询*/api*路径，以避免与 MVC 控制器的冲突。 如果你需要更改此设置，请参阅[路由在 ASP.NET Web API 中](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md)。

<a id="Ex3Task2"></a>
#### <a name="task-2--running-the-solution"></a>任务 2 – 运行解决方案

在此任务将使用 Internet Explorer **F12 开发人员工具**检查来自 Web API 控制器的完整响应。 你将看到如何可以捕获网络流量，以详细了解你的应用程序数据。

> [!NOTE]
> 请确保**Internet Explorer**中选择**启动**按钮位于 Visual Studio 工具栏上。
> 
> ![Internet Explorer 选项](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image27.png)
> 
> **F12 开发人员工具**具有一组范围广泛的此动手实验中未涉及的功能。 如果你想要了解详细信息，请参阅[使用 F12 开发人员工具](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85))。


1. 按**F5**运行该解决方案。

    > [!NOTE]
    > 为了正确执行此任务，你的应用程序需要具有的数据。 如果你的数据库为空，你可以返回在练习 2 中的任务 3，并按照如何创建新的用户使用 MVC 视图上的步骤。
2. 在浏览器中，按**F12**以打开**开发人员工具**面板。 按**CTRL** + **4**或单击**网络**图标，，然后单击绿色箭头按钮，以开始捕获网络流量。

    ![启动 Web API 网络捕获](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "启动 Web API 网络捕获")

    *启动 Web API 网络捕获*
3. 追加**api/ApiPerson**到浏览器的地址栏中的 URL。 现在将检查从响应的详细信息**ApiPersonController**。

    ![检索通过 Web API 的人员数据](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "检索通过 Web API 的个人数据")

    *通过 Web API 中检索用户数据*

    > [!NOTE]
    > 下载完成后，系统将提示与下载的文件进行操作。 使对话框保持打开状态，以能够查看响应内容通过开发人员工具窗口。
4. 现在，你将检查响应的正文。 若要执行此操作，请单击**详细信息**选项卡，然后单击**响应正文**。 你可以检查下载的数据是带有属性的对象列表**Id**，**名称**和**年龄**对应于这样的**人员**类。

    ![查看 Web API 响应正文](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "查看 Web API 响应正文")

    *查看 Web API 响应正文*

<a id="Ex3Task3"></a>
#### <a name="task-3--adding-web-api-help-pages"></a>任务 3 – 添加 Web API 帮助页

当你创建一个 Web API 时，它可用于创建一个帮助页，以便其他开发人员知道如何调用你的 API。 你可以创建并文档页手动更新，但最好将自动生成它们以避免维护工作。 在此任务将使用 Nuget 包以自动生成到解决方案的 Web API 帮助页。

1. 从**工具**菜单在 Visual Studio 中，选择**库程序包管理器**，然后单击**程序包管理器控制台**。
2. 在**程序包管理器控制台**窗口中，执行以下命令：

    [!code-powershell[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample7.ps1)]

    > [!NOTE]
    > **Microsoft.AspNet.WebApi.HelpPage**程序包安装必需的程序集并添加下的帮助页的 MVC 视图**区域/HelpPage**文件夹。

    ![HelpPage 区域](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "HelpPage 区域")

    *HelpPage 区域*
3. 默认情况下，帮助页具有文档的占位符字符串。 XML 文档注释可用于创建文档。 若要启用此功能，请打开**HelpPageConfig.cs**文件位于**应用程序区域/HelpPage\_启动**文件夹并取消注释以下行：

    [!code-javascript[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample8.js)]
4. 在**解决方案资源管理器**，右键单击该项目**MyHybridSite**，选择**属性**单击**生成**选项卡。

    ![生成选项卡](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "生成部分")

    *生成选项卡*
5. 下**输出**，选择**XML 文档文件**。 在编辑框中，键入**应用\_Data/XmlDocument.xml**。

    ![输出生成选项卡中的部分](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "输出生成选项卡中的部分")

    *生成选项卡中的输出部分*
6. 按**CTRL** + **S**以保存所做的更改。
7. 打开**ApiPersonController.cs**文件从**控制器**文件夹。
8. 输入新行之间*GetPeople*方法签名和*/ / 获取 api/ApiPerson*注释，，然后键入三个正斜杠。

    > [!NOTE]
    > Visual Studio 会自动插入的 XML 元素定义的方法文档。
9. 添加摘要文本和的返回值*GetPeople*方法。 其外观应如下所示。

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample9.cs)]
10. 按**F5**运行该解决方案。
11. 追加**/帮助**到在地址栏中的该 URL 可浏览到帮助页。

    ![ASP.NET Web API 帮助页](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "ASP.NET Web API 帮助页")

    *ASP.NET Web API 帮助页*

    > [!NOTE]
    > 页面的主要内容是 Api，由控制器分组的表。 表条目动态生成的使用**IApiExplorer**接口。 如果你添加或更新一个 API 控制器，表将自动更新生成应用程序的下一步时间。
    > 
    > **API**列列出的 HTTP 方法和相对 URI。 **说明**列包含具有已从该方法的文档中提取的信息。
12. 请注意，在说明列显示您在前面的方法定义添加的说明。

    ![API 方法描述](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "API 方法描述")

    *API 方法描述*
13. 单击其中一个 API 方法，以导航到具有更多详细信息，包括示例响应正文的页。

    ![详细信息页](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "详细信息页")

    *详细的信息页*

* * *

<a id="Summary"></a>
## <a name="summary"></a>摘要

通过完成本动手实验你已学习如何：

- 创建新 Web 应用程序在 Visual Studio 2013 中使用一个 ASP.NET 体验
- 将多个 ASP.NET 技术集成到一个单个的项目
- 从你使用 ASP.NET 基架的模型类生成 MVC 控制器和视图
- 生成 Web API 控制器，使用异步编程等通过 Entity Framework 数据访问的功能
- 为你的控制器自动生成 Web API 帮助页
