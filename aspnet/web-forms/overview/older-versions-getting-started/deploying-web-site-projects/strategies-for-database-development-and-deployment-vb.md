---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/strategies-for-database-development-and-deployment-vb
title: "进行数据库开发和部署 (VB) 的策略 |Microsoft 文档"
author: rick-anderson
description: "部署第一次数据驱动应用程序时可以盲目地将数据库复制到生产环境的开发环境中。 B..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/23/2009
ms.topic: article
ms.assetid: 07b8905d-78ac-4252-97fb-8675b3fb0bbf
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/strategies-for-database-development-and-deployment-vb
msc.type: authoredcontent
ms.openlocfilehash: 877056dc74e0b5a64d6e0f11d63ed9f642b0a2cd
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="strategies-for-database-development-and-deployment-vb"></a>进行数据库开发和部署 (VB) 的策略
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载 PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial10_DBDevel_vb.pdf)

> 部署第一次数据驱动应用程序时可以盲目地将数据库复制到生产环境的开发环境中。 但执行直接在后续部署中的副本将覆盖进入生产数据库的任何数据。 相反，将数据库部署涉及将应用到生产数据库上次部署以来对开发数据库所做的更改。 本教程中检查这些挑战，并提供不同的策略，用于帮助 chronicling 和应用自上次部署以来对数据库进行更改。


## <a name="introduction"></a>介绍

如前面的教程中所述，部署 ASP.NET 应用程序需要从开发环境的相关的内容复制到生产环境。 部署不是一次性事件，但而是指每次发布新版本的软件或时，会发生情况的 bug 或确定并解决安全问题。 在复制 ASP.NET 页时，图像、 JavaScript 文件和其他此类文件到生产环境不需要关注与这些文件已更改自上次部署以来中。 你可以隐式的文件复制到生产环境，覆盖现有的内容。 遗憾的是，这种简单性未扩展到部署数据库。

部署第一次数据驱动应用程序时可以盲目地将数据库复制到生产环境的开发环境中。 但执行直接在后续部署中的副本将覆盖进入生产数据库的任何数据。 相反，部署数据库涉及应用*更改*对开发数据库进行自上次部署到生产数据库。 本教程中检查这些挑战，并提供不同的策略，用于帮助 chronicling 和应用自上次部署以来对数据库进行更改。

## <a name="the-challenges-of-deploying-a-database"></a>部署数据库中的难题

数据驱动的应用程序已部署第一次之前，只有一个数据库，即在开发环境中，这正是你可以部署第一次数据驱动应用程序时盲目地数据库复制中的数据库到生产环境的开发环境。 但在部署应用程序后有两个数据库的副本： 一个在开发而在生产环境中的另一个。

部署之间的开发和生产数据库可能会变得不同步。虽然生产数据库的架构保持不变，添加新功能时可能会变化开发数据库的架构。 您可能添加或删除列、 表、 视图或存储的过程。 也可能是获取添加到开发数据库的重要数据。 许多数据驱动应用程序包括使用硬编码，特定于应用程序不是用户可编辑的数据进行填充的查找表。 例如，拍卖网站可能有一个带有描述正在 auctioned 的项的条件的选项的下拉列表： 新建、 新类似、 良好，和公平。 而不是直接在下拉列表中的这些选项进行硬编码，则通常最好将它们放在数据库表。 如果在开发期间，新的名为差条件添加到表然后部署应用程序时此相同记录将需要添加到生产数据库中的查找表。

理想情况下，将数据库部署会涉及复制数据库从开发到生产环境。 但请记住，你已部署应用程序并恢复开发后，生产数据库正在使用来自真实的用户的真实数据填充。 因此，如果你打算只需将数据库复制从开发到生产在下一步的部署将覆盖生产数据库，并且会丢失其现有的数据。 最终结果是，将数据库部署归结为应用自上次部署以来到开发数据库所做的更改。

由于将数据库部署涉及自上次部署以来应用中的架构和数据，如果可能，请更改，更改的历史记录必须维护 （或在部署时确定），以便可以在生产应用这些更改。 有多种技术来管理和将更改应用到数据模型。

