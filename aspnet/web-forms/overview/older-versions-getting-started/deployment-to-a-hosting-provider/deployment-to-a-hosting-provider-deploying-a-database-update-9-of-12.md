---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
title: "部署具有 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 的 ASP.NET Web 应用程序： 部署某一数据库更新-9 12 |Microsoft 文档"
author: tdykstra
description: "这一系列的教程演示如何部署 （发布） ASP.NET web 应用程序项目，它通过使用 Visual Stu 包含 SQL Server Compact 数据库..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: a8d776af-4735-4612-87f6-9f326587f2d3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
msc.type: authoredcontent
ms.openlocfilehash: 44faca6ac2cdb9193f5c7f6b1d7f5a26e6e22248
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-database-update---9-of-12"></a>部署具有 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 的 ASP.NET Web 应用程序： 部署某一数据库更新-9 12
====================
通过[Tom Dykstra](https://github.com/tdykstra)

[下载初学者项目](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> 这一系列的教程演示如何部署 （发布） ASP.NET web 应用程序项目，它通过使用 Visual Studio 2012 RC 或 Visual Studio Express 2012 RC for Web 中包含 SQL Server Compact 数据库。 如果你安装 Web 发布更新，也可以使用 Visual Studio 2010。 有关序列的简介，请参阅[序列中的第一个教程](deployment-to-a-hosting-provider-introduction-1-of-12.md)。
> 
> 显示部署功能后，将 Visual Studio 2012 RC 版本中推出，演示如何部署 SQL Server Compact 以外的 SQL Server 版本并演示如何将部署到 Azure App Service Web Apps 的教程，请参阅[的 ASP.NET Web 部署使用 Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md)。


## <a name="overview"></a>概述

在本教程中，使数据库更改和相关的代码的更改，在 Visual Studio 中，测试所做的更改，然后将更新部署到测试和生产环境。

提示： 如果你收到如下错误消息，或当你完成本教程的内容不起作用，请务必检查[故障排除页](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)。

## <a name="adding-a-new-column-to-a-table"></a>将新列添加到表

在本部分中，你添加的出生日期列`Person`基类`Student`和`Instructor`实体。 然后你将更新，使其显示新的列显示教师数据的页。

在*ContosoUniversity.DAL*项目中，打开*Person.cs* ，并在末尾添加以下属性`Person`（应两个关闭跟在它后面的大括号） 的类：

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample1.cs)]

接下来，更新的 Seed 方法，以便其提供了值的新列。 打开*Migrations\Configuration.cs* ，并将开始的代码块`var instructors = new List<Instructor>`与下面的代码块，其中包括出生日期信息：

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample2.cs)]

在 ContosoUniversity 项目中，打开*Instructors.aspx*并添加一个新的模板字段来显示出生日期。 将其添加之间对雇佣日期和 office 分配：

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample3.aspx)]

（如果代码缩进获取同步的你可以按 CTRL K，然后选择 CTRL D 自动重新设置格式的文件。）

生成解决方案，，然后打开**程序包管理器控制台**窗口。 请确保 ContosoUniversity.DAL 仍处于选中状态作为**默认项目**。

在**程序包管理器控制台**窗口中，选择**ContosoUniversity.DAL**作为**默认项目**，然后输入以下命令：

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample4.ps1)]

此命令完成时，Visual Studio 将打开定义新的类文件`DbMIgration`类，然后在`Up`方法你可以看到创建的新列的代码。

![AddBirthDate_migration_code](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image1.png)

生成解决方案，，然后输入以下命令包含在**程序包管理器控制台**窗口 （请确保 ContosoUniversity.DAL 项目仍处于选中状态）：

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample5.ps1)]

完成该命令后，运行应用程序并选择教师页。 在页面加载后，你看到它具有新出生日期字段。

[![Instructors_page_with_birth_date](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image3.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image2.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>将数据库更新部署到测试环境

在**解决方案资源管理器**选择 ContosoUniversity 项目。

在**Web 单键发布**工具栏上，选择**测试**发布配置文件，并依次**发布 Web**。 (如果禁用了工具栏上，选择中的 ContosoUniversity 项目**解决方案资源管理器**。)

Visual Studio 将部署更新的应用程序，并浏览器打开到主页。 运行教师页后，可以验证更新成功部署。 当应用程序尝试访问此页的数据库时，Code First，更新数据库架构，并运行`Seed`方法。 显示页时，你看到预期**出生日期**与在其中的日期的列。

[![Instructors_page_with_birth_date_Test](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image4.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>将数据库更新部署到生产环境

你现在可以部署到生产环境。 唯一的区别是，你将使用*应用\_offline.htm*来防止用户访问该站点并因此更新数据库，而你要部署的更改。 对于生产部署执行以下步骤：

- 上载*应用\_offline.htm*到生产站点的文件。
- 在 Visual Studio 中，选择中的生产配置文件**Web 单键发布**工具栏，然后单击**发布 Web**。
- 删除*应用\_offline.htm*生产站点中的文件。

> [!NOTE]
> 在生产环境中使用你的应用程序时你应实施备份计划。 也就是说，你应将定期复制*学校 Prod.sdf*和*aspnet Prod.sdf*文件从生产站点到一个安全存储位置，并且您应保留多个代如备份。 在更新数据库时，你应在更改之前立即从的备份副本。 然后，如果犯错，进而不发现它之前，部署到生产环境之后，你仍将能够将数据库恢复到它损坏之前的状态。


当 Visual Studio 中浏览器中，打开主页 URL 时*应用\_offline.htm*显示页。 删除后*应用\_offline.htm*文件，你可以浏览到您的主页上，以验证更新是否成功部署。

[![Instructors_page_with_birth_date_Prod](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image7.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image6.png)

现在，你已部署应用程序更新包含对测试和生产的数据库更改。 下一教程演示如何将数据库从 SQL Server Compact 迁移到 SQL Server Express 和 SQL Server。

>[!div class="step-by-step"]
[上一页](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
[下一页](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
