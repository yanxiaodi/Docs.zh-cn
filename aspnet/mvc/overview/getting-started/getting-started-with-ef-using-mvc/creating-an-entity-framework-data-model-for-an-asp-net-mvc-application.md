---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: "开始使用 Entity Framework 6 Code First 使用 MVC 5 |Microsoft 文档"
author: tdykstra
description: "提供了本系列教程的较新版本： 开始使用 ASP.NET Core 和使用 Visual Studio 2015 的实体框架核心。 Contoso Universi 中..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/22/2015
ms.topic: article
ms.assetid: 00bc8b51-32ed-4fd3-9745-be4c2a9c1eaf
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 46f53279e2e6daa4266c06feb4ba544e14b68a03
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="getting-started-with-entity-framework-6-code-first-using-mvc-5"></a>首先通过 MVC 5 使用 Entity Framework 6 Code First
====================
通过[Tom Dykstra](https://github.com/tdykstra)

[下载已完成的项目](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)或[下载 PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> > [!NOTE] 
> > 
> > 提供了本系列教程的较新版本：[开始使用 ASP.NET Core 和使用 Visual Studio 2015 的实体框架核心](https://docs.asp.net/en/latest/data/ef-mvc/intro.html)。
> 
> 
> Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 6 和 Visual Studio 2013 的 ASP.NET MVC 5 应用程序。 本教程使用 Code First 的工作流。 有关如何选择 Code First、 数据库第一个和第一个模型之间的信息，请参阅[实体框架开发工作流](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf)。
> 
> 示例应用程序是一个用于 fictional Contoso 大学网站。 它包括诸如学生许可、 过程创建和教师分配等功能。 本系列教程说明如何生成 Contoso 大学示例应用程序。 你可以[下载已完成的应用程序](https://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)。
> 
> 提供了由 Mike Brind 翻译的 Visual Basic 版本：[与在 Visual Basic 中的 EF 6 的 MVC 5](http://www.mikesdotnetting.com/Article/241/MVC-5-with-EF-6-in-Visual-Basic-Creating-an-Entity-Framework-Data-Model) Mikesdotnetting 站点上。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教程中使用的软件版本
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - 实体框架 6 （EntityFramework 6.1.0 NuGet 包）
> - [Windows Azure SDK 2.2](https://go.microsoft.com/fwlink/p/?linkid=323510) （可选）
>   
> 
> 本教程还应使用[Visual Studio 2013 Express for Web](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express)或 Visual Studio 2012。 [VS 2012 版本的 Windows Azure SDK](https://go.microsoft.com/fwlink/p/?linkid=323511)是使用 Visual Studio 2012 的 Windows Azure 部署所必需的。
> 
> 
> ## <a name="tutorial-versions"></a>教程版本
> 
> 对于本教程的早期版本，请参阅[EF 4.1 / MVC 3 电子书](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#GettingStartedwiththeEntityFramework4.1usingASP.NETMVC)和[Getting Started with EF 5 使用 MVC 4](../../older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。
> 
> ## <a name="questions-and-comments"></a>问题和意见
> 
> 请留下反馈上如何喜欢本教程的方式，我们可以提高在页面底部的注释中。 如果你有与本教程不直接相关的问题，你可以发布到[ASP.NET 实体框架论坛](https://forums.asp.net/1227.aspx)、[实体框架和 LINQ to Entities 论坛](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/)，或[StackOverflow.com](http://stackoverflow.com/)。
> 
> 如果你遇到无法解决的问题，你通常可以通过比较已完成的项目，你可以下载你的代码会发现问题的解决方案。 一些常见的错误和如何解决它们，请参阅[常见错误，以及解决方案或解决办法它们。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


## <a name="the-contoso-university-web-application"></a>Contoso 大学 Web 应用程序

你将在这些教程中构建的应用程序是简单大学网站。

用户可以查看和更新学生、 课程中和教师信息。 以下是一些你将创建的屏幕。

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![编辑学生](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

此站点的用户界面样式而保留接近什么由运行内置的任何模板，以便在本教程主要关注如何使用实体框架。

## <a name="prerequisites"></a>系统必备

请参阅**软件版本**页顶部。 实体框架 6 不是先决条件，因为本教程的一部分安装 EF NuGet 包。

## <a name="create-an-mvc-web-application"></a>创建 MVC Web 应用程序

打开 Visual Studio 并创建一个名为"ContosoUniversity"的新 C# Web 项目。

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image3.png)

在**新建 ASP.NET 项目**对话框框中，选择**MVC**模板。

如果**在云中托管**中的复选框**Microsoft Azure**选择部分后，清除它。

单击**更改身份验证**。

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image4.png)

在**更改身份验证**对话框中，选择**无身份验证**，然后单击**确定**。 对于本教程中你不会是要求用户登录，也限制基于登录的用户访问。

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image5.png)

返回在新建 ASP.NET 项目对话框中，单击**确定**以创建该项目。

## <a name="set-up-the-site-style"></a>设置站点样式

几个简单的更改将设置站点菜单、 布局和主页。

打开*views/shared\\_Layout.cshtml*，并进行以下更改：

- 将每个匹配项的"My ASP.NET Application"和"应用程序名称"更改为"Contoso 大学"。
- 学生、 课程，教师和部门，添加菜单项和删除联系人条目。

突出显示所做的更改。

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=6,19,24-27,38)]

在*Views\Home\Index.cshtml*，将文件的内容替换为以下代码以将有关 ASP.NET 和 MVC 的文本替换为有关此应用程序的文本：

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

按 CTRL + F5 以运行该站点。 你看到与主菜单的主页。

![Contoso_University_home_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image6.png)

## <a name="install-entity-framework-6"></a>安装的 Entity Framework 6

从**工具**菜单上，单击**NuGet 包管理器**，然后单击**程序包管理器控制台**。

在**程序包管理器控制台**窗口中输入以下命令：

`Install-Package EntityFramework`

![EF 安装](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image7.png)

图显示正在安装的 6.0.0 但 NuGet 将安装最新版本 （不包括预发行版本），实体框架的截至本教程的最新更新即 6.1.1。

此步骤是几个步骤，本教程将使你在手动执行此，但其无法完成操作自动通过 ASP.NET MVC 基架功能之一。 这样您可以看到使用实体框架所需的步骤，你在从手动执行它们。 你将更高版本使用基架创建 MVC 控制器和视图。 一种替代方法是让基架自动安装 EF NuGet 包，创建数据库上下文类，并创建连接字符串。 你已准备好执行此操作通过这种方式，你所要做时，跳过这些步骤并在创建实体类后，你的 MVC 控制器搭建基架。

## <a name="create-the-data-model"></a>创建数据模型

接下来你将创建 Contoso 大学应用程序的实体类。 你将从开始以下三个实体：

![Class_diagram](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image8.png)

之间的一对多关系`Student`和`Enrollment`实体，并且没有之间的一个对多关系`Course`和`Enrollment`实体。 换而言之，一名学生可以在任意数量的课程中, 注册，并且某一课程可以具有任意数量的学生在其中注册。

下列部分中，你将创建每个这些实体的类。

> [!NOTE]
> 如果你尝试编译项目，然后完成创建所有这些实体类，将收到编译器错误。


### <a name="the-student-entity"></a>学生实体

![Student_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image9.png)

在*模型*文件夹中，创建一个名为的类文件*Student.cs*和模板代码替换为以下代码：

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs)]

`ID`属性将成为对应于此类数据库表的主键列。 默认情况下，实体框架将解释一个属性，名为`ID`或*classname* `ID`为主键。

`Enrollments`属性是*导航属性*。 导航属性将分别包含与此实体相关的其他实体。 在这种情况下，`Enrollments`属性`Student`实体将保留所有`Enrollment`与该订阅相关的实体`Student`实体。 换而言之，如果给定`Student`数据库中的行具有两个相关`Enrollment`行 (包含该学生的主键的行值在其`StudentID`外键列)，则该`Student`实体的`Enrollments`导航属性将包含这两个`Enrollment`实体。

导航属性通常定义为`virtual`，以便它们可以充分利用某些实体框架功能，如*延迟加载*。 (将解释延迟加载更高版本，在[读取相关数据](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)更高版本中这一系列教程。)

如果导航属性可以具有多个实体 （如多对多或一对多关系），其类型必须是的列表中的条目可以是添加、 删除和更新，如`ICollection`。

### <a name="the-enrollment-entity"></a>注册实体

![Enrollment_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image10.png)

在*模型*文件夹中，创建*Enrollment.cs*和替换为以下代码替换现有代码：

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

`EnrollmentID`属性将是主键; 此实体使用*classname* `ID`模式而不是`ID`本身作为你在中看到`Student`实体。 通常，你将选择一个模式，并使用它在你的数据模型。 在这里，变体说明了你可以使用任一模式。 在更高版本的教程中，你将看到如何使用`ID`而无需`classname`轻松地在数据模型中实现继承。

`Grade`属性是[枚举](https://msdn.microsoft.com/data/hh859576.aspx)。 后问号`Grade`类型声明指示`Grade`属性是[可以为 null](https://msdn.microsoft.com/library/2cf62fcy.aspx)。 为 null 的等级是不同于零年级-null 意味着一个等级而言未知的或未尚未分配。

`StudentID`属性是一个外键，且相应的导航属性为`Student`。 `Enrollment`实体是与一个相关联`Student`实体，因此该属性只包含单个`Student`实体 (与不同`Student.Enrollments`导航属性前面所看到的后者可以容纳多个`Enrollment`实体)。

`CourseID`属性是一个外键，且相应的导航属性为`Course`。 `Enrollment`实体是与一个相关联`Course`实体。

实体框架： 将属性解释为外键属性，如果它名为*&lt;的导航属性名称&gt;&lt;主键属性名称&gt;* (例如， `StudentID`为`Student`以来的导航属性`Student`实体的主键是`ID`)。 外键属性还可以进行命名相同只需*&lt;主键属性名称&gt;* (例如，`CourseID`由于`Course`实体的主键是`CourseID`)。

### <a name="the-course-entity"></a>课程实体

![Course_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image11.png)

在*模型*文件夹中，创建*Course.cs*，模板代码替换为以下代码：

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

`Enrollments`属性是导航属性。 A`Course`可以与任意数量的相关实体`Enrollment`实体。

我们举例更多有关[DatabaseGenerated](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute(v=vs.110).aspx)本系列后面的教程中的属性。 基本上，此属性允许您输入的过程，而不是让生成它的数据库的主键。

## <a name="create-the-database-context"></a>创建的数据库上下文

协调为给定的数据模型的实体框架功能的主类是*数据库上下文*类。 通过派生自创建此类[System.Data.Entity.DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx)类。 在代码中你指定数据模型中包含哪些实体。 你还可以自定义某些实体框架行为。 在此项目中类命名为`SchoolContext`。

ContosoUniversity 项目中创建文件夹，右键单击中的项目**解决方案资源管理器**单击**添加**，然后单击**新文件夹**。 将新文件夹命名*DAL* （适用于数据访问层）。 在该文件夹中创建名为的新类文件*SchoolContext.cs*，并将模板代码替换为以下代码：

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

### <a name="specifying-entity-sets"></a>指定的实体集

此代码将创建[DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=VS.103).aspx)每个实体集的属性。 在实体框架术语中，*实体集*通常对应于一个数据库表，和*实体*对应于表中的行。

> [!NOTE] 
> 
> 可以省略`DbSet<Enrollment>`和`DbSet<Course>`语句和其效果相同。 实体框架会将其包含隐式因为`Student`实体引用`Enrollment`实体和`Enrollment`实体引用`Course`实体。


### <a name="specifying-the-connection-string"></a>指定的连接字符串

连接字符串的 （稍后将添加到 Web.config 文件） 的名称被传递给构造函数。

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs?highlight=1)]

您还无法传入的连接字符串本身而不是一个存储在 Web.config 文件的名称。 有关将数据库指定要使用的选项的详细信息，请参阅[实体框架的连接和模型](https://msdn.microsoft.com/data/jj592674)。

如果不显式指定连接字符串或其中一个的名称，实体框架将假定该连接字符串名称是否与类名称相同。 在此示例中的默认连接字符串名称将为`SchoolContext`，与你在指定的显式相同。

### <a name="specifying-singular-table-names"></a>指定采用单数形式的表名称

`modelBuilder.Conventions.Remove`中的语句[OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx)的方法阻止从正在变为复数形式的表名称。 如果你未执行此操作，数据库中生成的表将被命名为`Students`， `Courses`，和`Enrollments`。 相反，则表名将为`Student`， `Course`，和`Enrollment`。 开发者对表名称是否应为复数意见不一。 本教程使用单数形式，但重要的一点是，可以选择你的首选，通过包括或忽略这行代码的任何一个窗体。

## <a name="set-up-ef-to-initialize-the-database-with-test-data"></a>设置 EF 初始化测试数据的数据库

实体框架可以自动创建 （或除去并重新创建） 为你的应用程序运行时的数据库。 你可以指定，这应进行每次运行应用程序或仅时模型不同步的与现有数据库。 你还可以编写`Seed`实体框架自动填充测试数据以创建数据库后调用的方法。

默认行为是创建数据库，仅当它不存在 （和引发异常，如果该模型已更改，并且数据库已存在）。 在本部分中，你将指定应删除并重新创建该模型发生更改时数据库。 删除数据库导致所有数据的丢失。 这通常是确定在开发期间，因为`Seed`方法将在数据库重新创建，并将重新创建测试数据时运行。 但在生产环境中你通常不希望每次需要更改数据库架构时，会丢失所有数据。 更高版本，你将了解如何通过使用 Code First 迁移以更改而不是删除并重新创建数据库的数据库架构处理模型更改。

在 DAL 文件夹中，创建名为的新类文件*SchoolInitializer.cs*并使用该模板代码替换  
下面的代码，这会导致数据库创建时需要并将测试数据加载到新的数据库。

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.cs)]

`Seed`方法采用数据库上下文对象作为输入参数，并在方法中的代码使用  
要向数据库添加新实体该对象。 对于每个实体类型，该代码创建的集合新  
 实体，将它们添加到相应`DbSet`属性，然后将保存到数据库的更改。 它不  
不必调用`SaveChanges`方法之后每个组的实体，如上文所述，但这样做，可帮助  
如果向数据库写入代码时出现异常，你可以找到问题的根源。

若要指示实体框架可以使用初始值设定项类，将元素添加到`entityFramework`应用程序中的元素*Web.config*文件 （文件的根项目文件夹中），如下面的示例中所示：

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.xml?highlight=2-6)]