### <a name="defining-the-baseline"></a>定义基线

若要维护对你应用程序的 s 数据库的更改需要具有一些起始状态，向其所做的更改应用到的基线。 在极端情况起始状态，可能是没有表、 视图或存储的过程的空数据库。 此类基线产生显著更改日志中，因为它必须包括所有数据库的表、 视图和存储的过程以及在初始部署之后所做的任何更改的创建。 频谱图的另一端，你可以将基线设置为最初部署到生产环境的数据库的版本。 选择此选项时将产生多较小的更改日志，因为它只包括到以下第一次部署数据库所做的更改。 这是我更喜欢的方法。 当然你可以选择的道路方法中，多个中间定义基线初始创建数据库和数据库第一次部署时之间的某些点。

一旦选择基线考虑生成可以执行以重新创建的基线版本的 SQL 脚本。 此类脚本，使可以快速重新创建数据库的基线版本。 此功能是更大型的项目中尤其有用，其中可能有多个开发人员处理的项目或其他环境，例如测试或过渡，每个需要其自己的数据库的副本。

有多种工具可自由应用生成的基准版本的 SQL 脚本。 从 SQL Server Management Studio (SSMS) 你可以右键单击数据库，请转到任务子菜单，并选择生成脚本选项。 这将启动脚本向导中，你可以指示生成包含创建 s 对象的数据库的 SQL 命令的文件。 另一个选项是使用数据库发布向导，可以生成不仅在数据库表中创建数据库架构，但还数据的 SQL 命令。 数据库发布向导已检查详细信息中返回*部署数据库*教程。 无论你使用什么工具，你应具有最终可用于重新创建你的数据库的基线版本的脚本文件应需要出现。

## <a name="documenting-the-database-changes-in-prose"></a>记录 Prose 中的数据库更改

在开发阶段维护对数据模型的更改的日志的最简单方法是在文本中记录的更改。 例如，如果在已部署应用程序的开发过程中添加的新列`Employees`表，删除的列从`Orders`表，并添加新表 (`ProductCategories`)，你将维护的文本文件或 Microsoft Word 文档与以下历史记录：

<a id="0.8_table01"></a>


| **更改日期** | **更改的详细信息** |
| --- | --- |
| 2009-02-03: | 添加的列`DepartmentID`(`int`，NOT NULL) 到`Employees`表。 添加从外的键约束`Departments.DepartmentID`到`Employees.DepartmentID`。 |
| 2009-02-05: | 删除的列`TotalWeight`从`Orders`表。 中已捕获的数据关联`OrderDetails`记录。 |
| 2009-02-12: | 创建`ProductCategories`表。 有三列： `ProductCategoryID` (`int`， `IDENTITY`， `NOT NULL`)， `CategoryName` (`nvarchar(50)`， `NOT NULL`)，和`Active`(`bit`， `NOT NULL`)。 添加主键约束到`ProductCategoryID`，和默认值为 1 到`Active`。 |


有大量的这种方法的缺点。 对于初学者，自动化没有希望。 随时这些更改需要应用到数据库-例如，当部署该应用程序开发人员必须手动实现时每次更改，一次一个地。 此外，如果你需要重新构造使用更改日志的基线中的数据库的特定版本，这样做因此将需要越来越多时间随着日志的大小的增长。 对此方法的另一个缺点是，实现明晰度和的详细信息的每个更改日志条目的级别从左到记录更改的人员。 在多个开发人员与团队中一些可能会使比其他更多详细、 更具可读性，或更精确的条目。 此外，错误报告中也可能包含拼写错误和其他与用户相关的数据输入错误。

记录 prose 中的数据库更改的主要好处是简单。 您不 t 需要知道如何创建和更改数据库对象的 SQL 语法。 相反，你可以在文本中记录的更改，并实现它们通过 SQL Server Management Studio s 图形用户界面。

