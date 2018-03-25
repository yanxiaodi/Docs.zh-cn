---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
title: "在 ASP.NET 4.5 中最新信息在 Web 窗体 |Microsoft 文档"
author: rick-anderson
description: "ASP.NET Web 窗体的新版本引入了大量改进集中于改进用户体验，处理数据时。 在以前版本的..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 0a1f88bd-97da-4ed1-86f1-605199dc75a4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
msc.type: authoredcontent
ms.openlocfilehash: 560f949f79be8ba4936e4a6f8ee8ee32ef15acbf
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="whats-new-in-web-forms-in-aspnet-45"></a>什么是在 ASP.NET 4.5 Web 窗体中的新增功能
====================
通过[Web 营地团队](https://twitter.com/webcamps)

> ASP.NET Web 窗体的新版本引入了大量改进集中于改进用户体验，处理数据时。
> 
> 在以前版本的 Web 窗体，当使用数据绑定来发出对象成员的值时，你可以使用数据绑定表达式 Bind() 或因为 eval （）。 在 ASP.NET 的新版本，你是能够声明控件要通过使用新的 ItemType 属性绑定到哪种类型的数据。 将此属性将会启用你要使用强类型的变量以获得 Visual Studio 开发体验，例如 IntelliSense、 成员导航和编译时检查的完整好处。
> 
> 与数据绑定控件中，你现在还可以指定用于选择、 更新、 删除和插入数据，你自己自定义方法简化页面控件和应用程序逻辑之间的交互。 此外，已向 ASP.NET，这意味着你可以将数据从页映射直接进入方法类型参数添加了模型绑定功能。
> 
> 验证用户输入还应 Web 窗体的最新版本，更容易。 你可以现在对中的验证属性与您模型的类进行批注**System.ComponentModel.DataAnnotations**命名空间和所有站点都控制的请求验证用户输入使用该信息。 Web 窗体中的客户端验证现在与 jQuery，提供清除器客户端代码和非介入式 JavaScript 功能集成。
> 
> 在请求验证区域中，已得到改进以使其更轻松地有选择地关闭请求验证你的应用程序的特定部分或读取失效的请求数据。
> 
> 某些已得到改进 Web 窗体到服务器控件，若要利用的 HTML5 的新功能：
> 
> - TextBox 控件 TextMode 属性已更新以支持新 HTML5 输入的类型，如电子邮件、 datetime、 和等等。
> - FileUpload 控制现在支持从支持此 HTML5 功能的浏览器的多个文件上载。
> - 验证程序控制现在支持验证 HTML5 输入的元素。
> - 具有现在表示 URL 的特性的新 HTML5 元素支持 runat =&quot;服务器&quot;。 因此，你愿意，可以使用 ASP.NET 约定 URL 路径中 ~ 运算符来表示应用程序根目录 (例如，&lt;视频 runat =&quot;服务器&quot;src =&quot;~/myVideo.wmv&quot; &gt; &lt;/视频&gt;)。
> - 已修复 UpdatePanel 控件以支持发布 HTML5 输入的字段。
> 
> 在正式 ASP.NET 门户中你可以在 ASP.NET WebForms 4.5 中找到的新功能的更多示例：[什么是 ASP.NET 4.5 和 Visual Studio 2012 中的新增功能](../../../../whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012.md#_Toc318097385)
> 
> 在 Web 营地培训工具包中，在包括所有的示例代码和代码段[https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409)。


<a id="Objectives"></a>
### <a name="objectives"></a>目标

在此动手实验中，你将了解如何：

- 使用强类型化数据绑定表达式
- 使用 Web 窗体中的新模型绑定功能
- 用于值在提供程序将页面数据映射到代码隐藏方法
- 对于用户输入验证中使用数据注释
- 在 Web 窗体中执行 jQuery unobstrusive 客户端验证的 advange
- 实现精细请求验证
- 实现异步处理在 Web 窗体中的页

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>系统必备

你必须具有要完成本实验的以下项：

- [Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)或更高 (读取[附录 A](#AppendixA)有关如何安装它的说明)。

<a id="Setup"></a>
### <a name="setup"></a>安装

**安装代码片段**

为方便起见，你将沿此实验室管理大部分都是代码的可用作 Visual Studio 代码段。 若要安装运行的代码段**.\Source\Setup\CodeSnippets.vsi**文件。

如果你不熟悉 Visual Studio 代码段，并想要了解如何使用它们，你可以从该文档引用的附录&quot;[附录 c： 使用代码段](#AppendixC)&quot;。

<a id="Exercises"></a>
## <a name="exercises"></a>练习

本动手实验包括以下练习：

1. [练习 1： 模型在 ASP.NET Web 窗体中的绑定](#Exercise1)
2. [练习 2： 数据验证](#Exercise2)
3. [练习 3： 异步页处理在 ASP.NET Web 窗体](#Exercise3)

> [!NOTE]
> 每个练习均附带由**结束**包含生成您应该完成练习后获得的解决方案文件夹。 如果你需要通过在练习工作的更多帮助，可以使用此解决方案作为指南。


估计时间来完成该实验： **60 分钟**。

<a id="Exercise1"></a>

<a id="Exercise_1_Model_Binding_in_ASPNET_Web_Forms"></a>
### <a name="exercise-1-model-binding-in-aspnet-web-forms"></a>练习 1： 模型在 ASP.NET Web 窗体中的绑定

ASP.NET Web 窗体的新版本引入了一些侧重于处理数据时可以改进体验的增强功能。 在本练习中，你将了解有关强类型化数据控件和模型绑定。

<a id="Task_1_-_Using_Strongly-Typed_Data-Bindings"></a>
#### <a name="task-1---using-strongly-typed-data-bindings"></a>任务 1-使用强类型的数据绑定

在此任务中，你会发现新强类型的绑定 ASP.NET 4.5 中提供。

1. 打开**开始**解决方案位于**源/Ex1-ModelBinding/开始/**文件夹。

    1. 你将需要下载一些缺少的 NuGet 程序包才能继续。 若要执行此操作，请单击**项目**菜单，然后选择**管理 NuGet 包**。
    2. 在**管理 NuGet 包**对话框中，单击**还原**以便下载缺少的程序包。
    3. 最后，通过单击生成解决方案**生成** | **生成解决方案**。

    > [!NOTE]
    > 使用 NuGet 的优点之一是，你无需提供你的项目中的所有库减小项目大小。 使用 NuGet 增强工具，请通过指定的包版本在 Packages.config 文件中，你将能够下载首次运行该项目的所有所需的库。 这是你将需要从本实验打开现有的解决方案后运行这些步骤的原因。
2. 打开**Customers.aspx**页。 将未编号的列表放在主控件，并包括用于列出每个客户内的重复器控件。 将转发器名称设置为**customersRepeater**中下面的代码所示。

    在以前版本的 Web 窗体，你使用数据绑定发出对某个对象成员的值时是数据绑定，你将使用数据绑定表达式，以及调用 Eval 方法，传递成员的名称作为字符串。

    在运行时，这些对 Eval 将使用与在当前绑定对象的反射来读取的具有给定名称的成员的值和调用 html 格式显示结果。 这种方法可以非常轻松，以针对任意、 unshaped 数据进行数据绑定。

    遗憾的是，则会丢失的许多优秀的开发时体验功能在 Visual Studio 中，包括 IntelliSense 为成员名称、 支持导航 （例如转到定义），以及编译时检查。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample1.aspx)]
3. 打开**Customers.aspx.cs**文件。
4. 添加以下 using 语句。

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample2.cs)]
5. 在**页\_负载**方法，添加代码以填充转发器与客户的列表。

    (代码段- *Web 窗体实验-Ex01-绑定客户数据源*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample3.cs)]

    解决方案使用以及 CodeFirst EntityFramework 来创建和访问数据库。 在下面的代码中，customersRepeater 将绑定到从数据库中返回所有客户具体化的查询。
6. 按**F5**以运行该解决方案，转到**客户**页面以查看在操作中的转发器。 解决方案是使用 CodeFirst，将创建此数据库，并填充本地 SQL Express 实例中运行应用程序时。

    ![列出与中继器客户](whats-new-in-web-forms-in-aspnet-45/_static/image1.png "列出与中继器客户")

    *列出与中继器客户*

    > [!NOTE]
    > 在 Visual Studio 2012 中，IIS Express 是默认的 Web 开发服务器。
7. 关闭浏览器并返回到 Visual Studio。
8. 现在将要使用强类型的绑定的实现。 打开**Customers.aspx**页上，并使用新**ItemType**中转发器设置属性**客户**类型作为绑定类型。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample4.aspx)]

    ItemType 属性使您可以声明哪种类型的数据控件是要绑定到和允许你使用强类型绑定内的数据绑定控件。
9. 将内容替换为以下代码 ItemTemplate。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample5.aspx)]

    与上述方法的一个缺点是因为 eval （） 和 Bind() 的调用是后期绑定-这意味着传递字符串来表示的属性名称。 这意味着你不以成员名称、 支持的代码导航 （例如转到定义），也不是编译时检查支持会得到 Intellisense。

    设置 ItemType 属性将导致两个新的类型化的变量以生成数据绑定表达式的作用域中：**项**和**BindItem**。 你可以在数据绑定表达式中使用这些强类型的变量，并获取 Visual Studio 开发体验的全部好处。

    &quot; **:** &quot;用在表达式中将自动进行 HTML 编码的输出，以避免出现安全问题 （例如，跨站点脚本攻击）。 这一表示法针对的是可用自.NET 4 起响应编写，但现在也可以在数据绑定表达式中。

    > [!NOTE]
    > 项成员适用于单向绑定。 如果你想要执行双向绑定使用**BindItem**成员。

    ![在强类型的绑定中的 IntelliSense 支持](whats-new-in-web-forms-in-aspnet-45/_static/image2.png "强类型的绑定中的 IntelliSense 支持")

    *在强类型的绑定中的 IntelliSense 支持*
10. 按**F5**若要运行解决方案并转到客户页，以确保按预期方式工作所做的更改。

    ![列出客户详细信息](whats-new-in-web-forms-in-aspnet-45/_static/image3.png "列出客户详细信息")

    *列出客户详细信息*
11. 关闭浏览器并返回到 Visual Studio。

<a id="Task_2_-_Introducing_Model_Binding_in_Web_Forms"></a>
#### <a name="task-2---introducing-model-binding-in-web-forms"></a>任务 2-绑定在 Web 窗体中的模型简介

在以前版本的 ASP.NET Web 窗体中，当你想要执行双向数据绑定，同时检索和更新数据，你需要使用数据源对象。 这可能对象数据源、 SQL 数据源、 LINQ 数据源，依此类推。 但是如果你的方案需要处理数据的自定义代码，您需要使用对象数据源中，这使一些缺点。 例如，需要来避免复杂类型，你需要处理的异常时执行验证逻辑。

在新版本的 ASP.NET Web 窗体数据绑定控件支持模型绑定。 这意味着，你可以指定选择、 更新、 插入和删除直接在要从你的代码隐藏文件或从另一个类调用逻辑的数据绑定控件中的方法。

若要了解有关此，你将使用一个 GridView 列出使用新的产品类别**SelectMethod**属性。 此属性，可指定进行检索的 GridView 数据的方法。

1. 打开**Products.aspx**页上，并且包括**GridView**。 配置 GridView，如下所示使用强类型的绑定，并使排序和分页。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample6.aspx)]
2. 使用新**SelectMethod**属性配置 GridView 调用**GetCategories**方法来选择的数据。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample7.aspx)]
3. 打开**Products.aspx.cs**代码隐藏文件并添加以下 using 语句。

    (代码段- *Web 窗体实验-Ex01-命名空间*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample8.cs)]
4. 添加中的私有成员**产品**类，将分配的新实例**ProductsContext**。 此属性将存储使你能够连接到数据库的实体框架数据上下文。

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample9.cs)]
5. 创建**GetCategories**方法来检索使用 LINQ 的类别的列表。 该查询将包含**产品**属性，以便 GridView 可以显示的每个类别的产品量。 请注意，该方法返回一个表示要查询的原始 IQueryable 对象更高版本上执行的页生命周期。

    (代码段- *Web 窗体实验-Ex01-GetCategories*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample10.cs)]

    > [!NOTE]
    > 在以前版本的 ASP.NET Web 窗体，启用排序和分页使用自己存储库的逻辑在对象数据源上下文中，需要编写你自己的自定义代码和接收所需的所有必需参数。 现在，数据绑定方法可返回 IQueryable 以及这表示查询仍要执行，ASP.NET 可以满足的修改的查询，将添加正确的排序和分页参数。
6. 按**F5**能够开始调试站点并转到产品页。 你应看到 GridView 中填充了 GetCategories 方法返回的类别。

    ![填充使用模型绑定的 GridView](whats-new-in-web-forms-in-aspnet-45/_static/image4.png "填充使用模型绑定的 GridView")

    *填充使用模型绑定的 GridView*
7. 按**SHIFT**+**F5**停止调试。

<a id="Task_3_-_Value_Providers_in_Model_Binding"></a>
#### <a name="task-3---value-providers-in-model-binding"></a>任务 3-在模型绑定中的值提供程序

模型绑定不仅可用于指定自定义方法以处理直接在数据绑定控件中，数据还允许你将从页中的数据映射到从这些方法的参数。 方法参数中，你可以使用值提供程序属性以指定的值的数据源。 例如:

- 在页面上的控件
- 查询字符串值
- 查看数据
- 会话状态
- Cookie
- 已发布的窗体数据
- 视图状态
- 也支持自定义值提供程序

如果你已使用 ASP.NET MVC 4，你将注意到的模型绑定支持类似。 实际上，这些功能已来自 ASP.NET MVC 和移动到**System.Web**程序集能够以及在 Web 窗体上使用它们。

在此任务中，你将更新 GridView 要作为筛选依据的每个类别的产品量的其结果与模型绑定接收的筛选器参数。

1. 返回到**Products.aspx**页。
2. 在 GridView 的顶部，添加**标签**和**ComboBox**选择的每个类别的产品数目，如下所示。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample11.aspx)]
3. 添加**要**到 GridView 时没有具有所选的产品类别显示一条消息。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample12.aspx)]
4. 打开**Products.aspx.cs**隐藏代码并添加以下 using 语句。

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample13.cs)]
5. 修改**GetCategories**方法来接收一个整数**minProductsCount**参数和筛选返回的结果。 若要执行此操作，请将方法替换为下面的代码。

    (代码段- *Web 窗体实验-Ex01-GetCategories 2*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample14.cs)]

    新**[控件]**属性**minProductsCount**自变量将告知 ASP.NET 必须使用的页中的控件填充其值。 ASP.NET 将查找匹配自变量 (minProductsCount) 的名称的任何控件，并执行必要的映射和转换要用控件值填充参数。

    或者，该属性提供了可用于指定从何处可以获取的值的控件的重载构造函数。

    > [!NOTE]
    > 数据绑定功能的一个目标是代码的减少需要为页面交互编写量。 除了 [控件] 值提供程序，你可以在方法参数中使用其他模型绑定提供程序。 其中一些任务简介中列出。
6. 按**F5**能够开始调试站点并转到产品页。 在下拉列表中选择的产品数目，并注意如何 GridView 现已更新。

    ![筛选与下拉列表值 GridView](whats-new-in-web-forms-in-aspnet-45/_static/image5.png "筛选使用下拉列表值 GridView")

    *筛选与下拉列表值 GridView*
7. 停止调试。

<a id="Task_4_-_Using_Model_Binding_for_Filtering"></a>
#### <a name="task-4---using-model-binding-for-filtering"></a>任务 4-使用模型绑定进行筛选

在此任务中，你将添加第二个子 GridView 以显示所选类别中的产品。

1. 打开**Products.aspx**页上，并更新类别 GridView 自动生成的选择按钮。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample15.aspx)]
2. 添加第二个**GridView**名为**productsGrid**底部。 设置**ItemType**到**WebFormsLab.Model.Product**、**将字段名**到**ProductId**和**SelectMethod**到**GetProducts**。 设置**AutoGenerateColumns**到**false**并为 ProductId、 产品名称、 描述和单价添加列。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample16.aspx)]
3. 打开**Products.aspx.cs**代码隐藏文件。 实现**GetProducts**方法以从类别 GridView 接收类别 ID 和筛选的产品。 模型绑定将使用中的选定的行的参数值设置**categoriesGrid**。 由于不匹配的参数名称和控件名称，你应在控件的值提供程序指定控件的名称。

    (代码段- *Web 窗体实验-Ex01-GetProducts*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample17.cs)]

    > [!NOTE]
    > 此方法更加简单到单元测试这些方法。 单元测试上下文，其中 Web 窗体不在执行 [控件] 属性不会执行任何特定操作。
4. 打开**Products.aspx**页上，找到的产品 GridView。 更新的产品以显示一个链接，以便编辑所选的产品的 GridView。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample18.aspx)]
5. 打开**ProductDetails.aspx**页上的代码隐藏和替换**SelectProduct**方法替换为以下代码。

    (代码段- *Web 窗体实验-Ex01-SelectProduct 方法*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample19.cs)]

    > [!NOTE]
    > 请注意， **[QueryString]**属性用于填充与查询字符串中的产品 id 参数的方法参数。
6. 按**F5**能够开始调试站点并转到产品页。 从类别 GridView 中选择任何类别，请注意，更新的产品 GridView。

    ![显示从所选类别的产品](whats-new-in-web-forms-in-aspnet-45/_static/image6.png "显示从所选类别的产品")

    *显示从所选类别的产品*
7. 单击**视图**产品以打开 ProductDetails.aspx 页上的链接。

    请注意页面与使用查询字符串中的 productId 参数 SelectMethod 检索产品。

    ![查看产品详细信息](whats-new-in-web-forms-in-aspnet-45/_static/image7.png "查看产品详细信息")

    *查看产品详细信息*

    > [!NOTE]
    > 将在下一个练习中实现能够键入 HTML 描述。

<a id="Task_5_-_Using_Model_Binding_for_Update_Operations"></a>
#### <a name="task-5---using-model-binding-for-update-operations"></a>任务 5-更新操作的绑定的使用模型

在上一任务中，选择数据的主要使用模型绑定，在此任务中，您将学习如何在更新操作中使用模型绑定。

将更新的类别 GridView，以使用户能够更新类别。

1. 打开**Products.aspx**页上，并更新类别以自动生成的编辑按钮并使用新的 GridView **UpdateMethod**特性来指定**UpdateCategory**方法来更新选定的项。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample20.aspx)]

    GridView 中的将字段名属性定义哪些是唯一标识模型绑定对象的成员，因此，哪些对象是更新方法至少应接收的参数。
2. 打开**Products.aspx.cs**代码隐藏文件和实现**UpdateCategory**方法。 该方法应收到要加载当前的类别、 填充 GridView 中的值，然后更新类别的类别 ID。

    (代码段- *Web 窗体实验-Ex01-UpdateCategory*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample21.cs)]

    新**TryUpdateModel**页类中的方法负责填充模型对象使用的页中的控件中的值。 在这种情况下，它将替换从当前正在编辑到的 GridView 行更新后的值**类别**对象。

    > [!NOTE]
    > 下一步练习将说明用于验证用户在编辑对象时输入的数据 ModelState.IsValid 的使用情况。
3. 运行该站点，并转到产品页。 编辑类别。 键入新名称，然后单击**更新**以保留更改。

    ![编辑类别](whats-new-in-web-forms-in-aspnet-45/_static/image8.png "编辑类别")

    *编辑类别*

<a id="Exercise2"></a>

<a id="Exercise_2_Data_Validation"></a>
### <a name="exercise-2-data-validation"></a>练习 2： 数据验证

在此练习中，你将了解在 ASP.NET 4.5 的新数据验证功能。 您将签出在 Web 窗体中的新非介入式验证功能。 你将在应用程序模型类中使用数据注释，对于用户输入验证，并最后，你将学习如何打开或关闭为单个控件在页中的请求验证。

<a id="Task_1_-_Unobtrusive_Validation"></a>
#### <a name="task-1---unobtrusive-validation"></a>任务 1-非介入式验证

窗体的复杂数据包括验证程序往往会在页中，这样就能表示大约 60%的代码生成太多 JavaScript 代码。 启用非介入式验证，与你的 HTML 代码将查找更简洁且整洁。

在此部分中，你将启用 ASP.NET 要比较这两种配置生成的 HTML 代码中的非介入式验证。

1. 打开**Visual Studio 2012**并打开**开始**解决方案位于**Source\Ex2 Validation\Begin**这个实验室的文件夹。 或者，您可以对现有解决方案从上一练习中继续工作。

    1. 如果你打开提供**开始**解决方案，你将需要下载一些缺少的 NuGet 程序包才能继续。 为此，请在解决方案资源管理器中，单击**WebFormsLab**项目**管理 NuGet 包**。
    2. 在**管理 NuGet 包**对话框中，单击**还原**以便下载缺少的程序包。
    3. 最后，通过单击生成解决方案**生成** | **生成解决方案**。

    > [!NOTE]
    > 使用 NuGet 的优点之一是，你无需提供你的项目中的所有库减小项目大小。 使用 NuGet 增强工具，请通过指定的包版本在 Packages.config 文件中，你将能够下载首次运行该项目的所有所需的库。 这是你将需要从本实验打开现有的解决方案后运行这些步骤的原因。
2. 按**F5**启动 web 应用程序。 请转到客户页上，单击**添加新客户**链接。
3. 在浏览器页上，右键单击并选择**查看源**选项来打开应用程序生成的 HTML 代码。

    ![显示页的 HTML 代码](whats-new-in-web-forms-in-aspnet-45/_static/image9.png "显示页的 HTML 代码")

    *显示页的 HTML 代码*
4. 滚动查看页的源代码，请注意，ASP.NET 已插入的 JavaScript 代码和数据验证程序在页中的可以执行验证并显示错误列表。

    ![验证 CustomerDetails 页中的 JavaScript 代码](whats-new-in-web-forms-in-aspnet-45/_static/image10.png "CustomerDetails 页中的验证 JavaScript 代码")

    *验证 CustomerDetails 页中的 JavaScript 代码*
5. 关闭浏览器并返回到 Visual Studio。
6. 现在，你将启用非介入式验证。 打开**Web.Config**并找到**ValidationSettings:UnobtrusiveValidationMode**中的键**AppSettings**部分**。** 将密钥值设置为**WebForms**。

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample22.xml)]

    > [!NOTE]
    > 你也可以在设置此属性&quot;**页\_负载**&quot;在你想要仅对某些页面中启用非介入式验证的情况下的事件。
7. 打开**CustomerDetails.aspx**按**F5**启动 Web 应用程序。
8. 按 F12 键以打开 IE 开发人员工具。 打开开发人员工具后，选择脚本选项卡。选择**CustomerDetails.aspx**从菜单并采取脚本在页面上运行 jQuery 时所需的注意已加载到浏览器中从本地站点。

    ![加载 jQuery JavaScript 文件直接从本地 IIS 服务器](whats-new-in-web-forms-in-aspnet-45/_static/image11.png "加载 jQuery JavaScript 文件直接从本地 IIS 服务器")

    *直接从本地 IIS 服务器加载 jQuery JavaScript 文件*
9. 关闭浏览器以返回到 Visual Studio。 打开**Site.Master**再次文件并找到**ScriptManager**。 将属性添加**EnableCdn**属性的值**True**。 这将强制 jQuery 要从联机的 URL，而不在本地站点的 URL 加载。
10. 打开**CustomerDetails.aspx** Visual Studio 中。 按 F5 键以运行该站点。 Internet Explorer 打开后，按 F12 键以打开开发人员工具。 选择**脚本**选项卡，然后再看一看下拉列表。 请注意从本地站点，但而是从联机 jQuery CDN 不再正在加载的 jQuery JavaScript 文件。

    ![加载 jQuery JavaScript 文件从 CDN](whats-new-in-web-forms-in-aspnet-45/_static/image12.png "加载 jQuery JavaScript 文件从 CDN")

    *从 CDN 中加载的 jQuery JavaScript 文件*
11. 打开 HTML 页的源代码再次使用浏览器中的查看源选项。 请注意，通过启用非介入式验证 ASP.NET 具有替换插入的 JavaScript 代码以数据-\*属性。

    ![非介入式验证代码](whats-new-in-web-forms-in-aspnet-45/_static/image13.png "非介入式验证代码")

    *非介入式验证代码*

    > [!NOTE]
    > 在此示例中，您了解如何验证摘要数据批注已简化为几 HTML 和 JavaScript 行。 以前，而不进行非介入式验证，你添加更多验证控件越大 JavaScript 验证代码将会增长。

<a id="Task_2_-_Validating_the_Model_with_Data_Annotations"></a>
#### <a name="task-2---validating-the-model-with-data-annotations"></a>任务 2-验证与数据注释模型

ASP.NET 4.5 引入了 Web 窗体的数据批注验证。 而不是让每个输入中的验证控件，现在可以在你模型类中定义约束和跨所有 web 应用程序中使用它们。 在本部分中，你将了解如何验证新建/编辑客户窗体中使用数据注释。

1. 打开**CustomerDetail.aspx**页。 请注意，客户的名字和中的第二个名称**EditItemTemplate**和**InsertItemTemplate**部分使用 RequiredFieldValidator 控件进行验证。 每个验证程序将关联到特定的条件，，因此你需要作为条件，以检查包括尽可能多的验证程序。
2. 添加数据批注，以验证 Customer 模型类。 打开**Customer.cs**类**模型**文件夹和*修饰*使用数据 annotation 特性每个属性。

    (代码段- *Web 窗体实验-Ex02-数据注释*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample23.cs)]

    > [!NOTE]
    > .NET framework 4.5 扩展现有的数据批注集合。 以下是一些你可以使用数据注释: [CreditCard] [Phone] [EmailAddress] [区域] [比较]，[Url] [FileExtensions]，[Required]、[密钥]，[正则表达式]。
    > 
    > 某些用法示例：
    > 
    > [密钥]: Specifies that an attribute is the unique identifier
    > 
    > [Range(0.4, 0.5, ErrorMessage=&quot;{Write an error message}&quot;]: Double range
    > 
    > [EmailAddress(ErrorMessage=&quot;Invalid Email&quot;), MaxLength(56)]: Two annotations in the same line.
    > 
    > 你还可以定义自己的每个属性内的错误消息。
3. 打开**CustomerDetails.aspx**和 FormView 控件在 EditItemTemplate InsertItemTemplate 部分中删除第一个和最后一个名称字段的所有 RequiredFieldvalidators。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample24.aspx)]

    > [!NOTE]
    > 使用数据注释的一个优点是你的应用程序页中不重复验证逻辑。 你在模型中，定义一次，并使用它跨所有操作数据的应用程序页。
4. 打开**CustomerDetails.aspx**代码隐藏和找到 SaveCustomer 方法。 此方法调用时插入一个新的客户，并接收来自 FormView 控制值的客户参数。 页面控件和参数对象发生之间的映射，ASP.NET 将执行时对所有数据注释模型验证属性和 ModelState 字典填充遇到的错误，如果有的话。

    ModelState.IsValid 将只返回 true，如果执行验证后，模型上的所有字段都是有效。

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample25.cs)]
5. 添加**ValidationSummary** CustomerDetails 页后，可以显示的模型错误列表末尾的控件。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample26.aspx)]

    **ShowModelStateErrors**是 ValidationSummary 的新属性控制，在设置为**true**，控件将显示从 ModelState 字典错误。 这些错误来自于数据批注验证。
6. 按**F5**运行 Web 应用程序。 完成表单的某些错误值，然后单击**保存**执行验证。 请注意错误摘要底部。

    ![使用数据注释验证](whats-new-in-web-forms-in-aspnet-45/_static/image14.png "使用数据注释验证")

    *使用数据注释验证*

<a id="Task_3_-_Handling_Custom_Database_Errors_with_ModelState"></a>
#### <a name="task-3---handling-custom-database-errors-with-modelstate"></a>任务 3-ModelState 处理自定义数据库错误

在以前版本的 Web 窗体，处理数据库错误，例如太长的字符串或唯一键冲突可能涉及在存储库代码中引发异常，然后处理上你的代码隐藏以显示错误的异常。 非常高的代码需要做些什么相对简单。

在 Web 窗体 4.5 ModelState 对象可以用于以一致的方式在页上，从你的模型或从数据库中，显示的错误。

在此任务中，你将添加代码以正确处理数据库异常和使用 ModelState 对象向用户显示相应的消息。

1. 虽然应用程序仍在运行，尝试更新使用一个重复的值的类别的名称。

    ![具有重复名称更新类别](whats-new-in-web-forms-in-aspnet-45/_static/image15.png "具有重复名称更新类别")

    *具有重复名称更新类别*

    请注意，由于引发异常&quot;唯一&quot;约束**CategoryName**列。

    ![重复的类别名称的异常](whats-new-in-web-forms-in-aspnet-45/_static/image16.png "重复的类别名称的异常")

    *重复的类别名称的异常*
2. 停止调试。 在**Products.aspx.cs**代码隐藏文件、 更新**UpdateCategory**方法以处理数据库所引发的异常。Savechanges （） 方法调用，添加一个错误**ModelState**对象。

    新**TryUpdateModel**方法更新使用所提供的用户的窗体数据从数据库检索到的类别对象。

    (代码段- *Web 窗体实验-Ex02-UpdateCategory 句柄错误*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample27.cs)]

    > [!NOTE]
    > 理想情况下，你需要确定 DbUpdateException 的原因并检查的根本原因是否违反了唯一键约束。
3. 打开**Products.aspx**并添加**ValidationSummary**类别 GridView 以显示模型错误的列表下面的控件。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample28.aspx)]
4. 运行该站点，并转到产品页。 尝试更新使用重复的值的类别的名称。

    请注意，处理了该异常和错误消息将出现在**ValidationSummary**控件。

    ![重复类别错误](whats-new-in-web-forms-in-aspnet-45/_static/image17.png "重复类别错误")

    *重复的类别错误*

<a id="Task_4_-_Request_Validation_in_ASPNET_Web_Forms_45"></a>
#### <a name="task-4---request-validation-in-aspnet-web-forms-45"></a>任务 4-请求在 ASP.NET Web 窗体 4.5 中的验证

在 ASP.NET 中的请求验证功能可提供特定级别的针对跨站点脚本 (XSS) 攻击的默认保护。 在以前版本的 ASP.NET，请求验证已按默认启用，仅可以为整个页面已禁用。 使用新版本的 ASP.NET Web 窗体可以现在禁用单个控件的请求验证、 执行延迟请求验证或访问 （但请注意如果这样做 ！） 的未经过验证的请求数据。

1. 按**Ctrl + F5**以启动网站，而不进行调试，然后转到产品页。 选择一个类别，然后单击**编辑**上产品中的任何链接。
2. 键入包含具有潜在危险的内容、 对实例包括 HTML 标记的描述。 注意到由于请求验证引发的异常。

    ![编辑具有潜在危险的内容的产品](whats-new-in-web-forms-in-aspnet-45/_static/image18.png "编辑具有潜在危险的内容的产品")

    *编辑具有潜在危险的内容的产品*

    ![因请求验证而引发的异常](whats-new-in-web-forms-in-aspnet-45/_static/image19.png "因请求验证而引发的异常")

    *因请求验证而引发的异常*
3. 关闭页，然后在 Visual Studio 中，按**SHIFT + F5**来停止调试。
4. 打开**ProductDetails.aspx**页然后找到**说明**文本框。
5. 添加新**ValidateRequestMode**到文本框的属性并将其值设置为**禁用**。

    新**ValidateRequestMode**特性，您可以禁用每个控件梯度的请求验证。 当你想要使用的输入可能收到 HTML 代码，但想要保留用于网页的其余内容的验证时，这非常有用。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample29.aspx)]
6. 按**F5**运行 web 应用程序。 再次打开编辑产品页并完成包括 HTML 标记的产品说明。 请注意，你可以现在 HTML 内容添加到说明。

    ![请求验证有关产品说明禁用](whats-new-in-web-forms-in-aspnet-45/_static/image20.png "请求产品说明为禁用的验证")

    *请求产品说明为禁用的验证*

    > [!NOTE]
    > 应在生产应用程序中，整理要确保输入仅安全的 HTML 标记的用户输入的 HTML 代码 (例如，有没有&lt;脚本&gt;标记)。 若要执行此操作，可以使用[Microsoft Web 保护库](https://www.nuget.org/packages/AntiXSS)。
7. 再次编辑产品。 在名称字段中键入 HTML 代码，然后单击**保存**。 请注意，说明字段中仅禁用请求验证，并且重新字段的其余部分仍验证针对潜在危险的内容。

    ![请求验证在其余字段中启用](whats-new-in-web-forms-in-aspnet-45/_static/image21.png "请求在其余字段中启用的验证")

    *在其余字段中启用请求验证*

    ASP.NET Web 窗体 4.5 包括新的请求验证模式，延迟执行请求验证。 使用请求验证模式设置为**4.5**，如果一种代码访问*Request.Form [&quot;密钥&quot;]*，ASP.NET 4.5 请求验证将仅触发请求验证该窗体集合中的特定元素。

    此外，ASP.NET 4.5 现在包括从 Microsoft 反 XSS 库 v4.0 的核心编码例程。 编码例程实现新的反 XSS *AntiXssEncoder*类型位于新**System.Web.Security.AntiXss**命名空间。 与**encoderType**参数配置为使用*AntiXssEncoder*，所有输出自动编码在 ASP.NET 内使用的新的编码例程。
8. ASP.NET 4.5 请求验证还支持对请求数据的未经确认的访问。 ASP.NET 4.5 中添加到新集合属性**HttpRequest**调用对象**Unvalidated**。 导航到**HttpRequest.Unvalidated**有权访问所有请求数据，包括窗体、 QueryStrings、 Cookie、 Url 和等等的公共部分。

    ![Request.Unvalidated 对象](whats-new-in-web-forms-in-aspnet-45/_static/image22.png "Request.Unvalidated 对象")

    *Request.Unvalidated 对象*

    > [!NOTE]
    > **请谨慎使用 HttpRequest.Unvalidated 属性 ！** 请确保仔细原始请求数据，以确保危险的文本是往返，并且不返回到信任客户呈现上执行自定义验证 ！

<a id="Exercise3"></a>

<a id="Exercise_3_Asynchronous_Page_Processing_in_ASPNET_Web_Forms"></a>
### <a name="exercise-3-asynchronous-page-processing-in-aspnet-web-forms"></a>练习 3： 异步页处理在 ASP.NET Web 窗体

在此练习中，将向你介绍处理 ASP.NET Web 窗体中的功能的新异步页。

<a id="Task_1_-_Updating_the_Product_Details_Page_to_Upload_and_Show_Images"></a>
#### <a name="task-1---updating-the-product-details-page-to-upload-and-show-images"></a>任务 1-更新产品详细信息页面上载并显示图像

在此任务中，你将更新产品详细信息页后，可以允许用户指定产品的图像 URL 并将其显示在只读视图。 将通过同步下载创建指定的映像的本地副本。 在下一步的任务中，你将更新此实现，使其以异步方式工作。

1. 打开**Visual Studio 2012**和加载**开始**解决方案位于**Source\Ex3 Async\Begin**从本实验的文件夹。 或者，您可以对从前面的练习现有解决方案中继续工作。

    1. 如果你打开提供**开始**解决方案，你将需要下载一些缺少的 NuGet 程序包才能继续。 为此，请在解决方案资源管理器中，单击**WebFormsLab**项目，然后选择**管理 NuGet 包**。
    2. 在**管理 NuGet 包**对话框中，单击**还原**以便下载缺少的程序包。
    3. 最后，通过单击生成解决方案**生成** | **生成解决方案**。

    > [!NOTE]
    > 使用 NuGet 的优点之一是，你无需提供你的项目中的所有库减小项目大小。 使用 NuGet 增强工具，请通过指定的包版本在 Packages.config 文件中，你将能够下载首次运行该项目的所有所需的库。 这是你将需要从本实验打开现有的解决方案后运行这些步骤的原因。
2. 打开**ProductDetails.aspx**页上的源和 FormView ItemTemplate，以显示产品映像中添加字段。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample30.aspx)]
3. 添加字段以在 FormView EditTemplate 中指定图像 URL。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample31.aspx)]
4. 打开**ProductDetails.aspx.cs**代码隐藏文件，并添加以下命名空间指令。

    (代码段- *Web 窗体实验室-Ex03-命名空间*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample32.cs)]
5. 创建**UpdateProductImage**方法在本地中存储远程图像**映像**文件夹和更新产品实体与新的映像位置值。

    (代码段- *Web 窗体实验室-Ex03-UpdateProductImage*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample33.cs)]
6. 更新**UpdateProduct**方法来调用**UpdateProductImage**方法。

    (代码段- *Web 窗体实验室-Ex03-UpdateProductImage 调用*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample34.cs)]
7. 运行应用程序并重试上载产品的映像。 例如，可以使用下面的图像 URL，从 Office 剪贴画： [ [http://officeimg.vo.msecnd.net/images/MB900437099.jpg](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)

    ![设置产品的映像](whats-new-in-web-forms-in-aspnet-45/_static/image23.png "设置产品的映像")

    *设置产品的映像*

<a id="Task_2_-_Adding_Asynchronous_Processing_to_the_Product_Details_Page"></a>
#### <a name="task-2---adding-asynchronous-processing-to-the-product-details-page"></a>任务 2-添加异步处理于产品详细信息页面

在此任务中，你将更新产品详细信息页，以使其以异步方式工作。 将通过使用 ASP.NET 4.5 异步页处理来提高长时间运行任务-映像下载过程的。

Web 应用程序中的异步方法可以用于优化 ASP.NET 线程池的使用的方式。 在 ASP.NET 中存在是有限的数量的越过线程池中的线程请求，因此，在所有线程都非常忙，ASP.NET 开始拒绝新请求，发送应用程序错误消息并使你的站点不可用。

在网站上耗时的操作都适合进行异步编程，因为它们将长时间占用分配的线程。 这包括长时间运行的请求，具有许多不同的元素的页和需要脱机操作中，将此类查询数据库或访问的外部 web 服务器的页。 优点是处理页时，你会为这些操作，使用异步方法，如果线程是释放并返回到的线程池，以便用于处理新的页面请求。 这意味着，该页面才会开始在线程池中的一个线程中处理，并可能在异步处理完成后完成在一个不同的处理。

1. 打开**ProductDetails.aspx**页。 添加**异步**属性中**页**元素并将其设置为**true**。 此属性是告诉 ASP.NET 实现 IHttpAsyncHandler 接口。


    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample35.aspx)]
2. 在要显示的线程运行页面的详细信息的页的底部添加标签。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample36.aspx)]
3. 打开**ProductDetails.aspx.cs**并添加以下命名空间指令。

    (代码段- *Web 窗体实验室-Ex03-命名空间 2*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample37.cs)]
4. 修改**UpdateProductImage**方法与异步任务将映像下载。 将替换**WebClient** **后**方法替换**DownloadFileTaskAsync**方法和包括**await**关键字。

    (代码段- *Web 窗体实验室-Ex03-UpdateProductImage 异步*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample38.cs)]

    RegisterAsyncTask 注册新的页异步任务要在不同线程中执行。 它会收到要执行的任务 (t) 的 lambda 表达式。 **Await**中的关键字**DownloadFileTaskAsync**方法将转换后，将异步调用的回调方法的其余部分**DownloadFileTaskAsync**方法已完成。 ASP.NET 将通过自动维护所有原始 HTTP 请求值来继续执行该方法。 .NET 4.5 中的新异步编程模型，可编写异步代码，看起来非常相似的同步代码，并让编译器处理回调函数或延续代码的复杂性。
    > [!NOTE]
    > RegisterAsyncTask 和 PageAsyncTask 自.NET 2.0 以来已可供。 Await 关键字为新从.NET 4.5 的异步编程模型，并可与从.NET WebClient 对象的新 TaskAsync 方法结合使用。
5. 添加代码以显示的线程的代码在其启动和完成执行。 若要执行此操作，更新**UpdateProductImage**方法替换为以下代码。

    (代码段- *Web 窗体实验室-Ex03-显示线程*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample39.cs)]
6. 打开网站的**Web.config**文件。 添加以下 appSetting 变量。

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample40.xml)]
7. 按**F5**运行应用程序并上载该产品的映像。 请注意开始和完成的代码可能不同的线程 ID。 这是因为从 ASP.NET 线程池的单独线程上运行异步任务。 在任务完成后，ASP.NET 将任务放回队列中，并将分配任何可用线程。

    ![异步下载映像](whats-new-in-web-forms-in-aspnet-45/_static/image24.png "异步下载图像")

    *异步下载图像*

> [!NOTE]
> 此外，你可以部署此应用程序对 Azure 以下[附录 b： 发布 ASP.NET MVC 4 应用程序使用 Web Deploy](#AppendixB)。


* * *

<a id="Summary"></a>
## <a name="summary"></a>摘要

在此动手实验中，具有已发送并演示以下概念：

- 使用强类型化数据绑定表达式
- 使用 Web 窗体中的新模型绑定功能
- 用于值在提供程序将页面数据映射到代码隐藏方法
- 对于用户输入验证中使用数据注释
- 在 Web 窗体中执行 jQuery unobstrusive 客户端验证的 advange
- 实现精细请求验证
- 实现异步处理在 Web 窗体中的页

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>附录 a： 安装 Visual Studio Express 2012 for Web

你可以安装**Microsoft Visual Studio Express 2012 for Web**或另一个&quot;Express&quot;版本使用 **[Microsoft Web 平台安装程序](https://www.microsoft.com/web/downloads/platform.aspx)**. 以下说明将指导你完成安装所需的步骤*Visual studio Express 2012 for Web*使用*Microsoft Web 平台安装程序*。

1. 转到[ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)。 或者，如果你已安装 Web 平台安装程序，你可以打开它，并搜索产品&quot; *Visual Studio Express 2012 for Web 与 Azure SDK*&quot;。
2. 单击**立即安装**。 如果你没有**Web 平台安装程序**将重定向以下载并请先安装它。
3. 一次**Web 平台安装程序**处于打开状态，单击**安装**以启动安装程序。

    ![安装 Visual Studio Express](whats-new-in-web-forms-in-aspnet-45/_static/image25.png "安装 Visual Studio Express")

    *安装 Visual Studio Express*
4. 阅读所有产品的许可证和条款，然后单击**我接受**以继续。

    ![接受许可条款](whats-new-in-web-forms-in-aspnet-45/_static/image26.png)

    *接受许可条款*
5. 等待，直到下载和安装过程完成。

    ![安装进度](whats-new-in-web-forms-in-aspnet-45/_static/image27.png)

    *安装进度*
6. 当安装完成后时，单击**完成**。

    ![安装已完成](whats-new-in-web-forms-in-aspnet-45/_static/image28.png)

    *安装已完成*
7. 单击**退出**以关闭 Web 平台安装程序。
8. 若要打开 Visual Studio Express for Web，请转到**启动**屏幕并开始编写&quot; **VS Express**&quot;，然后单击**VS Express for Web**磁贴。

    ![Web 磁贴的 VS Express](whats-new-in-web-forms-in-aspnet-45/_static/image29.png)

    *Web 磁贴的 VS Express*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>附录 b： 发布 ASP.NET MVC 4 应用程序使用 Web 部署

本附录将演示如何从 Azure 门户创建新的网站和发布应用程序获取按照本实验中，利用 Azure 提供的 Web 部署发布功能。

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a>任务 1-从 Azure 门户创建新网站

1. 转到[Azure 管理门户](https://manage.windowsazure.com/)并使用与你的订阅关联的 Microsoft 凭据登录。

    > [!NOTE]
    > 使用 Azure 可以免费承载 10 个 ASP.NET 网站，然后随着流量增长而扩展。 你可以注册[此处](http://aka.ms/aspnet-hol-azure)。

    ![登录到 Windows Azure 门户](whats-new-in-web-forms-in-aspnet-45/_static/image30.png "登录到 Windows Azure 门户")

    *登录到门户*
2. 单击**新建**命令栏上。

    ![创建新网站](whats-new-in-web-forms-in-aspnet-45/_static/image31.png "创建新网站")

    *创建新网站*
3. 单击**计算** | **网站**。 然后选择**快速创建**选项。 为新网站提供可用的 URL，然后单击**创建网站**。

    > [!NOTE]
    > Azure 是运行在云中，你可以控制和管理 web 应用程序的宿主。 快速创建选项，可部署到从 Azure 门户外部的已完成的 web 应用程序。 它不包括用于设置数据库的步骤。

    ![创建新的网站使用快速创建](whats-new-in-web-forms-in-aspnet-45/_static/image32.png "创建新的网站使用快速创建")

    *创建新的网站使用快速创建*
4. 等到新**网站**创建。
5. 创建网站后单击下的链接**URL**列。 检查新的 Web 站点工作。

    ![浏览到新的 web 站点](whats-new-in-web-forms-in-aspnet-45/_static/image33.png "浏览到新的 web 站点")

    *浏览到新的 web 站点*

    ![运行网站](whats-new-in-web-forms-in-aspnet-45/_static/image34.png "运行的网站")

    *运行的网站*
6. 返回到门户并单击在网站的名称**名称**列以显示的管理页。

    ![打开网站管理页](whats-new-in-web-forms-in-aspnet-45/_static/image35.png "打开网站管理页")

    *打开网站管理页*
7. 在**仪表板**页上，在**速览**部分中，单击**下载发布配置文件**链接。

    > [!NOTE]
    > *发布配置文件*包含所有发布到 Azure 的 web 应用程序的每个启用的发布方法所需的信息。 发布配置文件包含的 Url、 用户凭据和连接到并针对每个发布方法启用的终结点进行身份验证所需的数据库字符串。 **Microsoft WebMatrix 2**， **Microsoft Visual Studio Express for Web**和**Microsoft Visual Studio 2012**支持读取发布配置文件以自动执行的这些程序，以便配置web 应用程序发布到 Azure。

    ![下载网站发布配置文件](whats-new-in-web-forms-in-aspnet-45/_static/image36.png "下载网站发布配置文件")

    *下载网站发布配置文件*
8. 到一个已知位置下载发布配置文件。 进一步在本练习中，你将了解如何使用此文件发布到 Azure web 应用程序从 Visual Studio。

    ![保存发布配置文件](whats-new-in-web-forms-in-aspnet-45/_static/image37.png "保存发布配置文件")

    *保存发布配置文件*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>任务 2-配置数据库服务器

如果你的应用程序将使用 SQL Server 数据库将需要创建 SQL 数据库服务器。 如果你想要部署的简单应用程序不使用 SQL Server 可能会跳过此任务。

1. 需要将用于存储应用程序数据库的 SQL 数据库服务器。 你可以从你的订阅在 Azure 管理门户中查看 SQL 数据库服务器**Sql 数据库** | **服务器** | **服务器的仪表板**. 如果你没有创建的服务器，则可以创建一个使用**添加**命令栏上的按钮。 请记下的**服务器名称和 URL、 管理员登录名和密码**，如你将在接下来的任务中使用它们。 不创建数据库，它将在后面的阶段中创建。

    ![SQL 数据库服务器仪表板](whats-new-in-web-forms-in-aspnet-45/_static/image38.png "SQL Database 服务器仪表板")

    *SQL 数据库服务器仪表板*
2. 在下一个任务将测试从 Visual Studio 中，数据库连接，因此你需要在的服务器的列表中包括你的本地 IP 地址**允许的 IP 地址**。 若要做到这一点，请单击**配置**，选择的 IP 地址从**当前客户端 IP 地址**并将其粘贴在**起始 IP 地址**和**结束 IP 地址**文本框和单击![add-client-ip-address-ok-button](whats-new-in-web-forms-in-aspnet-45/_static/image39.png)按钮。

    ![添加客户端 IP 地址](whats-new-in-web-forms-in-aspnet-45/_static/image40.png)

    *添加客户端 IP 地址*
3. 一次**客户端 IP 地址**添加到允许的 IP 地址列表中，单击**保存**以确认所做的更改。

    ![确认做的更改](whats-new-in-web-forms-in-aspnet-45/_static/image41.png)

    *确认做的更改*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>任务 3-发布 ASP.NET MVC 4 应用程序使用 Web 部署

1. 返回到 ASP.NET MVC 4 解决方案。 在**解决方案资源管理器**，右键单击网站项目，然后选择**发布**。

    ![发布应用程序](whats-new-in-web-forms-in-aspnet-45/_static/image42.png "发布应用程序")

    *发布此网站*
2. 导入发布配置文件保存在第一个任务。

    ![导入发布配置文件](whats-new-in-web-forms-in-aspnet-45/_static/image43.png "导入发布配置文件")

    *导入发布配置文件*
3. 单击**验证连接**。 验证完成后单击**下一步**。

    > [!NOTE]
    > 请参阅验证连接按钮旁边将显示绿色的复选标记后，验证已完成。

    ![正在验证连接](whats-new-in-web-forms-in-aspnet-45/_static/image44.png "验证连接")

    *正在验证连接*
4. 在**设置**页上，在**数据库**部分中，单击你的数据库连接的文本框旁边的按钮 (即**DefaultConnection**)。

    ![Web 部署配置](whats-new-in-web-forms-in-aspnet-45/_static/image45.png "Web 部署配置")

    *Web 部署配置*
5. 配置数据库连接，如下所示：

    - 在**服务器名称**类型 SQL 数据库服务器 URL 使用*tcp:*前缀。
    - 在**用户名**键入您的服务器管理员登录名。
    - 在**密码**键入服务器管理员登录密码。
    - 键入新的数据库名称。

    ![配置目标连接字符串](whats-new-in-web-forms-in-aspnet-45/_static/image46.png "配置目标连接字符串")

    *配置目标连接字符串*
6. 然后单击“确定” 。 当系统提示创建数据库单击**是**。

    ![创建数据库](whats-new-in-web-forms-in-aspnet-45/_static/image47.png "创建数据库字符串")

    *创建数据库*
7. 在默认连接文本框中显示了将用于连接到 Azure 中的 SQL 数据库连接字符串。 然后，单击 **“下一步”**。

    ![连接字符串指向 SQL 数据库](whats-new-in-web-forms-in-aspnet-45/_static/image48.png "指向 SQL 数据库的连接字符串")

    *连接字符串指向 SQL 数据库*
8. 在**预览**页上，单击**发布**。

    ![Web 应用程序发布](whats-new-in-web-forms-in-aspnet-45/_static/image49.png "发布 web 应用程序")

    *发布 web 应用程序*
9. 完成发布过程后，默认浏览器将打开已发布的网站。

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>附录 c： 使用代码段

和代码片段，必须将你能够轻松获得所需的所有代码所示。 实验室文档将告诉您完全时你可以使用它们，如下图中所示。

![使用 Visual Studio 代码段将代码插入到你的项目](whats-new-in-web-forms-in-aspnet-45/_static/image50.png "使用 Visual Studio 代码片段，可将代码插入到你的项目")

*使用 Visual Studio 代码段将代码插入到你的项目*

***若要添加代码片段使用键盘 (仅限 C#)***

1. 将光标置于想要插入代码。
2. 开始键入代码段名称 （不带空格或连字符）。
3. 观看作为 IntelliSense 显示匹配的代码段的名称。
4. 选择正确的代码段 （或继续键入直到选中整个代码段的名称）。
5. 按 Tab 键两次以光标位置处插入代码段。

![开始键入代码段名称](whats-new-in-web-forms-in-aspnet-45/_static/image51.png "开始键入代码段名称")

*开始键入代码段名称*

![按 Tab 以选择突出显示代码段](whats-new-in-web-forms-in-aspnet-45/_static/image52.png "按选项卡以选择突出显示代码段")

*按 Tab 以选择突出显示代码段*

![再次按 Tab 和代码段将会扩展](whats-new-in-web-forms-in-aspnet-45/_static/image53.png "再次按 Tab 和代码段将会扩展")

*再次按 Tab 和代码段将会扩展*

***若要添加代码片段使用鼠标 （C#、 Visual Basic 和 XML）*** 1。 右键单击你想要插入代码段。

1. 选择**插入代码段**跟**我的代码段**。
2. 选择相关的代码段从列表中，通过单击它。

![右键单击你想要插入代码段，并选择插入代码段](whats-new-in-web-forms-in-aspnet-45/_static/image54.png "右键单击你想要插入代码段，并选择插入代码段")

*右键单击你想要插入代码段，并选择插入代码段*

![通过单击它选取列表中中的相关代码片段](whats-new-in-web-forms-in-aspnet-45/_static/image55.png "选取相关的代码段从列表中，通过单击它")

*通过单击它选取从列表中，相关代码段*
