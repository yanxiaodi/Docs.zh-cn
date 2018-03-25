---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-vb
title: "使用实体框架 (VB) 创建模型类 |Microsoft 文档"
author: microsoft
description: "在本教程中，您将学习如何与 Microsoft 实体框架使用 ASP.NET MVC。 了解如何使用实体向导创建 ADO.NET 实体 Da..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: ff8322c9-12f3-4e24-aba6-a38046b9bb0d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-vb
msc.type: authoredcontent
ms.openlocfilehash: efc190d856fe9ebf1c09e0ae4758aabb1e3254dc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="creating-model-classes-with-the-entity-framework-vb"></a>使用实体框架 (VB) 中创建模型类
====================
通过[Microsoft](https://github.com/microsoft)

> 在本教程中，您将学习如何与 Microsoft 实体框架使用 ASP.NET MVC。 了解如何使用实体向导创建 ADO.NET 实体数据模型。 在本教程的过程中，我们生成的 web 应用程序演示如何以选择、 插入、 更新和删除通过使用实体框架的数据库数据。


本教程旨在说明如何创建生成 ASP.NET MVC 应用程序时使用 Microsoft 实体框架的数据访问类。 本教程假定之前不了解的 Microsoft 实体框架。 本教程结束时，你将了解如何使用实体框架来选择、 插入、 更新和删除数据库记录。

Microsoft 实体框架是一种对象关系映射 (O/RM) 工具，使您能够从数据库中自动生成的数据访问层。 实体框架，可避免手动生成你的数据访问类的繁琐工作。

> [!NOTE] 
> 
> ASP.NET MVC 和 Microsoft 实体框架之间没有基本连接。 有多种可选方案对你可以使用使用 ASP.NET MVC 的 Entity Framework。 例如，你可以构建使用其他 O/RM 工具，如 Microsoft LINQ to SQL、 NHibernate 或 SubSonic 你 MVC 模型类。


为了说明如何使用 Microsoft 实体框架使用 ASP.NET MVC，我们将生成一个简单的示例应用程序。 我们将创建的电影数据库应用程序，可显示和编辑电影数据库记录。

本教程假定你有 Visual Studio 2008 或 Visual Web Developer 2008 Service Pack 1。 若要使用实体框架需要 Service Pack 1。 你可以从以下地址下载 Visual Studio 2008 Service Pack 1 或 Service Pack 1 的 Visual Web Developer:

> [https://www.asp.net/downloads/](https://www.asp.net/downloads)


## <a name="creating-the-movie-sample-database"></a>创建电影示例数据库

电影数据库应用程序使用名为电影的数据库表包含以下列：

| 列名 | 数据类型 | 允许 null 值？ | 是主键？ |
| --- | --- | --- | --- |
| Id | int | False | True |
| 标题 | nvarchar(100) | False | False |
| 控制器 | nvarchar(100) | False | False |

通过执行以下步骤，可以将此表添加到 ASP.NET MVC 项目：

1. 右键单击该应用\_在解决方案资源管理器窗口中，然后选择菜单选项的数据文件夹**添加、 新项。**
2. 从**添加新项**对话框中，选择**SQL Server 数据库**，使数据库名称 MoviesDB.mdf，并单击**添加**按钮。
3. 双击 MoviesDB.mdf 文件以打开服务器资源管理器/数据库资源管理器窗口。
4. 展开 MoviesDB.mdf 数据库连接，右键单击表文件夹中，然后选择菜单选项**添加新表**。
5. 在表设计器中，添加的 Id、 标题和控制器列。
6. 单击**保存**（它具有的软盘图标） 的按钮以保存新的表中名称电影。

创建电影数据库表后，你应将一些示例数据添加到表中。 右键单击的电影表，然后选择菜单选项**显示表数据**。 可以将假影片数据输入出现的网格中。

## <a name="creating-the-adonet-entity-data-model"></a>创建 ADO.NET 实体数据模型

若要使用实体框架，你需要创建实体数据模型。 你可以利用 Visual Studio*实体数据模型向导*自动从数据库生成实体数据模型。

请执行这些步骤：

1. 右键单击解决方案资源管理器窗口中的 Models 文件夹，然后选择菜单选项**添加、 新项**。
2. 在**添加新项**对话框中，选择数据类别 （请参见图 1）。
3. 选择**ADO.NET 实体数据模型**模板，为实体数据模型提供的名称 MoviesDBModel.edmx，然后单击**添加**按钮。 单击**添加**按钮会启动数据模型向导。
4. 在**选择模型内容**步骤中，选择**从数据库生成**选项，然后单击**下一步**按钮 （请参见图 2）。
5. 在**选择你的数据连接**步骤，选择 MoviesDB.mdf 数据库连接，输入的实体连接设置命名 MoviesDBEntities，并单击**下一步**按钮 （请参见图 3）。
6. 在**选择数据库对象**步骤，选择电影数据库表，然后单击**完成**按钮 （请参见图 4）。

完成这些步骤后，将打开 ADO.NET 实体数据模型设计器 （实体设计器）。

**图 1 – 创建新的实体数据模型**

![clip_image002](creating-model-classes-with-the-entity-framework-vb/_static/image1.jpg)

**图 2 – 选择模型内容步骤**

![clip_image004](creating-model-classes-with-the-entity-framework-vb/_static/image2.jpg)

**图 3-选择你的数据连接**

![clip_image006](creating-model-classes-with-the-entity-framework-vb/_static/image3.jpg)

**图 4-选择数据库对象**

![clip_image008](creating-model-classes-with-the-entity-framework-vb/_static/image4.jpg)

## <a name="modifying-the-adonet-entity-data-model"></a>修改 ADO.NET 实体数据模型

创建实体数据模型后，你可以通过利用在实体设计器中修改模型 （请参见图 5）。 通过双击解决方案资源管理器窗口中的 Models 文件夹中包含的 MoviesDBModel.edmx 文件，可以在任何时候打开实体设计器。

**图 5 – ADO.NET 实体数据模型设计器**

![clip_image010](creating-model-classes-with-the-entity-framework-vb/_static/image5.jpg)

例如，实体设计器可用于更改的实体模型数据向导生成的类的名称。 该向导创建一个新的名为电影的数据访问类。 换而言之，该向导为类提供了与数据库表非常相同的名称。 因为我们将使用此类来表示特定电影实例，我们应该重命名电影电影的类。

如果你想要重命名的实体类，你可以双击实体设计器中的类名称，输入新名称 （请参阅图 6）。 或者，你可以在实体设计器中选择实体后更改属性窗口中的实体的名称。

**图 6 – 更改实体名称**

![clip_image012](creating-model-classes-with-the-entity-framework-vb/_static/image6.jpg)

请记住保存通过单击保存按钮 （软盘图标） 进行了修改后的实体数据模型。 在后台，实体设计器生成一组的 Visual Basic.NET 类。 你可以通过从解决方案资源管理器窗口中打开 MoviesDBModel.Designer.vb 文件来查看这些类。


由于所做的更改将覆盖下次使用实体设计器时，不要修改 Designer.vb 文件中的代码。 如果你想要扩展 Designer.vb 文件中定义的实体类的功能，则可以创建*分部类*在单独的文件。


#### <a name="selecting-database-records-with-the-entity-framework"></a>选择与实体框架的数据库记录

让我们开始构建我们电影数据库应用程序创建页，显示的电影记录的列表。 列表 1 中的主页控制器公开名为 index （） 的操作。 Index （） 操作返回的所有影片记录电影数据库表中通过利用的实体框架。

**列表 1 – Controllers\HomeController.vb**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample1.vb)]

