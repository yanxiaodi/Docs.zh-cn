---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-a-database-cs
title: "部署数据库 (C#) |Microsoft 文档"
author: rick-anderson
description: "部署 ASP.NET web 应用程序时，需要从开发环境中获取的必需的文件和资源到生产环境。 为 da..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/23/2009
ms.topic: article
ms.assetid: ff537a10-9f1f-43fe-9bcb-3dda161ba8f5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-a-database-cs
msc.type: authoredcontent
ms.openlocfilehash: f71e3cd1e81644df7b3dfed363b6f2ca826e610d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="deploying-a-database-c"></a>部署数据库 (C#)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载代码](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_07_CS.zip)或[下载 PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial07_DeployDB_cs.pdf)

> 部署 ASP.NET web 应用程序时，需要从开发环境中获取的必需的文件和资源到生产环境。 对于数据驱动的 web 应用程序中，这包括数据库架构和数据。 本教程是探讨成功部署到生产环境中开发环境的数据库所需的步骤一系列中的第一个。


## <a name="introduction"></a>介绍

部署 ASP.NET web 应用程序时，需要从开发环境中获取的必需的文件和资源到生产环境。 在过去六个教程过程中我们查看部署简单的图书评论 web 应用程序。 此演示站点已组成大量服务器端的资源-ASP.NET 页、 配置文件，`Web.sitemap`文件和等的以及客户端的资源，如图像和 CSS 文件。 但是，数据驱动的 web 应用程序？ 必须采取额外步骤来部署的 web 应用程序使用数据库？

在下一步的几个教程中，我们将解决部署数据驱动的 web 应用程序所需的步骤。 本教程通过检查如何获取数据库的架构和内容从开发环境到生产环境中，而后续教程查找所需的配置更改时启动。 以下，我们将探讨质询的部署使用应用程序服务 （成员资格、 角色、 配置文件和等等） 的数据库。

## <a name="examining-the-updated-book-reviews-web-application"></a>检查更新的簿评论 Web 应用程序

为了演示部署数据驱动的 web 应用程序，我已更新为数据驱动型从简单、 静态网站图书评论 web 应用程序。 因为在这之前，有两个版本的应用程序，该教程 s 下载内容中： 即使用 Web 应用程序项目模型，另一个使用网站项目模型。

更新的图书评论 web 应用程序使用[SQL Server 2008 Express Edition](https://www.microsoft.com/express/sql/default.aspx)数据库，存储在站点 s`App_Data`文件夹 (`~/App_Data/Reviews.mdf`)。 如果必须在计算机上安装的 SQL Server 2008 演示应运行的。 如果你有较旧版本的 SQL Server 可以安装免费 SQL Server 2008 Express Edition，或者可以使用脚本在本教程中 s 可用下载自行创建数据库的数据库。

`Reviews.mdf`数据库包含四个表：

- `Genres`-包括每个风格，如技术、 虚构和业务的记录。
- `Books`-包括每个查看具有类似的列的记录`Title`， `GenreId`， `ReviewDate`，和`Review`，等等。
- `Authors`-包含有关每个作者，但有贡献的已经过评审书的信息。
- `BooksAuthors`-指定哪些作者在编写完哪些丛书的多对多联接表。
  

图 1 显示以下四个表的紧急关系图。


[![簿评审 Web 应用程序的数据库是包含的四个表](deploying-a-database-cs/_static/image2.jpg)](deploying-a-database-cs/_static/image1.jpg) 

**图 1**: 簿评审 Web 应用程序的数据库是包含的四个表 ([单击以查看实际尺寸的图像](deploying-a-database-cs/_static/image3.jpg))


以前版本的图书评论网站具有单独的 ASP.NET 页的每本书。 例如，没有一个名为页`~/Tech/TYASP35.aspx`包含的评审*教授自己 ASP.NET 3.5 24 小时内*。 此新数据驱动版本的网站有存储在数据库中并将一个 ASP.NET 页面 Review.aspx?ID= 评审*bookId*，它会显示对指定的书籍的审查。 同样，没有 Genre.aspx?ID=*genreId*页，其中列出在指定的流派的已经过评审的书籍。

图 2 和 3 显示`Genre.aspx`和`Review.aspx`操作中的页。 请注意每一页的地址栏中的 URL。 在图 2 it s Genre.aspx？ ID = 85d164ba-1123年-4 c 的 47-82a0-c8ec75de7e0e。 因为 85d164ba-1123-4c47-82a0-c8ec75de7e0e`GenreId`技术 genre、"技术查看"页的标题读取和项目符号列表的值枚举属于这种类型站点上的这些评审。


[![技术流派页面](deploying-a-database-cs/_static/image5.jpg)](deploying-a-database-cs/_static/image4.jpg) 

**图 2**: 技术流派页面 ([单击以查看实际尺寸的图像](deploying-a-database-cs/_static/image6.jpg))


[![评审也自学 ASP.NET 3.5 在 24 小时](deploying-a-database-cs/_static/image8.jpg)](deploying-a-database-cs/_static/image7.jpg) 

**图 3**: 评审*教授自己 ASP.NET 3.5 24 小时内*([单击以查看实际尺寸的图像](deploying-a-database-cs/_static/image9.jpg))


簿查看 web 应用程序还包括管理部分，管理员可以添加、 编辑和删除风格，评审和创作信息。 当前，任何访问者可以访问管理部分。 在将来的教程中我们添加对用户帐户的支持，仅允许到管理页的授权的用户。

如果你下载簿评审应用程序请注意，其目的是演示如何部署数据驱动的应用程序。 它不会出现与应用程序设计的最佳做法。 例如，没有任何单独数据访问层 (DAL);ASP.NET 页直接与通过 SqlDataSource 控件或其代码隐藏类中的 ADO.NET 代码数据库通信。 有关在创建数据驱动的应用程序使用分层体系结构的更深入探讨，请参阅我[*使用数据*教程](../../data-access/index.md)。

## <a name="databases-on-development-versus-production"></a>在与生产开发的数据库

当你启动开发对数据驱动的 web 应用程序时必须指定数据库连接字符串，提供有关如何连接到数据库的应用程序详细信息。 此连接字符串指定其他操作，数据库服务器、 数据库名称和安全信息。 大多数情况下，在开发过程中应用程序使用的数据库是不同于数据库时使用它在生产环境中的 s。 有很多好处的开发与生产中使用不同的数据库。 具有不同的数据库中开发意味着不再需要担心意外修改或删除实时数据。 此外可以使用该文件放在 dummy 测试数据或对数据模型进行重大更改，而无需担心对生产中的应用程序的影响。 具有不同的数据库中开发和生产环境是应用程序时的不利之处部署到数据库的架构数据库和任何相关的更改，或必须将部署数据。

在首次部署之前没有数据库，只有一个实例，该实例是在开发环境中。 部署到生产环境首次应用程序时我们不仅必须复制了所需的服务器端和客户端文件，而且还将开发环境中的数据库复制到生产环境。 这是其中我们独立右，现在与图书评论 web 应用程序的数据库驻留在`App_Data`文件夹在我们的开发环境中但不是具有尚未被推送到生产环境。

部署应用程序后有两个数据库的副本。 随着应用程序的逐渐成熟，可能添加新功能，因而需要进行到数据模型更改 （如将新列添加到现有表中，进行更改的现有列，添加新表，等等）。 接下来部署 web 应用程序时，所做的更改应用于开发环境中的数据库中，由于最后一个部署必须应用到生产数据库。 将来的教程中讨论了一些策略可用于管理此过程。 本教程侧重于部署到生产环境中开发环境的整个数据库。

## <a name="deploying-the-database-to-the-production-environment"></a>将数据库部署到生产环境

本教程的其余部分讨论如何部署到生产环境中开发环境的数据库。 如果你是内容的观众你需要以确保你的帐户与你的 web 宿主提供程序包括 Microsoft SQL Server 数据库支持。 你还需要某些信息手头，即数据库服务器名称、 数据库名称，以及用户名和密码用于连接到数据库。

本教程中前面所述，图书评论网站的数据库是存储在 SQL Server 2008 Express Edition 数据库`App_Data`文件夹。 它将一定会原因，部署此类数据库将是为复制一样简单`App_Data`从开发环境到生产环境的文件夹。 但是，大多数 web 宿主提供程序不支持在托管数据库`App_Data`出于安全考虑的文件夹。 相反，web 主机提供的帐户在其环境中 SQL Server 数据库服务器上。 部署到生产环境中你的开发环境的数据库，则需要获取你在你的 web 主机的数据库服务器上注册的数据库。

那么，如何获取你的数据库从开发环境到生产环境？ 有几种方式来实现此目的具体取决于您的 web 主机提供服务。 可以使用某些主机，如 DiscountASP.NET，FTP 备份数据库或实际`.mdf`文件到你的网站，然后，从控制面板中，还原备份文件或附加`.mdf`到 SQL Server 数据库服务器的文件。 与此类工具将数据库部署非常简单，只复制`App_Data`到生产环境，然后将其连接通过控制面板中的文件夹。 这可能是首次发布你的数据库的最简单且最快速方法。

另一种方法是使用数据库发布向导。 数据库发布向导为 Windows 桌面应用程序将生成要在其表中创建的表、 存储的过程、 视图、 用户定义函数等-你数据库的架构和数据，（可选） 的 SQL 命令。 你可以然后连接到你的 web 宿主提供程序的数据库服务器通过 SQL Server Management Studio，然后执行此脚本以重复生产上的数据库。 获得更好，如果你的 web 宿主提供程序支持 Microsoft s [Database Publishing Services](http://www.codeplex.com/sqlhost/Wiki/View.aspx?title=Database%20Publishing%20Services&amp;referringTitle=Home)你可以通过在数据库服务器上替你自动执行使用数据库发布向导生成的脚本。 由于数据库发布向导生成脚本创建的数据库的架构和数据，因此将工作而不考虑你的 web 主机提供商是否提供功能，如附加上载`.mdf`文件。

### <a name="generating-the-sql-commands-to-create-the-database-schema-and-data-using-the-database-publishing-wizard"></a>生成要创建的数据库架构和数据使用发布向导的数据库的 SQL 命令

让我们来指导你完成使用数据库发布向导将图书评论数据库部署到生产环境。 使用 Visual Studio 2008 或更高，已安装数据库发布向导。 如果你使用的 Visual Studio 2005 然后，你将需要第一个[下载并安装](https://www.microsoft.com/downloads/details.aspx?familyid=56E5B1C5-BF17-42E0-A410-371A838E570A&amp;displaylang=en)向导。

打开 Visual Studio 并导航到`Reviews.mdf`数据库。 如果你使用 Visual Web Developer 中，请转到数据库资源管理器;如果你使用的 Visual Studio，，使用服务器资源管理器。 图 4 显示`Reviews.mdf`在 Visual Web Developer 中的数据库资源管理器中的数据库。 如图 4 所示，`Reviews.mdf`数据库组成四个表、 三个存储的过程和用户定义函数。


[![在数据库资源管理器或服务器资源管理器中找到数据库](deploying-a-database-cs/_static/image11.jpg)](deploying-a-database-cs/_static/image10.jpg) 

**图 4**： 在数据库资源管理器或服务器资源管理器中找到数据库 ([单击以查看实际尺寸的图像](deploying-a-database-cs/_static/image12.jpg))


右键单击数据库名称，然后从上下文菜单中选择"发布到提供程序"选项。 这将启动数据库发布向导 （请参见图 5）。 单击旁边高级过去的初始屏幕。


[![数据库发布向导初始屏幕](deploying-a-database-cs/_static/image14.jpg)](deploying-a-database-cs/_static/image13.jpg) 

**图 5**: 数据库发布向导初始屏幕 ([单击以查看实际尺寸的图像](deploying-a-database-cs/_static/image15.jpg))


向导中的第二个屏幕列出供数据库发布向导的数据库，并允许你选择是否在所选数据库中的所有对象的脚本，或选择编写脚本的对象。 选择相应的数据库并将"所有的脚本在所选数据库对象"选项处于选中状态。

> [!NOTE]
> 如果你将收到错误"数据库中没有任何对象*databaseName*通过此向导可编脚本的类型"时在图 6 所示的屏幕中，单击下一步，请确保您的数据库文件的路径不是很长。 已发现该错误可能发生，是否数据库文件的路径太长。


[![数据库发布向导初始屏幕](deploying-a-database-cs/_static/image17.jpg)](deploying-a-database-cs/_static/image16.jpg) 

**图 6**: 数据库发布向导初始屏幕 ([单击以查看实际尺寸的图像](deploying-a-database-cs/_static/image18.jpg))


从下一个屏幕可以生成的脚本文件或者，如果你的 web 主机支持它，数据库将直接发布到你的 web 主机提供商的数据库服务器。 如图 7 所示，我遇到写入到文件的脚本`C:\REVIEWS.MDF.sql`。


[![脚本文件到数据库或将其发布到你的 Web 宿主提供程序直接](deploying-a-database-cs/_static/image20.jpg)](deploying-a-database-cs/_static/image19.jpg) 

**图 7**： 脚本文件到数据库或将其发布到你的 Web 宿主提供程序直接 ([单击以查看实际尺寸的图像](deploying-a-database-cs/_static/image21.jpg))


后续屏幕将提示您输入了各种的脚本选项。 你可以指定脚本是否应包含 drop 语句，以删除这些现有的对象。 这将默认为 True，这样做也可以在首次部署数据库。 你还可以指定目标数据库将 SQL Server 2000、 SQL Server 2005 或 SQL Server 2008。 最后，指示是否编写脚本的架构和数据，只需数据或只为架构。 架构是数据库对象、 表、 存储的过程、 视图和等等的集合。 数据是驻留在表中的信息。

如图 8 所示，我遇到了配置为删除现有数据库对象，该向导生成的 SQL Server 2008 数据库，脚本并发布架构和数据。


[![指定发布选项](deploying-a-database-cs/_static/image23.jpg)](deploying-a-database-cs/_static/image22.jpg) 

**图 8**： 指定发布的选项 ([单击以查看实际尺寸的图像](deploying-a-database-cs/_static/image24.jpg))


最后两个屏幕汇总正准备进行，然后显示的脚本的状态的操作。 运行向导的最终结果是我们具有包含生产上创建数据库并使用相同的数据填充它与在上开发所需的 SQL 命令的脚本文件。

### <a name="executing-the-sql-commands-on-the-production-environment-database"></a>在生产环境数据库上执行的 SQL 命令

现在，我们已包含 SQL 命令来创建数据库和其数据所有剩余的脚本是在生产数据库上执行该脚本。 某些 web 主机提供商提供其控制面板，你可以在其中输入您的数据库执行 SQL 命令中的文本框。 如果您有一个非常大的脚本文件，则此选项可能不起作用 (`REVIEWS.MDF.sql`脚本文件是超过 425 KB 大小，例如)。

更好的方法是直接连接到使用 SQL Server Management Studio (SSMS) 生产数据库服务器。 如果你有非 Express Edition 的 SQL Server 计算机上安装然后你可能已安装的 SSMS。 否则，你可以[下载并安装](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)的 SQL Server Management Studio Express Edition 免费副本。

启动 SSMS 并连接到你 web 主机的数据库服务器上使用 web 主机提供商提供的信息。


[![连接到你的 Web 主机提供商的数据库服务器](deploying-a-database-cs/_static/image26.jpg)](deploying-a-database-cs/_static/image25.jpg) 

**图 9**： 连接到你的 Web 主机提供商 s 数据库服务器 ([单击以查看实际尺寸的图像](deploying-a-database-cs/_static/image27.jpg))


展开数据库选项卡并找到你的数据库。 单击工具栏上的左上角中的新建查询按钮，粘贴在从数据库发布向导中，创建的脚本文件中的 SQL 命令，然后单击执行按钮在生产数据库服务器上运行这些命令。 如果你的脚本文件是特别大它可能需要几分钟才能执行命令。


[![连接到你的 Web 主机提供商的数据库服务器](deploying-a-database-cs/_static/image29.jpg)](deploying-a-database-cs/_static/image28.jpg) 

**图 10**： 连接到你的 Web 主机提供商 s 数据库服务器 ([单击以查看实际尺寸的图像](deploying-a-database-cs/_static/image30.jpg))


该 s 就是它 ！ 此时开发数据库具有已复制到生产环境。 如果刷新 SSMS 中的数据库，你应看到新的数据库对象。 图 11 显示生产数据库的表、 存储的过程和用户定义函数，镜像上开发数据库。 并且生产数据库的表我们指示数据库发布向导，以将数据发布，因为在执行向导时具有与开发数据库的表相同的数据。 图 12 显示中的数据`Books`生产数据库上的表。


[![在生产数据库上具有已复制的数据库对象](deploying-a-database-cs/_static/image32.jpg)](deploying-a-database-cs/_static/image31.jpg) 

**图 11**: 数据库对象具有重复对生产数据库 ([单击以查看实际尺寸的图像](deploying-a-database-cs/_static/image33.jpg))


[![生产数据库包含与对开发数据库相同的数据](deploying-a-database-cs/_static/image35.jpg)](deploying-a-database-cs/_static/image34.jpg) 

**图 12**： 生产数据库开发数据库上包含相同的数据 ([单击以查看实际尺寸的图像](deploying-a-database-cs/_static/image36.jpg))


此时我们仅具有到生产环境部署开发数据库。 我们尚未查看部署 web 应用程序本身或检查哪些配置更改所需具有应用程序在生产上的使用生产数据库。 在下一教程中，我们将介绍这些问题 ！

## <a name="summary"></a>摘要

部署数据驱动的 web 应用程序要求复制到生产环境的开发期间使用的数据库。 许多 web 主机提供商提供了工具来简化部署数据库的过程。 例如，与 DiscountASP.NET 可以 FTP 数据库`.mdf`文件 （或备份），然后从控制面板将数据库附加到数据库服务器。 无论哪些功能都会正常工作的另一个选项你 web 主机提供商提供是 Microsoft 的数据库发布向导，该工具生成用于创建开发数据库的架构和数据的 SQL 命令的脚本。 后生成此脚本便可以在生产数据库上执行它。

现在，图书评论 web 应用程序的数据库位于生产我们可以部署应用程序。 但是，web 应用程序的配置信息指定数据库的连接字符串，并且该连接字符串引用开发数据库。 我们需要将站点部署到生产环境时更新此连接字符串信息。 下一教程查看这些配置差异，并演示数据驱动图书评论站点发布到生产环境所需的步骤。

尽情享受编程 ！

#### <a name="further-reading"></a>其他阅读材料

在本教程中讨论的主题的详细信息，请参阅以下资源：

- [下载 Microsoft SQL Server 数据库发布向导 1.1](https://www.microsoft.com/downloads/details.aspx?familyid=56E5B1C5-BF17-42E0-A410-371A838E570A&amp;displaylang=en)
- [下载 Microsoft SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)

>[!div class="step-by-step"]
[上一页](core-differences-between-iis-and-the-asp-net-development-server-cs.md)
[下一页](configuring-the-production-web-application-to-use-the-production-database-cs.md)
