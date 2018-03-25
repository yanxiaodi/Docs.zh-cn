---
uid: aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
title: "在实验室上处理： 容易维护的 Azure 网站： 管理更改和缩放 |Microsoft 文档"
author: rick-anderson
description: "在本实验中，了解如何 Microsoft Azure 可以轻松地生成和部署到生产环境的网站。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: ecfd0eb4-c4ad-44e6-9db9-a2a66611ff6a
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
msc.type: authoredcontent
ms.openlocfilehash: 4bce02b2c592ff04e0dbce78d18004c69268e4fd
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
<a name="hands-on-lab-maintainable-azure-websites-managing-change-and-scale"></a>在实验室上处理： 容易维护的 Azure 网站： 管理更改和缩放
====================
通过[Web 营地团队](https://twitter.com/webcamps)

[下载 Web 营地培训工具包](http://aka.ms/webcamps-training-kit)

> Microsoft Azure，可以轻松生成并部署到生产环境的网站。 但你还未完成你的应用程序处于活动状态时，你刚开始 ！ 你将需要处理更改要求、 数据库更新、 缩放和的详细信息。 幸运的是，Azure App Service 具有您的要求，并提供充足的功能来帮助您保留您的网站平稳运行。
> 
> Azure 提供安全、 灵活的开发、 部署和缩放选项任何规模的 web 应用程序。 利用现有工具创建和部署应用程序而无需管理基础结构。
> 
> 生产 web 应用程序自行以分钟为单位的设置轻松地部署使用你最喜欢的开发工具创建的内容。 你可以部署现有网站直接从支持的源控件**Git**， **GitHub**， **Bitbucket**， **TFS**，并甚至**DropBox**。 部署直接通过你喜欢的 IDE 或通过脚本使用**PowerShell** Windows 中或**CLI**在任何操作系统上运行的工具。 部署后，跟踪你的站点进行连续部署的支持不断最新。
> 
> 无论大小，azure 会为任何数据，提供可缩放、 持久的云存储、 备份和恢复解决方案。 在部署到生产环境，如表、 Blob 和 SQL 数据库的存储服务的应用程序，使你在云中将应用程序缩放。
> 
> 使用 SQL 数据库，务必使你的工作效率的数据库部署你的应用程序的新版本时保持最新。 感谢到**Entity Framework Code First 迁移**，已简化的开发和部署你的数据模型以在几分钟内更新你的环境。 本动手实验将显示在 web 应用部署到 Microsoft Azure 中的生产环境时，可能会遇到的不同主题。
> 
> 在 Web 营地培训工具包中，在包括所有的示例代码和代码段[http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit)。
> 
> 本主题的详细深入覆盖率，请参阅[构建真实世界云应用程序与 Azure 的电子书](building-real-world-cloud-apps-with-windows-azure/introduction.md)。


<a id="Overview"></a>
## <a name="overview"></a>概述

<a id="Objectives"></a>
### <a name="objectives"></a>目标

在此动手实验中，你将了解如何：

- 使用现有模型中启用 Entity Framework 迁移
- 更新的对象模型和相应地使用 Entity Framework 迁移数据库
- 将部署到 Azure App Service 中使用 Git
- 回滚到以前的部署使用 Azure 管理门户
- 使用 Azure 存储空间来扩展 web 应用
- 配置使用 Azure 管理门户的 web 应用程序的自动缩放
- 创建和配置 Visual Studio 中的负载测试项目

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>系统必备

完成本动手实验需要以下：

- [Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/)或更高版本
- [Azure SDK for .NET 2.2](https://www.microsoft.com/windowsazure/sdk/)
- [GIT 版本控制系统](http://git-scm.com/download)
- Microsoft Azure 订阅 

    - 注册[免费试用版](http://aka.ms/watk-freetrial)
    - 如果你是 Visual Studio 专业版、 专业测试工具版、 高级版或旗舰版与 MSDN 或 MSDN 平台订阅服务器，激活你[MSDN 权益](http://aka.ms/watk-msdn)现在，若要开始开发和测试在 Azure 上
    - [BizSpark](http://aka.ms/watk-bizspark)成员自动接收 Azure 通过其 Visual Studio Ultimate with MSDN 订阅权益
    - 成员[Microsoft 合作伙伴网络](http://aka.ms/watk-mpn)Cloud Essentials 程序中收到免费的月度 Azure 信用额度

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

1. [使用 Entity Framework 迁移](#Exercise1)
2. [将 Web 应用部署到过渡环境](#Exercise2)
3. [在生产环境中执行部署回滚](#Exercise3)
4. [使用 Azure 存储进行缩放](#Exercise4)
5. [使用 Web Apps 自动缩放](#Exercise5)（对于 Visual Studio 2013 Ultimate edition 可选）

估计时间来完成该实验： **75 分钟**

> [!NOTE]
> 当你首次启动 Visual Studio 时，你必须选择一个预定义的设置集合。 每个预定义的集合用于匹配特定的开发风格和确定窗口布局、 编辑器行为、 IntelliSense 代码段和对话框选项。 在本实验中的过程描述完成给定的任务 Visual Studio 中使用时所需的操作**常规开发设置**集合。 如果您为开发环境选择不同的设置集合，则表明可能存在你应考虑到帐户的步骤中的差异。


<a id="Exercise1"></a>
### <a name="exercise-1-using-entity-framework-migrations"></a>练习 1： 使用 Entity Framework 迁移

当正在开发的应用时，你的数据模型可能随时间变化。 （如果要创建新的版本），这些更改可能会影响你的数据库中的现有模型，而且，务必使数据库保持为最新，以防止出现错误。

若要简化在模型中，这些更改的跟踪**Entity Framework Code First 迁移**自动检测更改将数据库架构与您模型进行比较，生成特定的代码以更新你的数据库，创建新*版本*的你的数据库。

此练习展示如何启用**迁移**为你的应用程序以及如何可以轻松地检测和生成更改来更新你的数据库。

<a id="Ex1Task1"></a>
#### <a name="task-1--enabling-migrations"></a>任务 1 – 启用迁移

在此任务中，你将经历的步骤启用**Entity Framework Code First 迁移**到**专家 Quiz**数据库，更改模型和了解如何将这些更改反映在数据库。

1. 打开 Visual Studio 并打开**GeekQuiz.sln**解决方案文件从**Source\Ex1 UsingEntityFrameworkMigrations\Begin**。
2. 生成才能下载和安装解决方案**NuGet**包的依赖关系。 若要执行此操作，右键单击该解决方案，然后单击**生成解决方案**或按**Ctrl + Shift + B**。
3. 从**工具**菜单在 Visual Studio 中，选择**库程序包管理器**，然后单击**程序包管理器控制台**。
4. 在**程序包管理器控制台**，输入以下命令，然后按**Enter**。 将创建基于现有模型的初始迁移。

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample1.ps1)]

    ![启用迁移](maintainable-azure-websites-managing-change-and-scale/_static/image1.png "启用迁移")

    *启用迁移*

    > [!NOTE]
    > 此命令将添加**迁移**到奇 Quiz 项目包含文件的文件夹调用**Configuration.cs**。 **配置**类允许你配置迁移你上下文的行为方式。
5. 借助启用迁移，你需要更新**配置**类使用的初始数据填充数据库的**专家 Quiz**要求。 下**迁移**，替换**Configuration.cs**替换为文件位于**Source\Assets**这个实验室的文件夹。

    > [!NOTE]
    > 由于**迁移**将调用**种子**与数据库中的每个更新的方法，你需要以确保在数据库中的用户不能重复记录。 **AddOrUpdate**方法将有助于防止重复数据。
6. 若要添加的初始迁移，输入以下命令，然后按**Enter**。

    > [!NOTE]
    > 请确保没有名为数据库&quot;GeekQuizProd&quot; LocalDB 实例中。

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample2.ps1)]

    ![添加基础架构迁移](maintainable-azure-websites-managing-change-and-scale/_static/image2.png "添加基础架构迁移")

    *添加基础架构迁移*

    > [!NOTE]
    > **添加迁移**将创建基于自上次迁移创建以来对您的模型所做的更改的下一次迁移的基架。 在这种情况下，由于它是项目的初始迁移时，它将添加用于创建在中定义的所有表的脚本**TriviaContext**类。
7. 执行迁移后，以通过运行以下命令更新数据库。 对于此命令将指定**详细**标志以查看应用到目标数据库的 SQL 语句。

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample3.ps1)]

    ![创建初始数据库](maintainable-azure-websites-managing-change-and-scale/_static/image3.png "创建初始数据库")

    *创建初始数据库*

    > [!NOTE]
    > **更新数据库**将应用所有挂起的迁移到数据库。 在这种情况下，它将创建使用 web.config 文件中定义的连接字符串的数据库。
8. 转到**视图**菜单并打开**SQL Server 对象资源管理器**。

    ![在 SQL Server 对象资源管理器中打开](maintainable-azure-websites-managing-change-and-scale/_static/image4.png "在 SQL Server 对象资源管理器中打开")

    *在 SQL Server 对象资源管理器中打开*
9. 在**SQL Server 对象资源管理器**窗口中，右键单击连接到 LocalDB 实例**SQL Server**节点并选择**添加 SQL Server...**选项。

    ![添加 SQL Server 实例](maintainable-azure-websites-managing-change-and-scale/_static/image5.png "添加 SQL Server 实例")

    *将 SQL Server 实例添加到 SQL Server 对象资源管理器*
10. 设置**服务器名称**到*(localdb) \v11.0*并使**Windows 身份验证**作为身份验证模式。 单击**连接**以继续。

    ![连接到 LocalDB](maintainable-azure-websites-managing-change-and-scale/_static/image6.png "连接到 LocalDB")

    *连接到 LocalDB*
11. 打开**GeekQuizProd**数据库中，展开**表**节点。 如你所见，**更新数据库**命令生成定义中的所有表**TriviaContext**类。 找到**dbo。TriviaQuestions**表，然后打开列节点。 在下一步的任务中，你将向此表中添加新列，并更新数据库使用**迁移**。

    ![琐事问题列](maintainable-azure-websites-managing-change-and-scale/_static/image7.png "琐事问题列")

    *琐事问题列*

<a id="Ex1Task2"></a>
#### <a name="task-2--updating-database-schema-using-migrations"></a>任务 2 – 使用迁移更新数据库架构

在此任务中，你将使用**Entity Framework Code First 迁移**检测你的模型中的更改并生成必要的代码以更新数据库。 你将更新**TriviaQuestions**通过向其添加一个新属性的实体。 然后，将运行命令以创建新的迁移，以包括表中的新列。

1. 在**解决方案资源管理器**，双击**TriviaQuestion.cs**文件位于内**模型**文件夹。
2. 添加名为的新属性**提示**，下面的代码段中所示。

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample4.cs)]
3. 在**程序包管理器控制台**，输入以下命令，然后按**Enter**。 以反映在我们的模型更改，将创建一个新迁移。

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample5.ps1)]

    ![Add-Migration](maintainable-azure-websites-managing-change-and-scale/_static/image8.png "Add-Migration")

    *Add-Migration*

    > [!NOTE]
    > 迁移文件组成两种方法，**向上**和**下**。
    > 
    > - **向上**方法将用于指定哪些更改我们的应用程序需要将应用于数据库的当前版本。
    > - **下**用于反向我们已添加到的更改**向上**方法。
    > 
    > 当数据库迁移更新数据库时，它将按时间戳顺序仅显示那些尚未使用自上次更新后运行的所有迁移 ( \_MigrationHistory 表将跟踪的哪些迁移已应用)。 **向上**的所有迁移的方法将调用并将使我们指定到数据库的更改。 如果我们决定回到先前的迁移，**下**将调用方法，以重做以反向顺序中的更改。
4. 在**程序包管理器控制台**，输入以下命令，然后按**Enter**。

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample6.ps1)]
5. 输出**更新数据库**生成命令**Alter Table** SQL 语句添加到新列**TriviaQuestions**表下, 图中所示。

    ![添加列 SQL 语句生成](maintainable-azure-websites-managing-change-and-scale/_static/image9.png "添加列生成的 SQL 语句")

    *添加列生成的 SQL 语句*
6. 在**SQL Server 对象资源管理器**，刷新**dbo。TriviaQuestions**表，以检查新**提示**显示列。

    ![显示新的提示列](maintainable-azure-websites-managing-change-and-scale/_static/image10.png "显示新的提示列")

    *显示新的提示列*
7. 返回**TriviaQuestion.cs**编辑器中，添加**StringLength**约束*提示*属性，如下面的代码段中所示。

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample7.cs)]
8. 在**程序包管理器控制台**，输入以下命令，然后按**Enter**。

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample8.ps1)]
9. 在**程序包管理器控制台**，输入以下命令，然后按**Enter**。

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample9.ps1)]
10. 输出**更新数据库**生成命令**Alter Table** SQL 语句，以更新*提示*列类型**TriviaQuestions**表下, 图中所示。

    ![Alter column SQL 语句生成](maintainable-azure-websites-managing-change-and-scale/_static/image11.png "Alter 列生成的 SQL 语句")

    *更改列生成的 SQL 语句*
