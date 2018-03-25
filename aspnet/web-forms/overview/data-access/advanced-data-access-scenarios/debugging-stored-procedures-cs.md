---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/debugging-stored-procedures-cs
title: "调试存储的过程 (C#) |Microsoft 文档"
author: rick-anderson
description: "Visual Studio Professional 和 Team System 的版本，可以设置断点并单步执行到 SQL Server 中的存储过程进行调试存储..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: c655c324-2ffa-4c21-8265-a254d79a693d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/debugging-stored-procedures-cs
msc.type: authoredcontent
ms.openlocfilehash: 3c5f797691a6920c65db7e3906aa5fd3b348b54b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="debugging-stored-procedures-c"></a>调试存储的过程 (C#)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载代码](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_74_CS.zip)或[下载 PDF](debugging-stored-procedures-cs/_static/datatutorial74cs1.pdf)

> Visual Studio Professional 和 Team System 的版本，可以设置断点并单步执行到 SQL Server 中的存储过程进行调试简单地调试应用程序代码的存储的过程。 本教程演示了直接数据库调试和存储过程的调试应用程序。


## <a name="introduction"></a>介绍

Visual Studio 提供了丰富的调试体验。 与几击键或鼠标，只需点击下它可以使用断点停止执行程序，并检查其状态和控制流的 s。 调试应用程序代码，以及 Visual Studio 提供了对调试从 SQL Server 中的存储的过程的支持。 就像可以在 ASP.NET 代码隐藏类或业务逻辑层类的代码中设置断点一样以便太可以将它们放在存储过程。

在本教程中我们将查看从单步执行存储过程在 Visual Studio 中的服务器资源管理器以及如何设置命中的断点时调用存储的过程从正在运行的 ASP.NET 应用程序。

> [!NOTE]
> 遗憾的是，存储的过程只能单步执行和调试通过 Visual Studio 的专业人员和团队系统版本。 如果你使用 Visual Web Developer 或标准版本的 Visual Studio，你将欢迎沿中读取，因为我们逐步完成各步骤有必要调试存储的过程，但不是会复制在您的计算机上的这些步骤。


## <a name="sql-server-debugging-concepts"></a>SQL Server 调试概念

Microsoft SQL Server 2005 旨在提供与集成[公共语言运行时 (CLR)](https://msdn.microsoft.com/netframework/aa497266.aspx)，这是运行时使用的所有.NET 程序集。 因此，SQL Server 2005 支持托管的数据库对象。 也就是说，你可以创建数据库对象，如存储的过程和用户定义函数 (Udf) 作为 C# 类中的方法。 这样，这些存储的过程和 Udf 利用.NET Framework 中和从您自己的自定义类的功能。 当然，SQL Server 2005 还提供对支持 T-SQL 的数据库对象。

SQL Server 2005 提供调试支持 T-SQL 和托管的数据库对象。 但是，这些对象，仅通过 Visual Studio 2005 专业版和团队系统版本进行调试。 在本教程中我们将检查调试 T-SQL 的数据库对象。 调试托管的数据库对象考察后续的教程。

[概述的 T-SQL 和 CLR 调试 SQL Server 2005 中](https://blogs.msdn.com/sqlclr/archive/2006/06/29/651644.aspx)从博客文章[SQL Server 2005 CLR 集成团队](https://blogs.msdn.com/sqlclr/default.aspx)突出显示调试从 Visual Studio 的 SQL Server 2005 对象的三种方法：

- **直接数据库调试 (DDD)** -我们可以单步执行任何 T-SQL 的数据库对象，如存储的过程和 Udf 的服务器资源管理器。 我们将检查在步骤 1 中的 DDD。
- **应用程序调试**-我们可以设置的数据库对象中的断点，然后运行 ASP.NET 应用程序。 当执行数据库对象时，将会命中断点和控制移交到调试器。 请注意，与应用程序调试我们无法单步执行的数据库对象从应用程序代码。 我们必须显式设置断点在这些存储的过程或 Udf 我们希望调试器停止的位置。 应用程序调试检查在步骤 2 中启动。
- **从 SQL Server 项目调试**-Visual Studio Professional 和团队系统版本包含通常用于创建托管的数据库对象的 SQL Server 项目类型。 我们将检查使用 SQL Server 项目和在下一教程中调试其内容。

Visual Studio 可以调试在本地和远程 SQL Server 实例上的存储的过程。 本地 SQL Server 实例是指在 Visual Studio 的同一台计算机上安装。 如果你使用的 SQL Server 数据库未位于开发计算机上，然后认为是远程实例。 在本系列教程我们一直在使用本地 SQL Server 实例。 调试在远程 SQL server 实例上的存储的过程需要比时调试本地实例上的存储的过程的详细配置步骤。

如果你使用的本地的 SQL Server 实例，可以从步骤 1 开始，并且要完成本教程到末尾。 如果你使用的远程 SQL Server 实例，但是，你将首先需要确保你在调试时登录到你的开发计算机进行远程实例具有 SQL Server 登录名的 Windows 用户帐户。 Moveover，此数据库登录名和用于从运行的 ASP.NET 应用程序连接到数据库的数据库登录名必须是的成员`sysadmin`角色。 请参阅在远程实例部分上的调试 T-SQL 的数据库对象在本教程针对配置 Visual Studio 和 SQL Server，若要调试的远程实例的详细信息的末尾。

最后，了解，调试支持 T-SQL 的数据库对象不作为调试.NET 应用程序的支持丰富的功能。 例如，断点条件和筛选器不支持，只有一部分调试窗口可用，您无法使用编辑并继续，即时窗口呈现得毫无意义，等等。 请参阅[调试器命令和功能的限制](https://msdn.microsoft.com/library/ms165035(VS.80).aspx)有关详细信息。

## <a name="step-1-directly-stepping-into-a-stored-procedure"></a>步骤 1： 直接单步执行存储过程

Visual Studio 便于直接调试数据库对象。 让我们来探讨如何使用直接数据库调试 (DDD) 功能来单步执行`Products_SelectByCategoryID`Northwind 数据库中存储过程。 顾名思义，`Products_SelectByCategoryID`返回产品信息的特定类别; 它中创建[使用现有存储过程类型化数据集 s Tableadapter](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)教程。 通过转到服务器资源管理器启动并展开 Northwind 数据库节点。 接下来，向下钻取的存储过程文件夹，右键单击`Products_SelectByCategoryID`存储过程，并从上下文菜单中选择到存储过程的步骤选项。 这将启动调试器。

由于`Products_SelectByCategoryID`存储的过程希望`@CategoryID`输入的参数，我们会要求提供此值。 输入 1，这将返回有关饮料信息。


![使用值 1@CategoryID参数](debugging-stored-procedures-cs/_static/image1.png)

**图 1**： 使用值 1`@CategoryID`参数


提供的值后`@CategoryID`参数，该存储过程将会执行。 不过，而不是运行完成，调试器暂停执行第一个语句上。 请注意，该值指示当前存储过程中的位置边距中的黄色箭头。 你可以查看和编辑通过监视窗口或通过将鼠标悬停在存储过程中的参数名称的参数值。


[![调试器已停止的存储过程的第一个语句上](debugging-stored-procedures-cs/_static/image3.png)](debugging-stored-procedures-cs/_static/image2.png)

**图 2**： 的调试器已停止的存储过程的第一个语句上 ([单击以查看实际尺寸的图像](debugging-stored-procedures-cs/_static/image4.png))


若要单步执行存储的过程一次的一个语句，请单击工具栏中的逐过程按钮或按 F10 键。 `Products_SelectByCategoryID`存储的过程包含一个`SELECT`语句，以便达到 F10 将逐过程执行单个语句并完成存储过程的执行。 存储的过程完成后，其输出会在输出窗口中，调试器将终止。

> [!NOTE]
> T-SQL 调试发生在语句级;你不能将单步执行`SELECT`语句。


## <a name="step-2-configuring-the-website-for-application-debugging"></a>步骤 2： 配置的网站，应用程序调试

虽然调试直接从服务器资源管理器存储的过程是很方便，在许多情况下我们更加感兴趣调用从我们的 ASP.NET 应用程序时调试存储的过程。 我们可以将断点添加到存储过程从 Visual Studio 中，然后开始调试 ASP.NET 应用程序。 具有断点的存储的过程调用时从应用程序，执行将在断点处暂停和我们可以查看和更改存储的过程的参数值和逐步执行其语句，，就像我们未在步骤 1 中一样。

我们可以开始调试调用应用程序中的存储的过程之前，我们需要指示 ASP.NET web 应用程序将与 SQL Server 调试器集成。 通过右键单击解决方案资源管理器中的网站名称开始 (`ASPNET_Data_Tutorial_74_CS`)。 从上下文菜单中选择属性页选项选择的启动选项项在左侧，并选中调试器部分中的 SQL Server 复选框 （请参见图 3）。


[![检查应用程序的属性页中的 SQL Server 复选框](debugging-stored-procedures-cs/_static/image6.png)](debugging-stored-procedures-cs/_static/image5.png)

**图 3**： 检查应用程序的属性页中的 SQL Server 复选框 ([单击以查看实际尺寸的图像](debugging-stored-procedures-cs/_static/image7.png))


此外，我们需要更新应用程序，以便禁用连接池使用的数据库连接字符串。 关闭与数据库连接时，相应`SqlConnection`对象放置在池中的可用连接。 当建立到数据库的连接，可用的连接对象可以从该池中检索而无需编写创建并建立新连接。 连接对象的此池是一种性能增强并默认处于启用状态。 但是，在调试时我们想要关闭连接池，因为调试基础结构不正确地重新建立使用从池中提取连接时。

若要与已禁用的连接池，更新`NORTHWNDConnectionString`中`Web.config`以使其包括设置`Pooling=false`。


[!code-xml[Main](debugging-stored-procedures-cs/samples/sample1.xml)]

> [!NOTE]
> 完成后调试 ASP.NET 应用程序通过 SQL Server 请务必恢复连接池通过删除`Pooling`从连接字符串设置 (或通过将它设置为`Pooling=true`)。


在此时配置 ASP.NET 应用程序若要允许 Visual Studio 调试时通过 web 应用程序调用的 SQL Server 数据库对象。 现在剩下的是以将断点添加到存储过程并开始调试 ！

## <a name="step-3-adding-a-breakpoint-and-debugging"></a>步骤 3： 添加断点和调试

打开`Products_SelectByCategoryID`存储过程并的开头设置断点`SELECT`语句的适当位置的边距中单击或通过将光标放在开始`SELECT`语句并按 F9。 如图 4 所示，断点显示为边距中该一个红圈。


[![Products_SelectByCategoryID 中设置断点的存储过程](debugging-stored-procedures-cs/_static/image9.png)](debugging-stored-procedures-cs/_static/image8.png)

**图 4**： 中设置断点`Products_SelectByCategoryID`存储过程 ([单击以查看实际尺寸的图像](debugging-stored-procedures-cs/_static/image10.png))


为了使调试通过客户端应用程序的 SQL 数据库对象，则必须将数据库配置为支持应用程序调试。 当第一次设置了断点时，此设置应自动打开，但请仔细检查比较明智的做法。 右键单击`NORTHWND.MDF`服务器资源管理器中的节点。 上下文菜单应包括检查的菜单项名为应用程序调试。


![请确保启用了应用程序调试选项](debugging-stored-procedures-cs/_static/image11.png)

**图 5**： 请确保启用了应用程序调试选项


设置断点并启用了应用程序调试选项，我们现在可以调试存储的过程时从 ASP.NET 应用程序调用。 通过转到调试菜单启动调试器并选择启动调试，通过点击 F5，或通过单击绿色工具栏中播放图标。 这将启动调试器并启动该网站。

`Products_SelectByCategoryID`存储的过程的创建中[使用现有存储过程类型化数据集 s Tableadapter](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)教程。 其相应的网页 (`~/AdvancedDAL/ExistingSprocs.aspx`) 包含一个 GridView，显示此存储过程返回的结果。 访问此页面通过浏览器。 在到达页上中的断点`Products_SelectByCategoryID`存储的过程将被命中，并且控件返回到 Visual Studio。 就像在步骤 1 中，你可以单步执行该存储的过程的语句并查看和修改参数值。


[![ExistingSprocs.aspx 页最初显示饮料](debugging-stored-procedures-cs/_static/image13.png)](debugging-stored-procedures-cs/_static/image12.png)

**图 6**:`ExistingSprocs.aspx`页最初显示饮料 ([单击以查看实际尺寸的图像](debugging-stored-procedures-cs/_static/image14.png))


[![存储过程 s 已达到断点](debugging-stored-procedures-cs/_static/image16.png)](debugging-stored-procedures-cs/_static/image15.png)

**图 7**： 到达断点的存储过程 s ([单击以查看实际尺寸的图像](debugging-stored-procedures-cs/_static/image17.png))


图 7 所示的值中的监视窗口为`@CategoryID`参数为 1。 这是因为`ExistingSprocs.aspx`页最初显示具有饮料类别中的产品`CategoryID`值为 1。 从下拉列表中选择一个不同的类别。 这样做会导致回发并重新执行`Products_SelectByCategoryID`存储过程。 同样，但这次命中断点`@CategoryID`参数的值反映了选择的下拉列表项的`CategoryID`。


[![从下拉列表中选择不同的类别](debugging-stored-procedures-cs/_static/image19.png)](debugging-stored-procedures-cs/_static/image18.png)

**图 8**： 从下拉列表中选择不同的类别 ([单击以查看实际尺寸的图像](debugging-stored-procedures-cs/_static/image20.png))


[![@CategoryID参数反映从 Web 页所选类别](debugging-stored-procedures-cs/_static/image22.png)](debugging-stored-procedures-cs/_static/image21.png)

**图 9**:`@CategoryID`参数反映从 Web 页类别选择 ([单击以查看实际尺寸的图像](debugging-stored-procedures-cs/_static/image23.png))


> [!NOTE]
> 如果中的断点`Products_SelectByCategoryID`访问时，则不会命中存储的过程`ExistingSprocs.aspx`页上，请确保 SQL Server 复选框已 ASP.NET 应用程序 s 属性页的调试器部分中，确保连接池已禁用和启用数据库 s 应用程序调试选项。 如果您重新仍遇到问题，重新启动 Visual Studio，然后重试。


## <a name="debugging-t-sql-database-objects-on-remote-instances"></a>调试在远程实例上的 T-SQL 的数据库对象

Visual Studio 的同一台计算机上的 SQL Server 数据库实例时，调试通过 Visual Studio 的数据库对象将非常简单。 但是，如果 SQL Server 和 Visual Studio 驻留在不同的计算机然后某些经过慎重的配置需要获得一切正常工作。 有两个我们面对的核心任务：

- 请确保用于连接到通过 ADO.NET 数据库的登录名属于`sysadmin`角色。
- 确保在开发计算机上使用 Visual Studio 的 Windows 用户帐户是有效的 SQL Server 登录帐户属于`sysadmin`角色。

第一步是相对比较简单。 首先，请标识用于从 ASP.NET 应用程序连接到数据库，然后，从 SQL Server Management Studio，添加到该登录帐户的用户帐户`sysadmin`角色。

第二个任务要求你使用调试应用程序的 Windows 用户帐户是远程数据库上的有效登录。 但是，很有您登录到你工作站的 Windows 帐户不是 SQL Server 上的有效登录。 而不是将您的特定的登录帐户添加到 SQL Server，是更好的选择将一些 Windows 用户帐户指定为 SQL Server 调试帐户。 然后，若要调试的远程 SQL Server 实例的数据库对象，将运行 Visual Studio 中使用该 Windows 登录帐户的凭据。

示例可以帮助阐明事项。 假设是一个名为的 Windows 帐户`SQLDebug`Windows 域内。 此帐户需要作为有效的登录名的成员以及要添加到远程 SQL Server 实例`sysadmin`角色。 然后，若要调试从 Visual Studio 的远程 SQL Server 实例，我们将需要运行 Visual Studio 作为`SQLDebug`用户。 这可以通过我们的工作站，超出的日志记录返回作为日志记录`SQLDebug`，然后启动 Visual Studio 中，但更简单的方法将登录到我们的工作站使用我们自己的凭据，然后使用`runas.exe`若要启动作为 Visual Studio`SQLDebug`用户。 `runas.exe`允许特定的应用程序的其他用户帐户在伪装下执行。 若要启动 Visual Studio 作为`SQLDebug`，您可以输入以下语句在命令行：


[!code-console[Main](debugging-stored-procedures-cs/samples/sample2.cmd)]

有关此过程的更多详细说明，请参阅[William。 Vaughn](http://betav.com/BLOG/billva/) s *Hitchhiker s 到 Visual Studio 和 SQL Server，第七个版本的指南*以及[How To： 设置 SQL Server 权限用于调试](https://msdn.microsoft.com/library/w1bhybwz(VS.80).aspx)。

> [!NOTE]
> 如果你的开发计算机正在运行 Windows XP Service Pack 2 将需要 Internet 连接防火墙配置为允许远程调试。 [如何为： 启用 SQL Server 2005 调试](https://msdn.microsoft.com/library/s0fk6z6e(VS.80).aspx)文章说明这涉及到两个步骤: （a） 在 Visual Studio 主机计算机，则必须添加`Devenv.exe`到例外列表并打开 TCP 135 端口; 以及 （b） 上远程的 (SQL) 计算机，则必须打开TCP 135 端口，并添加`sqlservr.exe`到例外列表。 如果你的域策略要求通过 IPSec 进行网络通信，则必须打开 UDP 4500 和 UDP 500 端口。


## <a name="summary"></a>摘要

除了提供调试支持.NET 应用程序代码，Visual Studio 还提供了各种 SQL Server 2005 的调试选项。 在本教程中我们讨论这些选项的两名： 直接数据库调试和应用程序调试。 若要直接调试 T-SQL 的数据库对象，找到对象，通过服务器资源管理器，然后右键单击它并选择单步执行。 这将启动调试器并在数据库对象，此时你可以单步执行该对象的语句并查看和修改参数值中的第一个语句上暂停。 在步骤 1 中我们使用此方法进入并单步`Products_SelectByCategoryID`存储过程。

应用程序调试允许要直接在数据库对象中设置断点。 当从客户端应用程序 （如 ASP.NET web 应用程序） 调用具有断点的数据库对象时，程序将暂停调试器时。 应用程序调试很有用，因为它更清楚地显示哪些应用程序操作会导致要调用的特定数据库对象。 但是，它需要更多配置和比直接数据库调试的安装程序。

此外可以通过 SQL Server 项目调试数据库对象。 我们将着眼于使用 SQL Server 项目和如何使用它们来创建和调试在下一教程中管理数据库对象。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

>[!div class="step-by-step"]
[上一页](protecting-connection-strings-and-other-configuration-information-cs.md)
[下一页](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs.md)
