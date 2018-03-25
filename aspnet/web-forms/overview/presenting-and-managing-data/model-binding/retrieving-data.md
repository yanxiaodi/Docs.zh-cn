---
uid: web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
title: "检索和显示与数据模型绑定和 web 窗体 |Microsoft 文档"
author: tfitzmac
description: "本系列教程演示使用模型绑定的 ASP.NET Web 窗体项目的基本方面。 模型绑定使数据交互详细直接-..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 9f24fb82-c7ac-48da-b8e2-51b3da17e365
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
msc.type: authoredcontent
ms.openlocfilehash: e750250285fcb0088da284588d721ac21e73820c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="retrieving-and-displaying-data-with-model-binding-and-web-forms"></a>检索和显示使用模型的绑定和 web 窗体的数据
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> 本系列教程演示使用模型绑定的 ASP.NET Web 窗体项目的基本方面。 模型绑定使数据交互更直接的比处理数据源对象 （如 ObjectDataSource 或 SqlDataSource）。 这一系列开头介绍性材料，更高版本的教程中移动到更高级的概念。
> 
> 与任何数据访问技术配合使用模型绑定模式。 在本教程中，你将使用实体框架中，但你无法使用最熟悉的数据访问技术。 通过数据绑定服务器控件，如 GridView、 ListView、 说明或 FormView 控件，你可以指定要用于选择、 更新、 删除和创建数据的方法的名称。 在本教程中，你将为 SelectMethod 指定值。
> 
> 在该方法中，用于检索数据提供的逻辑。 在下一步的教程中，你将为 UpdateMethod、 delete 方法和 InsertMethod 设置值。
> 
> 你可以[下载](https://go.microsoft.com/fwlink/?LinkId=286116)在 C# 或 VB.完整项目 可下载的代码适用于 Visual Studio 2012 或 Visual Studio 2013。 它使用 Visual Studio 2012 模板，这是本教程中所示的 Visual Studio 2013 模板过程稍有不同。
> 
> 本教程中 Visual Studio 中运行应用程序。 你还可以让应用程序访问 Internet 上通过将其部署到托管提供商。 Microsoft 提供的最多 10 个网站中免费的 web 承载  
>  [免费 Azure 试用帐户](https://azure.microsoft.com/free/?WT.mc_id=A443DD604)。 有关如何将 Visual Studio web 项目部署到 Azure App Service Web Apps 的信息，请参阅[使用 Visual Studio 的 ASP.NET Web 部署](../../deployment/visual-studio-web-deployment/introduction.md)系列。 该教程还演示如何使用 Entity Framework Code First 迁移将您的 SQL Server 数据库部署到 Azure SQL 数据库。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教程中使用的软件版本
> 
> 
> - Microsoft Visual Studio 2013 或 Microsoft Visual Studio Express 2013 for Web
>   
> 
> 本教程还适用于 Visual Studio 2012，但将有一些差异的用户界面和项目模板。


## <a name="what-youll-build"></a>你将生成

在本教程中，你将：

1. 生成与学生课程中注册反映一所大学的数据对象
2. 生成从对象的数据库表
3. 使用测试数据填充数据库
4. 在 web 窗体中显示数据

## <a name="set-up-project"></a>设置项目

在 Visual Studio 2013 中，创建一个新**ASP.NET Web 应用程序**调用**ContosoUniversityModelBinding**。

![创建项目](retrieving-data/_static/image2.png)

选择 Web 窗体模板，并将其他默认选项。 单击确定以安装项目。

![选择 web 窗体](retrieving-data/_static/image3.png)

首先，将进行一些小更改自定义网站的外观。 打开**Site.Master**文件，并将更改要包括 Contoso 大学，而不是我的 ASP.NET 应用程序的标题。

[!code-aspx[Main](retrieving-data/samples/sample1.aspx?highlight=1)]

然后，将更改中的标头文本**应用程序名称**到**Contoso 大学**。

[!code-aspx[Main](retrieving-data/samples/sample2.aspx?highlight=7)]

此外在 Site.Master，更改以反映此站点相关的页面的标头中显示的导航链接。 将不需要以下两者之一**有关**页或**联系人**页，以便可以删除这些链接。 相反，您将需要调用的页链接**学生**。 此页尚未创建。

[!code-aspx[Main](retrieving-data/samples/sample3.aspx)]

保存并关闭 Site.Master。

现在，你将创建显示学生数据的 web 窗体。 右键单击你的项目，并**添加****新项**。 选择**包含母版页的 Web 窗体**模板，并将其命名**Students.aspx**。

![创建页](retrieving-data/_static/image5.png)

选择**Site.Master**作为新的 web 窗体母版页。

## <a name="create-the-data-models-and-database"></a>创建数据模型和数据库

Code First 迁移将用于创建对象和相应的数据库表。 这些表将存储有关学生和他们的课程的信息。

在 Models 文件夹中，添加一个名为的新类**UniversityModels.cs**。

![创建模型类](retrieving-data/_static/image7.png)

在此文件中，定义 SchoolContext、 学生、 注册、 和课程类，如下所示：

[!code-csharp[Main](retrieving-data/samples/sample4.cs)]

SchoolContext 类派生自 DbContext，它管理的数据库连接和数据中的更改。

在学生类中，请注意已应用于属性**FirstName**， **LastName**，和**年**属性。 这些属性将用于在本教程中的数据验证。 若要简化此演示项目的代码，只有这些属性都是使用数据验证特性标记。 在实际项目中，则将应用对需要验证的数据，例如注册和课程类中的属性的所有属性的验证特性。

保存 UniversityModels.cs。

对于 Code First 迁移工具将用于将基于这些类数据库设置。

在**程序包管理器控制台**，运行命令：  
`enable-migrations -ContextTypeName ContosoUniversityModelBinding.Models.SchoolContext`

如果命令成功完成，则你将收到一条消息已启用迁移，

![启用迁移](retrieving-data/_static/image8.png)

请注意，新文件命名为**Configuration.cs**已创建。 在 Visual Studio 中，将在创建后将自动打开此文件。 配置类包含**种子**方法使你以预填充使用测试数据的数据库表。

将以下代码添加到 Seed 方法。 你将需要添加**使用**语句**ContosoUniversityModelBinding.Models**命名空间。

[!code-csharp[Main](retrieving-data/samples/sample5.cs)]

保存 Configuration.cs。

在程序包管理器控制台中，运行命令`add-migration initial`。

然后，运行命令`update-database`。

如果运行此命令时，你会收到异常，，则可能 StudentID 和 CourseID 的值具有不同的 Seed 方法中的值。 在数据库中打开这些表，并查找 StudentID 和 CourseID 现有值。 在种子设定注册表的代码中添加这些值。

已添加的数据库文件，但它当前隐藏项目中。 单击**显示所有文件**若要显示的文件。

![显示所有文件](retrieving-data/_static/image10.png)

请注意.mdf 文件现在将出现在应用程序中\_数据文件夹。

![数据库文件](retrieving-data/_static/image12.png)

双击.mdf 文件，打开服务器资源管理器。 现在，表存在，且使用数据填充。

![数据库表](retrieving-data/_static/image14.png)

## <a name="display-data-from-students-and-related-tables"></a>显示学生和相关的表中的数据

数据库中的数据，你就现在可以检索该数据并将其显示在网页中。 你将使用**GridView**控件以显示行和列中的数据。

打开 Students.aspx，并找到**主内容**占位符。 在该占位符，添加**GridView**包括下面的代码的控件。

[!code-aspx[Main](retrieving-data/samples/sample6.aspx)]

有几个在此标记代码中为你需要注意的重要概念。 首先，请注意，为设置一个值**SelectMethod** GridView 元素中的属性。 此值指定用于检索数据的 GridView 方法的名称。 你将在下一步中创建此方法。 其次，请注意， **ItemType**属性设置为前面创建的学生类。 通过设置此值，你可以引用该类中的标记代码的属性。 例如，学生类包含名为注册的集合。 你可以使用**Item.Enrollments**来检索该集合，然后使用 LINQ 语法来检索每个学生的已注册的信用额度的总和。

在代码隐藏文件中，你需要添加为指定的方法**SelectMethod**值。 打开**Students.aspx.cs**，并添加**使用**语句**ContosoUniversityModelBinding.Models**和**System.Data.Entity**命名空间。

[!code-csharp[Main](retrieving-data/samples/sample7.cs)]

然后，添加以下方法。 请注意，此方法的名称与为 SelectMethod 提供的名称匹配。

[!code-csharp[Main](retrieving-data/samples/sample8.cs)]

**包括**子句提高了此查询的性能，但不是必需的工作的查询。 不带 Include 子句中，将使用延迟加载，涉及发送到数据库的单独查询每个时间相关检索数据检索的数据。 通过提供 Include 子句，使用预先加载，这意味着通过数据库的单个查询检索的所有相关的数据检索的数据。 在其中的大多数相关的数据是不能的情况下使用过的、 预先加载可能会效率较低，因为检索更多的数据。 但是，在这种情况下，预先加载提供最佳性能因为相关的数据显示为每个记录。

有关加载时的性能注意事项的详细信息与相关的数据，请参阅节**Lazy、 Eager 和显式加载的相关数据**中[读取与 ASP 在实体框架的相关数据.NET MVC 应用程序](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)主题。

默认情况下，数据按标记为键属性的值。 你可以添加一个 OrderBy 子句，以指定不同的值进行排序。 在此示例中，默认 StudentID 属性用于排序。 在[排序、 分页和筛选数据](sorting-paging-and-filtering-data.md)主题中，将使用户能够选择列进行排序。

运行 web 应用程序，并导航到学生页。 学生页显示以下学生信息。

![显示数据](retrieving-data/_static/image16.png)

## <a name="automatic-generation-of-model-binding-methods"></a>自动生成的模型绑定方法

当学习本教程系列中，你可以只需复制的代码从本教程到你的项目。 但是，此方法的一个缺点是功能的，你可能不会变得感知的 Visual Studio 自动生成模型绑定方法的代码提供。 处理的您自己的项目时，自动代码生成可为你节省时间并帮助你了解如何实现一个操作。 本部分介绍的自动代码生成功能。 本部分仅供参考，并不包含任何需要在你的项目中实现的代码。

设置的值时**SelectMethod**， **UpdateMethod**， **InsertMethod**，或**delete 方法**在标记代码中，属性你可以选择**创建新方法**选项。

![创建新的方法](retrieving-data/_static/image18.png)

Visual Studio 不仅具有正确签名的代码隐藏文件中创建一个方法，而且还会生成实现代码，以帮助您执行该操作。 如果第一次设置**ItemType**之前使用自动代码生成功能，生成的代码的属性将使用该类型的操作。 例如，设置 UpdateMethod 属性时，会自动会生成以下代码：

[!code-csharp[Main](retrieving-data/samples/sample9.cs)]

同样，上面的代码不必添加到你的项目。 在下一步的教程中，你将实现用于更新、 删除和添加新数据的方法。

## <a name="conclusion"></a>结束语

在本教程中，你将创建数据模型类，并从这些类生成数据库。 使用测试数据填充数据库表。 模型绑定用于从数据库中检索数据并在一个 GridView 中显示数据。

在下一个[教程](updating-deleting-and-creating-data.md)在此系列中，将启用更新、 删除和创建的数据。

>[!div class="step-by-step"]
[下一篇](updating-deleting-and-creating-data.md)