11. 在**SQL Server 对象资源管理器**，刷新**dbo。TriviaQuestions**表，然后检查**提示**列类型是**nvarchar(150)**。

    ![显示新约束](maintainable-azure-websites-managing-change-and-scale/_static/image12.png "显示新约束")

    *显示新约束*

<a id="Exercise2"></a>
### <a name="exercise-2-deploying-a-web-app-to-staging"></a>练习 2： 将 Web 应用部署到过渡环境

**Web 应用在 Azure App Service**使您可以执行过渡的发布。 过渡的发布创建为每个默认生产站点过渡站点槽，并使你能够交换这些槽位而无需停机时间。 这是真正发挥作用，在发布到公共之前验证更改，逐步集成站点内容，并回滚更改未按预期工作。

在本练习中，你将部署**专家 Quiz**到你使用 Git 源代码管理的 web 应用的过渡环境的应用程序。 若要执行此操作，将创建 web 应用和设置在管理门户所需的组件，配置**Git**的源存储库和推送应用程序从本地计算机到过渡槽的代码。 你还要更新与生产数据库**Code First 迁移**你在上一练习中创建。 然后，您将在此测试环境以验证其操作中执行应用程序。 一旦您满意后它按预期工作，将提升到生产应用程序。

> [!NOTE]
> 若要启用暂存的发布，web 应用必须处于**标准模式**。 请注意，是否将 web 应用程序更改为标准模式时将会产生额外费用。 有关定价的详细信息，请参阅[App Service 定价](https://azure.microsoft.com/pricing/details/app-service/)。


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-web-app-in-azure-app-service"></a>任务 1 – 在 Azure App Service 中创建 Web 应用

在此任务中，你将创建中的 web 应用**Azure App Service**从管理门户。 你还将配置**SQL 数据库**以持久保存应用程序数据，并配置源代码管理的本地 Git 存储库。

1. 转到[Azure 管理门户](https://manage.windowsazure.com)并使用与你的订阅关联的 Microsoft 帐户登录。

    ![登录到 Azure 管理门户](maintainable-azure-websites-managing-change-and-scale/_static/image13.png)

    *登录到 Azure 管理门户*
2. 单击**新建**在页面底部的命令栏中。

    ![创建新的 web 应用](maintainable-azure-websites-managing-change-and-scale/_static/image14.png "创建新的 web 应用程序")

    *创建新的 web 应用程序*
3. 单击**计算**，**网站**然后**自定义创建**。

    ![创建新的 web 应用程序使用自定义创建](maintainable-azure-websites-managing-change-and-scale/_static/image15.png "创建使用自定义创建新的 web 应用程序")

    *创建使用自定义创建新的 web 应用程序*
4. 在**新建网站-自定义创建**对话框框中，提供可用**URL** (例如*专家测验*) 中, 选择一个位置**区域**下拉列表，然后选择**创建新的 SQL 数据库**中**数据库**下拉列表。 最后，选择**--从源代码管理发布**复选框，然后单击**下一步**。

    ![自定义新的 web 应用](maintainable-azure-websites-managing-change-and-scale/_static/image16.png)

    *自定义新的 web 应用*
5. 指定数据库设置的以下信息：

    - 在**名称**文本框中，输入数据库名称 (例如*geekquiz\_db*)
    - 在服务器中**下拉**列表中，选择**新建 SQL 数据库服务器**。 或者，你可以选择现有的服务器。
    - 在**数据库用户名**和**数据库密码**框中，为 SQL 数据库服务器输入管理员用户名和密码。 如果你选择一个服务器已创建，系统将提示你输入密码。

    ![指定数据库设置](maintainable-azure-websites-managing-change-and-scale/_static/image17.png)

    *指定数据库设置*
6. 单击 **“下一步”** 以继续。
7. 选择**本地 Git 存储库**进行源代码管理，以使用单击**下一步**。

    > [!NOTE]
    > 可能会提示你输入部署凭据 （用户名和密码）。

    ![创建 Git 存储库](maintainable-azure-websites-managing-change-and-scale/_static/image18.png)

    *创建 Git 存储库*
8. 等待，直到创建新的 web 应用。

    > [!NOTE]
    > 默认情况下，Azure 提供在域*azurewebsites.net*但还为你提供将使用 Azure 管理门户的自定义域的可能性。 但是，如果你使用某些 Azure App Service 模式下，仅可以管理自定义域。
    > 
    > Azure App Service 是在免费、 共享、 基本、 标准和高级版中可用。 在免费和共享模式下的所有 web 应用在多租户环境中运行，并且都 CPU、 内存和网络使用量配额。 免费应用程序的最大数可能因您的计划。 在标准模式下，你可以选择到标准 Azure 对应的专用虚拟机上运行的应用的计算资源。 你可以查找中的 web 应用模式配置**缩放**的 web 应用程序的菜单。
    > 
    > ![Azure App Service 模式](maintainable-azure-websites-managing-change-and-scale/_static/image19.png "Azure App Service 模式")
    > 
    > 如果你使用**共享**或**标准**模式下，你将能够通过转到您的应用程序管理有关你的 web 应用的自定义域**配置**菜单，然后单击**管理域**下*域名*。
    > 
    > ![管理域](maintainable-azure-websites-managing-change-and-scale/_static/image20.png "管理域")
    > 
    > ![管理自定义域](maintainable-azure-websites-managing-change-and-scale/_static/image21.png "管理自定义域")
9. 创建 web 应用后，单击下的链接**URL**列来检查新的 web 应用正在运行。

    ![浏览到新的 web 应用](maintainable-azure-websites-managing-change-and-scale/_static/image22.png)

    *浏览到新的 web 应用*

    ![运行的 web 应用程序](maintainable-azure-websites-managing-change-and-scale/_static/image23.png)

    *运行的 web 应用程序*

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-production-sql-database"></a>任务 2 – 创建生产 SQL 数据库

在此任务中，你将使用**Entity Framework Code First 迁移**若要创建的数据库目标**Azure SQL 数据库**你在上一任务中创建的实例。

1. 在管理门户中，导航到你在上一任务中创建的 web 应用，并转到其**仪表板**。
2. 在**仪表板**页上，单击**查看连接字符串**链接下**速览**部分。

    ![查看连接字符串](maintainable-azure-websites-managing-change-and-scale/_static/image24.png "查看连接字符串")

    *查看连接字符串*
3. 复制**连接字符串**值并关闭对话框。

    ![在 Azure 管理门户中的连接字符串](maintainable-azure-websites-managing-change-and-scale/_static/image25.png "在 Azure 管理门户中的连接字符串")

    *在 Azure 管理门户中的连接字符串*
4. 单击**SQL 数据库**若要查看 Azure 中的 SQL 数据库的列表

    ![SQL 数据库菜单](maintainable-azure-websites-managing-change-and-scale/_static/image26.png "SQL 数据库菜单")

    *SQL 数据库菜单*
5. 找到你在上一任务中创建的数据库并单击该服务器。

    ![SQL 数据库服务器](maintainable-azure-websites-managing-change-and-scale/_static/image27.png "SQL 数据库服务器")

    *SQL 数据库服务器*
6. 在**快速启动**页的服务器上，单击**配置**。

    ![配置菜单](maintainable-azure-websites-managing-change-and-scale/_static/image28.png "配置菜单")

    *配置菜单*
7. 在**允许的 IP 地址**部分中，单击**将添加到允许的 IP 地址**链接以启用你的 IP 连接到 SQL 数据库服务器。

    ![允许的 IP 地址](maintainable-azure-websites-managing-change-and-scale/_static/image29.png "允许的 IP 地址")

    *允许的 IP 地址*
8. 单击**保存**要完成该步骤的页的底部。
9. 切换回 Visual Studio。
10. 在**程序包管理器控制台**，执行以下命令将*[您的连接字符串]*占位符替换为从 Azure 复制的连接字符串

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample10.ps1)]

    ![面向 Windows Azure SQL 数据库更新数据库](maintainable-azure-websites-managing-change-and-scale/_static/image30.png "面向 Windows Azure SQL 数据库更新数据库")

    *更新目标 Azure SQL 数据库的数据库*

