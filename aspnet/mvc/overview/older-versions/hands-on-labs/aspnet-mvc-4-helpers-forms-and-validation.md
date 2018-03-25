---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
title: "ASP.NET MVC 4 帮助器、 窗体和验证 |Microsoft 文档"
author: rick-anderson
description: "在 ASP.NET MVC 4 模型和数据访问动手实验中，你已加载并显示从数据库的数据。 在本动手实验中，你将添加到..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 187ee9cd-bc70-479b-bfed-f568b8da96eb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
msc.type: authoredcontent
ms.openlocfilehash: 243db3708ac4311d423c4c137f503f072f5553e6
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="aspnet-mvc-4-helpers-forms-and-validation"></a>ASP.NET MVC 4 帮助器、 窗体和验证

通过[Web 营地团队](https://twitter.com/webcamps)

[下载 Web 营地培训工具包](https://aka.ms/webcamps-training-kit)

在**ASP.NET MVC 4 模型和数据访问**动手实验中，你已被加载和显示来自数据库的数据。 在本动手实验中，你将添加到**音乐商店**应用程序编辑该数据的能力。

与该目标，首先将创建将支持唱片集的创建、 读取、 更新和删除 (CRUD) 操作的控制器。 将生成利用 ASP.NET MVC 基架功能在 HTML 表中显示唱片集的属性的索引视图模板。 若要增强该视图，你将添加将截断长说明的自定义 HTML 帮助。

然后，你将添加编辑和创建的视图，以便可以改变专辑在数据库中，如下拉列表的窗体元素的帮助。

最后，您就可以让用户删除唱片集，也将从通过验证其输入来输入数据错误阻止它们。

此动手实验假定你具有的基础知识**ASP.NET MVC**。 如果您未使用过**ASP.NET MVC**之前，我们建议你转到**ASP.NET MVC 基础知识**动手实验。

此实验室将引导你完成的增强功能和前面所述通过将细微的更改应用于源文件夹中提供的示例 Web 应用程序的新功能。

> [!NOTE]
> 在 Web 营地培训工具包中，在包括所有的示例代码和代码段[Microsoft 的 Web/WebCampTrainingKit 版本](https://aka.ms/webcamps-training-kit)。 特定于此实验室项目位于[ASP.NET MVC 4 帮助器、 窗体和验证](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation)。

<a id="Objectives"></a>
### <a name="objectives"></a>目标

在本动手实验中，你将了解如何：

- 创建控制器以支持 CRUD 操作
- 生成索引视图以显示 HTML 表中的实体属性
- 添加自定义 HTML 帮助器
- 创建和自定义编辑视图
- 区分响应 HTTP GET 或 HTTP POST 调用的操作方法
- 添加和自定义创建视图
- 处理删除的实体
- 验证用户输入

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>系统必备

你必须具有要完成本实验的以下项：

- [Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)或更高 (读取[附录 A](#AppendixA)有关如何安装它的说明)。

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>安装

**安装代码片段**

为方便起见，你将沿此实验室管理大部分都是代码的可用作 Visual Studio 代码段。 若要安装运行的代码段**.\Source\Setup\CodeSnippets.vsi**文件。

如果你不熟悉 Visual Studio 代码段，并想要了解如何使用它们，你可以从该文档引用的附录&quot;[附录 b： 使用代码段](#AppendixB)&quot;。

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>练习

在以下练习构成此动手实验：

1. [创建存储管理器控制器和其索引视图](#Exercise1)
2. [添加 HTML 帮助器](#Exercise2)
3. [创建编辑视图](#Exercise3)
4. [添加 Create 视图](#Exercise4)
5. [处理删除](#Exercise5)
6. [添加验证](#Exercise6)
7. [在客户端使用非介入式 jQuery](#Exercise7)

> [!NOTE]
> 每个练习均附带由**结束**包含生成您应该完成练习后获得的解决方案文件夹。 如果你需要通过在练习工作的更多帮助，可以使用此解决方案作为指南。


估计时间来完成该实验： **60 分钟**

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_the_Store_Manager_controller_and_its_Index_view"></a>
### <a name="exercise-1-creating-the-store-manager-controller-and-its-index-view"></a>练习 1： 创建存储管理器控制器和其索引视图

在本练习中，您将学习如何创建新的控制器以支持 CRUD 操作，自定义其索引操作方法，以从数据库和最后生成利用 ASP.NET MVC 基架的索引视图模板返回的唱片集列表若要在 HTML 表中显示唱片集的属性的功能。

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_StoreManagerController"></a>
#### <a name="task-1---creating-the-storemanagercontroller"></a>任务 1-创建 StoreManagerController

在此任务中，你将创建新的控制器调用**StoreManagerController**以支持 CRUD 操作。

1. 打开**开始**解决方案位于**源/Ex1-CreatingTheStoreManagerController/开始/**文件夹。

    1. 你将需要下载一些缺少的 NuGet 程序包才能继续。 若要执行此操作，请单击**项目**菜单，然后选择**管理 NuGet 包**。
    2. 在**管理 NuGet 包**对话框中，单击**还原**以便下载缺少的程序包。
    3. 最后，通过单击生成解决方案**生成** | **生成解决方案**。

    > [!NOTE]
    > 使用 NuGet 的优点之一是，你无需提供你的项目中的所有库减小项目大小。 使用 NuGet 增强工具，请通过指定的包版本在 Packages.config 文件中，你将能够下载首次运行该项目的所有所需的库。 这是你将需要从本实验打开现有的解决方案后运行这些步骤的原因。
2. 添加一个新的控制器。 要执行此操作，请右键单击**控制器**在解决方案资源管理器，选择文件夹**添加**然后**控制器**命令。 更改**控制器****名称**到**StoreManagerController**并确保选项**包含空读/写操作的MVC控制器**选择。 单击 **“添加”**。

    ![添加控制器对话框](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "添加控制器对话框")

    *添加控制器对话框*

    生成新的控制器类。 因为你要添加的读/写，对于那些，存根 （stub） 方法的操作来创建 TODO 注释填写，提示将应用程序特定逻辑包括常见 CRUD 操作。

<a id="Ex1Task2"></a>

<a id="Task_2_-_Customizing_the_StoreManager_Index"></a>
#### <a name="task-2---customizing-the-storemanager-index"></a>任务 2-自定义 StoreManager 索引

在此任务中，将自定义要从数据库返回具有唱片集的列表的视图的 StoreManager 索引操作方法。

1. 在 StoreManagerController 类中，添加以下*使用*指令。

    (代码段- *ASP.NET MVC 4 帮助器、 窗体和验证-Ex1 使用 MvcMusicStore*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample1.cs)]
2. 将字段添加到**StoreManagerController**来托管实例的**MusicStoreEntities。**

    (代码段- *ASP.NET MVC 4 帮助器、 窗体和验证-Ex1 MusicStoreEntities*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample2.cs)]
3. 实现 StoreManagerController 索引操作返回与唱片集的列表视图。

    控制器操作逻辑将非常类似于前面编写 StoreController 的索引操作。 使用 LINQ 检索所有专辑，包括的显示风格和艺术家信息。

    (代码段- *ASP.NET MVC 4 帮助器、 窗体和验证-Ex1 StoreManagerController 索引*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample3.cs)]

<a id="Ex1Task3"></a>

<a id="strongTask_3_-_Creating_the_Index_Viewstrong"></a>
#### <a name="task-3---creating-the-index-view"></a>任务 3-创建索引视图

在此任务中，你将创建索引视图模板，以显示列表中返回的专辑**StoreManager**控制器。

1. 在创建新的视图模板之前, 应生成项目，以便**添加视图对话框**就会了解有关**唱片集**要使用的类。 选择**生成 |生成 MvcMusicStore**以生成项目。
2. 右键单击内部**索引**操作方法，然后选择**添加视图**。 此时会弹出**添加视图**对话框。

    ![添加视图](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "添加视图")

    *添加从索引方法内的某个视图*
3. 在添加视图对话框中，验证是否视图名称**索引**。 选择**创建强类型化视图**选项，然后选择**唱片集 (MvcMusicStore.Models)**从**模型类**下拉列表。 选择**列表**从**基架模板**下拉列表。 保留**视图引擎**到**Razor**和其他字段使用其默认值，然后单击**添加**。

    ![添加索引视图](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "添加索引视图")

    *添加索引视图*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Customizing_the_scaffold_of_the_Index_View"></a>
#### <a name="task-4---customizing-the-scaffold-of-the-index-view"></a>任务 4-自定义索引视图的基架

在此任务中，您将调整与 ASP.NET MVC 基架功能，以使它显示所需的字段创建的简单视图模板。

> [!NOTE]
> **基架**支持 ASP.NET MVC 中的生成一个简单的视图模板，其中列出了唱片集模型中的所有字段。 **基架**快速地开始使用强类型化视图： 而不是无需手动编写查看模板，基架快速生成的默认模板，然后可以修改生成的代码。


1. 查看创建的代码。 生成的字段列表将作为一部分的以下 HTML 表**基架**用于显示表格格式数据。

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample4.cshtml)]
2. 替换**&lt;表&gt;**代码替换为以下代码，以仅显示**流派**，**艺术家**，**唱片集标题**，和**价格**字段。 这将删除**AlbumId**和**唱片集艺术作品 URL**列。 此外，它将更改 GenreId 和 ArtistId 列以显示其链接的类属性**Artist.Name**和**Genre.Name**，并删除**详细信息**链接。

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample5.cshtml)]
3. 更改以下说明。

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample6.cshtml)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>任务 5-运行应用程序

在此任务中，你将测试**StoreManager** **索引**视图模板显示根据前面的步骤的设计的唱片集的列表。

1. 按**F5**运行该应用程序。
2. 在主页页面中启动项目。 将 URL 更改为**/StoreManager**验证，显示的唱片集的列表，显示其**标题**，**艺术家**和**流派**。

    ![浏览的专辑列表](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "浏览唱片集的列表")

    *浏览唱片集的列表*

<a id="Exercise2"></a>

<a id="Exercise_2_Adding_an_HTML_Helper"></a>
### <a name="exercise-2-adding-an-html-helper"></a>练习 2： 添加 HTML 帮助器

StoreManager 索引页都有一个潜在问题： 标题和艺术家名称属性都可以是足够长，以引发关闭表格的格式。 在本练习中你将了解如何添加自定义的 HTML 帮助器截断该文本。

在下图中，你可以看到如何格式时所使用的小型浏览器大小修改由于文本的长度。

![浏览与唱片集的列表不会被截断文本](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "浏览与唱片集的列表不截断的文本")

*浏览与唱片集的列表不截断的文本*

<a id="Ex2Task1"></a>

<a id="Task_1_-_Extending_the_HTML_Helper"></a>
#### <a name="task-1---extending-the-html-helper"></a>任务 1-扩展 HTML 帮助器

在此任务中，你将添加一个新方法**Truncate**到**HTML**公开在 ASP.NET MVC 视图内的对象。 若要执行此操作，则将实现**扩展方法**到内置**System.Web.Mvc.HtmlHelper**类提供的 ASP.NET MVC。

> [!NOTE]
> 若要阅读更多有关**扩展方法**，请访问此 msdn 文章。 [https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).


1. 打开**开始**解决方案位于**源/Ex2-AddingAnHTMLHelper/开始/**文件夹。 否则，可能会继续使用**结束**解决方案获取通过完成上一练习。

    1. 如果你打开提供**开始**解决方案，你将需要下载一些缺少的 NuGet 程序包才能继续。 若要执行此操作，请单击**项目**菜单，然后选择**管理 NuGet 包**。
    2. 在**管理 NuGet 包**对话框中，单击**还原**以便下载缺少的程序包。
    3. 最后，通过单击生成解决方案**生成** | **生成解决方案**。

    > [!NOTE]
    > 使用 NuGet 的优点之一是，你无需提供你的项目中的所有库减小项目大小。 使用 NuGet 增强工具，请通过指定的包版本在 Packages.config 文件中，你将能够下载首次运行该项目的所有所需的库。 这是你将需要从本实验打开现有的解决方案后运行这些步骤的原因。
2. 打开 StoreManager 的索引视图。 若要执行此操作，在解决方案资源管理器中展开**视图**文件夹，则**StoreManager**并打开**Index.cshtml**文件。
3. 添加下面的代码下面 **@model** 指令来定义**Truncate**帮助器方法。

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample7.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Truncating_Text_in_the_Page"></a>
#### <a name="task-2---truncating-text-in-the-page"></a>任务 2-在页中的截断文本

在此任务中，你将使用**Truncate**方法进行截断操作中查看模板的文本。

1. 打开 StoreManager 的索引视图。 若要执行此操作，在解决方案资源管理器中展开**视图**文件夹，则**StoreManager**并打开**Index.cshtml**文件。
2. 显示的行替换**艺术家名称**和唱片集的**标题**。 若要执行此操作，请将以下行。

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample8.cshtml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>任务 3-运行应用程序

在此任务中，你将测试**StoreManager** **索引**视图模板将截断唱片集的标题和艺术家名称。

1. 按**F5**运行该应用程序。
2. 在主页页面中启动项目。 将 URL 更改为**/StoreManager**以验证该 long 类型的值中的文本**标题**和**艺术家**列将被截断。

    ![截断标题和艺术家名称](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "截断标题和艺术家名称")

    *截断的标题和艺术家姓名*

<a id="Exercise3"></a>

<a id="Exercise_3_Creating_the_Edit_View"></a>
### <a name="exercise-3-creating-the-edit-view"></a>练习 3： 创建编辑视图

在本练习中，您将学习如何创建窗体，以允许存储管理器编辑唱片集。 用户将浏览**/StoreManager/Edit/id** URL (**id**正在编辑的唱片集的唯一 id)，从而使到服务器的 HTTP GET 调用。

控制器编辑操作方法将从数据库中检索相应唱片集，创建**StoreManagerViewModel**对象，用于封装 （以及专业人员和风格的列表），并将其传递给的视图模板向用户呈现的 HTML 页面。 此页将包含**&lt;窗体&gt;**使用文本框和下拉列表编辑唱片集属性的元素。

用户更新唱片集窗体值并单击后**保存**按钮，所做的更改会提交通过 HTTP 发送回叫**/StoreManager/Edit/id**。虽然 URL 仍与最后一次调用中的相同，ASP.NET MVC 标识，此时它是 HTTP POST，并因此执行不同的编辑操作方法 (一个使用修饰**[HttpPost]**)。

<a id="Ex3Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Edit_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-edit-action-method"></a>任务 1-实现的 HTTP GET 编辑操作方法

在此任务中，将实现来检索从数据库中的相应唱片集的编辑操作方法的 HTTP GET 版本以及所有风格和专业人员的列表。 它将此数据打包成**StoreManagerViewModel**最后一个步骤，然后将传递给要呈现具有的响应的视图模板中定义的对象。

1. 打开**开始**解决方案位于**源/Ex3-CreatingTheEditView/开始/**文件夹。 否则，可能会继续使用**结束**解决方案获取通过完成上一练习。

    1. 如果你打开提供**开始**解决方案，你将需要下载一些缺少的 NuGet 程序包才能继续。 若要执行此操作，请单击**项目**菜单，然后选择**管理 NuGet 包**。
    2. 在**管理 NuGet 包**对话框中，单击**还原**以便下载缺少的程序包。
    3. 最后，通过单击生成解决方案**生成** | **生成解决方案**。

    > [!NOTE]
    > 使用 NuGet 的优点之一是，你无需提供你的项目中的所有库减小项目大小。 使用 NuGet 增强工具，请通过指定的包版本在 Packages.config 文件中，你将能够下载首次运行该项目的所有所需的库。 这是你将需要从本实验打开现有的解决方案后运行这些步骤的原因。
2. 打开**StoreManagerController**类。 若要执行此操作，展开**控制器**文件夹并双击**StoreManagerController.cs**。
3. 替换**HTTP GET 编辑**替换为以下代码，以检索相应的操作方法**唱片集**以及**风格**和**艺术家**列出。

    (代码段- *ASP.NET MVC 4 帮助器、 窗体和验证-Ex3 StoreManagerController HTTP GET 编辑操作*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample9.cs)]

    > [!NOTE]
    > 你使用**System.Web.Mvc** **此时**专业人员和风格而不是为**System.Collections.Generic**列表。
    > 
    > **此时**是填充 HTML 下拉列表和管理等当前所选内容的更简洁方法。 实例化和更高版本设置中的控制器操作这些 ViewModel 对象将使编辑窗体方案清理器。

<a id="Ex3Task2"></a>

<a id="Task_2_-_Creating_the_Edit_View"></a>
#### <a name="task-2---creating-the-edit-view"></a>任务 2-创建编辑视图

在此任务中，你将创建更高版本显示唱片集属性的编辑视图模板。

1. 创建编辑视图。 若要执行此操作，右键单击内**编辑**操作方法，然后选择**添加视图**。
2. 在添加视图对话框中，验证是否视图名称**编辑**。 检查**创建强类型化视图**复选框，然后选择**唱片集 (MvcMusicStore.Models)**从**查看数据类**下拉列表。 选择**编辑**从**基架模板**下拉列表。 将其他字段保留其默认值，然后单击**添加**。

    ![添加的编辑视图](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "添加的编辑视图")

    *添加的编辑视图*

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>任务 3-运行应用程序

在此任务中，你将测试**StoreManager** **编辑**视图页中显示的唱片集作为参数传递的属性的值。

1. 按**F5**运行该应用程序。
2. 在主页页面中启动项目。 将 URL 更改为**/StoreManager/Edit/1**以验证显示是否传递唱片集的属性的值。

    ![浏览唱片集的编辑视图](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "浏览唱片集的编辑视图")

    *浏览唱片集的编辑视图*

<a id="Ex3Task4"></a>

<a id="Task_4_-_Implementing_drop-downs_on_the_Album_Editor_Template"></a>
#### <a name="task-4---implementing-drop-downs-on-the-album-editor-template"></a>任务 4-在唱片集编辑器模板上实现下拉列表

在此任务中，你将添加下拉列表中的最后一个任务，创建的视图模板，以便用户可以选择从专业人员和风格的列表。

1. 全部替换**唱片集**fieldset 代码替换为以下：

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample10.cshtml)]

    > [!NOTE]
    > **Html.DropDownList**添加了帮助器以呈现下拉列表选择专业人员和风格。 参数传递给**Html.DropDownList**是：
    > 
    > 1. 窗体字段的名称 (**&quot;ArtistId&quot;**)。
    > 2. **此时**的下拉列表的值。

<a id="Ex3Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>任务 5-运行应用程序

在此任务中，你将测试**StoreManager** **编辑**视图页中显示而不是艺术家和风格 ID 文本字段的下拉列表。

1. 按**F5**运行该应用程序。
2. 在主页页面中启动项目。 将 URL 更改为**/StoreManager/Edit/1**以验证它显示而不是艺术家和风格 ID 文本字段的下拉列表。

    ![浏览唱片集的下拉列表编辑视图](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "浏览唱片集的下拉列表编辑视图")

    *浏览唱片集的编辑视图，这次它带有下拉列表*

<a id="Ex3Task6"></a>

<a id="Task_6_-_Implementing_the_HTTP-POST_Edit_action_method"></a>
#### <a name="task-6---implementing-the-http-post-edit-action-method"></a>任务 6-实现的 HTTP POST 编辑操作方法

现在，编辑视图显示按预期方式，你需要实现 HTTP POST 编辑操作方法，可保存到唱片集所做的更改。

1. 关闭浏览器，如果需要可以返回到 Visual Studio 窗口。 打开**StoreManagerController**从**控制器**文件夹。
2. 替换**HTTP POST 编辑**操作方法代码替换为以下 （请注意，必须将其替换的方法接收两个参数的重载的版本）：

    (代码段- *ASP.NET MVC 4 帮助器、 窗体和验证-Ex3 StoreManagerController HTTP POST 编辑操作*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample11.cs)]

    > [!NOTE]
    > 将执行此方法，当用户单击**保存**视图的按钮，并执行 HTTP POST 的回发到服务器的窗体值将其保留在数据库中。 修饰**[HttpPost]**指示该方法应用于这些 HTTP POST 方案。 该方法采用**唱片集**对象。 ASP.NET MVC 将自动创建唱片集对象从已发布&lt;窗体&gt;值。
    > 
    > 该方法将执行以下步骤：
    > 
    > 1. 如果模型是有效的：
    > 
    >     1. 更新要将其标记为已修改对象的上下文中的唱片集条目。
    >     2. 保存所做的更改并将重定向到索引视图。
    > 2. 如果模型不是有效的它会使用填充与 ViewBag **GenreId**和**ArtistId**，则会返回该视图并且接收到的唱片集对象，以允许用户执行任何所需的更新。

<a id="Ex3Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a>任务 7-运行应用程序

在此任务中，你将测试**StoreManager 编辑**视图页中实际将更新后的唱片集数据保存在数据库中。

1. 按**F5**运行该应用程序。
2. 在主页页面中启动项目。 将 URL 更改为**/StoreManager/Edit/1**。 将唱片集标题更改为**负载**，然后单击**保存**。 验证唱片集的列表中实际更改唱片集的标题。

    ![更新唱片集](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "更新唱片集")

    *更新唱片集*

<a id="Exercise4"></a>

<a id="Exercise_4_Adding_a_Create_View"></a>
### <a name="exercise-4-adding-a-create-view"></a>练习 4： 添加 Create 视图

现在， **StoreManagerController**支持**编辑**功能，在此练习中你将了解如何添加 Create View 模板以便存储管理器将新唱片集添加到应用程序。

如你具有编辑功能，您将实现使用两个不同方法中的创建方案**StoreManagerController**类：

1. 在首次访问应用商店管理器时，一种操作方法将显示空窗体**/StoreManager/创建**URL。
2. 第二个操作方法将处理该方案的存储管理器单击其中**保存**在窗体按钮和提交值回**/StoreManager/创建**URL 为 HTTP POST。

<a id="Ex4Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Create_action_method"></a>
#### <a name="task-1---implementing-the-http-get-create-action-method"></a>任务 1-实现的 HTTP GET 创建操作方法

在此任务中，你将实现若要检索的所有风格和艺术家列表，请打包到此数据的创建操作方法的 HTTP GET 版本**StoreManagerViewModel**对象，然后将传递给视图模板。

1. 打开**开始**解决方案位于**源/Ex4-AddingACreateView/开始/**文件夹。 否则，可能会继续使用**结束**解决方案获取通过完成上一练习。

    1. 如果你打开提供**开始**解决方案，你将需要下载一些缺少的 NuGet 程序包才能继续。 若要执行此操作，请单击**项目**菜单，然后选择**管理 NuGet 包**。
    2. 在**管理 NuGet 包**对话框中，单击**还原**以便下载缺少的程序包。
    3. 最后，通过单击生成解决方案**生成** | **生成解决方案**。

    > [!NOTE]
    > 使用 NuGet 的优点之一是，你无需提供你的项目中的所有库减小项目大小。 使用 NuGet 增强工具，请通过指定的包版本在 Packages.config 文件中，你将能够下载首次运行该项目的所有所需的库。 这是你将需要从本实验打开现有的解决方案后运行这些步骤的原因。
2. 打开**StoreManagerController**类。 若要执行此操作，展开**控制器**文件夹并双击**StoreManagerController.cs**。
3. 替换**创建**操作方法代码替换为以下：

    (代码段- *ASP.NET MVC 4 帮助器、 窗体和验证-Ex4 StoreManagerController HTTP GET 创建操作*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample12.cs)]

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_the_Create_View"></a>
#### <a name="task-2---adding-the-create-view"></a>任务 2-添加 Create 视图

在此任务中，你将添加 Create View 模板将显示新的 （空） 唱片集窗体。

1. 右键单击内部**创建**操作方法，然后选择**添加视图**。 这将显示添加视图对话框。
2. 在添加视图对话框中，验证是否视图名称**创建**。 选择**创建强类型化视图**选项并选择**唱片集 (MvcMusicStore.Models)**从**模型类**下拉列表和**创建**从**基架模板**下拉列表。 将其他字段保留其默认值，然后单击**添加**。

    ![添加 create 视图](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "添加-a-创建-view.png")

    *添加 Create 视图*
3. 更新**GenreId**和**ArtistId**字段使用下拉列表，如下所示：

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample13.cshtml)]

<a id="Ex4Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>任务 3-运行应用程序

在此任务中，你将测试**StoreManager** **创建**视图页中显示一个空的唱片集窗体。

1. 按**F5**运行该应用程序。
2. 在主页页面中启动项目。 将 URL 更改为**/StoreManager/创建**。 验证空窗体显示用于填充新唱片集属性。

    ![使用空的窗体中创建视图](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "创建空的窗体视图")

    *使用空的窗体中创建视图*

<a id="Ex4Task4"></a>

<a id="Task_4_-_Implementing_the_HTTP-POST_Create_Action_Method"></a>
#### <a name="task-4---implementing-the-http-post-create-action-method"></a>任务 4-实现 HTTP POST 创建操作方法

在此任务中，你将实现将在用户单击时调用的创建操作方法的 HTTP POST 版本**保存**按钮。 该方法应将新唱片集保存在数据库中。

1. 关闭浏览器，如果需要可以返回到 Visual Studio 窗口。 打开**StoreManagerController**类。 若要执行此操作，展开**控制器**文件夹并双击**StoreManagerController.cs**。
2. 替换**HTTP POST 创建**操作方法代码替换为以下：

    (代码段- *ASP.NET MVC 4 帮助器、 窗体和验证-Ex4 StoreManagerController HTTP POST 创建操作*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample14.cs)]

    > [!NOTE]
    > 创建操作非常类似于上一个编辑操作方法，但而不是将对象设置为已修改，它将被添加到上下文。

<a id="Ex4Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>任务 5-运行应用程序

在此任务中，你将测试**StoreManager 创建**视图页中可以创建新唱片集，然后将重定向到 StoreManager 索引视图。

1. 按**F5**运行该应用程序。
2. 在主页页面中启动项目。 将 URL 更改为**/StoreManager/创建**。 所有窗体字段用数据填充新唱片集，如下图中所示：

    ![创建唱片集](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "创建唱片集")

    *创建唱片集*
3. 验证你获取重定向到包含刚创建的新唱片集 StoreManager 索引视图。

    ![创建新唱片集](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "创建的新唱片集")

    *创建新唱片集*

<a id="Exercise5"></a>

<a id="Exercise_5_Handling_Deletion"></a>
### <a name="exercise-5-handling-deletion"></a>练习 5： 处理删除

若要删除唱片集的功能尚未实现。 这是本练习将哪些有关。 像之前，将实现使用两个不同方法中的删除方案**StoreManagerController**类：

1. 一种操作方法将显示一个确认窗体
2. 第二个操作方法将处理提交窗体

<a id="Ex5Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Delete_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-delete-action-method"></a>任务 1-实现 HTTP GET Delete 操作方法

在此任务中，你将实现来检索唱片集的信息的删除操作方法的 HTTP GET 版本。

1. 打开**开始**解决方案位于**源/Ex5-HandlingDeletion/开始/**文件夹。 否则，可能会继续使用**结束**解决方案获取通过完成上一练习。

    1. 如果你打开提供**开始**解决方案，你将需要下载一些缺少的 NuGet 程序包才能继续。 若要执行此操作，请单击**项目**菜单，然后选择**管理 NuGet 包**。
    2. 在**管理 NuGet 包**对话框中，单击**还原**以便下载缺少的程序包。
    3. 最后，通过单击生成解决方案**生成** | **生成解决方案**。

    > [!NOTE]
    > 使用 NuGet 的优点之一是，你无需提供你的项目中的所有库减小项目大小。 使用 NuGet 增强工具，请通过指定的包版本在 Packages.config 文件中，你将能够下载首次运行该项目的所有所需的库。 这是你将需要从本实验打开现有的解决方案后运行这些步骤的原因。
2. 打开**StoreManagerController**类。 若要执行此操作，展开**控制器**文件夹并双击**StoreManagerController.cs**。
3. 删除控制器操作正是之前存储详细信息控制器操作相同： 它会查询**唱片集**从数据库使用的对象**id** URL 和返回中提供适当**视图**。 若要执行此操作，将 HTTP GET**删除**操作方法代码替换为以下：

    (代码段- *ASP.NET MVC 4 帮助器、 窗体和验证-Ex5 处理删除 HTTP GET Delete 操作*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample15.cs)]
4. 右键单击内部**删除**操作方法，然后选择**添加视图**。 这将显示添加视图对话框。
5. 在添加视图对话框中，验证是否视图名称**删除**。 选择**创建强类型化视图**选项并选择**唱片集 (MvcMusicStore.Models)**从**模型类**下拉列表。 选择**删除**从**基架模板**下拉列表。 将其他字段保留其默认值，然后单击**添加**。

    ![添加删除视图](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "添加删除视图")

    *添加删除视图*
6. 删除模板显示模型中的所有字段。 将显示仅唱片集的标题。 若要执行此操作，请将视图的内容替换为以下代码：

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample16.cshtml)]

<a id="Ex05Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>任务 2-运行应用程序

在此任务中，你将测试**StoreManager** **删除**视图页中显示一个确认删除窗体。

1. 按**F5**运行该应用程序。
2. 在主页页面中启动项目。 将 URL 更改为**/StoreManager**。 选择要删除通过单击一个相册**删除**并验证是否已上载新视图。

    ![删除唱片集](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "删除唱片集")

    *删除唱片集*

<a id="Ex05Task3"></a>

<a id="Task_3-_Implementing_the_HTTP-POST_Delete_Action_Method"></a>
#### <a name="task-3--implementing-the-http-post-delete-action-method"></a>任务 3-实现 HTTP POST Delete 操作方法

在此任务中，你将实现将在用户单击时调用的删除操作方法的 HTTP POST 版本**删除**按钮。 该方法应删除数据库中的唱片集。

1. 关闭浏览器，如果需要可以返回到 Visual Studio 窗口。 打开**StoreManagerController**类。 若要执行此操作，展开**控制器**文件夹并双击**StoreManagerController.cs**。
2. 替换**HTTP POST 删除**操作方法代码替换为以下：

    (代码段- *ASP.NET MVC 4 帮助器、 窗体和验证-Ex5 处理删除 HTTP POST 删除操作*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample17.cs)]

<a id="Ex5Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>任务 4-运行应用程序

在此任务中，你将测试**StoreManager 删除**视图页可让您删除唱片集，然后重定向到 StoreManager 索引视图。

1. 按**F5**运行该应用程序。
2. 在主页页面中启动项目。 将 URL 更改为**/StoreManager**。 选择要删除通过单击一个相册**删除。** 通过单击确认删除**删除**按钮：

    ![删除唱片集](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "删除唱片集")

    *删除唱片集*
3. 验证唱片集中已删除，因为它未出现在**索引**页。

<a id="Exercise6"></a>

<a id="Exercise_6_Adding_Validation"></a>
### <a name="exercise-6-adding-validation"></a>练习 6： 添加验证

目前，创建和编辑窗体将不执行任何类型的验证。 如果用户离开在 price 字段中的必填的字段保留为空或类型字母，则会向您的第一个错误将从数据库中。

你可以通过将数据注释添加到您的模型类添加到应用程序的验证。 数据注释允许描述应用于您的模型属性，所需的规则和 ASP.NET MVC 将会负责的实施并向用户显示适当的消息。

<a id="Ex06Task1"></a>

<a id="Task_1_-_Adding_Data_Annotations"></a>
#### <a name="task-1---adding-data-annotations"></a>任务 1-添加数据批注

在此任务中，你将添加到将使创建和编辑页唱片集模型的数据注释显示在适当的时候验证消息。

对于简单的模型类，添加数据批注仅处理通过添加**使用**语句**System.ComponentModel.DataAnnotation**，然后将放到**[必需]**上适当的属性的属性。 下面的示例会使**名称**属性视图中的必需字段。

[!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample18.cs)]

这是在与此应用程序一样的情况下变得更加复杂实体数据模型生成的位置。 如果你直接向模型类添加数据注释，如果你从数据库更新模型它们将被覆盖。 相反，你可以使用元数据的分部类将是为了容纳批注，并且是与模型关联的类使用**[MetadataType]**属性。

1. 打开**开始**解决方案位于**源/Ex6-AddingValidation/开始/**文件夹。 否则，可能会继续使用**结束**解决方案获取通过完成上一练习。

    1. 如果你打开提供**开始**解决方案，你将需要下载一些缺少的 NuGet 程序包才能继续。 若要执行此操作，请单击**项目**菜单，然后选择**管理 NuGet 包**。
    2. 在**管理 NuGet 包**对话框中，单击**还原**以便下载缺少的程序包。
    3. 最后，通过单击生成解决方案**生成** | **生成解决方案**。

    > [!NOTE]
    > 使用 NuGet 的优点之一是，你无需提供你的项目中的所有库减小项目大小。 使用 NuGet 增强工具，请通过指定的包版本在 Packages.config 文件中，你将能够下载首次运行该项目的所有所需的库。 这是你将需要从本实验打开现有的解决方案后运行这些步骤的原因。
2. 打开**Album.cs**从**模型**文件夹。
3. 替换**Album.cs**内容替换为突出显示的代码，以便其类似于下面所示：

    > [!NOTE]
    > 行**[DisplayFormat(ConvertEmptyStringToNull=false)]**指示在数据源中更新数据字段时，将不会从模型的空字符串转换为 null。 实体框架将 null 值分配给模型之前数据注释验证字段时，此设置将避免异常。

    (代码段- *ASP.NET MVC 4 帮助器、 窗体和验证-Ex6 唱片集元数据分部类*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample19.cs)]

    > [!NOTE]
    > 这**唱片集**分部类具有**MetadataType**属性，用于指向**AlbumMetaData**对数据批注的类。 以下是一些你要用于批注唱片集模型的数据注释属性：
    > 
    > - 需要-指示该属性是必填的字段
    > - DisplayName-定义要在窗体字段以及验证消息上使用的文本
    > - DisplayFormat-指定显示和格式化数据字段的方式。
    > - StringLength-定义字符串字段的最大长度
    > - 范围为-为数字字段提供最大和最小值
    > - ScaffoldColumn-允许隐藏编辑器窗体中的字段

<a id="Ex06Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>任务 2-运行应用程序

在此任务中，你将测试的创建和编辑页验证字段，使用选择的最后一个任务中的显示名称。

1. 按**F5**运行该应用程序。
2. 在主页页面中启动项目。 将 URL 更改为**/StoreManager/创建**。 验证是否显示名称匹配的分部类中的 (如**唱片集艺术作品 URL**而不是**AlbumArtUrl**)
3. 单击**创建**，而不填充窗体。 验证你获取相应的验证消息。

    ![验证创建页中的字段](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "验证创建页中的字段")

    *在创建页中的已验证的字段*
4. 你可以验证相同出现**编辑**页。 将 URL 更改为**/StoreManager/Edit/1**和验证的显示名称与在分部类中的匹配 (如**唱片集艺术作品 URL**而不是**AlbumArtUrl**)。 空**标题**和**价格**字段，然后单击**保存**。 验证你获取相应的验证消息。

    ![在编辑页中的已验证的字段](aspnet-mvc-4-helpers-forms-and-validation/_static/image19.png)

    在编辑页中的已验证的字段

<a id="Exercise7"></a>

<a id="Exercise_7_Using_Unobtrusive_jQuery_at_Client_Side"></a>
### <a name="exercise-7-using-unobtrusive-jquery-at-client-side"></a>练习 7： 在客户端使用非介入式 jQuery

在此练习中，你将了解如何启用在客户端的 MVC 4 非介入式 jQuery 验证。

> [!NOTE]
> 非介入式 jQuery 使用数据 ajax 前缀 JavaScript 来调用在服务器而不是干扰的方式发出内联客户端脚本的操作方法。


<a id="Ex7Task1"></a>

<a id="Task_1_-_Running_the_Application_before_Enabling_Unobtrusive_jQuery"></a>
#### <a name="task-1---running-the-application-before-enabling-unobtrusive-jquery"></a>任务 1-运行应用程序，然后启用非介入式 jQuery

在此任务中，你将运行应用程序，然后才能比较这两个验证模型包括 jQuery。

1. 打开**开始**解决方案位于**源/Ex7-UnobtrusivejQueryValidation/开始/**文件夹。 否则，可能会继续使用**结束**解决方案获取通过完成上一练习。

    1. 如果你打开提供**开始**解决方案，你将需要下载一些缺少的 NuGet 程序包才能继续。 若要执行此操作，请单击**项目**菜单，然后选择**管理 NuGet 包**。
    2. 在**管理 NuGet 包**对话框中，单击**还原**以便下载缺少的程序包。
    3. 最后，通过单击生成解决方案**生成** | **生成解决方案**。

    > [!NOTE]
    > 使用 NuGet 的优点之一是，你无需提供你的项目中的所有库减小项目大小。 使用 NuGet 增强工具，请通过指定的包版本在 Packages.config 文件中，你将能够下载首次运行该项目的所有所需的库。 这是你将需要从本实验打开现有的解决方案后运行这些步骤的原因。
2. 按 **F5** 运行该应用程序。
3. 在主页页面中启动项目。 浏览**/StoreManager/创建**单击**创建**而不填充此表单以验证你收到验证消息：

    ![禁用的客户端验证](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "禁用客户端验证")

    *禁用的客户端验证*
4. 在浏览器中打开的 HTML 源代码：

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample20.html)]

<a id="Ex7Task2"></a>

<a id="Task_2_-_Enabling_Unobtrusive_Client_Validation"></a>
#### <a name="task-2---enabling-unobtrusive-client-validation"></a>任务 2-启用非介入式客户端验证

在此任务中，将启用 jQuery**非介入式客户端验证**从**Web.config**文件，它是默认情况下设置为 false 在所有新的 ASP.NET MVC 4 项目中。 此外，你将添加所需的脚本引用，以使 jQuery 非介入式客户端验证工作。

1. 打开**Web.Config**文件在项目根目录位置，并确保**ClientValidationEnabled**和**UnobtrusiveJavaScriptEnabled**密钥值设置为**true**。

    [!code-xml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample21.xml)]

    > [!NOTE]
    > 此外可以通过在 Global.asax.cs 以获得相同的结果代码来启用客户端验证：
    > 
    > **HtmlHelper.ClientValidationEnabled = true;**
    > 
    > 此外，你可以插入任何控制器具有自定义行为分配 ClientValidationEnabled 特性。
2. 打开**Create.cshtml**在**Views\StoreManager**。
3. 下面的脚本文件，请确保**jquery.validate**和**jquery.validate.unobtrusive**中视图按, 引用&quot; **~/bundles/jqueryval**&quot;捆绑包。

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample22.cshtml)]

    > [!NOTE]
    > 所有这些 jQuery 库包含在 MVC 4 中的新项目。 你可以找到更多库中的**/脚本**你项目的文件夹。
    > 
    > 为了使此验证的库工作，你需要添加对 jQuery framework 库的引用。 由于已在添加此引用 **\_Layout.cshtml**文件，则你不需要将其添加此特定的视图中。

<a id="Ex7Task3"></a>

<a id="Task_3_-_Running_the_Application_Using_Unobtrusive_jQuery_Validation"></a>
#### <a name="task-3---running-the-application-using-unobtrusive-jquery-validation"></a>任务 3-运行应用程序使用非介入式 jQuery 验证

在此任务中，你将测试**StoreManager**创建模板执行客户端验证使用 jQuery 库，当用户创建新唱片集的视图。

1. 按 **F5** 运行该应用程序。
2. 在主页页面中启动项目。 浏览**/StoreManager/创建**单击**创建**而不填充此表单以验证你收到验证消息：

    ![使用 jQuery 启用的客户端验证](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "使用 jQuery 启用的客户端验证")

    *使用 jQuery 启用的客户端验证*
3. 在浏览器中，打开创建视图的源代码:

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample23.html)]

    > [!NOTE]
    > 对于每个客户端验证规则，非介入式 jQuery 添加具有数据的属性-val-*rulename*=&quot;*消息*&quot;。 下面是标记列表该 Unobtrusive jQuery 将插入到要执行客户端验证的 html 输入字段：
    > 
    > - 数据 val
    > - Data-val-number
    > - 数据 val 范围
    > - 数据 val-范围最小/最大数据 val 范围
    > - Data-val-required
    > - 数据 val 长度
    > - 数据 val-长度最大/最小数据 val 长度
    > 
    > 所有数据值将都填入模型**数据注释**。 然后，可以在客户端运行在服务器端工作的所有逻辑。 例如，价格特性具有以下数据注释在模型中：
    > 
    > [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample24.cs)]
    > 
    > 使用非介入式 jQuery 后, 生成的代码为：
    >  
    > [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample25.html)]

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>摘要

