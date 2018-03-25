---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
title: "什么是实体框架 4.0 中的新增功能 |Microsoft 文档"
author: tdykstra
description: "本教程系列上的 Contoso 大学 web 应用程序创建的 Getting Started with 实体 Framework 4.0 教程系列生成。 I..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 393df4a8-b1db-44c4-9db7-2b533ca887d0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
msc.type: authoredcontent
ms.openlocfilehash: c114627388217e892c84d6b76366d0fa96b0b70c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="whats-new-in-the-entity-framework-40"></a>什么是实体框架 4.0 中的新增功能
====================
通过[Tom Dykstra](https://github.com/tdykstra)

> 本教程系列上的 Contoso 大学 web 应用程序创建的生成[Getting Started with 实体框架](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md)教程系列。 如果未完成前面的教程，作为一个起始点本教程，你可以[下载应用程序](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a)，你应已创建。 你还可以[下载应用程序](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa)，它由完整教程系列创建。 如果你有疑问的教程，你可以发布到[ASP.NET 实体框架论坛](https://forums.asp.net/1227.aspx)。


在前面的教程中，您将了解一些方法用于最大程度地使用实体框架的 web 应用程序的性能。 本教程中查看的一些最重要新功能在版本 4 的实体框架中，而且它链接到提供的所有新功能的更完整介绍的资源。 在本教程中突出显示的功能包括：

- 外键关联。
- 执行用户定义的 SQL 命令。
- 模型优先开发。
- POCO 支持。

此外，本教程将简要介绍*代码优先的开发*，即将推出的实体框架的下一个版本中的功能。

若要开始本教程，请启动 Visual Studio 并打开与你共事中以前的教程，Contoso 大学 web 应用程序。

## <a name="foreign-key-associations"></a>外键关联

包含实体框架版本 3.5 的导航属性，但它没有数据模型中包括外键属性。 例如，`CourseID`和`StudentID`列`StudentGrade`表将省略从`StudentGrade`实体。

[![Image01](what-s-new-in-the-entity-framework-4/_static/image2.png)](what-s-new-in-the-entity-framework-4/_static/image1.png)

此方法的原因时，严格地说外, 键是物理实现详细信息且不属于概念数据模型中。 但是，在实践中，通常很容易地使用代码中的实体时可以直接访问的外键。

为数据模型中的方式外键的一个示例可以简化你的代码，请考虑如何你将获得了对代码*DepartmentsAdd.aspx*页，不需要它们。 在`Department`实体，`Administrator`属性是对应的外键`PersonID`中`Person`实体。 为了建立新部门和其管理员之间的关联，你所要做已设置的值`Administrator`中的属性`ItemInserting`的数据绑定控件的事件处理程序：

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample1.cs)]

如果数据模型中的外键，没有将处理`Inserting`而不是数据源控件的事件`ItemInserting`的数据绑定控件，以获取对该实体本身的引用，该实体添加到实体集之前的事件。 该引用后，你建立使用类似于下面的示例中的代码的关联：

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample2.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample3.cs)]

在实体框架团队中可以看到[在外键关联上的博客文章](https://blogs.msdn.com/b/efdesign/archive/2009/03/16/foreign-keys-in-the-entity-framework.aspx)，有其他情况下，在代码复杂性方面的差异是大得多。 若要满足的要求的那些人员更愿意接受更简易的代码为概念数据模型中的实现详细信息，实体框架将会提供数据模型中包括外键的选项。

在实体框架术语中，如果数据模型中包括外键您正使用*外键关联*，并且如果你正在使用的外键中排除*独立关联*。

## <a name="executing-user-defined-sql-commands"></a>执行用户定义的 SQL 命令

在早期版本的实体框架中，没有简单方法来动态创建你自己的 SQL 命令，然后运行这些。 实体框架动态生成为你的 SQL 命令，或者您必须创建存储的过程并将其作为函数导入。 版本 4 添加`ExecuteStoreQuery`和`ExecuteStoreCommand`方法`ObjectContext`使其更轻松地任何查询将直接传递到数据库的类。

假设 Contoso 大学管理员想要能够对数据库中执行大容量更改，而无需经历创建存储的过程和导入数据模型的过程。 页，以便他们更改用于在数据库中的所有课程的信用额度数可以是其第一个请求。 他们想要能够输入要使用的值相乘的数字在网页上，每个`Course`行的`Credits`列。

创建新的页使用*Site.Master*母版页并将其命名*UpdateCredits.aspx*。 然后添加以下标记到`Content`控件名为`Content2`:

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample4.aspx)]