<a id="Ex2Task3"></a>
#### <a name="task-3--deploying-geek-quiz-to-staging-using-git"></a>任务 3 – 部署到过渡环境使用 Git 的专家测验

在此任务中，你将 web 应用中启用过渡的发布。 然后，将使用 Git 发布专家 Quiz 应用程序直接从本地计算机到你的 web 应用的过渡环境。

1. 返回到门户并单击下的 web 应用的名称**名称**列以显示的管理页。

    ![打开 web 应用程序管理页面](maintainable-azure-websites-managing-change-and-scale/_static/image31.png)

    *打开 web 应用程序管理页面*
2. 导航到**缩放**页。 下**常规**部分中，选择**标准**配置和单击**保存**命令栏中。

    > [!NOTE]
    > 若要在当前区域和订阅中的运行的所有 web 应用**标准**模式下，保留**选择所有**复选框中选择**选择站点**配置。 否则，请清除**选择所有**复选框。

    ![将 web 应用程序升级到标准模式](maintainable-azure-websites-managing-change-and-scale/_static/image32.png "将 web 应用程序升级到标准模式")

    *将 Web 应用程序升级到标准模式*
3. 单击**是**以确认所做的更改。

    ![确认更改为标准模式](maintainable-azure-websites-managing-change-and-scale/_static/image33.png "继续执行更改的 web 应用程序模式")

    *确认更改为标准模式*