通过完成本动手实验中，你已了解如何使用户能够更改带有以下使用的数据库中存储的数据：

- 控制器操作如索引，创建、 编辑、 删除
- ASP.NET MVC 基架功能用于 HTML 表中显示属性
- 自定义 HTML 帮助器以改进用户体验
- 响应 HTTP GET 或 HTTP POST 调用的操作方法
- 类似的视图模板，如创建和编辑共享的编辑器模板
- 窗体元素，如下拉列表
- 模型验证的数据批注
- 使用 jQuery 非介入式库的客户端验证

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>附录 a： 安装 Visual Studio Express 2012 for Web

你可以安装**Microsoft Visual Studio Express 2012 for Web**或另一个&quot;Express&quot;版本使用 **[Microsoft Web 平台安装程序](https://www.microsoft.com/web/downloads/platform.aspx)**. 以下说明将指导你完成安装所需的步骤*Visual studio Express 2012 for Web*使用*Microsoft Web 平台安装程序*。

1. 转到[ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)。 或者，如果你已安装 Web 平台安装程序，你可以打开它，并搜索产品&quot; *Visual Studio Express 2012 for Web 的 Windows Azure SDK*&quot;。
2. 单击**立即安装**。 如果你没有**Web 平台安装程序**将重定向以下载并请先安装它。
3. 一次**Web 平台安装程序**处于打开状态，单击**安装**以启动安装程序。

    ![安装 Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "安装 Visual Studio Express")

    *安装 Visual Studio Express*
4. 阅读所有产品的许可证和条款，然后单击**我接受**以继续。

    ![接受许可条款](aspnet-mvc-4-helpers-forms-and-validation/_static/image23.png)

    *接受许可条款*
5. 等待，直到下载和安装过程完成。

    ![安装进度](aspnet-mvc-4-helpers-forms-and-validation/_static/image24.png)

    *安装进度*
6. 当安装完成后时，单击**完成**。

    ![安装已完成](aspnet-mvc-4-helpers-forms-and-validation/_static/image25.png)

    *安装已完成*
7. 单击**退出**以关闭 Web 平台安装程序。
8. 若要打开 Visual Studio Express for Web，请转到**启动**屏幕并开始编写&quot; **VS Express**&quot;，然后单击**VS Express for Web**磁贴。

    ![Web 磁贴的 VS Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image26.png)

    *Web 磁贴的 VS Express*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>附录 b： 使用代码片段

和代码片段，必须将你能够轻松获得所需的所有代码所示。 实验室文档将告诉您完全时你可以使用它们，如下图中所示。

![使用 Visual Studio 代码段将代码插入到你的项目](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "使用 Visual Studio 代码片段，可将代码插入到你的项目")

*使用 Visual Studio 代码段将代码插入到你的项目*

***若要添加代码片段使用键盘 (仅限 C#)***

1. 将光标置于想要插入代码。
2. 开始键入代码段名称 （不带空格或连字符）。
3. 观看作为 IntelliSense 显示匹配的代码段的名称。
4. 选择正确的代码段 （或继续键入直到选中整个代码段的名称）。
5. 按 Tab 键两次以光标位置处插入代码段。

![开始键入代码段名称](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "开始键入代码段名称")

*开始键入代码段名称*

![按 Tab 以选择突出显示代码段](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "按选项卡以选择突出显示代码段")

*按 Tab 以选择突出显示代码段*

![再次按 Tab 和代码段将会扩展](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "再次按 Tab 和代码段将会扩展")

*再次按 Tab 和代码段将会扩展*

***若要添加代码片段使用鼠标 （C#、 Visual Basic 和 XML）*** 1。 右键单击你想要插入代码段。

1. 选择**插入代码段**跟**我的代码段**。
2. 选择相关的代码段从列表中，通过单击它。

![右键单击你想要插入代码段，并选择插入代码段](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "右键单击你想要插入代码段，并选择插入代码段")

*右键单击你想要插入代码段，并选择插入代码段*

![通过单击它选取列表中中的相关代码片段](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "选取相关的代码段从列表中，通过单击它")

*通过单击它选取从列表中，相关代码段*
