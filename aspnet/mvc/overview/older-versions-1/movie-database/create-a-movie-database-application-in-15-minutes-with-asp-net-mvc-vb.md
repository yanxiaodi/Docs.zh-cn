---
uid: mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb
title: "创建电影数据库应用程序在 15 分钟内使用 ASP.NET MVC (VB) |Microsoft 文档"
author: StephenWalther
description: "Stephen Walther 生成整个数据库驱动 ASP.NET MVC 应用程序从头到尾完成。 本教程是人士提供新 t 的极佳介绍..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: e4ba9786-734c-4eb3-91bb-089793325d0d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb
msc.type: authoredcontent
ms.openlocfilehash: b87a69df24a410161dfaf055519eb6137fa76c06
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="create-a-movie-database-application-in-15-minutes-with-aspnet-mvc-vb"></a>创建电影数据库应用程序在 15 分钟内使用 ASP.NET MVC (VB)
====================
通过[Stephen Walther](https://github.com/StephenWalther)

[下载代码](http://download.microsoft.com/download/7/2/8/728F8794-E59A-4D18-9A56-7AD2DB05BD9D/MovieApp_VB.zip)

> Stephen Walther 生成整个数据库驱动 ASP.NET MVC 应用程序从头到尾完成。 本教程是过程的，新的 ASP.NET MVC framework 并人员想要获取的意义上生成 ASP.NET MVC 应用程序的极佳简介。


本教程的目的是为你提供的"在"的意义上生成 ASP.NET MVC 应用程序。 在本教程中，我完全通过生成整个 ASP.NET MVC 应用程序从头到尾完成。 我将向你演示如何构建的简单数据库驱动应用程序演示了如何列出、 创建和编辑数据库记录。

为了简化生成我们的应用程序的过程，我们将充分利用 Visual Studio 2008 的基架功能。 我们将让 Visual Studio 生成的初始代码和我们控制器、 模型和视图的内容。

如果你在使用 Active Server Pages 或 ASP.NET，然后应找到 ASP.NET MVC 非常熟悉。 ASP.NET MVC 视图非常类似 Active Server Pages 应用程序中的页。 和类似于传统的 ASP.NET Web 窗体应用程序，只需 ASP.NET MVC 为您提供完全访问权限组丰富的语言和.NET framework 提供的类。

我希望是本教程将为你提供一种方式生成 ASP.NET MVC 应用程序的体验是相似和不同于生成不 Active Server Pages 或 ASP.NET Web 窗体应用程序的体验。

## <a name="overview-of-the-movie-database-application"></a>电影数据库应用程序的概述

由于我们的目标是为简单起见，我们将生成一个非常简单的电影数据库应用程序。 我们简单的电影数据库应用程序将使我们能够做三件事：

1. 列出电影数据库记录的集
2. 创建一个新的影片数据库记录
3. 编辑现有的电影数据库记录

同样，因为我们想要为简单起见，我们将充分利用 ASP.NET MVC framework 构建我们的应用程序所需的功能的最小数。 例如，我们将不会利用 Test-Driven 开发。

若要创建我们的应用程序，我们需要完成每个以下步骤：

1. 创建 ASP.NET MVC Web 应用程序项目
2. 创建数据库
3. 创建数据库模型
4. 创建 ASP.NET MVC 控制器
5. 创建 ASP.NET MVC 视图

## <a name="preliminaries"></a>初步操作

你将需要 Visual Studio 2008 或 Visual Web Developer 2008 Express 生成一个 ASP.NET MVC 应用程序。 你还需要下载 ASP.NET MVC framework。

如果你拥有 Visual Studio 2008，然后可以从该网站下载 Visual Studio 2008 90 天试用版：

[https://msdn.microsoft.com/vs2008/products/cc268305.aspx](https://msdn.microsoft.com/vs2008/products/cc268305.aspx)

或者，你可以创建 ASP.NET MVC 应用程序与 Visual Web Developer Express 2008。 如果你决定使用 Visual Web Developer Express，则你必须拥有安装 Service Pack 1。 你可以从此网站中下载 Visual Web Developer 2008 速成版 Service Pack 1:

[https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp;displaylang=en](https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp;displaylang=en)

安装 Visual Studio 2008 或 Visual Web Developer 2008 之后，你需要安装 ASP.NET MVC 框架。 你可以从以下网站下载的 ASP.NET MVC framework:

[https://www.asp.net/mvc/](../../../index.md)

> [!NOTE] 
> 
> 而不是单独下载 ASP.NET framework 和 ASP.NET MVC framework，你可以利用 Web 平台安装程序。 Web 平台安装程序是该应用程序，你可以轻松地管理已安装的应用程序是你的计算机：
> 
> [https://www.microsoft.com/web/gallery/Install.aspx](https://www.microsoft.com/web/gallery/Install.aspx)


## <a name="creating-an-aspnet-mvc-web-application-project"></a>创建 ASP.NET MVC Web 应用程序项目

让我们开始通过在 Visual Studio 2008 中创建一个新的 ASP.NET MVC Web 应用程序项目。 选择菜单选项**文件、 新项目**，你将看到图 1 中的新建项目对话框。 作为编程语言选择 Visual Basic 和选择 ASP.NET MVC Web 应用程序项目模板。 为你的项目提供 MovieApp 的名称，然后单击确定按钮。


[![新项目对话框](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image1.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image1.png)

**图 01**: 新建项目对话框中 ([单击以查看实际尺寸的图像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image2.png))


请确保从新项目对话框顶部的下拉列表中选择.NET Framework 3.5 或 ASP.NET MVC Web 应用程序项目模板不会出现。


每当创建新的 MVC Web 应用程序项目时，Visual Studio 会提示你创建单独的单元测试项目。 在图 2 显示对话框。 因为我们不会在本教程需要创建测试由于时间约束 （和，是的我们应感觉有关这有点感到） 选择**否**选项，然后单击**确定**按钮。

> [!NOTE] 
> 
> Visual Web Developer 不支持测试项目。


[![新项目对话框](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image2.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image3.png)

**图 02**: 创建单元测试项目对话框 ([单击以查看实际尺寸的图像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image4.png))


ASP.NET MVC 应用程序具有一组标准的文件夹： 模型、 视图和控制器的文件夹。 你可以看到此组标准的解决方案资源管理器窗口中的文件夹。 我们将需要将文件添加到每个模型、 视图和控制器文件夹中，以生成电影数据库应用程序。

当使用 Visual Studio 创建新的 MVC 应用程序时，可以示例应用程序。 由于我们想要从头开始，我们需要删除此示例应用程序的内容。 您需要删除以下文件和以下文件夹：

- Controllers\HomeController.vb
- Views\Home

## <a name="creating-the-database"></a>创建数据库

我们需要创建一个数据库以包含我们电影的数据库记录。 幸运的是，Visual Studio 包含一个名为 SQL Server Express 的免费数据库。 请按照下列步骤来创建数据库：

1. 右键单击该应用\_在解决方案资源管理器窗口中，然后选择菜单选项的数据文件夹**添加、 新项**。
2. 选择**数据**类别，然后选择**SQL Server 数据库**模板 （请参见图 3）。
3. 将新数据库命名*MoviesDB.mdf*单击**添加**按钮。

创建你的数据库后，你可以通过双击位于应用程序中的 MoviesDB.mdf 文件连接到数据库\_数据文件夹。 双击 MoviesDB.mdf 文件会打开服务器资源管理器窗口。

> [!NOTE] 
> 
> 服务器资源管理器窗口被命名为在 Visual Web Developer 的情况下的数据库资源管理器窗口。


[![新项目对话框](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image3.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image5.png)

**图 03**： 创建 Microsoft SQL Server 数据库 ([单击以查看实际尺寸的图像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image6.png))


接下来，我们需要创建一个新的数据库表。 在服务器资源管理器窗口中，右键单击表文件夹然后选择菜单选项**添加新表**。 选择此菜单选项将打开数据库表设计器。 创建以下数据库列：

<a id="0.2_table01"></a>


| **列名称** | **数据类型** | **允许 null 值** |
| --- | --- | --- |
| Id | Int | False |
| 标题 | Nvarchar(100) | False |
| 控制器 | Nvarchar(100) | False |
| DateReleased | DateTime | False |


第一列，Id 列中，具有两个特殊属性。 首先，你需要将标记为主键列的 Id 列。 选择 Id 列之后，单击**设置主键**按钮 （它位于图标，看起来类似的键）。 其次，你需要将标记为标识列的 Id 列。 在列属性窗口中，向下滚动到标识规范部分，并将其展开。 更改**是标识**属性值**是**。 完成后，应如下表所示图 4。


[![新项目对话框](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image4.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image7.png)

**图 04**: 电影数据库表 ([单击以查看实际尺寸的图像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image8.png))


最后一步是以保存新的表。 单击保存按钮 （软盘图标），并且提供名称电影的新表。

在完成创建表后，向表中添加一些影片记录。 右键单击服务器资源管理器窗口中的电影表然后选择菜单选项**显示表数据**。 输入您最喜爱的电影 （请参见图 5） 的列表。


[![新项目对话框](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image5.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image9.png)

**图 05**： 输入电影记录 ([单击以查看实际尺寸的图像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image10.png))


## <a name="creating-the-model"></a>创建模型

接下来，我们需要创建一组类来表示我们的数据库。 我们需要创建的数据库模型。 我们将充分利用 Microsoft 实体框架可以自动生成用于我们的数据库模型的类。

> [!NOTE] 
> 
> ASP.NET MVC framework 不依赖于 Microsoft 的实体框架中。 你也可以利用各种对象关系映射创建你的数据库模型类 (或者 / M) 工具包括 LINQ to SQL、 Subsonic 和 nhibernate 进行。


请按照下列步骤以启动实体数据模型向导：

1. 右键单击解决方案资源管理器窗口，然后选择菜单选项中的 Models 文件夹**添加、 新项**。
2. 选择**数据**类别，然后选择**ADO.NET 实体数据模型**模板。
3. 将命名的数据模型*MoviesDBModel.edmx*单击**添加**按钮。

单击添加按钮后，则将显示实体数据模型向导 （请参阅图 6）。 请按照下列步骤以完成向导：

1. 在**选择模型内容**步骤中，选择**从数据库生成**选项。
2. 在**选择你的数据连接**步骤中，使用*MoviesDB.mdf*数据连接和名称*MoviesDBEntities*对于连接设置。 单击**下一步**按钮。
3. 在**选择数据库对象**步骤，展开表节点，选择电影表。 输入的命名空间*MovieApp.Models*单击**完成**按钮。


[![新项目对话框](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image6.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image11.png)

**图 06**： 生成数据库模型的实体数据模型向导 ([单击以查看实际尺寸的图像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image12.png))


完成实体数据模型向导后，将打开实体数据模型设计器。 设计器应显示电影数据库表 （请参阅图 7）。


[![新项目对话框](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image7.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image13.png)

**图 07**: 实体数据模型设计器 ([单击以查看实际尺寸的图像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image14.png))


我们需要进行一次更改，我们继续操作之前。 实体数据向导将生成一个名为电影模型类，表示电影数据库表。 因为我们将使用影片类来表示特定电影，我们需要修改的类的名称*电影*而不是*电影*（单数而复数形式）。

双击设计器图面上的类的名称，并将影片中的类的名称更改为影片。 此更改后，单击**保存**按钮 （软盘图标） 生成电影类。

## <a name="creating-the-aspnet-mvc-controller"></a>创建 ASP.NET MVC 控制器

下一步是创建 ASP.NET MVC 控制器。 控制器负责控制用户如何与 ASP.NET MVC 应用程序交互。

请执行这些步骤：

1. 在解决方案资源管理器窗口中，右键单击 Controllers 文件夹，然后选择菜单选项**添加、 控制器**。
2. 在添加控制器对话框中，输入名称*HomeController*并选中复选框标记为**添加用于创建、 更新和详细信息的方案的操作方法**（请参阅图 8）。
3. 单击**添加**按钮，将新的控制器添加到你的项目。

完成这些步骤后，将创建列表 1 中的控制器。 请注意，它包含命名索引，详细信息，创建、 方法和编辑。 在以下部分中，我们将添加必要的代码来获取这些方法能够正常工作。


[![新项目对话框](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image8.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image15.png)

**图 08**： 添加新的 ASP.NET MVC 控制器 ([单击以查看实际尺寸的图像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image16.png))


**列表 1 – Controllers\HomeController.vb**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample1.vb)]

## <a name="listing-database-records"></a>列出数据库记录

Home 控制器的 index （） 方法是 ASP.NET MVC 应用程序的默认方法。 ASP.NET MVC 应用程序运行时，index （） 方法是调用的第一个控制器方法。

我们将使用 index （） 方法以显示电影数据库表中的记录的列表。 我们将充分利用数据库我们之前创建若要检索具有 index （） 方法的电影数据库记录的模型类。

我已修改中列出的 2 的 HomeController 类，以使其包含名为的新私有字段\_db。 MoviesDBEntities 类表示我们的数据库模型，并且我们将使用此类与我们的数据库进行通信。

我已修改中列出的 2 的 index （） 方法。 Index （） 方法使用 MoviesDBEntities 类来从电影数据库表中检索的所有影片记录。 表达式 *\_db。MovieSet.ToList()*返回电影数据库表中的所有影片记录的列表。

影片列表传递给视图。 获取传递给 View() 方法的任何内容获取传递给视图作为视图数据。

**列出 2 – Controllers/HomeController.vb （修改后的索引方法）**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample2.vb)]

Index （） 方法将返回名为索引视图。 我们需要创建此视图以显示的电影数据库记录的列表。 请执行这些步骤：


你应该能够生成你的项目 (选择菜单选项**生成，生成解决方案**) 在打开之前**添加视图**对话框或没有类将出现在**查看数据类**下拉列表。


1. 右键单击在代码编辑器中的 index （） 方法，然后选择菜单选项**添加视图**（请参阅图 9）。
2. 在添加视图对话框中，验证复选框标记为**创建强类型化视图**已选中。
3. 从**查看内容**下拉列表中，选择值*列表*。
4. 从**查看数据类**下拉列表中，选择值*MovieApp.Movie*。
5. 单击添加按钮以创建新查看 （请参阅图 10）。

完成这些步骤后，名为 Index.aspx 的新视图添加到 Views\Home 文件夹中。 索引视图的内容包含在列出 3 中。


[![新项目对话框](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image9.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image17.png)

**图 09**： 添加视图的控制器操作 ([单击以查看实际尺寸的图像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image18.png))


[![新项目对话框](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image10.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image19.png)

**图 10**： 使用添加视图对话框创建新视图 ([单击以查看实际尺寸的图像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image20.png))


[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample3.aspx)]

索引视图显示所有从 HTML 表内的电影数据库表的电影记录。 该视图包含一个 For 循环访问由 ViewData.Model 属性表示每个影片每个循环。 如果通过点击 F5 键运行应用程序，你将看到图 11 中的 web 页。


[![新项目对话框](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image11.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image21.png)

**图 11**: 索引视图 ([单击以查看实际尺寸的图像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image22.png))


## <a name="creating-new-database-records"></a>创建新的数据库记录

我们在上一节中创建的索引视图包括用于创建新的数据库记录的链接。 让我们继续操作，实现逻辑，并且创建用于创建新的影片数据库记录所必需的视图。

Home 控制器包含名为 create （） 的两个方法。 第一种 create （） 方法没有任何参数。 Create （） 方法的此重载用于显示用于创建新的影片数据库记录的 HTML 窗体。

第二个 create （） 方法有一个 FormCollection 参数。 创建新的影片的 HTML 窗体发布到服务器时，将调用 create （） 方法的此重载。 请注意，此第二个 create （） 方法 AcceptVerbs 属性，以防止方法被调用，除非执行 HTTP Post 操作。

列出 4 中的更新 HomeController 类中修改了此第二个 create （） 方法。 Create （） 方法的新版本接受电影参数，并包含电影数据库表中插入新的影片的逻辑。

> [!NOTE] 
> 
> 请注意绑定属性。 因为我们不想更新从 HTML 窗体的电影 Id 属性，我们需要此属性中显式排除。


**列出 4 – Controllers\HomeController.vb （修改后的创建方法）**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample4.vb)]

Visual Studio 可以轻松地创建用于创建新的影片数据库窗体记录 （请参阅图 12）。 请执行这些步骤：

1. 右键单击在代码编辑器中的 create （） 方法，然后选择菜单选项**添加视图**。
2. 验证该复选框标记为**创建强类型化视图**已选中。
3. 从**查看内容**下拉列表中，选择值*创建*。
4. 从**查看数据类**下拉列表中，选择值*MovieApp.Movie*。
5. 单击**添加**按钮以创建新视图。


[![新项目对话框](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image12.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image23.png)

**图 12**： 添加 Create 视图 ([单击以查看实际尺寸的图像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image24.png))


Visual Studio 视图列出 5 中会自动生成。 此视图包含 HTML 窗体，包括字段对应于每个电影类的属性。

**列出 5 – Views\Home\Create.aspx**

[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample5.aspx)]

> [!NOTE] 
> 
> 生成的添加视图对话框中的 HTML 格式生成 Id 窗体字段。 Id 列是标识列，因为我们不需要此窗体字段，你可以安全地删除它。


添加 Create 视图后，你可以向数据库添加新的影片记录。 通过按 F5 键运行你的应用程序并单击新创建的链接以查看图 13 中的窗体。 如果完成并提交表单时，会创建新的影片数据库记录。

请注意你自动获得窗体验证。 如果你忘记输入发布日期，对于影片，或输入无效的发布日期，然后重新显示窗体，并突出显示发布日期字段。


[![新项目对话框](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image13.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image25.png)

**图 13**： 创建新的影片数据库记录 ([单击以查看实际尺寸的图像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image26.png))


## <a name="editing-existing-database-records"></a>编辑现有的数据库记录

在前面的部分中，我们将讨论如何列表，以及如何创建新的数据库记录。 在此最后一节，我们将讨论如何可以编辑现有的数据库记录。

首先，我们需要生成编辑窗体。 此步骤很容易，因为 Visual Studio 将生成为我们的编辑窗体自动。 在 Visual Studio 代码编辑器中打开 HomeController.vb 类，并按照以下步骤：

1. 右键单击在代码编辑器中的 edit （） 方法，然后选择菜单选项**添加视图**（请参阅图 14）。
2. 选中复选框标记为**创建强类型化视图**。
3. 从**查看内容**下拉列表中，选择值*编辑*。
4. 从**查看数据类**下拉列表中，选择值*MovieApp.Movie*。
5. 单击**添加**按钮以创建新视图。

完成这些步骤将添加名为 Edit.aspx 到 Views\Home 文件夹的新视图。 此视图包含用于编辑电影记录 HTML 窗体。


[![新项目对话框](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image14.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image27.png)

**图 14**： 添加编辑视图 ([单击以查看实际尺寸的图像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image28.png))


> [!NOTE] 
> 
> 编辑视图包含一个 HTML 窗体字段，它对应于电影 Id 属性。 因为你不希望编辑的 Id 属性的值的人员，你应删除此窗体字段。


最后，我们需要修改主控制器，以便它支持编辑的数据库记录。 更新的 HomeController 类包含在列出 6。

**列出 6 – Controllers\HomeController.vb （编辑方法）**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample6.vb)]

在列出 6 中，我已经向 edit （） 方法的两个重载添加其他逻辑。 第一种 edit （） 方法返回到传递给方法的 Id 参数对应的电影数据库记录。 第二个重载执行到影片记录更新，在数据库中。

请注意，你必须检索原始的电影，，然后调用 ApplyPropertyChanges()，以更新数据库中已有的电影。

## <a name="summary"></a>摘要

本教程的目的是体验的为你提供一种生成 ASP.NET MVC 应用程序。 我希望你发现生成 ASP.NET MVC web 应用程序是非常类似于生成 Active Server Pages 或 ASP.NET 应用程序的体验。

在本教程中，我们将探讨的 ASP.NET MVC framework 仅最基本的功能。 在将来的教程中，我们深入了解主题，如控制器、 控制器操作、 视图、 查看数据和 HTML 帮助器。

>[!div class="step-by-step"]
[上一篇](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs.md)