`context type`指定完全限定的上下文类名称和在中，它是程序集和`databaseinitializer type`指定初始值设定项类和在程序集的完全限定的名称。 (如果你不想使用初始值设定项的 EF，您可以对设置属性`context`元素： `disableDatabaseInitialization="true"`。)有关详细信息，请参阅[实体框架的配置文件设置](https://msdn.microsoft.com/data/jj556606)。

作为设置初始值设定项的替代方法*Web.config*文件是代码以执行此操作通过添加`Database.SetInitializer`语句`Application_Start`中的方法*Global.asax.cs*文件。 有关详细信息，请参阅[了解数据库初始值设定项中 Entity Framework Code First](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm)。

应用程序现在已设置最多，以便在某给定运行过程的第一次访问数据库时  
应用程序，实体框架比较对模型数据库 (你`SchoolContext`和实体类)。 如果差异，应用程序将删除并重新创建数据库。

> [!NOTE]
> 在部署到生产 web 服务器应用程序时，你必须删除或禁用将删除并重新创建数据库的代码。 在本系列后面的教程中，将执行该操作。


## <a name="set-up-ef-to-use-a-sql-server-express-localdb-database"></a>设置为使用 SQL Server Express LocalDB 数据库的 EF

[LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx)是 SQL Server Express 数据库引擎的轻量版本。 它很容易安装和配置、 启动点播情况下，并在用户模式下运行。 LocalDB 运行中的 SQL Server Express，可用于处理数据库作为一种特殊的执行模式*.mdf*文件。 你可以将 LocalDB 数据库文件放入*应用\_数据*的 web 项目中，如果你想要能够复制数据库与项目的文件夹。 在 SQL Server Express 用户实例功能还可以使您能够使用*.mdf*文件，但用户实例功能已弃用; 因此，使用建议 LocalDB *.mdf*文件。 在 Visual Studio 2012 和更高版本中，默认情况下，使用 Visual Studio 安装 LocalDB。

通常 SQL Server Express 不用于生产 web 应用程序。 LocalDB 具体而言不是建议生产使用与 web 应用程序由于它不是使用 IIS。

在本教程中，您将可以使用 LocalDB。 打开应用程序*Web.config*文件并添加`connectionStrings`元素前面`appSettings`元素，如下面的示例中所示。 (请确保你更新*Web.config*根项目文件夹中的文件。 此外，还有*Web.config*文件位于*视图*无需更新的子文件夹。)

如果你使用的 Visual Studio 2015，替换连接字符串中的"v11.0""MSSQLLocalDB"，因为默认 SQL Server 实例名称已更改。

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.xml?highlight=1-3)]

