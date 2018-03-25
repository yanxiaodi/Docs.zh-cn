---
uid: web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
title: "生成与 ASP.NET Web API RESTful Api |Microsoft 文档"
author: rick-anderson
description: "最近几年，它已变得明确 HTTP 并不只是为了提供 HTML 页面。 它也是一个功能强大的平台，用于生成 Web Api，使用几 o..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 87daa99f-3810-407e-b969-dd28a192959d
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 49dcd86649ceb77cd5a02ebeb5d9d7b11ff4f344
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="build-restful-apis-with-aspnet-web-api"></a>生成与 ASP.NET Web API RESTful Api
====================
通过[Web 营地团队](https://twitter.com/webcamps)

> 最近几年，它已变得明确 HTTP 并不只是为了提供 HTML 页面。 它也是一个功能强大的平台，用于生成 Web Api，使用少量的谓词 （GET、 POST 和等） 以及几个简单的概念类似于*Uri*和*标头*。 ASP.NET Web API 是一组组件，用于简化 HTTP 编程。 由于它构建基于 ASP.NET MVC 运行时，Web API 将自动处理 HTTP 低级别的传输详细的信息。 同时，Web API 自然公开 HTTP 编程模型。 事实上，Web api 的一个目标是为*不*抽象化 HTTP 的现实。 因此，Web API 是灵活且易于扩展。 在此动手实验中，你将使用 Web API 构建一个简单的 REST API 为联系人管理器应用程序。 你还将建立客户端使用该 API。 REST 体系结构样式已被证明是一种利用 HTTP 的有效方法，尽管它肯定不是唯一有效的 HTTP 方法。 联系人管理器将公开用于列出、 添加和删除联系人，以及其他基于 Rest。 本实验需要基本了解 HTTP，其余部分，并且假定你已基本了解 HTML、 JavaScript 和 jQuery。
> 
> > [!NOTE]
> > ASP.NET Web 站点有专用于在 ASP.NET Web API 框架的区域[ [https://asp.net/web-api](https://asp.net/web-api)](https://asp.net/web-api)。 此站点将继续提供最新信息、 示例和新闻与 Web API，因此它经常检查如果你想要深入探讨创建可用于几乎任何设备或开发框架自定义 Web Api 的技巧。
> > 
> > ASP.NET Web API，类似于 ASP.NET MVC 4，具有很大的灵活性，以将服务层与允许你同时使用多个可用的依赖关系注入框架变得相当容易控制器分开。 没有很好的示例演示如何使用你可以下载它从 ASP.NET Web API 项目中的依赖关系注入 Ninject 的 MSDN 中[此处](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7)。
> 
> 
> 在 Web 营地培训工具包中，在包括所有的示例代码和代码段[https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409)。


<a id="Objectives"></a>
### <a name="objectives"></a>目标

在此动手实验中，你将了解如何：

- 实现 rest 样式 Web API
- 从 HTML 客户端调用 API

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>先决条件

完成本动手实验需要以下：

- [Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)或更高 (读取[附录 B](#AppendixB)有关如何安装它的说明)。

<a id="Setup"></a>
### <a name="setup"></a>安装

**安装代码片段**

为方便起见，你将沿此实验室管理大部分都是代码的可用作 Visual Studio 代码段。 若要安装运行的代码段**.\Source\Setup\CodeSnippets.vsi**文件。

如果你不熟悉 Visual Studio 代码段，并想要了解如何使用它们，你可以从该文档引用的附录&quot;[附录 a： 使用代码段](#AppendixA)&quot;。

<a id="Exercises"></a>
## <a name="exercises"></a>练习

本动手实验包括以下练习：

1. [练习 1： 创建只读的 Web API](#Exercise1)
2. [练习 2： 创建读/写 Web API](#Exercise2)
3. [练习 3： 使用 HTML 客户端从 Web API](#Exercise3)

> [!NOTE]
> 每个练习均附带由**结束**包含生成您应该完成练习后获得的解决方案文件夹。 如果你需要通过在练习工作的更多帮助，可以使用此解决方案作为指南。


估计时间来完成该实验： **60 分钟**。

<a id="Exercise1"></a>

<a id="Exercise_1_Create_a_Read-Only_Web_API"></a>
### <a name="exercise-1-create-a-read-only-web-api"></a>练习 1： 创建只读的 Web API

在此练习中，将为联系人管理器实现只读的 GET 方法。

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_API_Project"></a>
#### <a name="task-1---creating-the-api-project"></a>任务 1-创建 API 项目

在此任务中，你将使用新的 ASP.NET web 项目模板创建 Web API 的 web 应用程序。

1. 运行**Visual Studio 2012 Express for Web**，为此请转到**启动**和类型**VS Express for Web**然后按**Enter**。
2. 从**文件**菜单上，选择**新项目**。 选择**Visual C# |Web**项目类型从项目类型树视图，然后选择**ASP.NET MVC 4 Web 应用程序**项目类型。 设置项目的**名称**到*ContactManager*和**解决方案名称**到*开始*，然后单击**确定**.

    ![创建新的 ASP.NET MVC 4.0 Web 应用程序项目](build-restful-apis-with-aspnet-web-api/_static/image1.png "创建新的 ASP.NET MVC 4.0 Web 应用程序项目")

    *创建新的 ASP.NET MVC 4.0 Web 应用程序项目*
3. 在 ASP.NET MVC 4 项目的类型对话框中，选择**Web API**项目类型。 单击“确定”。

    ![指定 Web API 项目类型](build-restful-apis-with-aspnet-web-api/_static/image2.png "指定 Web API 项目类型")

    *指定 Web API 项目类型*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_the_Contact_Manager_API_Controllers"></a>
#### <a name="task-2---creating-the-contact-manager-api-controllers"></a>任务 2-创建联系人管理器 API 控制器

在此任务中，你将创建 API 方法将驻留在其中的控制器类。

1. 删除名为的文件**ValuesController.cs**内**控制器**从项目的文件夹。
2. 右键单击**控制器**文件夹中该项目并选择**添加 |控制器**从上下文菜单。

    ![向项目添加新的控制器](build-restful-apis-with-aspnet-web-api/_static/image3.png "向项目添加新控制器")

    *向项目添加新控制器*
3. 在**添加控制器**对话框中，选择**空 API 控制器**的模板菜单中。 将控制器类**ContactController**。 然后，单击**添加。**

    ![使用添加控制器对话框创建一个新的 Web API 控制器](build-restful-apis-with-aspnet-web-api/_static/image4.png "使用添加控制器对话框创建一个新的 Web API 控制器")

    *使用添加控制器对话框创建一个新的 Web API 控制器*
4. 以下代码添加到**ContactController**。

    (代码段- *Web API 实验-Ex01-Get API 方法*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample1.cs)]
5. 按**F5**调试应用程序。 Web API 项目的默认主页应显示。

    ![ASP.NET Web API 应用程序的默认主页](build-restful-apis-with-aspnet-web-api/_static/image5.png "ASP.NET Web API 应用程序的默认主页")

    *ASP.NET Web API 应用程序的默认主页*
6. 在 Internet Explorer 窗口中，按**F12**键以打开**开发人员工具**窗口。 单击**网络**选项卡上，并依次**启动捕获**按钮以开始到窗口中捕获网络流量。

    ![打开网络选项卡并启动网络捕获](build-restful-apis-with-aspnet-web-api/_static/image6.png "打开网络选项卡并启动网络捕获")

    *打开网络选项卡并启动网络捕获*
7. 追加与浏览器的地址栏中的 URL **/api/联系人**，然后按 enter。 传输详细信息会在网络捕获窗口中显示。 请注意，响应的 MIME 类型是**应用程序/json**。 此示例演示如何将默认输出格式为 JSON。

    ![在网络视图中查看 Web API 请求的输出中](build-restful-apis-with-aspnet-web-api/_static/image7.png "在网络视图中查看的 Web API 请求的输出")

    *在网络视图中查看的 Web API 请求的输出*

    > [!NOTE]
    > Internet Explorer 10 的默认行为在这将会询问如果用户想要保存或打开的 Web API 调用导致的流。 输出将包含 Web API URL 调用的 JSON 结果的文本文件。 若要观看通过开发人员工具窗口的响应的内容无法取消对话框。
8. 单击**转到详细视图**按钮以查看有关此 API 调用的响应的更多详细信息。

    ![切换到详细视图](build-restful-apis-with-aspnet-web-api/_static/image8.png "切换到详细信息视图")

    *切换到详细视图*
9. 单击**响应正文**选项卡以查看实际的 JSON 响应文本。

    ![查看 JSON 输出中网络监视器文本](build-restful-apis-with-aspnet-web-api/_static/image9.png "查看 JSON 输出中网络监视器的文本")

    *在网络监视器中查看的 JSON 输出文本*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Creating_the_Contact_Models_and_Augment_the_Contact_Controller"></a>
#### <a name="task-3---creating-the-contact-models-and-augment-the-contact-controller"></a>任务 3-创建联系人模型和补充联系人控制器

在此任务中，你将创建 API 方法将驻留在其中的控制器类。

1. 右键单击**模型**文件夹，然后选择**添加 |类...**从上下文菜单。

    ![将新的模型添加到 web 应用程序](build-restful-apis-with-aspnet-web-api/_static/image10.png "将新的模型添加到 web 应用程序")

    *将新的模型添加到 web 应用程序*
2. 在**添加新项**对话框中，将新文件**Contact.cs**单击**添加。**

    ![创建新的联系人类文件](build-restful-apis-with-aspnet-web-api/_static/image11.png "创建新的联系人类文件")

    *创建新的联系人类文件*
3. 添加以下突出显示的代码**联系人**类。

    (代码段- *Web API 实验-Ex01-联系人类*)


    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample2.cs)]
4. 在**ContactController**类中，选择 word**字符串**方法定义中**获取**方法和类型单词*联系人*。 中键入的单词后, 一个指示器将出现在单词的开头**联系人**。 可以按住**Ctrl**键和按句点 （.） 键或单击使用鼠标以打开代码编辑器中，以自动填充中的帮助对话框图标**使用**指令的模型命名空间。

    ![对于命名空间声明中使用 Intellisense 帮助](build-restful-apis-with-aspnet-web-api/_static/image12.png)

    *对于命名空间声明中使用 Intellisense 帮助*
5. 修改的代码**获取**方法，以便它返回的联系人模型实例数组。

    (代码段-*返回的联系人列表 Web API 实验-Ex01-*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample3.cs)]
6. 按**F5**调试 web 应用程序在浏览器中的。 若要查看到 API 的响应输出所做的更改，请执行以下步骤。

    1. 浏览器打开后，按**F12**如果开发人员工具还不打开。
    2. 单击**网络**选项卡。
    3. 按**启动捕获**按钮。
    4. 添加 URL 后缀**/api/联系人**到地址栏，然后按 URL **Enter**密钥。
    5. 按**转到详细视图**按钮。
    6. 选择**响应正文**选项卡。你应看到表示联系人实例的数组的序列化的格式的 JSON 字符串。

    ![JSON 序列化复杂的 Web API 方法调用的输出](build-restful-apis-with-aspnet-web-api/_static/image13.png "JSON 序列化复杂的 Web API 方法调用的输出")

    *复杂的 Web API 方法调用的序列化的 JSON 输出*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Extracting_Functionality_into_a_Service_Layer"></a>
#### <a name="task-4---extracting-functionality-into-a-service-layer"></a>任务 4-解压功能集成到服务层

此任务将演示如何提取到服务层，以便可以方便开发人员能够从控制器层，从而允许的实际完成工作的服务的可重用性分隔其服务功能的功能。

1. 在解决方案根目录中创建一个新文件夹并将其命名**服务**。 要执行此操作，请右键单击**ContactManager**项目，依次选择**添加** | **新文件夹**，将其命名为*服务*。

    ![创建服务文件夹](build-restful-apis-with-aspnet-web-api/_static/image14.png "创建服务文件夹")

    *创建服务文件夹*
2. 右键单击**服务**文件夹，然后选择**添加 |类...**从上下文菜单。

    ![将新类添加到服务文件夹](build-restful-apis-with-aspnet-web-api/_static/image15.png "将新类添加到服务文件夹")

    *将新类添加到服务文件夹*
3. 当**添加新项**对话框出现时，将新类**ContactRepository**单击**添加**。

    ![创建要包含联系人存储库服务层的代码的类文件](build-restful-apis-with-aspnet-web-api/_static/image16.png "创建类文件以包含联系人存储库服务层的代码")

    *创建一个类文件以包含联系人存储库服务层的代码*
4. 添加一条 using 指令至**ContactRepository.cs**文件以包括模型命名空间。


    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample4.cs)]
5. 添加以下突出显示的代码**ContactRepository.cs**文件实现 GetAllContacts 方法。

    (代码段- *Web API 实验-Ex01-联系存储库*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample5.cs)]
6. 打开**ContactController.cs**如果尚未打开文件。
7. 将以下代码添加到该文件的命名空间声明部分使用语句。


    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample6.cs)]