4. 转到**仪表板**页上，单击**启用暂存发布**下**速览**部分。

    ![启用过渡的发布](maintainable-azure-websites-managing-change-and-scale/_static/image34.png "启用暂存发布")

    *启用过渡的发布*
5. 单击**是**才能启用过渡的发布。

    ![确认暂存发布](maintainable-azure-websites-managing-change-and-scale/_static/image35.png "单击是以启用暂存的发布")

    *确认过渡的发布*
6. 在 web apps 列表中，展开您的 web 应用名称以显示过渡站点槽左侧的标记。 它具有名称的 web 应用跟***（过渡）***。 单击要转到管理页的过渡站点。

    ![导航到过渡 web 应用](maintainable-azure-websites-managing-change-and-scale/_static/image36.png "导航到过渡 web 应用")

    *导航到过渡应用*
7. 请注意该他管理页与下面类似任何其他 web 应用程序的管理页。 导航到**部署**页和复制**Git URL**值。 你将稍后在本练习中使用它。

    ![复制 Git URL 值](maintainable-azure-websites-managing-change-and-scale/_static/image37.png)

    *复制 Git URL 值*
8. 打开一个新**Git Bash**控制台，然后执行以下命令。 更新*[您的应用程序的路径]*占位符替换为的路径**GeekQuiz**解决方案，位于**Source\Ex1 DeployingWebSiteToStaging\Begin**文件夹此实验室中。

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample11.cmd)]

    ![Git 初始化和第一个提交](maintainable-azure-websites-managing-change-and-scale/_static/image38.png)

    *Git 初始化和第一个提交*
9. 运行以下命令以将你的 web 应用推送到远程**Git**存储库。 将占位符替换为你从管理门户获得的 URL。 系统将提示您输入部署密码。

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample12.cmd)]

    ![将推送到 Windows Azure](maintainable-azure-websites-managing-change-and-scale/_static/image39.png)

    *将推送到 Azure*

    > [!NOTE]
    > 将内容部署到的 FTP 主机或 GIT 存储库的 web 应用，你必须进行身份验证使用**部署凭据**从 web 应用创建**快速启动**或**仪表板**管理页。 如果你不知道你的部署凭据你可以轻松地重置它们使用管理门户。 打开 web 应用**仪表板**页上，单击**重置部署凭据**链接。 提供新密码并单击**确定**。 部署凭据可用于使用与你的订阅相关联的所有 web 应用。
10. 为了验证 web 应用已成功推送到 Azure，请返回到管理门户并单击**网站**。
11. 找到你的 web 应用并展开以显示过渡站点槽的条目。 单击其**名称**转到管理页。
12. 单击**部署**若要查看**部署历史记录**。 验证是否有**活动部署**与你*&quot;初始提交&quot;*。

    ![活动的部署](maintainable-azure-websites-managing-change-and-scale/_static/image40.png)

    *活动的部署*
13. 最后，单击**浏览**以转到 web 应用程序的命令栏中。

    ![浏览 web 应用](maintainable-azure-websites-managing-change-and-scale/_static/image41.png)

    *浏览 web 应用*
14. 如果已成功部署的应用，你将看到专家 Quiz 登录页。

    > [!NOTE]
    > 部署的应用程序的地址 URL 包含名称的 web 应用跟*-暂存*。

    ![在过渡环境中运行的应用程序](maintainable-azure-websites-managing-change-and-scale/_static/image42.png)

    *在过渡环境中运行的应用程序*
15. 如果你想要使用此应用程序，请单击**注册**注册新的用户。 通过输入用户名、 电子邮件地址和密码完成帐户详细信息。 接下来，应用程序显示测验的第一个问题。 回答几个问题以确保它按预期工作。

    ![应用程序可供使用](maintainable-azure-websites-managing-change-and-scale/_static/image43.png)

    *应用程序可供使用*

<a id="Ex2Task4"></a>
#### <a name="task-4--promoting-the-web-app-to-production"></a>任务 4 – 将提升到生产 Web 应用

现在，你已验证 web 应用在过渡环境中正常运行，你就可以将其升级到生产环境。 在此任务中，将交换过渡站点槽与生产站点槽。

1. 返回到管理门户并选择过渡站点槽。 单击**交换**命令栏中。

    ![切换到生产环境](maintainable-azure-websites-managing-change-and-scale/_static/image44.png)

    *切换到生产环境*
2. 单击**是**在确认对话框中，以继续执行交换操作。 Azure 将立即交换生产站点的暂存站点内容的内容。

    > [!NOTE]
    > 某些设置从的过渡版本将自动复制对生产版本 （例如连接字符串替代、 处理程序映射等），但其他设置将不会更改 （例如 DNS 终结点、 SSL 绑定等）。

    ![确认交换操作](maintainable-azure-websites-managing-change-and-scale/_static/image45.png)

    *确认交换操作*
3. 交换完成后，请选择生产槽，然后单击**浏览**命令栏中打开生产站点中。 请注意地址栏中的 URL。

    > [!NOTE]
    > 你可能需要刷新浏览器以清除缓存。 在 Internet Explorer 中，你可以执行此操作通过按**CTRL + R**。

    ![在生产环境中运行的 web 应用](maintainable-azure-websites-managing-change-and-scale/_static/image46.png)