你已添加的连接字符串指定实体框架将使用名为的 LocalDB 数据库*ContosoUniversity1.mdf*。 （数据库尚不存在;EF 将创建它。）如果你想要在中创建的数据库你*应用\_数据*文件夹中，你可以添加`AttachDBFilename=|DataDirectory|\ContosoUniversity1.mdf`到连接字符串。 有关连接字符串的详细信息，请参阅[ASP.NET Web 应用程序的 SQL Server 连接字符串](https://msdn.microsoft.com/library/jj653752.aspx)。

你实际上不必中有一个连接字符串*Web.config*文件。 如果你不提供的连接字符串，实体框架将使用的默认的基于上下文类。 有关详细信息，请参阅[到新数据库 Code First](https://msdn.microsoft.com/data/jj193542)。

## <a name="creating-a-student-controller-and-views"></a>正在创建学生控制器和视图

现在，你将要创建网页上显示数据，并请求数据的过程将自动触发  
创建数据库。 你将首先创建新的控制器。 但执行此操作之前，生成项目后，要提供给 MVC 控制器基架的模型和上下文类。

1. 右键单击**控制器**文件夹中的**解决方案资源管理器**，选择**添加**，然后单击**新建基架项**。
- 在**添加基架**对话框中，选择**数据与视图，MVC 5 控制器使用 Entity Framework**。

    ![添加基架](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image12.png)
- 在添加控制器对话框中，进行以下选择，然后单击**添加**:

    - 模型类：**学生 (ContosoUniversity.Models)**。 （如果你没有看见此选项在下拉列表中的，生成项目并重试。）
    - 数据上下文类： **SchoolContext (ContosoUniversity.DAL)**。
    - 控制器名称： **StudentController** (不 StudentsController)。
    - 保留其他字段的默认值。

    ![Add_Controller_dialog_box_for_Student_controller](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image13.png)

    当你单击**添加**，基架创建 StudentController.cs 文件和一组工作与控制器的视图 （.cshtml 文件）。 将来当你创建使用实体框架的项目，你还可以利用的基架一些附加功能： 只需创建第一个模型类，无需创建一个连接字符串，然后在**添加控制器**框中指定新的上下文类。 基架将创建你`DbContext`类和你的连接字符串以及控制器和视图。
- Visual Studio 将打开*Controllers\StudentController.cs*文件。 你看到类变量具有已创建可实例化数据库上下文对象：

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

    `Index`操作方法获取从学生列表*学生*实体集通过阅读`Students`数据库上下文实例属性：

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

    *Student\Index.cshtml*视图在表中显示此列表：

    [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cshtml)]
- 按 Ctrl+F5 运行项目。 （如果收到"无法创建卷影副本"错误时，关闭浏览器并重试。）

    单击**学生**选项卡以查看测试数据，`Seed`插入的方法。 具体取决于如何窄浏览器窗口，你将看到在顶部的地址栏中的学生选项卡链接或你将需要单击右上角才能看到该链接。

    ![菜单按钮](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

    ![学生索引页](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image15.png)

## <a name="view-the-database"></a>查看数据库

当你运行学生页和应用程序尝试访问数据库时，EF 会看到没有任何数据库，所以创建了一个，然后它运行 seed 方法，可使用数据填充数据库。

你可以使用**服务器资源管理器**或**SQL Server 对象资源管理器**(SSOX) 若要查看 Visual Studio 中的数据库。 对于本教程将使用**服务器资源管理器**。 (在 Visual Studio Express 版本早于 2013，**服务器资源管理器**称为**数据库资源管理器**。)

1. 关闭浏览器。
2. 在**服务器资源管理器**，展开**数据连接**，展开**学校上下文 (ContosoUniversity)**，然后展开**表**若要查看新的数据库中的表。

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image16.png)
3. 右键单击**学生**表，然后单击**显示表数据**若要查看已创建的列和已插入到表的行。

    ![学生表](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image17.png)
4. 关闭**服务器资源管理器**连接。

*ContosoUniversity1.mdf*和*.ldf*数据库文件位于`C:\Users\<yourusername>`文件夹。

因为您正在使用`DropCreateDatabaseIfModelChanges`初始值设定项，你可以现在进行的更改`Student`类，再次运行应用程序，并且数据库将自动为重新创建，以匹配所做的更改。 例如，如果你添加`EmailAddress`属性`Student`类，请再次运行学生页面，然后查看表再次，你将看到管理新`EmailAddress`列。

## <a name="conventions"></a>约定

你必须编写实体框架能够为你创建完整的数据库中的代码量很短的因为使用了*约定*，或使实体框架的假设。 其中一些已记录或已使用不带你不知道的：

- 实体类名称 pluralized 窗体用作表名称。
- 实体属性名称用于列名称。
- 命名的实体属性`ID`或*classname* `ID`被识别为主键属性。
- 如果它名为属性将被解释为外键属性*&lt;的导航属性名称&gt;&lt;主键属性名称&gt;* (例如，`StudentID`为`Student`以来的导航属性`Student`实体的主键是`ID`)。 外键属性还可以进行命名相同只需&lt;主键属性名称&gt;(例如，`EnrollmentID`由于`Enrollment`实体的主键是`EnrollmentID`)。

你已了解可以约定中重写。 例如，指定表名称不应变为复数形式，并稍后你将看到如何显式标记为外键属性的属性。 你将了解有关约定和如何重写在[创建多个复杂的数据模型](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)更高版本中这一系列教程。 有关约定的详细信息，请参阅[代码第一个约定](https://msdn.microsoft.com/data/jj679962)。

## <a name="summary"></a>摘要

你现在已创建的简单应用程序使用的实体框架和 SQL Server Express LocalDB 来存储和显示数据。 在以下教程中，您将学习如何执行基本的 CRUD （创建、 读取、 更新、 删除） 操作。

请在如何喜欢本教程的方式，我们可以提高上，留下反馈。 你还可以请求新主题[教我编写代码](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code)。

在找不到其他实体框架资源的链接[ASP.NET 数据访问的推荐资源](../../../../whitepapers/aspnet-data-access-content-map.md)。

>[!div class="step-by-step"]
[下一篇](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
