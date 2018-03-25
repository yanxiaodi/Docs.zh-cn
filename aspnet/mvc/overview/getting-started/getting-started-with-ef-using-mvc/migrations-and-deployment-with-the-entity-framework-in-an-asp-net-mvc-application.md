---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application
title: "代码优先迁移和部署 ASP.NET MVC 应用程序中的实体框架 |Microsoft 文档"
author: tdykstra
description: "Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 6 Code First 和 Visual Studio 的 ASP.NET MVC 5 应用程序..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: d4dfc435-bda6-4621-9762-9ba270f8de4e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 2294f2aba3f765d7849d1f407e85f424dc8b2518
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="code-first-migrations-and-deployment-with-the-entity-framework-in-an-aspnet-mvc-application"></a>代码优先迁移和部署 ASP.NET MVC 应用程序中的实体框架
====================
通过[Tom Dykstra](https://github.com/tdykstra)

[下载已完成的项目](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)或[下载 PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 6 Code First 和 Visual Studio 2013 的 ASP.NET MVC 5 应用程序。 有关教程系列的信息，请参阅[序列中的第一个教程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。

到目前为止应用程序已经运行本地 IIS Express 在开发计算机上。 若要实际的应用程序可用于通过 Internet 使用其他人，你必须将其部署到 web 宿主提供程序。 在本教程中，你将部署到 Azure 中云 Contoso 大学应用程序。

本教程包含以下各节：

- 启用 Code First 迁移。 迁移功能，可更改数据模型并将其部署到生产环境所做的更改，通过无需删除并重新创建该数据库中更新数据库架构。
- 将部署到 Azure。 此步骤是可选的;无部署项目的情况下，您可以使用剩余的教程继续。

我们建议你与源代码管理中使用持续集成过程进行部署，但本教程不涉及这些主题。 有关详细信息，请参阅[源代码管理](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control)和[持续集成](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery)的章节[使用 Azure 构建真实世界云应用](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction)电子书。

## <a name="enable-code-first-migrations"></a>启用 Code First 迁移

当你开发新的应用程序时，你的数据模型更改通常情况下，和每次将模型更改，它将获取与数据库不同步。 你已配置实体框架可以自动除去并重新创建数据库每次更改数据模型。 当添加、 删除，或更改实体类或更改你`DbContext`类，在下次运行应用程序时自动删除现有数据库，创建一个新的匹配模型，并设定其种子使用测试数据。

保持数据库与数据模型同步此方法适用很好地部署到生产应用程序之前。 当在生产环境中运行应用程序时，它通常存储数据，你想要保留，并不想丢失的所有内容每次进行一次更改如添加新列。 [Code First 迁移](https://msdn.microsoft.com/data/jj591621)功能启用 Code First 来更新数据库架构，而不是删除并重新创建数据库，从而解决了此问题。 在本教程中，你将部署该应用程序，并准备，你将启用迁移。

1. 禁用初始值设定项，你将其设置前面通过注释掉或删除`contexts`添加到应用程序 Web.config 文件的元素。

    [!code-xml[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.xml?highlight=2,6)]
2. 应用程序中还*Web.config*文件，将连接字符串中数据库的名称更改为 ContosoUniversity2。

    [!code-xml[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.xml?highlight=2)]

    此更改将项目设置，以便第一次迁移将创建一个新的数据库。 这不是必需，但你将看到更高版本的原因是一个不错的主意。
3. 从**工具**菜单上，单击**库程序包管理器**然后**程序包管理器控制台**。

    ![Selecting_Package_Manager_Console](https://asp.net/media/4336350/1pm.png)
4. 在`PM>`提示符处输入以下命令：

    `enable-migrations`  
    `add-migration InitialCreate`

    ![enable-migrations 命令](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

    `enable-migrations`命令创建*迁移*文件夹 ContosoUniversity 项目中，且在该文件夹中放入*Configuration.cs*可编辑以配置迁移的文件。

    (如果你错过了前面将引导你要更改数据库名称的步骤，迁移会查找现有的数据库，并自动执行`add-migration`命令。 这是确定，它仅表示部署数据库之前，不会运行迁移代码的测试。 在运行时，更高版本`update-database`命令不会发生因为数据库已存在。)

    ![迁移文件夹](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

    与前面，所看到的初始值设定项类一样`Configuration`类包括`Seed`方法。

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

    用途[种子](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx)方法是，可以插入或更新测试数据，Code First 创建或更新数据库之后。 创建数据库时和每次更新数据库架构时，数据模型更改后调用该方法。

### <a name="set-up-the-seed-method"></a>设置 Seed 方法

当你删除并重新创建每个数据模型更改的数据库，你使用初始值设定项类的`Seed`方法插入测试数据，因为每次模型更改后删除了该数据库及其测试的所有数据将丢失。 使用 Code First 迁移，数据保留在数据库更改后的测试，因此包括中的测试数据[种子](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx)方法通常不是必需。 事实上，你不希望`Seed`方法插入测试数据，如果你将使用迁移来将数据库部署到生产环境，因为`Seed`方法将在生产中运行。 在这种情况下你想`Seed`方法插入到数据库所需数据在生产环境中。 例如，你可能想要包括在实际部门名称的数据库`Department`表应用程序在生产环境中可用时。

对于本教程，你将在使用迁移部署，但你`Seed`方法将是否仍要插入测试数据，以更加轻松地查看应用程序功能而无需手动插入大量数据的工作原理。

1. 内容替换*Configuration.cs*替换为以下代码，将测试数据加载到新数据库的文件。 

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

    [种子](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx)方法采用数据库上下文对象作为输入参数，并方法中的代码使用该对象将新实体添加到数据库。 对于每个实体类型的代码将创建新的实体的集合，将它们添加到相应[DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx)属性，然后将保存到数据库的更改。 无需调用[SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx)后的实体的每个组的方法，按上文所述，但执行该操作可帮助你找到问题的源，如果代码写入数据库时发生异常。

    一些插入数据的语句使用[AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx)方法来执行"upsert"操作。 因为`Seed`方法运行每次执行时`update-database`命令时，通常每个迁移后，不能只是插入数据，因为你尝试添加的行已将存在后创建数据库的初始迁移。 "Upsert"操作可以防止如果你尝试插入的行已存在，但它会发生的错误***重写***对测试应用程序时可能已做的数据的任何更改。 使用某些表中的测试数据您可能不希望这种情况： 在某些情况下在测试期间更改数据时所需更改后数据库更新保留。 在这种情况下要执行条件的插入操作： 仅当它尚不存在插入行。 Seed 方法使用这两种方法。

    第一个参数传递给[AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx)方法指定要用于检查行是否已存在的属性。 你要提供，测试学生数据`LastName`属性可以用于此目的，因为每个列表中的最后一个名称是唯一的：

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

    此代码假定最后一个名称是唯一的。 如果你手动添加具有重复的最后一名学生，你将获得以下异常下次执行迁移。

    序列包含多个元素

    有关如何处理如名为"Alexander Carson"的两个学生的冗余数据的信息，请参阅[进行种子设定和调试 Entity Framework (EF) 数据库](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx)Rick Anderson 博客上。 有关详细信息`AddOrUpdate`方法，请参阅[负责使用 EF 4.3 AddOrUpdate 方法](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)Julie Lerman 博客上。

    创建的代码`Enrollment`实体假定你已具有`ID`在实体中的值`students`集合，虽然未在创建集合的代码中设置该属性。

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=2)]

    你可以使用`ID`此处属性因为`ID`在调用时设置值`SaveChanges`为`students`集合。 EF 自动获取的主键值时它将实体插入到数据库中，并由此更新`ID`在内存中的实体属性。

    每个添加的代码`Enrollment`实体到`Enrollments`实体集不使用`AddOrUpdate`方法。 它会检查实体是否已存在，以及插入该实体，如果它不存在。 此方法将保留通过使用应用程序 UI 对注册级进行的更改。 此代码循环访问的每个成员`Enrollment`[列表](https://msdn.microsoft.com/library/6sh2ey19.aspx)和如果数据库中未找到注册，它将注册添加到数据库。 第一次更新数据库，数据库将为空，，因此它会将添加每个注册。

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]
2. 生成项目。

### <a name="execute-the-first-migration"></a>执行第一次迁移

当执行`add-migration`命令时，迁移生成将从头开始创建数据库的代码。 此代码也是在*迁移*文件夹中，在名为的文件*&lt;时间戳&gt;\_InitialCreate.cs*。 `Up`方法`InitialCreate`类创建对应的数据模型实体集，将数据库表和`Down`方法将其删除。

[!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

迁移调用`Up`方法以实现迁移的数据模型更改。 当你输入命令回滚更新，迁移调用`Down`方法。

这是你输入时创建的初始迁移`add-migration InitialCreate`命令。 参数 (`InitialCreate`在示例中) 用于文件命名，并可以是任何所需内容; 你通常要选择的单词或短语，总结了中迁移正在进行的内容。 例如，你可能会将更高版本迁移&quot;AddDepartmentTable&quot;。

如果数据库已存在时创建初始迁移，则生成的数据库创建代码，但它无需运行，因为数据库已与匹配的数据模型。 时应用部署到另一个环境，其中数据库不存在，请运行此代码将创建数据库时，因此它会先测试一个好办法。 这就是为什么以便迁移可以创建一个从零开始的新更改连接字符串前面-中数据库的名称。

1. 在**程序包管理器控制台**窗口中，输入以下命令：

    `update-database`

    ![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

    `update-database`命令将运行`Up`方法，创建数据库然后运行`Seed`方法来填充数据库。 相同的过程后将运行自动在生产中部署应用程序，正如你将看到的下一节中。
- 使用**服务器资源管理器**以检查数据库，就像在第一个教程中，并运行应用程序以验证所有内容仍适用相同像以前那样。

## <a name="deploy-to-azure"></a>将部署到 Azure

到目前为止应用程序已经运行本地 IIS Express 在开发计算机上。 若要使其可供其他人通过 Internet 使用，必须将其部署到 web 宿主提供程序。 在本教程的本部分将将其部署到 Azure。 本部分是可选的;你可以跳过此并继续以下教程中，或者你可调整不同的宿主提供程序所选的本部分中说明。

### <a name="using-code-first-migrations-to-deploy-the-database"></a>使用 Code First 迁移将数据库部署

若要将数据库部署将使用 Code First 迁移。 在创建使用从 Visual Studio 部署的配置设置的发布配置文件时，你将选择一个复选框**更新数据库**。 此设置会导致自动配置应用程序的部署过程*Web.config*文件在目标服务器上，以便代码优先使用`MigrateDatabaseToLatestVersion`初始值设定项类。

Visual Studio 不执行任何操作与数据库在部署过程时它正在将你的项目复制到目标服务器。 当你运行部署的应用程序，它为部署后首次访问数据库时，Code First 检查数据库是否与数据模型匹配。 如果存在不匹配，Code First 自动创建数据库 （如果它尚不存在） 或 （如果数据库存在，但与模型不匹配） 的最新版本更新数据库架构。 如果应用程序实施迁移`Seed`方法，创建数据库或架构更新后的方法运行。

迁移过程`Seed`方法插入测试数据。 如果你在部署到生产环境，你将必须更改`Seed`方法，以便它只会将你想要插入到你的生产数据库的数据。 例如，当前数据模型中你可能想要开发数据库中具有真实课程但虚构学生。 你可以编写`Seed`加载同时在开发中，并在部署到生产环境之前然后注释虚构学生方法。 也可以编写`Seed`加载仅课程，并使用应用程序的用户界面手动测试数据库中输入虚构学生方法。

### <a name="get-an-azure-account"></a>获取 Azure 帐户

你将需要一个 Azure 帐户。 如果你还没有一个，但你有 Visual Studio 订阅，你可以[激活您的订阅权益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)。 否则，可以在几分钟内创建一个免费试用帐户。 有关详细信息，请参阅[Azure 免费试用版](https://azure.microsoft.com/free/)。

### <a name="create-a-web-site-and-a-sql-database-in-azure"></a>在 Azure 中创建网站和 SQL 数据库

你在 Azure 中的 web 应用将共享宿主环境中运行，这意味着它与其他 Azure 客户端共享的虚拟机 (Vm) 上运行。 共享宿主环境是在云中开始低成本方法。 更高版本，如果你的 web 流量增加，应用程序可以缩放以满足需要通过在专用 Vm 上运行。 若要了解为 Azure App Service 定价选项的详细信息，请阅读该文档上[Azure 文档](https://azure.microsoft.com/pricing/details/app-service/)

你将部署到 Azure SQL 数据库的数据库。 SQL 数据库是根据 SQL Server 技术构建的基于云的关系数据库服务。 工具和应用程序使用 SQL Server 还处理 SQL 数据库。

1. 在[Azure 管理门户](https://portal.azure.com)，单击**新建**在左侧选项卡中，单击**查看所有**在新的边栏选项卡，然后单击**Web 应用和 SQL**中**Web**部分和最后**创建**。

    ![在管理门户中的新按钮](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/CreateWeb-Sql.png)

 **新的 Web 应用和 SQL-创建**向导随即打开。

2. 在边栏选项卡，输入中的字符串**应用名称**框，以用作你的应用程序的唯一 URL。 完整的 URL 将包含你在此处输入的 Azure 应用程序服务的默认域 plus 的 (。 azurewebsites.net)。 如果**应用名称**已被使用，该向导将通知你这些带有红色*应用程序名称不可用*消息。 如果**应用名称**是可用，你将获得一个绿色复选标记。

    ![创建包含在管理门户中的数据库链接](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Create-WebApp.png)

3. 在**订阅**下拉列表中，请选择要在其中的 Azure 订阅**App Service**驻留。

4. 在**资源组**文本框中，选择一个资源组或创建一个新。 此设置指定您的网站将运行在哪个数据中心。 有关资源组的详细信息，请继续阅读文档[Azure 文档](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups)。
5. 创建一个新**App Service 计划**通过单击*App Service 部分*，**新建**，并填写**App Service 计划**（可以是相同的名称应用程序服务），**位置**，和**定价层**（没有可用的选项）。

    ![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Create-AppService.png)
6. 单击**SQL 数据库**，然后选择*新建*或选择现有数据库

    ![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Create-Database.png)

7. 在**名称**框中，输入你的数据库的名称。
8. 单击**目标服务器**框中，选择**创建新的服务器**。 或者，如果你以前创建的服务器，你可以从可用服务器列表中选择该服务器。
9. 选择**定价层**部分中，选择*免费*。 如果需要其他资源，数据库可以扩展至在任何时间。 若要了解有关 Azure SQL 定价的更多信息，请阅读该文档上[Azure 文档](https://azure.microsoft.com/pricing/details/sql-database/)。
10. 修改[排序规则](https://docs.microsoft.com/sql/relational-databases/collations/collation-and-unicode-support)根据需要。
11. 输入管理员**SQL 管理员用户名**和**SQL 管理员密码**。 如果你选择**新建 SQL 数据库服务器**，不要输入现有名称和密码在此处，你应输入新名称和密码，你现在定义以后访问数据库时使用。 如果你选择你之前创建的服务器，你将为该服务器输入凭据。
12. 可以为 App Service 中使用 Application Insights 启用遥测数据收集。 与配置少的 application Insights 收集有价值的事件、 异常、 依赖项、 请求和跟踪信息。 若要了解有关 Application Insights 的详细信息，开始[Azure 文档](https://azure.microsoft.com/services/application-insights/)。
12. 单击**创建**底部的边栏选项卡，以指示你已完成。
  
 管理门户返回到仪表板页上，与**通知**边栏选项卡页顶部显示正在创建网站。 （通常小于一分钟），一段时间后将部署成功的通知。 在左侧的导航栏中的新**App Service**出现在*应用程序服务*部分和新**SQL 数据库**出现在*SQL 数据库*部分。

### <a name="deploy-the-application-to-azure"></a>部署到 Azure 应用程序

1. 在 Visual Studio 中，右键单击中的项目**解决方案资源管理器**和选择**发布**从上下文菜单。
  
    ![在项目上下文菜单中发布](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)
2. 在**配置文件**选项卡**发布 Web**向导中，单击**Microsoft Azure App Service**。
  
    ![导入发布设置](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-ChooseTarget.png)
3. 如果之前未添加你的 Azure 订阅在 Visual Studio 中，在屏幕上执行步骤。 这些步骤启用 Visual Studio 以连接到你的 Azure 订阅因此，该列表的**应用程序服务**将包括您的网站。
 
4. 选择**订阅**App Service 添加到，然后**App Service 计划**App Service 是的一部分的文件夹和最后**App Service**本身跟**确定**。

    ![选择 App Service](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-AppService.png)
5. 在配置配置文件后，**连接**将显示选项卡。 单击**验证连接**若要确保设置正确无误

    ![验证连接](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Connection.png)
7. 在验证连接后，旁边出现一个绿色的复选标记**验证连接**按钮。 单击 **“下一步”**。
  
    ![已成功验证的连接](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-SettingsValidated.png)
8. 打开**远程连接字符串**下的下拉列表**SchoolContext** ，然后选择你创建的数据库的连接字符串。
9. 选择**更新数据库**。

    ![设置选项卡](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Settings.png)

    此设置会导致自动配置应用程序的部署过程*Web.config*文件在目标服务器上，以便代码优先使用`MigrateDatabaseToLatestVersion`初始值设定项类。
10. 单击 **“下一步”**。
11. 在**预览**选项卡上，单击**开始预览**。
  
    ![预览选项卡中的开始预览按钮](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Preview.png)
  
 选项卡显示将复制到服务器的文件列表。 显示预览不需要发布应用程序但有用的功能，需要注意。 在这种情况下，你不必使用显示的文件列表执行任何操作。 下次部署此应用程序，仅将已更改的文件将在此列表中。
    ![开始预览文件输出](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-PreviewLoaded.png)

12. 单击“发布” 。
 Visual Studio 开始将文件复制到 Azure 的服务器的过程。
13. **输出**窗口将显示已执行的部署操作并报告已成功完成的部署。
  
    ![输出窗口报告部署成功](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-BuildOutput.png)
14. 成功部署后，默认浏览器自动打开指向已部署的 web 站点的 URL。
 你创建的应用程序现在在云中运行。 
  
    ![Students_index_page_with_paging](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Site.png)

此时你*SchoolContext*已在 Azure SQL 数据库中创建了数据库由于你选择了**执行 Code First 迁移 （在应用启动时运行）**。 *Web.config*已部署的 web 站点中的文件已更改，以便[MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx)初始值设定项运行你的代码读取或写入数据库 （这种情况中的数据的第一个时间如果选择**学生**选项卡上):

![](https://asp.net/media/4367421/mig.png)

在部署过程还创建新的连接字符串*(SchoolContext\_DatabasePublish*) 的代码优先迁移来用于更新数据库架构和数据库进行种子设定。

![Database_Publish 连接字符串](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image26.png)

您可以找到在你自己的计算机上的 Web.config 文件的已部署的版本*ContosoUniversity\obj\Release\Package\PackageTmp\Web.config*。你可以访问部署*Web.config*文件本身通过使用 FTP。 有关说明，请参阅[使用 Visual Studio 的 ASP.NET Web 部署： 部署某一代码更新](xref:web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update)。 按照以开头的说明"若要使用 FTP 工具，需要以下三项： 的 FTP URL、 用户名和密码。"

> [!NOTE]
> Web 应用未实现安全，因此任何找到的 URL 的人都可以更改的数据。 有关如何确保网站的安全的说明，请参阅[将包含成员资格、 OAuth 和 SQL 数据库的安全 ASP.NET MVC 应用程序部署到 Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)。 可以使用 Azure 管理门户中使用该站点中阻止其他人或**服务器资源管理器**Visual Studio 停止该站点中。


![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Stop-Service.png)

## <a name="advanced-migrations-scenarios"></a>高级的迁移方案

如果将数据库部署通过自动本教程中所示运行迁移，并且您要部署到多个服务器运行的网站，你可以获得多个服务器尝试在同一时间运行迁移。 迁移是原子性的，因此如果两个服务器尝试运行相同的迁移，一个将成功，并且另将失败 （假设不能两次执行的操作）。 在这种情况下如果你想要避免这些问题，可以手动调用迁移并设置你自己的代码，以便它只发生一次。 有关详细信息，请参阅[运行和代码中的脚本迁移](http://romiller.com/2012/02/09/running-scripting-migrations-from-code/)Rowan Miller 博客上和[Migrate.exe](https://msdn.microsoft.com/data/jj618307) （用于从命令行执行迁移） MSDN 上。

有关其他迁移方案的信息，请参阅[迁移段视频系列](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx)。

## <a name="code-first-initializers"></a>代码 First 初始值设定项

在部署部分你已看到[MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx)正在使用的初始值设定项。 代码首先还提供其他初始值设定项，包括[CreateDatabaseIfNotExists](https://msdn.microsoft.com/library/gg679221(v=vs.103).aspx) （默认值）、 [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=VS.103).aspx) （它使用更早版本） 和[DropCreateDatabaseAlways](https://msdn.microsoft.com/library/gg679506(v=VS.103).aspx)。 `DropCreateAlways`初始值设定项可用于设置单元测试的条件。 你还可以编写你自己初始值设定项，并且如果不想等待，直到应用程序从读取或写入数据库，你可以显式调用初始值设定项。 在本教程在 2013 年 11 月正在编写时你可以只使用创建和 DropCreate 初始值设定项之前启用迁移。 实体框架团队正致力于使这些初始值设定项可使用以及迁移。

有关初始值设定项的详细信息，请参阅[了解数据库初始值设定项中 Entity Framework Code First](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm)和书籍的第 6 章[编程实体框架： Code First](http://shop.oreilly.com/product/0636920022220.do)通过 Julie Lerman和 Rowan Miller。

## <a name="summary"></a>摘要

在本教程中，你已了解如何启用迁移和部署应用程序。 在下一教程将首先通过展开数据模型来查看更高级的主题。

请在如何喜欢本教程的方式，我们可以提高上，留下反馈。 你还可以请求新主题[教我编写代码](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code)。

在找不到其他实体框架资源的链接[ASP.NET 数据访问的推荐资源](xref:whitepapers/aspnet-data-access-content-map)。

>[!div class="step-by-step"]
[上一页](xref:mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application)
[下一页](xref:mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-a-more-complex-data-model-for-an-asp-net-mvc-application)