4. 在**GitBash**控制台中，更新本地 Git 存储库的远程 URL 以面向生产槽。 若要执行此操作，请运行以下命令将占位符替换为你部署的用户名和你的 web 应用的名称。

    > [!NOTE]
    > 在以下练习中，会将更改推送到生产站点，而不是只是为了实验室的简单性过渡。 在实际方案中，建议在升级到生产环境之前，验证在过渡环境中的更改。

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample13.cmd)]

<a id="Exercise3"></a>
### <a name="exercise-3-performing-deployment-rollback-in-production"></a>练习 3： 在生产环境中执行部署回滚

其中你没有过渡槽以热之间执行交换过渡和生产，例如，如果你正在使用的情况下**免费**或**共享**模式。 在这些情况下，你应测试应用程序在测试环境中的 – 本地或远程站点中 – 部署到生产环境之前。 但是，很可能在生产站点中可能出现在测试阶段未检测到的问题。 在这种情况下，务必具有一种机制来轻松尽可能快地切换到上一个和更稳定版本的应用程序。

在**Azure App Service**，从源代码管理的连续部署功能可使到此可能谢谢**重新部署**在管理门户中可用的操作。 Azure 跟踪与提交推送到存储库关联的部署，并提供了一个选项以重新部署应用程序使用任何你以前的部署，在任何时间。

在本练习中，你将执行对中的代码的更改**专家 Quiz**应用程序有意将注入*bug*。 你将部署到生产应用程序，以查看错误，，然后你将利用重新部署功能，请返回到以前的状态。

<a id="Ex3Task1"></a>
#### <a name="task-1--updating-the-geek-quiz-application"></a>任务 1 – 更新专家测验应用程序

在此任务中，将重构代码的一小部分**TriviaController**类，以提取所选的测验选项从数据库中检索成新方法的逻辑的一部分。

1. 切换到 Visual Studio 实例与**GeekQuiz**从上一练习的解决方案。
2. 在**解决方案资源管理器**，打开**TriviaController.cs**文件**控制器**文件夹。
3. 找到**StoreAsync**方法，然后在下图中突出显示代码的选择。

    ![选择代码](maintainable-azure-websites-managing-change-and-scale/_static/image47.png)

    *选择代码*
4. 右键单击所选的代码，展开**重构**菜单，然后选择**提取方法...**.

    ![提取作为新方法的代码](maintainable-azure-websites-managing-change-and-scale/_static/image48.png)

    *选择提取方法*
5. 在**提取方法**对话框中，命名新方法*MatchesOption*单击**确定**。

    ![指定方法名称](maintainable-azure-websites-managing-change-and-scale/_static/image49.png)

    *指定提取的方法的名称*
6. 所选的代码然后提取到**MatchesOption**方法。 生成的代码将显示在下面的代码段。

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample14.cs)]
7. 按**CTRL + S**以保存所做的更改。

<a id="Ex3Task2"></a>
#### <a name="task-2--redeploying-the-geek-quiz-application"></a>任务 2 – 重新部署专家测验应用程序

你现在将推送到存储库，将触发新的部署到生产环境中前一项任务所做的更改。 然后，你将已进行故障诊断问题使用**F12 开发工具**提供由 Internet Explorer，然后执行回滚到以前的部署从 Azure 管理门户。

1. 打开一个新**Git Bash**控制台部署到 Azure App Service 更新的应用程序。
2. 执行以下命令以将更改推送到 Azure。 更新*[您的应用程序的路径]*占位符替换为的路径**GeekQuiz**解决方案。 系统将提示您输入部署密码。

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample15.cmd)]

    ![将重构的代码推送到 Azure](maintainable-azure-websites-managing-change-and-scale/_static/image50.png)

    *将重构的代码推送到 Azure*
3. 打开 Internet Explorer 并导航到你的 web 应用 (例如`http://<your-web-site>.azurewebsites.net`)。 使用以前创建的凭据登录。
4. 按**F12**若要启动的开发工具，选择**网络**选项卡，单击**播放**按钮以开始记录。

    ![启动网络录制](maintainable-azure-websites-managing-change-and-scale/_static/image51.png "启动网络录制")

    *启动网络录制*
5. 选择任何选项测验。 你将看到没有任何反应。
6. 在**F12**窗口中，对应 POST HTTP 请求的项显示 HTTP **500**结果。

    ![HTTP 500 error](maintainable-azure-websites-managing-change-and-scale/_static/image52.png)

    *HTTP 500 error*
7. 选择**控制台**选项卡。可能的原因的详细信息记录错误。

    ![记录的错误](maintainable-azure-websites-managing-change-and-scale/_static/image53.png)

    *记录的错误*
8. 查找错误的详细信息部分。 显然，重构你在前面的步骤中提交的代码导致此错误。

    `Details: LINQ to Entities does not recognize the method 'Boolean MatchesOption ...`。
9. 不要关闭浏览器。
10. 在新浏览器实例中，导航到[Azure 管理门户](https://manage.windowsazure.com)并使用与你的订阅关联的 Microsoft 帐户登录。
11. 选择**网站**单击在练习 2 中创建的 web 应用。
12. 导航到**部署**页。 请注意，在部署历史记录中会列出执行的所有提交。

    ![现有的部署列表](maintainable-azure-websites-managing-change-and-scale/_static/image54.png)

    *现有的部署列表*
13. 选择上一次提交，然后单击**重新部署**命令栏上。

    ![重新部署以前的提交](maintainable-azure-websites-managing-change-and-scale/_static/image55.png)

    *重新部署以前的提交*
14. 当系统提示确认，请单击**是**。

    ![确认重新部署](maintainable-azure-websites-managing-change-and-scale/_static/image56.png)
15. 部署完成后，切换回浏览器实例替换为你的 web 应用和按**CTRL + F5**。
16. 单击任一选项。 翻转动画现在需要位置并将该结果 (*正确/错误*) 将显示。
17. （可选）切换到**Git Bash**控制台，然后执行以下命令以恢复为以前的提交。

    > [!NOTE]
    > 这些命令创建新的提交，取消 Git 存储库中的错误提交所做的所有更改。 然后，azure 将重新部署使用新的提交的应用程序。

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample16.cmd)]

<a id="Exercise4"></a>
### <a name="exercise-4-scaling-using-azure-storage"></a>使用 Azure 存储缩放练习 4:

**Blob**是用于存储大量非结构化的文本或二进制数据，如视频、 音频和图像的最简单方式。 移动应用程序以便对存储的静态内容，可帮助你的应用程序的缩放量提供图像或直接向浏览器的文档。

在本练习中，你将移动到 Blob 容器应用程序的静态内容。 然后你将配置应用程序以添加**ASP.NET URL 重写规则**中**Web.config**将你的内容重定向到 Blob 容器。

<a id="Ex4Task1"></a>
#### <a name="task-1--creating-an-azure-storage-account"></a>任务 1 – 创建 Azure 存储帐户

在此任务中，您将学习如何创建新的存储帐户使用管理门户。

