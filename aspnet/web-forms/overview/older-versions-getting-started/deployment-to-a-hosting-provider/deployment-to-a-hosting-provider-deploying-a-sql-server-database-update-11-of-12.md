---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
title: "部署具有 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 的 ASP.NET Web 应用程序： 部署 SQL Server 数据库更新-11 12 |Microsoft 文档"
author: tdykstra
description: "这一系列的教程演示如何部署 （发布） ASP.NET web 应用程序项目，它通过使用 Visual Stu 包含 SQL Server Compact 数据库..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 5e2bb092-cb22-4511-ad0a-22ae12dd99b3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
msc.type: authoredcontent
ms.openlocfilehash: aeec69c7373a111d30e8f32a374a9f02fb4c080a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-sql-server-database-update---11-of-12"></a>部署具有 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 的 ASP.NET Web 应用程序： 部署 SQL Server 数据库更新-11 12
====================
通过[Tom Dykstra](https://github.com/tdykstra)

[下载初学者项目](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> 这一系列的教程演示如何部署 （发布） ASP.NET web 应用程序项目，它通过使用 Visual Studio 2012 RC 或 Visual Studio Express 2012 RC for Web 中包含 SQL Server Compact 数据库。 如果你安装 Web 发布更新，也可以使用 Visual Studio 2010。 有关序列的简介，请参阅[序列中的第一个教程](deployment-to-a-hosting-provider-introduction-1-of-12.md)。
> 
> 显示部署功能后，将 Visual Studio 2012 RC 版本中推出，演示如何部署 SQL Server Compact 以外的 SQL Server 版本并演示如何将部署到 Windows Azure 网站的教程，请参阅[的 ASP.NET Web 部署使用 Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md)。


## <a name="overview"></a>概述

本教程演示如何将数据库更新部署到完整的 SQL Server 数据库。 Code First 迁移进行的更新的数据库的所有工作，因为进程在几乎等同于为 SQL Server Compact 中未[部署某一数据库更新](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)教程。

提示： 如果你收到如下错误消息，或当你完成本教程的内容不起作用，请务必检查[故障排除页](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)。

## <a name="adding-a-new-column-to-a-table"></a>将新列添加到表

在本教程的本部分中，你将使数据库更改和相应的代码更改，然后将它们在准备将它们部署到测试和生产环境中测试 Visual Studio 中。 更改涉及添加`OfficeHours`列`Instructor`实体和显示中的新信息**教师**网页。

在 ContosoUniversity.DAL 项目中，打开*Instructor.cs*并添加以下属性之间`HireDate`和`Courses`属性：

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample1.cs)]

更新初始值设定项类，以便它设定种子测试数据的新列。 打开*Migrations\Configuration.cs* ，并将开始的代码块`var instructors = new List<Instructor>`与下面的代码块，其中包括新的列：

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample2.cs)]

在 ContosoUniversity 项目中，打开*Instructors.aspx*和为办公时间结束之前添加新的模板字段`</Columns>`中第一个标记`GridView`控件：

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample3.aspx)]

生成解决方案。

打开**程序包管理器控制台**窗口中，并选择作为 ContosoUniversity.DAL**默认项目**。

输入以下命令：

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample4.ps1)]

运行应用程序并选择**教师**页。 页面需要花费比平常若要加载，因为实体框架将重新创建数据库并设定其种子使用测试数据需要更长时间。

[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>将数据库更新部署到测试环境

当你使用 Code First 迁移时，则将数据库更改部署到 SQL Server 的方法是与 SQL Server Compact 相同。 但是，你必须将测试更改发布配置文件，因为它仍设置以从 SQL Server Compact 迁移到 SQL Server。

第一步是删除你在前面的教程中创建的连接字符串转换。 不再需要这些因为你将在发布配置文件，指定连接字符串转换就像你配置之前**打包/发布 SQL**迁移到 SQL Server 的选项卡。

打开*Web.Test.config*文件并删除`connectionStrings`元素。 中的唯一剩余转换*Web.Test.config*文件可供`Environment`中的值`appSettings`元素。

现在你可以更新的发布配置文件和发布到测试环境。

打开**发布 Web**向导，并切换至**配置文件**选项卡。

选择**测试**发布配置文件。

选择**设置**选项卡。

单击**启用新的数据库发布改进**。

中的连接字符串框**SchoolContext**，输入你在中使用的相同值*Web.Test.config*以前一教程中的转换文件：

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample5.cmd)]

选择**执行 Code First 迁移 （应用程序启动时运行）**。 (在你的 Visual Studio 版本，可能标记为复选框**应用 Code First 迁移**。)