此标记将创建`TextBox`控制用户可以在其中输入乘数值，`Button`控件若要执行该命令，请单击和`Label`，该值指示受影响的行数的控件。

打开*UpdateCredits.aspx.cs*，并添加以下`using`语句和为按钮的处理程序`Click`事件：

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample5.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample6.cs)]

此代码执行 SQL`Update`命令在文本框中使用的值，并使用标签来显示受影响的行数。 运行页面之前，运行*Courses.aspx*页后，可以获取一些数据的"之前"图片。

[![Image02](what-s-new-in-the-entity-framework-4/_static/image4.png)](what-s-new-in-the-entity-framework-4/_static/image3.png)

运行*UpdateCredits.aspx*，作为乘数中，输入"10"，然后单击**执行**。

[![Image03](what-s-new-in-the-entity-framework-4/_static/image6.png)](what-s-new-in-the-entity-framework-4/_static/image5.png)

运行*Courses.aspx*页上再次查看已更改的数据。

[![Image04](what-s-new-in-the-entity-framework-4/_static/image8.png)](what-s-new-in-the-entity-framework-4/_static/image7.png)

(如果你想要在将信用额度数设置回其原始值， *UpdateCredits.aspx.cs*更改`Credits * {0}`到`Credits / {0}`，然后重新运行页上，输入 10 作为除数。)