维护你更改日志中 prose，不可否认，不很好地与某些项目，例如在范围内，较大的非常复杂且获胜 t 工作具有对数据模型中，频繁的更改或涉及多个开发人员。 但是，我已经看到这种方法小、 one-man 项目具有对数据模型仅偶尔做出更改和其中单核的开发人员具有坚实的背景中没有用于创建和更改数据库对象的 SQL 语法中非常出色的工作。

> [!NOTE]
> 时更改日志中的信息，从技术上讲，直到部署时，才需要我建议你保留的更改历史记录。 但是，而不是维护单个，不断更改日志文件，请考虑让每个数据库版本不同的更改日志文件。 通常你需要把版本数据库每个部署的时间。 通过维护的更改日志的日志可以从该基线，开始重新创建任何数据库版本通过执行从版本 1 开始的更改日志脚本，你需要重新创建继续，直到你访问的版本。


## <a name="recording-the-sql-change-statements"></a>记录 SQL 更改语句

维护 prose 中的更改日志的主要缺点是自动化缺乏。 理想情况下，实现对生产数据库在部署时的数据库更改将简单地单击按钮以执行脚本，而不是无需手动执行一系列说明。 此类自动化是可能通过维护包含用于更改数据模型那些 SQL 命令的更改日志。

SQL 语法包括创建和修改各种数据库对象的语句数。 例如， [ *CREATE TABLE 语句*](https://msdn.microsoft.com/library/ms174979.aspx)、 执行时，使用指定的列和约束创建的新表。 [ *ALTER TABLE 语句*](https://msdn.microsoft.com/library/ms190273.aspx)修改现有表中，添加、 移除或修改其列或约束。 此外，还存在语句来创建、 修改和删除索引、 视图、 用户定义函数、 存储的过程、 触发器和其他数据库对象。

返回到我们前面的示例，在你添加的新列的已部署的应用程序开发期间映像`Employees`表，删除的列从`Orders`表，并添加新表 (`ProductCategories`)。 此类操作会导致具有以下 SQL 命令的更改日志文件：

[!code-sql[Main](strategies-for-database-development-and-deployment-vb/samples/sample1.sql)]

将这些更改推送到生产数据库在部署时是一次单击操作： 打开 SQL Server Management Studio、 连接到你的生产数据库、 打开新查询窗口、 粘贴的内容更改日志中，和单击执行以运行该脚本。

## <a name="using-a-comparison-tool-to-synchronize-the-data-models"></a>使用比较工具同步的数据模型

记录中 prose 数据库更改很简单，但实现所做的更改需要开发人员可以一次; 在一个生产数据库上进行每个更改记录更改 SQL 命令使在作为轻松、 快速与单击一个按钮，生产数据库上实现这些更改，但需要学习和控制的 SQL 语句和语法用于创建和更改数据库对象。 数据库比较工具需要从这两种方法的最佳和放弃所做的最差。

数据库比较工具比较架构或两个数据库的数据，并显示摘要报告显示了数据库之间的差异。 然后，只需单击一个按钮，可以生成有关同步一个或多个数据库对象的 SQL 命令。 简而言之，你可以使用数据库比较工具比较开发和生产数据库，在部署时，生成的文件，包含 SQL 命令，在执行时，会将更改应用于生产数据库的架构因此它镜像开发数据库的架构。

有多种第三方数据库比较工具提供的许多不同的供应商。 一个例子就是[ *SQL 比较*](http://www.red-gate.com/products/SQL_Compare/)，也可由[ *Red Gate Software*](http://www.red-gate.com/)。 允许 s 演练使用 SQL 比较比较和同步开发和生产数据库架构的过程。

> [!NOTE]
> 在撰写本文时 SQL 比较的当前版本是版本 7.1、 使用标准版 $395 的成本计算。 你可以遵循通过下载 14 天免费试用版。


SQL 比较启动时将打开比较项目对话框中，显示的已保存的 SQL 比较项目。 创建新项目。 这将启动项目配置向导中，提示输入的数据库的相关信息进行比较 （请参见图 1）。 输入开发和生产环境数据库的信息。


[![开发和生产数据库进行比较](strategies-for-database-development-and-deployment-vb/_static/image2.jpg)](strategies-for-database-development-and-deployment-vb/_static/image1.jpg)

**图 1**： 开发和生产数据库进行比较 ([单击以查看实际尺寸的图像](strategies-for-database-development-and-deployment-vb/_static/image3.jpg))


> [!NOTE]
> 如果你的开发环境数据库是中的 SQL Express Edition 数据库文件`App_Data`将需要在 SQL Server Express 数据库服务器中注册数据库，以便从图 1 所示的对话框中选择该网站的文件夹。 实现此目的的最简单方法是打开 SQL Server Management Studio (SSMS)，连接到 SQL Server Express 数据库服务器上，然后附加数据库。 如果你没有在计算机上安装的 SSMS 可以下载并安装免费[ *SQL Server 2008 Management Studio Basic 版本*](https://www.microsoft.com/downloads/details.aspx?FamilyId=7522A683-4CB2-454E-B908-E805E9BD4E28&amp;displaylang=en)。


除了选择数据库来进行比较，还可以指定各种比较设置从选项选项卡。你可能想要开启的一个选项是"忽略约束和索引名。 回想一下，在前面的教程，我们添加应用程序服务到开发和生产数据库的数据库对象。 如果你使用`aspnet_regsql.exe`工具在生产数据库上创建这些对象，那么你将找到 primary key 和 unique 约束名称，在开发和生产数据库之间有所不同。 因此，SQL 比较将标记的所有应用程序服务表作为不同。 你也可以将"忽略约束和索引名称"未选中状态和同步约束名称，或指示 SQL 比较忽略这些差异。

选择到数据库后比较 （和查看比较选项），单击立即比较按钮开始比较。 接下来的几秒内，通过 SQL 比较检查两个数据库的架构并生成一份有何不同。 我已特意进行了到开发数据库，以显示在 SQL 比较界面中注明此类差异了一些修改。 如图 2 所示，我遇到添加`BirthDate`列`Authors`表，删除`ISBN`列从`Books`表，并添加一个新表， `Ratings`，这为了让用户访问站点速率审阅的丛书。

> [!NOTE]
> 在本教程中所做的数据模型更改是为了演示如何使用数据库比较工具。 您不将在将来的教程在数据库中找到这些更改。


[![SQL 比较列出的开发和生产数据库之间的差异](strategies-for-database-development-and-deployment-vb/_static/image5.jpg)](strategies-for-database-development-and-deployment-vb/_static/image4.jpg)

**图 2**: SQL 比较列出开发之间的差异和生产数据库 ([单击以查看实际尺寸的图像](strategies-for-database-development-and-deployment-vb/_static/image6.jpg))


SQL 比较将分解成组的数据库对象、 快速显示哪些对象在这两个数据库中存在但不同，该对象存在于一个数据库，但不是另一个，和的对象相等。 如你所见，有两个数据库中存在但为不同的两个对象：`Authors`表，必须添加某列，与`Books`表，它具有一个删除。 存在仅在开发数据库中，即新创建的一个对象`Ratings`表。 并且有两个数据库中相同的 117 对象。

选择数据库对象将显示 SQL 差异窗口，其中显示了这些对象之间的差异。 SQL 差异窗口中，在图 2 中，底部显示突出显示，`Authors`开发数据库中的表具有`BirthDate`列，该列中找不到`Authors`生产数据库上的表。

后查看差异，并选择你想要同步的对象下, 一步是生成更新生产数据库的架构所需的 SQL 命令以匹配开发数据库。 这是通过同步向导来完成。 同步向导确认什么对象进行同步，并总结了操作计划 （请参见图 3）。 您可以立即同步数据库，或生成具有可以在方便的时候运行的 SQL 命令的脚本。


[![使用同步向导同步你的数据库架构](strategies-for-database-development-and-deployment-vb/_static/image8.jpg)](strategies-for-database-development-and-deployment-vb/_static/image7.jpg)

**图 3**： 使用同步数据库架构同步向导 ([单击以查看实际尺寸的图像](strategies-for-database-development-and-deployment-vb/_static/image9.jpg))


数据库比较工具，如 Red Gate Software 的 SQL 比较请将所做的更改应用于对生产数据库一样简单，如点，然后单击开发数据库架构。

> [!NOTE]
> SQL 比较比较和同步两个数据库*架构*。 遗憾的是，它不比较和同步两个数据库表内的数据。 Red Gate Software 确实提供了名为产品[ *SQL 数据比较*](http://www.red-gate.com/products/SQL_Data_Compare/) ，将比较，并将两个数据库间数据同步，但它是从 SQL 比较的单独产品，另一个 $395 的成本。


## <a name="taking-the-application-offline-during-deployment"></a>采用应用程序脱机在部署过程

正如我们看到在这些教程中，整个的已部署是一个包括多个步骤的过程： 将 ASP.NET 页、 母版页、 CSS 文件、 JavaScript 文件、 图像和其他所需的内容从开发环境复制到生产环境中;如果需要;，复制生产环境特定的配置信息，然后，自上次部署以来将所做的更改应用于数据模型。 根据文件的数目和数据库更改的复杂性，这些步骤需要任何位置从几秒到几分钟时间才能完成。 在此时段内的 web 应用程序变化不定，并且访问网站的用户可能会遇到错误或意外的行为。

部署网站时，最好使 web 应用程序"脱机"，直到完成部署。 脱机应用程序 （并完成部署过程后，则会将该备份） 化简为上载文件，然后将其删除。 从 ASP.NET 2.0，名为的文件的存在开始`app_offline.htm`在应用程序根目录使整个网站"脱机"。 向该站点上的 ASP.NET 页面的任何请求自动响应的内容`app_offline.htm`文件。 该文件在删除之后，应用程序重新联机。

采用应用程序脱机在部署期间，然后，非常简单，只上载`app_offline.htm`文件复制到生产环境 s 根目录开始部署过程，然后将其删除 （或将其更名为其他内容） 之前一次部署已完成。 有关此技术的详细信息请参阅 John Peterson 的文章，采用[*脱机 ASP.NET 应用程序*](http://www.15seconds.com/issue/061207.htm)。

## <a name="summary"></a>摘要

部署数据驱动应用程序的主要难题是围绕将数据库部署。 因为有两个版本的数据库在开发环境中一-以及一个在生产环境中的这些两个数据库架构可以成为同步的如在开发过程中添加新功能。 新增功能更多，因为作为正在用来自真实的用户的真实数据填充的生产数据库，你无法覆盖生产数据库具有修改的开发数据库一样部署构成应用程序 （ASP.NET 页的文件时图像文件，和等等）。 相反，将数据库部署需要实现的精确的一套自上次部署以来对生产数据库上的开发数据库所做更改。

本教程介绍了有关维护和将数据库更改的日志应用的这三种技术。 最简单方法是在文本中记录的更改。 当这种做法在生产数据库手动过程上实现这些更改时，它不用于创建和更改数据库对象需要的 SQL 命令的知识。 一个更复杂的方法，以及一个知识更有意思在更大的项目或项目中使用多个开发人员而言，是为 SQL 命令的一系列记录的更改。 这极大地 hastens 推出到目标数据库的这些更改。 可以通过使用数据库比较工具，如 Red Gate Software 的 SQL 比较实现这两种方法的最佳。

本教程到结束我们专注于将数据驱动的应用程序部署。 下一步系列教程讲述如何响应在生产环境中的错误。 我们将查看如何显示友好错误页，而是而不是黄色死亡屏幕。 并且，我们将了解如何将登录错误 s 详细信息以及如何在此类错误发生时向你发出警报。

尽情享受编程 ！

>[!div class="step-by-step"]
[上一页](configuring-a-website-that-uses-application-services-vb.md)
[下一页](displaying-a-custom-error-page-vb.md)