中的连接字符串框**DefaultConnection**，输入你在中使用的相同值*Web.Test.config*以前一教程中的转换文件：

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample6.cmd)]

保留**更新数据库**清除。

单击“发布” 。

Visual Studio 将代码更改部署到测试环境，并打开浏览器向 Contoso 大学主页上。

选择教师页。

当应用程序运行时此页时，它将尝试访问数据库。 Code First 迁移检查的数据库是最新状态，并查找不已尚未应用最新的迁移。 Code First 迁移应用最新的迁移，运行`Seed`方法，，然后选择页会正常运行。 你看到植入的数据的新办公时间列。

[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>将数据库更新部署到生产环境

你必须还更改生产环境的发布配置文件。 在这种情况下将删除现有配置文件，并创建一个新通过导入更新的.publishsettings 文件。 更新的文件将包括 Cytanium 上的 SQL Server 数据库的连接字符串。

正如你看到时部署到测试环境，你不再需要在连接字符串转换*Web.Production.config*转换文件。 打开文件并删除`connectionStrings`元素。 剩余的转换适用于`Environment`中的值`appSettings`元素和`location`限制的访问权限 Elmah 错误报告的元素。

在创建新的发布配置文件用于生产之前，下载更新的.publishsettings 文件相同的方式更早版本中未[将部署到生产环境](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)教程。 (在 Cytanium 控制面板中，单击**网站**，然后单击**contosouniversity.com**网站。 选择**Web 发布**选项卡上，并依次**下载发布配置文件添加为此网站**。)您这样做的原因是以拾取.publishsettings 文件中的数据库连接字符串。 连接字符串不可用因为仍在使用 SQL Server Compact 和尚未创建 SQL Server 数据库在 Cytanium 尚未下载该文件，第一次。

现在你可以更新的发布配置文件和发布到生产环境。

打开**发布 Web**向导，并切换至**配置文件**选项卡。

单击**管理配置文件**，然后删除生产配置文件。

关闭**发布 Web**向导以保存此更改。

打开**发布 Web**再次，向导，然后依次单击**导入**。

上**连接**选项卡上，更改**目标 URL**为适当的值，如果你使用的临时的 URL。

单击 **“下一步”**。

上**设置**选项卡上，单击**启用新的数据库发布改进**。

连接字符串下拉列表中**SchoolContext**，选择 Cytanium 连接字符串。

![Selecting_Cytanium_connection_string](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image5.png)

选择**执行 Code First 迁移 （应用程序启动时运行）**。

连接字符串下拉列表中**DefaultConnection**，选择 Cytanium 连接字符串。

选择**配置文件**选项卡上，单击**管理配置文件**，将配置文件从"contosouniversity.com-Web 部署"重命名为"生产"。

关闭发布配置文件以保存更改，然后再次打开它。

单击“发布” 。 (对于实际生产网站，会将复制*应用\_offline.htm*到生产并将置于在你的项目文件夹之前发布，然后将其删除部署完成后。)

Visual Studio 将代码更改部署到测试环境，并打开浏览器向 Contoso 大学主页上。

选择教师页。

Code First 迁移更新数据库相同的方式在测试环境中做过它。 你看到植入的数据的新办公时间列。

![Instructors_page_with_OfficeHours_Prod](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image6.png)

你现在已成功部署包含数据库更改，应用程序更新使用 SQL Server 数据库。

## <a name="more-information"></a>详细信息

这将完成这一系列的 ASP.NET web 应用程序部署到第三方托管提供商的教程。 有关任何这些教程中所涵盖的主题的详细信息，请参阅[ASP.NET 部署内容映射](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx)MSDN 网站上。

## <a name="acknowledgements"></a>致谢

我要特别感谢以下人员执行了巨大的内容的本系列教程的贡献：

- [Alberto Poblacion、 MVP &amp; MCT、 西班牙](https://mvp.support.microsoft.com/profile/Alberto)
- Jarod Ferguson，数据平台开发 MVP，美国
- 恶劣 Mittal，Microsoft
- [Kristina Olson, Microsoft](https://blogs.iis.net/krolson/default.aspx)
- [Mike Pope Microsoft](http://www.mikepope.com/blog/DisplayBlog.aspx)
- Mohit Srivastava Microsoft
- [Raffaele Rialdi，意大利](http://www.iamraf.net/)
- [Rick Anderson Microsoft](https://blogs.msdn.com/b/rickandy/)
- [Sayed Hashimi、 Microsoft](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))
- [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))
- [Scott 搜寻、 Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))
- [Srđan Božović、 塞尔维亚共和国](http://msforge.net/blogs/zmajcek/)
- [Vishal Joshi、 Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))

>[!div class="step-by-step"]
[上一页](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
[下一页](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)