有关执行查询，以在代码中定义的详细信息，请参阅[如何： 直接执行命令针对数据源](https://msdn.microsoft.com/library/ee358769.aspx)。

## <a name="model-first-development"></a>模型优先开发

这些演练中，你首先创建数据库，然后生成基于数据库结构的数据模型。 Entity Framework 4 中你可以改为启动与数据模型，并生成基于数据模型结构的数据库。 如果你要创建应用程序为其数据库尚不存在，模型为先的方法使你可以创建实体和关系从概念上讲对应用程序，并不需考虑如何物理实现详细信息的意义. （情况都是如此只能通过初始阶段的开发，但是。 最终将创建和中，必须将具有生产数据的数据库，重新创建它从模型将不再是实际可行的;此时，你将准备回数据库为先的方法。）

在本教程的此部分中，你将创建简单的数据模型，并从它生成数据库。

在**解决方案资源管理器**，右键单击*DAL*文件夹，然后选择**添加新项**。 在**添加新项**对话框中，在**已安装的模板**选择**数据**，然后选择**ADO.NET 实体数据模型**模板. 将新文件命名*AlumniAssociationModel.edmx*单击**添加**。

[![Image06](what-s-new-in-the-entity-framework-4/_static/image10.png)](what-s-new-in-the-entity-framework-4/_static/image9.png)

这将启动实体数据模型向导。 在**选择模型内容**步骤中，选择**空模型**，然后单击**完成**。

[![Image07](what-s-new-in-the-entity-framework-4/_static/image12.png)](what-s-new-in-the-entity-framework-4/_static/image11.png)

**Entity Data Model Designer**打开与空白设计图面。 拖动**实体**项从**工具箱**拖到设计图面。

[![Image08](what-s-new-in-the-entity-framework-4/_static/image14.png)](what-s-new-in-the-entity-framework-4/_static/image13.png)

更改从实体名称`Entity1`到`Alumnus`，更改`Id`属性名称设置为`AlumnusId`，并添加名为的新标量属性`Name`。 若要将新属性添加你可以更改的名称后按 Enter`Id`列，或者右键单击该实体，然后选择**添加标量属性**。 新属性的默认类型是`String`，这样做也可以为此简单演示中，但这是当然你可以更改等中的数据类型**属性**窗口。

创建相同的方式的另一个实体并将其命名`Donation`。 更改`Id`属性`DonationId`并添加名为的标量属性`DateAndAmount`。

[![Image09](what-s-new-in-the-entity-framework-4/_static/image16.png)](what-s-new-in-the-entity-framework-4/_static/image15.png)

若要添加这两个实体之间的关联，右键单击`Alumnus`实体中，选择**添加**，然后选择**关联**。

[![Image10](what-s-new-in-the-entity-framework-4/_static/image18.png)](what-s-new-in-the-entity-framework-4/_static/image17.png)

中的默认值**添加关联**对话框是所需 （一到多包括导航属性，包括外键），因此只需单击**确定**。

[![Image11](what-s-new-in-the-entity-framework-4/_static/image20.png)](what-s-new-in-the-entity-framework-4/_static/image19.png)

设计器添加一条关联连线和外键属性。

[![Image12](what-s-new-in-the-entity-framework-4/_static/image22.png)](what-s-new-in-the-entity-framework-4/_static/image21.png)

现在你已准备好创建数据库。 右键单击设计图面并选择**根据模型生成数据库**。

[![Image13](what-s-new-in-the-entity-framework-4/_static/image24.png)](what-s-new-in-the-entity-framework-4/_static/image23.png)

这将启动生成数据库向导。 （如果您看到指示实体未映射的警告，则可以忽略这些暂时。）

在**选择你的数据连接**步骤中，单击**新连接**。

[![Image14](what-s-new-in-the-entity-framework-4/_static/image26.png)](what-s-new-in-the-entity-framework-4/_static/image25.png)

在**连接属性**对话框中，选择 SQL Server Express 的本地实例并将数据库命名`AlumniAsssociation`。

[![Image15](what-s-new-in-the-entity-framework-4/_static/image28.png)](what-s-new-in-the-entity-framework-4/_static/image27.png)

单击**是**当系统询问你是否是否想要创建数据库。 当**选择你的数据连接**再次显示步骤，请单击**下一步**。

在**摘要和设置**步骤中，单击**完成**。

[![Image18](what-s-new-in-the-entity-framework-4/_static/image30.png)](what-s-new-in-the-entity-framework-4/_static/image29.png)

A *.sql*将创建与数据定义语言 (DDL) 命令的文件，但尚未运行命令。

[![Image20](what-s-new-in-the-entity-framework-4/_static/image32.png)](what-s-new-in-the-entity-framework-4/_static/image31.png)

使用一种工具，如**SQL Server Management Studio**来运行脚本并创建表，因为可能在执行了你在创建时`School`数据库[入门教程系列中的第一个教程](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md). （除非你下载此数据库）。

你现在可以使用`AlumniAssociation`数据模型中你的 web 页你所用的相同方式`School`模型。 若要试用此项，对表中添加一些数据，并创建网页上显示的数据。

使用**服务器资源管理器**，添加以下行到`Alumnus`和`Donation`表。

[![Image21](what-s-new-in-the-entity-framework-4/_static/image34.png)](what-s-new-in-the-entity-framework-4/_static/image33.png)

创建一个名为的新 web 页*Alumni.aspx*使用*Site.Master*母版页。 添加到以下标记`Content`控件名为`Content2`:

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample7.aspx)]

此标记将创建嵌套`GridView`控制，外部的一个，若要显示校友名称，内部的一个，以显示捐赠日期和金额。

打开*Alumni.aspx.cs*。 添加`using`语句的数据访问层和的处理程序外部`GridView`控件的`RowDataBound`事件：

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample8.cs)]

此代码 databinds 内部`GridView`控制使用`Donations`的当前行的导航属性`Alumnus`实体。

运行页面。

[![Image22](what-s-new-in-the-entity-framework-4/_static/image36.png)](what-s-new-in-the-entity-framework-4/_static/image35.png)