8. 添加以下突出显示的代码**ContactController.cs**类添加一个私有字段来表示实例存储库，以便成员可以产生的类的其余部分使用的服务实现。

    (代码段-*的 Web API 实验-Ex01-联系控制器*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample7.cs)]
9. 更改**获取**方法，以便它可以使用的联系人的存储库服务。

    (代码段-*通过存储库中返回的联系人列表 Web API 实验-Ex01-*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample8.cs)]
10. 放置一个断点**ContactController**的**获取**方法定义。

    ![将断点添加到联系人控制器](build-restful-apis-with-aspnet-web-api/_static/image17.png "将断点添加到联系人控制器")

    *将断点添加到联系人控制器*
11. 按 **F5** 运行该应用程序。
12. 当浏览器打开后时，按**F12**打开开发人员工具。
13. 单击**网络**选项卡。
14. 单击**启动捕获**按钮。
15. 追加后缀的地址栏中的 URL **/api/联系人**按**Enter**加载 API 控制器。
16. Visual Studio 2012 应会一次中断**获取**方法开始执行。

    ![Get 方法中的重大](build-restful-apis-with-aspnet-web-api/_static/image18.png "重大内 Get 方法")

    *Get 方法中的重大*
17. 按**F5**以继续。
18. 返回到 Internet Explorer 如果尚不具有焦点。 请注意网络捕获窗口。

    ![网络中显示的 Web API 调用的结果的 Internet 资源管理器视图](build-restful-apis-with-aspnet-web-api/_static/image19.png "网络中显示的 Web API 调用的结果的 Internet 资源管理器视图")

    *在 Internet Explorer 中显示的 Web API 调用的结果的网络视图*