请注意列表 1 中的控制器包含一个构造函数。 构造函数初始化名为的类级字段\_db。 \_Db 字段表示生成的 Microsoft 实体框架的数据库实体。 \_Db 字段是由实体设计器生成的 MoviesDBEntities 类的实例。

\_Db 字段使用 index （） 操作中，若要检索电影数据库表中的记录。 表达式\_db。MovieSet 表示的所有记录电影数据库表中。 Tolist （） 方法用于将电影该集转换到的电影对象的泛型集合： 列表 （的电影）。

LINQ to Entities 的帮助检索电影记录。 列表 1 中的 index （） 操作使用 LINQ*方法语法*检索的数据库记录集。 如果你愿意，你可以使用 LINQ*查询语法*相反。 下面的两个语句执行非常相同的操作：

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample2.vb)]

使用你发现最直观无论 LINQ 语法 – 方法语法或查询语法 –。 没有任何区别两种方法之间的性能 – 唯一的区别是样式。

列出 2 中的视图用于显示电影记录。

**列出 2 – Views\Home\Index.aspx**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample3.aspx)]

列出 2 中的视图包含**每个**循环，循环访问每个电影记录，并显示电影记录的标题和控制器属性的值。 请注意，每个记录旁边显示的编辑和删除链接。 此外，该视图的底部将显示添加电影链接 （请参阅图 7）。

**图 7-索引视图**

![clip_image014](creating-model-classes-with-the-entity-framework-vb/_static/image7.jpg)

索引视图是*类型化视图*。 索引视图具有&lt;%@ 页 %&gt;包括 Inherits 特性的指令。 Inherits 特性将强类型列表的泛型集合电影对象 – 列表 （的电影） ViewData.Model 属性强制转换。

