---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1
title: "如何开始使用实体框架 4.0 数据库和 ASP.NET 4 Web 窗体 |Microsoft 文档"
author: tdykstra
description: "Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 4.0 和 Visual Studio 2010 的 ASP.NET Web 窗体应用程序..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: 5cb00916-8f46-491f-be25-4739a615d619
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1
msc.type: authoredcontent
ms.openlocfilehash: ae2fddc81f6f4da866ec0719a0e74516bdd2a4d1
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms"></a>如何开始使用实体框架 4.0 数据库和 ASP.NET 4 Web 窗体
====================
通过[Tom Dykstra](https://github.com/tdykstra)

> Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 4.0 和 Visual Studio 2010 的 ASP.NET Web 窗体应用程序。 示例应用程序是虚构的 Contoso 大学网站。 它包括诸如学生许可、 过程创建和教师分配等功能。
> 
> 本教程将说明在 C# 示例。 [可下载示例](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a)包含 C# 和 Visual Basic 中的代码。
> 
> ## <a name="database-first"></a>第一个数据库
> 
> 有三种方法可以使用的实体框架中的数据： *Database First*， *Model First*，和*Code First*。 此教程适用于第一个数据库。 有关如何为你的方案选择最佳这些工作流和指南之间的差异信息，请参阅[实体框架开发工作流](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf)。
> 
> ## <a name="web-forms"></a>Web Forms — Web 窗体
> 
> 本系列教程使用 ASP.NET Web 窗体模型，并且假定你知道如何使用 Visual Studio 中的 ASP.NET Web 窗体。 如果不这样做，请参阅[Getting Started with ASP.NET 4.5 Web 窗体](../../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md)。 如果你更想要使用 ASP.NET MVC framework，请参阅[Getting Started with 使用 ASP.NET MVC 的 Entity Framework](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。
> 
> ## <a name="software-versions"></a>软件版本
> 
> | **本教程中所示** | **也适用于** |
> | --- | --- |
> | Windows 7 | Windows 8 |
> | Visual Studio 2010 | Visual Studio 2010 Express for Web. 本教程不经过测试与更高版本的 Visual Studio。 有许多菜单中选择、 对话框框中，和模板中的差异。 |
> | .NET 4 | .NET 4.5 是与.NET 4 中，向后的兼容，但本教程未经过测试.NET 4.5。 |
> | 实体框架 4 | 本教程不经过测试与实体框架的更高版本。 从实体框架 5 开始，默认情况下使用 EF `DbContext API` ，引入了 EF 4.1。 EntityDataSource 控件的设计用于`ObjectContext`API。 有关如何使用 EntityDataSource 控件`DbContext`API，请参阅[这篇博客文章](https://blogs.msdn.com/b/webdev/archive/2012/09/13/how-to-use-the-entitydatasource-control-with-entity-framework-code-first.aspx)。 |
> 
> ## <a name="questions"></a>问题
> 
> 如果你有与本教程不直接相关的问题，你可以发布到[ASP.NET 实体框架论坛](https://forums.asp.net/1227.aspx)、[实体框架和 LINQ to Entities 论坛](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/)，或[StackOverflow.com](http://stackoverflow.com/)。


## <a name="overview"></a>概述

你将在这些教程中构建的应用程序是一个简单大学网站。

[![Image03](the-entity-framework-and-aspnet-getting-started-part-1/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image1.png)

用户可以查看和更新学生、 课程中和教师信息。 下面显示了几个你将创建的屏幕。

[![Image30](the-entity-framework-and-aspnet-getting-started-part-1/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image3.png)

[![Image37](the-entity-framework-and-aspnet-getting-started-part-1/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image5.png)

[![Image31](the-entity-framework-and-aspnet-getting-started-part-1/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image7.png)

[![Image32](the-entity-framework-and-aspnet-getting-started-part-1/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image9.png)

## <a name="creating-the-web-application"></a>创建 Web 应用程序

若要开始本教程，请打开 Visual Studio，然后创建新的 ASP.NET Web 应用程序项目使用**ASP.NET Web 应用程序**模板：

[![Image01](the-entity-framework-and-aspnet-getting-started-part-1/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image11.png)

此模板创建已包含样式表和母版页的 web 应用程序项目：

[![Image02](the-entity-framework-and-aspnet-getting-started-part-1/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image13.png)

打开*Site.Master*文件并将更改"My ASP.NET Application"到"Contoso 大学"。

[!code-html[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample1.html)]

查找*菜单*控件名为`NavigationMenu`和将其替换为以下标记，添加你将创建的页的菜单项。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample2.aspx)]

打开*Default.aspx*页并更改`Content`控件名为`BodyContent`于此：

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample3.aspx)]

你现在具有一个包含你将创建的各种页面的链接的简单主页：

[![Image03](the-entity-framework-and-aspnet-getting-started-part-1/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image15.png)

## <a name="creating-the-database"></a>创建数据库

对于这些教程中，你将使用实体框架数据模型设计器自动创建基于现有数据库的数据模型 (通常称为*数据库一流*方法)。 在本教程系列未涵盖的替代方法是手动创建数据模型，然后具有创建数据库的设计器生成脚本 (*模型优先*方法)。

对于本教程中使用数据库第一个方法下, 一步是将数据库添加到站点。 最简单方法是首先下载用本教程的项目。 然后右键单击*应用\_数据*文件夹，选择**添加现有项**，然后选择*School.mdf*已下载项目中的数据库文件。

一种替代方法是遵循的说明[创建 School 示例数据库](https://msdn.microsoft.com/library/bb399731.aspx)。 是否下载数据库或创建它时，将复制*School.mdf*到你的应用程序的以下文件夹中的文件*应用\_数据*文件夹：

`%PROGRAMFILES%\Microsoft SQL Server\MSSQL10.SQLEXPRESS\MSSQL\DATA`

(此位置的*.mdf*文件假定你正在使用 SQL Server 2008 Express。)

如果从脚本中创建数据库，请执行以下步骤以创建数据库关系图：

1. 在**服务器资源管理器**，展开**数据连接**，展开*School.mdf*，右键单击**数据库关系图**，然后选择**添加新关系图**。

    [![Image35](the-entity-framework-and-aspnet-getting-started-part-1/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image17.png)
2. 选择的所有表，然后单击**添加**。

    [![Image36](the-entity-framework-and-aspnet-getting-started-part-1/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image19.png)

    SQL Server 创建数据库关系图，显示表，表和表之间的关系中的列。 您可以移动以将它们组织你喜欢的表。
3. 另存为"SchoolDiagram"关系图并将其关闭。

如果你下载*School.mdf*文件用本教程中，您可以通过双击查看数据库关系图**SchoolDiagram**下**数据库关系图**中**服务器资源管理器**。

[![Image38](the-entity-framework-and-aspnet-getting-started-part-1/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image21.png)

关系图看上去如下所示 （表可能会在此处所示的不同位置中）：

[![Image04](the-entity-framework-and-aspnet-getting-started-part-1/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image23.png)

## <a name="creating-the-entity-framework-data-model"></a>创建实体框架数据模型

现在你可以从此数据库创建的实体框架数据模型。 你可以在应用程序的根文件夹中创建数据模型，但对于本教程，则将它放在名为的文件夹中*DAL* （适用于数据访问层）。

在**解决方案资源管理器**，添加一个名为的项目文件夹*DAL* （确保它是在项目下，不在的解决方案下）。

右键单击*DAL*文件夹，然后选择**添加**和**新项**。 下**已安装的模板**，选择**数据**，选择**ADO.NET 实体数据模型**模板，将其命名为*SchoolModel.edmx*，和然后单击**添加**。

[![Image05](the-entity-framework-and-aspnet-getting-started-part-1/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image25.png)

这将启动实体数据模型向导。 在第一个的向导步骤中，**从数据库生成**选项默认为选中状态。 单击 **“下一步”**。

[![Image06](the-entity-framework-and-aspnet-getting-started-part-1/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image27.png)

在**选择你的数据连接**步骤，保留默认值，单击**下一步**。 School 数据库在默认情况下处于选中状态和连接设置保存在*Web.config*文件作为**SchoolEntities**。

[![Image07](the-entity-framework-and-aspnet-getting-started-part-1/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image29.png)

在**选择数据库对象**向导步骤中，选择所有除表`sysdiagrams`（其中已为你先前生成的关系图创建的），然后单击**完成**。

[![Image08](the-entity-framework-and-aspnet-getting-started-part-1/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image31.png)

完成创建模型后，Visual Studio 将显示对应于你的数据库表的实体框架对象 （实体） 的图形表示。 （与数据库关系图中，各个元素的位置可能不同于在此图中看到的内容。 您可以拖动元素以匹配图中，如果你想。）

[![Image09](the-entity-framework-and-aspnet-getting-started-part-1/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image33.png)

## <a name="exploring-the-entity-framework-data-model"></a>浏览实体框架数据模型

你可以看到的实体图表外观与包含两个区别的数据库关系图非常相似。 其中一个区别是指示关联的类型的每个关联一端的符号添加 （表关系在数据模型中称为实体关联）：

- 表示对零或一一个关联为"1"和"0..1"。

    [![Image39](the-entity-framework-and-aspnet-getting-started-part-1/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image35.png)

    在这种情况下，`Person`实体可能或可能不与关联`OfficeAssignment`实体。 `OfficeAssignment`实体必须与关联`Person`实体。 换而言之，一名教师 office，可能不会分配和任何 office 可以分配给只有一个教师。
- 一个对多关联都由"1"和"\*"。

    [![Image40](the-entity-framework-and-aspnet-getting-started-part-1/_static/image38.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image37.png)

    在这种情况下，`Person`实体可能或可能不具有关联`StudentGrade`实体。 A`StudentGrade`实体必须与一个相关联`Person`实体。 `StudentGrade`实体实际表示此数据库中; 中的已注册的课程如果在某一课程中注册一名学生并且尚未，没有无评分`Grade`属性为 null。 换而言之，一名学生可能未注册任何课程中，它可能在一个课程中，注册，或可能在多个课程中注册。 在注册过程中的每个年级适用于仅一名学生。
- 由多对多关联"\*"和"\*"。

    [![Image41](the-entity-framework-and-aspnet-getting-started-part-1/_static/image40.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image39.png)

    在这种情况下，`Person`实体可能或可能不具有关联`Course`实体和反过来也是如此：`Course`实体可能或可能不具有关联`Person`实体。 换而言之，一名教师可能教授多个课程，并可能由多个教师讲授的课程。 （在此数据库中，此关系仅适用于教师; 它不包含指向学生课程。 学生链接到课程的 StudentGrades 表。）

数据库关系图和数据模型之间的另一个区别是其他**导航属性**为每个实体的部分。 实体的导航属性引用的相关的实体。 例如，`Courses`中的属性`Person`实体包含的所有集合`Course`与该订阅相关的实体`Person`实体。

[![Image12](the-entity-framework-and-aspnet-getting-started-part-1/_static/image42.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image41.png)

数据库和数据模型的另一个区别是缺少`CourseInstructor`数据库中使用链接的关联表`Person`和`Course`中的多对多关系的表。 导航属性，你可以获取相关`Course`从实体`Person`实体和相关`Person`从实体`Course`实体，因此无需来表示数据模型中的关联表。

[![Image11](the-entity-framework-and-aspnet-getting-started-part-1/_static/image44.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image43.png)

对于本教程中，假设`FirstName`列`Person`表实际包含某人的名字和中间名。 你想要更改以反映此操作，请字段的名称，但数据库管理员 (DBA) 可能不希望更改的数据库。 你可以更改名称`FirstName`在数据模型中，同时使其数据库等效属性保持不变。

在设计器中，右键单击**FirstName**中`Person`实体，，然后选择**重命名**。

[![Image13](the-entity-framework-and-aspnet-getting-started-part-1/_static/image46.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image45.png)

键入新名称"FirstMidName"。 此更改而无需更改数据库引用在代码中的列的方式。

[![Image29](the-entity-framework-and-aspnet-getting-started-part-1/_static/image48.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image47.png)

模型浏览器提供另一种方法，以查看数据库结构、 数据模型结构，以及它们之间的映射。 若要查看它，右键单击在实体设计器中的空白区域，然后单击**模型浏览器**。

[![Image18](the-entity-framework-and-aspnet-getting-started-part-1/_static/image50.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image49.png)

**模型浏览器**窗格中显示的树视图。 (**模型浏览器**窗格可能会与停靠**解决方案资源管理器**窗格。)**SchoolModel**节点表示数据模型结构，与**SchoolModel.Store**节点表示数据库结构。

[![Image26](the-entity-framework-and-aspnet-getting-started-part-1/_static/image52.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image51.png)

展开**SchoolModel.Store**以查看的表，请展开**表 / 视图**来到表，然后展开**课程**以查看表中的列。

[![Image19](the-entity-framework-and-aspnet-getting-started-part-1/_static/image54.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image53.png)

展开**SchoolModel**，展开**实体类型**，然后展开**课程**节点以查看实体和实体内的属性。

[![Image20](the-entity-framework-and-aspnet-getting-started-part-1/_static/image56.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image55.png)

在任一设计器或**模型浏览器**窗格中可以看到如何实体框架相关的两个模型的对象。 右键单击`Person`实体，然后选择**表映射**。

[![Image21](the-entity-framework-and-aspnet-getting-started-part-1/_static/image58.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image57.png)

这将打开**映射详细信息**窗口。 请注意，此窗口允许你将看到的数据库列`FirstName`映射到`FirstMidName`，这是什么其重命名为数据模型中。

[![Image22](the-entity-framework-and-aspnet-getting-started-part-1/_static/image60.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image59.png)

实体框架使用 XML 来存储数据库、 数据模型中，以及它们之间的映射信息。 *SchoolModel.edmx*文件实际上是 XML 文件，其中包含此信息。 设计器呈现的信息以图形格式，但你还可以查看该文件为 XML，请右键单击*.edmx*文件中**解决方案资源管理器**、 单击**打开方式**，并选择**XML （文本） 编辑器**。 （数据模型设计器和 XML 编辑器是打开和使用相同的文件中，因此不能具有设计器打开并在 XML 编辑器中打开该文件，在同一时间仅使用两个不同的方式。）

你现在已创建网站、 数据库和数据模型。 在下一个演练中，你将开始处理数据使用数据模型和 ASP.NET`EntityDataSource`控件。

>[!div class="step-by-step"]
[下一篇](the-entity-framework-and-aspnet-getting-started-part-2.md)
