---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: "ASP.NET MVC 4 实体框架基架和迁移 |Microsoft 文档"
author: rick-anderson
description: "如果你熟悉 ASP.NET MVC 4 控制器方法，或已完成&quot;帮助器、 窗体和验证&quot;动手实验中，你应注意..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: 396859463446d95c58271c4b00fc950bcd0d539a
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a>ASP.NET MVC 4 实体框架基架和迁移

通过[Web 营地团队](https://twitter.com/webcamps)

[下载 Web 营地培训工具包](https://aka.ms/webcamps-training-kit)

如果你熟悉 ASP.NET MVC 4 控制器方法，或已完成&quot;帮助器、 窗体和验证&quot;动手实验中，你应注意的逻辑来创建，许多更新、 列出和删除它重复任何数据实体在应用程序。 更不必说，如果你的模型具有多个类来处理，你将可能花费相当长的时间编写 POST 和 GET 操作方法的每个实体操作，以及每个视图。

在此实验将了解如何使用 ASP.NET MVC 4 基架来自动生成的应用程序的 CRUD （创建、 读取、 更新和删除） 的基线。 启动从一个简单的模型类，，和而无需编写一行代码，将创建一个控制器，将包含所有的 CRUD 操作，以及所有必要视图。 生成并运行简单的解决方案后, 您将拥有生成的 MVC 逻辑和数据操作的视图一起应用程序数据库。

此外，你将了解如何轻松地使用 Entity Framework 迁移来执行整个应用程序中的模型更新。 Entity Framework 迁移将让你修改你的数据库后的模型已更改简单的步骤。 与所有这些记住，将能够构建和维护 web 应用程序更有效地利用 ASP.NET MVC 4 的最新功能。

> [!NOTE]
> 所有的示例代码和代码段都包含在 Web 营地培训工具包，可从在[Microsoft 的 Web/WebCampTrainingKit 版本](https://aka.ms/webcamps-training-kit)。 特定于此实验室项目位于[ASP.NET MVC 4 实体框架基架和迁移](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations)。

<a id="Objectives"></a>
### <a name="objectives"></a>目标

在本动手实验中，你将了解如何：

- 使用 ASP.NET 基架的控制器中的 CRUD 操作。
- 更改使用 Entity Framework 迁移的数据库模型。

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

下面的练习构成此动手实验：

1. [ASP.NET MVC 4 基架使用 Entity Framework 迁移](#Exercise1)

> [!NOTE]
> 本练习伴随**结束**包含生成您应该完成练习后获得的解决方案文件夹。 如果你需要通过练习工作的更多帮助，可以使用此解决方案作为指南。


估计时间来完成该实验： **30 分钟**

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a>练习 1： 将 ASP.NET MVC 4 基架使用 Entity Framework 迁移

ASP.NET MVC 基架提供创建必要的逻辑，可让你的应用程序与数据库层进行交互的标准化方式中生成的 CRUD 操作的快速方法。

在本练习中，您将学习如何使用 ASP.NET MVC 4 基架代码首先创建 CRUD 方法。 然后，你将了解如何更新模型来使用 Entity Framework 迁移应用在数据库中的更改。

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a>任务 1-创建新的 ASP.NET MVC 4 项目使用基架

1. 如果尚未打开，启动**Visual Studio 2012**。
2. 选择**文件 |新项目**。 在新建项目对话框中下, **Visual C# |Web**部分中，选择**ASP.NET MVC 4 Web 应用程序**。 命名项目**MVC4andEFMigrations**并将位置设置为**Source\Ex1 UsingMVC4ScaffoldingEFMigrations**这个实验室的文件夹。 设置**解决方案名称**到**开始**并确保**创建解决方案的目录**已选中。 单击 **“确定”**。

    ![新的 ASP.NET MVC 4 项目对话框](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "新建 ASP.NET MVC 4 项目对话框")

    *新的 ASP.NET MVC 4 项目对话框*
3. 在**新建 ASP.NET MVC 4 项目**对话框框中，选择**Internet 应用程序**模板，并确保**Razor**是所选**视图引擎**. 单击**确定**以创建该项目。

    ![新的 ASP.NET MVC 4 Internet 应用程序](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "新的 ASP.NET MVC 4 Internet 应用程序")

    *新的 ASP.NET MVC 4 Internet 应用程序*
4. 在解决方案资源管理器，右键单击**模型**和选择**添加 |类**创建简单的类人员 (POCO)。 将其命名为**人员**单击**确定**。
5. 打开 Person 类并插入以下属性。

    (代码段- *ASP.NET MVC 4 和 Entity Framework 迁移-Ex1 人员属性*)


    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
6. 单击**生成 |生成解决方案**以保存所做的更改并生成项目。

    ![生成应用程序](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "生成应用程序")

    *生成应用程序*
7. 在解决方案资源管理器，右键单击 controllers 文件夹，然后选择**添加 |控制器**。
8. 控制器*PersonController*并完成**基架选项**使用以下值。

    1. 在**模板**下拉列表中，选择**具有读/写操作和视图使用 Entity Framework 的 MVC 控制器**选项。
    2. 在**模型类**下拉列表中，选择**人员**类。
    3. 在**数据上下文类**列表中，选择**&lt;新建数据上下文...&gt;**. 选择任何名称，然后单击**确定**。
    4. 在**视图**下拉列表中，请确保**Razor**选择。

    ![添加基架的人员控制器](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "添加基架的人员控制器")

    *添加基架的人员控制器*
9. 单击**添加**使用基架的个人创建新的控制器。 您现在已生成的控制器操作以及视图。

    ![之后使用基架创建人员控制器](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "后使用基架创建人员控制器")

    *之后使用基架创建人员控制器*
10. 打开**PersonController**类。 请注意自动生成完整的 CRUD 操作方法。

    ![内部人员控制器](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "内部人员控制器")

    *内部人员控制器*

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a>任务 2-运行应用程序

此时，将不创建数据库。 在此任务中，将运行的应用程序第一次，并测试的 CRUD 操作。 将使用 Code First 动态创建数据库。

1. 按 **F5** 运行该应用程序。
2. 在浏览器中，添加**/Person**到 URL 以打开人员页。

    ![首次运行的应用程序](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "首次运行的应用程序")

    *首次运行应用程序：*
3. 现在，你将浏览人员页和测试的 CRUD 操作。

    1. 单击**新建**以添加新成员。 输入名字和姓氏，然后单击**创建**。

        ![添加新人员](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "添加新的用户")

        *添加新的用户*
    2. 在用户的列表中，你可以删除、 编辑或添加项。

        ![用户列表](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "用户列表")

        *用户列表*
    3. 单击**详细信息**以打开该人员的详细信息。

        ![人员的详细信息](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "人员的详细信息")

        *人员的详细信息*
4. 关闭浏览器并返回到 Visual Studio。 请注意，你已创建了人员实体整个 CRUD 整个-从到视图模型的应用程序而无需编写一行代码 ！

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a>任务 3-更新使用 Entity Framework 迁移的数据库

在此任务将更新使用 Entity Framework 迁移的数据库。 你会发现来更改模型和通过使用 Entity Framework 迁移功能反映数据库中的更改是多么容易。

1. 打开程序包管理器控制台。 选择**工具 |库包管理器 |程序包管理器控制台**。
2. 在程序包管理器控制台中，输入以下命令：

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    ![启用迁移](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "启用迁移")

    *启用迁移*

    启用迁移命令创建**迁移**文件夹，其中包含用于初始化数据库的脚本。

    ![迁移文件夹](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "Migrations 文件夹")

    *迁移文件夹*
3. 打开**Configuration.cs** Migrations 文件夹中的文件。 找到的类构造函数并将更改**AutomaticMigrationsEnabled**值赋给*true*。


    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
4. 打开 Person 类并添加人员的中间名属性。 使用此新的属性，您在更改模型。


    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
5. 选择**生成 |生成解决方案**上生成应用程序的菜单。

    ![生成应用程序](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "生成应用程序")

    *生成应用程序*
6. 在程序包管理器控制台中，输入以下命令：

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    此命令将会查找在对象的数据，更改，然后，它将添加必要的命令来相应地修改数据库。

    ![添加中间名](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "添加中间名")

    *添加中间名*
7. （可选）你可以运行以下命令以生成一个包含差异的更新的 SQL 脚本。 这将允许你手动更新数据库 （在这种情况下它不需要），或应用中其他数据库的更改：

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    ![生成 SQL 脚本](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "生成 SQL 脚本")

    *生成 SQL 脚本*

    ![SQL 脚本更新](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "SQL 脚本更新")

    *SQL 脚本更新*
8. 在程序包管理器控制台中，输入以下命令以更新数据库：

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    ![更新数据库](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "更新数据库")

    *更新数据库*

    这将添加**MiddleName**中的列**人员**表以匹配的当前定义**人员**类。
9. 更新数据库时，右键单击控制器文件夹，然后选择**添加 |控制器**人员将控制器添加再次 （具有相同值的完成）。 这将更新的现有方法和添加新属性的视图。

    ![添加控制器更新](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "添加控制器更新")

    *更新控制器*
10. 单击 **“添加”**。 然后，选择值**覆盖 PersonController.cs**和**覆盖关联视图**单击**确定**。

    ![添加控制器覆盖](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

    *更新控制器*

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a>任务 4-运行应用程序

1. 按 **F5** 运行该应用程序。
2. 打开**/Person**。 请注意已保留的数据时的中间名列已添加。

    ![添加的中间名](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "添加的中间名")

    *添加的中间名*
3. 如果你单击**编辑**，你将能够将中间名添加到当前的人员。

    ![中间名版本](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "中间名版本")

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>摘要

在此动手实验中，你学习了简单的步骤，使用 ASP.NET MVC 4 基架使用任何模型类创建 CRUD 操作。 然后，你已学习如何使用 Entity Framework 迁移-从数据库到视图的应用程序中执行的端到端更新。

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>附录 a： 安装 Visual Studio Express 2012 for Web

你可以安装**Microsoft Visual Studio Express 2012 for Web**或另一个&quot;Express&quot;版本使用 **[Microsoft Web 平台安装程序](https://www.microsoft.com/web/downloads/platform.aspx)**. 以下说明将指导你完成安装所需的步骤*Visual studio Express 2012 for Web*使用*Microsoft Web 平台安装程序*。

1. 转到[ [https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)。 或者，如果你已安装 Web 平台安装程序，你可以打开它，并搜索产品&quot; *Visual Studio Express 2012 for Web 的 Windows Azure SDK*&quot;。
2. 单击**立即安装**。 如果你没有**Web 平台安装程序**将重定向以下载并请先安装它。
3. 一次**Web 平台安装程序**处于打开状态，单击**安装**以启动安装程序。

    ![安装 Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "安装 Visual Studio Express")

    *安装 Visual Studio Express*
4. 阅读所有产品的许可证和条款，然后单击**我接受**以继续。

    ![接受许可条款](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    接受许可条款
5. 等待，直到下载和安装过程完成。

    ![安装进度](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    *安装进度*
6. 当安装完成后时，单击**完成**。

    ![安装已完成](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    安装已完成
7. 单击**退出**以关闭 Web 平台安装程序。
8. 若要打开 Visual Studio Express for Web，请转到**启动**屏幕并开始编写&quot; **VS Express**&quot;，然后单击**VS Express for Web**磁贴。

    ![Web 磁贴的 VS Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    Web 磁贴的 VS Express

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>附录 b： 使用代码片段

和代码片段，必须将你能够轻松获得所需的所有代码所示。 实验室文档将告诉您完全时你可以使用它们，如下图中所示。

![使用 Visual Studio 代码段将代码插入到你的项目](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "使用 Visual Studio 代码片段，可将代码插入到你的项目")

*使用 Visual Studio 代码段将代码插入到你的项目*

***若要添加代码片段使用键盘 (仅限 C#)***

1. 将光标置于想要插入代码。
2. 开始键入代码段名称 （不带空格或连字符）。
3. 观看作为 IntelliSense 显示匹配的代码段的名称。
4. 选择正确的代码段 （或继续键入直到选中整个代码段的名称）。
5. 按 Tab 键两次以光标位置处插入代码段。

![开始键入代码段名称](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "开始键入代码段名称")

*开始键入代码段名称*

![按 Tab 以选择突出显示代码段](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "按选项卡以选择突出显示代码段")

*按 Tab 以选择突出显示代码段*

![再次按 Tab 和代码段将会扩展](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "再次按 Tab 和代码段将会扩展")

*再次按 Tab 和代码段将会扩展*

***若要添加代码片段使用鼠标 （C#、 Visual Basic 和 XML）*** 1。 右键单击你想要插入代码段。

1. 选择**插入代码段**跟**我的代码段**。
2. 选择相关的代码段从列表中，通过单击它。

![右键单击你想要插入代码段，并选择插入代码段](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "右键单击你想要插入代码段，并选择插入代码段")

*右键单击你想要插入代码段，并选择插入代码段*

![通过单击它选取列表中中的相关代码片段](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "选取相关的代码段从列表中，通过单击它")

*通过单击它选取从列表中，相关代码段*