19. 单击**转到详细视图**按钮。
20. 单击**响应正文**选项卡。请注意 API 调用中，和它如何表示检索的服务层的两个联系人的 JSON 输出。

    ![在开发人员工具窗口中查看 Web API 的 JSON 输出](build-restful-apis-with-aspnet-web-api/_static/image20.png "在开发人员工具窗口中查看 Web API 的 JSON 输出")

    *在开发人员工具窗口中查看 Web API 的 JSON 输出*

<a id="Exercise2"></a>

<a id="Exercise_2_Create_a_ReadWrite_Web_API"></a>
### <a name="exercise-2-create-a-readwrite-web-api"></a>练习 2： 创建读/写 Web API

在本练习中，你将实现 POST 和 PUT 方法联系人管理器中，若要使用数据编辑功能启用它。

<a id="Ex2Task1"></a>

<a id="Task_1_-_Opening_the_Web_API_Project"></a>
#### <a name="task-1---opening-the-web-api-project"></a>任务 1-打开 Web API 项目

在此任务中，你将准备增强，以便它可以接受用户输入在练习 1 中创建的 Web API 项目。

1. 运行**Visual Studio 2012 Express for Web**，为此请转到**启动**和类型**VS Express for Web**然后按**Enter**。
2. 打开**开始**解决方案位于**源/Ex02-ReadWriteWebAPI/开始/**文件夹。 否则，可能会继续使用**结束**解决方案获取通过完成上一练习。

    1. 如果你打开提供**开始**解决方案，你将需要下载一些缺少的 NuGet 程序包才能继续。 若要执行此操作，请单击**项目**菜单，然后选择**管理 NuGet 包**。
    2. 在**管理 NuGet 包**对话框中，单击**还原**以便下载缺少的程序包。
    3. 最后，通过单击生成解决方案**生成** | **生成解决方案**。

    > [!NOTE]
    > 使用 NuGet 的优点之一是，你无需提供你的项目中的所有库减小项目大小。 使用 NuGet 增强工具，请通过指定的包版本在 Packages.config 文件中，你将能够下载首次运行该项目的所有所需的库。 这是你将需要从本实验打开现有的解决方案后运行这些步骤的原因。
3. 打开**Services/ContactRepository.cs**文件。

<a id="Ex2Task2"></a>

<a id="Task_2_-_Adding_Data-Persistence_Features_to_the_Contact_Repository_Implementation"></a>
#### <a name="task-2---adding-data-persistence-features-to-the-contact-repository-implementation"></a>任务 2-添加的联系人的存储库实现的数据暂留功能

在此任务中，将增加，以便它可以保留并接受用户输入和新的联系人实例在练习 1 中创建的 Web API 项目 ContactRepository 类。

1. 添加以下常量为**ContactRepository**类来表示 web 服务器缓存项键名称的更高版本在本练习中的名称。

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample9.cs)]
2. 添加到一个构造函数**ContactRepository**包含下面的代码。

    (代码段- *Web API 实验-Ex02 的联系人的存储库构造函数*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample10.cs)]
3. 修改的代码**GetAllContacts**方法如下所示。

    (代码段- *Web API 实验-Ex02-获取所有联系人*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample11.cs)]

    > [!NOTE]
    > 此示例出于演示目的，并将使用 web 服务器的缓存作为存储媒体，以便值将可供多个客户端同时，而不使用会话存储机制或请求存储生存期。 可以使用实体框架、 XML 存储或任何其他各种代替 web 服务器缓存。
4. 实现一个名为的新方法**SaveContact**到**ContactRepository**类来执行将联系人保存的工作。 **SaveContact**方法应采用单个**联系人**参数和返回一个布尔值，该值指示成功或失败。

    (代码段- *Web API 实验-Ex02-实现 SaveContact 方法*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample12.cs)]

<a id="Exercise3"></a>

<a id="Exercise_3_Consume_the_Web_API_from_an_HTML_Client"></a>
### <a name="exercise-3-consume-the-web-api-from-an-html-client"></a>练习 3： 使用 HTML 客户端从 Web API

在此练习中，你将创建 HTML 客户端调用 Web API。 此客户端将使用 Web API 使用 JavaScript 方便数据交换，并将在 web 浏览器使用 HTML 标记中显示结果。

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Displaying_Contacts"></a>
#### <a name="task-1---modifying-the-index-view-to-provide-a-gui-for-displaying-contacts"></a>任务 1-修改该索引视图，以提供用于显示联系人的 GUI

在此任务中，你将要修改默认索引视图的 web 应用程序以支持在 HTML 浏览器中显示现有联系人的列表的要求。

1. 打开**Visual Studio 2012 Express for Web**如果尚未打开。
2. 打开**开始**解决方案位于**源/Ex03-ConsumingWebAPI/开始/**文件夹。 否则，可能会继续使用**结束**解决方案获取通过完成上一练习。

    1. 如果你打开提供**开始**解决方案，你将需要下载一些缺少的 NuGet 程序包才能继续。 若要执行此操作，请单击**项目**菜单，然后选择**管理 NuGet 包**。
    2. 在**管理 NuGet 包**对话框中，单击**还原**以便下载缺少的程序包。
    3. 最后，通过单击生成解决方案**生成** | **生成解决方案**。

    > [!NOTE]
    > 使用 NuGet 的优点之一是，你无需提供你的项目中的所有库减小项目大小。 使用 NuGet 增强工具，请通过指定的包版本在 Packages.config 文件中，你将能够下载首次运行该项目的所有所需的库。 这是你将需要从本实验打开现有的解决方案后运行这些步骤的原因。
3. 打开**Index.cshtml**文件位于**视图/主页**文件夹。
4. 在 div 元素的 HTML 代码替换 id**正文**以便其类似下列代码所示。


    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample13.html)]
5. 在执行对 Web API 的 HTTP 请求的文件的底部添加以下 Javascript 代码。


    [!code-cshtml[Main](build-restful-apis-with-aspnet-web-api/samples/sample14.cshtml)]
6. 打开**ContactController.cs**如果尚未打开文件。
7. 上放置一个断点**获取**方法**ContactController**类。

    ![将断点放置在 API 控制器的 Get 方法](build-restful-apis-with-aspnet-web-api/_static/image21.png "将断点放置在 API 控制器的 Get 方法")

    *将断点放置在 API 控制器的 Get 方法*
8. 按**F5**以运行该项目。 浏览器将加载 HTML 文档。

    > [!NOTE]
    > 确保您在浏览到你的应用程序的根 URL。
9. 加载页面并 JavaScript 执行，将会命中断点，并执行代码将暂停在控制器中。

    ![调试使用适用于 Web VS Express 的 Web API 调用到](build-restful-apis-with-aspnet-web-api/_static/image22.png "调试到使用适用于 Web VS Express 的 Web API 调用")

    *使用适用于 Web Visual Studio 2012 Express 的 Web API 调用调试*
10. 删除断点和按**F5**或调试工具栏**继续**按钮以继续加载浏览器中的视图。 Web API 调用完成后，你应看到在浏览器中调用显示为列表项从 Web API 返回的联系人。

    ![在浏览器中显示为列表项的 API 调用的结果](build-restful-apis-with-aspnet-web-api/_static/image23.png "在浏览器中显示为列表项的 API 调用的结果")

    *在浏览器中显示为列表项的 API 调用的结果*
11. 停止调试。

<a id="Ex3Task2"></a>

<a id="Task_2_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Creating_Contacts"></a>
#### <a name="task-2---modifying-the-index-view-to-provide-a-gui-for-creating-contacts"></a>任务 2-修改该索引视图，以提供用于创建联系人的 GUI

在此任务中，你将继续修改索引视图的 MVC 应用程序。 窗体将添加到将捕获用户输入并将其发送到 Web API 来创建一个新的联系人的 HTML 页，并将创建一个新的 Web API 控制器方法来从 GUI 收集日期。

1. 打开**ContactController.cs**文件。
2. 将新方法添加到名为的控制器类**Post**中下面的代码所示。

    (代码段- *Web API 实验室-Ex03-Post 方法*)


    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample15.cs)]
3. 打开**Index.cshtml**文件在 Visual Studio 中，如果未打开。
4. 将以下 HTML 代码之后添加在上一任务中的无序列表添加到文件。


    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample16.html)]
5. 在底部的文档的脚本元素中，添加以下突出显示的代码，以处理按钮单击事件，将将数据发布到 Web API 使用的 HTTP POST 调用。

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample17.html)]
6. 在**ContactController.cs**上, 放置一个断点**Post**方法。
7. 按**F5**浏览器中运行该应用程序。
8. 一旦加载页面时浏览器中，键入新的联系人姓名和 Id，然后单击**保存**按钮。

    ![客户端 HTML 文档加载到浏览器](build-restful-apis-with-aspnet-web-api/_static/image24.png "客户端 HTML 文档加载到浏览器")

    *客户端 HTML 文档加载到浏览器*
9. 调试器窗口的中断时**Post**方法，是看一下的属性**联系**参数。 值应匹配窗体中输入的数据。

    ![从客户端发送到 Web API 的联系人对象](build-restful-apis-with-aspnet-web-api/_static/image25.png "从客户端发送到 Web API 的联系人对象")

    *正在从客户端发送到 Web API 的联系人对象*
10. 单步执行在调试器中直到方法**响应**创建变量。 在中检查**局部变量**在调试器中的窗口中，你将看到已设置了所有属性。

    ![以下在调试器中的创建的响应](build-restful-apis-with-aspnet-web-api/_static/image26.png "响应后在调试器中的创建")

    *响应后在调试器中的创建*
11. 如果按**F5**或单击**继续**请求将在调试器中完成。 新的联系人后切换回浏览器时，已添加到存储的联系人列表**ContactRepository**实现。

    ![浏览器反映成功创建新的联系人实例](build-restful-apis-with-aspnet-web-api/_static/image27.png "浏览器反映成功创建新的联系人实例")

    *浏览器反映成功创建新的联系人实例*

> [!NOTE]
> 此外，你可以部署此应用程序对 Azure 以下[附录 c： 发布 ASP.NET MVC 4 应用程序使用 Web Deploy](#AppendixC)。


* * *

<a id="Summary"></a>
## <a name="summary"></a>摘要

此实验室中已引入到新的 ASP.NET Web API 框架和使用框架的 RESTful Web Api 的实现。 从这里，你可以创建新的存储库，它方便了数据持久性使用任意数量的机制，并连接该服务而不是作为示例在本实验中提供的简单一个。 Web API 支持大量的其他功能，如启用从任何支持 HTTP 和 JSON 或 XML 的语言中编写的非 HTML 客户端的通信。 承载 Web API 典型 web 应用程序之外的能力也是有可能，以及是创建您自己的序列化格式的功能。

ASP.NET Web 站点有专用于在 ASP.NET Web API 框架的区域[ [https://asp.net/web-api](https://asp.net/web-api)](https://asp.net/web-api)。 此站点将继续提供最新信息、 示例和新闻与 Web API，因此它经常检查如果你想要深入探讨创建可用于几乎任何设备或开发框架自定义 Web Api 的技巧。

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a>附录 a： 使用代码片段

和代码片段，必须将你能够轻松获得所需的所有代码所示。 实验室文档将告诉您完全时你可以使用它们，如下图中所示。

![使用 Visual Studio 代码段将代码插入到你的项目](build-restful-apis-with-aspnet-web-api/_static/image28.png "使用 Visual Studio 代码片段，可将代码插入到你的项目")

*使用 Visual Studio 代码段将代码插入到你的项目*

<a id="CodeSnippetUsingKeyBoard"></a>

<a id="To_add_a_code_snippet_using_the_keyboard_C_only"></a>
### <a name="to-add-a-code-snippet-using-the-keyboard-c-only"></a>若要添加代码片段使用键盘 (仅限 C#)

1. 将光标置于想要插入代码。
2. 开始键入代码段名称 （不带空格或连字符）。
3. 观看作为 IntelliSense 显示匹配的代码段的名称。
4. 选择正确的代码段 （或继续键入直到选中整个代码段的名称）。
5. 按 Tab 键两次以光标位置处插入代码段。

    ![开始键入代码段名称](build-restful-apis-with-aspnet-web-api/_static/image29.png "开始键入代码段名称")

    *开始键入代码段名称*

    ![按 Tab 以选择突出显示代码段](build-restful-apis-with-aspnet-web-api/_static/image30.png "按选项卡以选择突出显示代码段")

    *按 Tab 以选择突出显示代码段*

    ![再次按 Tab 和代码段将会扩展](build-restful-apis-with-aspnet-web-api/_static/image31.png "再次按 Tab 和代码段将会扩展")

    *再次按 Tab 和代码段将会扩展*

<a id="CodeSnippetUsingMouse"></a>

<a id="To_add_a_code_snippet_using_the_mouse_C_Visual_Basic_and_XML"></a>
### <a name="to-add-a-code-snippet-using-the-mouse-c-visual-basic-and-xml"></a>若要添加代码片段使用鼠标 （C#、 Visual Basic 和 XML）

1. 右键单击你想要插入代码段。
2. 选择**插入代码段**跟**我的代码段**。
3. 选择相关的代码段从列表中，通过单击它。

    ![右键单击你想要插入代码段，并选择插入代码段](build-restful-apis-with-aspnet-web-api/_static/image32.png "右键单击你想要插入代码段，并选择插入代码段")

    *右键单击你想要插入代码段，并选择插入代码段*

    ![通过单击它选取列表中中的相关代码片段](build-restful-apis-with-aspnet-web-api/_static/image33.png "选取相关的代码段从列表中，通过单击它")

    *通过单击它选取从列表中，相关代码段*

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a>附录 b： 安装 Visual Studio Express 2012 for Web

你可以安装**Microsoft Visual Studio Express 2012 for Web**或另一个&quot;Express&quot;版本使用 **[Microsoft Web 平台安装程序](https://www.microsoft.com/web/downloads/platform.aspx)**. 以下说明将指导你完成安装所需的步骤*Visual studio Express 2012 for Web*使用*Microsoft Web 平台安装程序*。

1. 转到[ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)。 或者，如果你已安装 Web 平台安装程序，你可以打开它，并搜索产品&quot; *Visual Studio Express 2012 for Web 与 Azure SDK*&quot;。
2. 单击**立即安装**。 如果你没有**Web 平台安装程序**将重定向以下载并请先安装它。
3. 一次**Web 平台安装程序**处于打开状态，单击**安装**以启动安装程序。

    ![安装 Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "安装 Visual Studio Express")

    *安装 Visual Studio Express*
4. 阅读所有产品的许可证和条款，然后单击**我接受**以继续。

    ![接受许可条款](build-restful-apis-with-aspnet-web-api/_static/image35.png)

    *接受许可条款*
5. 等待，直到下载和安装过程完成。

    ![安装进度](build-restful-apis-with-aspnet-web-api/_static/image36.png)

    *安装进度*
6. 当安装完成后时，单击**完成**。

    ![安装已完成](build-restful-apis-with-aspnet-web-api/_static/image37.png)

    *安装已完成*
7. 单击**退出**以关闭 Web 平台安装程序。
8. 若要打开 Visual Studio Express for Web，请转到**启动**屏幕并开始编写&quot; **VS Express**&quot;，然后单击**VS Express for Web**磁贴。

    ![Web 磁贴的 VS Express](build-restful-apis-with-aspnet-web-api/_static/image38.png)

    *Web 磁贴的 VS Express*

<a id="AppendixC"></a>

<a id="Appendix_C_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-c-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>附录 c： 发布 ASP.NET MVC 4 应用程序使用 Web 部署

本附录将演示如何从 Azure 门户创建新的网站和发布应用程序获取按照本实验中，利用 Azure 提供的 Web 部署发布功能。

<a id="ApxCTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a>任务 1-从 Azure 门户创建新网站

1. 转到[Azure 管理门户](https://manage.windowsazure.com/)并使用与你的订阅关联的 Microsoft 凭据登录。

    > [!NOTE]
    > 使用 Azure 可以免费承载 10 个 ASP.NET 网站，然后随着流量增长而扩展。 你可以注册[此处](http://aka.ms/aspnet-hol-azure)。

    ![登录到 Windows Azure 门户](build-restful-apis-with-aspnet-web-api/_static/image39.png "登录到 Windows Azure 门户")

    *登录到门户*
2. 单击**新建**命令栏上。

    ![创建新网站](build-restful-apis-with-aspnet-web-api/_static/image40.png "创建新网站")

    *创建新网站*
3. 单击**计算** | **网站**。 然后选择**快速创建**选项。 为新网站提供可用的 URL，然后单击**创建网站**。

    > [!NOTE]
    > Azure 是运行在云中，你可以控制和管理 web 应用程序的宿主。 快速创建选项，可部署到从 Azure 门户外部的已完成的 web 应用程序。 它不包括用于设置数据库的步骤。

    ![创建新的网站使用快速创建](build-restful-apis-with-aspnet-web-api/_static/image41.png "创建新的网站使用快速创建")

    *创建新的网站使用快速创建*
4. 等到新**网站**创建。
5. 创建网站后单击下的链接**URL**列。 检查新的 Web 站点工作。

    ![浏览到新的 web 站点](build-restful-apis-with-aspnet-web-api/_static/image42.png "浏览到新的 web 站点")

    *浏览到新的 web 站点*

    ![运行网站](build-restful-apis-with-aspnet-web-api/_static/image43.png "运行的网站")

    *运行的网站*
6. 返回到门户并单击在网站的名称**名称**列以显示的管理页。

    ![打开网站管理页](build-restful-apis-with-aspnet-web-api/_static/image44.png "打开网站管理页")

    *打开网站管理页*
7. 在**仪表板**页上，在**速览**部分中，单击**下载发布配置文件**链接。

    > [!NOTE]
    > *发布配置文件*包含所有发布到 Azure，以便每个启用的发布方法的 web 应用程序所需的信息。 发布配置文件包含的 Url、 用户凭据和连接到并针对每个发布方法启用的终结点进行身份验证所需的数据库字符串。 **Microsoft WebMatrix 2**， **Microsoft Visual Studio Express for Web**和**Microsoft Visual Studio 2012**支持读取发布配置文件以自动执行的这些程序，以便配置web 应用程序发布到 Azure。

    ![下载网站发布配置文件](build-restful-apis-with-aspnet-web-api/_static/image45.png "下载网站发布配置文件")

    *下载网站发布配置文件*
8. 到一个已知位置下载发布配置文件。 进一步在本练习中，你将了解如何使用此文件发布到 Azure web 应用程序从 Visual Studio。

    ![保存发布配置文件](build-restful-apis-with-aspnet-web-api/_static/image46.png "保存发布配置文件")

    *保存发布配置文件*

<a id="ApxCTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>任务 2-配置数据库服务器

如果你的应用程序将使用 SQL Server 数据库将需要创建 SQL 数据库服务器。 如果你想要部署的简单应用程序不使用 SQL Server 可能会跳过此任务。

1. 需要将用于存储应用程序数据库的 SQL 数据库服务器。 你可以从你的订阅在 Azure 管理门户中查看 SQL 数据库服务器**Sql 数据库** | **服务器** | **服务器的仪表板**. 如果你没有创建的服务器，则可以创建一个使用**添加**命令栏上的按钮。 请记下的**服务器名称和 URL、 管理员登录名和密码**，如你将在接下来的任务中使用它们。 不创建数据库，它将在后面的阶段中创建。

    ![SQL 数据库服务器仪表板](build-restful-apis-with-aspnet-web-api/_static/image47.png "SQL Database 服务器仪表板")

    *SQL 数据库服务器仪表板*
2. 在下一个任务将测试从 Visual Studio 中，数据库连接，因此你需要在的服务器的列表中包括你的本地 IP 地址**允许的 IP 地址**。 若要做到这一点，请单击**配置**，选择的 IP 地址从**当前客户端 IP 地址**并将其粘贴在**起始 IP 地址**和**结束 IP 地址**文本框和单击![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png)按钮。

    ![添加客户端 IP 地址](build-restful-apis-with-aspnet-web-api/_static/image49.png)

    *添加客户端 IP 地址*
3. 一次**客户端 IP 地址**添加到允许的 IP 地址列表中，单击**保存**以确认所做的更改。

    ![确认做的更改](build-restful-apis-with-aspnet-web-api/_static/image50.png)

    *确认做的更改*

<a id="ApxCTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>任务 3-发布 ASP.NET MVC 4 应用程序使用 Web 部署

1. 返回到 ASP.NET MVC 4 解决方案。 在**解决方案资源管理器**，右键单击网站项目，然后选择**发布**。

    ![发布应用程序](build-restful-apis-with-aspnet-web-api/_static/image51.png "发布应用程序")

    *发布此网站*
2. 导入发布配置文件保存在第一个任务。

    ![导入发布配置文件](build-restful-apis-with-aspnet-web-api/_static/image52.png "导入发布配置文件")

    *导入发布配置文件*
3. 单击**验证连接**。 验证完成后单击**下一步**。

    > [!NOTE]
    > 请参阅验证连接按钮旁边将显示绿色的复选标记后，验证已完成。

    ![正在验证连接](build-restful-apis-with-aspnet-web-api/_static/image53.png "验证连接")

    *正在验证连接*
4. 在**设置**页上，在**数据库**部分中，单击你的数据库连接的文本框旁边的按钮 (即**DefaultConnection**)。

    ![Web 部署配置](build-restful-apis-with-aspnet-web-api/_static/image54.png "Web 部署配置")

    *Web 部署配置*
5. 配置数据库连接，如下所示：

    - 在**服务器名称**类型 SQL 数据库服务器 URL 使用*tcp:*前缀。
    - 在**用户名**键入您的服务器管理员登录名。
    - 在**密码**键入服务器管理员登录密码。
    - 键入新的数据库名称，例如： *MVC4SampleDB*。

    ![配置目标连接字符串](build-restful-apis-with-aspnet-web-api/_static/image55.png "配置目标连接字符串")

    *配置目标连接字符串*
6. 然后单击“确定” 。 当系统提示创建数据库单击**是**。

    ![创建数据库](build-restful-apis-with-aspnet-web-api/_static/image56.png "创建数据库字符串")

    *创建数据库*
7. 在默认连接文本框中显示了将用于连接到 Windows Azure 中的 SQL 数据库连接字符串。 然后，单击 **“下一步”**。

    ![连接字符串指向 SQL 数据库](build-restful-apis-with-aspnet-web-api/_static/image57.png "指向 SQL 数据库的连接字符串")

    *连接字符串指向 SQL 数据库*
8. 在**预览**页上，单击**发布**。

    ![Web 应用程序发布](build-restful-apis-with-aspnet-web-api/_static/image58.png "发布 web 应用程序")

    *发布 web 应用程序*
9. 完成发布过程后，默认浏览器将打开已发布的网站。

    ![应用程序发布到 Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "应用程序发布到 Windows Azure")

    *应用程序发布到 Azure*
