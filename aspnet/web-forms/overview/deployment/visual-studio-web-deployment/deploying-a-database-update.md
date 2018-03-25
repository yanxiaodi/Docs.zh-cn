---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
title: "使用 Visual Studio 的 ASP.NET Web 部署： 部署某一数据库更新 |Microsoft 文档"
author: tdykstra
description: "本系列教程演示如何部署 （发布） ASP.NET web 应用程序到 Azure App Service Web Apps 或第三方托管提供程序，使用的..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 9cad0833-486a-4474-a7f3-7715542ec4ce
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
msc.type: authoredcontent
ms.openlocfilehash: 8e875a4282df78ec647579e74c3fbeabd2495fc2
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/12/2018
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-database-update"></a>使用 Visual Studio 的 ASP.NET Web 部署： 部署某一数据库更新
====================
通过[Tom Dykstra](https://github.com/tdykstra)

[下载初学者项目](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> 本系列教程演示如何部署 （发布） ASP.NET web 应用程序到 Azure App Service Web Apps 或第三方托管提供程序，通过使用 Visual Studio 2012 或 Visual Studio 2010。 有关序列的信息，请参阅[序列中的第一个教程](introduction.md)。


## <a name="overview"></a>概述

在本教程中，使数据库更改和相关的代码的更改，在 Visual Studio 中，测试所做的更改，然后将更新部署到测试、 过渡和生产环境。

本教程首先演示如何更新管理的 Code First 迁移的数据库，然后更高版本显示如何通过使用 dbDacFx 提供程序更新数据库。

提示： 如果你收到如下错误消息，或当你完成本教程的内容不起作用，请务必检查[故障排除页](troubleshooting.md)。

## <a name="deploy-a-database-update-by-using-code-first-migrations"></a>将数据库更新部署通过使用 Code First 迁移

在本部分中，你添加的出生日期列`Person`基类`Student`和`Instructor`实体。 然后你将更新，使其显示新的列显示教师数据的页。 最后，将所做的更改部署到测试、 过渡和生产。

### <a name="add-a-column-to-a-table-in-the-application-database"></a>向应用程序数据库的表中添加列

1. 在*ContosoUniversity.DAL*项目中，打开*Person.cs* ，并在末尾添加以下属性`Person`（应两个关闭跟在它后面的大括号） 的类：

    [!code-csharp[Main](deploying-a-database-update/samples/sample1.cs)]

    接下来，更新`Seed`方法，以便它提供的新列的值。 打开*Migrations\Configuration.cs* ，并将开始的代码块`var instructors = new List<Instructor>`与下面的代码块，其中包括出生日期信息：

    [!code-csharp[Main](deploying-a-database-update/samples/sample2.cs)]
2. 生成解决方案，，然后打开**程序包管理器控制台**窗口。 请确保 ContosoUniversity.DAL 仍处于选中状态作为**默认项目**。
3. 在**程序包管理器控制台**窗口中，选择**ContosoUniversity.DAL**作为**默认项目**，然后输入以下命令：

    [!code-powershell[Main](deploying-a-database-update/samples/sample3.ps1)]

    此命令完成时，Visual Studio 将打开定义新的类文件`DbMIgration`类，然后在`Up`方法你可以看到创建的新列的代码。 `Up`方法实现此更改，请时创建列和`Down`方法删除列时您正在回滚更改。

    ![AddBirthDate_migration_code](deploying-a-database-update/_static/image1.png)
4. 生成解决方案，，然后输入以下命令包含在**程序包管理器控制台**窗口 （请确保 ContosoUniversity.DAL 项目仍处于选中状态）：

    [!code-powershell[Main](deploying-a-database-update/samples/sample4.ps1)]

    实体框架运行`Up`方法，然后运行`Seed`方法。

### <a name="display-the-new-column-in-the-instructors-page"></a>在教师页中显示新的列

1. 在 ContosoUniversity 项目中，打开*Instructors.aspx*并添加一个新的模板字段来显示出生日期。 将其添加之间对雇佣日期和 office 分配：

    [!code-aspx[Main](deploying-a-database-update/samples/sample5.aspx?highlight=9-17)]

    （如果代码缩进获取同步的你可以按 CTRL K，然后选择 CTRL D 自动重新设置格式的文件。）
2. 运行应用程序，然后单击**教师**链接。

    在页面加载后，你看到它具有新出生日期字段。

    ![出生日期教师页](deploying-a-database-update/_static/image2.png)
3. 关闭浏览器。

### <a name="deploy-the-database-update"></a>将数据库更新部署

1. 在**解决方案资源管理器**选择 ContosoUniversity 项目。
2. 在**Web 单键发布**工具栏上，单击**测试**发布配置文件，并依次**发布 Web**。 (如果禁用了工具栏上，选择中的 ContosoUniversity 项目**解决方案资源管理器**。)

    Visual Studio 将部署更新的应用程序，并浏览器打开到主页。
3. 运行**教师**页后，可以验证更新是否成功部署。

    当应用程序尝试访问此页的数据库时，Code First，更新数据库架构，并运行`Seed`方法。 显示页时，你看到预期**出生日期**与在其中的日期的列。
4. 在**Web 单键发布**工具栏上，单击**过渡**发布配置文件，并依次**发布 Web**。
5. 运行**教师**中将临时以验证更新是否成功部署中的页。
6. 在**Web 单键发布**工具栏上，单击**生产**发布配置文件，并依次**发布 Web**。
7. 运行**教师**在生产环境以验证更新是否成功部署中的页。

    对于包括数据库更改的实际生产应用程序更新你通过使用通常还需要在部署过程中，脱机应用*应用\_offline.htm*，如你在前面的教程中看到。

## <a name="deploy-a-database-update-by-using-the-dbdacfx-provider"></a>将数据库更新部署使用 dbDacFx 提供程序

在本部分中，你添加*注释*列*用户*表中成员资格数据库并创建一个页，可以显示并编辑每个用户的注释。 然后将所做的更改部署到测试、 过渡和生产。

### <a name="add-a-column-to-a-table-in-the-membership-database"></a>向成员资格数据库中的表中添加列

1. 在 Visual Studio 中，打开**SQL Server 对象资源管理器**。
2. 展开**(localdb) \v11.0**，展开**数据库**，展开**aspnet ContosoUniversity** (不**aspnet ContosoUniversity Prod**)然后展开**表**。

    如果看不到**(localdb) \v11.0**下**SQL Server**节点，右键单击**SQL Server**节点，然后单击**添加 SQL Server**。 在**连接到服务器**对话框中，输入*(localdb) \v11.0*作为**服务器名称**，然后单击**连接**。

    如果看不到**aspnet ContosoUniversity**、 运行该项目并使用登录*管理员*凭据 (密码*devpwd*)，然后刷新**SQL Server 对象资源管理器**窗口。
3. 右键单击**用户**表，并依次**视图设计器**。

    ![SSOX 视图设计器](deploying-a-database-update/_static/image3.png)
4. 在设计器中，添加*注释*列并使其*nvarchar （128)*和可以为 null，然后单击**更新**。

    ![添加注释列](deploying-a-database-update/_static/image4.png)
5. 在**预览数据库更新**框中，单击**更新数据库**。

    ![预览数据库更新](deploying-a-database-update/_static/image5.png)

### <a name="create-a-page-to-display-and-edit-the-new-column"></a>创建页，以显示和编辑新的列

1. 在**解决方案资源管理器**，右键单击**帐户**ContosoUniversity 项目中的文件夹，请单击**添加**，然后单击**新项**.
2. 创建一个新**Web 窗体使用母版页**并将其命名*UserInfo.aspx*。 接受默认值*Site.Master*作为主控页文件。
3. 将复制到以下标记`MainContent``Content`元素 (3 的最后一个`Content`元素):

    [!code-aspx[Main](deploying-a-database-update/samples/sample6.aspx)]
4. 右键单击*UserInfo.aspx*页上，单击**用浏览器查看**。
5. 登录你*管理员*用户凭据 (密码*devpwd*) 并将注释添加到用户以验证页是否工作正常。

    ![UserInfo 页](deploying-a-database-update/_static/image6.png)
6. 关闭浏览器。

## <a name="deploy-the-database-update"></a>将数据库更新部署

若要部署使用 dbDacFx 提供程序，只需选择**更新数据库**发布配置文件中的选项。 但是，对于初始部署使用此选项时你还配置一些其他的 SQL 脚本，运行： 这种仍在配置文件和你需要防止其再次运行。

1. 打开**发布 Web**向导通过右键单击 ContosoUniversity 项目并单击**发布**。
2. 选择**测试**配置文件。
3. 单击**设置**选项卡。
4. 下**DefaultConnection**，选择**更新数据库**。
5. 禁用配置为用于初始部署运行其他脚本：

    1. 单击**配置数据库更新**。
    2. 在**配置数据库更新**对话框中，清除的复选框旁边*Grant.sql*和*aspnet 数据 dev.sql*。
    3. 单击 **“关闭”**。
6. 单击**预览**选项卡。
7. 下**数据库**和右侧的**DefaultConnection**，单击**预览版数据库**链接。

    ![数据库预览](deploying-a-database-update/_static/image7.png)

    预览窗口显示将在要使该数据库架构与源数据库的架构匹配的目标数据库中运行的脚本。 该脚本包含的 ALTER TABLE 命令时，添加新列。
8. 关闭**Database 预览版**对话框中，然后单击**发布**。

    Visual Studio 将部署更新的应用程序，并浏览器打开到主页。
9. 运行 UserInfo 页 (添加*Account/UserInfo.aspx*到主页 URL) 以验证更新是否成功部署。 你将需要输入登录*管理员*和*devpwd*。

    默认情况下，不部署表中的数据和未配置数据部署脚本来运行，因此，您不会发现你在开发中添加注释。 你可以在暂存以验证已部署到数据库中更改，而该页运行正确现在添加新的注释。
10. 请按照相同的过程来将部署到过渡和生产。

    不要忘记禁用额外的脚本。 相比测试配置文件的唯一区别是，将禁用只有一个脚本在过渡和生产配置文件，因为它们已配置为仅运行*aspnet prod data.sql*。

    过渡和生产的凭据是管理员和 prodpwd。

    包括数据库更改的实际生产应用程序更新为您通常也将进行应用程序脱机在部署过程中通过上载*应用\_offline.htm*之前发布，将其删除此后，正如你在看到[以前一教程](deploying-a-code-update.md)。

## <a name="summary"></a>摘要

现在，你已部署包含使用 Code First 迁移和 dbDacFx 提供程序的数据库更改的应用程序更新。

![出生日期教师页](deploying-a-database-update/_static/image8.png)

![UserInfo 页](deploying-a-database-update/_static/image9.png)

下一教程演示如何通过使用命令行执行部署。

>[!div class="step-by-step"]
[上一页](deploying-a-code-update.md)
[下一页](command-line-deployment.md)