1. 导航到[Azure 管理门户](https://manage.windowsazure.com)并使用与你的订阅关联的 Microsoft 帐户登录。
2. 选择**新 |数据服务 |存储 |快速创建**若要开始创建新的存储帐户。 输入该帐户并选择的唯一名称**区域**从列表中。 单击**创建存储帐户**以继续。

    ![创建新的存储帐户](maintainable-azure-websites-managing-change-and-scale/_static/image57.png "创建新的存储帐户")

    *创建新的存储帐户*
3. 在**存储**部分中，等到新存储帐户的状态更改为*联机*以便继续以下步骤。

    ![创建存储帐户](maintainable-azure-websites-managing-change-and-scale/_static/image58.png "创建存储帐户")

    *创建存储帐户*
4. 单击存储帐户名，然后单击**仪表板**页顶部的链接。 **仪表板**页为您提供的帐户和可以在你的应用程序一起使用的服务终结点的状态信息。

    ![显示存储帐户仪表板](maintainable-azure-websites-managing-change-and-scale/_static/image59.png "显示存储帐户仪表板")

    *显示存储帐户仪表板*
5. 单击**管理访问密钥**导航栏中的按钮。

    ![管理访问密钥按钮](maintainable-azure-websites-managing-change-and-scale/_static/image60.png "管理访问密钥按钮")

    *管理访问密钥按钮*
6. 在**管理访问密钥**对话框中，复制**存储帐户名称**和**主访问密钥**因为您将在以下练习中需要它们。 然后，关闭对话框。

    ![管理访问密钥对话框](maintainable-azure-websites-managing-change-and-scale/_static/image61.png "管理访问密钥对话框")

    *管理访问密钥对话框*

<a id="Ex4Task2"></a>
#### <a name="task-2--uploading-an-asset-to-azure-blob-storage"></a>任务 2 – 将资产上载到 Azure Blob 存储

在此任务中，你将使用 Visual Studio 中的服务器资源管理器窗口连接到你的存储帐户。 然后，将创建 blob 容器，并将具有极客 Quiz 徽标的文件上载到容器。

1. 切换到 Visual Studio 实例与**GeekQuiz**从上一练习的解决方案。
2. 从菜单栏中，选择**视图**，然后单击**服务器资源管理器**。
3. 在**服务器资源管理器**，右键单击**Azure**节点，然后选择**连接到 Azure...**.使用与你的订阅关联的 Microsoft 帐户登录。

    ![连接到 Windows Azure](maintainable-azure-websites-managing-change-and-scale/_static/image62.png)

    *连接到 Azure*
4. 展开**Azure**节点，右键单击**存储**和选择**附加外部存储...**.
5. 在**添加新的存储帐户**对话框框中，输入**帐户名称**和**帐户密钥**获取在上一任务和单击**确定**.

    ![添加新的存储帐户对话框](maintainable-azure-websites-managing-change-and-scale/_static/image63.png)

    *添加新的存储帐户对话框*
6. 你的存储帐户应显示在**存储**节点。 展开你的存储帐户，右键单击**Blob**和选择**创建 Blob 容器...**.

    ![创建 Blob 容器](maintainable-azure-websites-managing-change-and-scale/_static/image64.png "创建 Blob 容器")

    *创建 Blob 容器*
7. 在**创建 Blob 容器**对话框框中，输入 blob 容器的名称，单击**确定**。

    ![创建 Blob 容器对话框](maintainable-azure-websites-managing-change-and-scale/_static/image65.png "创建 Blob 容器对话框")

    *创建 Blob 容器对话框*
8. 应将新的 blob 容器添加到**Blob**节点。 更改在容器访问权限，以使容器公开。 要执行此操作，请右键单击**映像**容器，然后选择**属性**。

    ![映像容器属性](maintainable-azure-websites-managing-change-and-scale/_static/image66.png "映像容器属性")

    *映像容器属性*
9. 在**属性**窗口中，设置**公共读取访问权限**到**容器**。

    ![更改公共读取访问权限属性](maintainable-azure-websites-managing-change-and-scale/_static/image67.png "更改公共读取访问权限属性")

    *更改公共读取访问权限属性*
10. 当系统提示你是否确实要更改的公共访问属性，请单击**是**。

    ![Microsoft Visual Studio 警告](maintainable-azure-websites-managing-change-and-scale/_static/image68.png "Microsoft Visual Studio 警告")

    *Microsoft Visual Studio 警告*
11. 在**服务器资源管理器**，右键单击**映像**blob 容器，然后选择**查看 Blob 容器**。

    ![查看 Blob 容器](maintainable-azure-websites-managing-change-and-scale/_static/image69.png "查看 Blob 容器")

    *查看 Blob 容器*
12. 对图像容器应在新窗口中打开，并且没有任何项图例应会显示。 单击**上载**图标以将文件上载到 blob 容器。

    ![没有任何项的图像容器](maintainable-azure-websites-managing-change-and-scale/_static/image70.png "没有任何项的图像容器")

    *没有任何项的图像容器*
13. 在**上载 Blob**对话框框中，导航到**资产**实验室的文件夹。 选择**徽标 big.png**文件并单击**打开**。
14. 等待，直到将文件上载。 上载完成后，应在图像容器中列出该文件。 右键单击文件条目并选择**将 URL 复制**。

    ![复制 blob URL](maintainable-azure-websites-managing-change-and-scale/_static/image71.png "复制 blob 文件 URL")

    *复制 blob URL*
15. 打开 Internet Explorer 并粘贴该 URL。 下图应显示在浏览器。

    ![从 Windows Blob 存储的徽标 big.png 映像](maintainable-azure-websites-managing-change-and-scale/_static/image72.png "徽标 big.png 图像从存储")

    *从 Azure Blob 存储的徽标 big.png 映像*

<a id="Ex4Task3"></a>
#### <a name="task-3--updating-the-solution-to-consume-static-content-from-azure-blob-storage"></a>任务 3 – 更新解决方案以使用 Azure Blob 存储中的静态内容

在此任务中，您将配置**GeekQuiz**解决方案来使用该映像上载到 Azure Blob 存储 （而不是位于在 web 应用中的映像） 通过添加在 ASP.NET URL 重写规则**web.config**文件。

1. 在 Visual Studio 中，打开**Web.config**文件**GeekQuiz**项目，然后找到 **&lt;system.webServer&gt;** 元素。
2. 添加以下代码以添加一个 URL 重写规则，更新将占位符替换为你的存储帐户名称。

    (代码段- *WebSitesInProduction-Ex4-UrlRewriteRule*)

    [!code-xml[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample17.xml)]

    > [!NOTE]
    > URL 重写是拦截传入 Web 请求，并将请求重定向到不同的资源的过程。 URL 重写规则告知重写引擎请求时需要重定向，并在其中应它们重定向。 重写规则两个字符串组成： 要在请求 URL 中查找的模式 （通常情况下，使用正则表达式），并找到要替换的模式，如果的字符串。 有关详细信息，请参阅[URL 重写在 ASP.NET 中](https://msdn.microsoft.com/library/ms972974.aspx)。
3. 按**CTRL + S**以保存所做的更改。
4. 打开一个新**Git Bash**控制台部署到 Azure App Service 更新的应用程序。
5. 执行以下命令以将更改推送到 Azure。 更新*[您的应用程序的路径]*占位符替换为的路径**GeekQuiz**解决方案。 系统将提示您输入部署密码。

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample18.cmd)]

    ![将更新部署到 Azure](maintainable-azure-websites-managing-change-and-scale/_static/image73.png)

    *将更新部署到 Azure*