## <a name="inserting-database-records-with-the-entity-framework"></a>插入与实体框架的数据库记录

可以使用实体框架，以便可以方便地将新记录插入数据库表。 列出 3 包含两个新的操作添加到 Home 控制器类可用于将新记录插入电影数据库表。

**列出 3 – Controllers\HomeController.vb （Add 方法）**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample4.vb)]

第一项 add （） 操作只需返回的视图。 该视图包含用于添加新的影片数据库的窗体记录 （请参阅图 8）。 当在提交表单时，将调用第二个 add （） 操作。

请注意，使用 AcceptVerbs 特性修饰的第二个 add （） 操作。 只有在执行 HTTP POST 操作时，可以调用此操作。 换而言之，仅当发布 HTML 窗体时，才可以调用此操作。

第二个 add （） 操作创建 ASP.NET MVC TryUpdateModel() 方法的帮助的实体框架电影类的新实例。 TryUpdateModel() 方法采用传递给 add （） 方法 FormCollection 中的字段，并将这些 HTML 窗体字段的值分配给电影类。


在使用实体框架时，使用的 TryUpdateModel 或 UpdateModel 方法来更新实体类的属性时，必须提供"允许列表"的属性。


接下来，add （） 操作执行一些简单的表单验证。 操作可验证的标题和控制器属性具有值。 如果没有验证错误，则会将验证错误消息添加到 ModelState。

如果没有任何验证错误然后会将新的影片记录添加到实体框架的帮助的电影数据库表。 新的记录添加到以下两行代码的数据库：

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample5.vb)]

第一行代码将新的影片实体添加到跟踪的实体框架的电影的集。 代码的第二行将保存到电影跟踪返回到基础数据库进行了任何更改。

**图 8-添加视图**

![clip_image016](creating-model-classes-with-the-entity-framework-vb/_static/image8.jpg)

## <a name="updating-database-records-with-the-entity-framework"></a>与实体框架的更新数据库记录

你可以按照几乎相同的方法来编辑与实体框架的数据库记录为我们刚插入的新的数据库记录的方法。 列出 4 包含名为 edit （） 的两个新控制器操作。 第一个 edit （） 操作返回用于编辑电影记录 HTML 窗体。 第二个 edit （） 操作尝试更新数据库。

**列出 4 – Controllers\HomeController.vb （编辑方法）**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample6.vb)]

通过从与正在编辑的影片的 Id 匹配的数据库中检索的影片记录启动第二个 edit （） 操作。 以下受 LINQ to Entities 语句获取的特定 Id 匹配的第一条数据库记录：

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample7.vb)]

接下来，TryUpdateModel() 方法用于将 HTML 窗体字段的值分配给电影实体的属性。 请注意，提供白名单来指定要更新的确切属性。

接下来，执行一些简单的验证，以便验证影片标题和控制器属性具有值。 如果这两个属性缺少一个值，然后验证错误消息添加到 ModelState 并 ModelState.IsValid 返回值 false。

最后，如果不不存在任何验证错误，然后基础的电影数据库表是使用更新的任何更改通过调用 savechanges （） 方法。

在编辑数据库记录时，请将传递到执行数据库更新的控制器操作正在编辑的记录的 Id。 否则，控制器操作不会知道哪一记录基础数据库中更新。 包含在列出 5 中，编辑视图包括隐藏的表单字段表示正在编辑的数据库记录的 Id。

**列出 5 – Views\Home\Edit.aspx**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample8.aspx)]

## <a name="deleting-database-records-with-the-entity-framework"></a>删除与实体框架的数据库记录

最终的数据库操作，我们需要处理在本教程中，正在删除数据库记录。 可以使用列出 6 中的控制器操作以删除特定的数据库记录。

**列出 6-\Controllers\HomeController.vb （删除操作）**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample9.vb)]

Delete （） 操作首先检索的影片的 Id 匹配的实体传递到操作。 接下来，影片从数据库删除通过调用 DeleteObject() 方法跟 savechanges （） 方法。 最后，用户重定向回索引视图。

## <a name="summary"></a>摘要

本教程的目的是演示如何利用 ASP.NET MVC 和 Microsoft 实体框架可以生成数据库驱动的 web 应用程序。 您学习了如何生成该应用程序，你可以选择、 插入、 更新和删除数据库记录。

首先，我们讨论了如何使用实体数据模型向导生成 Visual Studio 中从一个实体数据模型。 接下来，你将了解如何使用 LINQ to Entities 从数据库表中检索一组数据库记录。 最后，我们使用实体框架来插入、 更新和删除数据库记录。

>[!div class="step-by-step"]
[上一页](validation-with-the-data-annotation-validators-cs.md)
[下一页](creating-model-classes-with-linq-to-sql-vb.md)