(注意： 此页包含在可下载的项目中，但若要使它起作用，您必须创建数据库在本地 SQL Server Express 实例; 数据库中不包含作为*.mdf*文件中*应用\_数据*文件夹。)

有关使用实体框架的第一个模型的功能的详细信息，请参阅[实体 Framework 4 中的模型的第一个](https://msdn.microsoft.com/data/ff830362.aspx)。

## <a name="poco-support"></a>POCO 支持

当使用域驱动的设计方法时，您设计数据类表示数据和与业务领域相关的行为。 这些类就能独立于任何特定的技术，用于存储 （保留） 数据;换而言之，应将它们*持久性未知*。 此外可持久性无感知可以使一个类简单到单元测试，因为单元测试项目可以使用任何持久性技术是最方便的测试。 早期版本的实体框架提供对持久性无感知的支持有限，因为实体类必须继承自`EntityObject`类，因此包含大量实体框架特有的功能。

实体框架 4 引入了使用不继承的实体类的能力`EntityObject`类，因此可以在持久性未知。 在实体框架的上下文中，通常称为的类，如这*纯旧式 CLR 对象*（POCO 或 POCOs）。 你可以手动编写 POCO 类也可以自动生成基于使用实体框架提供的文本模板转换工具包 (T4) 模板的现有数据模型。

有关在实体框架中使用 POCOs 的详细信息，请参阅以下资源：

- [使用 POCO 实体](https://msdn.microsoft.com/library/dd456853.aspx)。 这是概述 POCOs，其中包含指向具有更多详细信息的其他文档的 MSDN 文档。
- [演练： POCO 实体框架的模板](https://blogs.msdn.com/b/adonet/archive/2010/01/25/walkthrough-poco-template-for-the-entity-framework.aspx)这是实体框架开发团队，以链接到有关 POCOs 其他博客文章的博客文章。

## <a name="code-first-development"></a>代码优先开发

POCO 支持 Entity Framework 4 中的仍然需要你创建数据模型，并链接到数据模型的实体类。 实体框架的下一个版本将包含一个称为功能*代码优先的开发*。 此功能，可用于实体框架使用您自己的 POCO 类而无需使用数据模型设计器或数据模型 XML 文件。 (因此，此选项也已调用*仅限代码的*;*代码优先*和*仅限代码的*都是指同一实体框架功能。)

有关使用开发的代码优先方法的详细信息，请参阅以下资源：

- [代码优先的开发使用实体框架 4](https://weblogs.asp.net/scottgu/archive/2010/07/16/code-first-development-with-entity-framework-4.aspx)。 这是由 Scott Guthrie 简介代码优先开发博客文章。
- [实体框架开发团队博客-文章标记的 CodeOnly](https://blogs.msdn.com/b/efdesign/archive/tags/codeonly/)
- [实体框架开发团队博客-文章标记 Code First](https://blogs.msdn.com/b/efdesign/archive/tags/code+first/)
- [MVC 音乐商店教程-第 4 部分： 模型和数据访问](../../../../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4.md)
- [Getting Started with MVC 3-第 4 部分： 实体框架代码优先开发](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model.md)

此外，生成类似于 Contoso 大学应用程序的应用程序的新 MVC 代码的第一个教程预计在 2011 年春季发布[https://asp.net/entity-framework/tutorials](../../../../entity-framework.md)

## <a name="more-information"></a>详细信息

这将完成将新实体框架和此继续实体框架教程系列中的概述。 有关实体框架 4 所未涵盖的新功能的详细信息，请参阅以下资源：

- [什么是 ADO.NET 中的新增功能](https://msdn.microsoft.com/library/ex6y04yf.aspx)MSDN 主题中的实体框架版本 4 的新功能。
- [宣布推出新版实体框架 4](https://blogs.msdn.com/b/efdesign/archive/2010/04/12/announcing-the-release-of-entity-framework-4.aspx)版本 4 中的新增功能有关实体框架开发团队的博客文章。

>[!div class="step-by-step"]
[上一篇](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)