<a id="Ex4Task4"></a>
#### <a name="task-4--verification"></a>任务 4 – 验证

在此任务将使用**Internet Explorer**浏览**专家 Quiz**应用程序并检查 URL 重写规则映像有效并且你将重定向到在上托管的映像**Azure Blob存储**。

1. 打开 Internet Explorer 并导航到你的 web 应用 (例如`http://<your-web-site>.azurewebsites.net`)。 使用以前创建的凭据登录。

    ![显示与映像专家 Quiz web 应用](maintainable-azure-websites-managing-change-and-scale/_static/image74.png "显示与映像专家 Quiz web 应用")

    *显示与映像专家 Quiz web 应用*
2. 按**F12**若要启动的开发工具，选择**网络**选项卡上，并开始记录。

    ![启动网络录制](maintainable-azure-websites-managing-change-and-scale/_static/image75.png "启动网络录制")

    *启动网络录制*
3. 按**CTRL + F5**刷新网页。
4. 一旦页面完成加载，你应看到的 HTTP 请求**/img/logo-big.png**带有 HTTP URL **301**结果 （重定向） 和另一请求`http://[YOUR-STORAGE-ACCOUNT].blob.core.windows.net/images/logo-big.png`URL 使用 HTTP **200**结果。

    ![验证 URL 重定向](maintainable-azure-websites-managing-change-and-scale/_static/image76.png "开发人员工具中显示重定向")

    *验证 URL 重定向*

<a id="Exercise5"></a>
### <a name="exercise-5-using-autoscale-for-web-apps"></a>练习 5： 使用自动缩放 Web Apps

> [!NOTE]
> 本练习中是可选的因为它需要支持用于 Web 负载&amp;性能测试该功能仅适用于**Visual Studio 2013 Ultimate Edition**。 有关特定的 Visual Studio 2013 功能的详细信息，比较版本[此处](https://www.microsoft.com/visualstudio/eng/products/compare)。


**Azure App Service Web Apps** web 运行的应用程序提供的自动缩放功能**标准模式**。 自动缩放会让 Azure 自动缩放 web 应用程序根据负载的实例计数。 启用自动缩放后，Azure 将检查一次每隔五分钟的 web 应用的 CPU，并根据需要在该点及时添加实例。 如果 CPU 使用率较低，Azure 将删除实例一次每两小时以确保不降低你的 web 应用的性能。

在本练习中你将经历配置所需的步骤**自动缩放**的功能**专家 Quiz** web 应用。 通过运行 Visual Studio 负载测试，以生成应用程序以触发实例升级足够的 CPU 负载，你将验证此功能。

<a id="Ex5Task1"></a>
#### <a name="task-1--configuring-autoscale-based-on-the-cpu-metric"></a>任务 1 – 基于 CPU 指标配置自动缩放

在此任务将使用 Azure 管理门户启用在练习 2 中创建的 web 应用的自动缩放功能。

1. 在[Azure 管理门户](https://manage.windowsazure.com/)，选择**网站**单击在练习 2 中创建的 web 应用。
2. 导航到**缩放**页。 下**容量**部分中，选择**CPU**为**按指标进行缩放**配置。

    > [!NOTE]
    > 时按 CPU 进行缩放，Azure 将动态调整应用程序使用如果 CPU 使用率发生更改的实例数。

    ![选择按 CPU 缩放到](maintainable-azure-websites-managing-change-and-scale/_static/image77.png "选择自动缩放的 CPU 指标")

    *选择按 CPU 进行缩放*
3. 更改**目标 CPU**配置到**20**-**40**百分比。

    > [!NOTE]
    > 此范围表示你的 web 应用的平均 CPU 使用率。 Azure 将添加或删除实例以使你的 web 应用保持在此范围内。 中指定用于缩放的实例的最小和最大数**实例计数**配置。 高于或超过该限制，azure 将永远不会。
    > 
    > 默认值**目标 CPU**此实验室为了修改值。 通过使用小的值配置 CPU 范围，你将要增加到触发器自动缩放几率中等负载放置上应用程序时。

    ![更改目标必须介于 20 和 40%的 CPU](maintainable-azure-websites-managing-change-and-scale/_static/image78.png "更改介于 20%到 40%的目标 CPU")

    *更改目标 CPU 必须介于 20 和 40%*
4. 单击**保存**以保存所做的更改的命令栏中。

<a id="Ex5Task2"></a>
#### <a name="task-2--load-testing-with-visual-studio"></a>任务 2 – 负载测试使用 Visual Studio

现在，**自动缩放**已配置，你将创建**Web 性能测试和负载测试项目**在 Visual Studio 生成你的 web 应用一些 CPU 负载。

1. 打开**Visual Studio Ultimate 2013**和选择**文件 |新 |项目...**启动一个新的解决方案。

    ![创建新的项目](maintainable-azure-websites-managing-change-and-scale/_static/image79.png "创建新项目")

    *创建新项目*
2. 在**新项目**对话框中，选择**Web 性能测试和负载测试项目**下**Visual C# |测试**选项卡。请确保**.NET Framework 4.5**是选择，将该项目命名*WebAndLoadTestProject*，选择**位置**单击**确定**。

    ![创建新的 Web 和负载测试项目](maintainable-azure-websites-managing-change-and-scale/_static/image80.png "创建新的 Web 和负载测试项目")

    *创建新的 Web 和负载测试项目*
3. 在**WebTest1.webtest**右键单击**WebTest1**节点，然后单击**添加请求**。

    ![将请求添加到 WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image81.png "将请求添加到 WebTest1")

    *将请求添加到 WebTest1*
4. 在**属性**窗口的新的请求节点中，更新**Url**属性以指向你的 web 应用的 URL (例如 *[http://geek-quiz.azurewebsites.net/](http://geek-quiz.azurewebsites.net/)*).

    ![更改 Url 属性](maintainable-azure-websites-managing-change-and-scale/_static/image82.png "更改 Url 属性")

    *更改 Url 属性*
5. 在**WebTest1.webtest**窗口中，右键单击**WebTest1**单击**添加循环...**.

    ![向 WebTest1 中添加循环](maintainable-azure-websites-managing-change-and-scale/_static/image83.png "WebTest1 向中添加循环")

    *向 WebTest1 中添加循环*
6. 在**添加条件规则和项循环**对话框中，选择**For 循环**规则和修改以下属性。

    1. **终止值：** 1000年
    2. **上下文参数名称：**迭代器
    3. **增量值：** 1

    ![选择 For 循环规则并更新属性](maintainable-azure-websites-managing-change-and-scale/_static/image84.png "选择 For 循环规则并更新属性")

    *选择 For 循环规则并更新属性*
7. 下**循环中的项**部分中，选择你以前创建为 for 循环的第一个和最后一个项的请求。 单击“确定”  继续。

    ![选择 for 循环的第一个和最后一个项](maintainable-azure-websites-managing-change-and-scale/_static/image85.png "选择 for 循环的第一个和最后一个项目")

    *选择 for 循环的第一个和最后一个项目*
8. 在**解决方案资源管理器**，右键单击**WebAndLoadTestProject**项目中，展开**添加**菜单，然后选择**负载测试...**.

    ![将负载测试添加到 WebAndLoadTestProject 项目](maintainable-azure-websites-managing-change-and-scale/_static/image86.png "将负载测试添加到 WebAndLoadTestProject 项目")

    *将负载测试添加到 WebAndLoadTestProject 项目*
9. 在**新建负载测试向导**对话框中，单击**下一步**。

    ![新建负载测试向导](maintainable-azure-websites-managing-change-and-scale/_static/image87.png "新建负载测试向导")

    *新建负载测试向导*
10. 在**方案**页上，选择**不使用思考时间**单击**下一步**。

    ![选择不是使用思考时间](maintainable-azure-websites-managing-change-and-scale/_static/image88.png "选择不是使用思考时间")

    *选择不使用思考时间*
11. 在**负载模式**页上，请确保**常量负载**选项。 更改**用户计数**将设置为**250**用户和单击**下一步**。

    ![更改用户数为 250](maintainable-azure-websites-managing-change-and-scale/_static/image89.png "为 250 更改的用户计数")

    *为 250 更改的用户计数*
12. 在**测试组合模型**页上，选择**基于顺序测试顺序**单击**下一步**。

    ![选择测试组合模型](maintainable-azure-websites-managing-change-and-scale/_static/image90.png "选择测试组合模型")

    *选择测试组合模型*
13. 在**测试组合模型**页上，单击**添加...**要添加到组合的测试。

    ![向测试组合中添加测试](maintainable-azure-websites-managing-change-and-scale/_static/image91.png "向测试组合中添加测试")

    *向测试组合中添加测试*
14. 在**添加测试**对话框中，双击**WebTest1**若要添加到测试**选定的测试**列表。 单击“确定”  继续。

    ![添加 WebTest1 测试](maintainable-azure-websites-managing-change-and-scale/_static/image92.png "添加 WebTest1 测试")

    *添加 WebTest1 测试*
15. 返回**测试组合**页上，单击**下一步**。

    ![完成测试组合页](maintainable-azure-websites-managing-change-and-scale/_static/image93.png "完成测试组合页")

    *完成测试组合页*
16. 在**网络组合**页上，单击**下一步**。

    ![单击网络组合页中的下一个](maintainable-azure-websites-managing-change-and-scale/_static/image94.png "接着在网络组合页中的单击")

    *单击网络组合页中的下一步*
17. 在**浏览器组合**页上，选择**Internet 资源管理器 10.0**作为浏览器类型，然后单击**下一步**。

    ![选择的浏览器类型](maintainable-azure-websites-managing-change-and-scale/_static/image95.png "选择浏览器类型")

    *选择的浏览器类型*
18. 在**计数器集**页上，单击**下一步**。

    ![在计数器集页中单击下一步](maintainable-azure-websites-managing-change-and-scale/_static/image96.png "单击下一步中计数器集页")

    *在计数器集页中单击下一步*
19. 在**运行设置**页上，设置**负载测试持续时间**到**5 分钟**单击**完成**。

    ![将负载测试持续时间设置为 5 分钟](maintainable-azure-websites-managing-change-and-scale/_static/image97.png "将负载测试持续时间设置为 5 分钟")

    *将负载测试持续时间设置为 5 分钟*
20. 在**解决方案资源管理器**，双击**Local.settings**要浏览的测试设置文件。 默认情况下，Visual Studio 使用本地计算机以运行测试。

    > [!NOTE]
    > 或者，可以配置测试项目中以运行在云使用的负载测试**Visual Studio Online (VSO)**。 VSO 提供基于云的负载测试模拟更加真实地模拟负载的服务、 避免如 CPU 容量、 可用内存和网络带宽的本地环境约束。 有关使用 VSO 运行负载测试的详细信息，请参阅[本文](https://www.visualstudio.com/get-started/load-test-your-app-vs)。

    ![测试设置](maintainable-azure-websites-managing-change-and-scale/_static/image98.png)

<a id="Ex5Task3"></a>
#### <a name="task-3--autoscale-verification"></a>任务 3 – 自动缩放验证

现在，你将执行你在上一任务中创建负载测试，并请参阅你的 web 应用在负载下的行为方式。

1. 在**解决方案资源管理器**，双击**LoadTest1.loadtest**以打开负载测试。

    ![打开 LoadTest1.loadtest](maintainable-azure-websites-managing-change-and-scale/_static/image99.png "打开 LoadTest1.loadtest")

    *打开 LoadTest1.loadtest*
2. 在**LoadTest1.loadtest**窗口中，单击工具箱以运行负载测试中的第一个按钮。

    ![运行负载测试](maintainable-azure-websites-managing-change-and-scale/_static/image100.png "运行负载测试")

    *运行负载测试*
3. 等待，直到负载测试完成。

    > [!NOTE]
    > 负载测试模拟多个同时将请求发送到 web 应用的用户。 当运行测试时，你可以监视可用的计数器，以检测任何错误、 警告或向负载测试运行相关的其他信息。

    ![负载测试运行](maintainable-azure-websites-managing-change-and-scale/_static/image101.png "等待在完成负载测试")

    *负载测试运行*
4. 测试完成后，请返回到管理门户并导航到**缩放**的 web 应用程序页。 下**容量**部分中，你应在图中查看自动部署的新实例。

    ![自动部署的新实例](maintainable-azure-websites-managing-change-and-scale/_static/image102.png)

    *自动部署的新实例*

    > [!NOTE]
    > 它可能需要几分钟才会显示在关系图的更改 (按**CTRL + F5**定期以刷新页面)。 如果看不到任何更改，你可以尝试以下操作：
    > 
    > - 增加的负载测试的持续时间 (例如到**10 分钟**)
    > - 减少的最大和最小值**目标 CPU**你的 web 应用的自动缩放配置中的范围
    > - 在与云中运行负载测试**Visual Studio Online**。 详细信息[此处](https://www.visualstudio.com/get-started/load-test-your-app-vs.aspx)

* * *

<a id="Summary"></a>
## <a name="summary"></a>摘要

在本动手实验，您学习了如何设置和应用程序部署到 Azure 中的生产 web 应用。 通过检测并更新数据库使用启动**Entity Framework Code First 迁移**，然后继续通过部署你的站点使用的新版本**Git**和执行回退到你的站点的最新稳定版本。 此外，您学习了如何缩放应用程序使用存储将静态内容移到 Blob 容器。
